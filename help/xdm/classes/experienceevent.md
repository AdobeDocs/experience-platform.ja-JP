---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；identityMap;IDマップ；IDマップ；スキーマ設計；マップ；マップ；イベントモデリング；イベントモデリング；ベストプラクティス；イベント；イベント；
solution: Experience Platform
title: XDM ExperienceEventクラス
topic-legacy: overview
description: このドキュメントでは、XDM ExperienceEventクラスの概要と、イベントデータモデリングのベストプラクティスを説明します。
exl-id: a8e59413-b52f-4ea5-867b-8d81088a3321
source-git-commit: 5405a2e2312e81db210a97a759681f66faa8b1fa
workflow-type: tm+mt
source-wordcount: '1759'
ht-degree: 3%

---

# [!DNL XDM ExperienceEvent] class

[!DNL XDM ExperienceEvent] は標準のエクスペリエンスデータモデル(XDM)クラスで、特定のイベントが発生した場合や特定の条件セットに達した場合に、システムのタイムスタンプ付きのスナップショットを作成できます。

エクスペリエンスイベントは、発生した事実（特定の時点や個人のIDなど）を記録したものです。 イベントは、明示的（直接観察可能な人間のアクション）または暗黙的（直接人間のアクションなしで発生）に設定でき、集計や解釈なしで記録されます。 プラットフォームエコシステムでのこのクラスの使用に関する概要については、[XDMの概要](../home.md#data-behaviors)を参照してください。

[!DNL XDM ExperienceEvent]クラス自体は、1つのスキーマに時系列関連のフィールドを複数提供します。 データの取得時に、これらのフィールドの一部の値が自動的に設定されます。

![](../images/classes/experienceevent/structure.png)

| プロパティ | 説明 |
| --- | --- |
| `_id` | イベントの一意の文字列識別子。 このフィールドは、個々のイベントの一意性を追跡し、データの重複を防ぎ、ダウンストリームサービスでそのイベントを検索するために使用します。 場合によっては、`_id`を[Universally Unique Identifier(UUID)](https://tools.ietf.org/html/rfc4122)または[Globally Unique Identifier(GUID)](https://docs.microsoft.com/en-us/dotnet/api/system.guid?view=net-5.0)にすることができます。<br><br>ソース接続からデータをストリーミングする場合、またはParquetファイルから直接取り込む場合は、プライマリID、タイムスタンプ、イベントタイプなど、イベントを一意にするフィールドの組み合わせを連結して、この値を生成する必要があります。連結された値は、`uri-reference`形式の文字列である必要があります。つまり、コロン文字は削除する必要があります。 その後、連結された値は、SHA-256または任意の別のアルゴリズムを使用してハッシュ化する必要があります。<br><br>このフィールドは、個人 **に関連するIDではなく、データ自体のレコードを表すという点を区別することが重要です**。個人に関するIDデータは、互換性のあるフィールドグループから提供される[IDフィールド](../schema/composition.md#identity)に置き換える必要があります。 |
| `eventMergeId` | [Adobe Experience Platform Web SDK](../../edge/home.md)を使用してデータを取り込む場合、これはレコードが作成される原因となった取り込みバッチのIDを表します。 このフィールドは、データの取り込み時にシステムによって自動的に設定されます。 Web SDK実装のコンテキスト以外でのこのフィールドの使用はサポートされていません。 |
| `eventType` | イベントのタイプまたはカテゴリを示す文字列。 このフィールドは、製品表示イベントと小売会社の買い物かごへの追加イベントを区別するなど、同じスキーマとデータセット内の異なるイベントタイプを区別する場合に使用できます。<br><br>このプロパティの標準値は、使用例の説明を含 [め、付録の節](#eventType)で説明しています。このフィールドは拡張可能な列挙型で、独自のイベントタイプ文字列を使用して、追跡するイベントを分類することもできます。<br><br>`eventType` では、アプリケーションでのヒットごとに1つのイベントのみを使用できるように制限するので、最も重要なイベントをシステムに知らせるには、計算フィールドを使用する必要があります。詳しくは、[計算フィールドのベストプラクティス](#calculated)に関する節を参照してください。 |
| `producedBy` | イベントのプロデューサーまたは発生元を表すstring値です。 このフィールドを使用すると、セグメント化のために必要な場合に、特定のイベントプロデューサーを除外できます。<br><br>このプロパティの推奨値は、付録の節に記載さ [れています](#producedBy)。このフィールドは拡張可能な列挙型で、独自の文字列を使用して、異なるイベントプロデューサーを表すこともできます。 |
| `identityMap` | イベントが適用される個人の名前空間付きIDのセットを含むmapフィールド。 このフィールドは、IDデータが取り込まれると自動的に更新されます。 このフィールドを[リアルタイム顧客プロファイル](../../profile/home.md)に適切に利用するには、データ操作でフィールドの内容を手動で更新しないでください。<br /><br />使用例について詳しくは、「スキーマ合成の基 [本」の「 IDマッ](../schema/composition.md#identityMap) プ」の節を参照してください。 |
| `timestamp` | イベントが発生した時点のISO 8601タイムスタンプ（ [RFC 3339 Section 5.6](https://tools.ietf.org/html/rfc3339#section-5.6)に従って形式設定）。 このタイムスタンプは過去に発生する必要があります。 このフィールドの使用に関するベストプラクティスについては、以下の[タイムスタンプ](#timestamps)の節を参照してください。 |

{style=&quot;table-layout:auto&quot;}

## イベントモデリングのベストプラクティス

以下の節では、Adobe Experience Platformでイベントベースのエクスペリエンスデータモデル(XDM)スキーマをデザインする際のベストプラクティスについて説明します。

### タイムスタンプ {#timestamps}

イベントスキーマのルート`timestamp`フィールドは、**のみ**&#x200B;で、イベント自体の観察を表すことができ、過去に存在する必要があります。 セグメントの使用例で、将来発生する可能性のあるタイムスタンプの使用が必要な場合、これらの値はエクスペリエンスイベントスキーマの他の場所で制限する必要があります。

例えば、旅行・接客業の企業がフライト予約イベントをモデリングしている場合、クラスレベルの`timestamp`フィールドは予約イベントが観察された時刻を表します。 旅行予約の開始日など、イベントに関連するその他のタイムスタンプは、標準フィールドグループまたはカスタムフィールドグループで提供される個別のフィールドに取り込む必要があります。

![](../images/classes/experienceevent/timestamps.png)

クラスレベルのタイムスタンプをイベントスキーマ内の他の関連する日時値とは別に保つことで、エクスペリエンスアプリケーションでカスタマージャーニーのタイムスタンプ付きの説明を保持しながら、柔軟なセグメント化の使用例を実装できます。

### 計算フィールドの使用 {#calculated}

エクスペリエンスアプリケーションの特定のインタラクションは、複数の関連イベントを生成し、同じイベントタイムスタンプを技術的に共有するので、単一のイベントレコードとして表すことができます。 例えば、顧客がWebサイトで製品を表示すると、次の2つの`eventType`値を持つイベントレコードが生成される可能性があります。「製品表示」イベント(`commerce.productViews`)または汎用の「ページ表示」イベント(`web.webpagedetails.pageViews`)。 このような場合、計算フィールドを使用して、1回のヒットで複数のイベントをキャプチャする際に最も重要な属性を取り込むことができます。

[Adobe Experience Platform Data ](../../data-prep/home.md) Prepaloes XDMとの間で、データのマッピング、変換、検証をおこなうことができます。Experience Platformに取り込まれる際に、論理演算子を呼び出して、複数イベントレコードのデータの優先順位付け、変換、統合をおこなうことができます。 [](../../data-prep/functions.md)上記の例では、「製品表示」と「ページ表示」の両方が発生したときに優先順位を付ける計算フィールドとして`eventType`を指定できます。

UIを使用してデータをPlatformに手動で取り込む場合、計算フィールドの作成方法に関する具体的な手順については、『[計算フィールドガイド](../../data-prep/calculated-fields.md)』を参照してください。

ソース接続を使用してPlatformにデータをストリーミングする場合は、代わりに計算フィールドを使用するようにソースを設定できます。 接続を設定する際の計算フィールドの実装方法については、特定のソース](../../sources/home.md)の[ドキュメントを参照してください。

## 互換性のあるスキーマフィールドグループ {#field-groups}

>[!NOTE]
>
>複数のフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../field-groups/name-updates.md)のドキュメントを参照してください。

Adobeは、[!DNL XDM ExperienceEvent]クラスで使用するいくつかの標準フィールドグループを提供します。 次に、クラスで一般的に使用されるフィールドグループのリストを示します。

* [[!UICONTROL キャンペーンマーケティングの詳細]](../field-groups/event/campaign-marketing-details.md)
* [[!UICONTROL チャネルの詳細]](../field-groups/event/channel-details.md)
* [[!UICONTROL コマースの詳細]](../field-groups/event/commerce-details.md)
* [[!UICONTROL デバイスのトレードインの詳細]](../field-groups/event/device-trade-in-details.md)
* [[!UICONTROL 食事予約]](../field-groups/event/dining-reservation.md)
* [[!UICONTROL エンドユーザーIDの詳細]](../field-groups/event/enduserids.md)
* [[!UICONTROL 環境の詳細]](../field-groups/event/environment-details.md)
* [[!UICONTROL 飛行予約]](../field-groups/event/flight-reservation.md)
* [[!UICONTROL IAB TCF 2.0の同意]](../field-groups/event/iab.md)
* [[!UICONTROL 宿泊予約]](../field-groups/event/lodging-reservation.md)
* [[!UICONTROL 予約の詳細]](../field-groups/event/reservation-details.md)
* [[!UICONTROL Webの詳細]](../field-groups/event/web-details.md)

## 付録

次の節では、[!UICONTROL XDM ExperienceEvent]クラスに関する追加情報を示します。

### `eventType`に指定できる値 {#eventType}

`eventType`に指定できる値とその定義を次の表に示します。

| 値 | 定義 |
| --- | --- |
| `advertising.clicks` | 広告のアクションをクリックします。 |
| `advertising.completes` | 時間指定メディアアセットが最後まで視聴されました。 視聴者が先にスキップした可能性があるので、必ずしも視聴者がビデオ全体を視聴したとは限りません。 |
| `advertising.conversions` | パフォーマンス評価のためのイベントをトリガーする顧客が実行する事前定義済みのアクション。 |
| `advertising.federated` | エクスペリエンスイベントがデータフェデレーション（顧客間でのデータ共有）を通じて作成されたかどうかを示します。 |
| `advertising.firstQuartiles` | デジタルビデオ広告は、通常の速度で再生時間の 25％ を再生しました。 |
| `advertising.impressions` | 顧客に対する広告のインプレッション（複数可）。 |
| `advertising.midpoints` | デジタルビデオ広告は、通常の速度で再生時間の 50％を再生しました。 |
| `advertising.starts` | デジタルビデオ広告の再生が開始されました。 |
| `advertising.thirdQuartiles` | デジタルビデオ広告は、通常の速度で再生時間の 75%を再生しました。 |
| `advertising.timePlayed` | 特定の時間指定メディアアセットに対するユーザーの滞在時間を示します。 |
| `application.close` | アプリケーションが閉じられたか、バックグラウンドに移行されました。 |
| `application.launch` | アプリケーションが起動されたか、フォアグラウンドに移動した。 |
| `commerce.checkouts` | 製品リストのチェックアウトイベントが発生しました。 チェックアウトプロセスに複数の手順がある場合、複数のチェックアウトイベントが存在する可能性があります。 複数の手順がある場合、各イベントのタイムスタンプと参照されるページ/エクスペリエンスが、順番に表される個々のイベント（手順）を識別するために使用されます。 |
| `commerce.productListAdds` | 製品が製品リストまたは買い物かごに追加されました。 |
| `commerce.productListOpens` | 新しい製品リスト（買い物かご）が初期化されたか、作成されました。 |
| `commerce.productListRemovals` | 1つ以上の製品エントリが製品リストまたは買い物かごから削除されました。 |
| `commerce.productListReopens` | アクセスできなくなった（破棄された）製品リスト（買い物かご）が、リマーケティングアクティビティを介してなど、顧客によって再アクティブ化された。 |
| `commerce.productListViews` | 製品リストまたは買い物かごに1つ以上の表示がある。 |
| `commerce.productViews` | 製品が1つ以上の表示を受け取りました。 |
| `commerce.purchases` | 注文が受け入れられました。コマース変換で必要なアクションはこれだけです。 購入イベントでは、製品リストを参照する必要があります。 |
| `commerce.saveForLaters` | 製品リストは、製品の一覧など、今後の使用のために保存されました。 |
| `decisioning.propositionDisplay` | 判定の提案が人に表示されました。 |
| `decisioning.propositionInteract` | 判定提案に対して何らかのアクションを起こした人。 |
| `delivery.feedback` | Eメール配信など、配信のフィードバックイベント。 |
| `directMarketing.emailBounced` | バウンスした人へのEメール。 |
| `directMarketing.emailBouncedSoft` | ソフトバウンスされたユーザーへのEメール。 |
| `directMarketing.emailClicked` | ある人がマーケティング用電子メールのリンクをクリックしました。 |
| `directMarketing.emailDelivered` | ユーザーの電子メールサービスに電子メールが正常に配信されました |
| `directMarketing.emailOpened` | ある人がマーケティング用のEメールを開きました。 |
| `directMarketing.emailUnsubscribed` | マーケティング用のEメールを購読解除したユーザー。 |
| `leadOperation.convertLead` | リードが変換されました。 |
| `leadOperation.interestingMoment` | ある人にとって興味深い瞬間が記録された。 |
| `leadOperation.newLead` | リードが作成されました。 |
| `leadOperation.scoreChanged` | リードのスコア属性の値が変更されました。 |
| `leadOperation.statusInCampaignProgressionChanged` | キャンペーン内のリードのステータスが変更されました。 |
| `listOperation.addToList` | マーケティングリストにユーザーが追加されました。 |
| `listOperation.removeFromList` | マーケティングリストから削除されたユーザー。 |
| `message.feedback` | 顧客に送信されたメッセージの送信/バウンス/エラーなどのフィードバックイベント。 |
| `message.tracking` | 顧客に送信されたメッセージに対する開く/クリック/カスタムアクションなどのイベントをトラッキングする。 |
| `opportunityEvent.addToOpportunity` | 機会に人を加えた。 |
| `opportunityEvent.opportunityUpdated` | オポチュニティが更新されました。 |
| `opportunityEvent.removeFromOpportunity` | 機会を失った人。 |
| `pushTracking.applicationOpened` | プッシュ通知からアプリを開いたユーザー。 |
| `pushTracking.customAction` | ユーザーがプッシュ通知でカスタムアクションをクリックしました。 |
| `web.formFilledOut` | Wepページのフォームに記入した人。 |
| `web.webinteraction.linkClicks` | リンクが1回以上選択されました。 |
| `web.webpagedetails.pageViews` | Webページに1つ以上のビューが表示されました。 |

{style=&quot;table-layout:auto&quot;}

### `producedBy`の推奨値 {#producedBy}

次の表に、`producedBy`で使用できる値の概要を示します。

| 値 | 定義 |
| --- | --- |
| `self` | 自己 |
| `system` | システム |
| `salesRef` | 営業担当 |
| `customerRep` | 顧客担当者 |
