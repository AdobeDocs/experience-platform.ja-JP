---
title: UI での Chatlio Source接続の作成
description: Adobe Experience Platform UI を使用して Chatlio ソース接続を作成する方法を説明します。
badge: ベータ版
exl-id: 55c10bcb-0332-45ff-970b-272d375b591d
source-git-commit: 8de45a54607bed17fd79bbed693666beb09c0502
workflow-type: tm+mt
source-wordcount: '1156'
ht-degree: 21%

---

# UI での [!DNL Chatlio] ソース接続の作成

>[!NOTE]
>
>[!DNL Chatlio] ソースはベータ版です。ベータラベル付きソースの使用について詳しくは、[ ソースの概要 ](../../../../home.md#terms-and-conditions) を参照してください。

このチュートリアルでは、Adobe Experience Platform ユーザーインターフェイスを使用して [!DNL Chatlio] ソース接続を作成する手順について説明します。

## はじめに {#getting-started}

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

## 前提条件 {#prerequisites}

次の節では、[!DNL Chatlio] ソース接続を作成する前に完了すべき前提条件について説明します。

### [!DNL Chatlio] のソーススキーマを定義するサンプル JSON {#prerequisites-json-schema}

[!DNL Chatlio] ソース接続を作成する前に、ソーススキーマを指定する必要があります。 以下の JSON を使用できます。

```
{
  "visitor": {
    "email": "test@example.com",
    "UUID": "2d3f4260-2235-903b-0a82-a23d326cc257"
  },
   "message": "Hi",
  "channelId": "C04J7M7LCMQ",
  "slackChannelName": "aep",
  "slackChannelId": "C04JVR71WKS"
}
```

### [!DNL Chatlio] 用の Platform スキーマの作成 {#create-platform-schema}

また、ソースに使用する Platform スキーマを作成する必要があります。 スキーマの作成方法に関する包括的な手順については、[Platform スキーマの作成 ](../../../../../xdm/schema/composition.md) に関するチュートリアルを参照してください。

![Chatlio のスキーマの例を示す Platform UI](../../../../images/tutorials/create/marketing-automation/chatlio-webhook/schema.png)

## [!DNL Chatlio] アカウントを接続 {#connect-account}

Platform UI の左側のナビゲーションから「**[!UICONTROL ソース]**」を選択し、「[!UICONTROL  ソース ]」ワークスペースにアクセスし、Experience Platformで使用可能なソースのカタログを確認します。

*[!UICONTROL カテゴリ]* メニューを使用して、カテゴリ別にソースをフィルタリングします。 または、検索バーにソース名を入力して、カタログから特定のソースを検索します。

[!UICONTROL  マーケティング自動化 ] カテゴリに移動して、[!DNL Chatlio] ソースカードを表示します。 開始するには、「**[!UICONTROL データを追加]**」を選択します。

![Chatlio カードを使用した Platform UI カタログ ](../../../../images/tutorials/create/marketing-automation/chatlio-webhook/catalog.png)

## データの選択 {#select-data}

**[!UICONTROL データを選択]** 手順が表示され、Platform に取り込むデータを選択するためのインターフェイスが表示されます。

* インターフェイスの左側は、アカウント内で利用可能なデータストリームを表示できるブラウザーです。
* インターフェイスの右側の部分では、JSON ファイルから最大 100 行のデータをプレビューできます。

**[!UICONTROL ファイルをアップロード]** を選択して、ローカルシステムから JSON ファイルをアップロードします。 または、アップロードする JSON ファイルを [!UICONTROL  ファイルをドラッグ&amp;ドロップ ] パネルにドラッグ&amp;ドロップすることもできます。

![ ソースワークフローのデータを追加ステップ ](../../../../images/tutorials/create/marketing-automation/chatlio-webhook/add-data.png)

ファイルがアップロードされると、プレビューインターフェイスが更新され、アップロードしたスキーマのプレビューが表示されます。 プレビューインターフェイスを使用すると、ファイルの内容と構造を検査できます。 また、[!UICONTROL  フィールドを検索 ] ユーティリティを使用して、スキーマ内から特定の項目にアクセスすることもできます。

終了したら、「**[!UICONTROL 次へ]**」を選択します。

![ ソースワークフローのプレビュー手順 ](../../../../images/tutorials/create/marketing-automation/chatlio-webhook/preview.png)

## データフローの詳細 {#dataflow-detail}

**データフローの詳細** 手順が表示され、既存のデータセットを使用するか、データフローの新しいデータセットを確立するかのオプションと、データフローの名前と説明を入力する機会が提供されます。 この手順では、プロファイルの取り込み、エラー診断、部分取り込み、アラートの設定も指定できます。

終了したら、「**[!UICONTROL 次へ]**」を選択します。

![ ソースワークフローのデータフローの詳細手順。](../../../../images/tutorials/create/marketing-automation/chatlio-webhook/dataflow-detail.png)

## マッピング {#mapping}

[!UICONTROL マッピング]手順が表示され、ソーススキーマのソースフィールドを、ターゲットスキーマの適切なターゲット XDM フィールドにマッピングするためのインターフェイスが提供されます。

Platform は、選択したターゲットスキーマまたはデータセットに基づいて、自動マッピングされたフィールドに対してインテリジェントなレコメンデーションを提供します。 マッピングルールは、ユースケースに合わせて手動で調整できます。 必要に応じて、フィールドを直接マッピングするか、データ準備機能を使用してソースデータを変換して計算値を導き出すかを選択できます。マッパーインターフェイスと計算フィールドの使用に関する包括的な手順については、[ データ準備 UI ガイド ](../../../../../data-prep/ui/mapping.md) を参照してください。

以下にリストされているマッピングは必須であり、[!UICONTROL  レビュー ] 段階に進む前に設定する必要があります。

| ターゲットフィールド | 説明 |
| --- | --- |
| `UUID` | イベントの [!DNL Chatlio] 識別子。 |

ソースデータが正常にマッピングされたら、「**[!UICONTROL 次へ]**」を選択します。

![ ソースワークフローのマッピングステップ ](../../../../images/tutorials/create/marketing-automation/chatlio-webhook/mapping.png)

## レビュー {#review}

**[!UICONTROL レビュー]**&#x200B;手順が表示され、新しいデータフローを作成する前に確認できます。詳細は、次のカテゴリに分類されます。

* **[!UICONTROL 接続]**：ソースのタイプ、選択したソースファイルの関連パスおよびそのソースファイル内の列の数を表示します。
* **[!UICONTROL データセットの割り当てとフィールドのマッピング]**：ソースデータがどのデータセットに取り込まれるかを、そのデータセットが準拠するスキーマを含めて表示します。

データフローをレビューしたら、「**[!UICONTROL 終了]**」を選択し、データフローが作成されるまでしばらく待ちます。

![ ソースワークフローのレビュー手順。](../../../../images/tutorials/create/marketing-automation/chatlio-webhook/review.png)

## ストリーミングエンドポイント URL の取得 {#get-streaming-endpoint-url}

ストリーミングデータフローを作成したので、ストリーミングエンドポイント URL を取得できるようになりました。 このエンドポイントは、Webhook をサブスクライブするために使用され、ストリーミングソースがExperience Platformと通信できるようになります。

で Webhook の設定に使用する URL を作成するには [!DNL Chatlio] 次を取得する必要があります。

* **[!UICONTROL データフロー ID]**
* **[!UICONTROL ストリーミングエンドポイント]**

**[!UICONTROL データフロー ID]** および **[!UICONTROL ストリーミングエンドポイント]** を取得するには、作成したばかりのデータフローの [!UICONTROL  データフローアクティビティ ] ページに移動し、[!UICONTROL  プロパティ ] パネルの下部から詳細をコピーします。

![ データフローアクティビティのストリーミングエンドポイント。](../../../../images/tutorials/create/marketing-automation/chatlio-webhook/endpoint-test.png)

ストリーミングエンドポイントとデータフロー ID を取得したら、パターン ```{STREAMING_ENDPOINT}?x-adobe-flow-id={DATAFLOW_ID}``` に基づいて URL を作成します。 例えば、作成された Webhook URL は次のようになります。``https://dcs.adobedc.net/collection/d56b47ee3985104beaf724efcd78a3e1a863d720471a482bebac0acc1ab95983``

## [!DNL Chatlio] での Webhook の設定 {#set-up-webhook}

Webhook URL を作成したら、[!DNL Chatlio] ユーザーインターフェイスを使用して Webhook を設定できます。

[[!DNL Chatlio]](https://chatlio.com/) アカウントにログインし、[ セットアップとインストールに関するガイド ](https://chatlio.com/docs/setup/) に従ってウィジェットを作成します。

ウィジェットが作成されたら、ウィジェットの設定ページに移動して、そのウィジェットに Webhook URL を追加します。

![Chatlio の Webhook 設定ページ ](../../../../images/tutorials/create/marketing-automation/chatlio-webhook/widget-settings.png)

次に、「**[!DNL Behavior]**」タブを選択し、*[!DNL Webhook when a new conversation starts]* フィールドおよび購読する他の Webhook イベントフィールドに Webhook URL を追加します。

![Webhook エンドポイントフィールドを表示する Chatlio UI。](../../../../images/tutorials/create/marketing-automation/chatlio-webhook/webhook.png)

>[!TIP]
>
>[!DNL Chatlio] Webhook では、様々なイベントを購読できます。 様々なイベントについて詳しくは、[[!DNL Chatlio]  イベントドキュメント ](https://chatlio.com/docs/webhooks/) を参照してください。

## 次の手順 {#next-steps}

このチュートリアルでは、[!DNL Chatlio] データをExperience Platformに取り込むためのストリーミングデータフローを正常に設定しました。 取り込まれるデータを監視するには、[Platform UI を使用したストリーミングデータフローの監視 ](../../monitor-streaming.md) のガイドを参照してください。

## その他のリソース {#additional-resources}

以下の節では、[!DNL Chatlio] ソースを使用する際に参照できるその他のリソースを示します。

### 検証 {#validation}

ソースを正しく設定したことと、メッセージが取り込まれてい [!DNL Chatlio] ことを検証するには、次の手順に従います。

* [!DNL Chatlio] **[!UICONTROL Reports]** > **[!UICONTROL Chat History]** ページをチェックすると、[!DNL Chatlio] によってキャプチャされているイベントを識別できます。

![ チャット履歴を示す Chatlio UI のスクリーンショット ](../../../../images/tutorials/create/marketing-automation/chatlio-webhook/chatlio-chat-history.png)

* Platform UI で、ソースカタログの [!DNL Chatlio] ーザーカードメニューの横にある **[!UICONTROL データフローを表示]** を選択します。 次に、「**[!UICONTROL データセットをプレビュー]**」をクリックして、[!DNL Chatlio] 内で設定した Webhook 用に取り込まれたデータを検証します。

![ 取り込んだイベントを示す Platform UI のスクリーンショット ](../../../../images/tutorials/create/marketing-automation/chatlio-webhook/platform-dataset.png)

[!DNL Chatlio] について詳しくは、[[!DNL Chatlio]  ドキュメント ](https://chatlio.com/docs/) and [FAQ](https://chatlio.com/pricing/#FAQ) を参照してください。
