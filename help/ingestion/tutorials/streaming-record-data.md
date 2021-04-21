---
keywords: Experience Platform；ホーム；人気のあるトピック；ストリーミング取り込み；取り込み；記録データ；ストリームレコードデータ；
solution: Experience Platform
title: ストリーミング取り込みAPIを使用したストリームレコードデータ
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルは、Adobe Experience Platform データ取得サービス API の一部であるストリーミング取得 API の使用を開始する際に役に立ちます。
exl-id: 097dfd5a-4e74-430d-8a12-cac11b1603aa
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1164'
ht-degree: 69%

---

# ストリーミング取り込みAPIを使用したストリームレコードデータ

このチュートリアルは、Adobe Experience Platform[!DNL Data Ingestion Service] APIの一部であるストリーミング取り込みAPIの使用を開始する際に役立ちます。

## はじめに

このチュートリアルでは、Adobe Experience Platform の各種サービスに関する実用的な知識が必要です。このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md):エクスペリエンスデータを [!DNL Platform] 編成する際に使用される標準化されたフレームワーク。
- [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、リアルタイムで統合された消費者プロファイルを提供します。
- [スキーマレジストリ開発ガイド](../../xdm/api/getting-started.md):APIの使用可能な各エンドポイントと、それらの [!DNL Schema Registry] APIを呼び出す方法をカバーする包括的なガイドです。これには、このチュートリアル全体の呼び出しで表示される `{TENANT_ID}` の理解と、取得用のデータセットの作成に使用されるスキーマの作成方法の理解が含まれます。

また、このチュートリアルでは、既にストリーミング接続を作成している必要があります。ストリーミング接続の作成について詳しくは、『[ストリーミング接続作成のチュートリアル](./create-streaming-connection.md)』を参照してください。

以下の節では、ストリーミング取得 API の呼び出しを正常におこなうために知っておく必要がある追加情報を示します。

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切に書式設定されたリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。サンプル API 呼び出しのドキュメントで使用されている規則については、[!DNL Experience Platform] トラブルシューテングガイドの[サンプル API 呼び出しの読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- Authorization: Bearer `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソースは、特定の仮想サンドボックスに分離されています。 [!DNL Platform] APIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform]のサンドボックスについて詳しくは、[サンドボックスの概要ドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

- Content-Type: application/json

## [!DNL XDM Individual Profile]クラスを基にしてスキーマを構成する

データセットを作成するには、まず[!DNL XDM Individual Profile]クラスを実装する新しいスキーマを作成する必要があります。 スキーマの作成方法について詳しくは、『[スキーマレジストリ API 開発者ガイド](../../xdm/api/getting-started.md)』を参照してください。

**API 形式**

```http
POST /schemaregistry/tenant/schemas
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "type": "object",
    "title": "Sample schema",
    "description": "Sample description",
    "allOf": [
        {
            "$ref": "https://ns.adobe.com/xdm/context/profile"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/profile-person-details"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/profile-work-details"
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
| `meta:immutableTags` | この例では、`union`タグを使用して、データを[[!DNL Real-time Customer Profile]](../../profile/home.md)に保持します。 |

**応答** 

正常な応答は、HTTP ステータス 201 と、新しく作成されたスキーマの詳細を返します。

```json
{
    "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
    "meta:altId": "_{TENANT_ID}.schemas.{SCHEMA_ID}",
    "meta:resourceType": "schemas",
    "version": "1.0",
    "type": "object",
    "title": "Sample schema",
    "description": "Sample description",
    "allOf": [
        {
            "$ref": "https://ns.adobe.com/xdm/context/profile"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/profile-person-details"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/profile-work-details"
        }
    ],
    "meta:class": "https://ns.adobe.com/xdm/context/profile",
    "meta:abstract": false,
    "meta:extensible": false,
    "meta:extends": [
        "https://ns.adobe.com/xdm/context/profile",
        "https://ns.adobe.com/xdm/data/record",
        "https://ns.adobe.com/xdm/cpmtext/identitymap",
        "https://ns.adobe.com/xdm/common/extensible",
        "https://ns.adobe.com/xdm/common/auditable",
        "https://ns.adobe.com/xdm/context/profile-person-details",
        "https://ns.adobe.com/xdm/context/profile-work-details"
    ],
    "meta:immutableTags": [
        "union"
    ],
    "meta:containerId": "tenant",
    "imsOrg": "{IMS_ORG}",
    "meta:xdmType": "object",
    "meta:registryMetadata": {
        "repo:createDate": 1551376506996,
        "repo:lastModifiedDate": 1551376506996,
        "xdm:createdClientId": "{CLIENT_ID}",
        "xdm:repositoryCreatedBy": "{CREATED_BY}"
    }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{TENANT_ID}` | この ID は、作成するリソースの名前空間が適切に設定され、IMS 組織内に含まれるようにするために使用されます。テナント ID の詳細については、『[スキーマレジストリガイド](../../xdm/api/getting-started.md#know-your-tenant-id)』を参照してください。 |

データセットを作成する際には、`$id` 属性と `version` 属性の両方が使用されるので、注意してください。

## スキーマのプライマリ ID 記述子の設定

次に、仕事用電子メールアドレス属性をプライマリ識別子として使用して、上で作成した識別子に [ID 記述子](../../xdm/api/descriptors.md)を追加します。これをおこなうと、次の 2 つの変更が行われます。

1. 仕事用電子メールアドレスは必須フィールドになります。つまり、このフィールドなしで送信されたメッセージは検証に失敗し、取得されません。

2. [!DNL Real-time Customer Profile] は、勤務先の電子メールアドレスを識別子として使用して、その個人に関する詳細情報を結合します。

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
    "xdm:sourceProperty":"/workEmail/address",
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
>&#x200B; &#x200B;**ID 名前空間コード**
>
> コードが有効であることを確認してください。上記の例では、標準の ID 名前空間である「email」を使用しています。その他の一般的に使用される標準 ID 名前空間は、[ID サービスの FAQ](../../identity-service/troubleshooting-guide.md#what-are-the-standard-identity-namespaces-provided-by-experience-platform) に記載されています。
>
> カスタム名前空間を作成する場合は、[ID 名前空間の概要](../../identity-service/home.md)で説明している手順に従います。

**応答** 

成功時の応答は、HTTP ステータス 201 と共に、スキーマ用に新しく作成されたプライマリ ID 記述子に関する情報を返します。

```json
{
    "xdm:property": "xdm:code",
    "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
    "xdm:namespace": "Email",
    "@type": "xdm:descriptorIdentity",
    "xdm:sourceVersion": 1,
    "xdm:isPrimary": true,
    "xdm:sourceProperty": "/workEmail/address",
    "@id": "17aaebfa382ce8fc0a40d3e43870b6470aab894e1c368d16",
    "meta:containerId": "tenant",
    "version": "1",
    "imsOrg": "{IMS_ORG}"
}
```

## レコードデータのデータセットの作成

スキーマを作成したら、レコードデータを取り込むためのデータセットを作成する必要があります。

>[!NOTE]
>
>このデータセットは&#x200B;**[!DNL Real-time Customer Profile]**&#x200B;と&#x200B;**[!DNL Identity Service]**&#x200B;で有効になります。

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
  -d ' {
    "name": "Dataset name",
    "description": "Dataset description",
    "schemaRef": {
        "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID},
        "contentType": "application/vnd.adobe.xed-full+json;version=1"
    },
    "tags": {
        "unifiedIdentity": ["enabled:true"],
        "unifiedProfile": ["enabled:true"]
    }
}'
```

**応答** 

成功時の応答は、HTTP ステータス 201 と共に、新しく作成されたデータセットの ID を `@/dataSets/{DATASET_ID}` の形式で含む配列を返します。

```json
[
    "@/dataSets/5e30d7986c0cc218a85cee65
]
```

## ストリーミング接続へのレコードデータの取り込み  {#ingest-data}

データセットとストリーミング接続が確立された状態で、XDM形式のJSONレコードを取り込み、レコードデータを[!DNL Platform]に取り込むことができます。

**API 形式**

```http
POST /collection/{CONNECTION_ID}?synchronousValidation=true
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | 作成済みのストリーミング接続の `id` 値です。 |
| `synchronousValidation` | 開発用のクエリパラメーター（オプション）。`true` に設定した場合、リクエストが正常に送信されたかどうかを確認するために、即座のフィードバックに使用できます。デフォルトでは、この値は `false` に設定されています。 |

**リクエスト**

レコードデータをストリーミング接続に取り込むには、ソース名を使用するか、使用しないかを指定します。

次の例のリクエストでは、ソース名が見つからないレコードをプラットフォームに取り込みます。 レコードにソース名がない場合は、ストリーミング接続定義からソースIDが追加されます。

>[!NOTE]
>
> 以下の API 呼び出しでは、認証ヘッダーは&#x200B;**必要ありません**。

```shell
curl -X POST https://dcs.adobedc.net/collection/{CONNECTION_ID}?synchronousValidation=true \
  -H "Cache-Control: no-cache" \
  -H "Content-Type: application/json" \
  -d '{
    "header": {
        "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
            "contentType": "application/vnd.adobe.xed-full+json;version=1"
        },
        "imsOrgId": "{IMS_ORG}",
        "datasetId": "{DATASET_ID}"
    },
    "body": {
        "xdmMeta": {
            "schemaRef": {
                "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
                "contentType": "application/vnd.adobe.xed-full+json;version=1"
            }
        },
        "xdmEntity": {
            "person": {
                "name": {
                    "firstName": "Jane",
                    "middleName": "F",
                    "lastName": "Doe"
                },
                "birthDate": "1969-03-14",
                "gender": "female"
            },
            "workEmail": {
                "primary": true,
                "address": "janedoe@example.com",
                "type": "work",
                "status": "active"
            }
        }
    }
}'
```

ソース名を含める場合は、次の例にそのソース名を含める方法を示します。

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

正常に応答すると、HTTPステータス200が返され、新たにストリーミングされた[!DNL Profile]の詳細が返されます。

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
| `{CONNECTION_ID}` | 以前に作成したストリーミング接続の ID。 |
| `xactionId` | 送信したレコードに対してサーバー側で生成された一意の ID です。この IDは、様々なシステムやデバッグを通じて、アドビがこのレコードのライフサイクルを追跡するのに役立ちます。 |
| `receivedTimeMs` | リクエストが受信された時刻を示すタイムスタンプ（ミリ秒単位のエポックタイム）。 |
| `synchronousValidation.status` | `synchronousValidation=true` クエリパラメーターが追加されたので、この値が表示されます。検証が成功した場合、ステータスは `pass` になります。 |

## 新しく取り込んだレコードデータの取得

以前に取り込んだレコードを検証するには、[[!DNL Profile Access API]](../../profile/api/entities.md)を使用してレコードデータを取得します。

>[!NOTE]
>
>結合ポリシーIDが定義されておらず、`schema.name`または`relatedSchema.name`が`_xdm.context.profile`の場合、[!DNL Profile Access]は&#x200B;**すべての**&#x200B;関連IDを取得します。

**API 形式**

```http
GET /access/entities
GET /access/entities?{QUERY_PARAMETERS}
GET /access/entities?schema.name=_xdm.context.profile&entityId=janedoe@example.com&entityIdNS=email
```

| パラメーター | 説明 |
| --------- | ----------- |
| `schema.name` | **必須.**。アクセスするスキーマの名前です。 |
| `entityId` | エンティティの ID です。指定する場合は、エンティティの名前空間も指定します。 |
| `entityIdNS` | 取得しようとしている ID の名前空間。 |

**リクエスト**

既に取り込んだレコードデータは、次の GET リクエストで確認できます。

```shell
curl -X GET 'https://platform.adobe.io/data/core/ups/access/entities?schema.name=_xdm.context.profile&entityId=janedoe@example.com&entityIdNS=email'\
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

成功時の応答は、HTTP ステータス 200 と共に、要求されたエンティティの詳細を返します。これは以前に正常に取り込まれた記録と同じであることがわかります。

```json
{
    "BVrqzwVv7o2p3naHvnsWpqZXv3KJgA": {
        "entityId": "BVrqzwVv7o2p3naHvnsWpqZXv3KJgA",
        "mergePolicy": {
            "id": "e161dae9-52f0-4c7f-b264-dc43dd903d56"
        },
        "sources": [
            "5e30d7986c0cc218a85cee65"
        ],
        "tags": [
            "1580346827274:2478:215"
        ],
        "identityGraph": [
            "BVrqzwVv7o2p3naHvnsWpqZXv3KJgA"
        ],
        "entity": {
            "person": {
                "name": {
                    "lastName": "Doe",
                    "middleName": "F",
                    "firstName": "Jane"
                },
                "gender": "female",
                "birthDate": "1969-03-14"
            },
            "workEmail": {
                "type": "work",
                "address": "janedoe@example.com",
                "status": "active",
                "primary": true
            },
            "identityMap": {
                "email": [
                    {
                        "id": "janedoe@example.com"
                    }
                ]
            }
        },
        "lastModifiedAt": "2020-01-30T01:13:59Z"
    }
}
```

## 次の手順

このドキュメントを読むと、ストリーミング接続を使用して[!DNL Platform]にレコードデータを取り込む方法が理解できます。 異なる値でさらに呼び出しを実行し、更新された値を取得してみてください。さらに、[!DNL Platform] UIを使用して、取り込んだデータを開始で監視できます。 詳しくは、『[データ取得監視ガイド](../quality/monitor-data-ingestion.md)』を参照してください。

一般的なストリーミング取得の詳細については、『[ストリーミング取得の概要](../streaming-ingestion/overview.md)』を参照してください。
