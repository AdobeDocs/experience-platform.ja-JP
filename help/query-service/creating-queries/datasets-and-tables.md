---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データセットとテーブルおよびスキーマ
topic: queries
translation-type: tm+mt
source-git-commit: 3b710e7a20975880376f7e434ea4d79c01fa0ce5
workflow-type: tm+mt
source-wordcount: '181'
ht-degree: 92%

---


# データセットとテーブルおよびスキーマ

[Adobe Experience Platform UI](https://platform.adobe.com/datasets) で使用できるデータセットのリストのレビューを実施し、データセット名を必ず確認してください。
>[!NOTE]
>
>一部のデータセット名にはスペースが含まれており、スペースが含まれていない場合は SQL に対応しない可能性があります。

![](../images/queries/datasets-and-tables/dataset-names.png)


データセットテーブル内のスキーマ名をクリックして、UI でデータセットスキーマの階層構造を確認します。

![](../images/queries/datasets-and-tables/schema-information.png)

PSQL コマンドラインを開き、次の場所に記載されている接続の詳細を使用します。[https://platform.adobe.com/query/configuration](https://platform.adobe.com/query/configuration)

![](../images/clients/psql/connect-bi.png)

To view the available tables on [!DNL Platform] with SQL, you can use either `\d` or `SHOW TABLES;`.


`\d` は、標準の PostgreSQL ビューを表示します

```
             List of relations
 Schema |       Name      | Type  |  Owner   
--------+-----------------+-------+----------
 public | luma_midvalues  | table | postgres
 public | luma_postvalues | table | postgres
(2 rows)
```

`SHOW TABLES;`[!DNL Platform] は、より詳細なビューを提供し、テーブルと、 UI にあるデータセット名を表示するカスタムコマンドです。

```
       name      |        dataSetId         |     dataSet    | description | resolved 
-----------------+--------------------------+----------------+-------------+----------
 luma_midvalues  | 5bac030c29bb8d12fa992e58 | Luma midValues |             | false
 luma_postvalues | 5c86b896b3c162151785b43c | Luma midValues |             | false
(2 rows)
```

テーブルのルートスキーマを表示するには、`\d table_name` コマンドを使用します。

>[!NOTE]
>
> 表示されたスキーマは、ルートフィールドを示し、そのほとんどが複雑で、データセットスキーマ　UI　のオブジェクトタイプを参照します。

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

さらに詳細なスキーマを表示するには、アンダースコア（`_`）を使用して、説明するテーブルの列を宣言します。例：`\d table_name_column`。

`\d luma_midvalues_web`

```
                 Composite type "public.luma_midvalues_web"
     Column     |               Type                | Collation | Nullable | Default 
----------------+-----------------------------------+-----------+----------+---------
 webpagedetails | luma_midvalues_web_webpagedetails |           |          | 
 webreferrer    | web_webreferrer                   |           |          | 
```
