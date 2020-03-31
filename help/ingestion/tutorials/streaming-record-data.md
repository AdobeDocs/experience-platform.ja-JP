---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ストリーミングレコードデータ
topic: tutorial
translation-type: tm+mt
source-git-commit: 79466c78fd78c0f99f198b11a9117c946736f47a

---


# Adobe Experience Platformへのレコードデータのストリーミング

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

## XDM個々のスキーマクラスを基にプロファイルを作成

データセットを作成するには、まずXDM Individualプロファイルクラスを実装する新しいスキーマを作成する必要があります。 スキーマの作成方法について詳しくは、 [スキーマレジストリAPI開発者ガイドを参照してください](../../xdm/api/getting-started.md)。

**API形式**

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
| `{SCHEMA_REF_ID}` | 以前 `$id` に受信したスキーマ。 次のようになります。 `"https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}"` |

>[!NOTE] ID&#x200B;&#x200B;**名前空間コード**
>
> コードが有効であることを確認してください。上の例では、標準のID名前空間である「email」を使用しています。 その他の一般的に使用される標準ID名前空間は、IDサービスのFAQ [に記載されています](../../identity-service/troubleshooting-guide.md#what-are-the-standard-identity-namespaces-provided-by-experience-platform)。
>
> カスタム名前空間を作成する場合は、「ID名前空間の概要」で概要を説明している手順に従 [います](../../identity-service/home.md)。

**応答**

正常な応答は、新たに作成されたプライマリID記述子に関する情報と共に、HTTPステータス201をスキーマに返します。

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

レコードデータを取り込むスキーマセットを作成する必要があります。

>[!NOTE] このデータセットは、リアルタイム **顧客プロファイル** / **IDサービスで有効になります**。

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
  -d ' {
    "name": "Dataset name",
    "description": "Dataset description",
    "schemaRef": {
        "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID},
        "contentType": "application/vnd.adobe.xed-full+json;version=1.0"
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
    "@/dataSets/5e30d7986c0cc218a85cee65
]
```

## レコードデータをストリーミング接続に取り込む

データセットとストリーミング接続が確立された状態で、XDM形式のJSONレコードを取り込み、レコードデータをプラットフォームに取り込むことができます。

**API形式**

```http
POST /collection/{CONNECTION_ID}?synchronousValidation=true
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | 以前に `id` 作成したストリーミング接続の値。 |
| `synchronousValidation` | 開発目的のクエリパラメーター（オプション）。 に設定した場合、リ `true`クエストが正常に送信されたかどうかを確認するために、即座のフィードバックに使用できます。 デフォルトでは、この値はに設定されていま `false`す。 |

**リクエスト**

>[!NOTE] 次のAPI呼び出しでは、認証ヘ **ッダー** は必要ありません。

```shell
curl -X POST https://dcs.adobedc.net/collection/{CONNECTION_ID}?synchronousValidation=true \
  -H "Cache-Control: no-cache" \
  -H "Content-Type: application/json" \
  -d '{
    "header": {
        "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
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

**応答**

成功した応答は、新しくストリーミングされた応答の詳細を含むHTTPステータス200を返します。プロファイル

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
| `receivedTimeMs` | リクエストが受信された時刻を示すタイムスタンプ（ミリ秒単位のエポック）。 |
| `synchronousValidation.status` | クエリパラメー `synchronousValidation=true` ターが追加されたので、この値が表示されます。 検証が成功した場合、ステータスは次のようになりま `pass`す。 |

## 新しく取り込んだレコードデータを取得する

以前に取り込んだレコードを検証するには、 [プロファイルアクセスAPI](../../profile/api/entities.md) （レコードデータ）を使用します。

>[!NOTE] マージポリシーIDが定義されていない場合、およびスキーマ。</span>nameまたはrelatedSchema</span>.nameがです。 `_xdm.context.profile`プロファイルアクセスは関連するすべ **ての** IDを取得します。

**API形式**

```http
GET /access/entities
GET /access/entities?{QUERY_PARAMETERS}
GET /access/entities?schema.name=_xdm.context.profile&entityId=janedoe@example.com&entityIdNS=email
```

| パラメーター | 説明 |
| --------- | ----------- |
| `schema.name` | **必須.** アクセスするスキーマの名前。 |
| `entityId` | エンティティのID。 指定する場合は、エンティティの名前空間も指定します。 |
| `entityIdNS` | 取得しようとしているIDの名前空間。 |

**リクエスト**

以前に取り込んだレコードデータは、次のGETリクエストで確認できます。

```shell
curl -X GET 'https://platform.adobe.io/data/core/ups/access/entities?schema.name=_xdm.context.profile&entityId=janedoe@example.com&entityIdNS=email'\
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、要求されたエンティティの詳細と共にHTTPステータス200を返します。 ご覧の通り、これは以前に正常に取り込まれたのと同じ記録です。

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

このドキュメントを読むと、ストリーミング接続を使用して、レコードデータをプラットフォームに取り込む方法を理解できます。 様々な値を持つ呼び出しをさらに行い、更新された値を取得してみることができます。 さらに、プラットフォームUIを使用して、開始による取り込んだデータの監視も可能です。 詳しくは、監視データ取り込みガイド [を参照してください](../quality/monitor-data-flows.md) 。

一般的なストリーミング取り込みの詳細については、ストリーミング取り込みの概要を [参照してくださ](../streaming-ingestion/overview.md)い。


