---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；identityMap;ID マップ；ID マップ；スキーマデザイン；マップ；マップ；イベントモデリング；ベストプラクティス；イベント；イベント；イベント；
solution: Experience Platform
title: XDM ExperienceEvent クラス
topic-legacy: overview
description: このドキュメントでは、XDM ExperienceEvent クラスの概要と、イベントデータモデリングのベストプラクティスを説明します。
exl-id: a8e59413-b52f-4ea5-867b-8d81088a3321
source-git-commit: 32d8798d426696d8fd4ace4c53a8bf9b4db26b61
workflow-type: tm+mt
source-wordcount: '1797'
ht-degree: 3%

---

# [!DNL XDM ExperienceEvent] クラス

[!DNL XDM ExperienceEvent] は標準 Experience Data Model(XDM) クラスで、特定のイベントが発生した場合や特定の条件が満たされた場合に、システムのタイムスタンプ付きのスナップショットを作成できます。

エクスペリエンスイベントは、発生した事実（個人の特定の時点や ID を含む）を記録したものです。 イベントは、明示的（直接観察可能な人間のアクション）または暗黙的（直接人間のアクションなしで発生）に設定でき、集計や解釈なしで記録されます。 Platform エコシステムでのこのクラスの使用に関する詳細については、 [XDM の概要](../home.md#data-behaviors).

この [!DNL XDM ExperienceEvent] クラス自体は、スキーマに対して複数の時系列関連フィールドを提供します。 データの取り込み時に、これらのフィールドの一部の値が自動的に設定されます。

![](../images/classes/experienceevent/structure.png)

| プロパティ | 説明 |
| --- | --- |
| `_id` | イベントの一意の文字列識別子。 このフィールドは、個々のイベントの一意性を追跡し、データの重複を防ぎ、ダウンストリームサービスでそのイベントを検索するために使用します。 場合によっては、 `_id` は、 [UUID(Universally Unique Identifier)[UUID(Universally Unique Identifier)]](https://tools.ietf.org/html/rfc4122) または [グローバル一意識別子 (GUID)](https://docs.microsoft.com/en-us/dotnet/api/system.guid?view=net-5.0).<br><br>ソース接続からデータをストリーミングする場合、または Parquet ファイルから直接取り込む場合は、プライマリ ID、タイムスタンプ、イベントタイプなど、イベントを一意にするフィールドの特定の組み合わせを連結して、この値を生成する必要があります。 連結された値は、 `uri-reference` 書式設定された文字列。つまり、任意のコロン文字を削除する必要があります。 その後、連結された値は、SHA-256 または選択した別のアルゴリズムを使用してハッシュ化する必要があります。<br><br>これを見極める事が重要だ **このフィールドは、個人に関連する id を表すものではありません**&#x200B;ではなく、データ自体の記録です。 人物に関する ID データは、次の場所に置き換える必要があります [id フィールド](../schema/composition.md#identity) 代わりに、互換性のあるフィールドグループによって提供されます。 |
| `eventMergeId` | を使用する場合、 [Adobe Experience Platform Web SDK](../../edge/home.md) データを取り込むには、レコードが作成される原因となった、取り込まれたバッチの ID を表します。 このフィールドは、データの取り込み時にシステムによって自動的に設定されます。 Web SDK 実装のコンテキスト外でのこのフィールドの使用はサポートされていません。 |
| `eventType` | イベントのタイプまたはカテゴリを示す文字列。 このフィールドは、同じスキーマとデータセット内の異なるイベントタイプを区別する場合（小売会社の製品表示イベントと買い物かごへの追加イベントを区別する場合など）に使用できます。<br><br>このプロパティの標準値は、 [付録の節](#eventType)（使用目的の事例の説明を含む） このフィールドは拡張可能な列挙型です。つまり、独自のイベントタイプ文字列を使用して、追跡するイベントを分類することもできます。<br><br>`eventType` では、アプリケーションでのヒットごとに 1 つのイベントのみを使用するように制限されているので、最も重要なイベントをシステムに知らせるには、計算フィールドを使用する必要があります。 詳しくは、 [計算フィールドのベストプラクティス](#calculated). |
| `producedBy` | イベントのプロデューサーまたは接触チャネルを表す string 値です。 このフィールドを使用すると、セグメント化の目的で必要に応じて、特定のイベントプロデューサーを除外できます。<br><br>このプロパティの推奨値の一部は、 [付録の節](#producedBy). このフィールドは拡張可能な列挙型です。つまり、独自の文字列を使用して、異なるイベントプロデューサーを表すこともできます。 |
| `identityMap` | イベントが適用される個人の名前空間付き ID のセットを含む map フィールド。 このフィールドは、ID データが取り込まれると、システムによって自動的に更新されます。 このフィールドを適切に利用するために [リアルタイム顧客プロファイル](../../profile/home.md)の場合は、データ操作でフィールドのコンテンツを手動で更新しようとしないでください。<br /><br />詳しくは、 [スキーマ構成の基本](../schema/composition.md#identityMap) を参照してください。 |
| `timestamp` | イベントが発生した時点の ISO 8601 タイムスタンプ ( 形式： [RFC 3339 セクション 5.6](https://tools.ietf.org/html/rfc3339#section-5.6). このタイムスタンプは過去に設定する必要があります。 以下のの節を参照してください： [タイムスタンプ](#timestamps) を参照してください。 |

{style=&quot;table-layout:auto&quot;}

## イベントモデリングのベストプラクティス

以下の節では、Adobe Experience Platformでイベントベースの Experience Data Model(XDM) スキーマをデザインする際のベストプラクティスについて説明します。

### タイムスタンプ {#timestamps}

ルート `timestamp` イベントスキーマのフィールド **のみ** は、過去に起こった、イベント自体の観測を表します。 セグメントの使用例で、将来発生する可能性のあるタイムスタンプの使用が必要な場合、これらの値はエクスペリエンスイベントスキーマの他の場所で制限する必要があります。

例えば、旅行業界や接客業のビジネスがフライト予約イベントをモデリングしている場合、クラスレベル `timestamp` フィールドは、予約イベントが観測された時間を表します。 旅行予約の開始日など、イベントに関連するその他のタイムスタンプは、標準フィールドまたはカスタムフィールドグループが提供する別のフィールドにキャプチャする必要があります。

![](../images/classes/experienceevent/timestamps.png)

イベントスキーマ内の他の関連する日時値とは別に、クラスレベルのタイムスタンプを保持することで、エクスペリエンスアプリケーションでカスタマージャーニーのタイムスタンプ付きの説明を保持しながら、柔軟なセグメント化の使用例を実装できます。

### 計算フィールドの使用 {#calculated}

エクスペリエンスアプリケーション内の特定のインタラクションでは、同じイベントタイムスタンプを技術的に共有する複数の関連イベントが発生する可能性があり、単一のイベントレコードとして表現できます。 例えば、顧客が Web サイトで製品を表示すると、イベントレコードに 2 つの潜在的な `eventType` 値：「製品表示」イベント (`commerce.productViews`) または汎用の「ページビュー」イベント (`web.webpagedetails.pageViews`) をクリックします。 このような場合、計算フィールドを使用して、1 回のヒットで複数のイベントがキャプチャされる際に、最も重要な属性をキャプチャできます。

[Adobe Experience Platform Data Prep](../../data-prep/home.md) では、XDM との間でデータのマッピング、変換、検証をおこなうことができます。 使用可能な [マッピング関数](../../data-prep/functions.md) サービスによって提供される論理演算子を呼び出して、Experience Platformに取り込まれる際に、複数イベントレコードのデータを優先順位付け、変換、統合することができます。 上記の例では、 `eventType` は、両方が発生した場合に「製品表示」よりも「ページ表示」の方を優先する計算フィールドとして使用します。

UI を使用して手動で Platform にデータを取り込む場合は、 [計算フィールド](../../data-prep/ui/mapping.md#calculated-fields) を参照してください。

ソース接続を使用してデータを Platform にストリーミングする場合は、代わりに計算フィールドを使用するようにソースを設定できます。 詳しくは、 [特定のソースのドキュメント](../../sources/home.md) を参照してください。

## 互換性のあるスキーマフィールドグループ {#field-groups}

>[!NOTE]
>
>複数のフィールドグループの名前が変更されました。 次のドキュメントを参照してください： [フィールドグループ名の更新](../field-groups/name-updates.md) を参照してください。

Adobeには、 [!DNL XDM ExperienceEvent] クラス。 次に、クラスで一般的に使用されるフィールドグループのリストを示します。

* [[!UICONTROL Adobe Analytics ExperienceEvent Full 拡張機能]](../field-groups/event/analytics-full-extension.md)
* [[!UICONTROL 残高移動]](../field-groups/event/balance-transfers.md)
* [[!UICONTROL キャンペーンマーケティングの詳細]](../field-groups/event/campaign-marketing-details.md)
* [[!UICONTROL カードアクション]](../field-groups/event/card-actions.md)
* [[!UICONTROL チャネルの詳細]](../field-groups/event/channel-details.md)
* [[!UICONTROL コマースの詳細]](../field-groups/event/commerce-details.md)
* [[!UICONTROL 預金の詳細]](../field-groups/event/deposit-details.md)
* [[!UICONTROL デバイスの下取りの詳細]](../field-groups/event/device-trade-in-details.md)
* [[!UICONTROL 食事の予約]](../field-groups/event/dining-reservation.md)
* [[!UICONTROL エンドユーザー ID の詳細]](../field-groups/event/enduserids.md)
* [[!UICONTROL 環境の詳細]](../field-groups/event/environment-details.md)
* [[!UICONTROL フライトの予約]](../field-groups/event/flight-reservation.md)
* [[!UICONTROL IAB TCF 2.0 同意]](../field-groups/event/iab.md)
* [[!UICONTROL 宿泊施設の予約]](../field-groups/event/lodging-reservation.md)
* [[!UICONTROL 見積依頼の詳細]](../field-groups/event/quote-request-details.md)
* [[!UICONTROL 予約の詳細]](../field-groups/event/reservation-details.md)
* [[!UICONTROL Web の詳細]](../field-groups/event/web-details.md)

## 付録

次の節では、 [!UICONTROL XDM ExperienceEvent] クラス。

### 指定できる値： `eventType` {#eventType}

次の表に、 `eventType`とその定義を組み合わせて使用します。

| 値 | 定義 |
| --- | --- |
| `advertising.clicks` | 広告でのクリックアクション。 |
| `advertising.completes` | タイムドメディアアセットが最後まで視聴されました。 視聴者が先にスキップした可能性があるので、必ずしも視聴者がビデオ全体を視聴したとは限りません。 |
| `advertising.conversions` | パフォーマンス評価用にイベントをトリガーする、顧客が実行した事前定義済みのアクション。 |
| `advertising.federated` | エクスペリエンスイベントがデータフェデレーション（顧客間のデータ共有）を通じて作成されたかどうかを示します。 |
| `advertising.firstQuartiles` | デジタルビデオ広告は、通常の速度で再生時間の 25％ を再生しました。 |
| `advertising.impressions` | 表示される可能性のある顧客に対する広告のインプレッション。 |
| `advertising.midpoints` | デジタルビデオ広告は、通常の速度で再生時間の 50％を再生しました。 |
| `advertising.starts` | デジタルビデオ広告の再生が開始されました。 |
| `advertising.thirdQuartiles` | デジタルビデオ広告は、通常の速度で再生時間の 75%を再生しました。 |
| `advertising.timePlayed` | 特定のタイムドメディアアセットでユーザーが費やした時間を示します。 |
| `application.close` | アプリケーションが閉じられたか、バックグラウンドに送信されました。 |
| `application.launch` | アプリケーションが起動したか、フォアグラウンドに移動しました。 |
| `commerce.checkouts` | 製品リストのチェックアウトイベントが発生しました。 チェックアウトプロセスに複数のステップがある場合、複数のチェックアウトイベントが存在する可能性があります。 複数の手順がある場合、各イベントのタイムスタンプと参照されているページ/エクスペリエンスを使用して、各イベント（手順）を順番に識別します。 |
| `commerce.productListAdds` | 製品が製品リストまたは買い物かごに追加されました。 |
| `commerce.productListOpens` | 新しい製品リスト（買い物かご）が初期化または作成されました。 |
| `commerce.productListRemovals` | 1 つ以上の製品エントリが製品リストまたは買い物かごから削除されました。 |
| `commerce.productListReopens` | アクセスできなくなった（放棄された）製品リスト（買い物かご）は、リマーケティングアクティビティを介してなど、顧客によって再アクティブ化されました。 |
| `commerce.productListViews` | 製品リストまたは買い物かごに 1 つ以上の表示がありました。 |
| `commerce.productViews` | 1 つの製品が 1 つ以上の表示を受け取りました。 |
| `commerce.purchases` | 注文が受け入れられました。コマースコンバージョンで必要なアクションはこれだけです。 購入イベントでは、製品リストが参照されている必要があります。 |
| `commerce.saveForLaters` | 製品リストは、今後の使用のために保存されました（例：製品ウィッシュリスト）。 |
| `decisioning.propositionDisplay` | 判定の提案が個人に表示されました。 |
| `decisioning.propositionInteract` | 人物が意思決定の提案を操作した。 |
| `delivery.feedback` | E メール配信など、配信のフィードバックイベント。 |
| `directMarketing.emailBounced` | バウンスした人に送信する E メール。 |
| `directMarketing.emailBouncedSoft` | ソフトバウンスされた人物への E メール。 |
| `directMarketing.emailClicked` | ある人がマーケティング電子メールのリンクをクリックしました。 |
| `directMarketing.emailDelivered` | 担当者の電子メールサービスに電子メールが正常に配信されました |
| `directMarketing.emailOpened` | マーケティング電子メールを開いた人。 |
| `directMarketing.emailUnsubscribed` | マーケティング電子メールの購読を解除した人。 |
| `inappmessageTracking.dismiss` | アプリ内メッセージが閉じられました。 |
| `inappmessageTracking.display` | アプリ内メッセージが表示されました。 |
| `inappmessageTracking.interact` | アプリ内メッセージがとやり取りしました。 |
| `leadOperation.callWebhook` | リードに応じてウェブフックが呼び出されました。 |
| `leadOperation.convertLead` | リードが変換されました。 |
| `leadOperation.interestingMoment` | ある人にとって面白い瞬間が記録された。 |
| `leadOperation.newLead` | リードが作成されました。 |
| `leadOperation.scoreChanged` | リードのスコア属性の値が変更されました。 |
| `leadOperation.statusInCampaignProgressionChanged` | キャンペーン内のリードのステータスが変更されました。 |
| `listOperation.addToList` | マーケティングリストに人が追加されました。 |
| `listOperation.removeFromList` | マーケティングリストから人が削除されました。 |
| `message.feedback` | 顧客に送信されたメッセージの送信/バウンス/エラーなどのフィードバックイベント。 |
| `message.tracking` | 顧客に送信されたメッセージの開く、クリック、カスタムアクションなどのイベントをトラッキングする。 |
| `opportunityEvent.addToOpportunity` | 担当者がオポチュニティに追加されました。 |
| `opportunityEvent.opportunityUpdated` | オポチュニティが更新されました。 |
| `opportunityEvent.removeFromOpportunity` | 担当者がオポチュニティから削除されました。 |
| `pushTracking.applicationOpened` | プッシュ通知からアプリを開いた人。 |
| `pushTracking.customAction` | 人物がプッシュ通知でカスタムアクションをクリックしました。 |
| `web.formFilledOut` | 人が Wep ページのフォームに入力しました。 |
| `web.webinteraction.linkClicks` | リンクが 1 回以上選択されました。 |
| `web.webpagedetails.pageViews` | Web ページに 1 つ以上のビューが届きました。 |

{style=&quot;table-layout:auto&quot;}

### 推奨値： `producedBy` {#producedBy}

次の表に、 `producedBy`:

| 値 | 定義 |
| --- | --- |
| `self` | 自分 |
| `system` | システム |
| `salesRef` | セールス担当者 |
| `customerRep` | 顧客担当者 |
