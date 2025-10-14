---
title: UI での Customer.io Source接続とデータフローの作成
description: Adobe Experience Platform UI を使用して Customer.io ソース接続を作成する方法を説明します。
badge: ベータ版
exl-id: 7655a34c-808a-46e3-94e3-022a433755a4
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1225'
ht-degree: 20%

---

# UI での [!DNL Customer.io] ソース接続とデータフローの作成

>[!NOTE]
>
>[!DNL Customer.io] ソースはベータ版です。ベータラベル付きソースの使用について詳しくは、[&#x200B; ソースの概要 &#x200B;](../../../../home.md#terms-and-conditions) を参照してください。

このチュートリアルでは、Adobe Experience Platform ユーザーインターフェイスを使用して [!DNL Customer.io] ソース接続とデータフローを作成する手順について説明します。

## はじめに {#getting-started}

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

## 前提条件 {#prerequisites}

次の節では、[!DNL Customer.io] ソース接続を作成する前に完了すべき前提条件について説明します。

### [!DNL Customer.io] のソーススキーマを定義するサンプル JSON {#prerequisites-json-schema}

[!DNL Customer.io] ソース接続を作成する前に、ソーススキーマを指定する必要があります。 以下の JSON を使用できます。

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

### [!DNL Customer.io] 用のExperience Platform スキーマの作成 {#create-platform-schema}

また、ソースに使用するExperience Platform スキーマを必ず作成する必要があります。 スキーマの作成方法に関する包括的な手順については、[Experience Platform スキーマの作成 &#x200B;](../../../../../xdm/schema/composition.md) に関するチュートリアルを参照してください。

![Customer.io のスキーマ例を示すExperience Platform UI のスクリーンショット &#x200B;](../../../../images/tutorials/create/marketing-automation/customerio-webhook/schema.png)

## [!DNL Customer.io] アカウントを接続 {#connect-account}

Experience Platformの UI で、左側のナビゲーションから **[!UICONTROL Sources]** を選択し、[!UICONTROL Sources] ワークスペースにアクセスして、Experience Platformで使用可能なソースのカタログを表示します。

*[!UICONTROL カテゴリ]* メニューを使用して、カテゴリ別にソースをフィルタリングします。 または、検索バーにソース名を入力して、カタログから特定のソースを検索します。

[!UICONTROL &#x200B; マーケティング自動化 &#x200B;] カテゴリに移動して、[!DNL Customer.io] ソースカードを表示します。 開始するには、「**[!UICONTROL データを追加]**」を選択します。

![Customer.io カードを含むカタログのExperience Platform UI のスクリーンショット &#x200B;](../../../../images/tutorials/create/marketing-automation/customerio-webhook/catalog.png)

## データの選択 {#select-data}

**[!UICONTROL データを選択]** 手順が表示され、Experience Platformに取り込むデータを選択するためのインターフェイスが表示されます。

* インターフェイスの左側は、アカウント内で利用可能なデータストリームを表示できるブラウザーです。
* インターフェイスの右側の部分では、JSON ファイルから最大 100 行のデータをプレビューできます。

**[!UICONTROL ファイルをアップロード]** を選択して、ローカルシステムから JSON ファイルをアップロードします。 または、アップロードする JSON ファイルを [!UICONTROL &#x200B; ファイルをドラッグ&amp;ドロップ &#x200B;] パネルにドラッグ&amp;ドロップすることもできます。

![&#x200B; ソースワークフローのデータを追加ステップ &#x200B;](../../../../images/tutorials/create/marketing-automation/customerio-webhook//add-data.png)

ファイルがアップロードされると、プレビューインターフェイスが更新され、アップロードしたスキーマのプレビューが表示されます。 プレビューインターフェイスを使用すると、ファイルの内容と構造を検査できます。 また、[!UICONTROL &#x200B; フィールドを検索 &#x200B;] ユーティリティを使用して、スキーマ内から特定の項目にアクセスすることもできます。

終了したら、「**[!UICONTROL 次へ]**」を選択します。

![&#x200B; ソースワークフローのプレビュー手順 &#x200B;](../../../../images/tutorials/create/marketing-automation/customerio-webhook//preview.png)

## データフローの詳細 {#dataflow-detail}

**データフローの詳細** 手順が表示され、既存のデータセットを使用するか、データフローの新しいデータセットを確立するかのオプションと、データフローの名前と説明を入力する機会が提供されます。 この手順では、プロファイルの取り込み、エラー診断、部分取り込み、アラートの設定も指定できます。

終了したら、「**[!UICONTROL 次へ]**」を選択します。

![&#x200B; ソースワークフローのデータフローの詳細手順。](../../../../images/tutorials/create/marketing-automation/customerio-webhook//dataflow-detail.png)

## マッピング {#mapping}

[!UICONTROL マッピング]手順が表示され、ソーススキーマのソースフィールドを、ターゲットスキーマの適切なターゲット XDM フィールドにマッピングするためのインターフェイスが提供されます。

Experience Platformは、選択したターゲットスキーマまたはデータセットに基づいて、自動マッピングされたフィールドに対してインテリジェントなレコメンデーションを提供します。 マッピングルールは、ユースケースに合わせて手動で調整できます。 必要に応じて、フィールドを直接マッピングするか、データ準備機能を使用してソースデータを変換して計算値を導き出すかを選択できます。マッパーインターフェイスと計算フィールドの使用に関する包括的な手順については、[&#x200B; データ準備 UI ガイド &#x200B;](../../../../../data-prep/ui/mapping.md) を参照してください。

以下にリストされているすべてのマッピングは必須であり、「レビュー [!UICONTROL &#x200B; 段階に進む前に設定する必要があ &#x200B;] ます。

| ターゲットフィールド | 説明 |
| --- | --- |
| `object_type` | オブジェクトタイプ。サポートされるタイプについては、[!DNL Customer.io] [events](https://customer.io/docs/webhooks/#events) のドキュメントを参照してください。 |
| `id` | オブジェクトの識別子。 |
| `email` | オブジェクトに関連付けられた電子メールアドレス。 |
| `event_id` | イベントの一意の ID。 |
| `cio_id` | イベントの [!DNL Customer.io] 識別子。 |
| `metric` | イベントタイプ。 詳しくは、[!DNL Customer.io] [events](https://customer.io/docs/webhooks/#events) ドキュメントでサポートされるタイプを参照してください。 |
| `timestamp` | イベントが発生したときのタイムスタンプ。 |

>[!IMPORTANT]
>
>`test mode` で Webhook を実行する際は、`cio_id` から送信され [!DNL Customer.io] 関連するフィールドがないので、[!DNL Customer.io] をマッピングしないでください。

ソースデータが正常にマッピングされたら、「**[!UICONTROL 次へ]**」を選択します。

![&#x200B; ソースワークフローのマッピングステップ &#x200B;](../../../../images/tutorials/create/marketing-automation/customerio-webhook/mapping.png)

## レビュー {#review}

**[!UICONTROL レビュー]**&#x200B;手順が表示され、新しいデータフローを作成する前に確認できます。詳細は、次のカテゴリに分類されます。

* **[!UICONTROL 接続]**：ソースのタイプ、選択したソースファイルの関連パスおよびそのソースファイル内の列の数を表示します。
* **[!UICONTROL データセットの割り当てとフィールドのマッピング]**：ソースデータがどのデータセットに取り込まれるかを、そのデータセットが準拠するスキーマを含めて表示します。

データフローをレビューしたら、「**[!UICONTROL 終了]**」を選択し、データフローが作成されるまでしばらく待ちます。

![&#x200B; ソースワークフローのレビュー手順。](../../../../images/tutorials/create/marketing-automation/customerio-webhook/review.png)

## ストリーミングエンドポイント URL の取得 {#get-streaming-endpoint}

ストリーミングデータフローを作成したので、ストリーミングエンドポイント URL を取得できるようになりました。 このエンドポイントは、Webhook をサブスクライブするために使用され、ストリーミングソースがExperience Platformと通信できるようになります。

で Webhook の設定に使用する URL を作成するには [!DNL Customer.io] 次を取得する必要があります。

* **[!UICONTROL データフロー ID]**
* **[!UICONTROL ストリーミングエンドポイント]**

**[!UICONTROL データフロー ID]** および **[!UICONTROL ストリーミングエンドポイント]** を取得するには、作成したばかりのデータフローの [!UICONTROL &#x200B; データフローアクティビティ &#x200B;] ページに移動し、[!UICONTROL &#x200B; プロパティ &#x200B;] パネルの下部から詳細をコピーします。

![&#x200B; データフローアクティビティのストリーミングエンドポイント。](../../../../images/tutorials/create/marketing-automation/customerio-webhook/endpoint-test.png)

ストリーミングエンドポイントとデータフロー ID を取得したら、パターン ```{STREAMING_ENDPOINT}?x-adobe-flow-id={DATAFLOW_ID}``` に基づいて URL を作成します。 例えば、作成された Webhook URL は次のようになります。``https://dcs.adobedc.net/collection/febc116d22ba0ea2868e9c93b199375302afb8a589617700991bb8f3f0341ad7?x-adobe-flow-id=439b3fc4-3042-4a3a-b5e0-a494898d3fb0``

## [!DNL Customer.io] でのレポート Webhook の設定 {#set-up-webhook}

Webhook URL を作成したので、[!DNL Customer.io] ユーザーインターフェイスを使用してレポート Webhook を設定できるようになりました。 レポート Webhook の設定手順については、Webhook の設定に関する [[!DNL Customer.io]  ガイド &#x200B;](https://customer.io/docs/webhooks/#setup) を参照してください。

[!DNL Customer.io] ユーザーインターフェイスの [!DNL WEBHOOK ENDPOINT] フィールドに [webhook URL](#get-streaming-endpoint-url) を入力します。

![Webhook エンドポイントフィールドを表示する Customer.io ユーザーインターフェイス &#x200B;](../../../../images/tutorials/create/marketing-automation/customerio-webhook/webhook.png)

>[!TIP]
>
>レポート Webhook 用に様々なイベントを購読できます。 各イベントのメッセージは、[!DNL Customer.io] のアクションイベントのトリガー条件が満たされるとExperience Platformに取り込まれます。 様々なイベントについて詳しくは、[[!DNL Customer.io]  イベントドキュメント &#x200B;](https://customer.io/docs/webhooks/#events) を参照してください。

## 次の手順 {#next-steps}

このチュートリアルでは、[!DNL Customer.io] データをExperience Platformに取り込むためのストリーミングデータフローを正常に設定しました。 取り込まれるデータを監視するには、[Experience Platform UI を使用したストリーミングデータフローのモニタリング &#x200B;](../../monitor-streaming.md) のガイドを参照してください。

## その他のリソース {#additional-resources}

以下の節では、[!DNL Customer.io] ソースを使用する際に参照できるその他のリソースを示します。

### ガードレール {#guardrails}

ガードレールについて詳しくは、[[!DNL Customer.io]  タイムアウトとエラーのページ &#x200B;](https://customer.io/docs/webhooks/#timeouts-and-failures) を参照してください。

### 検証 {#validation}

ソースを正しく設定したことと、メッセージが取り込まれてい [!DNL Customer.io] ことを検証するには、次の手順に従います。

* [!DNL Customer.io] **[!UICONTROL アクティビティログ]** ページをチェックすると、[!DNL Customer.io] によって取り込まれるイベントを特定できます。

![&#x200B; アクティビティログを示す Customer.io UI のスクリーンショット &#x200B;](../../../../images/tutorials/create/marketing-automation/customerio-webhook/activity-logs.png)

* Experience Platform UI で、ソースカタログの [!DNL Customer.io] ーザーカードメニューの横にある **[!UICONTROL データフローを表示]** を選択します。 次に、「**[!UICONTROL データセットをプレビュー]**」をクリックして、[!DNL Customer.io] で選択したイベントに対して取り込まれたデータを検証します。

![&#x200B; 取り込んだイベントを示すExperience Platform UI のスクリーンショット &#x200B;](../../../../images/tutorials/create/marketing-automation/customerio-webhook/platform-dataset.png)
