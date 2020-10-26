# review es 数据修复

```python
def run(es_url=None, review_indexes=None):
    from elasticsearch import Elasticsearch
    from elasticsearch_dsl import Search, Q
    from elasticsearch.helpers import bulk

    if not review_indexes:
        review_indexes = [
            u"crawler-amazon_bad_review_ca",
            u"crawler-amazon_bad_review_de",
            u"crawler-amazon_bad_review_es",
            u"crawler-amazon_bad_review_fr",
            u"crawler-amazon_bad_review_it",
            u"crawler-amazon_bad_review_jp",
            u"crawler-amazon_bad_review_uk",
            u"crawler-amazon_bad_review_us",
        ]

    es_url = es_url if es_url else u'http://localhost:9200'
    es = Elasticsearch(es_url)
    print('============= start =============')

    def travel_es(**kwargs):
        """遍历es的搜索结果

        :param kwargs: arguments same as elasticsearch search api.
        :return:
        """
        kwargs.setdefault(u"scroll", u"2m")
        kwargs.setdefault(u"size", 100)
        res = es.search(**kwargs)
        sid = res[u'_scroll_id']
        scroll_size = len(res[u'hits'][u'hits'])
        total_size = scroll_size
        print(total_size)
        while scroll_size > 0:
            "Scrolling..."
            # Before scroll, process current batch of hits
            for item in res[u'hits'][u'hits']:
                yield item
            res = es.scroll(scroll_id=sid, scroll=u'4m')
            # Update the scroll ID
            sid = res[u'_scroll_id']
            # Get the number of results that returned in the last scroll
            scroll_size = len(res[u'hits'][u'hits'])
            total_size += scroll_size
            print(total_size)

    def all_old_reviews(index):
        """

        获取所有review，按 `@timestamp` 倒序输出
        """
        s = Search(using=es, index=index).sort(u'-@timestamp')
        print(u'TOTAL: {}'.format(s.count()))
        for hit in travel_es(index=index, body=s.to_dict()):
            item = hit[u'_source'][u'data']
            item.update({
                u'task_id': hit[u'_source'][u'task_id'],
                u'@timestamp': hit[u'_source'][u'@timestamp'],
            })
            yield item

    def parse_asin_child(item):
        asin_child = None
        for key in [u'color', u'size', u'style']:
            if item.get(key):
                asin_child = item.get(u'asin')
                break
        return asin_child

    def create_asin_group_item(item):
        country_code = item.get(u'country_code') or item.get(u'country')
        asin_left = item.get(u'related_to') or item.get(u'father_asin')
        asin_right = parse_asin_child(item)
        pk = u'{}_{}_{}'.format(country_code, asin_left, asin_right)
        return {
            u'_op_type': u'index',
            u'_type': u'doc',
            u'_index': u'asin_group',
            u'_id': pk,
            u'task_id': item.get(u'task_id'),
            u'country_code': country_code,
            u'asin_left': asin_left,
            u'asin_right': asin_right,
            u'pk': pk,
            u'@timestamp': item.get(u'@timestamp')
        }

    def create_asin_review_id_item(item):
        """

        创建表 `asin_review_id` 的行项目
        """

        country_code = item.get(u'country_code') or item.get(u'country')
        asin_left = item.get(u'related_to') or item.get(u'father_asin')
        review_id = item.get(u'review_id') or item.get(u'id')
        pk = u'{}_{}_{}'.format(country_code, asin_left, review_id)
        return {
            u'_op_type': u'index',
            u'_type': u'doc',
            u'_index': u'asin_review_id',
            u'_id': pk,
            u'pk': pk,
            u'task_id': item.get(u'task_id'),
            u'country_code': country_code,
            u'asin': asin_left,
            u'asin_real': parse_asin_child(item),
            u'review_id': review_id,
            u'@timestamp': item.get(u'@timestamp')
        }

    def create_review_item(item):
        country_code = item.get(u'country_code') or item.get(u'country')
        review_id = item.get(u'review_id') or item.get(u'id')
        pk = u'{}_{}'.format(country_code, review_id)
        return {
            u'_op_type': u'index',
            u'_type': u'doc',
            u'_index': u'review_history',
            u'_id': pk,
            u'pk': pk,

            u"task_id": item.get(u'task_id'),
            u"country_code": country_code,
            u"star": item.get(u"star"),
            u"title": item.get(u"title"),
            u"author": item.get(u"author"),
            u"profile_id": item.get(u"profile_id") or item.get(u"author_profile_id"),
            u"issued_at": item.get(u"issued_at"),
            u"reviewed_in": item.get(u"reviewed_in"),
            u"is_local": item.get(u"is_local"),
            u"helpful_count": item.get(u"helpful_count"),
            u"size": item.get(u"size"),
            u"style": item.get(u"style"),
            u"color": item.get(u"color"),
            u"verified_purchase": item.get(u"verified_purchase"),
            u"content_text": item.get(u"content_text"),
            u"review_id": review_id,
            u"images": item.get(u"images"),
            u"videos": item.get(u"videos"),
            u"@timestamp": item.get('@timestamp'),
        }

    def data_generator():
        """新数据"""
        unique_asin_group = set()
        unique_review_history = set()
        unique_asin_review_id = set()

        for index in review_indexes:
            for item in all_old_reviews(index):
                # asin_group 行项目
                ag_item = create_asin_group_item(item)
                if ag_item.get(u'pk') not in unique_asin_group:
                    unique_asin_group.add(ag_item.get(u'pk'))
                    yield ag_item

                # asin_review_id 行项目
                ar_item = create_asin_review_id_item(item)
                if ar_item.get(u'pk') not in unique_asin_review_id:
                    unique_asin_review_id.add(ar_item.get(u'pk'))
                    yield ar_item

                # review_history 行项目
                r_item = create_review_item(item)
                if r_item.get(u'pk') not in unique_review_history:
                    unique_review_history.add(r_item.get(u'pk'))
                    yield r_item

    bulk(es, data_generator())
    print(u'============= end =============')

```