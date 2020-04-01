---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 時系列データのストリーミング
topic: tutorial
translation-type: tm+mt
source-git-commit: 79466c78fd78c0f99f198b11a9117c946736f47a

---


# 時系列データをAdobe Experience Platformにストリーミング配信

このチュートリアルは、Adobe Experience Platform Data Ingestion Service APIの一部であるストリーミングインジェストAPIの使用を開始する際に役立ちます。

## はじめに

このチュートリアルでは、Adobe Experience Platformの各種サービスに関する実用的な知識が必要です。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [エクスペリエンスデータモデル(XDM)](../../xdm/home.md):プラットフォームがエクスペリエンスデータを整理するための標準化されたフレームワーク。
- [リアルタイム顧客プロファイル](../../profile/home.md):複数のソースからの集計データに基づいて、統合されたプロファイルをリアルタイムで提供します。
- [スキーマレジストリ開発者ガイド](../../xdm/api/getting-started.md):スキーマレジストリAPIの使用可能な各エンドポイントと、それらのエンドポイントを呼び出す方法をカバーする包括的なガイドです。 これには、このチュート `{TENANT_ID}`リアル全体の呼び出しに表示されるスキーマを把握することや、取り込み用のデータセットを作成する際に使用するデータセットの作成方法を知ることも含まれます。

また、このチュートリアルでは、既にストリーミング接続を作成している必要があります。 ストリーミング接続の作成について詳しくは、「ストリーミング接続の作成」チ [ュートリアルを参照してくださ](./create-streaming-connection.md)い。

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

## XDM ExperienceEventクラスを基にスキーマを作成する

データセットを作成するには、まずXDM ExperienceEventクラスを実装する新しいスキーマを作成する必要があります。 スキーマの作成方法について詳しくは、 [スキーマレジストリAPI開発者ガイドを参照してください](../../xdm/api/getting-started.md)。

**API形式**

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
| `title` | スキーマ名。 この名前は一意である必要があります。 |
| `description` | 作成するスキーマのわかりやすい説明。 |
| `meta:immutableTags` | この例では、タグを使用 `union` して、データをリアルタイム顧客 [プロファイルに保持します](../../profile/home.md)。 |

**応答**

正常な応答は、新しく作成された応答の詳細と共にHTTPステータス201を返します。スキーマ

```json
{
    "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
    "meta:altId": "_{TENANT_ID}.schemas.{SCHEMA_ID}",
    "meta:resourceType": "schemas",
    "version": "{SCHEMA_VERSION}",
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
| `{TENANT_ID}` | このIDは、作成したリソースの名前が適切に付けられ、IMS組織内に含まれていることを確認するために使用されます。 テナントIDの詳細については、『 [スキーマレジストリガイド](../../xdm/api/getting-started.md#know-your-tenant-id)』 |

データセットを作成する `$id` 際には、属性と `version` 属性の両方が使用されるので、注意してください。

## スキーマのプライマリID記述子の設定

次に、作業用の電子メ [ールアドレス属性をプライマリスキーマとして](../../xdm/api/descriptors.md) 、上で作成した識別子にID記述子を追加します。 これを行うと、次の2つの変更が行われます。

1. 勤務先の電子メールアドレスは必須フィールドになります。 これは、このフィールドを使用しないで送信されたメッセージは検証に失敗し、取り込まれないことを意味します。

2. リアルタイムの顧客プロファイルは、勤務先の電子メールアドレスを識別子として使用し、個人に関する詳細情報をまとめます。

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
| `{SCHEMA_REF_ID}` | 以前 `$id` に受信したスキーマ。 次のようになります。 `"https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}"` |

>[!NOTE] ID&#x200B;&#x200B;**名前空間コード**
>
> コードが有効であることを確認してください。上の例では、標準のID名前空間である「email」を使用しています。 その他の一般的に使用される標準ID名前空間は、IDサービスのFAQ [に記載されています](../../identity-service/troubleshooting-guide.md#what-are-the-standard-identity-namespaces-provided-by-experience-platform)。
>
> カスタム名前空間を作成する場合は、「ID名前空間の概要」で概要を説明している手順に従 [います](../../identity-service/home.md)。
**応答**

成功応答は、新たに作成されたプライマリID情報と共に、HTTPステータス201をスキーマに返します。

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

レコードデータを取り込むスキーマセットを作成する必要があります。

>[!NOTE] このデータセットは、適切なタグを設 **定することで、リアルタイムの顧客プロファイル****** /ID(リアルタイム)に対して有効になります。

**API形式**

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
        "contentType": "application/vnd.adobe.xed-full+json;version={SCHEMA_VERSION}"
    },
    "fileDescription": {
        "persisted": true,
        "containerFormat": "parquet",
        "format": "parquet"
    },
    "tags": {
        "unifiedIdentity": ["enabled:true"],
        "unifiedProfile": ["enabled:true"]
    }
}'
```

**応答**

成功した応答は、HTTPステータス201と、新しく作成されたデータセットのIDを形式で含む配列を返しま `@/dataSets/{DATASET_ID}`す。

```json
[
    "@/dataSets/5e72608b10f6e318ad2dee0f"
]
```

## 時系列データをストリーミング接続に取り込む

データセットとストリーミング接続が確立された状態で、XDM形式のJSONレコードを取り込み、Platform内の時系列データを取り込むことができます。

**API形式**

```http
POST /collection/{CONNECTION_ID}?synchronousValidation=true
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | 新しく `id` 作成されたストリーミング接続の値。 |
| `synchronousValidation` | 開発目的のクエリパラメーター（オプション）。 に設定した場合、リ `true`クエストが正常に送信されたかどうかを確認するために、即座のフィードバックに使用できます。 デフォルトでは、この値はに設定されていま `false`す。 |

**リクエスト**

>[!NOTE] 独自のおよびを生成する必要があ `xdmEntity._id` ります `xdmEntity.timestamp`。 IDを生成する良い方法は、UUIDを使用することです。 また、次のAPI呼び出しでは、認証ヘ **ッダー** は必要ありません。


```shell
curl -X POST https://dcs.adobedc.net/collection/{CONNECTION_ID}?synchronousValidation=true \
  -H "Content-Type: application/json" \
  -d '{
    "header": {
        "schemaRef": {
            "id": "{SCHEMA_REF_ID}",
            "contentType": "application/vnd.adobe.xed-full+json;version={SCHEMA_VERSION}"
        },
        "imsOrgId": "{IMS_ORG}",
        "source": {
            "name": "GettingStarted"
        },
        "datasetId": "{DATASET_ID}"
    },
    "body": {
        "xdmMeta": {
            "schemaRef": {
                "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
                "contentType": "application/vnd.adobe.xed-full+json;version={SCHEMA_VERSION}"
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

**応答**

成功した応答は、新しくストリーミングされた応答の詳細と共にHTTPステータス200を返します。プロファイル

```json
{
    "inletId": "{CONNECTION_ID}",
    "xactionId": "1584479347507:2153:240",
    "receivedTimeMs": 1584479347507,
    "synchronousValidation": {
        "status": "pass"
    }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{CONNECTION_ID}` | 以前に作成したストリーミング接続のID。 |
| `xactionId` | 先ほど送信したレコードに対してサーバー側で生成された一意の識別子。 このIDは、様々なシステムやデバッグを通じて、アドビがこのレコードのライフサイクルを追跡するのに役立ちます。 |
| `receivedTimeMs`:リクエストが受信された時刻を示すタイムスタンプ（ミリ秒単位のエポック）。 |
| `synchronousValidation.status` | クエリパラメー `synchronousValidation=true` ターが追加されたので、この値が表示されます。 検証が成功した場合、ステータスは次のようになりま `pass`す。 |

## 新しく取り込んだ時系列データを取得する

以前に取り込んだレコードを検証するには、 [プロファイルアクセスAPI](../../profile/api/entities.md) (Time Series)を使用して時系列データを取得します。 これは、エンドポイントへのGET要求を使用し、オプションのパラメー `/access/entities` ターを使用してクエリできます。 複数のパラメーターを使用でき、アンパサンド(&amp;)で区切ります。」

>[!NOTE] マージポリシーIDが定義されていない場合、およびスキーマ。</span>nameまたはrelatedSchema</span>.nameがです。 `_xdm.context.profile`プロファイルアクセスは関連するすべ **ての** IDを取得します。

**API形式**

```http
GET /access/entities
GET /access/entities?{QUERY_PARAMETERS}
GET /access/entities?schema.name=_xdm.context.experienceevent&relatedSchema.name=_xdm.context.profile&relatedEntityId=janedoe@example.com&relatedEntityIdNS=email
```

| パラメーター | 説明 |
| --------- | ----------- |
| `schema.name` | **必須.** アクセスするスキーマの名前。 |
| `relatedSchema.name` | **必須.** にアクセスするので、こ `_xdm.context.experienceevent`の値は時系列プロファイルが関連するイベントエンティティのスキーマを指定します。 |
| `relatedEntityId` | 関連エンティティのID。 指定する場合は、エンティティの名前空間も指定します。 |
| `relatedEntityIdNS` | 取得しようとしているIDの名前空間。 |

**リクエスト**

```shell
curl -X GET \
  https://platform-stage.adobe.io/data/core/ups/access/entities?schema.name=_xdm.context.experienceevent&relatedSchema.name=_xdm.context.profile&relatedEntityId=janedoe@example.com&relatedEntityIdNS=email \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}"
```

**応答**

成功した応答は、要求されたエンティティの詳細と共にHTTPステータス200を返します。 ご覧のとおり、これは以前に取り込まれたのと同じ時系列データです。

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

このドキュメントを読むと、ストリーミング接続を使用して、レコードデータをプラットフォームに取り込む方法を理解できます。 様々な値を持つ呼び出しをさらに行い、更新された値を取得してみることができます。 さらに、プラットフォームUIを使用して、開始による取り込んだデータの監視も可能です。 詳しくは、監視データ取り込みガイド [を参照してください](../quality/monitor-data-flows.md) 。

一般的なストリーミング取り込みの詳細については、ストリーミング取り込みの概要を [参照してくださ](../streaming-ingestion/overview.md)い。
