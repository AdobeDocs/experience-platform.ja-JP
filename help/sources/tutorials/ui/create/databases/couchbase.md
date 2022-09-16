---
keywords: Experience Platform；ホーム；人気の高いトピック；Couchbase;couchbase
solution: Experience Platform
title: UI での Couchbase ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用して Couchbase ソース接続を作成する方法を説明します。
exl-id: 4270a48a-843c-4f1e-b280-35b620581d68
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '467'
ht-degree: 50%

---

# UI での [!DNL Couchbase] ソース接続の作成

>[!NOTE]
>
> この [!DNL Couchbase] コネクタはベータ版です。 詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータ版のコネクタの使用に関する詳細

のソースコネクタ [!DNL Adobe Experience Platform] は、外部ソースのデータをスケジュールに従って取り込む機能を提供します。 このチュートリアルでは、 [!DNL Couchbase] を使用したソースコネクタ [!DNL Platform] ユーザーインターフェイス。

## はじめに

このチュートリアルでは、次のコンポーネントに関する十分な知識が必要です。 [!DNL Platform]:

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Couchbase] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/databases.md)に関するチュートリアルに進むことができます。

### 必要な認証情報の収集

を認証するために [!DNL Couchbase] ソースコネクタの場合、次の接続プロパティの値を指定する必要があります。

| 認証情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | の [!DNL Couchbase] インスタンス。 次の接続文字列パターン： [!DNL Couchbase] が `Server={SERVER}; Port={PORT};AuthMech=1;CredString=[{\"user\": \"{USER}\", \"pass\":\"{PASS}\"}];`. 接続文字列の取得について詳しくは、 [[!DNL Couchbase] 接続](https://docs.Couchbase.com/c-sdk/2.10/client-settings.html#configuring-overview). |

## [!DNL Couchbase] アカウントの接続

必要な認証情報が揃ったら、次の手順に従って、[!DNL Couchbase] アカウントを [!DNL Platform] にリンクします。

にログインします。 [Adobe Experience Platform](https://platform.adobe.com) 次に、 **[!UICONTROL ソース]** 左側のナビゲーションバーから **[!UICONTROL ソース]** ワークスペース。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 **[!UICONTROL データベース]** カテゴリ、選択 **[!UICONTROL Couchbase]**. このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**. それ以外の場合は、「 **[!UICONTROL データを追加]** 新しい [!DNL Couchbase] コネクタ。

![カタログ](../../../../images/tutorials/create/couchbase/catalog.png)

この **[!UICONTROL Couchbase に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「 **[!UICONTROL 新しいアカウント]**. 表示される入力フォームで、名前、説明（オプション）および [!DNL Couchbase] 資格情報。 終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![接続](../../../../images/tutorials/create/couchbase/new.png)

### 既存のアカウント

既存のアカウントに接続するには、 [!DNL Couchbase] 接続するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして次に進みます。

![既存](../../../../images/tutorials/create/couchbase/existing.png)

## 次の手順

このチュートリアルでは、[!DNL Couchbase] アカウントとの接続を確立しました。次のチュートリアルに進み、[データを に取り込むためのデータフローの設定 [!DNL Platform]](../../dataflow/databases.md)を行いましょう。
