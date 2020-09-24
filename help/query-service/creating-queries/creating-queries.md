---
keywords: Experience Platform;home;popular topics;query service;Query service;create queries;
solution: Experience Platform
title: クエリの作成
topic: queries
type: Tutorial
translation-type: tm+mt
source-git-commit: 97dfd3a9a66fe2ae82cec8954066bdf3b6346830
workflow-type: tm+mt
source-wordcount: '114'
ht-degree: 17%

---


# クエリの作成

Adobe Experience Platform [!DNL Query Service] は、内のデータセットに対してSQLクエリを実行する機能 [!DNL Data Lake] を提供 [!DNL Experience Platform]します。 As you use SQL to interact with datasets in the Data Lake, it is important to understand that [!DNL Query Service] automatically manages certain aspects, such as creating SQL-safe table names for each dataset in the [!DNL Data Lake]. There are also considerations around working with hierarchical data in the [!DNL Data Lake], including discovering the schema upon which a dataset is based and ensuring that you are selecting the correct field within the hierarchical model.

The following documentation will help you to better understand core concepts within [!DNL Query Service]:

- [データセット vs テーブルとスキーマ](./datasets-and-tables.md)
- [クエリ執筆のための一般的なガイダンス](./writing-queries.md)
- [ExperienceEvent クエリ](./experience-event-queries.md)
- [データセットの結合](./joining-datasets.md)
- [データの重複排除](./deduplication.md)
