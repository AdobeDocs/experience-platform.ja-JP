---
title: メディアイベントを送信
description: メディアデータをAdobe Experience Platform Edge Networkに送信します。
source-git-commit: 639d3d3d8bfb51e6c97066aee61271d0b00ec455
workflow-type: tm+mt
source-wordcount: '828'
ht-degree: 0%

---

# メディアイベントを送信

**[!UICONTROL Send media event]** アクションは、メディアイベントをデータストリームに送信します。このデータストリームは、Adobe Experience PlatformやAdobe Analyticsなどのアプリやサービスで使用できます。 このアクションは、プロパティでストリーミングメディアコンテンツをトラックする場合に役立ちます。

![&#x200B; メディアイベント送信画面を示すExperience Platform UI 画像。](../assets/send-media-event.png)

使用できるオプションは、選択した **[!UICONTROL Media event type]** によって異なります。 すべてのメディアイベントタイプには **[!UICONTROL Player ID]** が必要です。これはコンテンツプレーヤーの名前です。

一部のイベントタイプには、他のフィールドを設定する機能があります。 メディアイベントタイプがここにリストされていない場合、使用可能なフィールドは [!UICONTROL Player ID] のみです。 次のメディアイベントタイプには、その他のフィールドが含まれます。

## [!UICONTROL Ad break start]

広告ポッドの詳細を設定できます。

* **[!UICONTROL Ad break name]**：広告ブレークのわかりやすい名前。
* **[!UICONTROL Ad break offset (seconds)]**：コンテンツ内の広告ブレークのオフセット （秒単位）。
* **[!UICONTROL Ad break index]**：コンテンツ内の広告ブレークのインデックス（1 から始まる）。

## [!UICONTROL Ad start]

広告の詳細を設定できます。

* **[!UICONTROL Ad name]**：広告のわかりやすい名前。
* **[!UICONTROL Ad ID]**：広告の ID。 任意の英数字の値を使用できます。
* **[!UICONTROL Ad length (seconds)]**：ビデオ広告の長さ（秒単位）。
* **[!UICONTROL Advertiser]**：広告で製品が取り上げられる会社またはブランド。
* **[!UICONTROL Campaign ID]**：広告キャンペーンの ID
* **[!UICONTROL Creative ID]**：広告クリエイティブの ID。
* **[!UICONTROL Creative URL]**：広告クリエイティブの URL。
* **[!UICONTROL Placement ID]**：広告のプレースメント ID。
* **[!UICONTROL Site ID]**：広告サイトの ID。
* **[!UICONTROL Pod position]**：親広告ブレーク内の広告のインデックス （0 から始まる）。

また、このイベントタイプでは、カスタムメタデータをメディアイベントペイロードの一部として提供する機能もサポートしています。

## [!UICONTROL Bitrate change]

* **[!UICONTROL Quality of experience data]**：ビットレート、ドロップフレーム数、1 秒あたりのフレーム数および開始時間を指定する [&#x200B; エクスペリエンス品質 &#x200B;](/help/xdm/data-types/qoe-data-details-collection.md) オブジェクト。

## [!UICONTROL Chapter start]

チャプターの詳細を設定できます。

* **[!UICONTROL Chapter name]**: チャプターまたはセグメントの名前。
* **[!UICONTROL Chapter length]**: チャプターの長さ（秒単位）。
* **[!UICONTROL Chapter index]**: コンテンツ内のチャプターの位置。
* **[!UICONTROL Chapter offset]**: コンテンツの開始時からのチャプターのオフセット （秒）。

また、このイベントタイプでは、カスタムメタデータをメディアイベントペイロードの一部として提供する機能もサポートしています。

## [!UICONTROL Error]

エラーの詳細を設定できます。

* **[!UICONTROL Error name]**: エラー名。
* **[!UICONTROL Source]**: エラーのソース。

## [!UICONTROL Session start]

メディアセッションの詳細を設定できます。

* **[!UICONTROL Handle media session automatically]**:Web SDKが必要な ping を自動的に送信するためのチェックボックス。 PING を自分で手動で送信する場合は、このチェックボックスをオフにできます。
* **[!UICONTROL Playhead]**：再生の再生ヘッド（秒単位）。
* **[!UICONTROL Content type]**：コンテンツタイプ。 任意の文字列値がサポートされます。Adobeには、次のプリセットも用意されています。
   * [!UICONTROL Audiobook]
   * [!UICONTROL Downloaded video-on-demand]
   * [!UICONTROL Linear playback of the media asset]
   * [!UICONTROL Live streaming]
   * [!UICONTROL Podcast]
   * [!UICONTROL Radio show]
   * [!UICONTROL Song]
   * [!UICONTROL User-generated content]
   * [!UICONTROL Video-on-demand]
* **[!UICONTROL Clip length/runtime (seconds)]**：消費されるコンテンツの最大継続時間（秒単位）。 時間が不明なライブメディアの場合、デフォルト値は `86400` です。
* **[!UICONTROL Content ID]**：コンテンツのコンテンツ ID。
* **[!UICONTROL Ad load type]**：読み込まれた広告のタイプ。 次の 2 つの値がサポートされています。
   * [!UICONTROL Ads same as TV]
   * [!UICONTROL Other (custom/dynamic ads)]
* **[!UICONTROL Album]**：曲が属するアルバム。
* **[!UICONTROL Artist]**：この曲のアーティスト。
* **[!UICONTROL Asset ID]**：メディアアセットのコンテンツの一意の ID。 これらの ID は、通常、EIDR、TMS/Gracenote、Rovi などのメタデータ認証機関から派生します。 これらの識別子は、他の独自システムや社内システムからも取得できます。
* **[!UICONTROL Author]**：オーディオブックの作成者の名前。
* **[!UICONTROL Authorized]**：ユーザーがAdobe認証でログインしているかどうかを判断するフラグ。
* **[!UICONTROL Day part]**：コンテンツが放送または再生された時刻。 任意の文字列値がサポートされています。
* **[!UICONTROL Episode]**：エピソード番号。
* **[!UICONTROL Feed type]**：フィードのタイプ。
* **[!UICONTROL First air date]**：コンテンツがテレビで最初に放送された日付。 任意の文字列値がサポートされますが、Adobeでは `YYYY-MM-DD` 形式を使用することをお勧めします。
* **[!UICONTROL First digital date]**：コンテンツが任意のデジタルチャネルまたはプラットフォームで最初に放送された日付。 任意の文字列値がサポートされますが、Adobeでは `YYYY-MM-DD` 形式を使用することをお勧めします。
* **[!UICONTROL Content name]**: コンテンツのわかりやすい名前。
* **[!UICONTROL Genre]**：コンテンツプロデューサーによって定義されたコンテンツのタイプまたはグループ。 このフィールドは、コンマで区切られた複数の値をサポートします。
* **[!UICONTROL Label]**: レコードラベルの名前。
* **[!UICONTROL Rating]**: テレビの保護者によるガイドラインで定義されているレーティング。
* **[!UICONTROL MVPD]**:Adobe認証で提供されるMVPD。
* **[!UICONTROL Network]**: ネットワーク名またはチャネル名。
* **[!UICONTROL Originator]**：コンテンツの作成者。
* **[!UICONTROL Publisher]**：オーディオコンテンツのパブリッシャー。
* **[!UICONTROL Season]**：その番組がシリーズの一部である場合は、その番組のシーズン番号。
* **[!UICONTROL Show]**：番組がシリーズの一部である場合は、シリーズ名。
* **[!UICONTROL Show type]**：表示タイプ。 任意の文字列値がサポートされます。Adobeには、次のプリセットも用意されています。
   * [!UICONTROL Clip]
   * [!UICONTROL Full episode]
   * [!UICONTROL Other]
   * [!UICONTROL Preview/trailer]
* **[!UICONTROL Stream type]**：ストリームのタイプ。
* **[!UICONTROL Stream format]**:HD や SD など、ストリームの形式。
* **[!UICONTROL Station]**: ラジオ局の名前または ID。

また、このイベントタイプでは、カスタムメタデータをメディアイベントペイロードの一部として提供する機能もサポートしています。 また、データストリーム設定の上書きが可能になり、このデータを受信するアプリやサービスを制御できます。 個々のコマンドとタグ拡張機能設定内の両方でデータストリーム設定の上書きを設定した場合、個々のコマンドが優先されます。 詳しくは、[&#x200B; データストリーム設定の上書き &#x200B;](../configure/configuration-overrides.md) を参照してください。

## [!UICONTROL States update]

状態の更新の詳細を設定できます。 次の状態を開始または終了できます。

* [!UICONTROL Closed captioning]
* [!UICONTROL Full screen]
* [!UICONTROL In focus]
* [!UICONTROL Mute]
* [!UICONTROL Picture in picture]

次のフィールドを使用できます。

* **[!UICONTROL States started]**：状態の開始を示すことができるドロップダウンメニュー。 「**[!UICONTROL Add another state that started]**」ボタンを選択すると、同じアクションで複数の状態を開始できます。
* **[!UICONTROL States ended]**：状態が終了したことを示すドロップダウンメニュー。 「**[!UICONTROL Add another state that ended]**」ボタンを選択すると、同じアクションで複数の状態を終了できます。
