---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでのIBM DB2ソースコネクターの作成
topic: overview
translation-type: tm+mt
source-git-commit: 41fe3e5b2a830c3182b46b3e0873b1672a1f1b03
workflow-type: tm+mt
source-wordcount: '529'
ht-degree: 14%

---



# UIでのIBM DB2ソースコネクターの作成

>[!NOTE]
> IBM DB2 Connectorはベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、ユー [!DNL Platform] ザーインターフェイスを使用してIBM DB2（以下「DB2」と呼ばれる）ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [エクスペリエンスデータモデルl（XDM）システム](../../../../../xdm/home.md)[!DNL Experience Platform]： が顧客体験データを整理するための標準化されたフレームワークです。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md)：複数のソースから集約されたデータに基づいて、統合されたリアルタイムのコンシューマープロファイルを提供します。

既に有効なDB2接続がある場合は、このドキュメントの残りの部分をスキップし、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/databases.md)。

### 必要な資格情報の収集

The following sections provide additional information that you will need to know in order to successfully connect to DB2 using the [!DNL Flow Service] API.

| Credential | 説明 |
| ---------- | ----------- |
| `server` | DB2サーバーの名前。 サーバー名の後に、コロンで区切ったポート番号を指定できます。 次に例を示します。server:port. |
| `database` | DB2データベースの名前。 |
| `username` | DB2データベースへの接続に使用するユーザー名。 |
| `password` | ユーザー名に指定したユーザーアカウントのパスワード。 |

使い始める前に詳しくは、 [このDB2ドキュメントを参照してください](https://www.ibm.com/support/knowledgecenter/SSFMBX/com.ibm.swg.im.dashdb.doc/connecting/connect_credentials.html)。

## IBM DB2アカウントの接続

必要な資格情報を収集したら、次の手順に従って、接続先の新しいDB2アカウントを作成でき [!DNL Platform]ます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左のナビゲーションバーで **[!UICONTROL 「ソース]** 」を選択して「 *[!UICONTROL ソース]* 」ワークスペースにアクセスします。 カ *[!UICONTROL タログ]* 画面には様々なソースが表示され、このソースを使用してインバウンドアカウントを作成できます。各ソースには既存のアカウントの数と関連するデータセットフローが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

「 *[!UICONTROL Databases]* 」 **[!UICONTROL カテゴリで、「]** IBM DB2 **** 」を選択し、次に新しいDB2コネクターを作成するデータを選択します。

![カタログ](../../../../images/tutorials/create/ibm-db2/catalog.png)

「IBM DB2 *[!UICONTROL に接続]* 」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、接続に名前、オプションの説明、およびDB2証明書を入力します。 完了したら、[ **[!UICONTROL 接続]** ]を選択し、新しいアカウントが確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/ibm-db2/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するDB2アカウントを選択し、「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/ibm-db2/existing.png)

## 次の手順

このチュートリアルに従って、DB2アカウントへの接続を確立しました。 次のチュートリアルに進み、データをプラットフォームに取り込むようにデータフローを [設定できるようになりました](../../dataflow/databases.md)。