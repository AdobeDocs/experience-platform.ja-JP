---
title: モバイルアプリからモバイル web/Web ビューへのIDの共有
description: モバイルアプリからモバイル web コンテンツやweb ビューにIDを渡し、web コンテキストでレポートとパーソナライゼーションを続行できます。
source-git-commit: bf0bb72777cacd822fd6e887ac3ef71764784214
workflow-type: tm+mt
source-wordcount: '525'
ht-degree: 0%

---

# モバイルアプリからモバイル web/Web ビューへのIDの共有

訪問者がモバイルアプリからWebViewまたはモバイル web ページに移動すると、アプリとweb コンテキストはそれぞれ独自のIDを維持します。 明示的な引き継ぎがなければ、web体験では訪問者を未知の新しい人物として扱い、レポートをフラグメント化し、パーソナライゼーションを再開します。

モバイルからwebへのID共有では、訪問者の[Experience Cloud ID （ECID） &#x200B;](./overview.md)をモバイルアプリから`adobe_mc` クエリ文字列パラメーターを介してweb宛先に渡すことにより、これを解決します。 パラメーターには、ECID、Experience Cloud組織ID、およびタイムスタンプが含まれます。 Web宛先が有効な`adobe_mc` パラメーターで読み込まれると、Web SDKはそれを自動的に読み取り、最初のEdge Network リクエストにハンドオフ IDを適用するため、両方のコンテキストで同じ訪問者が共有されます。

このパターンは、組織が管理するWebViewまたはモバイル web ページをモバイルアプリで開き、アプリのアクティビティとweb アクティビティを同じ訪問者に関連付けたままにする場合に使用します。 目標が異なるドメイン上のweb サイト間のID継続性である場合は、代わりに[&#x200B; クロスドメイン共有](cross-domain-sharing.md)を使用してください。

## 前提条件

開始する前に、実装が次の要件を満たしていることを確認してください。

* **モバイルアプリ**: Adobe Experience Platform Mobile SDKと[Identity for Edge Network](https://developer.adobe.com/client-sdks/edge/identity-for-edge-network/)拡張機能バージョン **1.1.0以降** （iOSおよびAndroid）。
* **Webの宛先**: [Web SDK](/help/collection/js/js-overview.md) バージョン **2.11.0以降**、またはWeb SDK タグ拡張機能。
* **URL コントロール**: コードは、アプリがWebViewまたはブラウザーに渡すURLを制御し、クエリ文字列パラメーターを追加できるようにします。
* **一致する設定**：同じExperience Cloud組織IDがモバイル実装とweb実装の両方で設定されています。

## モバイルアプリからIDを取得 {#retrieve-identity}

Edge Network用ID拡張機能の[`getUrlVariables`](https://developer.adobe.com/client-sdks/edge/identity-for-edge-network/api-reference/#geturlvariables) APIを使用して、訪問者のIDをクエリ文字列として取得します。 その後、WebViewまたはブラウザーを開く前に、その文字列をURLに追加できます。

返される文字列には、次のURL エンコードされたパラメーターが含まれます。

| パラメーター | 説明 |
| --- | --- |
| `MCID` | Experience Cloud ID （ECID）。 |
| `MCORGID` | Experience Cloud組織ID。 このパラメーターは、宛先ページのWeb SDKで設定された組織と一致する必要があります。 |
| `TS` | タイムスタンプ。 宛先は、**5分以内にこの値を受け取る必要があります**。そうしないと、ハンドオフが拒否されます。 |

次のコード例は、モバイルアプリでの引き継ぎがどのように表示されるかを示しています。

>[!BEGINTABS]

>[!TAB Swift （iOS） ]

```swift
Identity.getUrlVariables { (urlVariables, error) in
    if let error = error {
        // Handle the error
        return
    }

    guard let urlVariables = urlVariables else { return }

    // Construct the full URL by appending the identity query string
    if let url = URL(string: "https://example.com/webapp?\(urlVariables)") {
        // Open the URL in a WebView or browser
        let request = URLRequest(url: url)
        webView.load(request)
    }
}
```

>[!TAB Kotlin （Android） ]

```kotlin
Identity.getUrlVariables { urlVariables ->
    if (urlVariables != null) {
        // Construct the full URL by appending the identity query string
        val url = "https://example.com/webapp?$urlVariables"

        // Open the URL in a WebView or browser
        webView.loadUrl(url)
    }
}
```

>[!ENDTABS]

## Web側でIDを受信する {#receive-identity}

Web宛先に追加のコードは必要ありません。 Web SDKがページに存在し、URLに有効な`adobe_mc` パラメーターが含まれている場合、SDKはECIDを自動的に抽出し、最初のEdge Network リクエストで訪問者のID マップに適用します。

Web宛先でWeb SDK タグ拡張機能を使用しており、IDを保持しながら訪問者を別のページにリダイレクトする必要がある場合は、[IDでリダイレクト &#x200B;](/help/tags/extensions/client/web-sdk/actions/redirect-with-identity.md) アクションを使用して、`adobe_mc` パラメーターを次のページに転送します。

>[!NOTE]
>
>`adobe_mc` パラメーターは、**5分**&#x200B;後に有効期限が切れます。 URLを開いた後、Web宛先が最初のEdge Network リクエストを読み込んで送信することを確認します。
