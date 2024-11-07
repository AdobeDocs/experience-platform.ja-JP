---
keywords: Experience Platform；ホーム；人気のトピック；Couchbase;couchbase
solution: Experience Platform
title: UI での Couchbase Sourceの連携の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して Couchbase ソース接続を作成する方法を説明します。
exl-id: 4270a48a-843c-4f1e-b280-35b620581d68
source-git-commit: a32d0d7ed7d18454099d2b55b3f6809cfbcd9b62
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 51%

---

# UI での [!DNL Couchbase] ソース接続の作成

>[!IMPORTANT]
>
>[!DNL Couchbase] ソースは 2025 年 5 月末に非推奨（廃止予定）になります。

[!DNL Adobe Experience Platform] のSource コネクタには、外部ソースのデータをスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、[!DNL Platform] ユーザーインターフェイスを使用して [!DNL Couchbase] ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルでは、[!DNL Platform] の次のコンポーネントに関する十分な知識が必要です。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Couchbase] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/databases.md)に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL Couchbase] ソースコネクタを認証するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | [!DNL Couchbase] インスタンスへの接続に使用する接続文字列。 [!DNL Couchbase] の接続文字列のパターンは `Server={SERVER}; Port={PORT};AuthMech=1;CredString=[{\"user\": \"{USER}\", \"pass\":\"{PASS}\"}];` です。 接続文字列の取得について詳しくは、[[!DNL Couchbase] connection](https://docs.Couchbase.com/c-sdk/2.10/client-settings.html#configuring-overview) に関するドキュメントを参照してください。 |

## [!DNL Couchbase] アカウントの接続

必要な資格情報が揃ったら、次の手順に従って、[!DNL Couchbase] アカウントを [!DNL Platform] にリンクします。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから **[!UICONTROL ソース]** を選択して **[!UICONTROL ソース]** ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

**[!UICONTROL Databases]** カテゴリの下で **[!UICONTROL Couchbase]** を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して、新しい [!DNL Couchbase] コネクタを作成します。

![カタログ](../../../../images/tutorials/create/couchbase/catalog.png)

**[!UICONTROL Couchbase に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）、[!DNL Couchbase] 資格情報を入力します。 終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![ 接続 ](../../../../images/tutorials/create/couchbase/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Couchbase] アカウントを選択し、右上隅にある **[!UICONTROL 次へ]** を選択して続行します。

![既存](../../../../images/tutorials/create/couchbase/existing.png)

## 次の手順

このチュートリアルでは、[!DNL Couchbase] アカウントとの接続を確立しました。次のチュートリアルに進み、[ データをに取り込むためのデータフローの設定  [!DNL Platform]](../../dataflow/databases.md) を行いましょう。
