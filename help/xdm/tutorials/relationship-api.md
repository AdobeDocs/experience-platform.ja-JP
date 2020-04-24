---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: レジストリAPIを使用して2つのスキーマ間のスキーマを定義する
topic: tutorials
translation-type: tm+mt
source-git-commit: 7e867ee12578f599c0c596decff126420a9aca01

---


# レジストリAPIを使用して2つのスキーマ間のスキーマを定義する


様々なチャネルでの顧客とブランドとの関係を理解する能力は、Adobe Experience Platformの重要な部分です。 エクスペリエンスデータモデル(XDM)スキーマの構造内でこれらの関係を定義すると、顧客データに対する複雑なインサイトを得ることができます。

このドキュメントでは、 [スキーマレジストリAPIを使用して、組織が定義した2つのスキーマ間の1対1の関係を定義するためのチュートリアルを提供します](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml)。

## はじめに

このチュートリアルでは、Experience Data Model(XDM)とXDM Systemに関する十分な理解が必要です。 このチュートリアルを開始する前に、次のドキュメントを確認してください。

* [Experience PlatformのXDM System](../home.md):XDMとエクスペリエンスプラットフォームでの実装の概要を示します。
   * [スキーマ構成の基本](../schema/composition.md):XDMスキーマの構築ブロックの紹介。
* [リアルタイム顧客プロファイル](../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムのプロファイルを顧客に提供します。
* [サンドボックス](../../sandboxes/home.md):Experience Platformは、デジタルエクスペリエンスアプリケーションの開発と発展を支援するために、単一のプラットフォームインスタンスを別々の仮想環境に分割する仮想サンドボックスを提供します。

このチュートリアルを開始する前に、開発者 [ガイドを参照し](../api/getting-started.md) 、スキーマレジストリAPIを正しく呼び出すために知っておく必要がある重要な情報を確認してください。 これには、ユーザ `{TENANT_ID}`ー、「コンテナ」の概念、リクエストを行うために必要なヘッダー（Acceptヘッダーとその可能な値に特に注意）が含まれます。

## ソースと宛先のスキーマ {#define-schemas}

この関係で定義される2つのスキーマが既に作成されていると想定されます。 このチュートリアルでは、組織の現在の忠誠度プログラム(「忠誠度メンバー」スキーマで定義)のメンバーと、お気に入りのホテル(「ホテル」スキーマで定義)との間に関係を作成します。

スキーマ関係は、宛先スキーマ **内の** 、別のフィールドを参照するフィールドを有するソーススキーマで表 **されます**。 次の手順では、「Loyalty Members」がソーススキーマになり、「Hotels」がターゲットスキーマになります。

2つのスキーマ間の関係を定義するには、まず両方の関係の値を取得す `$id` る必要があります。 スキーマの表示名(`title`)がわかっている場合は、スキーマレジストリAPIでエンドポイントにGET `$id` リクエストを作成することで、 `/tenant/schemas` その値を見つけることができます。

**API形式**

```http
GET /tenant/schemas
```

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Accept: application/vnd.adobe.xed-id+json'
```

>[!NOTE] Acceptヘッダーは、結 `application/vnd.adobe.xed-id+json` 果として生成されるスキーマのタイトル、IDおよびバージョンのみを返します。

**応答**

成功した応答は、組織が定義したスキーマのリスト(組織の、、、お `name`よびを含 `$id`む)を `meta:altId`返しま `version`す。

```json
{
    "results": [
        {
            "title": "Newsletter Subscriptions",
            "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/192a66930afad02408429174c311ae73",
            "meta:altId": "_{TENANT_ID}.schemas.192a66930afad02408429174c311ae73",
            "version": "1.2"
        },
        {
            "title": "Loyalty Members",
            "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/2c66c3a4323128d3701289df4468e8a6",
            "meta:altId": "_{TENANT_ID}.schemas.2c66c3a4323128d3701289df4468e8a6",
            "version": "1.5"
        },
        {
            "title": "Hotels",
            "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/d4ad4b8463a67f6755f2aabbeb9e02c7",
            "meta:altId": "_{TENANT_ID}.schemas.d4ad4b8463a67f6755f2aabbeb9e02c7",
            "version": "1.0"
        }
    ],
    "_page": {
        "orderby": "updated",
        "next": null,
        "count": 3
    },
    "_links": {
        "next": null,
        "global_schemas": {
            "href": "https://platform-stage.adobe.io/data/foundation/schemaregistry/global/schemas"
        }
    }
}
```

関係を定 `$id` 義する2つのスキーマの値を記録します。 これらの値は、後の手順で使用します。

## 両方の参照フィールドの定義スキーマ

スキーマ・レジストリ内では、リレーションシップ記述子はSQLテーブルの外部キーと同様に機能します。ソーススキーマ内のフィールドは、ターゲットスキーマのフィールドへの参照として機能します。 関係を定義する場合、各スキーマは、他の関係への参照として使用する専用のフィールドを持つ必要があります。スキーマ

>[!IMPORTANT] スキーマをリアルタイム顧客プロファイルで使用できるようにする場合 [、宛先スキーマの参照フィールドはプライマリIDである必要](../../profile/home.md)があります ****。 これについては、このチュートリアルで後述します。

どちらのスキーマにもこの目的のフィールドがない場合は、新しいフィールドとのミックスインを作成し、スキーマに追加する必要があります。 この新しいフィールドの値 `type` は&quot;string&quot;である必要があります。

このチュートリアルの目的上、宛先スキーマ「Hotels」には、この目的のフィールドが既に含まれています。 `hotelId`. ただし、ソーススキーマ「忠誠度メンバー」にはこのようなフィールドがないので、名前空間の下に新しいフィールドを追加する新しいミックスインを与える `favoriteHotel`必要があ `TENANT_ID` ります。

### 新しいミックスインの作成

新しいフィールドをスキーマに追加するには、まずミックスインで定義する必要があります。 エンドポイントにPOSTリクエストを作成することで、新しいミックスインを作成で `/tenant/mixins` きます。

**API形式**

```http
POST /tenant/mixins
```

**リクエスト**

次のリクエストは、新しいミックスインを作成し、追加先の任意の `favoriteHotel` スキーマの `TENANT_ID` 名前空間の下にフィールドを追加します。

```shell
curl -X POST\
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/mixins \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'content-type: application/json' \
  -d '{
        "type": "object",
        "title": "Favorite Hotel",
        "meta:intendedToExtend": ["https://ns.adobe.com/xdm/context/profile"],
        "description": "Favorite hotel mixin for the Loyalty Members schema.",
        "definitions": {
            "favoriteHotel": {
              "properties": {
                "_{TENANT_ID}": {
                  "type":"object",
                  "properties": {
                    "favoriteHotel": {
                      "title": "Favorite Hotel",
                      "type": "string",
                      "description": "Favorite hotel for a Loyalty Member."
                    }
                  }
                }
              }
            }
        },
        "allOf": [
            {
              "$ref": "#/definitions/favoriteHotel"
            }
        ]
      }'
```

**応答**

成功した応答は、新しく作成されたミックスインの詳細を返します。

```json
{
    "$id": "https://ns.adobe.com/{TENANT_ID}/mixins/3387945212ad76ee59b6d2b964afb220",
    "meta:altId": "_{TENANT_ID}.mixins.3387945212ad76ee59b6d2b964afb220",
    "meta:resourceType": "mixins",
    "version": "1.0",
    "type": "object",
    "title": "Favorite Hotel",
    "meta:intendedToExtend": [
        "https://ns.adobe.com/xdm/context/profile"
    ],
    "description": "Favorite hotel mixin for the Loyalty Members schema.",
    "definitions": {
        "favoriteHotel": {
            "properties": {
                "_{TENANT_ID}": {
                    "type": "object",
                    "properties": {
                        "favoriteHotel": {
                            "title": "Favorite Hotel",
                            "type": "string",
                            "description": "Favorite hotel for a Loyalty Member.",
                            "meta:xdmType": "string"
                        }
                    },
                    "meta:xdmType": "object"
                }
            },
            "type": "object",
            "meta:xdmType": "object"
        }
    },
    "allOf": [
        {
            "$ref": "#/definitions/favoriteHotel"
        }
    ],
    "meta:xdmType": "object",
    "meta:abstract": true,
    "meta:extensible": true,
    "meta:containerId": "tenant",
    "meta:tenantNamespace": "_{TENANT_ID}",
    "meta:registryMetadata": {
        "eTag": "quM2aMPyb2NkkEiZHNCs/MG34E4=",
        "palm:sandboxName": "prod"
    }
}
```

| プロパティ | 説明 |
| --- | --- |
| `$id` | 読み取り専用で、新しいミックスインの一意の識別子が生成されました。 URIの形式を取ります。 |

ミックス `$id` インをソーススキーマに追加する次の手順で使用する、ミックスインのURIを記録します。

### 追加ソーススキーマ

ミックスインを作成したら、エンドポイントにPATCHリクエストを作成して、そのミックスインをソーススキーマに追加で `/tenant/schemas/{SCHEMA_ID}` きます。

**API形式**

```http
PATCH /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | URLエンコードされた `$id` URIまたはソ `meta:altId` ーススキーマ。 |

**リクエスト**

次のリクエストでは、「お気に入りのホテル」ミックスインが「ロイヤルティメンバー」スキーマに追加されます。

```shell
curl -X PATCH \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.533ca5da28087c44344810891b0f03d9 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '[
    { 
      "op": "add", 
      "path": "/allOf/-", 
      "value":  {
        "$ref": "https://ns.adobe.com/{TENANT_ID}/mixins/3387945212ad76ee59b6d2b964afb220"
      }
    }
  ]'
```

| プロパティ | 説明 |
| --- | --- |
| `op` | 実行するPATCH操作。 このリクエストでは、操作が使用 `add` されます。 |
| `path` | 新しいリソースを追加するスキーマフィールドへのパスを指定します。 ミックスインをスキーマに追加する場合、値はである必要がありま `/allOf/-`す。 |
| `value.$ref` | 追加 `$id` するミックスイン。 |

**応答**

正常に応答すると、更新されたスキーマの詳細が返されます。これには、追加されたミッ `$ref` クスインの値が配列に含まれ `allOf` ます。

```json
{
    "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/2c66c3a4323128d3701289df4468e8a6",
    "meta:altId": "_{TENANT_ID}.schemas.2c66c3a4323128d3701289df4468e8a6",
    "meta:resourceType": "schemas",
    "version": "1.1",
    "type": "object",
    "title": "Loyalty Members",
    "description": "",
    "allOf": [
        {
            "$ref": "https://ns.adobe.com/xdm/context/profile"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/profile-person-details"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/profile-personal-details"
        },
        {
            "$ref": "https://ns.adobe.com/{TENANT_ID}/mixins/ec16dfa484358f80478b75cde8c430d3"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/identitymap"
        },
        {
            "$ref": "https://ns.adobe.com/{TENANT_ID}/mixins/3387945212ad76ee59b6d2b964afb220"
        }
    ],
    "meta:containerId": "tenant",
    "meta:class": "https://ns.adobe.com/xdm/context/profile",
    "meta:abstract": false,
    "meta:extensible": false,
    "meta:tenantNamespace": "_{TENANT_ID}",
    "imsOrg": "{IMS_ORG}",
    "meta:extends": [
        "https://ns.adobe.com/xdm/context/profile",
        "https://ns.adobe.com/xdm/data/record",
        "https://ns.adobe.com/xdm/context/identitymap",
        "https://ns.adobe.com/xdm/common/extensible",
        "https://ns.adobe.com/xdm/common/auditable",
        "https://ns.adobe.com/xdm/context/profile-person-details",
        "https://ns.adobe.com/xdm/context/profile-personal-details",
        "https://ns.adobe.com/{TENANT_ID}/mixins/ec16dfa484358f80478b75cde8c430d3",
        "https://ns.adobe.com/{TENANT_ID}/mixins/61969bc646b66a6230a7e8840f4a4d33"
    ],
    "meta:xdmType": "object",
    "meta:registryMetadata": {
        "repo:createdDate": 1557525483804,
        "repo:lastModifiedDate": 1566419670915,
        "xdm:createdClientId": "{API_KEY}",
        "xdm:lastModifiedClientId": "{CLIENT_ID}",
        "eTag": "ITNzu8BVTO5pw9wfCtTTpk6U4WY="
    }
}
```

## 両方のユーザーの主IDフィールドのスキーマ

>[!NOTE] この手順は、スキーマがリアルタイム顧客プロファイルで使用できる [場合にのみ必要です](../../profile/home.md)。 どちらのスキーマも和集合に参加したくない場合や、スキーマにプライマリIDが既に定義されている場合は、宛先スキーマの参照ID記述子の作成の次の手順に進む [こと](#create-descriptor) ができます。

スキーマをリアルタイム顧客プロファイルで使用できるようにするには、プライマリIDが定義されている必要があります。 さらに、関係の宛先スキーマは、主IDを参照フィールドとして使用する必要があります。

このチュートリアルの目的では、ソーススキーマに既にプライマリIDが定義されていますが、宛先スキーマは定義されていません。 ID記述子を作成することで、スキーマフィールドをプライマリIDフィールドとしてマークすることができます。 これは、エンドポイントにPOSTリクエストを行うことで行わ `/tenant/descriptors` れます。

**API形式**

```http
POST /tenant/descriptors
```

**リクエスト**

次のリクエストでは、宛先スキーマ「Hotels」のフィールドをプライマリID `hotelId` フィールドとして定義する新しいID記述子を作成します。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
    "@type": "xdm:descriptorIdentity",
    "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/d4ad4b8463a67f6755f2aabbeb9e02c7",
    "xdm:sourceVersion": 1,
    "xdm:sourceProperty": "/_{TENANT_ID}/hotelId",
    "xdm:namespace": "ECID",
    "xdm:property": "xdm:code",
    "xdm:isPrimary": true
  }'
```

| パラメーター | 説明 |
| --- | --- |
| `@type` | 作成する記述子のタイプ。 ID記 `@type` 述子の値はです `xdm:descriptorIdentity`。 |
| `xdm:sourceSchema` | 前の手 `$id` 順で取得した宛先スキーマの [値です](#define-schemas)。 |
| `xdm:sourceVersion` | バージョンのスキーマ番号。 |
| `sourceProperty` | スキーマのプライマリIDとして機能する特定のフィールドへのパス。 このパスは、「/」で始まり、「/」で終わらないでください。また、「プロパティ」名前空間も除外します。 例えば、上記のリクエストでは、の代わりにが使 `/_{TENANT_ID}/hotelId` 用されていま `/properties/_{TENANT_ID}/properties/hotelId`す。 |
| `xdm:namespace` | ID名前空間フィールドのIDフィールド。 `hotelId` はこの例のECID値なので、「ECID」名前空間が使用されます。 使用可能な [名前空間のリストについては](../../identity-service/home.md) 、ID名前空間の概要を参照してください。 |
| `xdm:isPrimary` | IDフィールドをユーザーのプライマリIDにするかどうかを指定するブールスキーマプロパティ。 このリクエストはプライマリIDを定義するので、値はtrueに設定されます。 |

**応答**

正常な応答は、新しく作成されたID記述子の詳細を返します。

```json
{
    "@type": "xdm:descriptorIdentity",
    "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/d4ad4b8463a67f6755f2aabbeb9e02c7",
    "xdm:sourceVersion": 1,
    "xdm:sourceProperty": "/_{TENANT_ID}/hotelId",
    "xdm:namespace": "ECID",
    "xdm:property": "xdm:code",
    "xdm:isPrimary": true,
    "meta:containerId": "tenant",
    "@id": "e3cfa302d06dc27080e6b54663511a02dd61316f"
}
```

## 参照ID記述子の作成

スキーマフィールドは、関係内の他の要素からの参照として使用される場合、参照ID記述子を適用する必要があります。 「忠誠度メ `favoriteHotel` ンバー」のフィールドは「ホテル」のフ `hotelId` ィールドを参照するので、参照ID記 `hotelId` 述子を与える必要があります。

エンドポイントにPOSTリクエストを行って、宛先スキーマの参照記述子を作成 `/tenant/descriptors` します。

**API形式**

```http
POST /tenant/descriptors
```

**リクエスト**

次のリクエストは、宛先ディスクリプター「Hotels」内のフ `hotelId` ィールドの参照スキーマを作成します。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
    "@type": "xdm:descriptorReferenceIdentity",
    "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/d4ad4b8463a67f6755f2aabbeb9e02c7",
    "xdm:sourceVersion": 1,
    "xdm:sourceProperty": "/_{TENANT_ID}/hotelId",
    "xdm:identityNamespace": "ECID"
  }'
```

| パラメーター | 説明 |
| --- | --- |
| `xdm:sourceSchema` | リンク `$id` 先スキーマのURL。 |
| `xdm:sourceVersion` | 宛先バージョンのスキーマ番号。 |
| `sourceProperty` | 宛先スキーマのプライマリIDフィールドへのパス。 |
| `xdm:identityNamespace` | 参照フィールドのID名前空間です。 `hotelId` はこの例のECID値なので、「ECID」名前空間が使用されます。 使用可能な [名前空間のリストについては](../../identity-service/home.md) 、ID名前空間の概要を参照してください。 |

**応答**

正常な応答は、新たに作成された宛先ディスクリプタの詳細を返します。スキーマ。

```json
{
    "@type": "xdm:descriptorReferenceIdentity",
    "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/d4ad4b8463a67f6755f2aabbeb9e02c7",
    "xdm:sourceVersion": 1,
    "xdm:sourceProperty": "/_{TENANT_ID}/hotelId",
    "xdm:identityNamespace": "ECID",
    "meta:containerId": "tenant",
    "@id": "53180e9f86eed731f6bf8bf42af4f59d81949ba6"
}
```

## 関係記述子の作成 {#create-descriptor}

関係記述子は、ソース・ディスクリプタと宛先スキーマとの間に1対1の関係を確立します。 エンドポイントにPOSTリクエストを作成することで、新しい関係記述子を作成で `/tenant/descriptors` きます。

**API形式**

```http
POST /tenant/descriptors
```

**リクエスト**

次の要求では、新しい関係記述子が作成されます。この際、「Loyalty Members」がソーススキーマ、「Legacy Loyality Members」が宛先スキーマとして作成されます。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
    "@type": "xdm:descriptorOneToOne",
    "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/2c66c3a4323128d3701289df4468e8a6",
    "xdm:sourceVersion": 1,
    "xdm:sourceProperty": "/_{TENANT_ID}/favoriteHotel",
    "xdm:destinationSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/d4ad4b8463a67f6755f2aabbeb9e02c7",
    "xdm:destinationVersion": 1,
    "xdm:destinationProperty": "/_{TENANT_ID}/hotelId"
  }'
```

| パラメーター | 説明 |
| --- | --- |
| `@type` | 作成する記述子のタイプ。 関係記 `@type` 述子の値はです `xdm:descriptorOneToOne`。 |
| `xdm:sourceSchema` | ソー `$id` ススキーマのURL。 |
| `xdm:sourceVersion` | ソースバージョンのスキーマ番号。 |
|  `sourceProperty`。 | ソーススキーマ内の参照フィールドへのパス。 |
| `xdm:destinationSchema` | リンク `$id` 先スキーマのURL。 |
| `xdm:destinationVersion` | 宛先バージョンのスキーマ番号。 |
|  `destinationProperty`。 | リンク先スキーマの参照フィールドへのパス。 |

### 応答

正常な応答は、新しく作成された関係記述子の詳細を返します。

```json
{
    "@type": "xdm:descriptorOneToOne",
    "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/2c66c3a4323128d3701289df4468e8a6",
    "xdm:sourceVersion": 1,
    "xdm:sourceProperty": "/_{TENANT_ID}/favoriteHotel",
    "xdm:destinationSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/d4ad4b8463a67f6755f2aabbeb9e02c7",
    "xdm:destinationVersion": 1,
    "xdm:destinationProperty": "/_{TENANT_ID}/hotelId",
    "meta:containerId": "tenant",
    "@id": "76f6cc7105f4eaab7eb4a5e1cb4804cadc741669"
}
```

## 次の手順

このチュートリアルに従うと、2つのスキーマ間に1対1の関係を作成できます。 スキーマレジストリAPIを使用した記述子の操作について詳しくは、 [スキーマレジストリ開発者ガイドを参照してください](../api/getting-started.md)。 UIでスキーマの関係を定義する手順については、スキーマエディタを使用したスキーマ [の関係の定義に関するチュートリアルを参照してください](relationship-ui.md)。