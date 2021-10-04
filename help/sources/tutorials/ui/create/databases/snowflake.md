---
keywords: Experience Platform；ホーム；人気のあるトピック；Snowflake
title: UI でのSnowflakeソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用してSnowflakeソース接続を作成する方法を説明します。
exl-id: fb2038b9-7f27-4818-b5de-cc8072122127
source-git-commit: db483110b8bfd5290f6a9a30fdb008f478fdbbf4
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 10%

---

# UI での [!DNL Snowflake] ソース接続の作成

>[!NOTE]
>
> [!DNL Snowflake] コネクタはベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ ソースの概要 ](../../../../home.md#terms-and-conditions)」を参照してください。

このチュートリアルでは、Adobe Experience Platformユーザーインターフェイスを使用して [!DNL Snowflake] ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、 Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して、受信データの構造化、ラベル付け、強化をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

### 必要な資格情報の収集

[!DNL Platform] のSnowflakeアカウントにアクセスするには、次の認証値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| アカウント | Platform に接続する [!DNL Snowflake] アカウント。 |
| 倉庫 | [!DNL Snowflake] ウェアハウスは、アプリケーションのクエリ実行プロセスを管理します。 各 [!DNL Snowflake] ウェアハウスは互いに独立しており、データを Platform に引き渡す際には個別にアクセスする必要があります。 |
| データベース | [!DNL Snowflake] には、プラットフォームに取り込むデータが含まれます。 |
| ユーザー名 | [!DNL Snowflake] アカウントのユーザー名。 |
| パスワード | [!DNL Snowflake] ユーザーアカウントのパスワード。 |
| 接続文字列 | [!DNL Snowflake] インスタンスへの接続に使用する接続文字列。 [!DNL Snowflake] の接続文字列パターンは `jdbc:snowflake://{ACCOUNT_NAME}.snowflakecomputing.com/?user={USERNAME}&password={PASSWORD}&db={DATABASE}&warehouse={WAREHOUSE}` です。 |

これらの値の詳細については、[ このSnowflakeドキュメント ](https://docs.snowflake.com/en/user-guide/oauth-custom.html) を参照してください。

## Snowflakeアカウントの接続

Platform UI で、左のナビゲーションから「 **[!UICONTROL ソース]** 」を選択して、「 [!UICONTROL  ソース ] 」ワークスペースにアクセスします。 [!UICONTROL  カタログ ] 画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索バーを使用して、作業対象の特定のソースを見つけることもできます。

「[!UICONTROL Databases]」カテゴリで「**[!UICONTROL Snowflake]**」を選択し、「**[!UICONTROL データを追加]**」を選択します。

![](../../../../images/tutorials/create/snowflake/catalog.png)

「**[!UICONTROL Snowflakeに接続]**」ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントに接続するには、接続するSnowflakeアカウントを選択し、**[!UICONTROL 次へ]** を選択して次に進みます。

![](../../../../images/tutorials/create/snowflake/existing.png)

### 新規アカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、説明（オプション）およびSnowflakeの資格情報を入力します。 終了したら、[**[!UICONTROL 接続]**] を選択し、新しい接続が確立されるまでしばらく時間をかけます。

![](../../../../images/tutorials/create/snowflake/new.png)

## 次の手順

このチュートリアルに従って、Snowflakeアカウントへの接続を確立しました。 次のチュートリアルに進み、[ にデータを取り込むようにデータフローを設定します。 [!DNL Platform]](../../dataflow/databases.md)
