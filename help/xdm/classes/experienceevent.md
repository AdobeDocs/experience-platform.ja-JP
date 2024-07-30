---
keywords: Experience Platform;ホーム;人気のトピック;スキーマ;スキーマ;XDM;フィールド;スキーマ;スキーマ;identityMap;ID マップ;ID マップ;スキーマデザイン;マップ;マップ;イベントモデリング;ベストプラクティス;イベント;イベント;イベント;
solution: Experience Platform
title: XDM ExperienceEvent クラス
description: XDM ExperienceEvent クラスと、イベントデータモデリングのベストプラクティスについて説明します。
exl-id: a8e59413-b52f-4ea5-867b-8d81088a3321
source-git-commit: 5537485206c1625ca661d6b33f7bba08538a0fa3
workflow-type: tm+mt
source-wordcount: '2761'
ht-degree: 38%

---

# [!DNL XDM ExperienceEvent] クラス

[!DNL XDM ExperienceEvent] は、標準のエクスペリエンスデータモデル（XDM）クラスです。 このクラスを使用すると、特定のイベントが発生したとき、または特定の条件セットに到達したときに、システムのタイムスタンプ付きスナップショットを作成できます。

エクスペリエンスイベントは、発生した事実（特定の時点や個人の ID など）の記録したものです。イベントは、明示的（直接観察可能な人間のアクション）または暗黙的（人間の直接のアクションなしに発生したもの）に設定でき、集計や解釈なしで記録されます。Platform エコシステムでのこのクラスの使用に関するハイレベルな情報については、[XDM の概要](../home.md#data-behaviors)を参照してください。

[!DNL XDM ExperienceEvent] クラス自体は、スキーマに対していくつかの時系列関連フィールドを提供します。 これらのフィールドのうち 2 つ（`_id` と `timestamp`）は、このクラスに基づくすべてのスキーマに対して **必須** ですが、残りはオプションです。 一部のフィールドの値は、データの取り込み時に自動入力されます。

![Platform UI に表示される XDM ExperienceEvent の構造 ](../images/classes/experienceevent/structure.png)

| プロパティ | 説明 |
| --- | --- |
| `_id`<br>**（必須）** | 「エクスペリエンスイベントクラス `_id`」フィールドは、Adobe Experience Platformに取り込まれる個々のイベントを一意に識別します。 このフィールドは、個々のイベントの一意性を追跡、データの重複を防ぎ、ダウンストリームのサービスでそのイベントを検索するために使用されます。<br><br> 重複イベントが検出された場合、Platform アプリケーションおよびサービスの重複処理の方法が異なる可能性があります。 例えば、同じ `_id` を持つイベントが既にプロファイルストアに存在する場合、プロファイルサービスの重複イベントはドロップされます。<br><br> 場合によ `_id` ては、[ ユニバーサル固有識別子（UUID） ](https://datatracker.ietf.org/doc/html/rfc4122) または [ グローバル固有識別子（GUID） ](https://learn.microsoft.com/en-us/dotnet/api/system.guid?view=net-5.0) とすることができます。<br><br> ソース接続からデータをストリーミングする場合、または Parquet ファイルから直接取り込む場合は、イベントを一意にするフィールドの特定の組み合わせを連結して、この値を生成する必要があります。 連結できるイベントの例としては、プライマリ ID、タイムスタンプ、イベントタイプなどがあります。 連結された値は、`uri-reference` 形式の文字列にする（コロン文字は削除する）必要があります。 その後、連結された値は、SHA-256 または選択した別のアルゴリズムを使用してハッシュ化する必要があります。<br><br>**このフィールドは、個人に関連する ID を表すものではなく**、データ記録そのものを表していることを見極めることが重要です。人物に関する ID データは、代わりに互換性のあるフィールドグループが提供する [ID フィールド](../schema/composition.md#identity)に降格させるべきです。 |
| `eventMergeId` | [Adobe Experience Platform Web SDK](/help/web-sdk/home.md) を使用してデータを取り込む場合、レコードを作成する原因となった取り込まれたバッチの ID を表します。 このフィールドは、データの取り込み時にシステムによって自動的に入力されます。 Web SDK 実装のコンテキスト以外に、このフィールドの使用はサポートされていません。 |
| `eventType` | イベントのタイプまたはカテゴリを示す文字列。 このフィールドは、同じスキーマとデータセット内の異なるイベントタイプを区別する場合（リテール企業で製品を表示するイベントと買い物かごへの追加イベントを区別する場合など）に使用できます。<br><br>このプロパティの標準値は、[付録の節](#eventType)に記載されています（意図するユースケースの説明も含む）。このフィールドは拡張可能な列挙型で、つまり、独自のイベントタイプ文字列を使用して、追跡するイベントを分類することもできます。<br><br>`eventType` では、アプリケーションでのヒットごとに 1 つのイベントのみを使用するように制限されているので、最も重要なイベントをシステムに伝えるには、計算フィールドを使用する必要があります。 詳しくは、[計算フィールドのベストプラクティス](#calculated)の節を参照してください。 |
| `producedBy` | イベントのプロデューサーまたはオリジンを表す文字列の値。セグメント化で必要な場合、このフィールドを使用すると特定のイベントプロデューサーを除外できます。<br><br>このプロパティの推奨値の一部が、[付録の節](#producedBy)に記載されています。このフィールドは拡張可能な列挙型で、つまり、独自の文字列を使用して、異なるイベントプロデューサーを表すこともできます。 |
| `identityMap` | イベントが適用される個人の名前空間 ID のセットを含む map フィールド。 このフィールドは、ID データが取り込まれると、システムによって自動的に更新されます。 このフィールドを [ リアルタイム顧客プロファイル ](../../profile/home.md) に適切に利用するために、データ操作でフィールドの内容を手動で更新しようとしないでください。<br /><br />そのユースケースについては、[スキーマ構成の基本](../schema/composition.md#identityMap) の ID マップの節を参照してください。 |
| `timestamp`<br>**（必須）** | イベントが発生した時点の ISO 8601 タイムスタンプ（[RFC 3339 セクション 5.6](https://datatracker.ietf.org/doc/html/rfc3339) を準拠した書式設定）。このタイムスタンプは過去の日付 **必須** ですが、1970 年以降の日付 **必須** となります。 このフィールドの使用に関するベストプラクティスについては、以下の[タイムスタンプ](#timestamps)の節を参照してください。 |

{style="table-layout:auto"}

## イベントモデリングのベストプラクティス

以下の節で、Adobe Experience Platform でイベントベースの Experience Data Model（XDM）スキーマを設計する際のベストプラクティスについて説明します。

### タイムスタンプ {#timestamps}

イベントスキーマのルート `timestamp` フィールドは、イベント自体の観測&#x200B;**のみ**&#x200B;を表すことができ、過去の日付にする必要があります。 ただし、このイベント **必須** は 1970 年以降に発生します。 セグメント化のユースケースで、使用するタイムスタンプが将来の日付になる可能性がある場合、これらの値はエクスペリエンスイベントスキーマの他の場所で制約を受ける必要があります。

例えば、旅行業界や接客業のビジネスがフライト予約イベントをモデリングしている場合、クラスレベルの `timestamp`フィールドは、予約イベントが観測された時刻を表します。 旅行予約の開始日など、イベントに関連するその他のタイムスタンプは、標準フィールドグループまたはカスタムフィールドグループが提供する別のフィールドで取得する必要があります。

![ フライトの予約と開始日がハイライト表示されたサンプルエクスペリエンスイベントスキーマ ](../images/classes/experienceevent/timestamps.png)

クラスレベルのタイムスタンプをイベントスキーマの他の関連する日時値から分離することで、エクスペリエンスアプリケーションでカスタマージャーニーをタイムスタンプで記録しながら、柔軟なセグメント化のユースケースを実装することができます。

### 計算フィールドの使用 {#calculated}

エクスペリエンスアプリケーションにおける特定のインタラクションの結果、技術的に同じイベントタイムスタンプを共有する複数の関連イベントが発生する可能性があるので、それらのインタラクションを単一のイベントレコードとして表現できます。 例えば、顧客が web サイトで製品を閲覧すると、結果的に、可能性のある 2 つの `eventType` 値を持つイベントレコードになることがあります。「製品ビュー」イベント（`commerce.productViews`）または汎用的な「ページビュー」イベント（`web.webpagedetails.pageViews`）の 2 つです。 このような場合、1 回のヒットで複数のイベントがキャプチャされる際に、計算フィールドを使用して最も重要な属性をキャプチャすることができます。

[Adobe Experience Platform Data Prep](../../data-prep/home.md) を使用して、XDM との間でデータのマッピング、変換および検証を行います。 サービスから提供される[マッピング機能](../../data-prep/functions.md)を使用すると、複数のイベントレコードのデータを Experience Platform に取り込む際に、論理演算子を呼び出してデータの優先順位付け、変換および統合を行うことができます。上記の例では、「製品ビュー」と「ページビュー」の両方が発生した場合に「ページビュー」よりも「製品ビュー」を優先させる計算フィールドとして、`eventType` を指定することができます。

UI を使用して手動で Platform にデータを取り込む場合、計算フィールドの作成方法に関する具体的な手順については、[計算フィールド](../../data-prep/ui/mapping.md#calculated-fields)に関するガイドを参照してください。

ソース接続を使用してデータを Platform にストリーミングする場合は、代わりに計算フィールドを使用するようにソースを設定できます。 接続を設定する際に計算フィールドを実装する手順については、 [特定のソースのドキュメント](../../sources/home.md) を参照してください。

## 互換性のあるスキーマフィールドグループ {#field-groups}

>[!NOTE]
>
>複数のフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../field-groups/name-updates.md)のドキュメントを参照してください。

アドビでは、 [!DNL XDM ExperienceEvent] クラスで使用するためのいくつかの標準フィールドグループを提供しています。 このクラスで一般的に使用されるフィールドグループは次のとおりです。

* [[!UICONTROL Adobe Analytics ExperienceEvent 完全拡張機能 ]](../field-groups/event/analytics-full-extension.md)
* [[!UICONTROL  残高移動 ]](../field-groups/event/balance-transfers.md)
* [[!UICONTROL キャンペーンマーケティング詳細]](../field-groups/event/campaign-marketing-details.md)
* [[!UICONTROL  カードのアクション ]](../field-groups/event/card-actions.md)
* [[!UICONTROL チャンネル詳細]](../field-groups/event/channel-details.md)
* [[!UICONTROL コマース詳細]](../field-groups/event/commerce-details.md)
* [[!UICONTROL  供託内容等 ]](../field-groups/event/deposit-details.md)
* [[!UICONTROL デバイス下取り詳細]](../field-groups/event/device-trade-in-details.md)
* [[!UICONTROL 食事予約]](../field-groups/event/dining-reservation.md)
* [[!UICONTROL エンドユーザー ID 詳細]](../field-groups/event/enduserids.md)
* [[!UICONTROL 環境詳細]](../field-groups/event/environment-details.md)
* [[!UICONTROL フライト予約]](../field-groups/event/flight-reservation.md)
* [[!UICONTROL IAB TCF 2.0 同意]](../field-groups/event/iab.md)
* [[!UICONTROL 宿泊予約]](../field-groups/event/lodging-reservation.md)
* [[!UICONTROL MediaAnalytics インタラクションの詳細 ]](../field-groups/event/mediaanalytics-interaction.md)
* [[!UICONTROL  見積依頼の詳細 ]](../field-groups/event/quote-request-details.md)
* [[!UICONTROL 予約詳細]](../field-groups/event/reservation-details.md)
* [[!UICONTROL Web 詳細]](../field-groups/event/web-details.md)

## 付録

次の節では、[!UICONTROL XDM ExperienceEvent] クラスに関する追加情報を提供します。

### `eventType` の許容値 {#eventType}

次の表に、 `eventType` の許容値とその定義の概要を示します。

| 値 | 定義 |
| --- | --- |
| `advertising.clicks` | このイベントは、広告を選択するアクションが発生したタイミングを追跡します。 |
| `advertising.completes` | このイベントは、タイムドメディアアセットが最後まで視聴されたタイミングを追跡します。 視聴者が先にスキップした可能性があるので、必ずしも視聴者がビデオ全体を視聴したとは限りません。 |
| `advertising.conversions` | このイベントは、パフォーマンス評価のイベントをトリガーにするお客様が実行した事前定義済みのアクションをトラッキングします。 |
| `advertising.federated` | このイベントでは、エクスペリエンスイベントがデータフェデレーション（顧客間のデータ共有）を通じて作成されたかどうかを追跡します。 |
| `advertising.firstQuartiles` | このイベントは、デジタルビデオ広告が通常の速度でデュレーションの 25% を再生したタイミングを追跡します。 |
| `advertising.impressions` | このイベントは、閲覧される可能性のある広告が顧客に与えるインプレッションを追跡します。 |
| `advertising.midpoints` | このイベントは、デジタルビデオ広告が通常の速度でデュレーションの 50% を再生したタイミングを追跡します。 |
| `advertising.starts` | このイベントは、デジタルビデオ広告の再生が開始されたタイミングを追跡します。 |
| `advertising.thirdQuartiles` | このイベントは、デジタルビデオ広告が通常の速度でデュレーションの 75% を再生したタイミングを追跡します。 |
| `advertising.timePlayed` | このイベントは、特定のタイムドメディアアセットでユーザーが費やした時間を追跡します。 |
| `application.close` | このイベントは、アプリケーションがいつクローズされたか、またはバックグラウンドに送信されたかを追跡します。 |
| `application.launch` | このイベントは、アプリケーションがいつ起動されたか、またはフォアグラウンドに移動したかを追跡します。 |
| `click` | **非推奨** 代わりに、`decisioning.propositionInteract` を使用してください。 |
| `commerce.backofficeCreditMemoIssued` | このイベントは、顧客に対してクレジット通知が発行された際にトラッキングします。 |
| `commerce.backofficeOrderCancelled` | このイベントは、以前に開始した購入プロセスが完了前に終了したタイミングを追跡します。 |
| `commerce.backofficeOrderItemsShipped` | このイベントは、購入した品目が物理的に顧客に出荷された時期を追跡します。 |
| `commerce.backofficeOrderPlaced` | このイベントは、注文の発注をトラッキングします。 |
| `commerce.backofficeShipmentCompleted` | このイベントは、出荷プロセス全体の正常な完了を追跡します。 |
| `commerce.checkouts` | このイベントは、商品リストのチェックアウトイベントが発生したタイミングを追跡します。 チェックアウトプロセスに複数のステップがある場合、複数のチェックアウトイベントが存在する可能性があります。 複数のステップがある場合、各イベントのタイムスタンプと参照されているページ／エクスペリエンスを使用して、各イベント（ステップ）を識別し、順に表現します。 |
| `commerce.productListAdds` | このイベントは、商品が商品リストまたは買い物かごに追加されたタイミングを追跡します。 |
| `commerce.productListOpens` | このイベントは、新しい商品リスト（買い物かご）が初期化または作成された際に追跡します。 |
| `commerce.productListRemovals` | このイベントは、商品エントリが商品リストまたは買い物かごから削除された際にトラッキングします。 |
| `commerce.productListReopens` | このイベントは、アクセスできなくなった（放棄された）製品リスト（買い物かご）が、リマーケティングアクティビティなどを介して顧客によって再アクティブ化された際に追跡します。 |
| `commerce.productListViews` | このイベントは、商品リストまたは買い物かごが表示を受け取ったタイミングを追跡します。 |
| `commerce.productViews` | このイベントは、製品が 1 つ以上のビューを受信した際に追跡します。 |
| `commerce.purchases` | このイベントは、注文が受理されたタイミングを追跡します。 これはコマースコンバージョンで唯一必要なアクションです。購入イベントでは商品リストが参照されている必要があります。 |
| `commerce.saveForLaters` | このイベントは、商品リストが今後の使用のために保存されたタイミング（例：商品ウィッシュリスト）を追跡します。 |
| `decisioning.propositionDisplay` | このイベントは、Web SDK がページに表示されている内容に関する情報を自動的に送信する場合に使用されます。 ただし、ページヒットの上位と下位など、他の方法で表示情報を既に含めている場合は、このイベントタイプは必要ありません。 ページヒットの一番下では、任意のイベントタイプを選択できます。 |
| `decisioning.propositionDismiss` | このイベントタイプは、Adobe Journey Optimizerのアプリ内メッセージまたはコンテンツカードが閉じられるときに使用されます。 |
| `decisioning.propositionFetch` | イベントが主に決定を取得することを示すために使用されます。 Adobe Analyticsはこのイベントを自動的にドロップします。 |
| `decisioning.propositionInteract` | このイベントタイプは、パーソナライズされたコンテンツに対するインタラクション（クリックなど）を追跡するために使用されます。 |
| `decisioning.propositionSend` | このイベントは、見込み客に提案またはオファーを送信することを決定した際にトラッキングします。 |
| `decisioning.propositionTrigger` | このタイプのイベントは、[Web SDK](../../web-sdk/home.md) によってローカルストレージに保存されますが、Experience Edgeには送信されません。 ルールセットが満たされるたびに、イベントが生成されてローカルストレージに保存されます（その設定が有効な場合）。 |
| `delivery.feedback` | このイベントは、メール配信など、配信のフィードバックイベントを追跡します。 |
| `directMarketing.emailBounced` | このイベントは、人物へのメールがバウンスしたタイミングを追跡します。 |
| `directMarketing.emailBouncedSoft` | このイベントは、人物へのメールがソフトバウンスしたタイミングを追跡します。 |
| `directMarketing.emailClicked` | このイベントは、マーケティングメール内のリンクをユーザーがクリックした際にトラッキングします。 |
| `directMarketing.emailDelivered` | このイベントでは、メールが人物のメールサービスに正常に配信されたタイミングを追跡します。 |
| `directMarketing.emailOpened` | このイベントは、人物がマーケティングメールを開封したタイミングを追跡します。 |
| `directMarketing.emailSent` | このイベントは、マーケティングメールが人物に送信されたタイミングを追跡します。 |
| `directMarketing.emailUnsubscribed` | このイベントは、ユーザーがマーケティングメールの購読を解除したタイミングを追跡します。 |
| `display` | **非推奨** 代わりに、`decisioning.propositionDisplay` を使用してください。 |
| `inappmessageTracking.dismiss` | このイベントは、アプリ内メッセージが取り消されたタイミングを追跡します。 |
| `inappmessageTracking.display` | このイベントは、アプリ内メッセージが表示されたタイミングを追跡します。 |
| `inappmessageTracking.interact` | このイベントは、アプリ内メッセージがいつ操作されたかを追跡します。 |
| `leadOperation.callWebhook` | このイベントは、リードに応答して Webhook が呼び出されたタイミングを追跡します。 |
| `leadOperation.changeCampaignStream` | このイベントは、特定のビジネスリードのマーケティング戦略またはエンゲージメント戦略がシフトしたことを示します。 |
| `leadOperation.changeEngagementCampaignCadence` | このイベントは、キャンペーンの一環としてリードがとエンゲージする頻度に変更があったタイミングを追跡します。 |
| `leadOperation.convertLead` | このイベントは、リードがいつコンバージョンされたかを追跡します。 |
| `leadOperation.interestingMoment` | このイベントは、ある人物にとって興味深い瞬間が録画された際にトラッキングします。 |
| `leadOperation.mergeLeads` | このイベントは、同じエンティティを参照する複数のリードの情報が統合されたタイミングを追跡します。 |
| `leadOperation.newLead` | このイベントは、リードが作成されたタイミングを追跡します。 |
| `leadOperation.scoreChanged` | このイベントは、リードのスコア属性の値が変更されたタイミングを追跡します。 |
| `leadOperation.statusInCampaignProgressionChanged` | このイベントは、キャンペーンでのリードのステータスが変更されたタイミングを追跡します。 |
| `listOperation.addToList` | このイベントでは、人物がマーケティングリストに追加されたタイミングを追跡します。 |
| `listOperation.removeFromList` | このイベントは、ある人物がマーケティングリストから削除されたタイミングを追跡します。 |
| `media.adBreakComplete` | このイベントは、広告ブレークが完了したことを示します。 |
| `media.adBreakStart` | このイベントは、広告ブレークの開始を示します。 |
| `media.adComplete` | このイベントは、広告の完了を示します。 |
| `media.adSkip` | このイベントは、広告がスキップされた場合に通知します。 |
| `media.adStart` | このイベントは、広告の開始を示します。 |
| `media.bitrateChange` | このイベントは、ビットレートが変更された場合に通知します。 |
| `media.bufferStart` | `media.bufferStart` イベントタイプは、バッファー処理の開始時に送信されます。 特定の `bufferResume` イベントタイプはありません。`bufferStart` イベントの後に `play` イベントが送信された場合、バッファリングは再開されたと見なされます。 |
| `media.chapterComplete` | このイベントは、チャプターが完了したことを示します。 |
| `media.chapterSkip` | このイベントは、ユーザーが別のセクションまたはチャプターに進む、または戻る際にトリガーされます。 |
| `media.chapterStart` | このイベントは、チャプターの開始を示します。 |
| `media.downloaded` | このイベントは、メディアダウンロード済みコンテンツが発生したタイミングを追跡します。 |
| `media.error` | このイベントは、メディアの再生中にエラーが発生した場合に通知します。 |
| `media.pauseStart` | このイベントは、`pauseStart` イベントが発生した際に追跡します。 このイベントは、ユーザーがメディア再生で一時停止を開始するとトリガーされます。 再開イベントタイプはありません。 リク `pauseStart` ストの後に再生イベントを送信すると、再開が推論されます。 |
| `media.ping` | `media.ping` イベントタイプは、進行中の再生ステータスを示すために使用されます。 メインコンテンツの場合、このイベントは再生中に 10 秒ごとに送信される必要があります。これは、再生が開始されてから 10 秒後に開始されます。 広告コンテンツの場合は、広告トラッキング中に 1 秒ごとに送信される必要があります。 ping イベントでは、リクエスト本文にパラメーターマップを含めないでください。 |
| `media.play` | `media.play` イベントタイプは、プレーヤーが `buffering,` `paused` （ユーザーによって再開された場合）や `error` （自動再生などのシナリオを含む）などの別の状態から `playing` 状態に移行する際に送信されます。 このイベントは、プレーヤーの `on('Playing')` コールバックによってトリガーされます。 |
| `media.sessionComplete` | このイベントは、メインコンテンツの終わりに達したときに送信されます。 |
| `media.sessionEnd` | `media.sessionEnd` イベントタイプは、ユーザーが表示を放棄して戻る可能性が低いときに、セッションを直ちに閉じるように Media Analytics バックエンドに通知します。 このイベントが送信されない場合、セッションは、10 分間無操作状態が続いた後、または再生ヘッドを動かさずに 30 分後にタイムアウトします。 そのセッション ID を持つ後続のメディアコールは無視されます。 |
| `media.sessionStart` | `media.sessionStart` イベントタイプは、セッション開始呼び出しで送信されます。 応答を受け取ると、セッション ID が Location ヘッダーから抽出され、収集サーバーに対する以降のすべてのイベント呼び出しに使用されます。 |
| `media.statesUpdate` | このイベントは、`statesUpdate` イベントが発生した際に追跡します。 プレーヤーステートトラッキング機能は、オーディオストリームまたはビデオストリームに付加することができる。 標準の状態は、`fullscreen`、`mute`、`closedCaptioning`、`pictureInPicture`、`inFocus` です。 |
| `opportunityEvent.addToOpportunity` | このイベントでは、人物がオポチュニティに追加されたタイミングを追跡します。 |
| `opportunityEvent.opportunityUpdated` | このイベントは、オポチュニティが更新されたタイミングを追跡します。 |
| `opportunityEvent.removeFromOpportunity` | このイベントでは、人物がオポチュニティから削除されたタイミングを追跡します。 |
| `personalization.request` | **非推奨** 代わりに、`decisioning.propositionFetch` を使用してください。 |
| `pushTracking.applicationOpened` | このイベントは、ユーザーがプッシュ通知からアプリを開いたタイミングを追跡します。 |
| `pushTracking.customAction` | このイベントは、ユーザーがプッシュ通知でカスタムアクションを選択したタイミングを追跡します。 |
| `web.formFilledOut` | このイベントは、人物が web ページ上のフォームに入力したタイミングを追跡します。 |
| `web.webinteraction.linkClicks` | イベントは、リンククリックが Web SDK によって自動的に記録されたことを示します。 |
| `web.webpagedetails.pageViews` | このイベントタイプは、ヒットをページビューとしてマークするための標準的な方法です。 |
| `location.entry` | このイベントは、特定の場所でのユーザーまたはデバイスのエントリを追跡します。 |
| `location.exit` | このイベントは、特定の場所からの人物またはデバイスの出口を追跡します。 |

{style="table-layout:auto"}

### `producedBy` の推奨値 {#producedBy}

`producedBy` の許容値を次の表に示します。

| 値 | 定義 |
| --- | --- |
| `self` | 自分 |
| `system` | システム |
| `salesRef` | 営業担当者 |
| `customerRep` | 顧客担当者 |
