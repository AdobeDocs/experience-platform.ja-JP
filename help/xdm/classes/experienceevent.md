---
keywords: Experience Platform;ホーム;人気のトピック;スキーマ;スキーマ;XDM;フィールド;スキーマ;スキーマ;identityMap;ID マップ;ID マップ;スキーマデザイン;マップ;マップ;イベントモデリング;ベストプラクティス;イベント;イベント;イベント;
solution: Experience Platform
title: XDM ExperienceEvent クラス
description: このドキュメントでは、XDM ExperienceEvent クラスの概要と、イベントデータモデリングのベストプラクティスについて説明します。
exl-id: a8e59413-b52f-4ea5-867b-8d81088a3321
source-git-commit: 093f4881f2224d0a0c888c7be688000d31114944
workflow-type: tm+mt
source-wordcount: '2667'
ht-degree: 45%

---

# [!DNL XDM ExperienceEvent] クラス

[!DNL XDM ExperienceEvent] は標準のエクスペリエンスデータモデル（XDM）クラスで、特定のイベントが発生したとき、または特定の条件セットに到達したときに、システムのタイムスタンプ付きスナップショットを作成することができます。

エクスペリエンスイベントは、発生した事実（特定の時点や個人の ID など）の記録したものです。イベントは、明示的（直接観察可能な人間のアクション）または暗黙的（人間の直接のアクションなしに発生したもの）に設定でき、集計や解釈なしで記録されます。Platform エコシステムでのこのクラスの使用に関するハイレベルな情報については、[XDM の概要](../home.md#data-behaviors)を参照してください。

[!DNL XDM ExperienceEvent] クラス自体は、スキーマに対していくつかの時系列関連フィールドを提供します。 次の 2 つのフィールド (`_id` および `timestamp`) は **必須** はクラスに基づくすべてのスキーマに対して、残りはオプションです。 一部のフィールドの値は、データの取り込み時に自動的に設定されます。

![Platform UI に表示される XDM ExperienceEvent の構造](../images/classes/experienceevent/structure.png)

| プロパティ | 説明 |
| --- | --- |
| `_id`<br>**(必須)** | エクスペリエンスイベントクラス `_id` フィールドは、Adobe Experience Platformに取り込まれる個々のイベントを一意に識別します。 このフィールドは、個々のイベントの一意性を追跡、データの重複を防止し、ダウンストリームのサービスでそのイベントを検索するために使用されます。 <br><br>重複イベントが検出された場合、Platform のアプリケーションとサービスでは、重複の処理方法が異なる場合があります。 例えば、同じ `_id` は既にプロファイルストアに存在します。<br><br>場合によっては、`_id` は、[ユニバーサル固有識別子（UUID）](https://datatracker.ietf.org/doc/html/rfc4122) または [グローバル固有識別子（GUID）](https://learn.microsoft.com/en-us/dotnet/api/system.guid?view=net-5.0)とすることができます。<br><br>ソース接続からデータをストリーミングする場合、または Parquet ファイルから直接取り込む場合は、イベントを一意にするフィールドの特定の組み合わせを連結して、この値を生成する必要があります。 連結できるイベントの例としては、プライマリ ID、タイムスタンプ、イベントタイプなどがあります。 連結された値は、`uri-reference` 形式の文字列にする（コロン文字は削除する）必要があります。 その後、連結された値は、SHA-256 または選択した別のアルゴリズムを使用してハッシュ化する必要があります。<br><br>**このフィールドは、個人に関連する ID を表すものではなく**、データ記録そのものを表していることを見極めることが重要です。人物に関する ID データは、代わりに互換性のあるフィールドグループが提供する [ID フィールド](../schema/composition.md#identity)に降格させるべきです。 |
| `eventMergeId` | [Adobe Experience Platform Web SDK](../../edge/home.md) を使用してデータを取り込む場合、レコードを作成する原因となった取り込まれたバッチの ID を表します。 このフィールドは、データの取り込み時にシステムによって自動的に入力されます。 Web SDK 実装のコンテキスト以外に、このフィールドの使用はサポートされていません。 |
| `eventType` | イベントのタイプまたはカテゴリを示す文字列。 このフィールドは、同じスキーマとデータセット内の異なるイベントタイプを区別する場合（リテール企業で製品を表示するイベントと買い物かごへの追加イベントを区別する場合など）に使用できます。<br><br>このプロパティの標準値は、[付録の節](#eventType)に記載されています（意図するユースケースの説明も含む）。このフィールドは拡張可能な列挙型で、つまり、独自のイベントタイプ文字列を使用して、追跡するイベントを分類することもできます。<br><br>`eventType` では、アプリケーションでのヒットごとに 1 つのイベントのみを使用するように制限されているので、最も重要なイベントをシステムに伝えるには、計算フィールドを使用する必要があります。 詳しくは、[計算フィールドのベストプラクティス](#calculated)の節を参照してください。 |
| `producedBy` | イベントのプロデューサーまたはオリジンを表す文字列の値。セグメント化で必要な場合、このフィールドを使用すると特定のイベントプロデューサーを除外できます。<br><br>このプロパティの推奨値の一部が、[付録の節](#producedBy)に記載されています。このフィールドは拡張可能な列挙型で、つまり、独自の文字列を使用して、異なるイベントプロデューサーを表すこともできます。 |
| `identityMap` | イベントが適用される個人の名前空間 ID のセットを含む map フィールド。 このフィールドは、ID データが取り込まれると、システムによって自動的に更新されます。 このフィールドを適切に利用するには、次の手順に従います。 [リアルタイム顧客プロファイル](../../profile/home.md)の場合は、データ操作でフィールドのコンテンツを手動で更新しようとしないでください。<br /><br />そのユースケースについては、[スキーマ構成の基本](../schema/composition.md#identityMap) の ID マップの節を参照してください。 |
| `timestamp`<br>**(必須)** | イベントが発生した時点の ISO 8601 タイムスタンプ（[RFC 3339 セクション 5.6](https://datatracker.ietf.org/doc/html/rfc3339) を準拠した書式設定）。このタイムスタンプは過去の日付にする必要があります。このフィールドの使用に関するベストプラクティスについては、以下の[タイムスタンプ](#timestamps)の節を参照してください。 |

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
| `advertising.clicks` | このイベントは、広告を選択するアクションが発生した際に追跡します。 |
| `advertising.completes` | このイベントは、タイムドメディアアセットが最後まで視聴された際に追跡します。 視聴者が先にスキップした可能性があるので、必ずしも視聴者がビデオ全体を視聴したとは限りません。 |
| `advertising.conversions` | このイベントは、パフォーマンス評価用にイベントをトリガーする、顧客が実行した事前定義済みのアクションを追跡します。 |
| `advertising.federated` | このイベントは、エクスペリエンスイベントがデータフェデレーション（顧客間のデータ共有）を通じて作成されたかどうかを追跡します。 |
| `advertising.firstQuartiles` | このイベントは、デジタルビデオ広告が通常の速度で再生時間の 25%を再生した際に追跡します。 |
| `advertising.impressions` | このイベントは、顧客に対する広告のインプレッションを、表示される可能性を持って追跡します。 |
| `advertising.midpoints` | このイベントは、デジタルビデオ広告が通常の速度で再生時間の 50%を再生した際に追跡します。 |
| `advertising.starts` | このイベントは、デジタルビデオ広告の再生がいつ開始されたかを追跡します。 |
| `advertising.thirdQuartiles` | このイベントは、デジタルビデオ広告が通常の速度で再生時間の 75%を再生した際に追跡します。 |
| `advertising.timePlayed` | このイベントは、特定のタイムドメディアアセットでユーザーが費やした時間を追跡します。 |
| `application.close` | このイベントは、アプリが閉じられた、またはバックグラウンドに送信された際に追跡します。 |
| `application.launch` | このイベントは、アプリケーションが起動された、またはフォアグラウンドに移行された際に追跡します。 |
| `commerce.backofficeCreditMemoIssued` | このイベントは、顧客にクレジットの通知が発行された際にトラッキングします。 |
| `commerce.backofficeOrderCancelled` | このイベントは、以前に開始された購入プロセスが完了前に終了した際に追跡します。 |
| `commerce.backofficeOrderItemsShipped` | このイベントは、購入した品目が顧客に物理的に出荷された日時を追跡します。 |
| `commerce.backofficeOrderPlaced` | このイベントは、注文の配置を追跡します。 |
| `commerce.backofficeShipmentCompleted` | このイベントは、出荷プロセス全体の正常な完了を追跡します。 |
| `commerce.checkouts` | このイベントは、製品リストのチェックアウトイベントが発生した際に追跡します。 チェックアウトプロセスに複数のステップがある場合、複数のチェックアウトイベントが存在する可能性があります。 複数のステップがある場合、各イベントのタイムスタンプと参照されているページ／エクスペリエンスを使用して、各イベント（ステップ）を識別し、順に表現します。 |
| `commerce.productListAdds` | このイベントは、製品が製品リストまたは買い物かごに追加された際に追跡します。 |
| `commerce.productListOpens` | このイベントは、新しい製品リスト（買い物かご）が初期化または作成された際に追跡します。 |
| `commerce.productListRemovals` | このイベントは、製品エントリが製品リストまたは買い物かごから削除された際に追跡します。 |
| `commerce.productListReopens` | このイベントは、リマーケティングアクティビティを介してなど、顧客がアクセスできなくなった（破棄された）製品リスト（買い物かご）が再アクティブ化したタイミングを追跡します。 |
| `commerce.productListViews` | このイベントは、製品リストまたは買い物かごがいつビューを受け取ったかを追跡します。 |
| `commerce.productViews` | このイベントは、製品が 1 つ以上のビューを受け取った際に追跡します。 |
| `commerce.purchases` | このイベントは、注文が受け入れられた際に追跡します。 これはコマースコンバージョンで唯一必要なアクションです。購入イベントでは商品リストが参照されている必要があります。 |
| `commerce.saveForLaters` | このイベントは、製品リストなど、将来の使用のために製品リストが保存された際に追跡します。 |
| `decisioning.propositionDisplay` | このイベントは、判定の提案が人物に表示された際に追跡します。 |
| `decisioning.propositionDismiss` | このイベントは、決定が提示されたオファーに関与しなかった際に追跡します。 |
| `decisioning.propositionInteract` | このイベントは、人物が判定提案を操作した際に追跡します。 |
| `decisioning.propositionSend` | このイベントは、見込み客に対して検討のレコメンデーションまたはオファーを送信することが決定されたタイミングを追跡します。 |
| `decisioning.propositionTrigger` | このイベントは、提案プロセスのアクティブ化を追跡します。 オファーの表示を促す特定の条件またはアクションが発生しました。 |
| `delivery.feedback` | このイベントは、E メール配信などの配信のフィードバックイベントを追跡します。 |
| `directMarketing.emailBounced` | このイベントは、人物への E メールがバウンスした時を追跡します。 |
| `directMarketing.emailBouncedSoft` | このイベントは、個人への電子メールがソフトバウンスされた日時を追跡します。 |
| `directMarketing.emailClicked` | このイベントは、人がマーケティング電子メールのリンクをクリックした際に追跡します。 |
| `directMarketing.emailDelivered` | このイベントは、電子メールが人物の電子メールサービスに正常に配信されたタイミングを追跡します。 |
| `directMarketing.emailOpened` | このイベントは、人がマーケティング電子メールを開いた日時を追跡します。 |
| `directMarketing.emailSent` | このイベントは、マーケティング電子メールが人物に送信された際に追跡します。 |
| `directMarketing.emailUnsubscribed` | このイベントは、人物がいつマーケティング電子メールから購読解除されたかを追跡します。 |
| `inappmessageTracking.dismiss` | このイベントは、アプリ内メッセージが閉じられた際に追跡します。 |
| `inappmessageTracking.display` | このイベントは、アプリ内メッセージが表示された日時を追跡します。 |
| `inappmessageTracking.interact` | このイベントは、アプリ内メッセージがとやり取りされた日時を追跡します。 |
| `leadOperation.callWebhook` | このイベントは、リードに応じて Webhook が呼び出された日時を追跡します。 |
| `leadOperation.changeCampaignStream` | このイベントは、特定のビジネスリードに対するマーケティング戦略またはエンゲージメント戦略の変化を示します。 |
| `leadOperation.changeEngagementCampaignCadence` | このイベントは、キャンペーンの一環としてリードがエンゲージされた頻度に変更が生じた場合を追跡します。 |
| `leadOperation.convertLead` | このイベントは、リードがコンバートされた際に追跡します。 |
| `leadOperation.interestingMoment` | このイベントは、人物に対して興味深い瞬間が記録された日時を追跡します。 |
| `leadOperation.mergeLeads` | このイベントは、同じエンティティを参照する複数のリードからの情報が統合されたタイミングを追跡します。 |
| `leadOperation.newLead` | このイベントは、リードが作成された日時を追跡します。 |
| `leadOperation.scoreChanged` | このイベントは、リードのスコア属性の値が変更された日時を追跡します。 |
| `leadOperation.statusInCampaignProgressionChanged` | このイベントは、キャンペーン内のリードのステータスが変更された際に追跡します。 |
| `listOperation.addToList` | このイベントは、人物がマーケティングリストにいつ追加されたかを追跡します。 |
| `listOperation.removeFromList` | このイベントは、人物がマーケティングリストから削除された日時を追跡します。 |
| `media.adBreakComplete` | このイベントは、 `adBreakComplete` イベントが発生しました。 このイベントは、広告ブレークの開始時にトリガーされます。 |
| `media.adBreakStart` | このイベントは、 `adBreakStart` イベントが発生しました。 このイベントは、広告ブレークの終わりにトリガーされます。 |
| `media.adComplete` | このイベントは、 `adComplete` イベントが発生しました。 このイベントは、広告が完了したときにトリガーされます。 |
| `media.adSkip` | このイベントは、 `adSkip` イベントが発生しました。 このイベントは、広告がスキップされた場合にトリガーされます。 |
| `media.adStart` | このイベントは、 `adStart` イベントが発生しました。 このイベントは、広告が開始されたときにトリガーされます。 |
| `media.bitrateChange` | このイベントは、 `bitrateChange` イベントが発生しました。 このイベントは、ビットレートに変更がある場合にトリガーされます。 |
| `media.bufferStart` | このイベントは、 `bufferStart` イベントが発生しました。 このイベントは、メディアがバッファリングを開始したときにトリガーされます。 |
| `media.chapterComplete` | このイベントは、 `chapterComplete` イベントが発生しました。 このイベントは、メディアのチャプターが完了したときにトリガーされます。 |
| `media.chapterSkip` | このイベントは、 `chapterSkip` イベントが発生しました。 このイベントは、ユーザーがメディアコンテンツ内の別のセクションまたはチャプターにスキップして進む、または戻るとトリガーされます。 |
| `media.chapterStart` | このイベントは、 `chapterStart` イベントが発生しました。 このイベントは、メディアコンテンツ内の特定のセクションまたはチャプターの開始時にトリガーされます。 |
| `media.downloaded` | このイベントは、メディアダウンロードされたコンテンツがいつ発生したかを追跡します。 |
| `media.error` | このイベントは、 `error` イベントが発生しました。 このイベントは、メディアの再生中にエラーまたは問題が発生した場合にトリガーされます。 |
| `media.pauseStart` | このイベントは、 `pauseStart` イベントが発生しました。 このイベントは、ユーザーがメディア再生で一時停止を開始したときにトリガーされます。 |
| `media.ping` | このイベントは、 `ping` イベントが発生しました。 メディアリソースの可用性を検証します。 |
| `media.play` | このイベントは、 `play` イベントが発生しました。 このイベントは、メディアコンテンツの再生中に、ユーザーによるアクティブな消費を示してトリガーされます。 |
| `media.sessionComplete` | このイベントは、 `sessionComplete` イベントが発生しました。 このイベントは、メディア再生セッションの終わりをマークします。 |
| `media.sessionEnd` | このイベントは、 `sessionEnd` イベントが発生しました。 このイベントは、メディアセッションの終了を示します。 この結論には、メディアプレーヤーを閉じるか、再生を停止する場合があります。 |
| `media.sessionStart` | このイベントは、 `sessionStart` イベントが発生しました。 このイベントは、メディア再生セッションの開始をマークします。 ユーザーがメディアファイルの再生を開始したときにトリガーされます。 |
| `media.statesUpdate` | このイベントは、 `statesUpdate` イベントが発生しました。 プレーヤーステートトラッキング機能は、オーディオまたはビデオストリームにアタッチできます。標準の状態は、fullscreen、mute、closedCaptioning、pictureInPicture、inFocus です。 |
| `opportunityEvent.addToOpportunity` | このイベントは、人物がオポチュニティに追加された日時を追跡します。 |
| `opportunityEvent.opportunityUpdated` | このイベントは、オポチュニティが更新された日時を追跡します。 |
| `opportunityEvent.removeFromOpportunity` | このイベントは、人物がオポチュニティから削除された日時を追跡します。 |
| `pushTracking.applicationOpened` | このイベントは、人がプッシュ通知からアプリケーションを開いた日時を追跡します。 |
| `pushTracking.customAction` | このイベントは、人物がプッシュ通知でカスタムアクションを選択した日時を追跡します。 |
| `web.formFilledOut` | このイベントは、人が Web ページ上のフォームにいつ入力したかを追跡します。 |
| `web.webinteraction.linkClicks` | このイベントは、リンクが 1 回以上選択された場合に追跡します。 |
| `web.webpagedetails.pageViews` | このイベントは、Web ページが 1 つ以上のビューを受け取った際に追跡します。 |
| `location.entry` | このイベントは、特定の場所にある個人またはデバイスのエントリを追跡します。 |
| `location.exit` | このイベントは、特定の場所からの人またはデバイスの出口を追跡します。 |

{style="table-layout:auto"}

### `producedBy` の推奨値 {#producedBy}

`producedBy` の許容値を次の表に示します。

| 値 | 定義 |
| --- | --- |
| `self` | 自分 |
| `system` | システム |
| `salesRef` | 営業担当者 |
| `customerRep` | 顧客担当者 |
