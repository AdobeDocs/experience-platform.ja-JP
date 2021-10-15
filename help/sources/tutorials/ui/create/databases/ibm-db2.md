---
keywords: Experience Platform；ホーム；人気のあるトピック；DB2;DB2;IBM DB2;ibm db2
solution: Experience Platform
title: UI でのIBM DB2 ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用してIBM DB2 ソース接続を作成する方法を説明します。
exl-id: 69c99f94-9cb9-43ff-9315-ce166ab35a60
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 11%

---

# UI でのIBM DB2 ソース接続の作成

>[!NOTE]
>
> IBM DB2 コネクタはベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ ソースの概要 ](../../../../home.md#terms-and-conditions)」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに従って取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform] ユーザーインターフェイスを使用してIBM DB2（以下「DB2」と呼びます）ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを整理する際に使用す [!DNL Experience Platform] る標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターの UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な DB2 接続がある場合は、このドキュメントの残りの部分をスキップし、[ データフローの設定 ](../../dataflow/databases.md) に関するチュートリアルに進んでください。

### 必要な資格情報の収集

以下の節では、[!DNL Flow Service] API を使用して DB2 に正しく接続するために知っておく必要がある追加情報を示します。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `server` | DB2 サーバーの名前。 サーバー名の後にコロンで区切ったポート番号を指定できます。 例：server:port. |
| `database` | DB2 データベースの名前。 |
| `username` | DB2 データベースへの接続に使用するユーザー名。 |
| `password` | ユーザー名に指定したユーザーアカウントのパスワード。 |

使い始める方法については、[ この DB2 ドキュメント ](https://www.ibm.com/support/knowledgecenter/SSFMBX/com.ibm.swg.im.dashdb.doc/connecting/connect_credentials.html) を参照してください。

## IBM DB2 アカウントの接続

必要な資格情報を収集したら、以下の手順に従って DB2 アカウントを [!DNL Platform] にリンクします。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから「**[!UICONTROL ソース]**」を選択して、「**[!UICONTROL ソース]**」ワークスペースにアクセスします。 **[!UICONTROL カタログ]** 画面には、アカウントを作成するための様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、目的の特定のソースを見つけることもできます。

「**[!UICONTROL Databases]**」カテゴリで、「**[!UICONTROL IBM DB2]**」を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL Add data]**」を選択して新しい DB2 コネクタを作成します。

![カタログ](../../../../images/tutorials/create/ibm-db2/catalog.png)

「IBM DB2 に接続 ]**」ページが表示されます。**[!UICONTROL &#x200B;このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新規アカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、説明（オプション）、DB2 資格情報を入力します。 終了したら、[**[!UICONTROL 接続]**] を選択し、新しい接続が確立されるまでしばらく時間をかけます。

![接続](../../../../images/tutorials/create/ibm-db2/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する DB2 アカウントを選択し、**[!UICONTROL 次へ]** を選択して次に進みます。

![既存](../../../../images/tutorials/create/ibm-db2/existing.png)

## 次の手順

このチュートリアルに従って、DB2 アカウントへの接続を確立しました。 次のチュートリアルに進み、[ にデータを取り込むようにデータフローを設定します。 [!DNL Platform]](../../dataflow/databases.md)
