---
title: セッションの詳細収集データタイプ
description: セッションの詳細収集エクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: ffe6bcf7-61e1-4f7a-ba95-7fcb78683cc9
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '764'
ht-degree: 13%

---

# [!UICONTROL Session Details] コレクション データ型

[!UICONTROL Session Details] コレクションは、メディア再生セッションに関連するデータを追跡する標準のExperience Data Model （XDM） データ型です。 メディア収集フィールドは、さらなる処理のために他のAdobe サービスに送信されるデータをキャプチャするために使用されます。 このスキーマには、ユーザーの行動やコンテンツの消費パターンに関するインサイトを提供するために使用できる、幅広いプロパティが含まれています。 [!UICONTROL Session Details] コレクションのデータ型を使用して、再生イベント、広告インタラクション、進行状況マーカー、一時停止などの指標をログに記録し、ユーザーのエンゲージメントを取得します。

+++を選択して、セッションの詳細収集データタイプのダイアグラムを表示します。
![&#x200B; セッション詳細収集データタイプのダイアグラム。](../images/data-types/session-details-collection.png)
+++

>[!NOTE]
>
>各表示名には、オーディオおよびビデオのパラメーターに関する詳細情報へのリンクが含まれています。 リンクされたページには、Adobeによって収集されたビデオとデータ、実装値、ネットワークパラメーター、レポート、および重要な考慮事項の詳細が含まれています。

| 表示名 | プロパティ | データタイプ | 必須 | 説明 |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------|-----------|----------|---------------------------------------------------------------------------------------|
| [!UICONTROL Ad Load Type] | `adLoad` | 文字列 | × | 各顧客の内部表現によって定義され、読み込まれる広告のタイプ。 |
| [[!UICONTROL Album]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#album) | `album` | 文字列 | × | ミュージックレコーディングまたはビデオが属するアルバムの名前。 |
| [[!UICONTROL Artist]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#artist) | `artist` | 文字列 | × | ミュージックレコーディングまたはビデオを実行するアルバムアーティストまたはグループの名前。 |
| [[!UICONTROL Asset ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#asset-id) | `assetID` | 文字列 | × | [!UICONTROL Asset ID]は、TV シリーズのエピソード識別子、映画アセット識別子、ライブイベント識別子など、メディアアセットのコンテンツの一意の識別子です。 通常、これらのIDは、EIDR、TMS/Gracenote、Roviなどのメタデータ機関から取得されます。 このIDは、他の専有システムや社内システムから取得することもできます。 |
| [[!UICONTROL Author]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#author) | `author` | 文字列 | × | メディア作成者の名前。 |
| [[!UICONTROL Broadcast Content Type]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-type) | `contentType` | 文字列 | ○ | ストリーム配信の[!UICONTROL Broadcast Content Type]。 [!UICONTROL Stream Type]ごとに使用可能な値は次のとおりです。<br> オーディオ：「song」、「podcast」、「audiobook」、「radio」、<br> ビデオ：「VoD」、「Live」、「Linear」、「UGC」、および「DVoD」。<br>お客様は、このパラメーターにカスタム値を指定できます。 |
| [[!UICONTROL Broadcast Network]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#network) | `network` | 文字列 | × | ネットワーク/チャネル名。 |
| [[!UICONTROL Content Channel]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-channel) | `channel` | 文字列 | ○ | [!UICONTROL Content Channel]は、コンテンツが再生された配信チャネルです。 |
| [!UICONTROL Content Delivery Network] | `cdn` | 文字列 | × | コンテンツの[!UICONTROL Content Delivery Network]が再生されました。 |
| [[!UICONTROL Content ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-id) | `name` | 文字列 | ○ | [!UICONTROL Content ID]は、コンテンツの一意のIDです。 ほかの業界やCMS IDにリンクするために使用できます。 |
| [[!UICONTROL Content Name]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-name-(variable)) | `friendlyName` | 文字列 | × | [!UICONTROL Content Name]は、コンテンツの「わかりやすい」（人間が読み取れる）名前です。 |
| [[!UICONTROL Content Player Name]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-player-name) | `playerName` | 文字列 | ○ | コンテンツプレーヤーの名前。 |
| [[!UICONTROL Creator Name]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#originator) | `originator` | 文字列 | × | コンテンツ作成者の名前。 |
| [[!UICONTROL Day Part]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#day-part) | `dayPart` | 文字列 | × | コンテンツがブロードキャストまたは再生された時刻を定義するプロパティ。 これは、顧客が必要に応じて設定した値を持つ可能性があります |
| [[!UICONTROL Episode Number]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#episode) | `episode` | 文字列 | × | エピソードの番号。 |
| [[!UICONTROL Feed Type]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#media-feed-type) | `feed` | 文字列 | × | フィードのタイプ。EAST HDやSDなどの実際のフィード関連データを表すか、URLなどのフィードのソースを表すことができます。 |
| [[!UICONTROL First Air Date]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#first-air-date) | `firstAirDate` | 文字列 | × | コンテンツが最初にテレビで放映された日付。 任意の日付フォーマットを使用できますが、AdobeではYYYY-MM-DDをお勧めします。 |
| [[!UICONTROL First Digital Date]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#first-digital-date) | `firstDigitalDate` | 文字列 | × | デジタルチャネルやプラットフォームでコンテンツが最初に放映された日付。 任意の日付フォーマットを使用できますが、AdobeではYYYY-MM-DDをお勧めします。 |
| [[!UICONTROL Genre]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#genre) | `genre` | 文字列 | × | コンテンツプロデューサーによって定義されたコンテンツのタイプまたはグループ。 変数の実装では、値はコンマ区切りにする必要があります。 |
| [[!UICONTROL Media Authorized]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#authorized) | `authorized` | 文字列 | × | Adobe認証を介してユーザーが承認されているかどうかを確認します。 |
| [[!UICONTROL Media Content Length]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-length-(variable)) | `length` | 整数 | ○ | [!UICONTROL Media Content Length]には、クリップの長さ/ランタイムが含まれています。これは、消費されるコンテンツの最大長（またはデュレーション）です（秒単位）。 |
| [[!UICONTROL MVPD Identifier]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#mvpd) | `mvpd` | 文字列 | × | Adobe認証を介して提供されたMulti-channel Video Programming Distributor （MVPD） ID。 |
| [[!UICONTROL Publisher]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#publisher) | `publisher` | 文字列 | × | オーディオコンテンツパブリッシャーの名前。 |
| [[!UICONTROL Radio Station]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#station) | `station` | 文字列 | × | オーディオが再生されるラジオ局名。 |
| [[!UICONTROL Rating Value]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-rating) | `rating` | 文字列 | × | TV Parental Guidelinesで定義された評価。 |
| [[!UICONTROL Record Label]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#label) | `label` | 文字列 | × | レコードラベルの名前。 |
| [[!UICONTROL Resume]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#content-resumes) | `hasResume` | ブール | × | 30 分を超えるバッファー、一時停止または停止期間の後に再開された各再生を示します。 |
| [[!UICONTROL Season Number]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#season) | `season` | 文字列 | × | 番組が属する[!UICONTROL Season Number]です。 シーズンシリーズは、番組がシリーズの一部である場合にのみ必要です。 |
| [[!UICONTROL Series Name]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#show) | `show` | 文字列 | × | プログラム/シリーズ名。 プログラム名は、番組がシリーズの一部である場合にのみ必要です。 |
| [[!UICONTROL Show Type]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#show-type) | `showType` | 文字列 | × | コンテンツの種類： たとえば、予告編や完全なエピソードです。 コンテンツのタイプは、0 ～ 3の整数で表されます。 例えば、「0」 = フルエピソード、「1」 = プレビュー/トレーラー、「2」 = クリップ、「3」 =その他です。 |
| [[!UICONTROL Stream Format]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#stream-format) | `streamFormat` | 文字列 | × | ストリームの形式（HD、SD）。 |
| [[!UICONTROL Stream Type]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#stream-type) | `streamType` | 文字列 | × | メディアストリームのタイプ。 |
| [[!UICONTROL Version]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/audio-video-parameters.html#sdk-version) | `appVersion` | 文字列 | × | プレイヤーが使用するSDK バージョン。 これは、プレイヤーにとって意味のある任意のカスタム値を持つことができます。 |

{style="table-layout:auto"}
