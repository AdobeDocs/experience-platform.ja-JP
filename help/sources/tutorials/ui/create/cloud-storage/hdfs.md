---
keywords: Experience Platform;home;popular topics;Apache HDFS;HDFS;hdfs
solution: Experience Platform
title: UIでApache HDFSソースコネクタを作成する
topic: overview
type: Tutorial
description: このチュートリアルでは、プラットフォームユーザーインターフェイスを使用してApache Hadoop Distributed File System（以下「HDFS」と呼ばれる）ソースコネクタを認証する手順を説明します。
translation-type: tm+mt
source-git-commit: f86f7483e7e78edf106ddd34dc825389dadae26a
workflow-type: tm+mt
source-wordcount: '489'
ht-degree: 6%

---


# Create an [!DNL Apache] HDFS source connector in the UI

>[!NOTE]
>
>HDFS [!DNL Apache] コネクタはベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

のソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を [!DNL Adobe Experience Platform] 提供します。 このチュートリアルでは、ユー [!DNL Apache Hadoop Distributed File System][!DNL Platform] ザーインターフェイスを使用して、HDFS(HDFS)ソースコネクタを認証する手順を説明します。

## はじめに

This tutorial requires a working understanding of the following components of [!DNL Platform]:

- [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 整理する際に使用される標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効なHDFS接続がある場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/batch/cloud-storage.md)。

### 必要な資格情報の収集

HDFSソースコネクタを認証するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `url` | URLは、HDFSへの接続に必要な認証パラメーターを匿名で定義します。 この値の取得方法について詳しくは、HDFSの [HTTPS認証に関する次のドキュメントを参照してください](https://hadoop.apache.org/docs/r1.2.1/HttpAuthentication.html)。 |

## HDFSアカウントの接続

必要な資格情報を収集したら、次の手順に従ってHDFSアカウントをにリンクでき [!DNL Platform]ます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左のナビゲーションバーで **[!UICONTROL 「ソース]** 」を選択して「 **[!UICONTROL ソース]** 」ワークスペースにアクセスします。 [ **[!UICONTROL カタログ]** ]画面には、アカウントを作成する際に使用できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

「 **[!UICONTROL クラウドストレージ]** 」カテゴリで、「 **[!UICONTROL Apache HDFS]**」を選択します。 このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**」を選択します。 それ以外の場合は、 **[!UICONTROL 追加data]** （データ）を選択して新しいHDFSコネクタを作成します。

![カタログ](../../../../images/tutorials/create/hdfs/catalog.png)

「 **[!UICONTROL HDFSに]** 接続」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明、およびHDFS証明書を入力します。 完了したら、「 **[!UICONTROL Connect to source]** 」を選択し、新しい接続が確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/hdfs/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するHDFSアカウントを選択し、 **[!UICONTROL 「次へ]** 」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/hdfs/existing.png)

## 次の手順

このチュートリアルに従って、HDFSアカウントへの接続を確立しました。 次のチュートリアルに進み、クラウドストレージのデータをに取り込むようにデータフローを [設定できます [!DNL Platform]](../../dataflow/batch/cloud-storage.md)。