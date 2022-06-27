---
title: エラー処理
description: Adobe Experience Platform Edge Network Server API に対して API リクエストを実行する際に発生する可能性のあるエラーについて説明します。
exl-id: f6b8435c-b163-4046-b5fb-50a13a897637
source-git-commit: f52603f7e65ac553e00a2b632857561cd07ae441
workflow-type: tm+mt
source-wordcount: '765'
ht-degree: 3%

---

# エラー処理

## 概要 {#overview}

Adobe Experience Platform Edge Network Server API の API エラーには、内部（Edge Network 自体）、外部（入力、設定、アップストリーム関連）など、様々な原因があります。

## エラータイプ {#error-types}

| エラー | タイプ | 説明 | ステータスコード |
| --- | --- | --- | --- |
| `RequestProcessingError` | 内部からのアクセス | 予期しない状況でAdobe Experience Platform Edge Network が発生する汎用エラー。 | `500` |
| `InputError` | 外部 web アプリケーション | 入力の形式が正しくないことに起因するエラーと、エンティティ検証エラーが含まれます。 | `4xx` |
| `ConfigurationError` | 外部 web アプリケーション | サーバー側の設定エラー。 | `422` |
| `UpstreamError` | 外部 web アプリケーション | アップストリームサービスとの通信エラー。 | `207 Multi-Status` |

## 重大度

Server API のエラーは、次の重大度で分割することもできます。

* **致命的なエラー** がディスパッチパイプラインを停止します。
* **致命的でないエラー** は部分処理を示す一方で、リクエスト処理を続行できるようにします。
   * 存在する場合、リクエストの全体的なステータスコードは `207 Multi-Status`.

| エラー | タイプ | 備考 |
| --- | --- | --- |
| `RequestProcessingError` | 致命的 | リクエスト処理中の任意の時点で発生する可能性があります。 |
| `InputError` | 致命的 | リクエストの受け入れ時に発生します。この時点では、リクエストがアップストリームでディスパッチされます。 |
| `ConfigurationError` | 致命的 | リクエストの受け入れ時に発生します。この時点では、リクエストがアップストリームでディスパッチされます。 |
| `UpstreamError` | 致命的でない | アップストリームサービスとの通信エラー。 |

### 致命的なエラー {#fatal-errors}

致命的なエラーは、リクエストの処理を停止し、2xx 以外の応答ステータスが返される原因となります。 以下を確認します。 [エラータイプ](#error-types) を参照して、各エラータイプに対応する期待されるステータスコードを確認します。

エラーは、エラーオブジェクトを含む応答本文と共に表示されます。 この場合、応答本文には問題の詳細が含まれます。詳細は、 [RFC 7807 HTTP API の問題詳細](https://tools.ietf.org/html/rfc7807).

返される content-type は `application/problem+json` メディアタイプ。 存在する場合、この応答には、エラーに関する機械が読み取り可能な詳細が含まれます。 問題の詳細には、URI タイプが含まれます。

すべてのエラーオブジェクトには `type`, `status`, `title`, `detail` および `report` API クライアントが問題の内容を知ることができるようにするメッセージのプロパティ。

| プロパティ | タイプ | 説明 |
| -------- | ------ | ----------- |
| `type` | 文字列 | 問題の種類を識別する URI 参照 (RFC3986) で、形式に従います `https://ns.adobe.com/aep/errors/<ERROR-CODE>`. |
| `status` | 数値 | この問題が発生した場合にサーバーで生成される HTTP ステータスコード。 |
| `title` | 文字列 | 問題の種類に関する、人間が読み取り可能な短い概要です。 |
| `detail` | 文字列 | 人間が読み取れる、問題タイプの短い説明。 |
| `report` | オブジェクト | リクエスト ID や組織 ID など、デバッグに役立つ追加のプロパティのマップ。 場合によっては、検証エラーのリストなど、目下のエラーに固有のデータが含まれることがあります。 |

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

* エラー：リクエストの処理中に発生したが、リクエスト全体を拒否しなかった問題 ( 例： 非クリティカルなアップストリームエラー )。
* 警告：リクエストの部分処理が発生したことを示す可能性のあるアップストリームサービスからのメッセージ。

致命的でないエラー（警告を除く）が発生した場合、 [!DNL Server API] 応答のステータスを次に変更します： `207 Multi-Status`.

一方、警告は、通常、一時的な状態を表し、要求に完全に影響を与えなかったので、ほとんどの場合、参考情報です。 ここでの例の 1 つは、セグメント化エンジンで読み取られた部分的なプロファイルです。その場合、精度にある程度の影響がありますが、機能は引き続き提供されます。

致命的でないエラーは、 _問題の詳細_ 形式に変換されるが、Edge ゲートウェイの標準応答（タイプ）に直接埋め込まれる。 `application/json`.

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

## 処理 `4xx` および `5xx` 応答


| エラーコード | 説明 |
|---|---|
| `4xx Bad Request` | 最も多い `4xx` 400、403、404 などのエラーは、クライアントの代わりに再試行しないでください ( ただし、 `429`. これらはクライアントエラーで、成功しません。 リクエストを再試行する前に、クライアントがエラーを解決する必要があります。 |
| `429 Too Many Requests` | `429` HTTP 応答コードは、Adobe Experience Platform Edge Network またはアップストリームサービスがリクエストをレート制限していることを示します。 この場合、呼び出し元は `Retry-After` 応答ヘッダー。 戻される応答には、ドメイン固有のエラーコードが記述された HTTP 応答コードが含まれている必要があります。 |
| `500 Internal Server Error` | `500` エラーは汎用の包括的エラーです。 `500` エラーは再試行できません ( `502` および `503`. 仲介者は、 `500` エラーが発生し、一般的なエラーコードやメッセージ、またはドメイン固有のエラーコードやメッセージで応答する場合があります。 |
| `502 Bad Gateway` | Adobe Experience Platform Edge Network がアップストリームサーバーから無効な応答を受け取ったことを示します。 この問題は、サーバー間のネットワークの問題が原因で発生する可能性があります。 一時的なネットワークの問題が解決し、再試行によって問題が解決する場合があります。そのため、 `502` エラーは、しばらくしてからリクエストを再試行する可能性があります。 |
| `503 Service Unavailable` | このエラーコードは、サービスが一時的に利用できないことを示します。 この問題は、メンテナンスの間に発生する可能性があります。 受信者 `503` エラーはリクエストを再試行する可能性がありますが、 `Retry-After` ヘッダー。 |
| `504 Gateway Timeout` | アップストリームサーバーに対するAdobe Experience Platform Edge Network リクエストがタイムアウトしたことを示します。 この問題は、サーバー間のネットワークの問題、DNS の問題、その他のネットワークの問題が原因で発生する場合があります。 一時的なネットワークの問題は、しばらくすると解決し、再試行すると問題が解決する場合があります。 |
