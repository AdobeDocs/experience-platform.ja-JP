---
keywords: Experience Platform;home;popular topics;streaming ingestion;ingestion;time series data;stream time series data;
solution: Experience Platform
title: Stream Time-Series Data Using Streaming Ingestion APIs
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルは、Adobe Experience Platform データ取得サービス API の一部であるストリーミング取得 API の使用を開始する際に役に立ちます。
exl-id: 720b15ea-217c-4c13-b68f-41d17b54d500
source-git-commit: d6b16f09dc4e97135f42ddadd8e34b0f7db93327
workflow-type: tm+mt
source-wordcount: '1369'
ht-degree: 66%

---

# Stream time-series data using Streaming Ingestion APIs

[!DNL Data Ingestion Service]

## はじめに

このチュートリアルでは、Adobe Experience Platform の各種サービスに関する実用的な知識が必要です。このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)[!DNL Platform]
- [[!DNL Real-time Customer Profile]](../../profile/home.md)
- [](../../xdm/api/getting-started.md)[!DNL Schema Registry]これには、このチュートリアル全体の呼び出しで表示される `{TENANT_ID}` の理解と、取得用のデータセットの作成に使用されるスキーマの作成方法の理解が含まれます。

また、このチュートリアルでは、既にストリーミング接続を作成している必要があります。ストリーミング接続の作成について詳しくは、『[ストリーミング接続作成のチュートリアル](./create-streaming-connection.md)』を参照してください。

以下の節では、ストリーミング取得 API の呼び出しを正常におこなうために知っておく必要がある追加情報を示します。

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- Authorization： Bearer `{ACCESS_TOKEN}`
- x-api-key： `{API_KEY}`
- x-gw-ims-org-id： `{IMS_ORG}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name：`{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

- Content-Type: application/json

## XDM ExperienceEvent クラスベースのスキーマの作成

[!DNL XDM ExperienceEvent]スキーマの作成方法について詳しくは、『[スキーマレジストリ API 開発者ガイド](../../xdm/api/getting-started.md)』を参照してください。

**API 形式**

```http
POST /schemaregistry/tenant/schemas
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "type": "object",
    "title": "{SCHEMA_NAME}",
    "description": "{SCHEMA_DESCRIPTION}",
    "allOf": [
        {
            "$ref": "https://ns.adobe.com/xdm/context/experienceevent"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/experienceevent-environment-details"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/experienceevent-commerce"
        },
        {  
         "$ref":"https://ns.adobe.com/experience/campaign/experienceevent-profile-work-details"
        }
    ],
    "meta:immutableTags": [
        "union"
    ]
}'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `title` | スキーマ名。この名前は一意である必要があります。 |
| `description` | 作成するスキーマのわかりやすい説明。 |
| `meta:immutableTags` | `union`[[!DNL Real-time Customer Profile]](../../profile/home.md) |

**応答** 

正常な応答は、HTTP ステータス 201 と、新しく作成されたスキーマの詳細を返します。

```json
{
    "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
    "meta:altId": "_{TENANT_ID}.schemas.{SCHEMA_ID}",
    "meta:resourceType": "schemas",
    "version": "1",
    "type": "object",
    "title": "{SCHEMA_NAME}",
    "description": "{SCHEMA_DESCRIPTION}",
    "allOf": [
        {
            "$ref": "https://ns.adobe.com/xdm/context/experienceevent",
            "type": "object",
            "meta:xdmType": "object"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/experienceevent-environment-details",
            "type": "object",
            "meta:xdmType": "object"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/experienceevent-commerce",
            "type": "object",
            "meta:xdmType": "object"
        },
        {
            "$ref": "https://ns.adobe.com/experience/campaign/experienceevent-profile-work-details",
            "type": "object",
            "meta:xdmType": "object"
        }
    ],
    "refs": [
        "https://ns.adobe.com/xdm/context/experienceevent-commerce",
        "https://ns.adobe.com/experience/campaign/experienceevent-profile-work-details",
        "https://ns.adobe.com/xdm/context/experienceevent-environment-details",
        "https://ns.adobe.com/xdm/context/experienceevent"
    ],
    "imsOrg": "{IMS_ORG}",
    "meta:immutableTags": [
        "union"
    ],
    "meta:class": "https://ns.adobe.com/xdm/context/experienceevent",
    "required": [
        "@id",
        "xdm:timestamp"
    ],
    "meta:abstract": false,
    "meta:extensible": false,
    "meta:extends": [
        "https://ns.adobe.com/xdm/context/experienceevent",
        "https://ns.adobe.com/xdm/data/time-series",
        "https://ns.adobe.com/xdm/context/identitymap",
        "https://ns.adobe.com/xdm/context/experienceevent-environment-details",
        "https://ns.adobe.com/xdm/context/experienceevent-commerce",
        "https://ns.adobe.com/experience/campaign/experienceevent-profile-work-details"
    ],
    "meta:containerId": "tenant",
    "imsOrg": "{IMS_ORG}",
    "meta:xdmType": "object",
    "meta:class": "https://ns.adobe.com/xdm/context/experienceevent",
    "meta:registryMetadata": {
        "repo:createDate": 1551229957987,
        "repo:lastModifiedDate": 1551229957987,
        "xdm:createdClientId": "{CLIENT_ID}",
        "xdm:repositoryCreatedBy": "{CREATED_BY}"
    },
    "meta:tenantNamespace": "{NAMESPACE}"
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{TENANT_ID}` | この ID は、作成するリソースの名前空間が適切に設定され、IMS 組織内に含まれるようにするために使用されます。テナント ID の詳細については、『[スキーマレジストリガイド](../../xdm/api/getting-started.md#know-your-tenant-id)』を参照してください。 |

データセットを作成する際には、`$id` 属性と `version` 属性の両方が使用されるので、注意してください。

## スキーマのプライマリ ID 記述子の設定

次に、仕事用電子メールアドレス属性をプライマリ識別子として使用して、上で作成した識別子に [ID 記述子](../../xdm/api/descriptors.md)を追加します。これをおこなうと、次の 2 つの変更が行われます。

1. 仕事用電子メールアドレスは必須フィールドになります。つまり、このフィールドなしで送信されたメッセージは検証に失敗し、取得されません。

2. [!DNL Real-time Customer Profile]

### リクエスト

```shell
curl -X POST https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "@type":"xdm:descriptorIdentity",
    "xdm:sourceProperty":"/_experience/campaign/message/profileSnapshot/workEmail/address",
    "xdm:property":"xdm:code",
    "xdm:isPrimary":true,
    "xdm:namespace":"Email",
    "xdm:sourceSchema":"{SCHEMA_REF_ID}",
    "xdm:sourceVersion":1
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SCHEMA_REF_ID}` | 以前にスキーマを構成したときに受け取った `$id`。次のようになります。 `"https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}"` |

>[!NOTE]
>
>&#x200B;**ID 名前空間コード**
>
> コードが有効であることを確認してください。上記の例では、標準の ID 名前空間である「email」を使用しています。その他の一般的に使用される標準 ID 名前空間は、[ID サービスの FAQ](../../identity-service/troubleshooting-guide.md#what-are-the-standard-identity-namespaces-provided-by-experience-platform) に記載されています。
>
> カスタム名前空間を作成する場合は、[ID 名前空間の概要](../../identity-service/home.md)で説明している手順に従います。

**応答** 

正常な応答は、HTTP ステータス 201 と、新しく作成されたスキーマのプライマリ ID 名前空間に関する情報を返します。

```json
{
    "xdm:property": "xdm:code",
    "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
    "xdm:namespace": "Email",
    "@type": "xdm:descriptorIdentity",
    "xdm:sourceVersion": 1,
    "xdm:isPrimary": true,
    "xdm:sourceProperty": "/_experience/campaign/message/profileSnapshot/workEmail/address",
    "@id": "ec31c09e0906561861be5a71fcd964e29ebe7923b8eb0d1e",
    "meta:containerId": "tenant",
    "version": "1",
    "imsOrg": "{IMS_ORG}"
}
```

## 時系列データのデータセットの作成

スキーマを作成したら、レコードデータを取得するデータセットを作成する必要があります。

>[!NOTE]
>
>**[!DNL Real-time Customer Profile]****[!DNL Identity]**

**API 形式**

```http
POST /catalog/dataSets
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/catalog/dataSets \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "name": "{DATASET_NAME}",
    "description": "{DATASET_DESCRIPTION}",
    "schemaRef": {
        "id": "{SCHEMA_REF_ID}",
        "contentType": "application/vnd.adobe.xed-full+json;version=1"
    },
    "tags": {
        "unifiedIdentity": ["enabled:true"],
        "unifiedProfile": ["enabled:true"]
    }
}'
```

**応答** 

正常な応答は、HTTP ステータス 201 と、新しく作成されたデータセットの ID を `@/dataSets/{DATASET_ID}` の形式で含む配列を返します。

```json
[
    "@/dataSets/5e72608b10f6e318ad2dee0f"
]
```


## ストリーミング接続の作成

After creating your schema and dataset, you will need to create a streaming connection to ingest your data.

ストリーミング接続の作成について詳しくは、『[ストリーミング接続作成のチュートリアル](./create-streaming-connection.md)』を参照してください。

## 時系列データのストリーミング接続への取得

[!DNL Platform]

**API 形式**

```http
POST /collection/{CONNECTION_ID}?syncValidation=true
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | 新しく作成されたストリーミング接続の `id` 値。 |
| `syncValidation` | 開発目的のクエリーパラメーター（オプション）。`true` に設定した場合、リクエストが正常に送信されたかどうかを確認するために、即座のフィードバックに使用できます。デフォルトでは、この値は `false` に設定されています。`true``CONNECTION_ID` |

**リクエスト**

Ingesting time series data to a streaming connection can be done either with or without the source name.

The example request below ingests time series data with a missing source name to Platform. If the data is missing the source name, it will add the source ID from the streaming connection definition.

`xdmEntity._id``xdmEntity.timestamp``xdmEntity._id`****

`xdmEntity._id``xdmEntity.timestamp`Ideally, your source system will contain these values. If an ID is not available, consider concatenating values of other fields in the record to create a unique value that can be consistently regenerated from the record on re-ingestion.

>[!NOTE]
>
> 以下の API 呼び出しでは、認証ヘッダーは&#x200B;**必要ありません**。

```shell
curl -X POST https://dcs.adobedc.net/collection/{CONNECTION_ID}?syncValidation=true \
  -H "Content-Type: application/json" \
  -d '{
    "header": {
        "schemaRef": {
            "id": "{SCHEMA_REF_ID}",
            "contentType": "application/vnd.adobe.xed-full+json;version=1"
        },
        "flowId": "{FLOW_ID}",
        "datasetId": "{DATASET_ID}"
    },
    "body": {
        "xdmMeta": {
            "schemaRef": {
                "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
                "contentType": "application/vnd.adobe.xed-full+json;version=1"
            }
        },
        "xdmEntity":{
            "_id": "9af5adcc-db9c-4692-b826-65d3abe68c22",
            "timestamp": "2019-02-23T22:07:01Z",
            "environment": {
                "browserDetails": {
                    "userAgent": "Mozilla\/5.0 (Windows NT 5.1) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/29.0.1547.57 Safari\/537.36 OPR\/16.0.1196.62",
                    "acceptLanguage": "en-US",
                    "cookiesEnabled": true,
                    "javaScriptVersion": "1.6",
                    "javaEnabled": true
                },
                "colorDepth": 32,
                "viewportHeight": 799,
                "viewportWidth": 414
            },
            "productListItems": [
                {
                    "SKU": "CC",
                    "name": "Fernie Snow",
                    "quantity": 30,
                    "priceTotal": 7.8
                }
            ],
            "commerce": {
                "productViews": {
                    "value": 1
                }
            },
            "_experience": {
                "campaign": {
                    "message": {
                        "profileSnapshot": {
                            "workEmail":{
                                "address": "janedoe@example.com"
                            }
                        }
                    }
                }
            }
        }
    }
}'
```

If you want to include a source name, the following example shows how you would include it.

```json
    "header": {
        "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
            "contentType": "application/vnd.adobe.xed-full+json;version=1"
        },
        "imsOrgId": "{IMS_ORG}",
        "datasetId": "{DATASET_ID}",
        "source": {
            "name": "Sample source name"
        }
    }
```

**応答**

[!DNL Profile]

```json
{
    "inletId": "{CONNECTION_ID}",
    "xactionId": "1584479347507:2153:240",
    "receivedTimeMs": 1584479347507,
    "syncValidation": {
        "status": "pass"
    }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{CONNECTION_ID}` | `inletId` |
| `xactionId` | 送信したレコードに対してサーバー側で生成された一意の ID です。この ID は、様々なシステムやデバッグを通じて、アドビがこのレコードのライフサイクルを追跡するのに役立ちます。 |
| `receivedTimeMs`：リクエストが受信された時刻を示すタイムスタンプ（ミリ秒単位のエポック）。 |
| `syncValidation.status` | クエリーパラメーター `syncValidation=true` が追加されたので、この値が表示されます。検証が成功した場合、ステータスは `pass` になります。 |

## 新しく取得した時系列データの取得

[[!DNL Profile Access API]](../../profile/api/entities.md)これは、`/access/entities` エンドポイントへの GET リクエストとオプションのクエリーパラメーターを使用しておこなうことができます。複数のパラメーターを使用でき、アンパサンド（&amp;）で区切ります。

>[!NOTE]
>
>`schema.name``relatedSchema.name``_xdm.context.profile`[!DNL Profile Access]****

**API 形式**

```http
GET /access/entities
GET /access/entities?{QUERY_PARAMETERS}
GET /access/entities?schema.name=_xdm.context.experienceevent&relatedSchema.name=_xdm.context.profile&relatedEntityId=janedoe@example.com&relatedEntityIdNS=email
```

| パラメーター | 説明 |
| --------- | ----------- |
| `schema.name` | **必須.** アクセスするスキーマの名前。 |
| `relatedSchema.name` | **必須.** `_xdm.context.experienceevent` にアクセスするので、この値は、時系列イベントが関連するプロファイルエンティティのスキーマを指定します。 |
| `relatedEntityId` | 関連エンティティの ID。指定する場合は、エンティティの名前空間も指定します。 |
| `relatedEntityIdNS` | 取得しようとしている ID の名前空間。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/access/entities?schema.name=_xdm.context.experienceevent&relatedSchema.name=_xdm.context.profile&relatedEntityId=janedoe@example.com&relatedEntityIdNS=email \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}"
```

**応答** 

正常な応答は、HTTP ステータス 200 と、リクエストされたエンティティの詳細を返します。ご覧のとおり、これは以前に取得されたのと同じ時系列データです。

```json
{
    "_page": {
        "orderby": "timestamp",
        "start": "9af5adcc-db9c-4692-b826-65d3abe68c22",
        "count": 1,
        "next": ""
    },
    "children": [
        {
            "relatedEntityId": "BVrqzwVv7o2p3naHvnsWpqZXv3KJgA",
            "entityId": "9af5adcc-db9c-4692-b826-65d3abe68c22",
            "timestamp": 1582495621000,
            "entity": {
                "environment": {
                    "browserDetails": {
                        "javaScriptVersion": "1.6",
                        "cookiesEnabled": true,
                        "userAgent": "Mozilla/5.0 (Windows NT 5.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/29.0.1547.57 Safari/537.36 OPR/16.0.1196.62",
                        "acceptLanguage": "en-US",
                        "javaEnabled": true
                    },
                    "colorDepth": 32,
                    "viewportHeight": 799,
                    "viewportWidth": 414
                },
                "_id": "9af5adcc-db9c-4692-b826-65d3abe68c22",
                "commerce": {
                    "productViews": {
                        "value": 1
                    }
                },
                "productListItems": [
                    {
                        "name": "Fernie Snow",
                        "quantity": 30,
                        "SKU": "CC",
                        "priceTotal": 7.8
                    }
                ],
                "_experience": {
                    "campaign": {
                        "message": {
                            "profileSnapshot": {
                                "workEmail": {
                                    "address": "janedoe@example.com"
                                }
                            }
                        }
                    }
                },
                "timestamp": "2020-02-23T22:07:01Z"
            },
            "lastModifiedAt": "2020-03-18T18:51:19Z"
        }
    ],
    "_links": {
        "next": {
            "href": ""
        }
    }
}
```

## 次の手順

[!DNL Platform]異なる値でさらに呼び出しを実行し、更新された値を取得してみてください。[!DNL Platform]詳しくは、『[データ取得監視ガイド](../quality/monitor-data-ingestion.md)』を参照してください。

一般的なストリーミング取得の詳細については、『[ストリーミング取得の概要](../streaming-ingestion/overview.md)』を参照してください。
