---
keywords: Experience Platform；ホーム；人気のあるトピック；Azure HDInsights;Apache Spark
solution: Experience Platform
title: UI での Azure HDInsights Source Connection での Apache Spark の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用して、Azure HDInsights ソース接続で Apache Spark を作成する方法を説明します。
exl-id: 30d0b740-cec4-486f-9c9b-1579fd04f28b
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 15%

---

# UI で [!DNL Azure HDInsights] ソース接続に [!DNL Apache Spark] を作成します

>[!NOTE]
>
> [!DNL Azure HDInsights] コネクタの [!DNL Apache Spark] はベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ ソースの概要 ](../../../../home.md#terms-and-conditions)」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに従って取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform] ユーザーインターフェイスを使用して [!DNL Azure HDInsights] ソースコネクタに [!DNL Apache Spark] を作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [エクスペリエンスデータモデルl（XDM）システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データを整理するための標準化されたフレームワークです。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターの UI を使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md)：複数のソースから集約されたデータに基づいて、統合されたリアルタイムのコンシューマープロファイルを提供します。

既に有効な [!DNL Spark] 接続がある場合は、このドキュメントの残りの部分をスキップし、[ データフローの設定 ](../../dataflow/databases.md) に関するチュートリアルに進んでください。

### 必要な資格情報の収集

[!DNL Platform] の [!DNL Spark] アカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | [!DNL Spark] サーバの IP アドレスまたはホスト名。 |
| `username` | [!DNL Spark] サーバーへのアクセスに使用するユーザー名。 |
| `password` | ユーザーに対応するパスワード。 |

使い始める方法について詳しくは、[ この Spark ドキュメント ](https://docs.microsoft.com/en-us/azure/hdinsight/spark/apache-spark-overview) を参照してください。

## [!DNL Spark] アカウントに接続

必要な資格情報を収集したら、以下の手順に従って [!DNL Spark] アカウントをリンクし、[!DNL Platform] に接続します。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから「**[!UICONTROL ソース]**」を選択して、「**[!UICONTROL ソース]**」ワークスペースにアクセスします。 **[!UICONTROL カタログ]** 画面には、アカウントを作成するための様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、目的の特定のソースを見つけることもできます。

「**[!UICONTROL データベース]**」カテゴリで、「**[!UICONTROL Spark]**」を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して新しい [!DNL Spark] コネクタを作成します。

![カタログ](../../../../images/tutorials/create/spark/catalog.png)

**[!UICONTROL Spark に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新規アカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、説明（オプション）、[!DNL Spark] 資格情報を入力します。 終了したら、[**[!UICONTROL 接続]**] を選択し、新しい接続が確立されるまでしばらく時間をかけます。

![新規](../../../../images/tutorials/create/spark/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Spark] アカウントを選択し、**[!UICONTROL 次へ]** を選択して次に進みます。

![既存](../../../../images/tutorials/create/spark/existing.png)

## 次の手順

このチュートリアルに従って、[!DNL Spark] アカウントへの接続を確立しました。 次のチュートリアルに進み、[ にデータを取り込むようにデータフローを設定します。 [!DNL Platform]](../../dataflow/databases.md)
