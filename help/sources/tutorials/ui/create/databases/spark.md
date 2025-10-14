---
keywords: Experience Platform；ホーム；人気のトピック；Azure HDInsights;Apache Spark
solution: Experience Platform
title: UI での Azure HDInsights Source接続上の Apache Spark の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して、Azure HDInsights ソース接続で Apache Spark を作成する方法を説明します。
exl-id: 30d0b740-cec4-486f-9c9b-1579fd04f28b
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '489'
ht-degree: 37%

---

# UI でのソース接続 [!DNL Azure HDInsights] の [!DNL Apache Spark] の作成

>[!NOTE]
>
> [!DNL Azure HDInsights] コネクタの [!DNL Apache Spark] はベータ版です。 ベータ版のコネクタの使用に関して詳しくは、[&#x200B; ソースの概要 &#x200B;](../../../../home.md#terms-and-conditions) を参照してください。

Adobe Experience PlatformのSource コネクタには、外部ソースのデータをスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、[!DNL Experience Platform] ユーザーインターフェイス [!DNL Azure HDInsights] 使用してソースコネクタ上に [!DNL Apache Spark] を作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [エクスペリエンスデータモデルl（XDM）システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データを整理するための標準化されたフレームワークです。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md)：複数のソースから集計したデータに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Spark] 接続がある場合は、このドキュメントの残りの部分をスキップして、[&#x200B; データフローの設定 &#x200B;](../../dataflow/databases.md) に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL Experience Platform] で [!DNL Spark] アカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | [!DNL Spark] サーバーの IP アドレスまたはホスト名。 |
| `username` | [!DNL Spark] サーバーへのアクセスに使用するユーザー名。 |
| `password` | ユーザーに対応するパスワード。 |

基本について詳しくは、[&#x200B; この Spark ドキュメント &#x200B;](https://docs.microsoft.com/en-us/azure/hdinsight/spark/apache-spark-overview) を参照してください。

## [!DNL Spark] アカウントを接続

必要な資格情報を収集したら、次の手順に従って [!DNL Spark] アカウントを [!DNL Experience Platform] に接続するようにリンクできます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから **[!UICONTROL ソース]** を選択して **[!UICONTROL ソース]** ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

「**[!UICONTROL データベース]**」カテゴリで、「**[!UICONTROL Spark]**」を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して、新しい [!DNL Spark] コネクタを作成します。

![カタログ](../../../../images/tutorials/create/spark/catalog.png)

**[!UICONTROL Spark に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）、[!DNL Spark] 資格情報を入力します。 終了したら「**[!UICONTROL 接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![新規](../../../../images/tutorials/create/spark/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Spark] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![既存](../../../../images/tutorials/create/spark/existing.png)

## 次の手順

このチュートリアルでは、[!DNL Spark] アカウントとの接続を確立しました。次のチュートリアルに進み、[&#x200B; データをに取り込むためのデータフローの設定  [!DNL Experience Platform]](../../dataflow/databases.md) を行いましょう。
