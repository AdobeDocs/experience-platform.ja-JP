---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: スキーマレジストリAPIを使用して2つのスキーマ間の関係を定義する
topic: tutorials
translation-type: tm+mt
source-git-commit: d04bf35e49488ab7d5e07de91eb77d0d9921b6fa
workflow-type: tm+mt
source-wordcount: '1467'
ht-degree: 1%

---


# APIを使用して2つのスキーマ間の関係を定義する [!DNL Schema Registry]


様々なチャネルにわたる顧客とブランドとの関係を理解する能力は、Adobe Experience Platformの重要な部分です。 これらの関係を [!DNL Experience Data Model] (XDM)スキーマの構造内で定義すると、顧客データに対する複雑な洞察を得ることができます。

このドキュメントでは、を使用して、組織で定義されている2つのスキーマ間の1対1の関係を定義するためのチュートリアルを提供し [!DNL Schema Registry API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml)ます。

## はじめに

このチュートリアルでは、 [!DNL Experience Data Model] (XDM)との詳細を理解している必要があり [!DNL XDM System]ます。 このチュートリアルを開始する前に、次のドキュメントを確認してください。

* [Experience PlatformのXDMシステム](../home.md): XDMとExperience Platformでのその実装の概要を示します。
   * [スキーマ構成の基本](../schema/composition.md): XDMスキーマの構築ブロックの紹介。
* [!DNL Real-time Customer Profile](../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [!DNL Sandboxes](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

このチュートリアルを開始する前に、 [開発者ガイドを参照して](../api/getting-started.md) 、 [!DNL Schema Registry] APIの呼び出しを正常に行うために知っておく必要がある重要な情報を確認してください。 例えば、ユーザー `{TENANT_ID}`、「コンテナ」の概念、リクエストを行う際に必要なヘッダー（Acceptヘッダーとその可能な値に特に注意）などがあります。

## ソースと宛先のスキーマの定義 {#define-schemas}

この関係で定義される2つのスキーマを既に作成済みであることが想定されます。 このチュートリアルでは、組織の現在の忠誠度プログラム(「忠誠度メンバー」スキーマで定義)のメンバーと、お気に入りのホテル(「ホテル」スキーマで定義)との間に関係を作成します。

スキーマ関係は、 **[!UICONTROL ソーススキーマ]** ( **[!UICONTROL ソーススキーマ]**)が表し、ターゲット内の別のフィールドを参照するフィールドを持ちます。 次の手順では、「[!UICONTROL Loyalty Members]」がソーススキーマになり、「[!UICONTROL Hotels]」がターゲットスキーマになります。

2つのスキーマ間の関係を定義するには、まず両方のスキーマの `$id` 値を取得する必要があります。 スキーマの表示名(`title`)がわかっている場合は、 `$id` APIの `/tenant/schemas`[!DNL Schema Registry] エンドポイントにGETリクエストを行うと、その値を見つけることができます。

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

>[!NOTE]
>
>Acceptヘッダーは、結果のスキーマのタイトル、IDおよびバージョンのみを返します。 `application/vnd.adobe.xed-id+json`

**応答**

成功した場合は、組織で定義されたスキーマ（ユーザー、ユーザー、ユーザー、およびなど）のリストが返され `name``$id``meta:altId``version`ます。

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

関係を定義する2つのスキーマの `$id` 値を記録します。 これらの値は、後の手順で使用します。

## 両方のスキーマの参照フィールドの定義

リレーションシップ記述子 [!DNL Schema Registry]は、SQLテーブルの外部キーと同様に機能します。 ソーススキーマのフィールドは、ターゲットスキーマのフィールドへの参照として機能します。 関係を定義する場合、他のスキーマへの参照として使用する各スキーマには専用のフィールドが必要です。

>[!IMPORTANT]
>
>スキーマをでの使用を有効にする場合は、 [!DNL Real-time Customer Profile](../../profile/home.md)宛先スキーマの参照フィールドを **[!UICONTROL 主IDにする必要があります]**。 これについては、このチュートリアルで後述します。

どちらのスキーマにもこの目的のフィールドがない場合は、新しいフィールドを使用してミックスインを作成し、スキーマに追加する必要があります。 この新しいフィールドの `type` 値は、&quot;string&quot;である必要があります。

このチュートリアルの目的上、リンク先スキーマ「Hotels」には、次のフィールドが既に含まれています。 `hotelId`. ただし、ソーススキーマ「忠誠度メンバー」にはこのようなフィールドがないので、 `favoriteHotel``TENANT_ID` 名前空間の下に新しいフィールドを追加する新しいミックスインを与える必要があります。

### 新しいミックスインの作成

スキーマに新しいフィールドを追加するには、まずmixinで定義する必要があります。 エンドポイントにPOSTリクエストを作成して、新しいミックスインを作成でき `/tenant/mixins` ます。

**API形式**

```http
POST /tenant/mixins
```

**リクエスト**

次の要求は、新しいミックスインを作成し、追加先のスキーマの `favoriteHotel``TENANT_ID` 名前空間の下にフィールドを追加します。

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

正常に完了した応答は、新たに作成されたmixinの詳細を返します。

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
| `$id` | 読み取り専用で、システムが生成した新しいミックスインの一意の識別子。 URIの形式を取ります。 |

mixinをソーススキーマに追加する次の手順で使用するmixinの `$id` URIを記録します。

### ソーススキーマ追加へのミックスイン

ミックスインを作成したら、エンドポイントにPATCHリクエストを作成して、そのミックスインをソーススキーマに追加でき `/tenant/schemas/{SCHEMA_ID}` ます。

**API形式**

```http
PATCH /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | URLエンコードされた `$id` URIまたはソーススキーマ `meta:altId` のURLです。 |

**リクエスト**

次のリクエストは、「お気に入りのホテル」ミックスインを「ロイヤルティメンバー」スキーマに追加します。

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
| `op` | 実行するPATCH操作。 このリクエストでは、 `add` 操作が使用されます。 |
| `path` | 新しいリソースを追加するスキーマフィールドへのパスです。 スキーマにミックスインを追加する場合、値をにする必要があり `/allOf/-`ます。 |
| `value.$ref` | 追加 `$id` するミックスインの名前。 |

**応答**

正常に応答すると、更新されたスキーマの詳細が返されます。これには、追加されたmixinの `$ref` 値が配列に含まれ `allOf` ます。

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

## 両方のスキーマの主なIDフィールドの定義

>[!NOTE]
>
>この手順は、での使用を有効にするスキーマに対してのみ必要で [!DNL Real-time Customer Profile](../../profile/home.md)す。 スキーマを和集合に参加させたくない場合、またはスキーマにプライマリIDが既に定義されている場合は、次の手順に進んで、宛先スキーマの参照ID記述子 [を](#create-descriptor) 作成します。

での使用を有効にするには、スキーマにプライマリIDが定義されている [!DNL Real-time Customer Profile]必要があります。 さらに、関係の宛先スキーマは、その主IDを参照フィールドとして使用する必要があります。

このチュートリアルでは、ソーススキーマに既にプライマリIDが定義されていますが、宛先スキーマは定義されていません。 ID記述子を作成することで、スキーマフィールドをプライマリIDフィールドとしてマークすることができます。 これは、エンドポイントにPOSTリクエストを行うことで行われ `/tenant/descriptors` ます。

**API形式**

```http
POST /tenant/descriptors
```

**リクエスト**

次の要求は、宛先スキーマ「Hotels」の `hotelId` フィールドを主IDフィールドとして定義する新しいID記述子を作成します。

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
| `@type` | 作成する記述子の型です。 ID記述子の `@type` 値はです `xdm:descriptorIdentity`。 |
| `xdm:sourceSchema` | `$id` 前の手順で取得した宛先スキーマの [値](#define-schemas)。 |
| `xdm:sourceVersion` | スキーマのバージョン番号。 |
| `sourceProperty` | スキーマのプライマリIDとなる特定のフィールドへのパス。 このパスは、「プロパティ」名前空間も除外し、末尾が「/」ではなく「/」で始まる必要があります。 例えば、上記のリクエストでは、の `/_{TENANT_ID}/hotelId` 代わりにが使用され `/properties/_{TENANT_ID}/properties/hotelId`ます。 |
| `xdm:namespace` | IDフィールドのID名前空間です。 `hotelId` は、この例のECID値なので、「ECID」名前空間が使用されます。 使用可能な [名前空間のリストについては、「](../../identity-service/home.md) ID名前空間の概要」を参照してください。 |
| `xdm:isPrimary` | IDフィールドをスキーマのプライマリIDにするかどうかを指定するブール型プロパティです。 このリクエストはプライマリIDを定義するので、値はtrueに設定されます。 |

**応答**

正常な応答は、新たに作成されたID記述子の詳細を返します。

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

スキーマフィールドは、リレーションシップ内の他のスキーマからの参照として使用される場合、参照ID記述子を適用する必要があります。 「Loyalty Members」の `favoriteHotel` フィールドは「Hotels」の `hotelId` フィールドを参照するので、参照ID記述子を指定する `hotelId` 必要があります。

エンドポイントにPOSTリクエストを行って、宛先スキーマの参照記述子を作成し `/tenant/descriptors` ます。

**API形式**

```http
POST /tenant/descriptors
```

**リクエスト**

次のリクエストは、宛先スキーマ「Hotels」の `hotelId` フィールドの参照記述子を作成します。

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
| `xdm:sourceSchema` | リンク先スキーマの `$id` URL。 |
| `xdm:sourceVersion` | 宛先スキーマのバージョン番号。 |
| `sourceProperty` | 宛先スキーマのプライマリIDフィールドへのパス。 |
| `xdm:identityNamespace` | 参照フィールドのID名前空間。 `hotelId` は、この例のECID値なので、「ECID」名前空間が使用されます。 使用可能な [名前空間のリストについては、「](../../identity-service/home.md) ID名前空間の概要」を参照してください。 |

**応答**

正常な応答は、新たに作成された宛先スキーマの参照記述子の詳細を返します。

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

関係記述子は、ソース・スキーマと宛先スキーマの間に1対1の関係を確立します。 エンドポイントにPOSTリクエストを作成して、新しい関係記述子を作成でき `/tenant/descriptors` ます。

**API形式**

```http
POST /tenant/descriptors
```

**リクエスト**

次の要求は、新しい関係記述子を作成します。この際、「Loyality Members」がソース・スキーマ、「Legacy Loyality Members」が宛先スキーマとして作成されます。

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
| `@type` | 作成する記述子の型です。 関係記述子の `@type` 値はで `xdm:descriptorOneToOne`す。 |
| `xdm:sourceSchema` | ソーススキーマの `$id` URL。 |
| `xdm:sourceVersion` | ソーススキーマのバージョン番号。 |
|  `sourceProperty`。 | ソーススキーマーの参照フィールドへのパス。 |
| `xdm:destinationSchema` | リンク先スキーマの `$id` URL。 |
| `xdm:destinationVersion` | 宛先スキーマのバージョン番号。 |
|  `destinationProperty`。 | リンク先スキーマの参照フィールドへのパス。 |

### 応答

正常な応答は、新たに作成された関係記述子の詳細を返します。

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

このチュートリアルに従うと、2つのスキーマ間に1対1の関係が正しく作成されます。 APIを使用した記述子の操作について詳しくは、『 [!DNL Schema Registry] スキーマレジストリ開発者ガイド [](../api/getting-started.md)』を参照してください。 UIでスキーマの関係を定義する手順については、スキーマエディタを使用したスキーマの関係の [定義に関するチュートリアルを参照してください](relationship-ui.md)。