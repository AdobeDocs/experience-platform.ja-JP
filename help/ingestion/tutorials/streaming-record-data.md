---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ストリーミングレコードデータ
topic: tutorial
translation-type: tm+mt
source-git-commit: 80392190c7fcae9b6e73cc1e507559f834853390
workflow-type: tm+mt
source-wordcount: '1059'
ht-degree: 2%

---


# ストリームレコードデータをAdobe Experience Platformに送信

このチュートリアルは、Adobe Experience PlatformAPIの一部であるストリーミング取り込みAPIの使用を開始する際に役立ち [!DNL Data Ingestion Service] ます。

## はじめに

このチュートリアルでは、さまざまなAdobe Experience Platformサービスに関する実用的な知識が必要です。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [!DNL Experience Data Model (XDM)](../../xdm/home.md): エクスペリエンスデータを [!DNL Platform] 編成するための標準化されたフレームワーク。
- [!DNL Real-time Customer Profile](../../profile/home.md): 複数のソースからの集計データに基づいて、リアルタイムで統合された顧客プロファイルを提供します。
- [スキーマレジストリ開発ガイド](../../xdm/api/getting-started.md): APIの使用可能な各エンドポイントと、それらのエンドポイントへの呼び出し方法をカバーする包括的なガイドです。 [!DNL Schema Registry] これには、このチュートリアル全体の呼び出しに表示されるユーザ `{TENANT_ID}`ー情報や、スキーマの作成方法を知ることが含まれます。この方法は、統合用のデータセットの作成に使用されます。

また、このチュートリアルでは、既にストリーミング接続を作成している必要があります。 ストリーミング接続の作成について詳しくは、「ストリーミング接続の [作成」チュートリアルを参照してください](./create-streaming-connection.md)。

以下の節では、ストリーミング取り込みAPIの呼び出しを正常に行うために知っておく必要がある追加情報について説明します。

### サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、トラブルシューティングガイドのAPI呼び出し例 [を読む方法に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照して [!DNL Experience Platform] ください。

### 必要なヘッダーの値の収集

APIを呼び出すには、まず [!DNL Platform] 認証チュートリアルを完了する必要があり [ます](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべての [!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定する

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

内のすべてのリソース [!DNL Experience Platform] は、特定の仮想サンドボックスに分離されます。 APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>のサンドボックスについて詳し [!DNL Platform]くは、 [Sandboxの概要ドキュメントを参照してください](../../sandboxes/home.md)。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

- Content-Type: application/json

## クラスに基づいてスキーマを作成する [!DNL XDM Individual Profile]

データセットを作成するには、まず、そのク [!DNL XDM Individual Profile] ラスを実装する新しいスキーマを作成する必要があります。 スキーマの作成方法について詳しくは、 [スキーマレジストリAPI開発者ガイドを参照してください](../../xdm/api/getting-started.md)。

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
| `title` | スキーマに使用する名前。 この名前は一意にする必要があります。 |
| `description` | 作成しているスキーマに関するわかりやすい説明。 |
| `meta:immutableTags` | この例では、 `union` タグを使用してデータを保持しま [!DNL Real-time Customer Profile](../../profile/home.md)す。 |

**応答**

正常に応答すると、新しく作成したスキーマの詳細と共にHTTPステータス201が返されます。

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
| `{TENANT_ID}` | このIDは、作成するリソースの名前が適切に指定され、IMS組織内に含まれていることを確認するために使用されます。 テナントIDの詳細については、 [スキーマレジストリガイドを参照してください](../../xdm/api/getting-started.md#know-your-tenant-id)。 |

データセットを作成する際 `$id` には、これらの両方が使用されるので、 `version` 属性と共にこれらも注意してください。

## スキーマのプライマリID記述子を設定します

次に、 [ID記述子](../../xdm/api/descriptors.md) を上で作成したスキーマに追加します。プライマリ識別子として、work email address属性を使用します。 これを行うと、次の2つの変更が行われます。

1. 勤務先の電子メールアドレスは必須フィールドになります。 これは、このフィールドを使用せずに送信されたメッセージは検証に失敗し、取り込まれないことを意味します。

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
| `{SCHEMA_REF_ID}` | スキーマ `$id` を作成した際に以前に受け取ったもの。 次のようになります。 `"https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}"` |

>[!NOTE]
>
>&#x200B;ID&#x200B;名前空間コード&#x200B;****
>
> コードが有効であることを確認してください。上の例では、標準的なID名前空間である「email」が使用されています。 一般的に使用されるその他の標準ID名前空間は、「 [IDサービスFAQ](../../identity-service/troubleshooting-guide.md#what-are-the-standard-identity-namespaces-provided-by-experience-platform)」に記載されています。
>
> カスタム名前空間を作成する場合は、「 [ID名前空間の概要](../../identity-service/home.md)」に説明されている手順に従います。

**応答**

成功した応答は、新たに作成されたスキーマのプライマリID記述子に関する情報と共にHTTPステータス201を返す。

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

スキーマの作成後は、レコードデータを取り込むデータセットを作成する必要があります。

>[!NOTE]
>
>このデータセットは、 **[!DNL Real-time Customer Profile]** およびで有効になり **[!DNL Identity Service]**&#x200B;ます。

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

正常に応答した場合、HTTPステータス201と、新しく作成されたデータセットのIDを含む配列が形式で返され `@/dataSets/{DATASET_ID}`ます。

```json
[
    "@/dataSets/5e30d7986c0cc218a85cee65
]
```

## レコードデータをストリーミング接続に取り込みます

データセットとストリーミング接続が確立された状態で、XDM形式のJSONレコードを取り込み、レコードデータを取り込むことができ [!DNL Platform]ます。

**API形式**

```http
POST /collection/{CONNECTION_ID}?synchronousValidation=true
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | 以前に作成したストリーミング接続の `id` 値。 |
| `synchronousValidation` | 開発を目的としたオプションのクエリパラメーターです。 に設定した場合 `true`は、リクエストが正常に送信されたかどうかを確認するためのフィードバックを即時に使用できます。 デフォルトでは、この値はに設定されてい `false`ます。 |

**リクエスト**

>[!NOTE]
>
>次のAPI呼び出しでは、認証ヘッダーは **必要ありません** 。

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

正常な応答が返されると、HTTPステータス200が返され、新たにストリーム化されたHTTPステータスの詳細が返され [!DNL Profile]ます。

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
| `xactionId` | 先ほど送信したレコード用にサーバー側で生成された一意の識別子。 このIDは、様々なシステムを介して、デバッグ機能を備えたこのレコードのライフサイクルをアドビが追跡するのに役立ちます。 |
| `receivedTimeMs` | リクエストが受信された時間を示すタイムスタンプ(エポック（ミリ秒）。 |
| `synchronousValidation.status` | クエリパラメーター `synchronousValidation=true` が追加されたので、この値が表示されます。 検証が成功した場合は、ステータスがになり `pass`ます。 |

## 新しく取り込んだレコードデータを取得する

以前に取り込んだレコードを検証するには、を使用してレコードデータ [!DNL Profile Access API](../../profile/api/entities.md) を取得します。

>[!NOTE]
>
>マージポリシーIDが定義されていない場合とスキーマ。</span>nameまたはrelatedSchema</span>.nameは `_xdm.context.profile`、 [!DNL Profile Access] す **べての** 関連IDを取得します。

**API形式**

```http
GET /access/entities
GET /access/entities?{QUERY_PARAMETERS}
GET /access/entities?schema.name=_xdm.context.profile&entityId=janedoe@example.com&entityIdNS=email
```

| パラメーター | 説明 |
| --------- | ----------- |
| `schema.name` | **必須。** アクセスしているスキーマの名前。 |
| `entityId` | エンティティのID。 指定する場合は、エンティティ名前空間も指定する必要があります。 |
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

正常に応答すると、HTTPステータス200が返され、要求されたエンティティの詳細が返されます。 ご覧の通り、これは初めに正常に摂取された記録と同じです。

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

このドキュメントを読むと、記録データをストリーミング接続に取り込む方法が理解でき [!DNL Platform] ます。 様々な値を持つ呼び出しをさらに試して、更新された値を取得できます。 また、 [!DNL Platform] UIを使用して、取り込んだデータを開始で監視することもできます。 詳しくは、『 [監視データ取り込み](../quality/monitor-data-flows.md) 』ガイドを参照してください。

一般的なストリーミング取り込みの詳細については、 [ストリーミング取り込みの概要を参照してください](../streaming-ingestion/overview.md)。


