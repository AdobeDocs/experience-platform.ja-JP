---
keywords: Experience Platform；ホーム；人気のトピック；ストリーミング取得；取得；時系列データ；時系列データのストリーミング；
solution: Experience Platform
title: ストリーミング取得 API を使用した時系列データのストリーミング
type: Tutorial
description: このチュートリアルは、Adobe Experience Platform データ取得サービス API の一部であるストリーミング取得 API の使用を開始する際に役に立ちます。
exl-id: 720b15ea-217c-4c13-b68f-41d17b54d500
source-git-commit: 35ccc39fdfef31ca1f59e2e11f0d3d762e423635
workflow-type: tm+mt
source-wordcount: '1210'
ht-degree: 59%

---

# ストリーミング取得 API を使用した時系列データのストリーミング

このチュートリアルは、Adobe Experience Platform [!DNL Data Ingestion Service] API の一部であるストリーミング取得 API の使用を開始する際に役立ちます。

## はじめに

このチュートリアルでは、Adobe Experience Platform の各種サービスに関する実用的な知識が必要です。このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md):[!DNL Platform] がエクスペリエンスデータを整理する際に使用する、標準化されたフレームワーク。
- [[!DNL Real-Time Customer Profile]](../../profile/home.md)：複数のソースから集計したデータに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
- [ スキーマレジストリ開発者ガイド ](../../xdm/api/getting-started.md):[!DNL Schema Registry] API の使用可能な各エンドポイントとそれらの呼び出し方法を説明する包括的なガイド。 これには、このチュートリアル全体の呼び出しで表示される `{TENANT_ID}` の理解と、取得用のデータセットの作成に使用されるスキーマの作成方法の理解が含まれます。

また、このチュートリアルでは、既にストリーミング接続を作成している必要があります。ストリーミング接続の作成について詳しくは、『[ストリーミング接続作成のチュートリアル](./create-streaming-connection.md)』を参照してください。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../landing/api-guide.md)のガイドを参照してください。

## XDM ExperienceEvent クラスベースのスキーマの作成

データセットを作成するには、まず [!DNL XDM ExperienceEvent] クラスを実装する新しいスキーマを作成する必要があります。 スキーマの作成方法について詳しくは、『[スキーマレジストリ API 開発者ガイド](../../xdm/api/getting-started.md)』を参照してください。

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `meta:immutableTags` | この例では、データを [[!DNL Real-Time Customer Profile]](../../profile/home.md) に保持するために `union` タグが使用されています。 |

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
    "imsOrg": "{ORG_ID}",
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
    "imsOrg": "{ORG_ID}",
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
| `{TENANT_ID}` | この ID は、作成したリソースの名前空間が適切に設定され、組織内に格納されていることを確認するために使用されます。 テナント ID の詳細については、『[スキーマレジストリガイド](../../xdm/api/getting-started.md#know-your-tenant-id)』を参照してください。 |

データセットを作成する際には、`$id` 属性と `version` 属性の両方が使用されるので、注意してください。

## スキーマのプライマリ ID 記述子の設定

次に、仕事用電子メールアドレス属性をプライマリ識別子として使用して、上で作成した識別子に [ID 記述子](../../xdm/api/descriptors.md)を追加します。これをおこなうと、次の 2 つの変更が行われます。

1. 仕事用電子メールアドレスは必須フィールドになります。つまり、このフィールドなしで送信されたメッセージは検証に失敗し、取得されません。

2. [!DNL Real-Time Customer Profile] は、その個人に関する詳細を結合するのに役立つ識別子として、作業メールアドレスを使用します。

### リクエスト

```shell
curl -X POST https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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
    "imsOrg": "{ORG_ID}"
}
```

## 時系列データのデータセットの作成

スキーマを作成したら、レコードデータを取得するデータセットを作成する必要があります。

>[!NOTE]
>
>このデータセットは、適切なタグを設定することで、**[!DNL Real-Time Customer Profile]** と **[!DNL Identity]** に対して有効になります。

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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

スキーマとデータセットを作成したら、ストリーミング接続を作成してデータを取り込む必要があります。

ストリーミング接続の作成について詳しくは、『[ストリーミング接続作成のチュートリアル](./create-streaming-connection.md)』を参照してください。

## 時系列データのストリーミング接続への取得

データセット、ストリーミング接続、データフローを作成したら、XDM 形式の JSON レコードを取り込んで、[!DNL Platform] 内で時系列データを取り込むことができます。

**API 形式**

```http
POST /collection/{CONNECTION_ID}?syncValidation=true
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | 新しく作成されたストリーミング接続の `id` 値。 |
| `syncValidation` | 開発目的のクエリーパラメーター（オプション）。`true` に設定した場合、リクエストが正常に送信されたかどうかを確認するために、即座のフィードバックに使用できます。デフォルトでは、この値は `false` に設定されています。 このクエリパラメーターを `true` に設定すると、リクエストのレートは 1`CONNECTION_ID` あたり 1 分あたり 60 回に制限されます。 |

**リクエスト**

時系列データのストリーミング接続への取り込みは、ソース名を使用して行うことも、使用せずに行うこともできます。

次のリクエスト例では、ソース名が欠落している時系列データを Platform に取り込みます。 データにソース名がない場合、ストリーミング接続定義からソース ID が追加されます。

`xdmEntity._id` と `xdmEntity.timestamp` は両方とも、時系列データの必須フィールドです。 `xdmEntity._id` 属性は、レコード自体の一意の識別子を表します **レコードが存在する個人またはデバイスの一意の ID ではありません**。

レコードを再度取り込む必要がある場合は、一貫性を維持する方法で、独自の `xdmEntity._id` と `xdmEntity.timestamp` を生成する必要があります。 ソースシステムにこれらの値が含まれているのが理想です。 ID が使用できない場合は、レコード内の他のフィールドの値を連結して一意の値を作成し、再取り込み時にレコードから常に再生成できるようにすることを検討してください。

>[!NOTE]
>
>次の API 呼び出しには、認証ヘッダーは必要ありません ****。

```shell
curl -X POST https://dcs.adobedc.net/collection/{CONNECTION_ID}?syncValidation=true \
  -H "Content-Type: application/json" \
  -d '{
    "header": {
    "datasetId": "{DATASET_ID}",
    "flowId": "{FLOW_ID}",
    "imsOrgID": "{ORG_ID}"
    },
    "body": {
        "xdmMeta": {
            "schemaRef": {
                "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
                "contentType": "application/vnd.adobe.xed-full+json;version=1"
            },
        "identityMap": {
                "Email": [
                  {
                    "id": "acme_user@gmail.com",
                    "primary": true
                  }
                ]
              },
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

ソース名を含める場合に、ソース名をどのように含めるかを次の例に示します。

```json
    "header": {
    "datasetId": "{DATASET_ID}",
    "flowId": "{FLOW_ID}",
    "imsOrgID": "{ORG_ID}",
      "source": {
        "name": "ACME source"
      }
    }
```

**応答**

応答が成功すると、HTTP ステータス 200 が、新しくストリーミングされた [!DNL Profile] の詳細と共に返されます。

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
| `{CONNECTION_ID}` | 以前に作成したストリーミング接続の `inletId`。 |
| `xactionId` | 送信したレコードに対してサーバー側で生成された一意の ID です。この ID は、様々なシステムやデバッグを通じて、アドビがこのレコードのライフサイクルを追跡するのに役立ちます。 |
| `receivedTimeMs`：リクエストが受信された時刻を示すタイムスタンプ（ミリ秒単位のエポック）。 |
| `syncValidation.status` | クエリーパラメーター `syncValidation=true` が追加されたので、この値が表示されます。検証が成功した場合、ステータスは `pass` になります。 |

## 新しく取得した時系列データの取得

以前に取り込んだレコードを検証するには、[[!DNL Profile Access API]](../../profile/api/entities.md) を使用して時系列データを取得します。 これは、`/access/entities` エンドポイントへの GET リクエストとオプションのクエリーパラメーターを使用しておこなうことができます。複数のパラメーターを使用でき、アンパサンド（&amp;）で区切ります。

>[!NOTE]
>
>結合ポリシー ID が定義されておらず、`schema.name` または `relatedSchema.name` が `_xdm.context.profile` の場合、[!DNL Profile Access] れは **すべて** 関連の ID を取得します。

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
  -H "x-gw-ims-org-id: {ORG_ID}" \
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

このドキュメントでは、ストリーミング接続を使用してレコードデータを [!DNL Platform] に取り込む方法について説明しました。 異なる値でさらに呼び出しを実行し、更新された値を取得してみてください。さらに、UI を使用して、取り込んだデータの監視 [!DNL Platform] 開始できます。 詳しくは、『[データ取得監視ガイド](../quality/monitor-data-ingestion.md)』を参照してください。

一般的なストリーミング取得の詳細については、『[ストリーミング取得の概要](../streaming-ingestion/overview.md)』を参照してください。
