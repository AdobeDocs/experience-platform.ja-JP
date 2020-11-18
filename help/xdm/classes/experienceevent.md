---
keywords: Experience Platform;home;popular topics;schema;Schema;XDM;individual profile;fields;schemas;Schemas;identityMap;identity map;Identity map;Schema design;map;Map;union schema;union
solution: Experience Platform
title: XDM ExperienceEventクラス
topic: overview
description: このドキュメントでは、XDM ExperienceEventクラスの概要を説明します。
translation-type: tm+mt
source-git-commit: 9e55e9ef6c619a952ca7519ca05dab59da2c573b
workflow-type: tm+mt
source-wordcount: '794'
ht-degree: 6%

---


# [!DNL XDM ExperienceEvent] class

[!DNL XDM ExperienceEvent] は標準のXDMクラスで、特定のイベントが発生した場合や特定の条件が満たされた場合に、システムのタイムスタンプ付きのスナップショットを作成できます。

エクスペリエンスイベントは、発生した事実（特定の時点や個人の身元など）を記録したものです。 イベントは、明示的（直接観察可能な人間の行動）か、暗黙的（直接的な人間の行動なしで発生）のいずれかで、集約や解釈なしで記録されます。 プラットフォームエコシステムでのこのクラスの使用に関する詳細は、 [XDMの概要を参照してください](../home.md#data-behaviors)。

この [!DNL XDM ExperienceEvent] クラス自体は、スキーマに対して複数の時系列関連のフィールドを提供します。 データを取り込むと、以下の一部のフィールドの値が自動的に入力されます。

<img src="../images/classes/experienceevent.png" width="650" /><br />

| プロパティ | 説明 |
| --- | --- |
| `_id` | システムで生成された、イベントの一意の文字列識別子。 このフィールドは、個々のイベントの一意性を追跡し、データの重複を防ぎ、そのイベントをダウンストリームサービスで調べるために使用します。 このフィールドはシステム生成なので、データ取り込み中に明示的な値を指定しないでください。<br><br>このフィールドは、個人に関連するIDではなく、データ **** 自体のレコードを表すものであると区別することが重要です。 個人に関連するIDデータは、 [IDフィールドに左右する必要があります](../schema/composition.md#identity) 。 |
| `eventMergeId` | レコードを作成した、取り込んだバッチのID。 このフィールドは、データ取り込み時にシステムによって自動的に入力されます。 |
| `eventType` | レコードの主イベントタイプを示す文字列。 指定できる値とその定義については、 [付録の節で説明します](#eventType)。 |
| `identityMap` | イベントが適用される個々の名前空間IDのセットが含まれるmapフィールド。 このフィールドは、IDデータが取り込まれると自動的に更新されます。 このフィールドを [リアルタイム顧客プロファイルに適切に活用するため](../../profile/home.md)、データ操作で手動でフィールドの内容を更新しないでください。<br /><br />使用事例について詳しくは、スキーマ構成の [基本のIDマップに関する節を参照してください](../schema/composition.md#identityMap) 。 |
| `timestamp` | イベントまたは観測が発生した時刻。 [RFC 3339 Section 5.6](https://tools.ietf.org/html/rfc3339#section-5.6))に従ってフォーマットされます。 |

## 互換性のあるミックスイン {#mixins}

>[!NOTE]
>
>いくつかのミックスインの名前が変更されました。 詳しくは、 [mixin名の更新に関するドキュメントを参照してください](../mixins/name-updates.md) 。

Adobeは、クラスで使用するいくつかの標準ミックスインを提供し [!DNL XDM ExperienceEvent] ます。 以下は、このクラスで一般的に使用されるミックスインのリストです。

* [[!UICONTROL エンドユーザーIDの詳細]](../mixins/event/enduserids.md)
* [[!UICONTROL 環境の詳細]](../mixins/event/environment-details.md)

## 付録

次の節では、 [!UICONTROL XDM ExperienceEvent] クラスに関する追加情報を説明します。

### xdm:eventTypeに指定できる値です。 {#eventType}

次の表に、で使用可能な値とその定義 `xdm:eventType`を示します。

| 値 | 定義 |
| --- | --- |
| `advertising.completes` | 時間指定メディアアセットが完了まで視聴されました。 これは、視聴者が先にスキップした可能性があるので、ビデオ全体を視聴したとは限りません。 |
| `advertising.timePlayed` | 特定の時間指定メディアアセットに対するユーザーの滞在時間を示します。 |
| `advertising.federated` | エクスペリエンスイベントがデータフェデレーション（顧客間のデータ共有）を通じて作成されたかどうかを示します。 |
| `advertising.clicks` | 広告の「アクション」をクリックします。 |
| `advertising.conversions` | パフォーマンス評価のイベントをトリガーする、顧客が実行する事前定義済みのアクション。 |
| `advertising.firstQuartiles` | デジタルビデオ広告は、通常の速度で再生時間の 25％ を再生しました。 |
| `advertising.impressions` | 視聴可能な顧客に対する広告のインプレッション。 |
| `advertising.midpoints` | デジタルビデオ広告は、通常の速度で再生時間の 50％を再生しました。 |
| `advertising.starts` | デジタルビデオ広告の再生が開始されました。 |
| `advertising.thirdQuartiles` | デジタルビデオ広告は、通常の速度で再生時間の 75%を再生しました。 |
| `web.webpagedetails.pageViews` | Webページが1つ以上の表示を受信しました。 |
| `web.webinteraction.linkClicks` | リンクが1回以上クリックされた。 |
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