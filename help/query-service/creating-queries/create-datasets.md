---
keywords: Experience Platform;home;popular topics;query service;Query service;generate datasets;generate dataset;create dataset;
solution: Experience Platform
title: データセットをクエリ結果から生成する
topic: queries
translation-type: tm+mt
source-git-commit: 17ef6c1c6ce58db2b65f1769edf719b98d260fc6
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 56%

---


# データセットをクエリ結果から生成する

の真の力 [!DNL Query Service] は、クエリを使用してデータセットを生成し、より多くのクエリへの入力として、または [!DNL Data Lake] 、 [!DNL Data Science Workspace]、などの他のサービスで使用する場合に明らかになり [!DNL Real-time Customer Profile][!DNL Analysis Workspace]ます。

[!DNL Query Service] UIからデータセットを作成できます。 次の手順に従います。

1. 接続されたクライアントを使用してクエリを記述し、出力を検証します。
2. Log in to the [!DNL Platform] UI and go to Queries.
3. リストでクエリを探し、その行にカーソルを合わせます。
4. 「**[!UICONTROL データセットを作成]**」をクリックします。![画像](../images/queries/create-datasets/click-create-dataset.png)
5. LDAP ID の後にデータセット名を入力します（一意である必要も、SQL セーフである必要もありません。システムは、ここで指定した名前に基づいて「テーブル名」を生成します）。
6. データセットの説明を入力し、「**[!UICONTROL クエリを実行]**」をクリックします。![画像](../images/queries/create-datasets/run-query.png)
7. クエリの完了を確認し、データセットのリストページに移動して、作成したデータセットを確認します。

After a dataset is created, it can be accessed like any other dataset in the [!DNL Data Lake] and used for a variety of use cases.

>[!NOTE]
>
>In a live implementation, you must apply [!DNL Data Governance] labels after the dataset is created.

## Generate datasets with a pre-defined [!DNL Experience Data Model] schema

In order to generate a dataset with a pre-defined [!DNL Experience Data Model] (XDM) schema, you will have to use the SQL syntax. 使用する構文について詳しくは、[SQL 構文のガイド](../sql/syntax.md#create-table-as-select)を参照してください。

## データセットを出力する

この機能を使用して作成されるデータセットは、SQL 文で定義されているように、出力データの構造と一致するアドホックスキーマを使用して生成されます。Some downstream services require datasets with particular [!DNL Experience Data Model] (XDM) schemas. ダウンストリームサービスのデータ形式設定要件を確認してから、クエリを記述してください。