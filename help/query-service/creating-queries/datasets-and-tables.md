---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データセットとテーブルとスキーマ
topic: queries
translation-type: tm+mt
source-git-commit: 7d5d98d8e32607abf399fdc523d2b3bc99555507

---


# データセットとテーブルとスキーマ

[Adobe Experience Platform UIで使用できるデータセットのリストを確認し](https://platform.adobe.com/datasets)、データセット名を必ず確認してください。
>[!NOTE] 一部のデータセット名にはスペースが含まれ、スペースが含まれていない場合はSQLセーフにならない可能性があります。

![](../images/queries/datasets-and-tables/dataset-names.png)


データセットテーブル内のスキーマ名をクリックして、UIでデータセットスキーマの階層構造を確認します。

![](../images/queries/datasets-and-tables/schema-information.png)

PSQLコマンド・ラインを開き、次の場所から接続の詳細を使用します。 [https://platform.adobe.com/query/configuration](https://platform.adobe.com/query/configuration)。

![](../images/clients/psql/connect-bi.png)

SQLを使用したプラットフォームで使用可能な表を表示するには、またはを使用で `\d` きま `SHOW TABLES;`す。


`\d` 標準のPostgreSQL表示

```
             List of relations
 Schema |       Name      | Type  |  Owner   
--------+-----------------+-------+----------
 public | luma_midvalues  | table | postgres
 public | luma_postvalues | table | postgres
(2 rows)
```

`SHOW TABLES;` は、より詳細な表示を提供し、テーブルと、プラットフォームUIにあるデータセット名を表示するカスタムコマンドです。

```
       name      |        dataSetId         |     dataSet    | description | resolved 
-----------------+--------------------------+----------------+-------------+----------
 luma_midvalues  | 5bac030c29bb8d12fa992e58 | Luma midValues |             | false
 luma_postvalues | 5c86b896b3c162151785b43c | Luma midValues |             | false
(2 rows)
```

テーブルのルートスキーマを表示するには、コマンドを使用 `\d table_name` します。

>[!NOTE] 表示されたスキーマは、ルートフィールドを示し、そのほとんどが複雑で、データセットスキーマUIのオブジェクトタイプを参照します。

`\d luma_midvalues`

```
                         Table "public.luma_midvalues"
      Column       |             Type            | Collation | Nullable | Default 
-------------------+-----------------------------+-----------+----------+---------
 timestamp         | timestamp                   |           |          | 
 _id               | text                        |           |          | 
 productlistitems  | anyarray                    |           |          | 
 commerce          | luma_midvalues_commerce     |           |          | 
 receivedtimestamp | timestamp                   |           |          | 
 enduserids        | luma_midvalues_enduserids   |           |          | 
 datasource        | datasource                  |           |          | 
 web               | luma_midvalues_web          |           |          | 
 placecontext      | luma_midvalues_placecontext |           |          | 
 identitymap       | anymap                      |           |          | 
 marketing         | marketing                   |           |          | 
 environment       | luma_midvalues_environment  |           |          | 
 _experience       | luma_midvalues__experience  |           |          | 
 device            | device                      |           |          | 
 search            | search                      |           |          | 
```

さらに詳細なスキーマを表示するには、アンダースコア(`_`)を使用して、説明するテーブルの列を宣言します。 例：`\d table_name_column`。

`\d luma_midvalues_web`

```
                 Composite type "public.luma_midvalues_web"
     Column     |               Type                | Collation | Nullable | Default 
----------------+-----------------------------------+-----------+----------+---------
 webpagedetails | luma_midvalues_web_webpagedetails |           |          | 
 webreferrer    | web_webreferrer                   |           |          | 
```
