---
keywords: Experience Platform；ホーム；人気の高いトピック；Snowflake
title: UI でのSnowflakeソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用してSnowflakeソース接続を作成する方法を説明します。
exl-id: fb2038b9-7f27-4818-b5de-cc8072122127
source-git-commit: 25cc0c5a1e6dcf01b82956ea1022663445315a27
workflow-type: tm+mt
source-wordcount: '445'
ht-degree: 10%

---

# の作成 [!DNL Snowflake] UI のソース接続

このチュートリアルでは、 [!DNL Snowflake] Adobe Experience Platformユーザーインターフェイスを使用したソースコネクタ

## はじめに

このチュートリアルは、 Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、次のコードを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。 [!DNL Platform] サービス。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

### 必要な資格情報の収集

次の場所でSnowflakeアカウントにアクセス [!DNL Platform]に設定する場合は、次の認証値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| アカウント | 次に関連付けられた完全なアカウント名： [!DNL Snowflake] アカウント 完全修飾 [!DNL Snowflake] アカウント名には、アカウント名、地域、クラウドプラットフォームが含まれます。 たとえば、`cj12345.east-us-2.azure` のように設定します。アカウント名について詳しくは、 [[!DNL Snowflake document on account identifiers]](https://docs.snowflake.com/en/user-guide/admin-account-identifier.html). |
| ウェアハウス | この [!DNL Snowflake] warehouse は、アプリケーションのクエリ実行プロセスを管理します。 各 [!DNL Snowflake] ウェアハウスは互いに独立しており、データを Platform に引き渡す際には、個別にアクセスする必要があります。 |
| データベース | この [!DNL Snowflake] データベースには、Platform を取り込むデータが含まれます。 |
| ユーザー名 | のユーザー名 [!DNL Snowflake] アカウント |
| パスワード | のパスワード [!DNL Snowflake] ユーザーアカウント。 |
| 接続文字列 | の [!DNL Snowflake] インスタンス。 次の接続文字列パターン： [!DNL Snowflake] が `jdbc:snowflake://{ACCOUNT_NAME}.snowflakecomputing.com/?user={USERNAME}&password={PASSWORD}&db={DATABASE}&warehouse={WAREHOUSE}` |

これらの値について詳しくは、 [このSnowflake文書](https://docs.snowflake.com/en/user-guide/oauth-custom.html).

## Snowflakeアカウントに接続

Platform UI で、「 **[!UICONTROL ソース]** 左側のナビゲーションから [!UICONTROL ソース] ワークスペース。 この [!UICONTROL カタログ] 画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索バーを使用して、作業対象の特定のソースを見つけることもできます。

以下 [!UICONTROL データベース] カテゴリ、選択 **[!UICONTROL Snowflake]** 次に、 **[!UICONTROL データを追加]**.

![](../../../../images/tutorials/create/snowflake/catalog.png)

この **[!UICONTROL Snowflakeに接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントに接続するには、接続するSnowflakeアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして続行します。

![](../../../../images/tutorials/create/snowflake/existing.png)

### 新しいアカウント

新しい資格情報を使用している場合は、「 **[!UICONTROL 新しいアカウント]**. 表示される入力フォームで、名前、説明（オプション）およびSnowflakeの資格情報を入力します。 終了したら、「 」を選択します。 **[!UICONTROL 接続]** その後、新しい接続が確立されるまでしばらく時間をかけます。

![](../../../../images/tutorials/create/snowflake/new.png)

## 次の手順

このチュートリアルに従って、Snowflakeアカウントへの接続を確立しました。 次のチュートリアルに進み、 [データをに取り込むためのデータフローの設定 [!DNL Platform]](../../dataflow/databases.md).
