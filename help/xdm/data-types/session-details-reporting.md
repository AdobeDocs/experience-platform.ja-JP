---
title: セッションの詳細レポートのデータタイプ
description: セッションの詳細レポートのエクスペリエンスデータモデル (XDM) データタイプについて説明します。
source-git-commit: a604dc8b541784ace8aedef42030e5bd8b646c28
workflow-type: tm+mt
source-wordcount: '1482'
ht-degree: 18%

---

# [!UICONTROL セッションの詳細] レポートのデータタイプ

[!UICONTROL セッションの詳細] レポートは、メディア再生セッションに関連するデータを追跡する、標準の Experience Data Model(XDM) データタイプです。
メディアレポートフィールドは、Adobe サービスが送信したメディア収集フィールドを分析するために使用します。 このデータは、他の特定のユーザー指標と共に、計算され、レポートされます。 スキーマには、ユーザーの行動とコンテンツの使用パターンに関するインサイトを提供する様々なプロパティが含まれています。 以下を使用します。 [!UICONTROL セッションの詳細] 再生イベント、広告インタラクション、プログレスマーカー、一時停止、その他の指標を記録することでユーザーエンゲージメントをキャプチャするためのレポートデータ型です。

+++「 」を選択して、「Session Details Reporting」データ型のダイアグラムを表示します。
![Session Details Reporting データ型の図です。](../images/data-types/session-details-reporting.png)
+++

>[!NOTE]
>
>各ディスプレイ名には、オーディオおよびビデオパラメーターに関する詳細情報へのリンクが含まれます。 リンクされたページには、Adobe、実装値、ネットワークパラメーター、レポートおよび重要な考慮事項によって収集されるビデオ広告データの詳細が含まれます。

| 表示名 | プロパティ | データタイプ | 説明 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------|-----------|--------------------------------------------------------------------------------------------------|
| [[!UICONTROL 10%プログレスマーカー]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#ten-%25-progress-marker) | `hasProgress10` | ブール値 | 再生ヘッドが、ストリームの長さに基づいてメディアの 10％マーカーを通過したことを示します。マーカーは、逆方向にシークした場合でも 1 回だけカウントされます。 早送りのシークがあった場合は、スキップされたマーカーはカウントされません。  |
| [[!UICONTROL 25%プログレスマーカー]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#twenty-five-%25-progress-marker) | `hasProgress25` | ブール値 | 再生ヘッドが、ストリームの長さに基づいてメディアの 25%マーカーを通過したことを示します。マーカーは、逆方向にシークした場合でも 1 回だけカウントされました。 早送りのシークがあった場合は、スキップされたマーカーはカウントされません。  |
| [[!UICONTROL 50%プログレスマーカー]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#50%25-progress-marker) | `hasProgress50` | ブール値 | 再生ヘッドが、ストリームの長さに基づいてメディアの 50%マーカーを通過したことを示します。マーカーは、逆方向にシークした場合でも 1 回だけカウントされました。 早送りのシークがあった場合は、スキップされたマーカーはカウントされません。  |
| [[!UICONTROL 75%プログレスマーカー]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#seventy-five-%25-progress-marker) | `hasProgress75` | ブール値 | 再生ヘッドが、ストリームの長さに基づいてメディアの 75%マーカーを通過したことを示します。マーカーは、逆方向にシークした場合でも 1 回だけカウントされました。 早送りのシークがあった場合は、スキップされたマーカーはカウントされません。  |
| [[!UICONTROL 95%プログレスマーカー]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#ninety-five-%25-progress-marker) | `hasProgress95` | ブール値 | 再生ヘッドが、ストリームの長さに基づいてメディアの 95%マーカーを通過したことを示します。マーカーは、逆方向にシークした場合でも 1 回だけカウントされました。 早送りのシークがあった場合は、スキップされたマーカーはカウントされません。  |
| [[!UICONTROL 広告数]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#ad-count) | `adCount` | 整数 | 再生中に開始された広告の数。 |
| [!UICONTROL 広告読み込みタイプ] | `adLoad` | 文字列 | 各顧客の内部表現によって定義され、読み込まれる広告のタイプ。 |
| [[!UICONTROL アルバム]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#album) | `album` | 文字列 | 音楽の録音またはビデオが属するアルバムの名前。 |
| [[!UICONTROL アーティスト]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#artist) | `artist` | 文字列 | 音楽の録音やビデオを実行するアルバムアーティストまたはグループの名前。 |
| [[!UICONTROL アセット ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#asset-id) | `assetID` | 文字列 | The [!UICONTROL アセット ID] は、TV シリーズのエピソードの識別子、ムービーアセットの識別子、ライブイベントの識別子など、メディアアセットのコンテンツの一意の識別子です。 この ID は通常、EIDR、TMS／Gracenote、Rovi などのメタデータを扱う機関から取得します。その他の独自のシステムや社内システムから取得することもできます。 |
| [[!UICONTROL 作成者]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#author) | `author` | 文字列 | メディア作成者の名前。 |
| [[!UICONTROL 分平均オーディエンス]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#average-minute-audience) | `averageMinuteAudience` | 数値：特定のメディアアイテムに費やしたコンテンツの平均時間、つまり、コンテンツに費やした合計時間を、すべての再生セッションの長さで割った値です。 |
| [[!UICONTROL 放送コンテンツタイプ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-type) | `contentType` | 文字列 | The [!UICONTROL 放送コンテンツタイプ] ストリーム配信の。 次の単位で使用可能な値： [!UICONTROL ストリームタイプ] 次を含む：<br>オーディオ： &quot;song&quot;、&quot;podcast&quot;、&quot;audiobook&quot;、および&quot;radio&quot;;<br>ビデオ：「VoD」、「Live」、「Linear」、「UGC」、「DVoD」。<br>お客様は、このパラメーターのカスタム値を指定できます。 |
| [[!UICONTROL 放送網]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#network) | `network` | 文字列 | ネットワーク/チャネル名。 |
| [[!UICONTROL チャプター数]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#chapter-count) | `chapterCount` | 整数 | 再生中に開始したチャプターの数。 |
| [[!UICONTROL コンテンツチャネル]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-channel) | `channel` | 文字列 | The [!UICONTROL コンテンツチャネル] は、コンテンツの再生元となった配信チャネルです。 |
| [[!UICONTROL コンテンツ完了]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-complete) | `isCompleted` | ブール値 | [!UICONTROL コンテンツ完了] タイムドメディアアセットが最後まで視聴されたかどうかを示します。 このイベントは、必ずしも視聴者がビデオ全体を視聴したとは限らず、視聴者が先にスキップした可能性もあります。 |
| [!UICONTROL コンテンツ配信ネットワーク] | `cdn` | 文字列 | The [!UICONTROL コンテンツ配信ネットワーク] 再生されたコンテンツの数を示します。 |
| [[!UICONTROL コンテンツ ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-id) | `name` | 文字列 | The [!UICONTROL コンテンツ ID] は、コンテンツの一意の識別子です。 他の業界や CMS ID へのリンクに使用できます。 |
| [[!UICONTROL コンテンツ名]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-name-(variable)) | `friendlyName` | 文字列 | The [!UICONTROL コンテンツ名] は、コンテンツのわかりやすい（人間が読み取れる）名前です。 |
| [[!UICONTROL コンテンツプレイヤー名]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-player-name) | `playerName` | 文字列 | コンテンツプレーヤーの名前。 |
| [[!UICONTROL コンテンツ開始]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-starts) | `isPlayed` | ブール値 | [!UICONTROL コンテンツ開始] は、メディアの最初のフレームが視聴されると true になります。 ユーザーが広告やバッファリングなどの最中にドロップした場合、 [!UICONTROL コンテンツ開始] イベント。 |
| [[!UICONTROL コンテンツ視聴時間]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-time-spent) | `timePlayed` | 整数 | [!UICONTROL コンテンツ視聴時間] メインコンテンツでの再生タイプの全イベントの合計時間（秒）。 |
| [[!UICONTROL 作成者名]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#originator) | `originator` | 文字列 | コンテンツ作成者の名前。 |
| [[!UICONTROL 日パート]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#day-part) | `dayPart` | 文字列 | コンテンツが放送または再生された時間帯を定義するプロパティ。 お客様の必要に応じて、任意の値を設定できます。 |
| [[!UICONTROL エピソード番号]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#episode) | `episode` | 文字列 | エピソードの番号。 |
| [[!UICONTROL 推定ストリーム]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#estimated-streams) | `estimatedStreams` | 数値 | コンテンツの個々の部分に対するビデオまたはオーディオストリームの数の推定値。 |
| [[!UICONTROL Federated Data]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#federated-data) | `isFederated` | ブール値 | [!UICONTROL Federated Data] ヒットがフェデレーションされた（つまり、独自の実装ではなくフェデレーテッドデータ共有の一部として顧客に受け取られた）場合、は true に設定されます。 |
| [[!UICONTROL フィードのタイプ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#media-feed-type) | `feed` | 文字列 | フィードのタイプ。実際のフィード関連データ（EAST HD や SD など）を表す場合と、フィードのソース（URL など）を表す場合があります。 |
| [[!UICONTROL 初回放送日]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#first-air-date) | `firstAirDate` | 文字列 | コンテンツがテレビで最初に放送された日付。 どのような形式でも問題ありませんが、Adobeでは YYYY-MM-DD の形式にすることをお勧めします。 |
| [[!UICONTROL 初回デジタル日]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#first-digital-date) | `firstDigitalDate` | 文字列 | コンテンツが何らかのデジタルチャネルまたはプラットフォームで最初に放送された日付。 どのような形式でも問題ありませんが、Adobeでは YYYY-MM-DD の形式にすることをお勧めします。 |
| [[!UICONTROL ジャンル]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#genre) | `genre` | 文字列 | コンテンツ作成者が定義したコンテンツのタイプまたは分類。 変数の実装では、値をコンマで区切る必要があります。 |
| [[!UICONTROL メディア認証済み]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#authorized) | `authorized` | 文字列 | ユーザーが認証を通じて認証されたかどうかをAdobeします。 |
| [[!UICONTROL メディアコンテンツの長さ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-length-(variable)) | `length` | 整数 | はい | The [!UICONTROL メディアコンテンツの長さ] クリップの長さ/ランタイムを含む — 視聴されるコンテンツの最大の長さまたは時間（秒単位）です。 |
| [[!UICONTROL メディアダウンロード済みフラグ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#media-downloaded-flag) | `isDownloaded` | ブール値 | ストリームは、ダウンロード後にデバイス上でローカルで再生されました。 |
| [[!UICONTROL メディアセグメント表示回数]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-segment-views) | `hasSegmentView` | ブール値 | [!UICONTROL メディアセグメント表示回数] 少なくとも 1 つのフレーム（最初のフレームとは限りません）が表示された日時を示します。 |
| [[!UICONTROL メディアセッション ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#media-session-id) | `ID` | 文字列 | The [!UICONTROL メディアセッション ID] は、個々の再生ごとのコンテンツストリームのインスタンスを識別します。<br><em>注意：<em>`sessionId` は、以下を除くすべてのイベントで送信されます。 `sessionStart` およびを含むすべてのダウンロードされたイベントに対して。 |
| [[!UICONTROL メディアセッションサーバーのタイムアウト]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#seconds-since-last-call) | `secondsSinceLastCall` | 数値： [!UICONTROL メディアセッションサーバーのタイムアウト] ユーザーの最後の既知のインタラクションからセッションが閉じられた瞬間までの経過時間を秒単位で示します。 |
| [[!UICONTROL メディア開始]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#media-starts) | `isViewed` | ブール値 | メディアの load イベント。 これは、ビューアが再生ボタンを選択したときに発生します。 これは、プリロール広告、バッファリング、エラーなどがある場合でもカウントされます。 |
| [[!UICONTROL メディア視聴時間]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#media-time-spent) | `totalTimePlayed` | 整数 | 広告の視聴に費やした時間を含む、特定の時間指定メディアアセットに対するユーザーの合計滞在時間を示します。 |
| [[!UICONTROL MVPD 識別子]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#mvpd) | `mvpd` | 文字列 | The [!UICONTROL MVPD 識別子] これはAdobe認証を介して提供された。 |
| [[!UICONTROL 一時停止イベント]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#pause-events) | `pauseCount` | 整数 | [!UICONTROL 一時停止イベント] 再生中に発生した一時停止期間の数をカウントします。 |
| [[!UICONTROL 影響を受けたストリームの一時停止]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#paused-impacted-streams) | `hasPauseImpactedStreams` | Boolean 単一のメディアアイテムの再生中に一時停止が 1 回以上発生したかどうかを示します。 |
| [!UICONTROL Pccr] | `pccr` | ブール値 | [!UICONTROL Pccr] リダイレクトが発生したことを示します。 |
| [!UICONTROL Pev3] | `pev3` | 文字列 | [!UICONTROL Pev3] は、レポートに使用されるメディアストリームのタイプです。 |
| [[!UICONTROL 投稿者]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#publisher) | `publisher` | 文字列 | オーディオコンテンツパブリッシャーの名前。 |
| [[!UICONTROL ラジオ局]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#station) | `station` | 文字列 | オーディオを再生するラジオ局の名前。 |
| [[!UICONTROL 評価値]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-rating) | `rating` | 文字列 | TV Parental Guidelines で定義されたレーティング。 |
| [[!UICONTROL レコードラベル]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#label) | `label` | 文字列 | レコードラベルの名前。 |
| [[!UICONTROL 再開]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-resumes) | `hasResume` | ブール値 | 30 分を超えるバッファー、一時停止または停止期間の後に再開された各再生を示します。 |
| [[!UICONTROL シーズン番号]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#season) | `season` | 文字列 | The [!UICONTROL シーズン番号] 番組が属する シーズンのシリーズは、番組がシリーズものの一部の場合のみ必要です。 |
| [[!UICONTROL シリーズ名]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#show) | `show` | 文字列 | プログラム/シリーズの名前。 番組名は、番組がシリーズものの一部の場合にのみ必要です。 |
| [[!UICONTROL 番組タイプ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#show-type) | `showType` | 文字列 | コンテンツのタイプ（トレーラー、完全なエピソードなど）。 |
| [[!UICONTROL ストリーム形式]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#stream-format) | `streamFormat` | 文字列 | ストリームの形式 (HD、SD)。 |
| [[!UICONTROL ストリームタイプ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#stream-type) | `streamType` | 文字列 | メディアストリームのタイプ。 |
| [[!UICONTROL 一時停止時間合計]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#total-pause-duration) | `pauseTime` | 整数 | [!UICONTROL 一時停止時間合計] ユーザーが再生を一時停止した時間（秒）を表します。 |
| [[!UICONTROL ユニーク再生時間]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#unique-time-played) | `uniqueTimePlayed` | 整数 | タイムドメディアアセットでユーザーが確認した一意の間隔の合計を表します。つまり、複数回表示された再生間隔の長さは 1 回のみカウントされます。 |
| [[!UICONTROL バージョン]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#sdk-version) | `appVersion` | 文字列 | プレーヤーで使用される SDK のバージョン。 プレーヤーに対して意味のある任意のカスタム値を持つことができます。 |
| [[!UICONTROL ビデオセグメント]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-segment) | `segment` | 文字列 | 視聴されたコンテンツの部分を示す間隔（分単位）。 |

{style="table-layout:auto"}

<!-- Could not find details for :
Ad Load Type
Content Delivery Network
Pccr
Pev3
-->
