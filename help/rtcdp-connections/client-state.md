---
title: クライアント状態の管理
description: Adobe Experience Platform Edge Network によるクライアント状態の管理方法を説明します
seo-description: Learn how the Adobe Experience Platform Edge Network  manages client state
keywords: クライアント；状態；管理；エッジ；ネットワーク；ゲートウェイ；API
exl-id: 798ecc52-1af1-4480-a2a3-3198a83538f8
source-git-commit: 0a01dd2b0d8a1039178e3593475f9a87639ccdcd
workflow-type: tm+mt
source-wordcount: '848'
ht-degree: 3%

---

# クライアント状態の管理

Edge Network 自体はステートレスです（独自のセッションを維持しません）。 ただし、次のようなクライアント側の状態持続性が必要な使用例もあります。

* 一貫したデバイス識別 ( [訪問者特定](visitor-identification.md))
* ユーザーの同意を収集して実施する
* パーソナライゼーションセッション ID の保持

Edge Network は、状態管理プロトコルを使用し、ストレージ要素をクライアント/SDK に委任し、応答に状態エントリを含めます。 ブラウザーの場合、エントリは cookie として保存されます。

クライアントの責任は、それらを保存し、後続のすべてのリクエストに含めることです。 また、クライアントは、ゲートウェイの指示に従って、エントリに対して適切な有効期限を設定する必要があります。 エントリが cookie として保存された場合、ブラウザーはこの処理をすべて自動的に実行します。

状態エントリは常にプレーンを持ちます `String` の値（呼び出し元や SDK に表示）の場合、値を使用したり、値を改ざんしたりしないでください。 値の structure/format または名前自体はいつでも変更される可能性があり、内部的に状態を使用するクライアントで予期しない動作が発生する可能性があります。 この状態は、常にゲートウェイ自体または他のエッジサービスによって使用されることを意図しています。

## クライアントの状態をメタデータとして保持する

が返す状態 [!DNL Edge Network] 応答本文には、 `Handle` 型のオブジェクト `state:store`.

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
| `maxAge` | 整数 | *オプション* エントリの有効期間 (TTL)（秒単位）。 見つからない場合、エントリは現在のセッションに対してのみ保存する必要があります。 |
| `attrs` | `Map<String, String>` | *オプション*。エントリ属性のオプションのリスト。 セキュアリファラー HTTP ヘッダーを使用するセキュリティで保護されたすべての接続の場合、 `SameSite` 属性が `None`. |


複数のタグ付けをサポートする（同じプロパティ内の複数の SDK インスタンスが異なる組織を参照している可能性がある）ために、すべての状態エントリには、自動的にプレフィックスが付きます。 `kndctr_` URL で使用できる組織 ID。

クライアント SDK が `state:store` 応答でを処理するには、次の操作をおこなう必要があります。

* ゲートウェイが提供する有効期限を考慮して、クライアント側のエントリを格納します。
* クライアントストアから読み込み、期限切れでないエントリをすべて後続のリクエストに含めます。

クライアント側で保存された状態で渡されるリクエストの例を次に示します。

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

ブラウザークライアントを操作する場合、Edge ネットワークは、エントリをブラウザー cookie として自動的に保持できます。 これにより、ブラウザーはデフォルトで状態管理プロトコルを尊重するので、透過的な状態ストレージのサポートが可能になります。

ほとんどのエントリは、有効でサポートされている場合、ファーストパーティ cookie として実体化されます（以下の注意を参照）が、サードパーティ cookie を格納する場合は、ゲートウェイも一部のサードパーティ cookie を格納できます `adobedc.demdex.net` ドメインが使用されます。

エントリは、定義によって常に特定のスコープ（デバイス/アプリケーション）に結び付けられるので、現在のリクエストコンテキストと互換性のあるサブセットのみが Edge ネットワークによって書き込まれます。 書き込まれていないエントリは、 `state:store` ハンドル。

原則として、アプリケーションスコープエントリは常にファーストパーティ cookie として書き込まれ、デバイススコープエントリはサードパーティ cookie として書き込まれます。 判定は呼び出し元に対して完全に透過的で、ゲートウェイは呼び出しコンテキストに応じて、どのエントリを書き込むかを決定します。

呼び出し元は、 `meta.state.cookiesEnabled` フラグ：

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
| `cookiesEnabled` | ブール値 | 設定すると、cookie のサポートが有効になります。 デフォルト値は `false` です。 |
| `domain` | 文字列 | 必須 when `cookiesEnabled: true`. Cookie を書き込む最上位ドメイン。 Edge Network はこの値を使用して、状態を cookie として保持できるかどうかを決定します。 |

( `cookiesEnabled` フラグを指定した場合、Adobe Experience Platform Edge Network は、リクエストの最上位ドメインが `domain` 呼び出し元によって指定されます。 不一致がある場合、エントリは `state:store` ハンドル。

次の場合は、（サポートが有効になっている場合でも）ファーストパーティ cookie を書き込むことができません。

* リクエストはサードパーティでおこなわれています `adobedc.demdex.net` ドメイン。
* リクエストはファーストパーティで受信されています `CNAME` ドメイン ( `meta.state.domain`.

## cookie のセキュリティ

すべての cookie に [セキュアフラグ](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies#restrict_access_to_cookies) 可能な限り有効にします。

セキュリティで保護されているすべての cookie に [SameSite 属性](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie/SameSite) に設定 `None`：つまり、すべてのコンテキストで、ファーストパーティとクロスオリジンの両方で cookie が送信されます。

* ファーストパーティ Cookie の場合 (`kndcrt_*`)、 `Secure` フラグは、リクエストコンテキストがセキュア (HTTPS) で、かつリファラー ([リファラー HTTP ヘッダー](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referer)) は HTTPS でもあります。 リファラーがセキュアでない場合 (HTTP)、 `Secure` フラグが省略され、Web SDK が読み取れるようになります。 安全な Cookie を安全でないコンテキストから読み取ることはできません。
* サードパーティ Cookie(demdex) の場合、 `Secure` フラグは常に設定されます。これは、すべてのリクエストが HTTPS なので、リクエストコンテキストはセキュリティで保護され、この cookie は JavaScript から読み取られないからです。

この `Secure` フラグが [cookie のメタデータ表現](#state-as-metadata). 次の項目のみ `SameSite` 属性が含まれます。 この場合、 `Secure` 常にフラグを付ける `SameSite` 属性が存在する。 Cookie の `SameSite=None` また、 `Secure` 属性を設定する必要があります。
