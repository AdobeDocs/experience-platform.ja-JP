---
title: context
description: デバイス、環境、または場所のデータを自動的に収集します。
exl-id: 911cabec-2afb-4216-b413-80533f826b0e
source-git-commit: dc2a2ecf7b602d2fcfd3b6c93cecdb6f3368a3f9
workflow-type: tm+mt
source-wordcount: '900'
ht-degree: 14%

---

# `context`

The `context` プロパティは、Web SDK が自動的に収集できる内容を決定する文字列の配列です。 このデータは大きな価値を提供しますが、このデータの一部を省略すると、組織のプライバシーポリシーに準拠できるので有益です。

## コンテキストキーワードと XDM 要素

特定のコンテキストキーワードを含めると、Web SDK は関連するすべての XDM 要素を自動的に設定します。 他のユーザーを許可しながら特定の XDM 要素を省略する場合は、 [`onBeforeEventSend`](onbeforeeventsend.md). 1 つのページで複数のイベントを送信する場合、Web SDK の各ページに `SendEvent` を呼び出します。

### Web

The `"web"` キーワードは、現在のページに関する情報を収集します。

| ディメンション | 説明 | XDM パス | 値の例 |
| --- | --- | --- | --- |
| ページ URL | 現在のページの URL。 | `xdm.web.webPageDetails.URL` | `https://example.com/index.html` |
| リファラー URL | 前に訪問したページの URL。 | `xdm.web.webReferrer.URL` | `http://example.org/linkedpage.html` |

{style="table-layout:auto"}

### デバイス

The `"device"` キーワードは、ユーザーのデバイスに関する情報を収集します。

| ディメンション | 説明 | XDM パス | 値の例 |
| --- | --- | --- | --- |
| 画面の高さ | 画面の高さ（ピクセル単位）。 | `xdm.device.screenHeight` | `900` |
| 画面の幅 | 画面の幅（ピクセル単位）。 | `xdm.device.screenWidth` | `1440` |
| 画面の向き | 画面の向き。 | `xdm.device.screenOrientation` | `landscape` または `portrait` |

{style="table-layout:auto"}

### 環境

The `"environment"` キーワードは、ユーザーのブラウザーに関する情報を収集します。

| ディメンション | 説明 | XDM パス | 値の例 |
| --- | --- | --- | --- |
| 環境タイプ | エクスペリエンスが表示される環境のタイプ。 Web SDK は、このフィールドを常に `browser`. | `xdm.environment.type` | `browser` |
| ビューポートの高さ | ブラウザーのコンテンツ領域の高さをピクセル単位で指定します。 | `xdm.environment.browserDetails.viewportHeight` | `679` |
| ビューポートの幅 | ブラウザーのコンテンツ領域の幅をピクセル単位で指定します。 | `xdm.environment.browserDetails.viewportWidth` | `642` |

{style="table-layout:auto"}

### 場所の背景

The `"placeContext"` キーワードは、ユーザーの場所に関する情報を収集します。

| ディメンション | 説明 | XDM パス | 値の例 |
| --- | --- | --- | --- |
| 現地時間 | エンドユーザーのローカルタイムスタンプ（簡易拡張） [ISO 8601](https://datatracker.ietf.org/doc/html/rfc3339#section-5.6) 形式を使用します。 | `xdm.placeContext.localTime` | `YYYY-08-07T15:47:17.129-07:00` |
| ローカルタイムゾーンのオフセット | ユーザーが GMT からオフセットする時間（分）。 | `xdm.placeContext.localTimezoneOffset` | `360` |
| 国コード | エンドユーザーの国コード。 | `xdm.placeContext.geo.countryCode` | `US` |
| 都道府県 | エンドユーザーの都道府県コード。 | `xdm.placeContext.geo.stateProvince` | `CA` |
| 緯度 | エンドユーザーの場所の緯度。 | `xdm.placeContext.geo._schema.latitude` | `37.3307447` |
| 経度 | エンドユーザーの場所の経度。 | `xdm.placeContext.geo._schema.longitude` | `-121.8945965` |

{style="table-layout:auto"}


### タイムスタンプ

The `timestamp` キーワードは、イベントのタイムスタンプに関する情報を収集します。 コンテキストのこの部分は削除できません。

| ディメンション | 説明 | XDM パス | 値の例 |
| --- | --- | --- | --- |
| イベントのタイムスタンプ | エンドユーザーの UTC タイムスタンプ（簡易拡張） [ISO 8601](https://datatracker.ietf.org/doc/html/rfc3339#section-5.6) 形式を使用します。 | `xdm.timestamp` | `2019-08-07T22:47:17.129Z` |

{style="table-layout:auto"}

### 実装の詳細

The `implementationDetails` キーワードは、イベントの収集に使用された SDK バージョンに関する情報を収集します。

| ディメンション | 説明 | XDM パス | 値の例 |
| --- | --- | --- | --- |
| 名前 | ソフトウェア開発キット (SDK) の識別子。 このフィールドでは、URI を使用で、異なるソフトウェアライブラリで提供される ID 間の一意性を改善します。 | `xdm.implementationDetails.name` | スタンドアロンライブラリを使用する場合、値は `https://ns.adobe.com/experience/alloy`. ライブラリをタグ拡張の一部として使用する場合、値は `https://ns.adobe.com/experience/alloy+reactor`. |
| バージョン | ソフトウェア開発キット (SDK) のバージョン。 | `xdm.implementationDetails.version` | スタンドアロンライブラリを使用する場合、値はライブラリのバージョンになります。 ライブラリをタグ拡張の一部として使用する場合、値はライブラリバージョンになり、タグ拡張バージョンは `+`. 例えば、ライブラリのバージョンが `2.1.0` タグ拡張のバージョンは、 `2.1.3`の場合、値は `2.1.0+2.1.3`. |
| 環境 | データが収集された環境。 これは常にに設定されています。 `browser`. | `xdm.implementationDetails.environment` | `browser` |


### 高エントロピーのクライアントヒント

The `"highEntropyUserAgentHints"` keyword は、ユーザーのデバイスに関する詳細情報を収集します。 このデータは、Adobeに送信されるリクエストの HTTP ヘッダーに含まれます。 Edge ネットワーク内にデータが到達すると、XDM オブジェクトはそれぞれの XDM パスに設定されます。 XDM のパスを `sendEvent` を呼び出すと、HTTP ヘッダー値よりも優先されます。

デバイスを検索する場合は、 [データストリームの設定](/help/datastreams/configure.md)の場合、データは消去され、デバイスのルックアップ値が優先されます。 一部のクライアントヒントフィールドとデバイス参照フィールドが同じヒットに存在しない。

| ディメンション | 説明 | HTTP ヘッダー | XDM パス | 値の例 |
| --- | --- | --- | --- | --- |
| オペレーティングシステムのバージョン | オペレーティングシステムのバージョン。 | `Sec-CH-UA-Platform-Version` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.platformVersion` | |
| アーキテクチャ | 基盤となる CPU アーキテクチャ。 | `Sec-CH-UA-Arch` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.architecture` | |
| デバイスモデル | 使用するデバイスの名前。 | `Sec-CH-UA-Model` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.model` | |
| ビット数 | 基盤となる CPU アーキテクチャがサポートするビット数。 | `Sec-CH-UA-Bitness` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.bitness` | |
| ブラウザーベンダー | ブラウザーを作成した会社。 低エントロピーのヒント `Sec-CH-UA` は、この要素も収集します。 | `Sec-CH-UA-Full-Version-List` | | |
| ブラウザー名 | 使用したブラウザー。 低エントロピーのヒント `Sec-CH-UA` は、この要素も収集します。 | `Sec-UA-Full-Version-List` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.brand` | |
| ブラウザーのバージョン | ブラウザーの重要なバージョン。 低エントロピーのヒント `Sec-CH-UA` は、この要素も収集します。 正確なブラウザーバージョンは、自動的には収集されません。 | `Sec-UA-Full-Version-List` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.version` | |

{style="table-layout:auto"}

## Web SDK タグ拡張機能を使用してコンテキスト情報を収集する

コンテキスト情報設定は、ラジオボタンとチェックボックスの組み合わせで、 [タグ拡張の設定](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md). 各チェックボックスは、コンテキストキーワードにマッピングされます。

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL 拡張機能]**&#x200B;を選択し、次に **[!UICONTROL 設定]** の [!UICONTROL Adobe Experience Platform Web SDK] カード。
1. 下にスクロールして、 [!UICONTROL データ収集] 「 」セクションで、「 」を選択します。 **[!UICONTROL すべてのデフォルトのコンテキスト情報]** または **[!UICONTROL 特定のコンテキスト情報]**.
1. 次を選択した場合、 **[!UICONTROL 特定のコンテキスト情報]**&#x200B;で、必要な各コンテキスト情報要素の横にあるチェックボックスをオンにします。
1. クリック **[!UICONTROL 保存]**&#x200B;をクリックし、変更を公開します。

## Web SDK JavaScript ライブラリを使用したコンテキスト情報の収集

を設定します。 `context` 実行時の文字列の配列 `configure` コマンドを使用します。 SDK を設定する際にこのプロパティを省略した場合、以下を除くすべてのコンテキスト情報 `"highEntropyUserAgentHints"` はデフォルトで収集されます。 高エントロピーのクライアントヒントを収集する場合、またはデータ収集から他のコンテキスト情報を省略する場合は、このプロパティを設定します。 文字列は、任意の順序で含めることができます。

>[!NOTE]
>
>高エントロピーのクライアントヒントを含むすべてのコンテキスト情報を収集する場合は、 `context` 配列文字列。 デフォルト `context` 値の省略 `highEntropyUserAgentHints`、および `context` プロパティを省略した場合、データは収集されません。

```js
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "context": ["web", "device", "environment", "placeContext", "highEntropyUserAgentHints"]
});
```
