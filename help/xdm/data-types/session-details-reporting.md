---
title: セッションの詳細レポートのデータタイプ
description: セッション詳細レポートエクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: 8bcaa0d8-2f85-4189-b0b5-8c72ecbb0660
source-git-commit: 16cc811a545414021b8686ae303d6112bcf6cebb
workflow-type: tm+mt
source-wordcount: '1293'
ht-degree: 17%

---

# [!UICONTROL Session Details] レポートデータタイプ

[!UICONTROL Session Details] レポートは、メディア再生セッションに関連するデータを追跡する、標準のエクスペリエンスデータモデル（XDM）データタイプです。
メディアレポートフィールドは、Adobe サービスが送信したメディアコレクションフィールドを分析するためにユーザーが使用します。 このデータは、他の特定のユーザー指標と共に計算され、レポートされます。 このスキーマには、ユーザーの行動やコンテンツ消費パターンに関するインサイトを提供する幅広いプロパティが含まれています。 [!UICONTROL Session Details] レポートデータタイプを使用すると、再生イベント、広告インタラクション、進捗マーカー、一時停止、その他の指標をログに記録して、ユーザーエンゲージメントをキャプチャすることができます。

+++選択すると、セッションの詳細レポートデータタイプの図が表示されます。
![&#x200B; セッションの詳細レポートデータタイプの図。](../images/data-types/session-details-reporting.png)
+++

>[!NOTE]
>
>各表示名には、オーディオおよびビデオパラメーターに関する詳細情報へのリンクが含まれています。 リンク先のページには、Adobeで収集されたビデオ広告データ、実装値、ネットワークパラメーター、レポートおよび重要な検討事項の詳細が含まれています。

| 表示名 | プロパティ | データタイプ | 説明 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------|-----------|--------------------------------------------------------------------------------------------------|
| [[!UICONTROL 10% Progress Marker]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#ten-%25-progress-marker) | `hasProgress10` | ブール値 | 再生ヘッドが、ストリームの長さに基づいてメディアの 10％マーカーを通過したことを示します。後方に検索した場合でも、マーカーは 1 回だけカウントされます。 早送りのシークがあった場合は、スキップされたマーカーはカウントされません。  |
| [[!UICONTROL 25% Progress Marker]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#twenty-five-%25-progress-marker) | `hasProgress25` | ブール値 | 再生ヘッドが、ストリームの長さに基づいてメディアの 25%マーカーを通過したことを示します。後方を検索した場合でも、マーカーは 1 回だけカウントされます。 早送りのシークがあった場合は、スキップされたマーカーはカウントされません。  |
| [[!UICONTROL 50% Progress Marker]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#50%25-progress-marker) | `hasProgress50` | ブール値 | 再生ヘッドが、ストリームの長さに基づいてメディアの 50%マーカーを通過したことを示します。後方を検索した場合でも、マーカーは 1 回だけカウントされます。 早送りのシークがあった場合は、スキップされたマーカーはカウントされません。  |
| [[!UICONTROL 75% Progress Marker]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#seventy-five-%25-progress-marker) | `hasProgress75` | ブール値 | 再生ヘッドが、ストリームの長さに基づいてメディアの 75%マーカーを通過したことを示します。後方を検索した場合でも、マーカーは 1 回だけカウントされます。 早送りのシークがあった場合は、スキップされたマーカーはカウントされません。  |
| [[!UICONTROL 95% Progress Marker]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#ninety-five-%25-progress-marker) | `hasProgress95` | ブール値 | 再生ヘッドが、ストリームの長さに基づいてメディアの 95%マーカーを通過したことを示します。後方を検索した場合でも、マーカーは 1 回だけカウントされます。 早送りのシークがあった場合は、スキップされたマーカーはカウントされません。  |
| [[!UICONTROL Ad Count]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#ad-count) | `adCount` | 整数 | 再生中に開始された広告の数。 |
| [!UICONTROL Ad Load Type] | `adLoad` | 文字列 | 各顧客の内部表現によって定義され、読み込まれる広告のタイプ。 |
| [[!UICONTROL Album]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#album) | `album` | 文字列 | 音楽の録音またはビデオが属するアルバムの名前。 |
| [[!UICONTROL Artist]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#artist) | `artist` | 文字列 | 音楽の録音またはビデオを実行しているアルバムアーティストまたはグループの名前。 |
| [[!UICONTROL Asset ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#asset-id) | `assetID` | 文字列 | [!UICONTROL Asset ID] は、TV シリーズのエピソード識別子、映画アセット識別子、ライブイベント識別子など、メディアアセットのコンテンツの一意の識別子です。 通常、これらの ID は、EIDR、TMS/Gracenote、Rovi などのメタデータ認証機関から派生します。 これらの識別子は、他の独自システムや社内システムからも取得できます。 |
| [[!UICONTROL Author]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#author) | `author` | 文字列 | メディア作成者の名前。 |
| [[!UICONTROL Average Minute Audience]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#average-minute-audience) | `averageMinuteAudience` | 数値：特定のメディア項目に対して費やしたコンテンツの平均時間（つまり、コンテンツに費やした合計時間をすべての再生セッションの長さで割ったもの）を表します。 |  |
| [[!UICONTROL Broadcast Content Type]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-type) | `contentType` | 文字列 | ストリーム配信の [!UICONTROL Broadcast Content Type]。 [!UICONTROL Stream Type] ごとに使用できる値は次のとおりです。<br> オーディオ：「song」、「podcast」、「audiobook」、「radio」;<br> ビデオ：「VoD」、「Live」、「Linear」、「UGC」、「DVoD」。<br> お客様は、このパラメーターにカスタム値を指定できます。 |
| [[!UICONTROL Broadcast Network]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#network) | `network` | 文字列 | ネットワーク/チャネル名。 |
| [[!UICONTROL Chapter Count]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#chapter-count) | `chapterCount` | 整数 | 再生中に開始されたチャプターの数。 |
| [[!UICONTROL Content Channel]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-channel) | `channel` | 文字列 | [!UICONTROL Content Channel] は、コンテンツの再生元となった配信チャネルです。 |
| [[!UICONTROL Content Completes]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-complete) | `isCompleted` | ブール値 | タイムドメディアアセットが最後まで視聴されたかどうかを [!UICONTROL Content Completes] が示します。 このイベントは、視聴者がビデオ全体を視聴したことを必ずしも意味するものではなく、視聴者が先にスキップした可能性があります。 |
| [!UICONTROL Content Delivery Network] | `cdn` | 文字列 | 再生されるコンテンツの [!UICONTROL Content Delivery Network]。 |
| [[!UICONTROL Content ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-id) | `name` | 文字列 | [!UICONTROL Content ID] は、コンテンツの一意の ID です。 他の業界 ID やCMS ID へのリンクに使用できます。 |
| [[!UICONTROL Content Name]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-name-(variable)) | `friendlyName` | 文字列 | [!UICONTROL Content Name] は、コンテンツの「わかりやすい」（人間が読み取れる）名前です。 |
| [[!UICONTROL Content Player Name]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-player-name) | `playerName` | 文字列 | コンテンツプレイヤーの名前。 |
| [[!UICONTROL Content Starts]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-starts) | `isPlayed` | ブール値 | [!UICONTROL Content Starts] は、メディアの最初のフレームが消費されると true になります。 広告中やバッファリング中などにユーザーがドロップした場合、[!UICONTROL Content Starts] イベントは発生しません。 |
| [[!UICONTROL Content Time Spent]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-time-spent) | `timePlayed` | 整数 | [!UICONTROL Content Time Spent] は、メインコンテンツで再生タイプのすべてのイベントに対するイベント時間（秒単位）を合計します。 |
| [[!UICONTROL Creator Name]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#originator) | `originator` | 文字列 | コンテンツ作成者の名前。 |
| [[!UICONTROL Day Part]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#day-part) | `dayPart` | 文字列 | コンテンツが放送または再生された時間帯を定義するプロパティ。 これには、顧客が必要に応じて任意の値を設定できます |
| [[!UICONTROL Episode Number]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#episode) | `episode` | 文字列 | エピソードの数。 |
| [[!UICONTROL Estimated Streams]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#estimated-streams) | `estimatedStreams` | 数値 | コンテンツの個々の部分ごとのビデオまたはオーディオストリームの推定数。 |
| [[!UICONTROL Federated Data]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#federated-data) | `isFederated` | ブール値 | ヒットがフェデレーテッドされる（つまり、独自の実装ではなく、フェデレーティッドデータ共有の一部として顧客によって受信される）場合、[!UICONTROL Federated Data] は true に設定されます。 |
| [[!UICONTROL Feed Type]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#media-feed-type) | `feed` | 文字列 | フィードのタイプ。実際のフィード関連データ （EAST HD、SD など）を表すものと、フィードのソース （URL など）を表すものがあります。 |
| [[!UICONTROL First Air Date]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#first-air-date) | `firstAirDate` | 文字列 | コンテンツがテレビで最初に放送された日付。 任意の日付フォーマットを使用できますが、Adobeでは YYYY-MM-DD の形式を使用することをお勧めします。 |
| [[!UICONTROL First Digital Date]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#first-digital-date) | `firstDigitalDate` | 文字列 | コンテンツが任意のデジタルチャネルまたはプラットフォームで最初に放送された日付。 任意の日付フォーマットを使用できますが、Adobeでは YYYY-MM-DD の形式を使用することをお勧めします。 |
| [[!UICONTROL Genre]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#genre) | `genre` | 文字列 | コンテンツプロデューサーによって定義されたコンテンツのタイプまたはグループ。 値は、変数の実装でコンマで区切る必要があります。 |
| [[!UICONTROL Media Authorized]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#authorized) | `authorized` | 文字列 | Adobe認証によってユーザーが認証されているかどうかを確認します。 |
| [[!UICONTROL Media Content Length]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-length-(variable)) | `length` | 整数     はい | [!UICONTROL Media Content Length] にはクリップの長さ/ランタイムが含まれます。これは、使用されるコンテンツの最大長（またはデュレーション）です（秒単位）。 |
| [[!UICONTROL Media Downloaded Flag]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#media-downloaded-flag) | `isDownloaded` | ブール値 | ストリームは、ダウンロードされた後、デバイス上でローカルで再生されました。 |
| [[!UICONTROL Media Segment Views]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-segment-views) | `hasSegmentView` | ブール値 | [!UICONTROL Media Segment Views] は、必ずしも最初のフレームとは限らないが、少なくとも 1 つのフレームが表示されたことを示します。 |
| [[!UICONTROL Media Session ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#media-session-id) | `ID` | 文字列 | [!UICONTROL Media Session ID] は、個々の再生に固有のコンテンツストリームのインスタンスを識別する。<br><em> メモ：<em>`sessionId` は、`sessionStart` を除くすべてのイベントで送信されます。ただし、ダウンロードされたすべてのイベントでは送信されません。 |
| [[!UICONTROL Media Session Server Timeout]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#seconds-since-last-call) | `secondsSinceLastCall` | 数値 [!UICONTROL Media Session Server Timeout] は、ユーザーが最後に既知のインタラクションを行ってからセッションが終了するまでに経過した時間（秒）を示します。 |  |
| [[!UICONTROL Media Starts]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#media-starts) | `isViewed` | ブール値 | メディアの読み込みイベント。 この問題は、ビューアが再生ボタンを選択したときに発生します。 これは、プリロール広告、バッファリング、エラーなどが発生した場合でもカウントされます。 |
| [[!UICONTROL Media Time Spent]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#media-time-spent) | `totalTimePlayed` | 整数 | 広告の視聴に費やした時間を含む、特定の時間指定メディアアセットに対するユーザーの合計滞在時間を示します。 |
| [[!UICONTROL MVPD Identifier]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#mvpd) | `mvpd` | 文字列 | Adobe認証を介して提供された [!UICONTROL MVPD Identifier]。 |
| [[!UICONTROL Pause Events]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#pause-events) | `pauseCount` | 整数 | 再生中に発生した一時停止期間の数を [!UICONTROL Pause Events] カウントします。 |
| [[!UICONTROL Pause Impacted Streams]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#paused-impacted-streams) | `hasPauseImpactedStreams` | ブール値：1 つのメディア項目の再生中に一時停止が 1 つ以上発生したかどうかを示します。 |  |
| [!UICONTROL Pccr] | `pccr` | ブール値 | [!UICONTROL Pccr] は、リダイレクトが発生したことを示します。 |
| [!UICONTROL Pev3] | `pev3` | 文字列 | レポートに使用されるメディアストリームのタイプは [!UICONTROL Pev3] のとおりです。 |
| [[!UICONTROL Publisher]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#publisher) | `publisher` | 文字列 | オーディオコンテンツパブリッシャーの名前。 |
| [[!UICONTROL Radio Station]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#station) | `station` | 文字列 | オーディオが再生されるラジオ局名。 |
| [[!UICONTROL Rating Value]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-rating) | `rating` | 文字列 | テレビの保護者のガイドラインで定義されている評価。 |
| [[!UICONTROL Record Label]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#label) | `label` | 文字列 | レコードラベルの名前。 |
| [[!UICONTROL Resume]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-resumes) | `hasResume` | ブール値 | 30 分を超えるバッファー、一時停止または停止期間の後に再開された各再生を示します。 |
| [[!UICONTROL Season Number]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#season) | `season` | 文字列 | 番組が属する [!UICONTROL Season Number]。 シーズン シリーズは、番組がシリーズの一部である場合にのみ必要です。 |
| [[!UICONTROL Series Name]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#show) | `show` | 文字列 | プログラム/シリーズ名。 番組がシリーズの一部である場合にのみ、プログラム名が必要です。 |
| [[!UICONTROL Show Type]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#show-type) | `showType` | 文字列 | コンテンツのタイプ（例：予告編や完全なエピソード）。 |
| [[!UICONTROL Stream Format]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#stream-format) | `streamFormat` | 文字列 | ストリームの形式（HD、SD）。 |
| [[!UICONTROL Stream Type]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#stream-type) | `streamType` | 文字列 | メディアストリームのタイプ。 |
| [[!UICONTROL Total Pause Duration]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#total-pause-duration) | `pauseTime` | 整数 | [!UICONTROL Total Pause Duration] は、ユーザーが再生を一時停止した時間（秒）を表します。 |
| [[!UICONTROL Unique Time Played]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#unique-time-played) | `uniqueTimePlayed` | 整数 | タイムドメディアアセットでユーザーが見たユニークな間隔の合計を表します。つまり、複数回表示された再生間隔の長さは、1 回のみカウントされます。 |
| [[!UICONTROL Version]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#sdk-version) | `appVersion` | 文字列 | プレイヤーが使用するSDKのバージョン。 これには、プレーヤーにとって意味のあるカスタム値を含めることができます。 |
| [[!UICONTROL Video Segment]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-segment) | `segment` | 文字列 | 表示されたコンテンツの部分を表す間隔（分）。 |

{style="table-layout:auto"}

<!-- Could not find details for :
Ad Load Type
Content Delivery Network
Pccr
Pev3
-->
