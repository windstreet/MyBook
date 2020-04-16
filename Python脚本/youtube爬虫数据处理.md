# youtube 爬虫数据处理

## 合并目录内所有csv文件为一个csv文件

```python
def main(directory, result_file_name='total'):
    """
    将目录内的 相同格式的 csv 文件，合并成一个csv文件
    """

    import os
    import pandas as pd

    def run(folder_path):
        """获取某文件夹下的所有 CSV 的完整路径"""

        if not folder_path.endswith('/'):
            folder_path = folder_path + '/'

        for file_name in os.listdir(folder_path):
            # 非文件夹且为csv文件
            if not os.path.isdir(file_name) and '.csv' in file_name:
                # csv文件不为空
                if os.stat(os.path.join(folder_path, file_name)).st_size:
                    yield os.path.join(folder_path, file_name)

    result_file = os.path.join(directory, '%s.csv' % result_file_name)
    print u'最终结果文件为：%s' % result_file

    files = list(run(directory))
    df1 = pd.read_csv(files[0])  # 读取首个csv文件，保存到df1中; encoding='gbk'
    for f in files[1:]:
        df2 = pd.read_csv(f)  # 打开csv文件，注意编码问题，保存到df2中
        df1 = pd.concat([df1, df2], axis=0, ignore_index=True)  # 将df2数据与df1合并

    df1 = df1.drop_duplicates()  # 去重
    # df1 = df1.reset_index(drop=True)  # 重新生成index
    df1.to_csv(result_file)  # 将结果保存为新的csv文件
```

运行

```python
main(directory='/Users/linrenwei/Desktop/had')
```


## 合并 `关键字搜索数据` 与 `个人主页数据` 
```python
def main(keyword_dir, profile_file):
    """将 `keyword_dir` 目录内的所有 `YouTube关键词搜索结果文件` ，分别与
        个人主页（profile）信息数据集， 合并成完整的数据

    :param keyword_dir:
    :param profile_file:
    :return:
    """

    import pandas as pd
    import os

    def run(folder_path):
        """获取某文件夹下的所有 CSV 的完整路径"""

        if not folder_path.endswith('/'):
            folder_path = folder_path + '/'

        for file_name in os.listdir(folder_path):
            # 非文件夹且为csv文件
            if not os.path.isdir(file_name) and '.csv' in file_name:
                # csv文件不为空
                if os.stat(os.path.join(folder_path, file_name)).st_size:
                    yield os.path.join(folder_path, file_name)

    result_dir = os.path.join(keyword_dir, 'result')
    print u"最终数据结果存放在：%s\n" % result_dir
    if not os.path.exists(result_dir):
        os.mkdir(result_dir)

    profile_f = pd.read_csv(profile_file)
    print u"含 profile 主页数据的文件为：%s\n" % profile_file

    for f in run(keyword_dir):
        print u"keyword 文件：%s" % f
        keyword_f = pd.read_csv(f)
        data = pd.merge(keyword_f, profile_f, left_on='homepage_link', right_on='profile_url', how='left')
        data = data[
            [
                'name', 'subscriber_count', 'view_count', 'mail_string', 'link_string', 'google', 'twitter', 'facebook',
                'instagram', 'website', 'country', 'joined_date', 'description', 'other_contact', 'homepage_link',
                'keyword'
            ]
        ]

        # 保存合并后的csv文件
        f_path, f_name = os.path.split(f)
        data.to_csv(os.path.join(result_dir, f_name), index=None)
```

运行

```python
main(
    keyword_dir='/Users/linrenwei/Desktop/1',
    profile_file='/Users/linrenwei/Desktop/had/total.csv'
)
```