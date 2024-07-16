---
title: Flow Service API を使用した Streaming SDK のフロー仕様の更新
description: 次のドキュメントでは、セルフサービスソース用 Flow Service API （ストリーミング SDK）を使用してフロー仕様を取得および更新する手順を説明します。
exl-id: cc9dab7a-08fa-4c6c-bbac-cb658a6376fb
badge: ベータ版
source-git-commit: 256857103b4037b2cd7b5b52d6c5385121af5a9f
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 13%

---

# [!DNL Flow Service] API を使用したフロー仕様の更新

>[!NOTE]
>
>セルフサービスソース Streaming SDK はベータ版です。 ベータラベル付きソースの使用について詳しくは、[ ソースの概要 ](../../home.md#terms-and-conditions) を参照してください。

新しい接続仕様 ID を生成したら、データフローを作成するために、この ID をフロー仕様に追加する必要があります。

フロー仕様には、サポートするソース接続 ID とターゲット接続 ID、データへの適用に必要な変換仕様、フローの生成に必要なスケジュールパラメーターなど、フローを定義する情報が含まれています。 `/flowSpecs` エンドポイントを使用して、フローの仕様を編集できます。

次のドキュメントでは、セルフサービスソース用の [!DNL Flow Service] API （ストリーミング SDK）を使用してフロー仕様を取得および更新する手順を説明します。

## はじめに

先に進む前に、[はじめる前に](./getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## フロー仕様の検索 {#lookup}

`generic-streaming` テンプレートで作成されたソースはすべて、`GenericStreamingAEP` フロー仕様を使用します。 このフロー仕様は、`/flowSpecs/` エンドポイントに対してGETリクエストを行い、`e77fde5a-22a8-11ed-861d-0242ac120002` の `flowSpec.id` を指定することで取得できます。

**API 形式**

```http
GET /flowSpecs/e77fde5a-22a8-11ed-861d-0242ac120002
```

**リクエスト**

次のリクエストは、`e77fde5a-22a8-11ed-861d-0242ac120002` フロー仕様を取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/flowSpecs/e77fde5a-22a8-11ed-861d-0242ac120002' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' 
```

**応答**

応答が成功すると、クエリされたフロー仕様の詳細が返されます。

```json
{
  "items": [
    {
      "id": "e77fde5a-22a8-11ed-861d-0242ac120002",
      "name": "GenericStreamingAEP",
      "providerId": "521eee4d-8cbe-4906-bb48-fb6bd4450033",
      "version": "1.0",
      "sourceConnectionSpecIds": [
        "e77fd9d2-22a8-11ed-861d-0242ac120002"
      ],
      "targetConnectionSpecIds": [
        "c604ff05-7f1a-43c0-8e18-33bf874cb11c"
      ],
      "optionSpec": {
        "name": "OptionSpec",
        "spec": {
          "$schema": "http://json-schema.org/draft-07/schema#",
          "type": "object",
          "properties": {
            "errorDiagnosticsEnabled": {
              "title": "Error diagnostics.",
              "description": "Flag to enable detailed and sample error diagnostics summary.",
              "type": "boolean",
              "default": false
            },
            "partialIngestionPercent": {
              "title": "Partial ingestion threshold.",
              "description": "Percentage which defines the threshold of errors allowed before the run is marked as failed.",
              "type": "number",
              "exclusiveMinimum": 0
            },
            "inletUrl": {
              "title": "Inlet Url.",
              "description": "Inlet Url which defines the streaming endpoint.",
              "type": "string"
            }
          }
        }
      },
      "transformationSpecs": [
        {
          "name": "Mapping",
          "spec": {
            "$schema": "http://json-schema.org/draft-07/schema#",
            "type": "object",
            "description": "defines various params required for different mapping from source to target",
            "properties": {
              "mappingId": {
                "type": "string"
              },
              "mappingVersion": {
                "type": "string"
              }
            }
          }
        }
      ],
      "attributes": {
        "isSourceFlow": true,
        "flacValidationSupported": true,
        "frequency": "streaming",
        "uiAttributes": {
          "apiFeatures": {
            "updateSupported": true,
            "flowRunsSupported": true
          }
        }
      },
      "permissionsInfo": {
        "manage": [
          {
            "@type": "lowLevel",
            "name": "StreamingSource",
            "permissions": [
              "write"
            ]
          }
        ],
        "view": [
          {
            "@type": "lowLevel",
            "name": "StreamingSource",
            "permissions": [
              "read"
            ]
          }
        ]
      }
    }
  ]
}
```

## フロー仕様の更新 {#update}

PUT操作を使用して、フロー仕様のフィールドを更新できます。 POSTリクエストを通じてフロー仕様を更新する場合、本体は、PUTリクエストで新しいフロー仕様を作成する際に必要なすべてのフィールドを含める必要があります。

>[!IMPORTANT]
>
>新しいソースの接続仕様を作成する場合は、ソースに対応するフロー仕様の `sourceConnectionSpecIds` 配列に仕様 ID を追加する必要があります。 これにより、新しいソースが既存のフロー仕様でサポートされるので、新しいソースを使用してデータフローの作成プロセスを完了できます。

**API 形式**

```http
PUT /flowSpecs/e77fde5a-22a8-11ed-861d-0242ac120002
```

**リクエスト**

次のリクエストは、接続仕様 ID `bdb5b792-451b-42de-acf8-15f3195821de` を含むように `e77fde5a-22a8-11ed-861d-0242ac120002` のフロー仕様を更新します。

```shell
PUT -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs/e77fde5a-22a8-11ed-861d-0242ac120002' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
  -d '{
      "id": "e77fde5a-22a8-11ed-861d-0242ac120002",
      "name": "GenericStreamingAEP",
      "providerId": "521eee4d-8cbe-4906-bb48-fb6bd4450033",
      "version": "1.0",
      "sourceConnectionSpecIds": [
          "e77fd9d2-22a8-11ed-861d-0242ac120002",
          "bdb5b792-451b-42de-acf8-15f3195821de"
      ],
      "targetConnectionSpecIds": [
          "c604ff05-7f1a-43c0-8e18-33bf874cb11c"
      ],
      "optionSpec": {
          "name": "OptionSpec",
          "spec": {
              "$schema": "http://json-schema.org/draft-07/schema#",
              "type": "object",
              "properties": {
                  "errorDiagnosticsEnabled": {
                      "title": "Error diagnostics.",
                      "description": "Flag to enable detailed and sample error diagnostics summary.",
                      "type": "boolean",
                      "default": false
                  },
                  "partialIngestionPercent": {
                      "title": "Partial ingestion threshold.",
                      "description": "Percentage which defines the threshold of errors allowed before the run is marked as failed.",
                      "type": "number",
                      "exclusiveMinimum": 0
                  },
                  "inletUrl": {
                      "title": "Inlet Url.",
                      "description": "Inlet Url which defines the streaming endpoint.",
                      "type": "string"
                  }
              }
          }
      },
      "transformationSpecs": [
          {
              "name": "Mapping",
              "spec": {
                  "$schema": "http://json-schema.org/draft-07/schema#",
                  "type": "object",
                  "description": "defines various params required for different mapping from source to target",
                  "properties": {
                      "mappingId": {
                          "type": "string"
                      },
                      "mappingVersion": {
                          "type": "string"
                      }
                  }
              }
          }
      ],
      "attributes": {
          "isSourceFlow": true,
          "flacValidationSupported": true,
          "frequency": "streaming",
          "uiAttributes": {
              "apiFeatures": {
                  "updateSupported": true,
                  "flowRunsSupported": true
              }
          }
      },
      "permissionsInfo": {
          "view": [
              {
                  "@type": "lowLevel",
                  "name": "StreamingSource",
                  "permissions": [
                      "read"
                  ]
              }
          ],
          "manage": [
              {
                  "@type": "lowLevel",
                  "name": "StreamingSource",
                  "permissions": [
                      "write"
                  ]
              }
          ]
      }
  }'
```

**応答**

応答が成功すると、更新された `sourceConnectionSpecIds` のリストを含む、クエリされたフロー仕様の詳細が返されます。

```json
{
    "id": "e77fde5a-22a8-11ed-861d-0242ac120002",
    "updatedAt": 1633393222979,
    "updatedBy": "1633393222979",
    "updatedClient": "{UPDATED_CLIENT}",
    "sandboxId": "{SANDBOX_ID}",
    "sandboxName": "{SANDBOX_NAME}",
    "imsOrgId": "{ORG_ID}",
    "name": "GenericStreamingAEP",
    "providerId": "521eee4d-8cbe-4906-bb48-fb6bd4450033",
    "version": "1.0",
      "sourceConnectionSpecIds": [
        "e77fd9d2-22a8-11ed-861d-0242ac120002"
      ],
      "targetConnectionSpecIds": [
        "c604ff05-7f1a-43c0-8e18-33bf874cb11c"
      ],
      "optionSpec": {
        "name": "OptionSpec",
        "spec": {
          "$schema": "http://json-schema.org/draft-07/schema#",
          "type": "object",
          "properties": {
            "errorDiagnosticsEnabled": {
              "title": "Error diagnostics.",
              "description": "Flag to enable detailed and sample error diagnostics summary.",
              "type": "boolean",
              "default": false
            },
            "partialIngestionPercent": {
              "title": "Partial ingestion threshold.",
              "description": "Percentage which defines the threshold of errors allowed before the run is marked as failed.",
              "type": "number",
              "exclusiveMinimum": 0
            },
            "inletUrl": {
              "title": "Inlet Url.",
              "description": "Inlet Url which defines the streaming endpoint.",
              "type": "string"
            }
          }
        }
      },
      "transformationSpecs": [
        {
          "name": "Mapping",
          "spec": {
            "$schema": "http://json-schema.org/draft-07/schema#",
            "type": "object",
            "description": "defines various params required for different mapping from source to target",
            "properties": {
              "mappingId": {
                "type": "string"
              },
              "mappingVersion": {
                "type": "string"
              }
            }
          }
        }
      ],
      "attributes": {
        "isSourceFlow": true,
        "flacValidationSupported": true,
        "frequency": "streaming",
        "uiAttributes": {
          "apiFeatures": {
            "updateSupported": true,
            "flowRunsSupported": true
          }
        }
      },
      "permissionsInfo": {
        "manage": [
          {
            "@type": "lowLevel",
            "name": "StreamingSource",
            "permissions": [
              "write"
            ]
          }
        ],
        "view": [
          {
            "@type": "lowLevel",
            "name": "StreamingSource",
            "permissions": [
              "read"
            ]
          }
        ]
      }
    }
  ]
}
```

## 次の手順

新しい接続仕様を適切なフロー仕様に追加したら、新しいソースのテストと送信に進むことができます。 詳しくは、[ 新しいソースのテストと送信 ](./submit.md) に関するガイドを参照してください。
