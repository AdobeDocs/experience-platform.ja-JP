---
keywords: Experience Platform；ホーム；人気のトピック；DB2;db2;IBM DB2;ibm db2
solution: Experience Platform
title: UI でのIBM DB2 Source接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用してIBM DB2 ソース接続を作成する方法を説明します。
exl-id: 69c99f94-9cb9-43ff-9315-ce166ab35a60
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '527'
ht-degree: 29%

---

# UI でのIBM DB2 ソースコネクタの作成

>[!NOTE]
>
> IBM DB2 コネクタはベータ版です。 ベータ版のコネクタの使用に関して詳しくは、[&#x200B; ソースの概要 &#x200B;](../../../../home.md#terms-and-conditions) を参照してください。

Adobe Experience PlatformのSource コネクタには、外部ソースのデータをスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、[!DNL Experience Platform] ユーザーインターフェイスを使用して、IBM DB2 （以下「DB2」と呼びます）ソースコネクタを作成する手順について説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な DB2 接続がある場合は、このドキュメントの残りの部分をスキップして、[&#x200B; データフローの設定 &#x200B;](../../dataflow/databases.md) に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

次の節では、[!DNL Flow Service] API を使用して DB2 に正しく接続するために必要な追加情報を示します。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `server` | DB2 サーバーの名前。 サーバー名の後にコロンで区切ったポート番号を指定できます。 例：server:port |
| `database` | DB2 データベースの名前。 |
| `username` | DB2 データベースへの接続に使用するユーザー名。 |
| `password` | ユーザー名に指定したユーザーアカウントのパスワード。 |

基本について詳しくは、[&#x200B; この DB2 ドキュメント &#x200B;](https://www.ibm.com/support/knowledgecenter/SSFMBX/com.ibm.swg.im.dashdb.doc/connecting/connect_credentials.html) を参照してください。

## IBM DB2 アカウントの接続

必要な資格情報を収集したら、次の手順に従って DB2 アカウントを [!DNL Experience Platform] にリンクできます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから **[!UICONTROL ソース]** を選択して **[!UICONTROL ソース]** ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

**[!UICONTROL Databases]** カテゴリで、**[!UICONTROL IBM DB2]** を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して、新しい DB2 コネクタを作成します。

![カタログ](../../../../images/tutorials/create/ibm-db2/catalog.png)

**[!UICONTROL IBM DB2 に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）、DB2 資格情報を入力します。 終了したら「**[!UICONTROL 接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![&#x200B; 接続 &#x200B;](../../../../images/tutorials/create/ibm-db2/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する DB2 アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![既存](../../../../images/tutorials/create/ibm-db2/existing.png)

## 次の手順

このチュートリアルでは、DB2 アカウントとの接続を確立しました。 次のチュートリアルに進み、[&#x200B; データをに取り込むためのデータフローの設定  [!DNL Experience Platform]](../../dataflow/databases.md) を行いましょう。
