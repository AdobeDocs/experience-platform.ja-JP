---
title: セッション詳細レポートのデータタイプ
description: セッションの詳細レポート エクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: 8bcaa0d8-2f85-4189-b0b5-8c72ecbb0660
source-git-commit: e4ee4accdb28dafda7e37625eb84062bb6e53644
workflow-type: tm+mt
source-wordcount: '1293'
ht-degree: 17%

---

# [!UICONTROL Session Details] レポート データ型

[!UICONTROL Session Details] レポートは、メディア再生セッションに関連するデータを追跡する標準のExperience Data Model （XDM） データ型です。
メディアレポートフィールドは、Adobe サービスが送信したメディア収集フィールドを分析するために使用されます。 このデータは、他の特定のユーザー指標とともに計算され、レポートされます。 このスキーマには、ユーザーの行動やコンテンツの消費パターンに関するインサイトを提供する、幅広いプロパティが含まれています。 [!UICONTROL Session Details] レポート データ型を使用して、再生イベント、広告インタラクション、進行状況マーカー、一時停止などの指標をログに記録して、ユーザーのエンゲージメントを取得します。

+++を選択して、セッション詳細レポートのデータタイプのダイアグラムを表示します。
![ セッション詳細レポートのデータタイプの図。](../images/data-types/session-details-reporting.png)
+++

>[!NOTE]
>
>各表示名には、オーディオおよびビデオのパラメーターに関する詳細情報へのリンクが含まれています。 リンクされたページには、Adobeによって収集されたビデオとデータ、実装値、ネットワークパラメーター、レポート、および重要な考慮事項の詳細が含まれています。

| 表示名 | プロパティ | データタイプ | 説明 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------|-----------|--------------------------------------------------------------------------------------------------|
| [[!UICONTROL 10% Progress Marker]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#ten-%25-progress-marker) | `hasProgress10` | ブール | 再生ヘッドが、ストリームの長さに基づいてメディアの 10％マーカーを通過したことを示します。マーカーは、逆方向に求める場合でも、1回だけカウントされます。 早送りのシークがあった場合は、スキップされたマーカーはカウントされません。  |
| [[!UICONTROL 25% Progress Marker]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#twenty-five-%25-progress-marker) | `hasProgress25` | ブール | 再生ヘッドが、ストリームの長さに基づいてメディアの 25%マーカーを通過したことを示します。マーカーは、逆方向に求める場合でも、1回だけカウントされます。 早送りのシークがあった場合は、スキップされたマーカーはカウントされません。  |
| [[!UICONTROL 50% Progress Marker]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#50%25-progress-marker) | `hasProgress50` | ブール | 再生ヘッドが、ストリームの長さに基づいてメディアの 50%マーカーを通過したことを示します。マーカーは、逆方向に求める場合でも、1回だけカウントされます。 早送りのシークがあった場合は、スキップされたマーカーはカウントされません。  |
| [[!UICONTROL 75% Progress Marker]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#seventy-five-%25-progress-marker) | `hasProgress75` | ブール | 再生ヘッドが、ストリームの長さに基づいてメディアの 75%マーカーを通過したことを示します。マーカーは、逆方向に求める場合でも、1回だけカウントされます。 早送りのシークがあった場合は、スキップされたマーカーはカウントされません。  |
| [[!UICONTROL 95% Progress Marker]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#ninety-five-%25-progress-marker) | `hasProgress95` | ブール | 再生ヘッドが、ストリームの長さに基づいてメディアの 95%マーカーを通過したことを示します。マーカーは、逆方向に求める場合でも、1回だけカウントされます。 早送りのシークがあった場合は、スキップされたマーカーはカウントされません。  |
| [[!UICONTROL Ad Count]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#ad-count) | `adCount` | 整数 | 再生中に開始された広告の数。 |
| [!UICONTROL Ad Load Type] | `adLoad` | 文字列 | 各顧客の内部表現によって定義され、読み込まれる広告のタイプ。 |
| [[!UICONTROL Album]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#album) | `album` | 文字列 | ミュージックレコーディングまたはビデオが属するアルバムの名前。 |
| [[!UICONTROL Artist]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#artist) | `artist` | 文字列 | ミュージックレコーディングまたはビデオを実行するアルバムアーティストまたはグループの名前。 |
| [[!UICONTROL Asset ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#asset-id) | `assetID` | 文字列 | [!UICONTROL Asset ID]は、TV シリーズのエピソード識別子、映画アセット識別子、ライブイベント識別子など、メディアアセットのコンテンツの一意の識別子です。 通常、これらのIDは、EIDR、TMS/Gracenote、Roviなどのメタデータ機関から取得されます。 このIDは、他の専有システムや社内システムから取得することもできます。 |
| [[!UICONTROL Author]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#author) | `author` | 文字列 | メディア作成者の名前。 |
| [[!UICONTROL Average Minute Audience]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#average-minute-audience) | `averageMinuteAudience` | Number：特定のメディア項目に費やした平均コンテンツ時間を表します。つまり、費やしたコンテンツの合計時間を、すべての再生セッションの長さで割ったものです。 |  |
| [[!UICONTROL Broadcast Content Type]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-type) | `contentType` | 文字列 | ストリーム配信の[!UICONTROL Broadcast Content Type]。 [!UICONTROL Stream Type]ごとに使用可能な値は次のとおりです。<br> オーディオ：「song」、「podcast」、「audiobook」、「radio」、<br> ビデオ：「VoD」、「Live」、「Linear」、「UGC」、および「DVoD」。<br>お客様は、このパラメーターにカスタム値を指定できます。 |
| [[!UICONTROL Broadcast Network]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#network) | `network` | 文字列 | ネットワーク/チャネル名。 |
| [[!UICONTROL Chapter Count]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#chapter-count) | `chapterCount` | 整数 | 再生中にチャプターの数が開始されました。 |
| [[!UICONTROL Content Channel]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-channel) | `channel` | 文字列 | [!UICONTROL Content Channel]は、コンテンツが再生された配信チャネルです。 |
| [[!UICONTROL Content Completes]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-complete) | `isCompleted` | ブール | [!UICONTROL Content Completes]は、タイムドメディアアセットが完了するまで監視されたかどうかを示します。 このイベントは、必ずしも視聴者がビデオ全体を視聴したことを意味するものではありません。視聴者は先にスキップした可能性があります。 |
| [!UICONTROL Content Delivery Network] | `cdn` | 文字列 | コンテンツの[!UICONTROL Content Delivery Network]が再生されました。 |
| [[!UICONTROL Content ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-id) | `name` | 文字列 | [!UICONTROL Content ID]は、コンテンツの一意のIDです。 ほかの業界やCMS IDにリンクするために使用できます。 |
| [[!UICONTROL Content Name]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-name-(variable)) | `friendlyName` | 文字列 | [!UICONTROL Content Name]は、コンテンツの「わかりやすい」（人間が読み取れる）名前です。 |
| [[!UICONTROL Content Player Name]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-player-name) | `playerName` | 文字列 | コンテンツプレーヤーの名前。 |
| [[!UICONTROL Content Starts]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-starts) | `isPlayed` | ブール | メディアの最初のフレームが消費されると、[!UICONTROL Content Starts]がtrueになります。 ユーザーが広告やバッファリングなどの実行中にドロップした場合、[!UICONTROL Content Starts] イベントは発生しません。 |
| [[!UICONTROL Content Time Spent]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-time-spent) | `timePlayed` | 整数 | [!UICONTROL Content Time Spent]は、メインコンテンツでPLAY タイプのすべてのイベントのイベント期間（秒単位）を合計します。 |
| [[!UICONTROL Creator Name]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#originator) | `originator` | 文字列 | コンテンツ作成者の名前。 |
| [[!UICONTROL Day Part]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#day-part) | `dayPart` | 文字列 | コンテンツがブロードキャストまたは再生された時刻を定義するプロパティ。 これは、顧客が必要に応じて設定した値を持つ可能性があります |
| [[!UICONTROL Episode Number]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#episode) | `episode` | 文字列 | エピソードの番号。 |
| [[!UICONTROL Estimated Streams]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#estimated-streams) | `estimatedStreams` | 数値 | 個々のコンテンツのビデオストリームまたはオーディオストリームの推定数。 |
| [[!UICONTROL Federated Data]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#federated-data) | `isFederated` | ブール | [!UICONTROL Federated Data]は、ヒットがフェデレーションされたときにtrueに設定されます（つまり、フェデレーションデータ共有の一部として、お客様が独自の実装ではなく受け取ります）。 |
| [[!UICONTROL Feed Type]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#media-feed-type) | `feed` | 文字列 | フィードのタイプ。EAST HDやSDなどの実際のフィード関連データを表すか、URLなどのフィードのソースを表すことができます。 |
| [[!UICONTROL First Air Date]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#first-air-date) | `firstAirDate` | 文字列 | コンテンツが最初にテレビで放映された日付。 任意の日付フォーマットを使用できますが、AdobeではYYYY-MM-DDをお勧めします。 |
| [[!UICONTROL First Digital Date]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#first-digital-date) | `firstDigitalDate` | 文字列 | デジタルチャネルやプラットフォームでコンテンツが最初に放映された日付。 任意の日付フォーマットを使用できますが、AdobeではYYYY-MM-DDをお勧めします。 |
| [[!UICONTROL Genre]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#genre) | `genre` | 文字列 | コンテンツプロデューサーによって定義されたコンテンツのタイプまたはグループ。 変数の実装では、値はコンマ区切りにする必要があります。 |
| [[!UICONTROL Media Authorized]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#authorized) | `authorized` | 文字列 | Adobe認証を介してユーザーが承認されているかどうかを確認します。 |
| [[!UICONTROL Media Content Length]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-length-(variable)) | `length` | 整数     はい | [!UICONTROL Media Content Length]には、クリップの長さ/ランタイムが含まれています。これは、消費されるコンテンツの最大長（またはデュレーション）です（秒単位）。 |
| [[!UICONTROL Media Downloaded Flag]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#media-downloaded-flag) | `isDownloaded` | ブール | ストリームは、ダウンロード後にデバイス上でローカルに再生されました。 |
| [[!UICONTROL Media Segment Views]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-segment-views) | `hasSegmentView` | ブール | [!UICONTROL Media Segment Views]は、1つ以上のフレーム（必ずしも最初のフレームではない）が表示されたことを示します。 |
| [[!UICONTROL Media Session ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#media-session-id) | `ID` | 文字列 | [!UICONTROL Media Session ID]は、個々の再生に固有のコンテンツストリームのインスタンスを識別します。<br><em> メモ：<em>`sessionId`は、`sessionStart`およびダウンロードされたすべてのイベントを除くすべてのイベントで送信されます。 |
| [[!UICONTROL Media Session Server Timeout]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#seconds-since-last-call) | `secondsSinceLastCall` | 数値[!UICONTROL Media Session Server Timeout]は、ユーザーの最後の既知のインタラクションからセッションが閉じられた瞬間までの間に渡された時間（秒単位）を示します。 |  |
| [[!UICONTROL Media Starts]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#media-starts) | `isViewed` | ブール | メディアの読み込みイベント。 これは、ビューアが再生ボタンを選択したときに発生します。 これは、プリロール広告、バッファリング、エラーなどが存在する場合でもカウントされます。 |
| [[!UICONTROL Media Time Spent]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#media-time-spent) | `totalTimePlayed` | 整数 | 広告の視聴に費やした時間を含む、特定の時間指定メディアアセットに対するユーザーの合計滞在時間を示します。 |
| [[!UICONTROL MVPD Identifier]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#mvpd) | `mvpd` | 文字列 | Adobe認証を介して提供された[!UICONTROL MVPD Identifier]。 |
| [[!UICONTROL Pause Events]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#pause-events) | `pauseCount` | 整数 | [!UICONTROL Pause Events]は、再生中に発生した中断期間の数をカウントします。 |
| [[!UICONTROL Pause Impacted Streams]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#paused-impacted-streams) | `hasPauseImpactedStreams` | Boolean 1つのメディアアイテムの再生中に1つ以上の一時停止が発生したかどうかを示します。 |  |
| [!UICONTROL Pccr] | `pccr` | ブール | [!UICONTROL Pccr]は、リダイレクトが発生したことを示します。 |
| [!UICONTROL Pev3] | `pev3` | 文字列 | [!UICONTROL Pev3]は、レポートに使用されるメディア ストリームのタイプです。 |
| [[!UICONTROL Publisher]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#publisher) | `publisher` | 文字列 | オーディオコンテンツパブリッシャーの名前。 |
| [[!UICONTROL Radio Station]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#station) | `station` | 文字列 | オーディオが再生されるラジオ局名。 |
| [[!UICONTROL Rating Value]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-rating) | `rating` | 文字列 | TV Parental Guidelinesで定義された評価。 |
| [[!UICONTROL Record Label]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#label) | `label` | 文字列 | レコードラベルの名前。 |
| [[!UICONTROL Resume]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-resumes) | `hasResume` | ブール | 30 分を超えるバッファー、一時停止または停止期間の後に再開された各再生を示します。 |
| [[!UICONTROL Season Number]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#season) | `season` | 文字列 | 番組が属する[!UICONTROL Season Number]です。 シーズンシリーズは、番組がシリーズの一部である場合にのみ必要です。 |
| [[!UICONTROL Series Name]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#show) | `show` | 文字列 | プログラム/シリーズ名。 プログラム名は、番組がシリーズの一部である場合にのみ必要です。 |
| [[!UICONTROL Show Type]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#show-type) | `showType` | 文字列 | コンテンツの種類（予告編や完全エピソードなど）。 |
| [[!UICONTROL Stream Format]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#stream-format) | `streamFormat` | 文字列 | ストリームの形式（HD、SD）。 |
| [[!UICONTROL Stream Type]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#stream-type) | `streamType` | 文字列 | メディアストリームのタイプ。 |
| [[!UICONTROL Total Pause Duration]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#total-pause-duration) | `pauseTime` | 整数 | [!UICONTROL Total Pause Duration]は、ユーザーが再生を一時停止した時間を秒単位で表します。 |
| [[!UICONTROL Unique Time Played]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#unique-time-played) | `uniqueTimePlayed` | 整数 | タイムドメディアアセットでユーザーが見たユニークな間隔の合計を表します。つまり、複数回再生された長さの再生間隔は1回のみカウントされます。 |
| [[!UICONTROL Version]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#sdk-version) | `appVersion` | 文字列 | プレイヤーが使用するSDK バージョン。 これは、プレイヤーにとって意味のある任意のカスタム値を持つことができます。 |
| [[!UICONTROL Video Segment]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-segment) | `segment` | 文字列 | 分単位で表示されたコンテンツの部分を表す間隔。 |

{style="table-layout:auto"}

<!-- 
Could not find details for :
Ad Load Type
Content Delivery Network
Pccr
Pev3
-->
