---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでApache HDFSソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: d3c725c4760acb3857a67d0d30b24732c963a030
workflow-type: tm+mt
source-wordcount: '483'
ht-degree: 12%

---


# Create an [!DNL Apache] HDFS source connector in the UI

>[!NOTE]
>HDFS [!DNL Apache] コネクタはベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

のソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を [!DNL Adobe Experience Platform] 提供します。 このチュートリアルでは、ユー [!DNL Apache Hadoop Distributed File System][!DNL Platform] ザーインターフェイスを使用して、HDFS(HDFS)ソースコネクタを認証する手順を説明します。

## はじめに

This tutorial requires a working understanding of the following components of [!DNL Platform]:

- [エクスペリエンスデータモデルl（XDM）システム](../../../../../xdm/home.md)[!DNL Experience Platform]： が顧客体験データを整理するための標準化されたフレームワークです。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [リアルタイム顧客プロファイル](../../../../../profile/home.md)：複数のソースから集約されたデータに基づいて、統合されたリアルタイムのコンシューマープロファイルを提供します。

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