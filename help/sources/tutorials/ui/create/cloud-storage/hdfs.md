---
keywords: Experience Platform；ホーム；人気のトピック；Apache HDFS;HDFS;hdfs
solution: Experience Platform
title: UI で Apache HDFS Source接続を作成
type: Tutorial
description: Adobe Experience Platform UI を使用して、Apache Hadoop分散ファイルシステムのソース接続を作成する方法を説明します。
exl-id: 3b8bf210-13b6-44e6-9090-152998f67452
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '487'
ht-degree: 35%

---

# UI での [!DNL Apache] HDFS ソース接続の作成

>[!NOTE]
>
>[!DNL Apache] HDFS コネクタはベータ版です。 ベータ版のコネクタの使用に関して詳しくは、[&#x200B; ソースの概要 &#x200B;](../../../../home.md#terms-and-conditions) を参照してください。

[!DNL Adobe Experience Platform] のSource コネクタには、外部ソースのデータをスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、[!DNL Experience Platform] ユーザーインターフェイスを使用して [!DNL Apache Hadoop Distributed File System] （以下「HDFS」と呼びます）ソースコネクタを認証する手順について説明します。

## はじめに

このチュートリアルでは、[!DNL Experience Platform] の次のコンポーネントに関する十分な知識が必要です。

- [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な HDFS 接続がある場合は、このドキュメントの残りの部分をスキップして、[&#x200B; データフローの設定 &#x200B;](../../dataflow/batch/cloud-storage.md) に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

HDFS ソースコネクタを認証するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `url` | この URL は、HDFS に匿名で接続するために必要な認証パラメーターを定義します。 この値の取得方法について詳しくは、[HDFS の HTTPS 認証 &#x200B;](https://hadoop.apache.org/docs/r1.2.1/HttpAuthentication.html) に関する次のドキュメントを参照してください。 |

## HDFS アカウントの接続

必要な資格情報を収集したら、次の手順に従って HDFS アカウントを [!DNL Experience Platform] にリンクできます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから **[!UICONTROL ソース]** を選択して **[!UICONTROL ソース]** ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

**[!UICONTROL クラウドストレージ]** カテゴリで、「**[!UICONTROL Apache HDFS]**」を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して、新しい HDFS コネクタを作成します。

![カタログ](../../../../images/tutorials/create/hdfs/catalog.png)

**[!UICONTROL HDFS に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）、HDFS 資格情報を入力します。 終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![&#x200B; 接続 &#x200B;](../../../../images/tutorials/create/hdfs/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する HDFS アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![既存](../../../../images/tutorials/create/hdfs/existing.png)

## 次の手順

このチュートリアルでは、HDFS アカウントへの接続を確立しました。 次のチュートリアルに進み、[&#x200B; クラウドストレージからデータをに取り込むためのデータフローの設定  [!DNL Experience Platform]](../../dataflow/batch/cloud-storage.md) を行いましょう。
