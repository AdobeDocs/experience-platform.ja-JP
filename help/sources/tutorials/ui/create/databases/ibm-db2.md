---
keywords: Experience Platform;home;popular topics;DB2;db2;IBM DB2;ibm db2
solution: Experience Platform
title: UIでのIBM DB2ソースコネクターの作成
topic: overview
type: Tutorial
description: このチュートリアルでは、プラットフォームユーザーインターフェイスを使用してIBM DB2（以下「DB2」と呼ばれる）ソースコネクタを作成する手順を説明します。
translation-type: tm+mt
source-git-commit: f86f7483e7e78edf106ddd34dc825389dadae26a
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 8%

---



# UIでのIBM DB2ソースコネクターの作成

>[!NOTE]
>
> IBM DB2 Connectorはベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、ユー [!DNL Platform] ザーインターフェイスを使用してIBM DB2（以下「DB2」と呼ばれる）ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 整理する際に使用される標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効なDB2接続がある場合は、このドキュメントの残りの部分をスキップし、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/databases.md)。

### 必要な資格情報の収集

The following sections provide additional information that you will need to know in order to successfully connect to DB2 using the [!DNL Flow Service] API.

| Credential | 説明 |
| ---------- | ----------- |
| `server` | DB2サーバーの名前。 サーバー名の後に、コロンで区切ったポート番号を指定できます。 次に例を示します。server:port. |
| `database` | DB2データベースの名前。 |
| `username` | DB2データベースへの接続に使用するユーザー名。 |
| `password` | ユーザー名に指定したユーザーアカウントのパスワード。 |

使い始める前に、 [このDB2ドキュメントを参照してください](https://www.ibm.com/support/knowledgecenter/SSFMBX/com.ibm.swg.im.dashdb.doc/connecting/connect_credentials.html)。

## IBM DB2アカウントの接続

必要な資格情報を収集したら、次の手順に従ってDB2アカウントをにリンクでき [!DNL Platform]ます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左のナビゲーションバーで **[!UICONTROL 「ソース]** 」を選択して「 **[!UICONTROL ソース]** 」ワークスペースにアクセスします。 [ **[!UICONTROL カタログ]** ]画面には、アカウントを作成する際に使用できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

「 **[!UICONTROL Databases]** 」カテゴリで「 **[!UICONTROL IBM DB2]**」を選択します。 このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**」を選択します。 それ以外の場合は、 **[!UICONTROL 追加data]** （データ）を選択して新しいDB2コネクタを作成します。

![カタログ](../../../../images/tutorials/create/ibm-db2/catalog.png)

「IBM DB2 **[!UICONTROL に接続]** 」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明、およびDB2証明書を入力します。 終了したら、 **[!UICONTROL [接続]** ]を選択し、新しい接続が確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/ibm-db2/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するDB2アカウントを選択し、「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/ibm-db2/existing.png)

## 次の手順

このチュートリアルに従って、DB2アカウントへの接続を確立しました。 次のチュートリアルに進み、データを取り込むデータフローを [設定できます [!DNL Platform]](../../dataflow/databases.md)。