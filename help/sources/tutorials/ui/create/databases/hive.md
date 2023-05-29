---
keywords: Experience Platform；ホーム；人気のトピック；Apache Hive;Azure HDInsights;azure hdinsights
solution: Experience Platform
title: UI での Azure HDInsights Source Connection での Apache Hive の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して、Azure HDInsights ソース接続で Apache Hive を作成する方法を説明します。
exl-id: 3eb3cb02-9867-451a-b847-ab895310eedf
source-git-commit: e37c00863249e677f1645266859bf40fe6451827
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 49%

---

# の作成 [!DNL Apache Hive] オン [!DNL Azure HDInsights] UI のソース接続

>[!NOTE]
>
> この [!DNL Apache Hive] オン [!DNL Azure HDInsights] コネクタはベータ版です。 詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータ版のコネクタの使用に関する詳細

Adobe Experience Platform のソースコネクタには、外部ソースの データを設定したスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、 [!DNL Apache Hive] オン [!DNL Azure HDInsights] を使用したソースコネクタ [!DNL Platform] ユーザーインターフェイス。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Hive] 接続する場合は、このドキュメントの残りの部分をスキップして、 [データフローの設定](../../dataflow/databases.md)

### 必要な資格情報の収集

 で [!DNL Hive] アカウントにアクセスするには、次の値を指定する必要があります。[!DNL Platform]

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | の IP アドレスまたはホスト名 [!DNL Hive] サーバー。 |
| `username` | 次にアクセスするために使用するユーザー名 [!DNL Hive] サーバー。 |
| `password` | ユーザーに対応するパスワード。 |

の導入について詳しくは、 [この [!DNL Hive] 文書](https://cwiki.apache.org/confluence/display/Hive/Tutorial#Tutorial-GettingStarted).

## [!DNL Hive] アカウントの接続

必要な認証情報が揃ったら、次の手順に従って、[!DNL Hive] アカウントを [!DNL Platform] にリンクします。

にログインします。 [Adobe Experience Platform](https://platform.adobe.com) 次に、 **[!UICONTROL ソース]** 左側のナビゲーションバーから **[!UICONTROL ソース]** ワークスペース。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 **[!UICONTROL データベース]** カテゴリ、選択 **[!UICONTROL Hive]**. このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**. それ以外の場合は、「 **[!UICONTROL データを追加]** 新しい [!DNL Hive] コネクタ。

![カタログ](../../../../images/tutorials/create/hive/catalog.png)

この **[!UICONTROL Hive に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）および [!DNL Hive] 資格情報。 終了したら、「 」を選択します。 **[!UICONTROL 接続]** その後、新しい接続が確立されるまでしばらく時間をかけます。

![接続](../../../../images/tutorials/create/hive/new.png)

### 既存のアカウント

既存のアカウントに接続するには、 [!DNL Hive] 接続するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして続行します。

![既存](../../../../images/tutorials/create/hive/existing.png)

## 次の手順

このチュートリアルでは、[!DNL Hive] アカウントとの接続を確立しました。次のチュートリアルに進み、[データを に取り込むためのデータフローの設定 [!DNL Platform]](../../dataflow/databases.md)を行いましょう。
