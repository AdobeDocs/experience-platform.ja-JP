---
title: UI での Customer.io ソース接続とデータフローの作成
description: Adobe Experience Platform UI を使用して Customer.io ソース接続を作成する方法を説明します。
badge: ベータ版
exl-id: 7655a34c-808a-46e3-94e3-022a433755a4
source-git-commit: b66a50e40aaac8df312a2c9a977fb8d4f1fb0c80
workflow-type: tm+mt
source-wordcount: '1232'
ht-degree: 20%

---

# の作成 [!DNL Customer.io] UI でのソース接続とデータフロー

>[!NOTE]
>
>[!DNL Customer.io] ソースはベータ版です。詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

このチュートリアルでは、 [!DNL Customer.io] Adobe Experience Platformユーザーインターフェイスを使用したソース接続とデータフロー。

## はじめに {#getting-started}

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

## 前提条件 {#prerequisites}

次の節では、 [!DNL Customer.io] ソース接続。

### のソーススキーマを定義するサンプル JSON [!DNL Customer.io] {#prerequisites-json-schema}

を作成する前に [!DNL Customer.io] ソース接続の場合は、ソーススキーマを指定する必要があります。 以下の JSON を使用できます。

```
{
  "event_id": "01E4C4CT6YDC7Y5M7FE1GWWPQJ",
  "object_type": "customer",
  "metric": "subscribed",
  "timestamp": 1613063089,
  "data": {
    "customer_id": "42",
    "email_address": "test@example.com",
    "identifiers": {
      "id": "42",
      "email": "test@example.com",
      "cio_id": "d9c106000001"
    }
  }
}
```

### 用の Platform スキーマの作成 [!DNL Customer.io] {#create-platform-schema}

また、ソースに使用する Platform スキーマを作成する必要があります。 に関するチュートリアルを参照してください。 [Platform スキーマの作成](../../../../../xdm/schema/composition.md) スキーマの作成方法に関する包括的な手順を参照してください。

![Customer.io のスキーマの例を示す Platform UI のスクリーンショット](../../../../images/tutorials/create/marketing-automation/customerio-webhook/schema.png)

## [!DNL Customer.io] アカウントを接続 {#connect-account}

Platform UI で、「 」を選択します。 **[!UICONTROL ソース]** 左側のナビゲーションから、 [!UICONTROL ソース] workspace を参照し、Experience Platformで使用可能なソースのカタログを確認します。

以下を使用します。 *[!UICONTROL カテゴリ]* メニューを使用して、ソースをカテゴリでフィルタリングできます。 または、検索バーにソース名を入力して、カタログから特定のソースを検索します。

次に移動： [!UICONTROL マーケティングの自動化] 表示するカテゴリ [!DNL Customer.io] ソースカード。 最初に、「 」を選択します。 **[!UICONTROL データを追加]**.

![Customer.io カードを含むカタログの Platform UI スクリーンショット](../../../../images/tutorials/create/marketing-automation/customerio-webhook/catalog.png)

## データの選択 {#select-data}

The **[!UICONTROL データを選択]** の手順が表示され、Platform に取り込むデータを選択するためのインターフェイスが提供されます。

* インターフェイスの左側には、アカウント内で使用可能なデータストリームを表示できるブラウザーがあります。
* インターフェイスの右側では、JSON ファイルから最大 100 行のデータをプレビューできます。

選択 **[!UICONTROL ファイルをアップロード]** をクリックして、ローカルシステムから JSON ファイルをアップロードします。 または、アップロードする JSON ファイルをにドラッグ&amp;ドロップすることもできます [!UICONTROL ファイルをドラッグ&amp;ドロップ] パネル。

![ソースワークフローのデータ追加手順。](../../../../images/tutorials/create/marketing-automation/customerio-webhook//add-data.png)

ファイルがアップロードされると、プレビューインターフェイスが更新され、アップロードしたスキーマのプレビューが表示されます。 プレビューインターフェイスを使用すると、ファイルの内容と構造を検査できます。 また、 [!UICONTROL 検索フィールド] スキーマ内から特定の項目にアクセスするユーティリティ。

終了したら、「**[!UICONTROL 次へ]**」を選択します。

![ソースワークフローのプレビューステップ。](../../../../images/tutorials/create/marketing-automation/customerio-webhook//preview.png)

## データフローの詳細 {#dataflow-detail}

The **データフローの詳細** 手順が表示され、既存のデータセットを使用するか、データフローの新しいデータセットを確立するか、およびデータフローの名前と説明を指定する機会が提供されます。 この手順では、プロファイルの取り込み、エラー診断、部分取り込み、アラートの設定も指定できます。

終了したら、「**[!UICONTROL 次へ]**」を選択します。

![ソースワークフローのデータフローの詳細ステップ。](../../../../images/tutorials/create/marketing-automation/customerio-webhook//dataflow-detail.png)

## マッピング {#mapping}

[!UICONTROL マッピング]手順が表示され、ソーススキーマのソースフィールドを、ターゲットスキーマの適切なターゲット XDM フィールドにマッピングするためのインターフェイスが提供されます。

Platform は、選択したターゲットスキーマまたはデータセットに基づいて、自動マッピングされたフィールドに対するインテリジェントなレコメンデーションを提供します。 マッピングルールは、ユースケースに合わせて手動で調整できます。必要に応じて、フィールドを直接マッピングするか、データ準備機能を使用してソースデータを変換して計算値を導き出すかを選択できます。マッパーインターフェイスと計算フィールドを使用した包括的な手順については、 [データ準備 UI ガイド](../../../../../data-prep/ui/mapping.md).

以下に示すすべてのマッピングは必須で、 [!UICONTROL レビュー] ステージ。

| ターゲットフィールド | 説明 |
| --- | --- |
| `object_type` | オブジェクトのタイプ ( [!DNL Customer.io] [イベント](https://customer.io/docs/webhooks/#events) サポートされるタイプに関するドキュメント。 |
| `id` | オブジェクトの識別子。 |
| `email` | オブジェクトに関連付けられている電子メールアドレス。 |
| `event_id` | イベントの一意の ID。 |
| `cio_id` | The [!DNL Customer.io] イベントの識別子。 |
| `metric` | イベントタイプ. 詳しくは、 [!DNL Customer.io] [イベント](https://customer.io/docs/webhooks/#events) サポートされるタイプに関するドキュメント。 |
| `timestamp` | イベントが発生したときのタイムスタンプ。 |

>[!IMPORTANT]
>
>マッピングしない `cio_id` 実行時 [!DNL Customer.io] ウェブフック `test mode` 関連付けられたフィールドが送信されないので [!DNL Customer.io].

ソースデータが正常にマッピングされたら、「 」を選択します。 **[!UICONTROL 次へ]**.

![ソースワークフローのマッピング手順です。](../../../../images/tutorials/create/marketing-automation/customerio-webhook/mapping.png)

## レビュー {#review}

**[!UICONTROL レビュー]**&#x200B;手順が表示され、新しいデータフローを作成する前に確認できます。詳細は、次のカテゴリに分類されます。

* **[!UICONTROL 接続]**：ソースのタイプ、選択したソースファイルの関連パスおよびそのソースファイル内の列の数を表示します。
* **[!UICONTROL データセットの割り当てとフィールドのマッピング]**：ソースデータがどのデータセットに取り込まれるかを、そのデータセットが準拠するスキーマを含めて表示します。

データフローをレビューしたら、「**[!UICONTROL 終了]**」を選択し、データフローが作成されるまでしばらく待ちます。

![ソースワークフローのレビューステップ。](../../../../images/tutorials/create/marketing-automation/customerio-webhook/review.png)

## ストリーミングエンドポイント URL を取得する {#get-streaming-endpoint}

ストリーミングデータフローを作成したら、ストリーミングエンドポイント URL を取得できます。 このエンドポイントは、Webhook を購読するために使用され、ストリーミングソースとExperience Platformが通信できます。

Webhook の設定に使用する URL を構築するために [!DNL Customer.io] 次を取得する必要があります。

* **[!UICONTROL データフロー ID]**
* **[!UICONTROL ストリーミングエンドポイント]**

次の手順で **[!UICONTROL データフロー ID]** および **[!UICONTROL ストリーミングエンドポイント]**、に移動します。 [!UICONTROL データフローアクティビティ] 作成したデータフローのページを開き、詳細を [!UICONTROL プロパティ] パネル。

![データフローアクティビティのストリーミングエンドポイント。](../../../../images/tutorials/create/marketing-automation/customerio-webhook/endpoint-test.png)

ストリーミングエンドポイントとデータフロー ID を取得したら、次のパターンに基づいて URL を作成します。 ```{STREAMING_ENDPOINT}?x-adobe-flow-id={DATAFLOW_ID}```. 例えば、生成される Webhook URL は次のようになります。 ``https://dcs.adobedc.net/collection/febc116d22ba0ea2868e9c93b199375302afb8a589617700991bb8f3f0341ad7?x-adobe-flow-id=439b3fc4-3042-4a3a-b5e0-a494898d3fb0``

## でのレポート Webhook の設定 [!DNL Customer.io] {#set-up-webhook}

Webhook URL を作成すると、 [!DNL Customer.io] ユーザーインターフェイス。 レポート用 Web フックの設定手順については、 [[!DNL Customer.io] ガイド](https://customer.io/docs/webhooks/#setup) をクリックします。

Adobe Analytics の [!DNL Customer.io] ユーザーインターフェイス、 [webhook URL](#get-streaming-endpoint-url) （内） [!DNL WEBHOOK ENDPOINT] フィールドに入力します。

![Webhook エンドポイントフィールドを表示する Customer.io ユーザーインターフェイス](../../../../images/tutorials/create/marketing-automation/customerio-webhook/webhook.png)

>[!TIP]
>
>レポート用 Webhook には、様々なイベントを購読できます。 各イベントのメッセージは、 [!DNL Customer.io] アクションイベントのトリガー条件が満たされている。 各イベントについて詳しくは、 [[!DNL Customer.io] イベントドキュメント](https://customer.io/docs/webhooks/#events).

## 次の手順 {#next-steps}

このチュートリアルに従うことで、 [!DNL Customer.io] データをExperience Platformに送信します。 取り込まれるデータを監視するには、 [Platform UI を使用したストリーミングデータフローの監視](../../monitor-streaming.md).

## その他のリソース {#additional-resources}

以下の節では、 [!DNL Customer.io] ソース。

### ガードレール {#guardrails}

ガードレールについて詳しくは、 [[!DNL Customer.io] タイムアウトと失敗のページ](https://customer.io/docs/webhooks/#timeouts-and-failures).

### 検証 {#validation}

ソースとが正しく設定されていることを検証するには、以下を実行します。 [!DNL Customer.io] メッセージを取り込むには、次の手順に従います。

* 次の項目を確認できます。 [!DNL Customer.io] **[!UICONTROL アクティビティログ]** がキャプチャしたイベントを識別するページ [!DNL Customer.io].

![アクティビティログを示す Customer.io UI のスクリーンショット](../../../../images/tutorials/create/marketing-automation/customerio-webhook/activity-logs.png)

* Platform UI で、「 」を選択します。 **[!UICONTROL データフローを表示]** の横に [!DNL Customer.io] ソースカタログのカードメニュー 次に、「 **[!UICONTROL データセットをプレビュー]** で選択したイベントに対して取り込まれたデータを検証するには、以下を実行します。 [!DNL Customer.io].

![取り込んだイベントを示す Platform UI のスクリーンショット](../../../../images/tutorials/create/marketing-automation/customerio-webhook/platform-dataset.png)
