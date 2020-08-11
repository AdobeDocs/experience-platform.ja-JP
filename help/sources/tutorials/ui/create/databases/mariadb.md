---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UI での MariaDB ソースコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: 598b29f681ac930a4e1781f7f298608c8344d807
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 17%

---


# Create a [!DNL MariaDB] source connector in the UI

>[!NOTE]
> コネクタ [!DNL MariaDB] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、ユー [!DNL Platform] ザインターフェイスを使用してMaria DBソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [エクスペリエンスデータモデルl（XDM）システム](../../../../../xdm/home.md)[!DNL Experience Platform]： が顧客体験データを整理するための標準化されたフレームワークです。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md)：複数のソースから集約されたデータに基づいて、統合されたリアルタイムのコンシューマープロファイルを提供します。

Maria DBベースの接続が既にある場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/databases.md)。

### 必要な資格情報の収集

で [!DNL Maria DB] アカウントにアクセスするに [!DNL Platform]は、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | MariaDB認証に関連付けられている接続文字列。 MariaDB接続文字列パターンは次のとおりです。 `Server={HOST};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}`. |

MariaDBの使い始めについて詳しくは、 [このドキュメント](https://mariadb.com/kb/en/about-mariadb-connector-odbc/) を参照してください。

## アカウントに接続 [!DNL Maria DB] する

必要な資格情報を収集したら、次の手順に従って新しい受信ベース接続を作成し、ア [!DNL Maria DB] カウントをリンクし [!DNL Platform]ます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左のナビゲーションバーで **[!UICONTROL 「ソース]** 」を選択して「 *[!UICONTROL ソース]* 」ワークスペースにアクセスします。 [ *[!UICONTROL カタログ]* ]画面には、様々なソースが表示され、このソースを使用して受信ベース接続を作成できます。各ソースには、それらに関連付けられた既存のベース接続の数が表示されます。

「 *[!UICONTROL Databases]* 」カテゴリの下で、「 **[!UICONTROL Maria DB]** 」を選択して、情報バーを画面の右側に表示します。 情報バーには、選択したソースの簡単な説明と、ソースまたは表示のドキュメントに接続するためのオプションが表示されます。 新しい受信ベース接続を作成するには、 **[!UICONTROL 追加「]** data」を選択します。

![](../../../../images/tutorials/create/maria-db/catalog.png)

Maria DB *[!UICONTROL に接続]* ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、基本接続に名前、オプションの説明および [!DNL Maria DB] 資格情報を指定します。 終了したら、[ **[!UICONTROL 接続]** ]を選択し、新しいベース接続が確立されるまでの時間を許可します。

![](../../../../images/tutorials/create/maria-db/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Maria DB] アカウントを選択し、「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![](../../../../images/tutorials/create/maria-db/existing.png)

## 次の手順

このチュートリアルに従って、ア [!DNL Maria DB] カウントへの基本的な接続を確立しました。 次のチュートリアルに進み、データをプラットフォームに取り込むようにデータフローを [設定できるようになりました](../../dataflow/databases.md)。