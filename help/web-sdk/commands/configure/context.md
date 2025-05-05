---
title: context
description: デバイス、環境、または場所のデータを自動的に収集します。
exl-id: 911cabec-2afb-4216-b413-80533f826b0e
source-git-commit: 89dfe037e28bae51e335dc67185afa42b2c418e3
workflow-type: tm+mt
source-wordcount: '915'
ht-degree: 13%

---

# `context`

`context` プロパティは、Web SDK が自動的に収集できるものを決定する文字列の配列です。 このデータは大きな価値を提供しますが、このデータの一部を省略すると、組織のプライバシーポリシーに準拠できるというメリットがあります。

## コンテキストキーワードと XDM 要素

特定のコンテキストキーワードを含めると、Web SDK は関連するすべての XDM 要素を自動的に設定します。 特定の XDM 要素を省略して他の要素を許可する場合は、[`onBeforeEventSend`](onbeforeeventsend.md) を使用して値を消去できます。 1 つのページに複数のイベントを送信する場合、Web SDK には、`SendEvent` 呼び出しごとにこれらのフィールドが含まれます。

### Web

`"web"` キーワードは、現在のページに関する情報を収集します。

| ディメンション | 説明 | XDM パス | 値の例 |
| --- | --- | --- | --- |
| ページ URL | 現在のページの URL。 | `xdm.web.webPageDetails.URL` | `https://example.com/index.html` |
| リファラー URL | 前に訪問したページの URL。 | `xdm.web.webReferrer.URL` | `http://example.org/linkedpage.html` |

{style="table-layout:auto"}

### デバイス

`"device"` キーワードは、ユーザーのデバイスに関する情報を収集します。

| ディメンション | 説明 | XDM パス | 値の例 |
| --- | --- | --- | --- |
| 画面の高さ | 画面の高さ（ピクセル単位）。 | `xdm.device.screenHeight` | `900` |
| 画面の幅 | 画面の幅（ピクセル単位）。 | `xdm.device.screenWidth` | `1440` |
| 画面の向き | 画面の向き。 | `xdm.device.screenOrientation` | `landscape` または `portrait` |

{style="table-layout:auto"}

### 環境

`"environment"` キーワードは、ユーザーのブラウザに関する情報を収集します。

| ディメンション | 説明 | XDM パス | 値の例 |
| --- | --- | --- | --- |
| 環境タイプ | エクスペリエンスが表示された環境のタイプ。 Web SDK は常にこのフィールドを `browser` に設定します。 | `xdm.environment.type` | `browser` |
| ビューポートの高さ | ブラウザーのコンテンツ領域の高さ（ピクセル単位）。 | `xdm.environment.browserDetails.viewportHeight` | `679` |
| ビューポートの幅 | ブラウザーのコンテンツ領域の幅（ピクセル単位）。 | `xdm.environment.browserDetails.viewportWidth` | `642` |

{style="table-layout:auto"}

### 場所の背景

`"placeContext"` キーワードは、ユーザーの場所に関する情報を収集します。

| ディメンション | 説明 | XDM パス | 値の例 |
| --- | --- | --- | --- |
| 現地時間 | 簡略化された拡張 [ISO 8601](https://datatracker.ietf.org/doc/html/rfc3339#section-5.6) 形式のエンドユーザーのローカルタイムスタンプ。 | `xdm.placeContext.localTime` | `YYYY-08-07T15:47:17.129-07:00` |
| ローカルタイムゾーンのオフセット | ユーザーが GMT からオフセットされる分数。 | `xdm.placeContext.localTimezoneOffset` | `360` |
| 国コード | エンドユーザーの国コード。 | `xdm.placeContext.geo.countryCode` | `US` |
| 州 | エンドユーザーの都道府県コード。 | `xdm.placeContext.geo.stateProvince` | `CA` |
| 緯度 | エンドユーザーの場所の緯度。 | `xdm.placeContext.geo._schema.latitude` | `37.3307447` |
| 経度 | エンドユーザーの場所の経度。 | `xdm.placeContext.geo._schema.longitude` | `-121.8945965` |

{style="table-layout:auto"}


### タイムスタンプ

`timestamp` キーワードは、イベントのタイムスタンプに関する情報を収集します。 コンテキストのこの部分は削除できません。

| ディメンション | 説明 | XDM パス | 値の例 |
| --- | --- | --- | --- |
| イベントのタイムスタンプ | 簡略化された拡張 [ISO 8601](https://datatracker.ietf.org/doc/html/rfc3339#section-5.6) 形式のエンドユーザーの UTC タイムスタンプ。 | `xdm.timestamp` | `2019-08-07T22:47:17.129Z` |

{style="table-layout:auto"}

### 実装の詳細

`implementationDetails` キーワードは、イベントの収集に使用される SDK バージョンに関する情報を収集します。

| ディメンション | 説明 | XDM パス | 値の例 |
| --- | --- | --- | --- |
| 名前 | ソフトウェア開発キット （SDK）の識別子。 このフィールドでは、URI を使用で、異なるソフトウェアライブラリで提供される ID 間の一意性を改善します。 | `xdm.implementationDetails.name` | スタンドアロンライブラリを使用する場合、値は `https://ns.adobe.com/experience/alloy` です。 ライブラリがタグ拡張機能の一部として使用されている場合、値は `https://ns.adobe.com/experience/alloy+reactor` です。 |
| バージョン | ソフトウェア開発キット （SDK） バージョン。 | `xdm.implementationDetails.version` | スタンドアロンライブラリを使用する場合、値はライブラリのバージョンです。 ライブラリがタグ拡張機能の一部として使用される場合、値はライブラリバージョンと、タグで結合された `+` グ拡張機能バージョンです。 例えば、ライブラリのバージョンが `2.1.0` で、タグ拡張機能のバージョンが `2.1.3` の場合、値は `2.1.0+2.1.3` になります。 |
| 環境 | データが収集された環境。 これは常に `browser` に設定されています。 | `xdm.implementationDetails.environment` | `browser` |


### 高エントロピーのクライアントヒント {#high-entropy-client-hints}

>[!TIP]
>
>設定方法について詳しくは、[user agent client hints](../../use-cases/client-hints.md) に関するドキュメントを参照してください。

`"highEntropyUserAgentHints"` キーワードは、ユーザーのデバイスに関する詳細情報を収集します。 このデータは、Adobeに送信されるリクエストの HTTP ヘッダーに含まれます。 Edge ネットワーク内にデータが到着すると、XDM オブジェクトは、それぞれの XDM パスを入力します。 `sendEvent` 呼び出しでそれぞれの XDM パスを設定すると、HTTP ヘッダー値よりも優先されます。

[ データストリームの設定 ](/help/datastreams/configure.md) 時にデバイス検索を使用する場合は、デバイス検索値を優先してデータを消去できます。 一部のクライアントヒントフィールドとデバイス検索フィールドは、同じヒットに存在できません。

| プロパティ | 説明 | HTTP ヘッダー | XDM パス | 例 |
| --- | --- | --- | --- | --- |
| オペレーティングシステムのバージョン | オペレーティングシステムのバージョン。 | `Sec-CH-UA-Platform-Version` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.platformVersion` | `10.15.7` |
| アーキテクチャ | 基盤となる CPU アーキテクチャ。 | `Sec-CH-UA-Arch` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.architecture` | `x86` |
| デバイスモデル | 使用されるデバイスの名前。 | `Sec-CH-UA-Model` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.model` | `Intel Mac OS X 10_15_7` |
| ビット数 | 基になる CPU アーキテクチャがサポートするビット数です。 | `Sec-CH-UA-Bitness` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.bitness` | `64` |
| ブラウザーベンダー | ブラウザーを作成した会社。 低エントロピーのヒント `Sec-CH-UA` でも、この要素が収集されます。 | `Sec-CH-UA-Full-Version-List` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.vendor` | `Google` |
| ブラウザー名 | 使用されるブラウザー。 低エントロピーのヒント `Sec-CH-UA` でも、この要素が収集されます。 | `Sec-UA-Full-Version-List` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.brand` | `Chrome` |
| ブラウザーバージョン | ブラウザーの重要なバージョン。 低エントロピーのヒント `Sec-CH-UA` でも、この要素が収集されます。 正確なブラウザーバージョンは自動的に収集されません。 | `Sec-UA-Full-Version-List` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.version` | `105` |

{style="table-layout:auto"}

## Web SDK タグ拡張機能を使用してコンテキスト情報を収集

コンテキスト情報の設定は、（タグ拡張機能の設定 [ 時のラジオボタンとチェックボックスの組み合わせです ](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md)。 各チェックボックスは、コンテキストキーワードにマッピングされます。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL 拡張機能]** に移動し、[!UICONTROL Adobe Experience Platform Web SDK **[!UICONTROL カードの]** 設定 &#x200B;] をクリックします。
1. [!UICONTROL &#x200B; データ収集 &#x200B;] セクションまでスクロールし、**[!UICONTROL すべてのデフォルトのコンテキスト情報]** または **[!UICONTROL 特定のコンテキスト情報]** を選択します。
1. **[!UICONTROL 特定のコンテキスト情報]** を選択した場合は、目的の各コンテキスト情報要素の横にあるチェックボックスを有効にします。
1. 「**[!UICONTROL 保存]**」をクリックして、変更を公開します。

## Web SDK JavaScript ライブラリを使用してコンテキスト情報を収集

`configure` コマンドを実行する場合は、文字列の `context` 配列を設定します。 SDK の設定時にこのプロパティを省略すると、`"highEntropyUserAgentHints"` を除くすべてのコンテキスト情報がデフォルトで収集されます。 高エントロピーのクライアントヒントを収集する場合や、データ収集から他のコンテキスト情報を省略する場合は、このプロパティを設定します。 文字列は、任意の順序で含めることができます。

>[!NOTE]
>
>高エントロピーのクライアントヒントを含むすべてのコンテキスト情報を収集する場合は、`context` 配列の文字列にすべての値を含める必要があります。 デフォルトの `context` 値は `highEntropyUserAgentHints` を省略します。`context` プロパティを設定した場合、省略された値はデータを収集しません。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  context: ["web", "device", "environment", "placeContext", "highEntropyUserAgentHints"]
});
```
