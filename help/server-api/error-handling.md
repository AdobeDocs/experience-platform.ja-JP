---
title: エラー処理
description: Adobe Experience Platform Edge Networkサーバー API への API リクエストを実行する際に発生する可能性のあるエラーについて説明します。
exl-id: f6b8435c-b163-4046-b5fb-50a13a897637
source-git-commit: 3bf13c3f5ac0506ac88effc56ff68758deb5f566
workflow-type: tm+mt
source-wordcount: '766'
ht-degree: 9%

---

# エラー処理

## 概要 {#overview}

Adobe Experience Platform Edge Networkサーバー API の API エラーには、内部（Edge Network自体）または外部（入力、設定、アップストリーム関連）など、様々な原因が考えられます。

## エラータイプ {#error-types}

| エラー | タイプ | 説明 | ステータスコード |
| --- | --- | --- | --- |
| `RequestProcessingError` | 内部 | 予期しない状況でAdobe Experience Platform Edge Networkにより一般目的のエラーが発生しました。 | `500` |
| `InputError` | 外部 | 不正な入力によって発生したエラーと、エンティティ検証エラーが含まれます。 | `4xx` |
| `ConfigurationError` | 外部 | サーバーサイド設定エラー。 | `422` |
| `UpstreamError` | 外部 | アップストリームサービスとの通信エラー。 | `207 Multi-Status` |

## 重大度

サーバー API エラーは、重大度で分割することもできます。

* **致命的なエラー** により、ディスパッチパイプラインが停止します。
* **致命的でないエラー** は、リクエスト処理の続行を許可している間に、部分的な処理が行われたことを示す場合があります。
   * 存在する場合、リクエストの全体的なステータスコードは `207 Multi-Status` に変更されます。

| エラー | タイプ | 備考 |
| --- | --- | --- |
| `RequestProcessingError` | 致命的 | リクエスト処理中のあらゆる時点で発生する可能性があります。 |
| `InputError` | 致命的 | アップストリームでディスパッチする前に要求を受け入れるときに発生します。 |
| `ConfigurationError` | 致命的 | アップストリームでディスパッチする前に要求を受け入れるときに発生します。 |
| `UpstreamError` | Non-Fatal | アップストリームサービスとの通信エラー。 |

### 致命的なエラー {#fatal-errors}

重大なエラーが発生すると、リクエスト処理が停止し、2xx 以外の応答ステータスが返されます。 [ エラータイプ ](#error-types) の節を確認して、各エラータイプに対応する期待されるステータスコードを確認します。

エラーは、エラーオブジェクトを含む応答本文を伴います。 この場合、応答本文には、[RFC 7807 HTTP API に関する問題の詳細 ](https://tools.ietf.org/html/rfc7807) で定義されている問題の詳細が含まれます。

返される content-type は `application/problem+json` メディアタイプです。 存在する場合、この応答には、エラーに関する機械で読み取り可能な詳細が含まれています。 問題の詳細には、URI タイプが含まれます。

すべてのエラーオブジェクトには `type`、`status`、`title`、`detail` および `report` のメッセージプロパティがあり、API クライアントは問題が何であるかを知ることができます。

| プロパティ | タイプ | 説明 |
| -------- | ------ | ----------- |
| `type` | 文字列 | 形式 `https://ns.adobe.com/aep/errors/<ERROR-CODE>` に従って、問題の種類を識別する URI 参照（RFC3986）。 |
| `status` | 数値 | この問題の発生に対してサーバーによって生成される HTTP ステータスコード。 |
| `title` | 文字列 | 問題タイプの短い、人間が判読可能な概要。 |
| `detail` | 文字列 | 人間が判読できる、問題のタイプに関する短い説明。 |
| `report` | オブジェクト | リクエスト ID や組織 ID など、デバッグに役立つ追加プロパティのマップ。 検証エラーのリストなど、手元のエラーに固有のデータが含まれている場合もあります。 |

```json
{
   "type":"https://ns.adobe.com/aep/errors/EXEG-0104-422",
   "status":422,
   "title":"Unprocessable entity",
   "detail":"Invalid request (report attached). Please check your input and try again.",
   "report":{
      "errors":[
         "Allowed Adobe version is 1.0 for standard 'Adobe' at index 0",
         "Allowed IAB version is 2.0 for standard 'IAB TCF' at index 1",
         "IAB consent string value must not be empty for standard 'IAB TCF' at index 1"
      ],
      "requestId":"0f8821e5-ed1a-4301-b445-5f336fb50ee8",
      "orgId":"53A16ACB5CC1D3760A495C99@AdobeOrg"
   }
}
```

### 致命的でないエラー {#non-fatal-errors}

致命的でないエラーは、さらに次のように分類できます。

* エラー：リクエストの処理中に発生したが、リクエスト全体が拒否されなかった問題（例： 重要でないアップストリームエラー）。
* 警告：リクエストの部分的な処理が発生したことを示す可能性のあるアップストリームサービスのメッセージ。

致命的でないエラー（警告を除く）が発生した場合、[!DNL Server API] は応答ステータスを `207 Multi-Status` に変更します。

一方、警告は通常、一時的な状態を表すため、ほとんどの場合に情報を提供します。これは、リクエストには完全には影響しませんでした。 この例の 1 つは、セグメント化エンジンで読み取られた部分的なプロファイルです。この場合、精度はある程度影響を受けますが、機能は引き続き提供されます。

致命的でないエラーは _Problem Details_ フォーマットで表されますが、Edge ゲートウェイの標準レスポンス（`application/json` 型）に直接埋め込まれます。

```json
{
  "requestId": "72eaa048-207e-4dde-bf16-0cb2b21336d5",
  "handle": [
  ],
  "errors": [
    {
      "type": "https://ns.adobe.com/aep/errors/EXEG-0201-503",
      "status": 503,
      "title": "The 'com.adobe.experience.platform.ode' service is temporarily unable to serve this request. Please try again later."
    }
  ],
  "warnings": [
    {
      "type": "https://ns.adobe.com/aep/errors/EXEG-0204-200",
      "status": 200,
      "title": "A warning occurred while calling the 'com.adobe.audiencemanager' service for this request.",
      "report": {
        "cause": {
          "message": "Cannot read related customer for device id: ...",
          "code": 202
        }
      }
    }
  ]
}
```

## `4xx` と `5xx` の応答の処理

| エラーコード | 説明 |
|---|---|
| `4xx Bad Request` | 400、403、404 など、ほとんどの `4xx` エラーは、`429` を除き、クライアントに代わって再試行しないでください。 これらはクライアントエラーであり、成功しません。 クライアントは、リクエストを再試行する前にエラーに対処する必要があります。 |
| `429 Too Many Requests` | HTTP 応答コード `429`、Adobe Experience Platform Edge Networkまたはアップストリームサービスがリクエストをレート制限していることを示します。 この場合、このようなシナリオでは、呼び出し元は `Retry-After` 応答ヘッダーを尊重する必要があります。 戻ってくる応答には、ドメイン固有のエラーコードなど、HTTP 応答コードが含まれている必要があります。 |
| `500 Internal Server Error` | `500` エラーは一般的かつ包括的なエラーです。`502` と `503` を除き、`500` 件のエラーを再試行しないでください。 仲介者は `500` エラーで応答する必要があり、汎用エラーコード/メッセージ、またはドメイン固有のエラーコード/メッセージで応答する場合があります。 |
| `502 Bad Gateway` | Adobe Experience Platform Edge Networkがアップストリームサーバーから無効な応答を受信したことを示します。 これは、サーバー間のネットワークの問題によって発生する可能性があります。一時的なネットワークの問題が解決され、再試行によって問題が解決される場合があります。その `502`、エラーの受信者は、しばらくしてからリクエストを再試行できます。 |
| `503 Service Unavailable` | このエラーコードは、サービスが一時的に利用できないことを示します。これはメンテナンス期間中に発生する場合があります。`503` エラーの受信者はリクエストを再試行できますが、`Retry-After` ヘッダーに従う必要があります。 |
| `504 Gateway Timeout` | アップストリームサーバーへのAdobe Experience PlatformEdge Networkリクエストがタイムアウトしたことを示します。 これは、サーバー間のネットワークの問題、DNS の問題、その他のネットワークの問題によって発生する可能性があります。 一時的なネットワークの問題は、しばらくすると解決される場合があり、再試行すると問題が解決する場合があります。 |
