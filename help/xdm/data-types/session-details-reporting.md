---
title: セッションの詳細レポートのデータタイプ
description: セッション詳細レポートエクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: 8bcaa0d8-2f85-4189-b0b5-8c72ecbb0660
source-git-commit: 799a384556b43bc844782d8b67416c7eea77fbf0
workflow-type: tm+mt
source-wordcount: '1482'
ht-degree: 21%

---

# [!UICONTROL &#x200B; セッションの詳細 &#x200B;] レポートデータタイプ

[!UICONTROL &#x200B; セッションの詳細 &#x200B;] レポートは、メディア再生セッションに関連するデータを追跡する、標準のエクスペリエンスデータモデル（XDM）データタイプです。
メディアレポートフィールドは、Adobe サービスが送信したメディアコレクションフィールドを分析するためにユーザーが使用します。 このデータは、他の特定のユーザー指標と共に計算され、レポートされます。 このスキーマには、ユーザーの行動やコンテンツ消費パターンに関するインサイトを提供する幅広いプロパティが含まれています。 [!UICONTROL &#x200B; セッションの詳細 &#x200B;] レポートデータタイプを使用すると、再生イベント、広告インタラクション、進捗マーカー、一時停止、その他の指標をログに記録して、ユーザーエンゲージメントをキャプチャできます。

+++選択すると、セッションの詳細レポートのデータタイプを示す図が表示されます。
![&#x200B; セッションの詳細レポートデータタイプの図。](../images/data-types/session-details-reporting.png)
+++

>[!NOTE]
>
>各表示名には、オーディオおよびビデオパラメーターに関する詳細情報へのリンクが含まれています。 リンク先のページには、Adobeで収集された動画広告データの詳細、実装値、ネットワークパラメーター、レポート、重要な検討事項が含まれています。

| 表示名 | プロパティ | データタイプ | 説明 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------|-----------|--------------------------------------------------------------------------------------------------|
| [[!UICONTROL 10% プログレスマーカー &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#ten-%25-progress-marker) | `hasProgress10` | ブール値 | 再生ヘッドが、ストリームの長さに基づいてメディアの 10％マーカーを通過したことを示します。後方に検索した場合でも、マーカーは 1 回だけカウントされます。 早送りのシークがあった場合は、スキップされたマーカーはカウントされません。  |
| [[!UICONTROL 25% プログレスマーカー &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#twenty-five-%25-progress-marker) | `hasProgress25` | ブール値 | 再生ヘッドが、ストリームの長さに基づいてメディアの 25%マーカーを通過したことを示します。後方を検索した場合でも、マーカーは 1 回だけカウントされます。 早送りのシークがあった場合は、スキップされたマーカーはカウントされません。  |
| [[!UICONTROL 50% プログレスマーカー &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#50%25-progress-marker) | `hasProgress50` | ブール値 | 再生ヘッドが、ストリームの長さに基づいてメディアの 50%マーカーを通過したことを示します。後方を検索した場合でも、マーカーは 1 回だけカウントされます。 早送りのシークがあった場合は、スキップされたマーカーはカウントされません。  |
| [[!UICONTROL 75% プログレスマーカー &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#seventy-five-%25-progress-marker) | `hasProgress75` | ブール値 | 再生ヘッドが、ストリームの長さに基づいてメディアの 75%マーカーを通過したことを示します。後方を検索した場合でも、マーカーは 1 回だけカウントされます。 早送りのシークがあった場合は、スキップされたマーカーはカウントされません。  |
| [[!UICONTROL 95% プログレスマーカー &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#ninety-five-%25-progress-marker) | `hasProgress95` | ブール値 | 再生ヘッドが、ストリームの長さに基づいてメディアの 95%マーカーを通過したことを示します。後方を検索した場合でも、マーカーは 1 回だけカウントされます。 早送りのシークがあった場合は、スキップされたマーカーはカウントされません。  |
| [[!UICONTROL &#x200B; 広告数 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#ad-count) | `adCount` | 整数 | 再生中に開始された広告の数。 |
| [!UICONTROL &#x200B; 広告読み込みのタイプ &#x200B;] | `adLoad` | 文字列 | 各顧客の内部表現によって定義され、読み込まれる広告のタイプ。 |
| [[!UICONTROL &#x200B; アルバム &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#album) | `album` | 文字列 | 音楽の録音やビデオが含まれるアルバムの名前。 |
| [[!UICONTROL &#x200B; アーティスト &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#artist) | `artist` | 文字列 | 音楽の録音やビデオで演奏しているアルバムアーティストやグループの名前。 |
| [[!UICONTROL &#x200B; アセット ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#asset-id) | `assetID` | 文字列 | [!UICONTROL &#x200B; アセット ID] は、TV シリーズのエピソード識別子、映画アセット識別子、ライブイベント識別子など、メディアアセットのコンテンツの一意の識別子です。 通常、これらの ID は、EIDR、TMS/Gracenote、Rovi などのメタデータ認証機関から派生します。 これらの識別子は、他の独自システムや社内システムからも取得できます。 |
| [[!UICONTROL &#x200B; 作成者 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#author) | `author` | 文字列 | メディア作成者の名前。 |
| [[!UICONTROL &#x200B; 分平均オーディエンス &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#average-minute-audience) | `averageMinuteAudience` | 数値：特定のメディア項目に対して費やしたコンテンツの平均時間（つまり、コンテンツに費やした合計時間をすべての再生セッションの長さで割ったもの）を表します。 |
| [[!UICONTROL &#x200B; 放送コンテンツの種類 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#content-type) | `contentType` | 文字列 | ストリーム配信の [!UICONTROL &#x200B; ブロードキャストコンテンツタイプ &#x200B;]。 [!UICONTROL &#x200B; ストリームタイプ &#x200B;] ごとに使用できる値は次のとおりです。<br> オーディオ：&quot;song&quot;、&quot;podcast&quot;、&quot;audiobook&quot;、および&quot;radio&quot;;<br> ビデオ：&quot;VoD&quot;、&quot;Live&quot;、&quot;Linear&quot;、&quot;UGC&quot;、および&quot;DVoD&quot;。<br> お客様は、このパラメーターにカスタム値を指定できます。 |
| [[!UICONTROL &#x200B; 放送ネットワーク &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#network) | `network` | 文字列 | ネットワーク/チャネル名。 |
| [[!UICONTROL &#x200B; 章数 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#chapter-count) | `chapterCount` | 整数 | 再生中に開始されたチャプターの数。 |
| [[!UICONTROL &#x200B; コンテンツチャネル &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#content-channel) | `channel` | 文字列 | [!UICONTROL &#x200B; コンテンツチャネル &#x200B;] は、コンテンツの再生元となる配信チャネルです。 |
| [[!UICONTROL &#x200B; コンテンツ完了 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#content-complete) | `isCompleted` | ブール値 | [!UICONTROL &#x200B; コンテンツ完了 &#x200B;] タイムドメディアアセットが最後まで視聴されたかどうかを示します。 このイベントは、視聴者がビデオ全体を視聴したことを必ずしも意味するものではなく、視聴者が先にスキップした可能性があります。 |
| [!UICONTROL &#x200B; コンテンツ配信ネットワーク &#x200B;] | `cdn` | 文字列 | 再生されるコンテンツの [!UICONTROL &#x200B; コンテンツ配信ネットワーク &#x200B;]。 |
| [[!UICONTROL &#x200B; コンテンツ ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#content-id) | `name` | 文字列 | [!UICONTROL &#x200B; コンテンツ ID] は、コンテンツの一意の識別子です。 他の業界や CMS ID へのリンクに使用できます。 |
| [[!UICONTROL &#x200B; コンテンツ名 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#content-name-(variable)) | `friendlyName` | 文字列 | [!UICONTROL &#x200B; コンテンツ名 &#x200B;] は、コンテンツの「わかりやすい」（人間が読み取れる）名前です。 |
| [[!UICONTROL &#x200B; コンテンツプレーヤー名 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#content-player-name) | `playerName` | 文字列 | コンテンツプレイヤーの名前。 |
| [[!UICONTROL &#x200B; コンテンツ開始 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#content-starts) | `isPlayed` | ブール値 | [!UICONTROL &#x200B; コンテンツ開始 &#x200B;] は、メディアの最初のフレームが消費されると true になります。 広告中やバッファー中などにユーザーが削除された場合、[!UICONTROL &#x200B; コンテンツ開始 &#x200B;] イベントは発生しません。 |
| [[!UICONTROL &#x200B; コンテンツに費やした時間 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#content-time-spent) | `timePlayed` | 整数 | [!UICONTROL &#x200B; コンテンツ滞在時間 &#x200B;] メインコンテンツの PLAY タイプのすべてのイベントに対するイベント期間（秒単位）を合計します。 |
| [[!UICONTROL &#x200B; 作成者名 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#originator) | `originator` | 文字列 | コンテンツ作成者の名前。 |
| [[!UICONTROL &#x200B; 日分割 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#day-part) | `dayPart` | 文字列 | コンテンツが放送または再生された時間帯を定義するプロパティ。 これには、顧客が必要に応じて任意の値を設定できます |
| [[!UICONTROL &#x200B; エピソード番号 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#episode) | `episode` | 文字列 | エピソードの数。 |
| [[!UICONTROL &#x200B; 推定ストリーム &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#estimated-streams) | `estimatedStreams` | 数値 | コンテンツの個々の部分ごとのビデオまたはオーディオストリームの推定数。 |
| [[!UICONTROL &#x200B; 連合データ &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#federated-data) | `isFederated` | ブール値 | [!UICONTROL Federated Data] は、ヒットがフェデレーションされた（つまり、顧客が独自の実装ではなく、フェデレーティッドデータ共有の一部として受信した）場合に、true に設定されます。 |
| [[!UICONTROL &#x200B; フィードの種類 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#media-feed-type) | `feed` | 文字列 | フィードのタイプ。実際のフィード関連データ （EAST HD、SD など）を表すものと、フィードのソース （URL など）を表すものがあります。 |
| [[!UICONTROL &#x200B; 初回放送日 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#first-air-date) | `firstAirDate` | 文字列 | コンテンツがテレビで最初に放送された日付。 任意の日付フォーマットを使用できますが、Adobeでは YYYY-MM-DD の形式を使用することをお勧めします。 |
| [[!UICONTROL &#x200B; 初回デジタル日 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#first-digital-date) | `firstDigitalDate` | 文字列 | コンテンツが任意のデジタルチャネルまたはプラットフォームで最初に放送された日付。 任意の日付フォーマットを使用できますが、Adobeでは YYYY-MM-DD の形式を使用することをお勧めします。 |
| [[!UICONTROL &#x200B; ジャンル &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#genre) | `genre` | 文字列 | コンテンツプロデューサーによって定義されたコンテンツのタイプまたはグループ。 値は、変数の実装でコンマで区切る必要があります。 |
| [[!UICONTROL &#x200B; メディア承認済み &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#authorized) | `authorized` | 文字列 | ユーザーがAdobe認証によって認証されているかどうかを確認します。 |
| [[!UICONTROL &#x200B; メディアコンテンツの長さ &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#content-length-(variable)) | `length` | 整数 | はい | [!UICONTROL &#x200B; メディアコンテンツ長 &#x200B;] には、クリップの長さ/ランタイムが含まれます。これは、消費されるコンテンツの最大長（またはデュレーション）です（秒単位）。 |
| [[!UICONTROL &#x200B; メディアダウンロード済みフラグ &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#media-downloaded-flag) | `isDownloaded` | ブール値 | ストリームは、ダウンロードされた後、デバイス上でローカルで再生されました。 |
| [[!UICONTROL &#x200B; メディアセグメント表示 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#content-segment-views) | `hasSegmentView` | ブール値 | [!UICONTROL &#x200B; メディアセグメント表示 &#x200B;] は、必ずしも最初のフレームとは限らない少なくとも 1 つのフレームが表示されたことを示します。 |
| [[!UICONTROL &#x200B; メディアセッション ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#media-session-id) | `ID` | 文字列 | [!UICONTROL &#x200B; メディアセッション ID] は、個々の再生に固有のコンテンツストリームのインスタンスを識別します。<br><em> メモ：<em>`sessionId` は、`sessionStart` を除くすべてのイベントで送信されます。ただし、ダウンロードされたすべてのイベントでは送信されません。 |
| [[!UICONTROL &#x200B; メディアセッションサーバータイムアウト &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#seconds-since-last-call) | `secondsSinceLastCall` | 数値 [!UICONTROL Media Session Server Timeout] は、ユーザーが最後に既知のインタラクションを行ってからセッションが終了するまでに経過した時間（秒単位）を示します。 |
| [[!UICONTROL &#x200B; メディア開始 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#media-starts) | `isViewed` | ブール値 | メディアの読み込みイベント。 この問題は、ビューアが再生ボタンを選択したときに発生します。 これは、プリロール広告、バッファリング、エラーなどが発生した場合でもカウントされます。 |
| [[!UICONTROL &#x200B; メディアに費やした時間 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#media-time-spent) | `totalTimePlayed` | 整数 | 広告の視聴に費やした時間を含む、特定の時間指定メディアアセットに対するユーザーの合計滞在時間を示します。 |
| [[!UICONTROL MVPD 識別子 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#mvpd) | `mvpd` | 文字列 | Adobe認証によって提供された [!UICONTROL MVPD 識別子 &#x200B;]。 |
| [[!UICONTROL &#x200B; 一時停止イベント &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#pause-events) | `pauseCount` | 整数 | [!UICONTROL &#x200B; 一時停止イベント &#x200B;] 再生中に発生した一時停止期間の数をカウントします。 |
| [[!UICONTROL &#x200B; 影響を受けたストリームの一時停止 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#paused-impacted-streams) | `hasPauseImpactedStreams` | ブール値：1 つのメディア項目の再生中に一時停止が 1 つ以上発生したかどうかを示します。 |
| [!UICONTROL Pccr] | `pccr` | ブール値 | [!UICONTROL Pccr] は、リダイレクトが発生したことを示します。 |
| [!UICONTROL Pev3] | `pev3` | 文字列 | [!UICONTROL Pev3] は、レポートに使用されるメディアストリームのタイプです。 |
| [[!UICONTROL &#x200B; 発行者 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#publisher) | `publisher` | 文字列 | オーディオコンテンツパブリッシャーの名前。 |
| [[!UICONTROL &#x200B; 無線局 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#station) | `station` | 文字列 | オーディオが再生されるラジオ局名。 |
| [[!UICONTROL &#x200B; 評価値 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#content-rating) | `rating` | 文字列 | テレビの保護者のガイドラインで定義されている評価。 |
| [[!UICONTROL &#x200B; レコードラベル &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#label) | `label` | 文字列 | レコードラベルの名前。 |
| [[!UICONTROL &#x200B; 再開 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#content-resumes) | `hasResume` | ブール値 | 30 分を超えるバッファー、一時停止または停止期間の後に再開された各再生を示します。 |
| [[!UICONTROL &#x200B; シーズン番号 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#season) | `season` | 文字列 | 番組が属する [!UICONTROL &#x200B; シーズン番号 &#x200B;]。 シーズン シリーズは、番組がシリーズの一部である場合にのみ必要です。 |
| [[!UICONTROL &#x200B; シリーズ名 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#show) | `show` | 文字列 | プログラム/シリーズ名。 番組がシリーズの一部である場合にのみ、プログラム名が必要です。 |
| [[!UICONTROL &#x200B; 表示タイプ &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#show-type) | `showType` | 文字列 | コンテンツのタイプ（例：予告編や完全なエピソード）。 |
| [[!UICONTROL &#x200B; ストリーム形式 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#stream-format) | `streamFormat` | 文字列 | ストリームの形式（HD、SD）。 |
| [[!UICONTROL &#x200B; ストリームの種類 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#stream-type) | `streamType` | 文字列 | メディアストリームのタイプ。 |
| [[!UICONTROL &#x200B; 合計一時停止時間 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#total-pause-duration) | `pauseTime` | 整数 | [!UICONTROL &#x200B; 合計一時停止時間 &#x200B;] は、ユーザーによって再生が一時停止された時間（秒）を表します。 |
| [[!UICONTROL &#x200B; ユニーク再生時間 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#unique-time-played) | `uniqueTimePlayed` | 整数 | タイムドメディアアセットでユーザーが見たユニークな間隔の合計を表します。つまり、複数回表示された再生間隔の長さは、1 回のみカウントされます。 |
| [[!UICONTROL &#x200B; バージョン &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#sdk-version) | `appVersion` | 文字列 | プレーヤーが使用する SDK のバージョン。 これには、プレーヤーにとって意味のあるカスタム値を含めることができます。 |
| [[!UICONTROL &#x200B; ビデオセグメント &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html?lang=ja#content-segment) | `segment` | 文字列 | 表示されたコンテンツの部分を表す間隔 (分)。 |

{style="table-layout:auto"}

<!-- Could not find details for :
Ad Load Type
Content Delivery Network
Pccr
Pev3
-->
