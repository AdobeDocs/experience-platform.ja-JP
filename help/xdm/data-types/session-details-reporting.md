---
title: セッションの詳細レポートのデータタイプ
description: セッション詳細レポートエクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: 8bcaa0d8-2f85-4189-b0b5-8c72ecbb0660
source-git-commit: 799a384556b43bc844782d8b67416c7eea77fbf0
workflow-type: tm+mt
source-wordcount: '1482'
ht-degree: 21%

---

# [!UICONTROL  セッションの詳細 ] レポートデータタイプ

[!UICONTROL  セッションの詳細 ] レポートは、メディア再生セッションに関連するデータを追跡する、標準のエクスペリエンスデータモデル（XDM）データタイプです。
メディアレポートフィールドは、Adobe サービスが送信したメディアコレクションフィールドを分析するためにユーザーが使用します。 このデータは、他の特定のユーザー指標と共に計算され、レポートされます。 このスキーマには、ユーザーの行動やコンテンツ消費パターンに関するインサイトを提供する幅広いプロパティが含まれています。 [!UICONTROL  セッションの詳細 ] レポートデータタイプを使用すると、再生イベント、広告インタラクション、進捗マーカー、一時停止、その他の指標をログに記録して、ユーザーエンゲージメントをキャプチャできます。

+++選択すると、セッションの詳細レポートのデータタイプを示す図が表示されます。
![ セッションの詳細レポートデータタイプの図。](../images/data-types/session-details-reporting.png)
+++

>[!NOTE]
>
>各表示名には、オーディオおよびビデオパラメーターに関する詳細情報へのリンクが含まれています。 リンク先のページには、Adobeで収集された動画広告データの詳細、実装値、ネットワークパラメーター、レポート、重要な検討事項が含まれています。

| 表示名 | プロパティ | データタイプ | 説明 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------|-----------|--------------------------------------------------------------------------------------------------|
| [[!UICONTROL 10% プログレスマーカー ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#ten-%25-progress-marker) | `hasProgress10` | ブール値 | 再生ヘッドが、ストリームの長さに基づいてメディアの 10％マーカーを通過したことを示します。後方に検索した場合でも、マーカーは 1 回だけカウントされます。 早送りのシークがあった場合は、スキップされたマーカーはカウントされません。  |
| [[!UICONTROL 25% プログレスマーカー ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#twenty-five-%25-progress-marker) | `hasProgress25` | ブール値 | 再生ヘッドが、ストリームの長さに基づいてメディアの 25%マーカーを通過したことを示します。後方を検索した場合でも、マーカーは 1 回だけカウントされます。 早送りのシークがあった場合は、スキップされたマーカーはカウントされません。  |
| [[!UICONTROL 50% プログレスマーカー ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#50%25-progress-marker) | `hasProgress50` | ブール値 | 再生ヘッドが、ストリームの長さに基づいてメディアの 50%マーカーを通過したことを示します。後方を検索した場合でも、マーカーは 1 回だけカウントされます。 早送りのシークがあった場合は、スキップされたマーカーはカウントされません。  |
| [[!UICONTROL 75% プログレスマーカー ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#seventy-five-%25-progress-marker) | `hasProgress75` | ブール値 | 再生ヘッドが、ストリームの長さに基づいてメディアの 75%マーカーを通過したことを示します。後方を検索した場合でも、マーカーは 1 回だけカウントされます。 早送りのシークがあった場合は、スキップされたマーカーはカウントされません。  |
| [[!UICONTROL 95% プログレスマーカー ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#ninety-five-%25-progress-marker) | `hasProgress95` | ブール値 | 再生ヘッドが、ストリームの長さに基づいてメディアの 95%マーカーを通過したことを示します。後方を検索した場合でも、マーカーは 1 回だけカウントされます。 早送りのシークがあった場合は、スキップされたマーカーはカウントされません。  |
| [[!UICONTROL  広告数 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#ad-count) | `adCount` | 整数 | 再生中に開始された広告の数。 |
| [!UICONTROL  広告読み込みのタイプ ] | `adLoad` | 文字列 | 各顧客の内部表現によって定義され、読み込まれる広告のタイプ。 |
| [[!UICONTROL  アルバム ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#album) | `album` | 文字列 | 音楽の録音やビデオが含まれるアルバムの名前。 |
| [[!UICONTROL  アーティスト ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#artist) | `artist` | 文字列 | 音楽の録音やビデオで演奏しているアルバムアーティストやグループの名前。 |
| [[!UICONTROL  アセット ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#asset-id) | `assetID` | 文字列 | [!UICONTROL  アセット ID] は、TV シリーズのエピソード識別子、映画アセット識別子、ライブイベント識別子など、メディアアセットのコンテンツの一意の識別子です。 通常、これらの ID は、EIDR、TMS/Gracenote、Rovi などのメタデータ認証機関から派生します。 これらの識別子は、他の独自システムや社内システムからも取得できます。 |
| [[!UICONTROL  作成者 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#author) | `author` | 文字列 | メディア作成者の名前。 |
| [[!UICONTROL  分平均オーディエンス ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#average-minute-audience) | `averageMinuteAudience` | 数値：特定のメディア項目に対して費やしたコンテンツの平均時間（つまり、コンテンツに費やした合計時間をすべての再生セッションの長さで割ったもの）を表します。 |
| [[!UICONTROL  放送コンテンツの種類 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-type) | `contentType` | 文字列 | ストリーム配信の [!UICONTROL  ブロードキャストコンテンツタイプ ]。 [!UICONTROL  ストリームタイプ ] ごとに使用できる値は次のとおりです。<br> オーディオ：&quot;song&quot;、&quot;podcast&quot;、&quot;audiobook&quot;、および&quot;radio&quot;;<br> ビデオ：&quot;VoD&quot;、&quot;Live&quot;、&quot;Linear&quot;、&quot;UGC&quot;、および&quot;DVoD&quot;。<br> お客様は、このパラメーターにカスタム値を指定できます。 |
| [[!UICONTROL  放送ネットワーク ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#network) | `network` | 文字列 | ネットワーク/チャネル名。 |
| [[!UICONTROL  章数 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#chapter-count) | `chapterCount` | 整数 | 再生中に開始されたチャプターの数。 |
| [[!UICONTROL  コンテンツチャネル ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-channel) | `channel` | 文字列 | [!UICONTROL  コンテンツチャネル ] は、コンテンツの再生元となる配信チャネルです。 |
| [[!UICONTROL  コンテンツ完了 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-complete) | `isCompleted` | ブール値 | [!UICONTROL  コンテンツ完了 ] タイムドメディアアセットが最後まで視聴されたかどうかを示します。 このイベントは、視聴者がビデオ全体を視聴したことを必ずしも意味するものではなく、視聴者が先にスキップした可能性があります。 |
| [!UICONTROL  コンテンツ配信ネットワーク ] | `cdn` | 文字列 | 再生されるコンテンツの [!UICONTROL  コンテンツ配信ネットワーク ]。 |
| [[!UICONTROL  コンテンツ ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-id) | `name` | 文字列 | [!UICONTROL  コンテンツ ID] は、コンテンツの一意の識別子です。 他の業界や CMS ID へのリンクに使用できます。 |
| [[!UICONTROL  コンテンツ名 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-name-(variable)) | `friendlyName` | 文字列 | [!UICONTROL  コンテンツ名 ] は、コンテンツの「わかりやすい」（人間が読み取れる）名前です。 |
| [[!UICONTROL  コンテンツプレーヤー名 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-player-name) | `playerName` | 文字列 | コンテンツプレイヤーの名前。 |
| [[!UICONTROL  コンテンツ開始 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-starts) | `isPlayed` | ブール値 | [!UICONTROL  コンテンツ開始 ] は、メディアの最初のフレームが消費されると true になります。 広告中やバッファー中などにユーザーが削除された場合、[!UICONTROL  コンテンツ開始 ] イベントは発生しません。 |
| [[!UICONTROL  コンテンツに費やした時間 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-time-spent) | `timePlayed` | 整数 | [!UICONTROL  コンテンツ滞在時間 ] メインコンテンツの PLAY タイプのすべてのイベントに対するイベント期間（秒単位）を合計します。 |
| [[!UICONTROL  作成者名 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#originator) | `originator` | 文字列 | コンテンツ作成者の名前。 |
| [[!UICONTROL  日分割 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#day-part) | `dayPart` | 文字列 | コンテンツが放送または再生された時間帯を定義するプロパティ。 これには、顧客が必要に応じて任意の値を設定できます |
| [[!UICONTROL  エピソード番号 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#episode) | `episode` | 文字列 | エピソードの数。 |
| [[!UICONTROL  推定ストリーム ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#estimated-streams) | `estimatedStreams` | 数値 | コンテンツの個々の部分ごとのビデオまたはオーディオストリームの推定数。 |
| [[!UICONTROL  連合データ ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#federated-data) | `isFederated` | ブール値 | [!UICONTROL Federated Data] は、ヒットがフェデレーションされた（つまり、顧客が独自の実装ではなく、フェデレーティッドデータ共有の一部として受信した）場合に、true に設定されます。 |
| [[!UICONTROL  フィードの種類 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#media-feed-type) | `feed` | 文字列 | フィードのタイプ。実際のフィード関連データ （EAST HD、SD など）を表すものと、フィードのソース （URL など）を表すものがあります。 |
| [[!UICONTROL  初回放送日 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#first-air-date) | `firstAirDate` | 文字列 | コンテンツがテレビで最初に放送された日付。 任意の日付フォーマットを使用できますが、Adobeでは YYYY-MM-DD の形式を使用することをお勧めします。 |
| [[!UICONTROL  初回デジタル日 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#first-digital-date) | `firstDigitalDate` | 文字列 | コンテンツが任意のデジタルチャネルまたはプラットフォームで最初に放送された日付。 任意の日付フォーマットを使用できますが、Adobeでは YYYY-MM-DD の形式を使用することをお勧めします。 |
| [[!UICONTROL  ジャンル ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#genre) | `genre` | 文字列 | コンテンツプロデューサーによって定義されたコンテンツのタイプまたはグループ。 値は、変数の実装でコンマで区切る必要があります。 |
| [[!UICONTROL  メディア承認済み ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#authorized) | `authorized` | 文字列 | ユーザーがAdobe認証によって認証されているかどうかを確認します。 |
| [[!UICONTROL  メディアコンテンツの長さ ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-length-(variable)) | `length` | 整数 | はい | [!UICONTROL  メディアコンテンツ長 ] には、クリップの長さ/ランタイムが含まれます。これは、消費されるコンテンツの最大長（またはデュレーション）です（秒単位）。 |
| [[!UICONTROL  メディアダウンロード済みフラグ ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#media-downloaded-flag) | `isDownloaded` | ブール値 | ストリームは、ダウンロードされた後、デバイス上でローカルで再生されました。 |
| [[!UICONTROL  メディアセグメント表示 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-segment-views) | `hasSegmentView` | ブール値 | [!UICONTROL  メディアセグメント表示 ] は、必ずしも最初のフレームとは限らない少なくとも 1 つのフレームが表示されたことを示します。 |
| [[!UICONTROL  メディアセッション ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#media-session-id) | `ID` | 文字列 | [!UICONTROL  メディアセッション ID] は、個々の再生に固有のコンテンツストリームのインスタンスを識別します。<br><em> メモ：<em>`sessionId` は、`sessionStart` を除くすべてのイベントで送信されます。ただし、ダウンロードされたすべてのイベントでは送信されません。 |
| [[!UICONTROL  メディアセッションサーバータイムアウト ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#seconds-since-last-call) | `secondsSinceLastCall` | 数値 [!UICONTROL Media Session Server Timeout] は、ユーザーが最後に既知のインタラクションを行ってからセッションが終了するまでに経過した時間（秒単位）を示します。 |
| [[!UICONTROL  メディア開始 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#media-starts) | `isViewed` | ブール値 | メディアの読み込みイベント。 この問題は、ビューアが再生ボタンを選択したときに発生します。 これは、プリロール広告、バッファリング、エラーなどが発生した場合でもカウントされます。 |
| [[!UICONTROL  メディアに費やした時間 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#media-time-spent) | `totalTimePlayed` | 整数 | 広告の視聴に費やした時間を含む、特定の時間指定メディアアセットに対するユーザーの合計滞在時間を示します。 |
| [[!UICONTROL MVPD 識別子 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#mvpd) | `mvpd` | 文字列 | Adobe認証によって提供された [!UICONTROL MVPD 識別子 ]。 |
| [[!UICONTROL  一時停止イベント ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#pause-events) | `pauseCount` | 整数 | [!UICONTROL  一時停止イベント ] 再生中に発生した一時停止期間の数をカウントします。 |
| [[!UICONTROL  影響を受けたストリームの一時停止 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#paused-impacted-streams) | `hasPauseImpactedStreams` | ブール値：1 つのメディア項目の再生中に一時停止が 1 つ以上発生したかどうかを示します。 |
| [!UICONTROL Pccr] | `pccr` | ブール値 | [!UICONTROL Pccr] は、リダイレクトが発生したことを示します。 |
| [!UICONTROL Pev3] | `pev3` | 文字列 | [!UICONTROL Pev3] は、レポートに使用されるメディアストリームのタイプです。 |
| [[!UICONTROL  発行者 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#publisher) | `publisher` | 文字列 | オーディオコンテンツパブリッシャーの名前。 |
| [[!UICONTROL  無線局 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#station) | `station` | 文字列 | オーディオが再生されるラジオ局名。 |
| [[!UICONTROL  評価値 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-rating) | `rating` | 文字列 | テレビの保護者のガイドラインで定義されている評価。 |
| [[!UICONTROL  レコードラベル ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#label) | `label` | 文字列 | レコードラベルの名前。 |
| [[!UICONTROL  再開 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-resumes) | `hasResume` | ブール値 | 30 分を超えるバッファー、一時停止または停止期間の後に再開された各再生を示します。 |
| [[!UICONTROL  シーズン番号 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#season) | `season` | 文字列 | 番組が属する [!UICONTROL  シーズン番号 ]。 シーズン シリーズは、番組がシリーズの一部である場合にのみ必要です。 |
| [[!UICONTROL  シリーズ名 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#show) | `show` | 文字列 | プログラム/シリーズ名。 番組がシリーズの一部である場合にのみ、プログラム名が必要です。 |
| [[!UICONTROL  表示タイプ ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#show-type) | `showType` | 文字列 | コンテンツのタイプ（例：予告編や完全なエピソード）。 |
| [[!UICONTROL  ストリーム形式 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#stream-format) | `streamFormat` | 文字列 | ストリームの形式（HD、SD）。 |
| [[!UICONTROL  ストリームの種類 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#stream-type) | `streamType` | 文字列 | メディアストリームのタイプ。 |
| [[!UICONTROL  合計一時停止時間 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#total-pause-duration) | `pauseTime` | 整数 | [!UICONTROL  合計一時停止時間 ] は、ユーザーによって再生が一時停止された時間（秒）を表します。 |
| [[!UICONTROL  ユニーク再生時間 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#unique-time-played) | `uniqueTimePlayed` | 整数 | タイムドメディアアセットでユーザーが見たユニークな間隔の合計を表します。つまり、複数回表示された再生間隔の長さは、1 回のみカウントされます。 |
| [[!UICONTROL  バージョン ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#sdk-version) | `appVersion` | 文字列 | プレーヤーが使用する SDK のバージョン。 これには、プレーヤーにとって意味のあるカスタム値を含めることができます。 |
| [[!UICONTROL  ビデオセグメント ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-segment) | `segment` | 文字列 | 表示されたコンテンツの部分を表す間隔 (分)。 |

{style="table-layout:auto"}

<!-- Could not find details for :
Ad Load Type
Content Delivery Network
Pccr
Pev3
-->
