---
keywords: Experience Platform；ホーム；人気のトピック；アラート；宛先
description: データフローを作成する際にアラートの配信を登録して、フロー実行のステータス、成功または失敗に関するアラートメッセージを受信できます。
title: コンテキスト内宛先アラートを購読
exl-id: 134144a0-cdfe-49a8-bd8b-e36a4f053de5
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '953'
ht-degree: 16%

---

# コンテキスト内宛先アラートを購読

Adobe Experience Platform では、Adobe Experience Platform アクティビティに関するイベントベースのアラートを登録できます。 アラートにより、ジョブが完了したか、ワークフロー内の特定のマイルストーンに達したか、何らかのエラーが発生したかを確認するために、[[!DNL Observability Insights] API](../../observability/api/overview.md) をポーリングする必要が低減される、またはなくなります。

データフローを作成する際にアラートの配信を登録して、フロー実行のステータス、成功または失敗に関するアラートメッセージを受信できます。

このドキュメントでは、宛先データフローのアラートメッセージの受信を登録する手順を説明します。

## はじめに

このドキュメントでは、Adobe Experience Platformの次のコンポーネントに関する十分な知識が必要です。

* [ 宛先 ](../home.md):Adobe Experience Platformからのデータの円滑なアクティベーションを可能にする、宛先プラットフォームとの事前定義済みの統合。 宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。
* [ 可観測性 ](../../observability/home.md):[!DNL Observability Insights] を使用すると、統計指標とイベント通知を使用して、Experience Platform アクティビティを監視できます。
   * [ アラート ](../../observability/alerts/overview.md):Experience Platform操作の特定の条件（システムがしきい値に達した場合に問題が発生する可能性があるなど）に達すると、Experience Platformはその条件を登録している組織内のユーザーにアラートメッセージを配信できます。

## UI でのアラートの登録 {#subscribe-destination-alerts}

>[!CONTEXTUALHELP]
>id="platform_destination_alerts_subscribe"
>title="宛先アラートの登録"
>abstract="アラートを使用すると、宛先データフローのステータスに基づいて、通知を受け取ることができます。アラート通知を設定すると、データフローが開始された場合、成功した場合、失敗した場合、宛先にデータが送信されなかった場合に、更新情報をを受け取ることができます。"
>text="Learn more in documentation"

>[!IMPORTANT]
>
>データフローでメールベースのアラート通知を受け取るには、Experience Platform アカウントでメールの即時通知を有効にする必要があります。

[ 宛先接続 ](connect-destination.md) ワークフローの [!UICONTROL  新しい宛先を設定 ] 手順で、データフローのアラートを有効にできます。

![ 宛先アラートセクションを示す UI 画像。](../assets/ui/alerts/destination-alerts.png)

購読するアラートを選択し、「**[!UICONTROL 次へ]**」を選択してデータフローを確認して完了します。

以下の表に、宛先データフローで使用できるアラートを示します。

* ストリーミング宛先の場合は、[!DNL Activation Skipped Rate Exceeded] アラートのみ使用できます。
* ファイルベースの宛先の場合、すべてのアラートを使用できます。

| アラート | 説明 |
| --- | --- |
| 宛先フロー実行遅延 | このアラートは、宛先フローの実行でオーディエンスのアクティブ化に 150 分以上かかった場合に通知します。 |
| 宛先フロー実行の失敗 | このアラートは、宛先に対してオーディエンスをアクティブ化する際にエラーが発生した場合に通知します。 |
| 宛先フロー実行の成功 | このアラートは、オーディエンスが宛先に対して正常にアクティブ化された場合に通知します。 |
| 宛先フロー実行開始 | このアラートは、宛先フローの実行がオーディエンスのアクティブ化を開始すると通知します。 |
| アクティベーションスキップ率を超過 | このアラートは、アクティベーションのスキップ率がアクティベーション全体の 1% を超えた場合に通知します。 属性が見つからない場合や同意違反がある場合、ID はアクティベーション中にスキップされます。 |

## アラートの受信 {#receiving-alerts}

宛先データフローが実行されると、UI またはメールでアラートを受け取ることができます。

### UI でのアラートの受信 {#receiving-alerts-in-ui}

アラートは、Experience Platform UI の上部ヘッダーにある通知アイコンによって UI に表示されます。 通知アイコンを選択して、データフローに関する特定のアラートメッセージを表示します。

![Experience Platformの通知アイコンを示す UI 画像 ](../assets/ui/alerts/notification.png)

通知パネルが表示され、作成したデータフローのステータス更新のリストが表示されます。

![ 通知パネルを示す UI 画像 ](../assets/ui/alerts/alert-window.png)

アラートメッセージにポインタを合わせて、それらを既読としてマークしたり、時計アイコンを選択して、データフローのステータスに関する今後のリマインダーを設定したりできます。

![ 通知リマインダーオプションを示す UI 画像 ](../assets/ui/alerts/remind-me.png)

アラートメッセージを選択して、データフローに関する特定の情報を表示します。

![ 通知の選択方法を示す UI 画像 ](../assets/ui/alerts/select-alert-message.png)

[!UICONTROL  データフロー実行の詳細 ] ページが表示されます。 画面の上半分には、属性に関する情報、対応するデータフロー実行 ID、高レベルのエラー概要など、データフローの概要が表示されます。

![ データフロー実行の詳細ページを示す UI 画像。](../assets/ui/alerts/dataflow-overview.png)

ページの下半分には、データフロー実行ステージで発生した [!UICONTROL  データフロー実行エラー ] が表示されます。 ここから、エラー診断をプレビューしたり、[[!DNL Data Access] API](https://www.adobe.io/experience-platform-apis/references/data-access/) を使用して、データフローに対応するエラー診断またはファイルマニフェストをダウンロードしたりできます。

![ データフロー実行の詳細ページを示す UI 画像（「エラー」セクションをハイライト表示） ](../assets/ui/alerts/dataflow-run-error.png)

データフローエラーの処理について詳しくは、[UI での宛先データフローの監視 ](../../dataflows/ui/monitor-destinations.md) を参照してください。

### メールによるアラートの受信 {#receiving-alerts-by-email}

データフローに関するアラートもメールで配信されます。 メール本文でデータフロー名を選択すると、データフローの詳細が表示されます。

![ アラートメールのスクリーンショット ](../assets/ui/alerts/email.png)

UI アラートと同様に、[!UICONTROL  データフロー実行の概要 ] ページが表示され、データフローに関連付けられたエラーを調査するためのインターフェイスが提供されます。

![dataflow-overview](../assets/ui/alerts/dataflow-overview.png)

## アラートを購読および購読解除 {#subscribe-and-unsubscribe}

宛先 [!UICONTROL  参照 ] ページで、追加のアラートを購読したり、既存の宛先データフローに対して設定されたアラートを登録解除したりできます。

![ 宛先の参照ページを示す UI 画像 ](../assets/ui/alerts/destination-list.png)

アラートを受信する宛先接続を見つけ、省略記号（`...`）を選択してオプションのドロップダウンメニューを表示します。 次に、「**[!UICONTROL アラートを購読]**」を選択して、宛先データフローのアラート設定を変更します。

![ 宛先オプションを示す UI 画像 ](../assets/ui/alerts/destination-alerts-subscribe.png)

ポップアップウィンドウが開き、宛先アラートのリストが表示されます。 登録するアラートを選択するか、登録解除するアラートの選択を解除します。 完了したら「**[!UICONTROL 保存]**」を選択します。

![ 宛先アラートの購読ページを示す UI 画像 ](../assets/ui/alerts/destination-alerts-list.png)

## 次の手順 {#next-steps}

このドキュメントでは、宛先データフローのコンテキスト内アラートを購読する方法を順を追って説明しました。 詳しくは、[ アラート UI ガイド ](../../observability/alerts/ui.md) を参照してください。
