---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: APIを使用したストリーミング接続の作成
topic: tutorial
translation-type: tm+mt
source-git-commit: 181719e729748adcde62199c9406a97b7a807182

---


# APIを使用したストリーミング接続の作成

このチュートリアルは、Adobe Experience Platform Data Ingestion Service APIの一部であるストリーミングインジェストAPIの使用を開始する際に役立ちます。

## はじめに

Adobe Experience Platformにストリーミングデータを開始するには、ストリーミング接続の登録が必要です。 ストリーミング接続を登録する場合は、ストリーミングデータのソースなど、いくつかの重要な詳細情報を指定する必要があります。

ストリーミング接続を登録すると、データプロデューサーとして、一意のURLを持ち、データをプラットフォームにストリーミングするために使用できます。

また、このチュートリアルでは、Adobe Experience Platformの各種サービスに関する実用的な知識が必要です。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [エクスペリエンスデータモデル(XDM)](../../xdm/home.md):プラットフォームがエクスペリエンスデータを整理するための標準化されたフレームワーク。
- [リアルタイム顧客プロファイル](../../profile/home.md):複数のソースからの集計データに基づいて、統合されたプロファイルをリアルタイムで提供します。

以下の節では、ストリーミング取り込みAPIの呼び出しを正常に行うために知っておく必要がある追加情報を示します。

### サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) （英語のみ）を参照してください。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず認証チュートリアルを完了する必要 [があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

- 認証：無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、サンドボックスの概要ドキュメ [ントを参照してくださ](../../sandboxes/home.md)い。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次のヘッダーが追加で必要です。

- コンテンツタイプ：application/json

## 接続の作成

接続は、ソースを指定し、フローをストリーミング取り込みAPIと互換性を持たせるために必要な情報を含みます。

**API形式**

```http
POST /flowservice/connections
```

**リクエスト**

>[!NOTE] この例に示すように、と `providerId` の値を `connectionSpec`**** 使用する必要があります。これは、ストリーミング取り込み用のストリーミング接続を作成するAPIに対して指定されている値です。

```shell
curl -X POST https://platform.adobe.io/data/foundation/flowservice/connections \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '{
     "name": "Sample name",
     "providerId": "521eee4d-8cbe-4906-bb48-fb6bd4450033",
     "description": "Sample description",
     "connectionSpec": {
         "id": "bc7b00d6-623a-4dfc-9fdb-f1240aeadaeb",
         "version": "1.0"
     },
     "auth": {
         "specName": "Streaming Connection",
         "params": {
             "sourceId": "Sample connection",
             "dataType": "xdm",
             "name": "Sample connection"
         }
     }
 }
```

**応答**

成功した応答は、新しく作成された接続の詳細を含むHTTPステータス201を返します。

```json
{
    "id": "77a05521-91d6-451c-a055-2191d6851c34",
    "etag": "\"a500e689-0000-0200-0000-5e31df730000\""
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | 新しく `id` 作成した接続の。 ここでは、を参照します `{CONNECTION_ID}`。 |
| `etag` | 接続に割り当てられ、接続のリビジョンを指定する識別子。 |

## データ収集URLの取得

接続が作成されたら、データ収集URLを取得できるようになります。

**API形式**

```http
GET /flowservice/connections/{CONNECTION_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | 前に `id` 作成した接続の値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/flowservice/connections/{CONNECTION_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、HTTPステータス200と、要求された接続に関する詳細情報を返します。 データ収集URLは接続と共に自動的に作成され、この値を使用して取得でき `inletUrl` ます。

```json
{
    "items": [
        {
            "createdAt": 1583971856947,
            "updatedAt": 1583971856947,
            "createdBy": "{API_KEY}",
            "updatedBy": "{API_KEY}",
            "createdClient": "{USER_ID}",
            "updatedClient": "{USER_ID}",
            "id": "77a05521-91d6-451c-a055-2191d6851c34",
            "name": "Another new sample connection (Experience Event)",
            "description": "Sample description",
            "connectionSpec": {
                "id": "bc7b00d6-623a-4dfc-9fdb-f1240aeadaeb",
                "version": "1.0"
            },
            "state": "enabled",
            "auth": {
                "specName": "Streaming Connection",
                "params": {
                    "sourceId": "Sample connection (ExperienceEvent)",
                    "inletUrl": "https://dcs.adobedc.net/collection/a868e1ce678a911ef1482b083329af3cafa4bafdc781285f25911eaae9e00eb2",
                    "inletId": "a868e1ce678a911ef1482b083329af3cafa4bafdc781285f25911eaae9e00eb2",
                    "dataType": "xdm",
                    "name": "Sample connection (ExperienceEvent)"
                }
            },
            "version": "\"56008aee-0000-0200-0000-5e697e150000\"",
            "etag": "\"56008aee-0000-0200-0000-5e697e150000\""
        }
    ]
}
```

## 次の手順

ストリーミング接続を作成したら、時系列またはデータを記録して、プラットフォーム内でデータを取り込むことができます。 時系列データをプラットフォームにストリーミングする方法については、時系列データのストリーミ [ングチュートリアルを参照してくださ](./streaming-time-series-data.md)い。 レコードデータをプラットフォームにストリーミングする方法については、ストリーミングレコードデータのチュ [ートリアルを参照してくださ](./streaming-record-data.md)い。

## 付録

この節では、APIを使用したストリーミング接続の作成に関する補足情報を提供します。

### 認証済みストリーミング接続

認証済みのデータ収集を使用すると、リアルタイムの顧客プロファイルやIDなどのAdobe Experience Platformサービスで、信頼できるソースと信頼できないソースからのレコードを区別できます。 PII(Personally Identifial Information)を送信するクライアントは、POSTリクエストの一部としてIMSアクセストークンを送信し、IMSトークンが有効な場合、有効なソースから収集されたレコードとしてマークされます。

認証済みストリーミング接続の作成について詳しくは、認証済みストリーミング接続の作成のチ [ュートリアルを参照してくださ](create-authenticated-streaming-connection.md)い。