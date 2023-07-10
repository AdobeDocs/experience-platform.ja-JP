---
keywords: Experience Platform;ホーム;人気のトピック;スキーマ;スキーマ;XDM;フィールド;スキーマ;スキーマ;identityMap;ID マップ;ID マップ;スキーマデザイン;マップ;マップ;イベントモデリング;ベストプラクティス;イベント;イベント;イベント;
solution: Experience Platform
title: XDM ExperienceEvent クラス
description: このドキュメントでは、XDM ExperienceEvent クラスの概要と、イベントデータモデリングのベストプラクティスについて説明します。
exl-id: a8e59413-b52f-4ea5-867b-8d81088a3321
source-git-commit: d648a2151060d1013a6bce7a8180378400337829
workflow-type: tm+mt
source-wordcount: '1880'
ht-degree: 92%

---

# [!DNL XDM ExperienceEvent] クラス

[!DNL XDM ExperienceEvent] は標準のエクスペリエンスデータモデル（XDM）クラスで、特定のイベントが発生したとき、または特定の条件セットに到達したときに、システムのタイムスタンプ付きスナップショットを作成することができます。

エクスペリエンスイベントは、発生した事実（特定の時点や個人の ID など）の記録したものです。イベントは、明示的（直接観察可能な人間のアクション）または暗黙的（人間の直接のアクションなしに発生したもの）に設定でき、集計や解釈なしで記録されます。Platform エコシステムでのこのクラスの使用に関するハイレベルな情報については、[XDM の概要](../home.md#data-behaviors)を参照してください。

[!DNL XDM ExperienceEvent] クラス自体は、スキーマに対していくつかの時系列関連フィールドを提供します。 次の 2 つのフィールド (`_id` および `timestamp`) は **必須** はクラスに基づくすべてのスキーマに対して、残りはオプションです。 一部のフィールドの値は、データの取り込み時に自動的に設定されます。

![Platform UI に表示される XDM ExperienceEvent の構造](../images/classes/experienceevent/structure.png)

| プロパティ | 説明 |
| --- | --- |
| `_id`<br>**(必須)** | エクスペリエンスイベントクラス `_id` フィールドは、Adobe Experience Platformに取り込まれる個々のイベントを一意に識別します。 このフィールドは、個々のイベントの一意性を追跡、データの重複を防止し、ダウンストリームのサービスでそのイベントを検索するために使用されます。 <br><br>重複イベントが検出された場合、Platform のアプリケーションとサービスでは、重複の処理方法が異なる場合があります。  例えば、同じ `_id` は既にプロファイルストアに存在します。<br><br>場合によっては、`_id` は、[ユニバーサル固有識別子（UUID）](https://tools.ietf.org/html/rfc4122) または [グローバル固有識別子（GUID）](https://docs.microsoft.com/ja-jp/dotnet/api/system.guid?view=net-5.0)とすることができます。<br><br>単一ソースの接続からデータをストリーミングする場合、または Parquet ファイルから直接取り込む場合は、プライマリ ID、タイムスタンプ、イベントタイプなど、イベントを一意にするフィールドの特定の組み合わせを連結して、この値を生成する必要があります。 連結された値は、`uri-reference` 形式の文字列にする（コロン文字は削除する）必要があります。 その後、連結された値は、SHA-256 または選択した別のアルゴリズムを使用してハッシュ化する必要があります。<br><br>**このフィールドは、個人に関連する ID を表すものではなく**、データ記録そのものを表していることを見極めることが重要です。人物に関する ID データは、代わりに互換性のあるフィールドグループが提供する [ID フィールド](../schema/composition.md#identity)に降格させるべきです。 |
| `eventMergeId` | [Adobe Experience Platform Web SDK](../../edge/home.md) を使用してデータを取り込む場合、レコードを作成する原因となった取り込まれたバッチの ID を表します。 このフィールドは、データの取り込み時にシステムによって自動的に入力されます。 Web SDK 実装のコンテキスト以外に、このフィールドの使用はサポートされていません。 |
| `eventType` | イベントのタイプまたはカテゴリを示す文字列。 このフィールドは、同じスキーマとデータセット内の異なるイベントタイプを区別する場合（リテール企業で製品を表示するイベントと買い物かごへの追加イベントを区別する場合など）に使用できます。<br><br>このプロパティの標準値は、[付録の節](#eventType)に記載されています（意図するユースケースの説明も含む）。このフィールドは拡張可能な列挙型で、つまり、独自のイベントタイプ文字列を使用して、追跡するイベントを分類することもできます。<br><br>`eventType` では、アプリケーションでのヒットごとに 1 つのイベントのみを使用するように制限されているので、最も重要なイベントをシステムに伝えるには、計算フィールドを使用する必要があります。 詳しくは、[計算フィールドのベストプラクティス](#calculated)の節を参照してください。 |
| `producedBy` | イベントのプロデューサーまたはオリジンを表す文字列の値。セグメント化で必要な場合、このフィールドを使用すると特定のイベントプロデューサーを除外できます。<br><br>このプロパティの推奨値の一部が、[付録の節](#producedBy)に記載されています。このフィールドは拡張可能な列挙型で、つまり、独自の文字列を使用して、異なるイベントプロデューサーを表すこともできます。 |
| `identityMap` | イベントが適用される個人の名前空間 ID のセットを含む map フィールド。 このフィールドは、ID データが取り込まれると、システムによって自動的に更新されます。 このフィールドを適切に利用するために [リアルタイム顧客プロファイル](../../profile/home.md)の場合は、データ操作でフィールドのコンテンツを手動で更新しようとしないでください。<br /><br />そのユースケースについては、[スキーマ構成の基本](../schema/composition.md#identityMap) の ID マップの節を参照してください。 |
| `timestamp`<br>**(必須)** | イベントが発生した時点の ISO 8601 タイムスタンプ（[RFC 3339 セクション 5.6](https://tools.ietf.org/html/rfc3339#section-5.6) を準拠した書式設定）。このタイムスタンプは過去の日付にする必要があります。このフィールドの使用に関するベストプラクティスについては、以下の[タイムスタンプ](#timestamps)の節を参照してください。 |

{style="table-layout:auto"}

## イベントモデリングのベストプラクティス

以下の節で、Adobe Experience Platform でイベントベースの Experience Data Model（XDM）スキーマを設計する際のベストプラクティスについて説明します。

### タイムスタンプ {#timestamps}

イベントスキーマのルート `timestamp` フィールドは、イベント自体の観測&#x200B;**のみ**&#x200B;を表すことができ、過去の日付にする必要があります。 セグメント化のユースケースで、使用するタイムスタンプが将来の日付になる可能性がある場合、これらの値はエクスペリエンスイベントスキーマの他の場所で制約を受ける必要があります。

例えば、旅行業界や接客業のビジネスがフライト予約イベントをモデリングしている場合、クラスレベルの `timestamp`フィールドは、予約イベントが観測された時刻を表します。 旅行予約の開始日など、イベントに関連するその他のタイムスタンプは、標準フィールドグループまたはカスタムフィールドグループが提供する別のフィールドで取得する必要があります。

![フライトの予約と開始日がハイライトされたエクスペリエンスイベントスキーマのサンプルです。](../images/classes/experienceevent/timestamps.png)

クラスレベルのタイムスタンプをイベントスキーマの他の関連する日時値から分離することで、エクスペリエンスアプリケーションでカスタマージャーニーをタイムスタンプで記録しながら、柔軟なセグメント化のユースケースを実装することができます。

### 計算フィールドの使用 {#calculated}

エクスペリエンスアプリケーションにおける特定のインタラクションの結果、技術的に同じイベントタイムスタンプを共有する複数の関連イベントが発生する可能性があるので、それらのインタラクションを単一のイベントレコードとして表現できます。 例えば、顧客が web サイトで製品を閲覧すると、結果的に、可能性のある 2 つの `eventType` 値を持つイベントレコードになることがあります。「製品ビュー」イベント（`commerce.productViews`）または汎用的な「ページビュー」イベント（`web.webpagedetails.pageViews`）の 2 つです。 このような場合、1 回のヒットで複数のイベントがキャプチャされる際に、計算フィールドを使用して最も重要な属性をキャプチャすることができます。

[Adobe Experience Platform のデータ準備機能](../../data-prep/home.md)により、XDM との間でデータのマッピング、変換および検証を行うことができます。 サービスから提供される[マッピング機能](../../data-prep/functions.md)を使用すると、複数のイベントレコードのデータを Experience Platform に取り込む際に、論理演算子を呼び出してデータの優先順位付け、変換および統合を行うことができます。上記の例では、「製品ビュー」と「ページビュー」の両方が発生した場合に「ページビュー」よりも「製品ビュー」を優先させる計算フィールドとして、`eventType` を指定することができます。

UI を使用して手動で Platform にデータを取り込む場合、計算フィールドの作成方法に関する具体的な手順については、[計算フィールド](../../data-prep/ui/mapping.md#calculated-fields)に関するガイドを参照してください。

ソース接続を使用してデータを Platform にストリーミングする場合は、代わりに計算フィールドを使用するようにソースを設定できます。 接続を設定する際に計算フィールドを実装する手順については、 [特定のソースのドキュメント](../../sources/home.md) を参照してください。

## 互換性のあるスキーマフィールドグループ {#field-groups}

>[!NOTE]
>
>複数のフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../field-groups/name-updates.md)のドキュメントを参照してください。

アドビでは、 [!DNL XDM ExperienceEvent] クラスで使用するためのいくつかの標準フィールドグループを提供しています。 このクラスで一般的に使用されるフィールドグループは次のとおりです。

* [[!UICONTROL Adobe Analytics ExperienceEvent Full 拡張機能]](../field-groups/event/analytics-full-extension.md)
* [[!UICONTROL 残高繰り越し]](../field-groups/event/balance-transfers.md)
* [[!UICONTROL キャンペーンマーケティング詳細]](../field-groups/event/campaign-marketing-details.md)
* [[!UICONTROL カードのアクション]](../field-groups/event/card-actions.md)
* [[!UICONTROL チャンネル詳細]](../field-groups/event/channel-details.md)
* [[!UICONTROL コマース詳細]](../field-groups/event/commerce-details.md)
* [[!UICONTROL 入金明細]](../field-groups/event/deposit-details.md)
* [[!UICONTROL デバイス下取り詳細]](../field-groups/event/device-trade-in-details.md)
* [[!UICONTROL 食事予約]](../field-groups/event/dining-reservation.md)
* [[!UICONTROL エンドユーザー ID 詳細]](../field-groups/event/enduserids.md)
* [[!UICONTROL 環境詳細]](../field-groups/event/environment-details.md)
* [[!UICONTROL フライト予約]](../field-groups/event/flight-reservation.md)
* [[!UICONTROL IAB TCF 2.0 同意]](../field-groups/event/iab.md)
* [[!UICONTROL 宿泊予約]](../field-groups/event/lodging-reservation.md)
* [[!UICONTROL 見積依頼の詳細]](../field-groups/event/quote-request-details.md)
* [[!UICONTROL 予約詳細]](../field-groups/event/reservation-details.md)
* [[!UICONTROL Web 詳細]](../field-groups/event/web-details.md)

## 付録

次の節では、[!UICONTROL XDM ExperienceEvent] クラスに関する追加情報を提供します。

### `eventType` の許容値 {#eventType}

次の表に、 `eventType` の許容値とその定義の概要を示します。

| 値 | 定義 |
| --- | --- |
| `advertising.clicks` | 広告でのクリックアクション。 |
| `advertising.completes` | 時間指定メディアアセットが最後まで視聴されました。視聴者が先までスキップした可能性があるので、必ずしも視聴者が動画全体を視聴したとは限りません。 |
| `advertising.conversions` | 顧客によって実行された事前定義済みアクション（パフォーマンス評価のイベントをトリガーします）。 |
| `advertising.federated` | エクスペリエンスイベントが、データフェデレーション（顧客間のデータ共有）を通じて作成されたかどうかを示します。 |
| `advertising.firstQuartiles` | デジタルビデオ広告が通常の速度で再生時間の 25％まで再生されました。 |
| `advertising.impressions` | 閲覧される可能性のある広告が顧客に与えるインプレッション。 |
| `advertising.midpoints` | デジタルビデオ広告が通常の速度で再生時間の 50％まで再生されました。 |
| `advertising.starts` | デジタルビデオ広告の再生が開始されました。 |
| `advertising.thirdQuartiles` | デジタルビデオ広告が通常の速度で再生時間の 75％まで再生されました。 |
| `advertising.timePlayed` | 特定の時間指定メディアアセットにユーザーが費やした時間を記述します。 |
| `application.close` | アプリケーションが閉じられたか、バックグラウンドに送られました。 |
| `application.launch` | アプリケーションが起動したか、フォアグラウンドに移動しました。 |
| `commerce.checkouts` | 商品リストのチェックアウトイベントが発生しました。チェックアウトプロセスに複数のステップがある場合、複数のチェックアウトイベントが存在する可能性があります。 複数のステップがある場合、各イベントのタイムスタンプと参照されているページ／エクスペリエンスを使用して、各イベント（ステップ）を識別し、順に表現します。 |
| `commerce.productListAdds` | ある商品が商品リストまたは買い物かごに追加されました。 |
| `commerce.productListOpens` | 新しい商品リスト（買い物かご）が初期化または作成されました。 |
| `commerce.productListRemovals` | 1 つ以上の商品エントリが商品リストまたは買い物かごから削除されました。 |
| `commerce.productListReopens` | アクセスできなくなった（放棄された）商品リスト（買い物かご）が、リマーケティングアクティビティなどを介して顧客によって再びアクティブ化されました。 |
| `commerce.productListViews` | 商品リストまたは買い物かごに対して 1 回以上のビューがありました。 |
| `commerce.productViews` | 1 つの商品に対して 1 回以上のビューがありました。 |
| `commerce.purchases` | 注文が受理されました。これはコマースコンバージョンで唯一必要なアクションです。購入イベントでは商品リストが参照されている必要があります。 |
| `commerce.saveForLaters` | 商品リストが今後の使用のために保存されました（例：商品ウィッシュリスト）。 |
| `decisioning.propositionDisplay` | ある人物に決定の提案が表示されました。 |
| `decisioning.propositionInteract` | ある人物が決定の提案を操作しました。 |
| `delivery.feedback` | メール配信など、配信のフィードバックイベント。 |
| `directMarketing.emailBounced` | バウンスした、ある人物へのメール。 |
| `directMarketing.emailBouncedSoft` | ソフトバウンスした、ある人物へのメール。 |
| `directMarketing.emailClicked` | ある人物がマーケティングメールのリンクをクリックしました。 |
| `directMarketing.emailDelivered` | 送信先のメールサービスにメールが正常に配信されました |
| `directMarketing.emailOpened` | マーケティングメールを開いた人。 |
| `directMarketing.emailUnsubscribed` | ある人物がマーケティングメールの購読を解除しました。 |
| `inappmessageTracking.dismiss` | アプリ内メッセージが取り消されました。 |
| `inappmessageTracking.display` | アプリ内メッセージが表示されました。 |
| `inappmessageTracking.interact` | アプリ内メッセージが応答されました。 |
| `leadOperation.callWebhook` | リードに応答して webhook が呼び出されました。 |
| `leadOperation.convertLead` | リードがコンバージョンに至りました。 |
| `leadOperation.interestingMoment` | ある人物にとって興味深い瞬間が記録されました。 |
| `leadOperation.newLead` | リードが作成されました。 |
| `leadOperation.scoreChanged` | リードのスコア属性の値が変更されました。 |
| `leadOperation.statusInCampaignProgressionChanged` | キャンペーンにおけるリードのステータスが変更されました。 |
| `listOperation.addToList` | ある人物がマーケティングリストに追加されました。 |
| `listOperation.removeFromList` | ある人物がマーケティングリストから削除されました。 |
| `message.feedback` | 顧客に送信したメッセージに対する送信／バウンス／エラーなどのフィードバックイベント。 |
| `message.tracking` | 顧客に送信されたメッセージに対する開く／クリック／カスタムアクションなどのトラッキングイベント。 |
| `opportunityEvent.addToOpportunity` | ある人物がオポチュニティに追加されました。 |
| `opportunityEvent.opportunityUpdated` | オポチュニティが更新されました。 |
| `opportunityEvent.removeFromOpportunity` | ある人物がオポチュニティから削除されました。 |
| `pushTracking.applicationOpened` | ある人物がプッシュ通知からアプリを開きました。 |
| `pushTracking.customAction` | ある人物がプッシュ通知でカスタムアクションをクリックしました。 |
| `web.formFilledOut` | ある人物が wep ページのフォームに入力しました。 |
| `web.webinteraction.linkClicks` | あるリンクが 1 回以上選択されました。 |
| `web.webpagedetails.pageViews` | ある web ページに対して 1 回以上のビューが発生しました。 |

{style="table-layout:auto"}

### `producedBy` の推奨値 {#producedBy}

`producedBy` の許容値を次の表に示します。

| 値 | 定義 |
| --- | --- |
| `self` | 自分 |
| `system` | システム |
| `salesRef` | 営業担当者 |
| `customerRep` | 顧客担当者 |
