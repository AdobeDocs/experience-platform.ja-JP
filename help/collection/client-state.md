---
title: クライアント状態の管理
description: Adobe Experience Platform Edge Network によるクライアント状態の管理方法を説明します
seo-description: Learn how the Adobe Experience Platform Edge Network  manages client state
keywords: クライアント;状態;管理;edge;network;ゲートウェイ;API
exl-id: 798ecc52-1af1-4480-a2a3-3198a83538f8
source-git-commit: 1ab1c269fd43368e059a76f96b3eb3ac4e7b8388
workflow-type: tm+mt
source-wordcount: '848'
ht-degree: 100%

---

# クライアント状態の管理

Edge Network 自体はステートレスであり、独自のセッションを維持しません。ただし、次のように、クライアントサイドの状態を保持することが必要なユースケースもあります。

* 一貫したデバイス識別（[訪問者特定](visitor-identification.md)を参照してください）
* ユーザー同意の収集と強制
* パーソナライゼーションセッション ID の保持

Edge Network は状態管理プロトコルを使用することで、ストレージの要素をクライアント／SDKに委任し、状態についてのエントリを応答に含めます。ブラウザーの場合、これらのエントリは cookie として保存されます。

クライアントの責任は、それらを保存し、後続のすべてのリクエストに含めることです。クライアントは、ゲートウェイの指示に従って、エントリの有効期限を適切に管理する必要もあります。エントリが cookie として保存された場合、ブラウザーはこの処理をすべて自動的に実行します。

状態エントリには常にプレーンな `String` 値（呼び出し元や SDK から見えます）がありますが、値を使用したり、値を改ざんしたりしないでください。値の構造や形式、名前自体はいつでも変更される可能性があり、その状態を内部的に使用するクライアントでは予期しない動作が発生する可能性があります。この状態は、常にゲートウェイ自身または他のエッジサービスによって使用されることを意図しています。

## クライアントの状態をメタデータとして保持する

[!DNL Edge Network] が応答本文で返す状態は、`state:store` 型の `Handle` オブジェクトです。

```json
{
   "requestId":"421036b3-a7ff-480b-a9ab-30adba6eb4f0",
   "handle":[
      {
         "payload":[
            {
               "key":"kndctr_53A16ACB5CC1D3760A495C99_AdobeOrg_consent_check",
               "value":"1",
               "maxAge":7200,
               "attrs":{
                  "SameSite":"None"
               }
            },
            {
               "key":"kndctr_53A16ACB5CC1D3760A495C99_AdobeOrg_identity",
               "value":"CiY1NDc1ODIxNzIzODk5MDY5MzQzMTIzNjQ1NTczNzExNjE4OTA1MFINCLGOvszNLhABGAEgBKABsY6-zM0uqAGHz-z2y82cul3wAbGOvszNLg==",
               "maxAge":34128000,
               "attrs":{
                  "SameSite":"None"
               }
            },
            {
               "key":"kndctr_53A16ACB5CC1D3760A495C99_AdobeOrg_consent",
               "value":"general=in",
               "maxAge":15552000,
               "attrs":{
                  "SameSite":"None"
               }
            }
         ],
         "type":"state:store"
      }
   ]
}
```

| 属性 | タイプ | 説明 |
| --- | --- | --- |
| `key` | 文字列 | **必須**。エントリ名。 |
| `value` | 文字列 | *オプション*。エントリ値。 |
| `maxAge` | 整数 | *オプション* エントリの有効期間（TTL）（秒単位）。ない場合、エントリを現在のセッションに対してのみ保存する必要があります。 |
| `attrs` | `Map<String, String>` | *オプション*。エントリ属性のオプションのリスト。セキュアリファラー HTTP ヘッダーを使用するセキュリティで保護されたすべての接続の場合、`SameSite` 属性を `None` に設定します。 |


複数のタグ付けでは、同じプロパティ内の複数の SDK インスタンスが異なる組織を参照する可能性があります。すべての状態エントリには、複数のタグ付けをサポートするために、`kndctr_` と URL で使用できる組織 ID のプレフィックスが自動的に付きます。

クライアント SDK が、応答で `state:store` ハンドルを受信した場合、次の処理を行う必要があります。

* ゲートウェイから提供された有効期限を考慮して、クライアントサイドでエントリを格納します。
* クライアントストアからエントリを読み込み、期限が切れていないエントリを後続のリクエストにすべて含めます。

クライアントサイドで保存された状態で渡されるリクエストの例を次に示します。

```json
{
   "meta":{
      "state":{
         "entries":[
            {
               "key":"kndctr_53A16ACB5CC1D3760A495C99_AdobeOrg_consent_check",
               "value":"1"
            },
            {
               "key":"kndctr_53A16ACB5CC1D3760A495C99_AdobeOrg_personalization_sessionId",
               "value":"0a88f43e-044b-41a6-a4f3-6c2848bbc672"
            }
         ]
      }
   }
}
```

## ブラウザー cookie にクライアント状態を保持する

ブラウザークライアントを操作する場合、Edge Network は、エントリをブラウザー cookie として自動的に保持できます。これにより、ブラウザーはデフォルトで状態管理プロトコルに従うので、透過的な状態のストレージをサポートできます。

ほとんどのエントリは、有効でサポートされている場合、ファーストパーティ cookie として実体化されますが（以下の注を参照）、サードパーティの `adobedc.demdex.net` ドメインを使用すると、ゲートウェイでも一部のサードパーティ cookie を格納することができます。

エントリは、定義上、常に特定のスコープ（デバイスやアプリケーション）に結び付けられるので、現在のリクエストコンテキストと互換性のあるサブセットのみが Edge Network によって書き込まれます。書き込まれていないエントリは、
`state:store` ハンドルで返されます。

原則として、アプリケーションスコープのエントリは常にファーストパーティ cookie として書き込まれ、デバイススコープのエントリはサードパーティ cookie として書き込まれます。判定は呼び出し元に対して完全に透過的であり、ゲートウェイは呼び出しコンテキストに応じて、どのエントリを書き込むかを決定します。

呼び出し元は、`meta.state.cookiesEnabled` フラグを使用して、クライアントの状態を cookie として保存するサポートを明示的に有効にする必要があります。

```json
{
   "meta":{
      "state":{
         "cookiesEnabled":true,
         "domain":"foo.com"
      }
   }
}
```

| 属性 | タイプ | 説明 |
| --- | --- | --- |
| `cookiesEnabled` | ブール値 | 設定すると、cookie のサポートが有効になります。デフォルト値は `false` です。 |
| `domain` | 文字列 | 必須`cookiesEnabled: true` の場合。Cookie を書き込むトップレベルドメイン。Edge Network はこの値を使用して、状態を cookie で保持できるかどうかを判定します。 |

`cookiesEnabled` フラグで cookie サポートが有効になっていても、Adobe Experience Platform Edge Network は、リクエストのトップレベルドメインが呼び出し元によって指定された `domain` に一致する場合にのみ、状態エントリを書き込みます。不一致がある場合、エントリは `state:store` ハンドルで返されます。

次の場合は、サポートが有効になっている場合でも、ファーストパーティ cookie を書き込むことができません。

* サードパーティの `adobedc.demdex.net` ドメインからのリクエスト。
* ファーストパーティの `CNAME` ドメインからのリクエストだが、呼び出し元が `meta.state.domain` で指定したドメインと異なる。

## Cookie のセキュリティ

すべての cookie は、可能な限り[セキュアフラグ](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Cookies#restrict_access_to_cookies)を有効にします。

すべてのセキュア cookie は [SameSite 属性](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/Set-Cookie/SameSite) が `None` に設定されており、ファーストパーティとクロスオリジンの両方のすべてのコンテキストで cookie が送信されます。

* ファーストパーティ Cookie の場合（`kndcrt_*`）、リクエストコンテキストがセキュア（HTTPS）で、かつリファラー（[リファラー HTTP ヘッダー](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/Referer)）も HTTPS の場合のみ、`Secure` フラグが設定されます。リファラーがセキュアではない場合（HTTP）、`Secure` フラグは省略され、Web SDK で読み取れるようになります。セキュア cookie は、安全ではないコンテキストから読み取ることはできません。
* サードパーティ Cookie の場合（demdex）、`Secure` フラグは常に設定されます。これは、すべてのリクエストが HTTPS なので、リクエストコンテキストはセキュリティで保護され、この cookie は JavaScript から読み取られないからです。

`Secure` フラグは、[cookie のメタデータ表示枠](#state-as-metadata) 内には存在しません。`SameSite` 属性のみが含まれます。この場合、`SameSite` 属性が存在するときは常に `Secure` フラグを正しく設定するのはクライアントの責任です。`SameSite=None` の cookie は、セキュアなコンテキスト（HTTPS）を必要とするため、`Secure` 属性も設定する必要があります。
