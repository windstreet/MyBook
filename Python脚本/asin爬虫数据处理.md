# asin 爬虫数据处理

## 爬虫任务数据源

- `xlsx`格式，按列分成若干个csv数据    

```python
def list_to_csv(list_obj, out_csv):
    import csv
    with open(out_csv, u'wb') as outfile:
        writer = csv.DictWriter(outfile, fieldnames=['country_code', 'asin'])
        writer.writeheader()
        for i in list_obj:
            writer.writerow({'country_code': 'US', 'asin': i})


import pandas as pd
res = pd.read_excel('/Users/linrenwei/Desktop/us-asin.xlsx')
for fn in res.columns:
    new_fn = fn.replace('/', ' ')
    fn_path = u'/Users/linrenwei/Desktop/1/%s.csv' % new_fn
    print(fn_path)
    list_to_csv(list(res[fn]), fn_path)
```

- `xlsx`格式，一张表一类asin  

```python

def run(country_code=u'US',
        filename=u'/Users/linrenwei/Desktop/11.xlsx',
        output_dir=u'/Users/linrenwei/Desktop/1'):

    import pandas as pd
    import os

    if not os.path.exists(output_dir):
        os.mkdir(output_dir)
        print u'新建目录：%s' % output_dir
    else:
        print u'目录已存在'

    def list_to_csv(list_obj, out_csv):
        import csv
        with open(out_csv, u'wb') as outfile:
            writer = csv.DictWriter(outfile, fieldnames=['country_code', 'asin'])
            writer.writeheader()
            for i in list_obj:
                writer.writerow({'country_code': country_code, 'asin': i})

    res = pd.read_excel(filename, sheet_name=None)
    for sheet_name in res.keys():
        sheet_path = os.path.join(output_dir, u'%s.csv' % sheet_name)
        print sheet_name
        print sheet_path

        sheet_res = pd.read_excel(filename, sheet_name=sheet_name)
        sheet_list = list()
        for column_name in sheet_res.columns:
            print column_name
            sheet_list = sheet_list + list(sheet_res[column_name])

        list_to_csv(sheet_list, sheet_path)
        print '============================'

```

- 处理合并（挂单时间）后的数据      
     
```python
def run(fn_list,
        out_dir=u'/Users/linrenwei/Desktop/data'):
    import os
    import csv
    
    if not os.path.exists(out_dir):
        os.mkdir(out_dir)
    else:
        print u'目录已存在：{}'.format(out_dir)

    mapping = dict()
    with open(u'/Users/linrenwei/Desktop/asins.csv', u'r') as map_file:
        reader = csv.DictReader(map_file)
        for row in reader:
            mapping[row.get('asin')] = row

    fieldnames = [
        'country_code', 'asin', 'pk', 'deliver_to', 'node_tree', 'images', 'image_main', 'title', 'brand',
        'review_rating', 'review_count', 'price', 'features', 'offers_count', 'seller', 'fulfillment_channel',
        'issued_at',  # date_first
        'date_first', 'item_weight', 'shipping_weight', 'package_dimensions', 'product_dimensions', 'size', 'color',
        'sponsored_products', 'shopped_for', 'buy_after_view', 'also_viewed', 'similar_items',
        'description', 'best_sellers_rank', 'product_information', 'product_details',
        'review_layer', 'by_feature', 'customer_images', 'reviews_mention', 'crawl_timestamp'
    ]

    for fn in fn_list:
        fn_name = os.path.split(fn)[-1]
        print fn_name
        new_fn = os.path.join(out_dir, fn_name)
        print new_fn

        with open(fn, u'r') as infile, open(new_fn, u'wb') as outfile:
            writer = csv.DictWriter(outfile, fieldnames=fieldnames)
            writer.writeheader()
            for item in csv.DictReader(infile):
                asin = item.get('asin')
                if mapping.get(asin):
                    writer.writerow(mapping.get(asin))

```

```python
from scripts.utils import csv_in_dir

root_dir = u'/Users/linrenwei/Desktop/1'
fns = list(csv_in_dir(root_dir))
run(fns)

```