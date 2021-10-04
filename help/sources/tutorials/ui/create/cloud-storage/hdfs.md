---
keywords: Experience Platform；ホーム；人気のあるトピック；Apache HDFS;HDFS;hdfs
solution: Experience Platform
title: UI での Apache HDFS ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用して、ApacheHadoopの分散ファイルシステムソース接続を作成する方法を説明します。
exl-id: 3b8bf210-13b6-44e6-9090-152998f67452
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 9%

---

# UI で [!DNL Apache] HDFS ソース接続を作成する

>[!NOTE]
>
>[!DNL Apache] HDFS コネクタはベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ ソースの概要 ](../../../../home.md#terms-and-conditions)」を参照してください。

[!DNL Adobe Experience Platform] のソースコネクタは、外部ソースのデータをスケジュールに従って取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform] ユーザーインターフェイスを使用して [!DNL Apache Hadoop Distributed File System]（以下「HDFS」と呼びます）ソースコネクタを認証する手順を説明します。

## はじめに

このチュートリアルでは、[!DNL Platform] の次のコンポーネントに関する十分な知識が必要です。

- [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを整理する際に使用す [!DNL Experience Platform] る標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターの UI を使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な HDFS 接続がある場合は、このドキュメントの残りの部分をスキップし、[ データフローの設定 ](../../dataflow/batch/cloud-storage.md) に関するチュートリアルに進んでください。

### 必要な資格情報の収集

HDFS ソースコネクタを認証するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `url` | URL は、HDFS に匿名で接続するために必要な認証パラメータを定義します。 この値の取得方法の詳細は、[HDFS の HTTPS 認証 ](https://hadoop.apache.org/docs/r1.2.1/HttpAuthentication.html) に関する次のドキュメントを参照してください。 |

## HDFS アカウントの接続

必要な資格情報を収集したら、以下の手順に従って HDFS アカウントを [!DNL Platform] にリンクします。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから「**[!UICONTROL ソース]**」を選択して、「**[!UICONTROL ソース]**」ワークスペースにアクセスします。 **[!UICONTROL カタログ]** 画面には、アカウントを作成するための様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、目的の特定のソースを見つけることもできます。

「**[!UICONTROL クラウドストレージ]**」カテゴリで、「**[!UICONTROL Apache HDFS]**」を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL Add data]**」を選択して新しい HDFS コネクタを作成します。

![カタログ](../../../../images/tutorials/create/hdfs/catalog.png)

[**[!UICONTROL HDFS に接続]**] ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新規アカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、説明（オプション）、HDFS 資格情報を入力します。 終了したら、[**[!UICONTROL ソースに接続]**] を選択し、新しい接続が確立されるまでしばらく時間をかけます。

![接続](../../../../images/tutorials/create/hdfs/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する HDFS アカウントを選択し、**[!UICONTROL 次へ]** を選択して次に進みます。

![既存](../../../../images/tutorials/create/hdfs/existing.png)

## 次の手順

このチュートリアルに従って、HDFS アカウントへの接続を確立しました。 次のチュートリアルに進み、クラウドストレージから  [!DNL Platform]](../../dataflow/batch/cloud-storage.md) にデータを取り込むように [ データフローを設定します。
