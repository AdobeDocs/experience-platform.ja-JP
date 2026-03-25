---
title: コンテキスト
description: デバイス、環境、位置情報などのデータを自動的に収集。
exl-id: 911cabec-2afb-4216-b413-80533f826b0e
source-git-commit: 9f7464b78da9615bf6966e34eb129150a481fb5f
workflow-type: tm+mt
source-wordcount: '1017'
ht-degree: 12%

---

# `context`

`context` プロパティは、Web SDKが自動的に収集できる内容を決定する文字列の配列です。 これらのデータは大きな価値を提供しますが、こうしたデータを一部省略することで有益な結果を得ることができ、組織のプライバシーポリシーに準拠できるようになります。

## コンテキストキーワードとXDM要素

特定のコンテキストキーワードを含めると、Web SDKは、関連するすべてのXDM要素を自動的に入力します。 他のユーザーを許可する間に特定のXDM要素を省略する場合は、[`onBeforeEventSend`](onbeforeeventsend.md)を使用して値をクリアできます。 1つのページで複数のイベントを送信する場合、Web SDKには、すべての`SendEvent`呼び出しにこれらのフィールドが含まれます。

### Web

`"web"` キーワードは、現在のページに関する情報を収集します。

| ディメンション | 説明 | XDM パス | 値の例 |
| --- | --- | --- | --- |
| ページ URL | 現在のページの URL。 | `xdm.web.webPageDetails.URL` | `https://example.com/index.html` |
| リファラー URL | 前に訪問したページの URL。 | `xdm.web.webReferrer.URL` | `http://example.org/linkedpage.html` |

### デバイス

`"device"` キーワードは、ユーザーのデバイスに関する情報を収集します。

| ディメンション | 説明 | XDM パス | 値の例 |
| --- | --- | --- | --- |
| 画面の高さ | 画面の高さ（ピクセル単位）。 | `xdm.device.screenHeight` | `900` |
| 画面の幅 | 画面の幅（ピクセル単位）。 | `xdm.device.screenWidth` | `1440` |
| 画面の向き | 画面の向き。 | `xdm.device.screenOrientation` | `landscape` または `portrait` |

### 環境

`"environment"` キーワードは、ユーザーのブラウザーに関する情報を収集します。

| ディメンション | 説明 | XDM パス | 値の例 |
| --- | --- | --- | --- |
| 環境タイプ | エクスペリエンスが表示された環境のタイプ。 Web SDKは、このフィールドを常に`browser`に設定します。 | `xdm.environment.type` | `browser` |
| ビューポートの高さ | ブラウザーのコンテンツ領域の高さ（ピクセル単位）。 | `xdm.environment.browserDetails.viewportHeight` | `679` |
| ビューポートの幅 | ブラウザーのコンテンツ領域の幅（ピクセル単位）。 | `xdm.environment.browserDetails.viewportWidth` | `642` |

### 場所の背景

`"placeContext"` キーワードは、ユーザーの場所に関する情報を収集します。

| ディメンション | 説明 | XDM パス | 値の例 |
| --- | --- | --- | --- |
| 現地時間 | エンドユーザーのローカルタイムスタンプ （簡略化された拡張[ISO 8601](https://datatracker.ietf.org/doc/html/rfc3339#section-5.6)形式）。 | `xdm.placeContext.localTime` | `YYYY-08-07T15:47:17.129-07:00` |
| ローカルタイムゾーンのオフセット | ユーザーがGMTからオフセットされる分数。 | `xdm.placeContext.localTimezoneOffset` | `360` |
| 国コード | エンドユーザーの国コード。 | `xdm.placeContext.geo.countryCode` | `US` |
| 州都 | エンドユーザーの都道府県コード。 | `xdm.placeContext.geo.stateProvince` | `CA` |
| 緯度 | エンドユーザーの位置の緯度。 | `xdm.placeContext.geo._schema.latitude` | `37.3307447` |
| 経度 | エンドユーザーの場所の経度。 | `xdm.placeContext.geo._schema.longitude` | `-121.8945965` |
| IANA タイムゾーン | エンドユーザーのIANA タイムゾーン。 ライブラリのバージョン 2.32.0以降に含まれています。 | `xdm.placeContext.ianaTimezone` | `America/Denver` |

### タイムスタンプ

`"timestamp"` キーワードは、イベントのタイムスタンプに関する情報を収集します。 このコンテキストは常に含まれ、削除できません。

| ディメンション | 説明 | XDM パス | 値の例 |
| --- | --- | --- | --- |
| イベントのタイムスタンプ | エンドユーザーのUTC タイムスタンプ （簡易拡張[ISO 8601](https://datatracker.ietf.org/doc/html/rfc3339#section-5.6)形式）。 | `xdm.timestamp` | `YYYY-08-07T22:47:17.129Z` |

### 実装の詳細

`implementationDetails` キーワードは、イベントの収集に使用されたSDK バージョンに関する情報を収集します。

| ディメンション | 説明 | XDM パス | 値の例 |
| --- | --- | --- | --- |
| 名前 | ソフトウェア開発キット （SDK） ID。 このフィールドでは、URI を使用で、異なるソフトウェアライブラリで提供される ID 間の一意性を改善します。 | `xdm.implementationDetails.name` | スタンドアロン ライブラリを使用する場合、値は`https://ns.adobe.com/experience/alloy`です。 ライブラリがタグ拡張機能の一部として使用されている場合、値は`https://ns.adobe.com/experience/alloy+reactor`です。 |
| バージョン | ソフトウェア開発キット（SDK）のバージョン。 | `xdm.implementationDetails.version` | スタンドアロンライブラリを使用する場合、値はライブラリバージョンです。 ライブラリがタグ拡張機能の一部として使用されている場合、値はライブラリ バージョンとタグ拡張機能のバージョンが`+`と結合されています。 例えば、ライブラリのバージョンが`2.1.0`、タグ拡張機能のバージョンが`2.1.3`の場合、値は`2.1.0+2.1.3`になります。 |
| 環境 | データが収集された環境。 JavaScript ライブラリを使用する場合、このフィールドは常に`browser`に設定されます。 | `xdm.implementationDetails.environment` | `browser` |

### 高いエントロピーのクライアントヒント {#high-entropy-client-hints}

`"highEntropyUserAgentHints"` キーワードは、ユーザーのデバイスに関する詳細情報を収集します。 このデータは、Adobeに送信されるリクエストのHTTP ヘッダーに含まれます。 データがEdge ネットワークに到着すると、XDM オブジェクトはそれぞれのXDM パスに入力されます。 `sendEvent`呼び出しでそれぞれのXDM パスを設定した場合は、HTTP ヘッダーの値よりも優先されます。

データストリームを[設定](/help/datastreams/configure.md)するときにデバイス参照を使用する場合、データを消去してデバイス参照値を優先できます。 一部のクライアントヒントフィールドとデバイス参照フィールドは、同じヒットに存在できません。

| プロパティ | 説明 | HTTP ヘッダー | XDM パス | 例 |
| --- | --- | --- | --- | --- |
| オペレーティングシステムのバージョン | オペレーティングシステムのバージョン。 | `Sec-CH-UA-Platform-Version` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.platformVersion` | `10.15.7` |
| アーキテクチャ | CPUアーキテクチャ。 | `Sec-CH-UA-Arch` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.architecture` | `x86` |
| デバイスモデル | 使用するデバイスの名前。 | `Sec-CH-UA-Model` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.model` | `Intel Mac OS X 10_15_7` |
| ビット数 | 基礎となるCPU アーキテクチャがサポートするビット数。 | `Sec-CH-UA-Bitness` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.bitness` | `64` |
| ブラウザーベンダー | ブラウザーを作成した会社。 低エントロピーのヒント `Sec-CH-UA`もこの要素を収集します。 | `Sec-CH-UA-Full-Version-List` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.vendor` | `Google` |
| ブラウザー名 | 使用するブラウザー。 低エントロピーのヒント `Sec-CH-UA`もこの要素を収集します。 | `Sec-UA-Full-Version-List` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.brand` | `Chrome` |
| ブラウザーバージョン | ブラウザーの重要なバージョン。 低エントロピーのヒント `Sec-CH-UA`もこの要素を収集します。 正確なブラウザーバージョンは自動的には収集されません。 | `Sec-UA-Full-Version-List` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.version` | `105` |

詳しくは、[ ユーザーエージェントクライアントヒント ](/help/collection/use-cases/client-hints.md)を参照してください。

### 1回限りのAnalytics リファラー {#one-time-analytics-referrer}

`"oneTimeAnalyticsReferrer"` キーワードは、ページの最初の非決定`sendEvent`呼び出しでのみ、Adobe Analyticsにリファラー値を送信します。 このコンテキストキーワードの主な使用例は、AnalyticsとTargetの統合で主に使用されるヒットによって、Adobe Analyticsの[Referrer](https://experienceleague.adobe.com/en/docs/analytics/components/dimensions/referrer) ディメンションが膨らまるのを防ぐことです。

特定の`sendEvent` コマンドで決定イベント型（`decisioning.propositionFetch`、`decisioning.propositionDisplay`、`decisioning.propositionInteract`）が使用されている場合、ページの最初の`sendEvent`を計算する際に無視されます。 リファラー値がページ上で変更され、別の`sendEvent`がトリガーされた場合、新しいリファラー値がペイロードに含まれます。 この条件を使用すると、この機能をシングルページアプリケーションで使用できます。

重複するリファラー値が検出されると、ライブラリは`data.__adobe.analytics.referrer`を空の文字列（`""`）に設定します。
このデータオブジェクトフィールドを空の文字列に設定すると、ヒットがAdobe Analyticsに到達したときに値が実質的にクリアされます。これは、データオブジェクトがXDM オブジェクトに相当するフィールドを上書きするためです。 XDM オブジェクトには影響しません。データストリームに複数のサービスを含める場合は、そのデータをExperience Platform データセットに引き続き送信できます。

## 実装

`context` コマンドの実行時に、文字列の`configure`配列を設定します。 SDKの設定時にこのプロパティを省略すると、`"highEntropyUserAgentHints"`と`"oneTimeAnalyticsReferrer"`を除くすべてのコンテキスト情報がデフォルトで収集されます。 高エントロピーのクライアントヒントを収集する場合や、データ収集から他のコンテキスト情報を省略する場合は、このプロパティを設定します。 文字列は任意の順序で含めることができます。

>[!TIP]
>
>高エントロピーのクライアントヒントを含むすべてのコンテキスト情報を収集する場合は、`context`配列文字列にすべての値を含める必要があります。 デフォルトの`context`値では`"highEntropyUserAgentHints"`と`"oneTimeAnalyticsReferrer"`が省略されます。`context` プロパティを設定した場合、省略された値はデータを収集しません。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  context: ["web", "device", "environment", "placeContext", "highEntropyUserAgentHints", "oneTimeAnalyticsReferrer"]
});
```

## Web SDK タグ拡張機能を使用してコンテキスト情報を収集する

Web SDK タグ拡張機能ドキュメントのData Collection Configuration Settingsの[Context settings](/help/tags/extensions/client/web-sdk/configure/data-collection.md#context-settings)を参照してください。
