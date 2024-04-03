---
title: セッションの詳細コレクションのデータタイプ
description: セッション詳細収集のエクスペリエンスデータモデル (XDM) データタイプについて説明します。
source-git-commit: fb000d45d828c01fdb08d7555512f07ab52e1f7b
workflow-type: tm+mt
source-wordcount: '926'
ht-degree: 15%

---

# [!UICONTROL セッションの詳細] コレクションデータタイプ

[!UICONTROL セッションの詳細] コレクションは、メディア再生セッションに関連するデータを追跡する、標準の Experience Data Model(XDM) データタイプです。 メディア収集フィールドは、他のメディアに送信されるデータをキャプチャして、さらに処理するためにAdobe サービスに送信するために使用されます。 このスキーマには、ユーザーの行動とコンテンツ消費パターンに関するインサイトを提供するために使用できる様々なプロパティが含まれています。 以下を使用します。 [!UICONTROL セッションの詳細] 再生イベント、広告インタラクション、プログレスマーカー、一時停止およびその他の指標を記録することで、ユーザーエンゲージメントをキャプチャするための収集データ型です。

+++「 」を選択して、Session Details Collection データ型の図を表示します。
![Session Details Collection データ型の図です。](../images/data-types/session-details-collection.png)
+++

>[!NOTE]
>
>各ディスプレイ名には、オーディオおよびビデオパラメーターに関する詳細情報へのリンクが含まれます。 リンクされたページには、Adobe、実装値、ネットワークパラメーター、レポートおよび重要な考慮事項によって収集されるビデオ広告データの詳細が含まれます。

| 表示名 | プロパティ | データタイプ | 必須 | 説明 |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------|-----------|----------|---------------------------------------------------------------------------------------|
| [!UICONTROL 広告読み込みタイプ] | `adLoad` | 文字列 | × | 各顧客の内部表現によって定義され、読み込まれる広告のタイプ。 |
| [[!UICONTROL アルバム]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#album) | `album` | 文字列 | × | 音楽の録音またはビデオが属するアルバムの名前。 |
| [[!UICONTROL アーティスト]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#artist) | `artist` | 文字列 | × | 音楽の録音やビデオを実行するアルバムアーティストまたはグループの名前。 |
| [[!UICONTROL アセット ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#asset-id) | `assetID` | 文字列 | × | The [!UICONTROL アセット ID] は、TV シリーズのエピソードの識別子、ムービーアセットの識別子、ライブイベントの識別子など、メディアアセットのコンテンツの一意の識別子です。 この ID は通常、EIDR、TMS／Gracenote、Rovi などのメタデータを扱う機関から取得します。その他の独自のシステムや社内システムから取得することもできます。 |
| [[!UICONTROL 作成者]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#author) | `author` | 文字列 | × | メディア作成者の名前。 |
| [[!UICONTROL 放送コンテンツタイプ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-type) | `contentType` | 文字列 | ○ | The [!UICONTROL 放送コンテンツタイプ] ストリーム配信の。 次の単位で使用可能な値： [!UICONTROL ストリームタイプ] 次を含む：<br>オーディオ： &quot;song&quot;、&quot;podcast&quot;、&quot;audiobook&quot;、および&quot;radio&quot;;<br>ビデオ：「VoD」、「Live」、「Linear」、「UGC」、「DVoD」。<br>お客様は、このパラメーターのカスタム値を指定できます。 |
| [[!UICONTROL 放送網]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#network) | `network` | 文字列 | × | ネットワーク/チャネル名。 |
| [[!UICONTROL コンテンツチャネル]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-channel) | `channel` | 文字列 | ○ | The [!UICONTROL コンテンツチャネル] は、コンテンツの再生元となった配信チャネルです。 |
| [[!UICONTROL コンテンツ完了]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-complete) | `isCompleted` | ブール値 | × | [!UICONTROL コンテンツ完了] タイムドメディアアセットが最後まで視聴されたかどうかを示します。 このイベントは、必ずしも視聴者がビデオ全体を視聴したとは限らず、視聴者が先にスキップした可能性もあります。 |
| [!UICONTROL コンテンツ配信ネットワーク] | `cdn` | 文字列 | × | The [!UICONTROL コンテンツ配信ネットワーク] 再生されたコンテンツの数を示します。 |
| [[!UICONTROL コンテンツ ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-id) | `name` | 文字列 | ○ | The [!UICONTROL コンテンツ ID] は、コンテンツの一意の識別子です。 他の業界や CMS ID へのリンクに使用できます。 |
| [[!UICONTROL コンテンツ名]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-name-(variable)) | `friendlyName` | 文字列 | × | The [!UICONTROL コンテンツ名] は、コンテンツのわかりやすい（人間が読み取れる）名前です。 |
| [[!UICONTROL コンテンツプレイヤー名]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-player-name) | `playerName` | 文字列 | ○ | コンテンツプレーヤーの名前。 |
| [[!UICONTROL 作成者名]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#originator) | `originator` | 文字列 | × | コンテンツ作成者の名前。 |
| [[!UICONTROL 日パート]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#day-part) | `dayPart` | 文字列 | × | コンテンツが放送または再生された時間帯を定義するプロパティ。 お客様の必要に応じて、任意の値を設定できます。 |
| [[!UICONTROL エピソード番号]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#episode) | `episode` | 文字列 | × | エピソードの番号。 |
| [[!UICONTROL フィードのタイプ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#media-feed-type) | `feed` | 文字列 | × | フィードのタイプ。実際のフィード関連データ（EAST HD や SD など）を表す場合と、フィードのソース（URL など）を表す場合があります。 |
| [[!UICONTROL 初回放送日]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#first-air-date) | `firstAirDate` | 文字列 | × | コンテンツがテレビで最初に放送された日付。 どのような形式でも問題ありませんが、Adobeでは YYYY-MM-DD の形式にすることをお勧めします。 |
| [[!UICONTROL 初回デジタル日]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#first-digital-date) | `firstDigitalDate` | 文字列 | × | コンテンツが何らかのデジタルチャネルまたはプラットフォームで最初に放送された日付。 どのような形式でも問題ありませんが、Adobeでは YYYY-MM-DD の形式にすることをお勧めします。 |
| [[!UICONTROL ジャンル]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#genre) | `genre` | 文字列 | × | コンテンツ作成者が定義したコンテンツのタイプまたは分類。 変数の実装では、値をコンマで区切る必要があります。 |
| [[!UICONTROL メディア認証済み]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#authorized) | `authorized` | 文字列 | × | ユーザーが認証を通じて認証されたかどうかをAdobeします。 |
| [[!UICONTROL メディアコンテンツの長さ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-length-(variable)) | `length` | 整数 | ○ | The [!UICONTROL メディアコンテンツの長さ] クリップの長さ/ランタイムを含む — 視聴されるコンテンツの最大の長さまたは時間（秒単位）です。 |
| [[!UICONTROL メディア開始]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#media-starts) | `isViewed` | ブール値 | × | メディアの load イベント。 これは、ビューアが再生ボタンを選択したときに発生します。 これは、プリロール広告、バッファリング、エラーなどがある場合でもカウントされます。 |
| [[!UICONTROL MVPD 識別子]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#mvpd) | `mvpd` | 文字列 | × | Adobe認証を介して提供されたマルチチャネルビデオプログラミングディストリビューター (MVPD) の識別子。 |
| [[!UICONTROL 投稿者]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#publisher) | `publisher` | 文字列 | × | オーディオコンテンツパブリッシャーの名前。 |
| [[!UICONTROL ラジオ局]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#station) | `station` | 文字列 | × | オーディオを再生するラジオ局の名前。 |
| [[!UICONTROL 評価値]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-rating) | `rating` | 文字列 | × | TV Parental Guidelines で定義されたレーティング。 |
| [[!UICONTROL レコードラベル]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#label) | `label` | 文字列 | × | レコードラベルの名前。 |
| [[!UICONTROL 再開]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-resumes) | `hasResume` | ブール値 | × | 30 分を超えるバッファー、一時停止または停止期間の後に再開された各再生を示します。 |
| [[!UICONTROL シーズン番号]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#season) | `season` | 文字列 | × | The [!UICONTROL シーズン番号] 番組が属する シーズンのシリーズは、番組がシリーズものの一部の場合のみ必要です。 |
| [[!UICONTROL シリーズ名]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#show) | `show` | 文字列 | × | プログラム/シリーズの名前。 番組名は、番組がシリーズものの一部の場合にのみ必要です。 |
| [[!UICONTROL 番組タイプ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#show-type) | `showType` | 文字列 | × | コンテンツのタイプ。 例えば、トレーラーや完全なエピソードなどです。 コンテンツのタイプは 0 ～ 3 の整数で表します。 例えば、「0」はエピソード全体、「1」はプレビュー/トレーラー、「2」はクリップ、「3」はエピソード全体を表します。 |
| [[!UICONTROL ストリーム形式]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#stream-format) | `streamFormat` | 文字列 | × | ストリームの形式 (HD、SD)。 |
| [[!UICONTROL ストリームタイプ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#stream-type) | `streamType` | 文字列 | × | メディアストリームのタイプ。 |
| [[!UICONTROL バージョン]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#sdk-version) | `appVersion` | 文字列 | × | プレーヤーで使用される SDK のバージョン。 プレーヤーに対して意味のある任意のカスタム値を持つことができます。 |

{style="table-layout:auto"}

<!-- This is required for sessionStart. 
Q) How do I indicate that?
Q) Do you know where to link for:
Ad Load Type
Content Delivery Network
 -->
