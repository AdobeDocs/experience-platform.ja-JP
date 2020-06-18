---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでApache HDFSソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 855f543a1cef394d121502f03471a60b97eae256
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 1%

---


# UIでApache HDFSソースコネクタを作成する

>[!NOTE]
>Apache HDFSコネクタはベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

のソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を [!DNL Adobe Experience Platform] 提供します。 このチュートリアルでは、ユー [!DNL Platform] ザーインターフェイスを使用してApache Hadoop Distributed File System（以下「HDFS」と呼ばれる）ソースコネクタを認証する手順を説明します。

## はじめに

このチュートリアルでは、次のコンポーネントについて十分に理解している必要があり [!DNL Platform]ます。

- [Experience Data Model(XDM)System](../../../../../xdm/home.md): 顧客体験データを [!DNL Experience Platform] 整理するための標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成における主な原則とベストプラクティスが含まれます。
   - [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [リアルタイム顧客プロファイル](../../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

HDFS接続が既にある場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/batch/cloud-storage.md)。

### 必要な資格情報の収集

HDFSソースコネクタを認証するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `url` | URLは、HDFSへの接続に必要な認証パラメーターを匿名で定義します。 この値の取得方法について詳しくは、HDFSの [HTTPS認証に関する次のドキュメントを参照してください](https://hadoop.apache.org/docs/r1.2.1/HttpAuthentication.html)。 |

## HDFSアカウントの接続

必要な資格情報を収集したら、次の手順に従って、接続する新しいHDFSアカウントを作成でき [!DNL Platform]ます。

「 [Adobe Experience Platform](https://platform.adobe.com) 」にログインし、左のナビゲーションバーで「 **[!UICONTROL ソース]** 」を選択して「 *[!UICONTROL ソース]* 」ワークスペースにアクセスします。 「 *[!UICONTROL カタログ]* 」画面には様々なソースが表示され、このソースを使用してインバウンドアカウントを作成できます。各ソースには、関連付けられた既存のアカウントおよびデータフローの数が表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

「 *[!UICONTROL クラウドストレージ]* 」カテゴリで、「 **[!UICONTROL Apache HDFS]** 」を選択し、「+」アイコン(+) **** をクリックして新しいHDFSコネクタを作成します。

![カタログ](../../../../images/tutorials/create/hdfs/catalog.png)

「 *[!UICONTROL HDFSに]* 接続」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、接続に名前、オプションの説明、およびファイルストレージの資格情報を入力します。 完了したら、「 **[!UICONTROL Connect to source]** 」を選択し、新しいアカウントが確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/hdfs/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するHDFSアカウントを選択し、 **[!UICONTROL 「次へ]** 」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/hdfs/existing.png)

## 次の手順

このチュートリアルに従って、HDFSアカウントへの接続を確立しました。 次のチュートリアルに進み、クラウドストレージのデータをPlatformに取り込むようにデータフローを [設定できます](../../dataflow/batch/cloud-storage.md)。