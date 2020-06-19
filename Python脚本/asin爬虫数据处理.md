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


import panda as pd
res = pd.read_excel('/Users/linrenwei/Desktop/us-asin.xlsx')
for fn in res.columns:
    new_fn = fn.replace('/', ' ')
    fn_path = u'/Users/linrenwei/Desktop/1/%s.csv' % new_fn
    print(fn_path)
    list_to_csv(list(res[fn]), fn_path)
```


- 处理合并（挂单时间）后的数据      
     
```python
def run():
    import os
    import csv

    mapping = dict()
    with open(u'/Users/linrenwei/Desktop/asins.csv', u'r') as map_file:
        reader = csv.DictReader(map_file)
        for row in reader:
            mapping[row.get('asin')] = row

    out_dir = u'/Users/linrenwei/Desktop/data'
    fn_list = [
        u"/Users/linrenwei/Desktop/1/Science Kits & Toys.csv",
        u"/Users/linrenwei/Desktop/1/Hammering & Pounding Toys.csv",
        u"/Users/linrenwei/Desktop/1/Electronic Learning & Education Toys.csv",
        u"/Users/linrenwei/Desktop/1/Baby Swimming Pool Floats.csv",
        u"/Users/linrenwei/Desktop/1/Children's Swim Rings.csv",
        u"/Users/linrenwei/Desktop/1/Kiddie Pools.csv",
        u"/Users/linrenwei/Desktop/1/Lawn Water Slides.csv",
        u"/Users/linrenwei/Desktop/1/Outdoor Water Play Sprinklers.csv",
        u"/Users/linrenwei/Desktop/1/Pool Rafts & Inflatable Ride-ons.csv",
        u"/Users/linrenwei/Desktop/1/Pool Toys.csv",
        u"/Users/linrenwei/Desktop/1/Swimming Pool Basketball & Volleyball.csv",
        u"/Users/linrenwei/Desktop/1/Water Balloons.csv",
        u"/Users/linrenwei/Desktop/1/Toy Golf Products.csv",
        u"/Users/linrenwei/Desktop/1/Toy Basketball Products.csv",
        u"/Users/linrenwei/Desktop/1/Playground Fitness Equipment.csv",
        u"/Users/linrenwei/Desktop/1/Play Set Ring & Trapeze Attachments.csv",
        u"/Users/linrenwei/Desktop/1/Play Set Climber Attachments.csv",
        u"/Users/linrenwei/Desktop/1/Basketball Products.csv",
        u"/Users/linrenwei/Desktop/1/Outdoor Inflatable Bouncers.csv",
        u"/Users/linrenwei/Desktop/1/Play & Swing Sets.csv",
        u"/Users/linrenwei/Desktop/1/Play Set Swings.csv",
        u"/Users/linrenwei/Desktop/1/Playground Equipment Parts & Hardware.csv",
        u"/Users/linrenwei/Desktop/1/Hobby RC Car Crawlers Trucks.csv",
        u"/Users/linrenwei/Desktop/1/Hobby RC Quadcopters & Multirotors.csv",
        u"/Users/linrenwei/Desktop/1/Remote- & App-Controlled Figures & Robotic Toys.csv",
        u"/Users/linrenwei/Desktop/1/Solar Power Kits.csv",
        u"/Users/linrenwei/Desktop/1/Toy Vehicle Playsets.csv",
    ]
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