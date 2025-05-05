---
title: セッションの詳細コレクション データ タイプ
description: セッション詳細収集エクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: ffe6bcf7-61e1-4f7a-ba95-7fcb78683cc9
source-git-commit: 9350cfc299c20bd63a2a559c177b3af02739e5b9
workflow-type: tm+mt
source-wordcount: '857'
ht-degree: 16%

---

# [!UICONTROL &#x200B; セッションの詳細 &#x200B;] 収集データタイプ

[!UICONTROL &#x200B; セッションの詳細 &#x200B;] コレクションは、メディア再生セッションに関連するデータを追跡する標準の Experience Data Model （XDM）データタイプです。 Media Collection フィールドは、他のAdobe サービスに送信され、さらに処理されるデータをキャプチャするために使用されます。 このスキーマには、ユーザーの行動やコンテンツ消費パターンに関するインサイトを提供するために使用できる幅広いプロパティが含まれています。 [!UICONTROL &#x200B; セッションの詳細 &#x200B;] コレクション データタイプを使用すると、再生イベント、広告インタラクション、進捗マーカー、一時停止、その他の指標をログに記録して、ユーザーエンゲージメントをキャプチャできます。

+++選択すると、セッションの詳細の収集データタイプを示す図が表示されます。
![ セッションの詳細コレクション データ型の図。](../images/data-types/session-details-collection.png)
+++

>[!NOTE]
>
>各表示名には、オーディオおよびビデオパラメーターに関する詳細情報へのリンクが含まれています。 リンク先のページには、Adobeで収集された動画広告データの詳細、実装値、ネットワークパラメーター、レポート、重要な検討事項が含まれています。

| 表示名 | プロパティ | データタイプ | 必須 | 説明 |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------|-----------|----------|---------------------------------------------------------------------------------------|
| [!UICONTROL &#x200B; 広告読み込みのタイプ &#x200B;] | `adLoad` | 文字列 | × | 各顧客の内部表現によって定義され、読み込まれる広告のタイプ。 |
| [[!UICONTROL &#x200B; アルバム &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#album) | `album` | 文字列 | × | 音楽の録音やビデオが含まれるアルバムの名前。 |
| [[!UICONTROL &#x200B; アーティスト &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#artist) | `artist` | 文字列 | × | 音楽の録音やビデオで演奏しているアルバムアーティストやグループの名前。 |
| [[!UICONTROL &#x200B; アセット ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#asset-id) | `assetID` | 文字列 | × | [!UICONTROL &#x200B; アセット ID] は、TV シリーズのエピソード識別子、映画アセット識別子、ライブイベント識別子など、メディアアセットのコンテンツの一意の識別子です。 通常、これらの ID は、EIDR、TMS/Gracenote、Rovi などのメタデータ認証機関から派生します。 これらの識別子は、他の独自システムや社内システムからも取得できます。 |
| [[!UICONTROL &#x200B; 作成者 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#author) | `author` | 文字列 | × | メディア作成者の名前。 |
| [[!UICONTROL &#x200B; 放送コンテンツの種類 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-type) | `contentType` | 文字列 | ○ | ストリーム配信の [!UICONTROL &#x200B; ブロードキャストコンテンツタイプ &#x200B;]。 [!UICONTROL &#x200B; ストリームタイプ &#x200B;] ごとに使用できる値は次のとおりです。<br> オーディオ：&quot;song&quot;、&quot;podcast&quot;、&quot;audiobook&quot;、および&quot;radio&quot;;<br> ビデオ：&quot;VoD&quot;、&quot;Live&quot;、&quot;Linear&quot;、&quot;UGC&quot;、および&quot;DVoD&quot;。<br> お客様は、このパラメーターにカスタム値を指定できます。 |
| [[!UICONTROL &#x200B; 放送ネットワーク &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#network) | `network` | 文字列 | × | ネットワーク/チャネル名。 |
| [[!UICONTROL &#x200B; コンテンツチャネル &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-channel) | `channel` | 文字列 | ○ | [!UICONTROL &#x200B; コンテンツチャネル &#x200B;] は、コンテンツの再生元となる配信チャネルです。 |
| [!UICONTROL &#x200B; コンテンツ配信ネットワーク &#x200B;] | `cdn` | 文字列 | × | 再生されるコンテンツの [!UICONTROL &#x200B; コンテンツ配信ネットワーク &#x200B;]。 |
| [[!UICONTROL &#x200B; コンテンツ ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-id) | `name` | 文字列 | ○ | [!UICONTROL &#x200B; コンテンツ ID] は、コンテンツの一意の識別子です。 他の業界 ID やCMS ID へのリンクに使用できます。 |
| [[!UICONTROL &#x200B; コンテンツ名 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-name-(variable)) | `friendlyName` | 文字列 | × | [!UICONTROL &#x200B; コンテンツ名 &#x200B;] は、コンテンツの「わかりやすい」（人間が読み取れる）名前です。 |
| [[!UICONTROL &#x200B; コンテンツプレーヤー名 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-player-name) | `playerName` | 文字列 | ○ | コンテンツプレイヤーの名前。 |
| [[!UICONTROL &#x200B; 作成者名 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#originator) | `originator` | 文字列 | × | コンテンツ作成者の名前。 |
| [[!UICONTROL &#x200B; 日分割 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#day-part) | `dayPart` | 文字列 | × | コンテンツが放送または再生された時間帯を定義するプロパティ。 これには、顧客が必要に応じて任意の値を設定できます |
| [[!UICONTROL &#x200B; エピソード番号 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#episode) | `episode` | 文字列 | × | エピソードの数。 |
| [[!UICONTROL &#x200B; フィードの種類 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#media-feed-type) | `feed` | 文字列 | × | フィードのタイプ。実際のフィード関連データ （EAST HD、SD など）を表すものと、フィードのソース （URL など）を表すものがあります。 |
| [[!UICONTROL &#x200B; 初回放送日 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#first-air-date) | `firstAirDate` | 文字列 | × | コンテンツがテレビで最初に放送された日付。 任意の日付フォーマットを使用できますが、Adobeでは YYYY-MM-DD の形式を使用することをお勧めします。 |
| [[!UICONTROL &#x200B; 初回デジタル日 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#first-digital-date) | `firstDigitalDate` | 文字列 | × | コンテンツが任意のデジタルチャネルまたはプラットフォームで最初に放送された日付。 任意の日付フォーマットを使用できますが、Adobeでは YYYY-MM-DD の形式を使用することをお勧めします。 |
| [[!UICONTROL &#x200B; ジャンル &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#genre) | `genre` | 文字列 | × | コンテンツプロデューサーによって定義されたコンテンツのタイプまたはグループ。 値は、変数の実装でコンマで区切る必要があります。 |
| [[!UICONTROL &#x200B; メディア承認済み &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#authorized) | `authorized` | 文字列 | × | ユーザーがAdobe認証によって認証されているかどうかを確認します。 |
| [[!UICONTROL &#x200B; メディアコンテンツの長さ &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-length-(variable)) | `length` | 整数 | ○ | [!UICONTROL &#x200B; メディアコンテンツ長 &#x200B;] には、クリップの長さ/ランタイムが含まれます。これは、消費されるコンテンツの最大長（またはデュレーション）です（秒単位）。 |
| [[!UICONTROL MVPD 識別子 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#mvpd) | `mvpd` | 文字列 | × | Adobe認証で提供された Multi-channel Video Programming Distributor （MVPD）識別子。 |
| [[!UICONTROL &#x200B; 発行者 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#publisher) | `publisher` | 文字列 | × | オーディオコンテンツパブリッシャーの名前。 |
| [[!UICONTROL &#x200B; 無線局 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#station) | `station` | 文字列 | × | オーディオが再生されるラジオ局名。 |
| [[!UICONTROL &#x200B; 評価値 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-rating) | `rating` | 文字列 | × | テレビの保護者のガイドラインで定義されている評価。 |
| [[!UICONTROL &#x200B; レコードラベル &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#label) | `label` | 文字列 | × | レコードラベルの名前。 |
| [[!UICONTROL &#x200B; 再開 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-resumes) | `hasResume` | ブール値 | × | 30 分を超えるバッファー、一時停止または停止期間の後に再開された各再生を示します。 |
| [[!UICONTROL &#x200B; シーズン番号 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#season) | `season` | 文字列 | × | 番組が属する [!UICONTROL &#x200B; シーズン番号 &#x200B;]。 シーズン シリーズは、番組がシリーズの一部である場合にのみ必要です。 |
| [[!UICONTROL &#x200B; シリーズ名 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#show) | `show` | 文字列 | × | プログラム/シリーズ名。 番組がシリーズの一部である場合にのみ、プログラム名が必要です。 |
| [[!UICONTROL &#x200B; 表示タイプ &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#show-type) | `showType` | 文字列 | × | コンテンツのタイプ。 例えば、トレーラーや完全なエピソード。 コンテンツのタイプは、0 ～ 3 の整数で表されます。 例えば、「0」 =完全なエピソード、「1」 = プレビュー/トレーラー、「2」 = クリップ、「3」 =その他です。 |
| [[!UICONTROL &#x200B; ストリーム形式 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#stream-format) | `streamFormat` | 文字列 | × | ストリームの形式（HD、SD）。 |
| [[!UICONTROL &#x200B; ストリームの種類 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#stream-type) | `streamType` | 文字列 | × | メディアストリームのタイプ。 |
| [[!UICONTROL &#x200B; バージョン &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#sdk-version) | `appVersion` | 文字列 | × | プレーヤーが使用する SDK のバージョン。 これには、プレーヤーにとって意味のあるカスタム値を含めることができます。 |

{style="table-layout:auto"}
