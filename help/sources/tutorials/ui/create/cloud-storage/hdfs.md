---
keywords: Experience Platform；ホーム；人気の高いトピック；Apache HDFS;HDFS;hdfs
solution: Experience Platform
title: UI での Apache HDFS ソース接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して、ApacheHadoopの分散ファイルシステムソース接続を作成する方法を説明します。
exl-id: 3b8bf210-13b6-44e6-9090-152998f67452
source-git-commit: e37c00863249e677f1645266859bf40fe6451827
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 35%

---

# の作成 [!DNL Apache] UI での HDFS ソース接続

>[!NOTE]
>
>この [!DNL Apache] HDFS コネクタはベータ版です。 詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータ版のコネクタの使用に関する詳細

のソースコネクタ [!DNL Adobe Experience Platform] は、外部ソースのデータをスケジュールに従って取り込む機能を提供します。 このチュートリアルでは、 [!DNL Apache Hadoop Distributed File System] （以下「HDFS」といいます） [!DNL Platform] ユーザーインターフェイス。

## はじめに

このチュートリアルでは、[!DNL Platform] の次のコンポーネントに関する十分な知識が必要です。

- [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な HDFS 接続がある場合は、このドキュメントの残りの部分をスキップして、次のチュートリアルに進んでください。 [データフローの設定](../../dataflow/batch/cloud-storage.md).

### 必要な資格情報の収集

HDFS ソースコネクタを認証するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `url` | URL は、匿名で HDFS に接続するために必要な認証パラメーターを定義します。 この値の取得方法の詳細については、次のドキュメントを参照してください： [HDFS の HTTPS 認証](https://hadoop.apache.org/docs/r1.2.1/HttpAuthentication.html). |

## HDFS アカウントを接続

必要な資格情報を収集したら、以下の手順に従って、HDFS アカウントをにリンクできます。 [!DNL Platform].

にログインします。 [Adobe Experience Platform](https://platform.adobe.com) 次に、 **[!UICONTROL ソース]** 左側のナビゲーションバーから **[!UICONTROL ソース]** ワークスペース。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 **[!UICONTROL クラウドストレージ]** カテゴリ、選択 **[!UICONTROL Apache HDFS]**. このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**. それ以外の場合は、「 **[!UICONTROL データを追加]** 新しい HDFS コネクタを作成します。

![カタログ](../../../../images/tutorials/create/hdfs/catalog.png)

この **[!UICONTROL HDFS に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、オプションの説明、HDFS 資格情報を入力します。 終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![接続](../../../../images/tutorials/create/hdfs/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する HDFS アカウントを選択し、「 」を選択します。 **[!UICONTROL 次へ]** をクリックして続行します。

![既存](../../../../images/tutorials/create/hdfs/existing.png)

## 次の手順

このチュートリアルに従って、HDFS アカウントへの接続を確立しました。 次のチュートリアルに進み、 [データフローを設定して、クラウドストレージからにデータを取り込みます。 [!DNL Platform]](../../dataflow/batch/cloud-storage.md).
