---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UI での PostgreSQL ソースコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: 598b29f681ac930a4e1781f7f298608c8344d807
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 18%

---


# Create a [!DNL PostgreSQL] source connector in the UI

>[!NOTE]
> コネクタ [!DNL PostgreSQL] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、ユー [!DNL PostgreSQL] ザーインターフェイスを使用してソースコネクタ [!DNL Platform] （以下「PSQL」と呼ばれます）を作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [エクスペリエンスデータモデルl（XDM）システム](../../../../../xdm/home.md)[!DNL Experience Platform]： が顧客体験データを整理するための標準化されたフレームワークです。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md)：複数のソースから集約されたデータに基づいて、統合されたリアルタイムのコンシューマープロファイルを提供します。

PSQLベースの接続が既に存在する場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/databases.md)。

### 必要な資格情報の収集

でPSQLアカウントにアクセスするに [!DNL Platform]は、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | PSQLアカウントに関連付けられている接続文字列です。 PSQL接続文字列パターンは次のとおりです。 `Server={SERVER};Database={DATABASE};Port={PORT};UID={USERNAME};Password={PASSWORD}`. |

使用を開始する方法の詳細については、この [PSQLドキュメントを参照してください](https://www.postgresql.org/docs/9.2/app-psql.html)。

## PSQLアカウントの接続

必要な資格情報を収集したら、次の手順に従って、PSQLアカウントをリンクする新しい受信ベース接続を作成でき [!DNL Platform]ます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左のナビゲーションバーで **[!UICONTROL 「ソース]** 」を選択して「 *[!UICONTROL ソース]* 」ワークスペースにアクセスします。 [ *[!UICONTROL カタログ]* ]画面には、様々なソースが表示され、このソースを使用して受信ベース接続を作成できます。各ソースには、それらに関連付けられた既存のベース接続の数が表示されます。

「 *[!UICONTROL Databases]* 」カテゴリの下で、「 **[!UICONTROL PostgreSQL DB]** 」を選択して、画面の右側に情報バーを表示します。 情報バーには、選択したソースの簡単な説明と、ソースまたは表示のドキュメントに接続するためのオプションが表示されます。 新しい受信ベース接続を作成するには、 **[!UICONTROL 追加「]** data」を選択します。

![](../../../../images/tutorials/create/psql/catalog.png)

「 *[!UICONTROL PSQLに]* 接続」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、基本接続に名前、オプションの説明、およびPSQL秘密鍵証明書を指定します。 終了したら、[ **[!UICONTROL 接続]** ]を選択し、新しいベース接続が確立されるまでの時間を許可します。

![](../../../../images/tutorials/create/psql/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するPSQLアカウントを選択し、「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![](../../../../images/tutorials/create/psql/existing.png)

## 次の手順

このチュートリアルに従って、PSQLアカウントへの基本接続を確立しました。 次のチュートリアルに進み、データをプラットフォームに取り込むようにデータフローを [設定できるようになりました](../../dataflow/databases.md)。