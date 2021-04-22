---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；個々のプロファイル；フィールド；スキーマ;スキーマ;identityMap；アイデンティティマップ；スキーマ設計；マップ；和集合スキーマ;和集合
solution: Experience Platform
title: XDM ExperienceEventクラス
topic-legacy: overview
description: このドキュメントでは、XDM ExperienceEventクラスの概要を説明します。
exl-id: a8e59413-b52f-4ea5-867b-8d81088a3321
translation-type: tm+mt
source-git-commit: 9b63b38e664e5776ca638f8ed407896f185bcab0
workflow-type: tm+mt
source-wordcount: '869'
ht-degree: 6%

---

# [!DNL XDM ExperienceEvent] class

[!DNL XDM ExperienceEvent] は標準のXDMクラスで、特定のイベントが発生した場合や特定の条件が満たされた場合に、システムのタイムスタンプ付きのスナップショットを作成できます。

エクスペリエンスイベントは、発生した事実（特定の時点や個人の身元など）を記録したものです。 イベントは、明示的（直接観察可能な人間の行動）か、暗黙的（直接的な人間の行動なしで発生）のいずれかで、集約や解釈なしで記録されます。 プラットフォームエコシステムでのこのクラスの使用に関する詳細は、[XDMの概要](../home.md#data-behaviors)を参照してください。

[!DNL XDM ExperienceEvent]クラス自体は、スキーマに対して時系列関連のフィールドをいくつか提供します。 データを取り込むと、以下の一部のフィールドの値が自動的に入力されます。

<img src="../images/classes/experienceevent.png" width="650" /><br />

| プロパティ | 説明 |
| --- | --- |
| `_id` | システムで生成された、イベントの一意の文字列識別子。 このフィールドは、個々のイベントの一意性を追跡し、データの重複を防ぎ、そのイベントをダウンストリームサービスで調べるために使用します。 このフィールドはシステム生成なので、データ取り込み中に明示的な値を指定しないでください。<br><br>このフィールドは、個々の人に関連するアイデンティティ **ではなく、データの記録自体を表しているのではな** いことを区別することが重要です。個人に関するIDデータは、代わりに[IDフィールド](../schema/composition.md#identity)に左右する必要があります。 |
| `eventMergeId` | レコードを作成した、取り込んだバッチのID。 このフィールドは、データ取り込み時にシステムによって自動的に入力されます。 |
| `eventType` | レコードの主イベントタイプを示す文字列。 受け入れられた値とその定義は、[付録セクション](#eventType)に記載されています。 |
| `identityMap` | イベントが適用される個々の名前空間IDのセットが含まれるmapフィールド。 このフィールドは、IDデータが取り込まれると自動的に更新されます。 このフィールドを[リアルタイム顧客プロファイル](../../profile/home.md)に適切に利用するため、データ操作でフィールドの内容を手動で更新しないでください。<br /><br />使用事例の詳細については、「スキーマ [合成の](../schema/composition.md#identityMap) 基本」の「IDマップ」の節を参照してください。 |
| `timestamp` | イベントが発生した時のISO 8601タイムスタンプ。[RFC 3339 Section 5.6](https://tools.ietf.org/html/rfc3339#section-5.6)に従ってフォーマットされます。<br><br>このタイムスタンプは、イベント自体の観察を **** 示すものであり、過去に発生する必要があります。セグメントの使用例で、将来発生する可能性のあるタイムスタンプ（出発日など）を使用する必要がある場合、これらの値はエクスペリエンスイベントスキーマの他の場所で制限する必要があります。 |

## 互換性のあるミックスイン{#mixins}

>[!NOTE]
>
>いくつかのミックスインの名前が変更されました。 詳しくは、[mixin name updates](../mixins/name-updates.md)のドキュメントを参照してください。

Adobeは、[!DNL XDM ExperienceEvent]クラスで使用するいくつかの標準ミックスインを提供します。 以下は、このクラスで一般的に使用されるミックスインのリストです。

* [[!UICONTROL エンドユーザーIDの詳細]](../mixins/event/enduserids.md)
* [[!UICONTROL 環境の詳細]](../mixins/event/environment-details.md)

## 付録

次の節では、[!UICONTROL XDM ExperienceEvent]クラスに関する追加情報を説明します。

### xdm:eventType {#eventType}に指定できる値

次の表に、`xdm:eventType`に使用できる値とその定義を示します。

| 値 | 定義 |
| --- | --- |
| `advertising.completes` | 時間指定メディアアセットが完了まで視聴されました。 これは、視聴者が先にスキップした可能性があるので、ビデオ全体を視聴したとは限りません。 |
| `advertising.timePlayed` | 特定の時間指定メディアアセットに対するユーザーの滞在時間を示します。 |
| `advertising.federated` | エクスペリエンスイベントがデータフェデレーション（顧客間のデータ共有）を通じて作成されたかどうかを示します。 |
| `advertising.clicks` | 広告の「アクション」をクリックします。 |
| `advertising.conversions` | パフォーマンス評価のイベントをトリガーする顧客が実行する事前定義済みのアクション。 |
| `advertising.firstQuartiles` | デジタルビデオ広告は、通常の速度で再生時間の 25％ を再生しました。 |
| `advertising.impressions` | 視聴可能な顧客に対する広告のインプレッション。 |
| `advertising.midpoints` | デジタルビデオ広告は、通常の速度で再生時間の 50％を再生しました。 |
| `advertising.starts` | デジタルビデオ広告の再生が開始されました。 |
| `advertising.thirdQuartiles` | デジタルビデオ広告は、通常の速度で再生時間の 75%を再生しました。 |
| `web.webpagedetails.pageViews` | Webページが1つ以上の表示を受信しました。 |
| `web.webinteraction.linkClicks` | リンクが1回以上選択されています。 |
| `commerce.checkouts` | 製品リストに対してチェックアウトイベントが発生しました。 チェックアウトプロセスに複数のステップがある場合は、複数のチェックアウトイベントが存在する可能性があります。 複数の手順がある場合、各イベントのタイムスタンプと参照先のページ/エクスペリエンスを使用して、各イベント（手順）を順番に識別します。 |
| `commerce.productListAdds` | 商品が商品リストまたは買い物かごに追加されました。 |
| `commerce.productListOpens` | 新しい製品リスト（買い物かご）が初期化または作成されました。 |
| `commerce.productListRemovals` | 1つ以上の商品エントリが商品リストまたは買い物かごから削除されました。 |
| `commerce.productListReopens` | アクセスできなくなった（破棄された）製品リスト（買い物かご）は、再マーケティングアクティビティを介して、顧客によって再アクティブ化されました。 |
| `commerce.productListViews` | 製品リストまたは買い物かごが1つ以上の表示を受け取りました。 |
| `commerce.productViews` | 製品が1つ以上の表示を受け取りました。 |
| `commerce.purchases` | 注文が受け入れられました。コマースコンバージョンで必要なアクションはこれだけです。 購入イベントは、商品リストを参照する必要があります。 |
| `commerce.saveForLaters` | 製品リストは、製品ウィッシュリストなど、将来的に使用するために保存されています。 |
| `delivery.feedback` | 電子メール配信など、配信に対するフィードバックイベント。 |
| `message.feedback` | 顧客に送信するメッセージに対する送信/バウンス/エラーなどのフィードバックイベント。 |
| `message.tracking` | 顧客に送信されたメッセージに対するオープン/クリック/カスタムアクションなどのイベントを追跡する。 |
