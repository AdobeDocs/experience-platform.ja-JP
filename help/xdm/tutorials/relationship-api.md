---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: スキーマレジストリAPIを使用して2つのスキーマ間の関係を定義する
topic: tutorials
translation-type: tm+mt
source-git-commit: 849142e44c56f2958e794ca6aefaccd5670c28ba
workflow-type: tm+mt
source-wordcount: '1274'
ht-degree: 1%

---


# APIを使用して2つのスキーマ間の関係を定義する [!DNL Schema Registry]


様々なチャネルにわたる顧客とブランドとの関係を理解する能力は、Adobe Experience Platformの重要な部分です。 これらの関係を [!DNL Experience Data Model] (XDM)スキーマの構造内で定義すると、顧客データに対する複雑な洞察を得ることができます。

スキーマの関係は、和集合スキーマを使用して推論できますが、 [!DNL Real-time Customer Profile]これは同じクラスを共有するスキーマにのみ適用されます。 異なるクラスに属する2つのスキーマ間の関係を確立するには、目的のスキーマのIDを参照するソーススキーマに、専用の **関係フィールド** を追加する必要があります。

このドキュメントでは、を使用して、組織で定義されている2つのスキーマ間の1対1の関係を定義するためのチュートリアルを提供し [!DNL Schema Registry API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml)ます。

## はじめに

このチュートリアルでは、 [!DNL Experience Data Model] (XDM)との詳細を理解している必要があり [!DNL XDM System]ます。 このチュートリアルを開始する前に、次のドキュメントを確認してください。

* [Experience PlatformのXDMシステム](../home.md): XDMとその実装の概要を、で説明し [!DNL Experience Platform]ます。
   * [スキーマ構成の基本](../schema/composition.md): XDMスキーマの構築ブロックの紹介。
* [!DNL Real-time Customer Profile](../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [サンドボックス](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

このチュートリアルを開始する前に、 [開発者ガイドを参照して](../api/getting-started.md) 、 [!DNL Schema Registry] APIの呼び出しを正常に行うために知っておく必要がある重要な情報を確認してください。 例えば、ユーザー `{TENANT_ID}`、「コンテナ」の概念、リクエストを行うために必要なヘッダー(ヘッダーと可能な値に特に注意して [!DNL Accept] ください)が含まれます。

## ソースと宛先のスキーマの定義 {#define-schemas}

この関係で定義される2つのスキーマを既に作成済みであることが想定されます。 このチュートリアルでは、組織の現在の忠誠度プログラム(「[!DNL Loyalty Members][!DNL Hotels]」スキーマで定義)のメンバーと、お気に入りのホテル(「」スキーマで定義)との間に関係を作成します。

スキーマ関係は、 **ソーススキーマ** ( **ソーススキーマ**)が表し、ターゲット内の別のフィールドを参照するフィールドを持ちます。 次の手順では、「[!DNL Loyalty Members]」がソーススキーマ、「[!DNL Hotels]」がターゲットスキーマとして機能します。

>[!IMPORTANT] 関係を確立するには、両方のスキーマがプライマリIDを定義し、有効にする必要があり [!DNL Real-time Customer Profile]ます。 スキーマをそれに応じて設定する方法のガイダンスが必要な場合は、「プロファイルの作成」チュートリアルの「スキーマをスキーマで使用できるようにする」のセクションを参照して [](./create-schema-api.md#profile) ください。

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
>この [!DNL Accept]`application/vnd.adobe.xed-id+json` ヘッダーは、結果のスキーマのタイトル、IDおよびバージョンのみを返します。

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

## ソーススキーマの参照フィールドの定義

リレーショナル・データベース・テーブル内で [!DNL Schema Registry]は、リレーショナル・ディスクリプタは、リレーショナル・データベース・テーブル内の外部キーと同様に機能します。 ソーススキーマ内のフィールドは、宛先スキーマの **主なidentity** フィールドへの参照として機能します。 ソーススキーマにこの目的のフィールドがない場合は、新しいフィールドを使用してミックスインを作成し、スキーマに追加する必要があります。 この新しいフィールドの `type` 値は「[!DNL string]」にする必要があります。

>[!IMPORTANT]
>
>宛先スキーマとは異なり、ソーススキーマは、その主IDを参照フィールドとして使用できません。

このチュートリアルでは、宛先スキーマ「[!DNL Hotels]」に、スキーマの主IDとしての役割を果たす `email` フィールドが含まれているので、その参照フィールドとしても機能します。 ただし、ソーススキーマ「[!DNL Loyalty Members]」には参照として使用する専用のフィールドがないため、スキーマに新しいフィールドを追加する新しいミックスインを与える必要があります。 `favoriteHotel`.

>[!NOTE] ソーススキーマに、参照フィールドとして使用する専用のフィールドが既に存在する場合は、参照記述子の [作成の手順に進むことができます](#reference-identity)。

### 新しいミックスインの作成

スキーマに新しいフィールドを追加するには、まずmixinで定義する必要があります。 エンドポイントにPOSTリクエストを作成して、新しいミックスインを作成でき `/tenant/mixins` ます。

**API形式**

```http
POST /tenant/mixins
```

**リクエスト**

次の要求は、新しいミックスインを作成し、追加先のスキーマの `favoriteHotel``_{TENANT_ID}` 名前空間の下にフィールドを追加します。

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

次のリクエストは、「[!DNL Favorite Hotel]」ミックスインを「[!DNL Loyalty Members]」スキーマに追加します。

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
| `path` | 新しいリソースを追加するスキーマフィールドへのパスです。 スキーマにミックスインを追加する場合、値は「/allOf/ — 」にする必要があります。 |
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

## 参照ID記述子の作成 {#reference-identity}

スキーマフィールドは、リレーションシップ内の他のスキーマからの参照として使用される場合、参照ID記述子を適用する必要があります。 「」内の `favoriteHotel` フィールドは「」内の[!DNL Loyalty Members]フィールドを参照するので、参照ID記述子 `email`[!DNL Hotels]`email` を与える必要があります。

エンドポイントにPOSTリクエストを行って、宛先スキーマの参照記述子を作成し `/tenant/descriptors` ます。

**API形式**

```http
POST /tenant/descriptors
```

**リクエスト**

次のリクエストは、宛先スキーマ「 `email`[!DNL Hotels]」のフィールドの参照記述子を作成します。

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
    "xdm:sourceProperty": "/_{TENANT_ID}/email",
    "xdm:identityNamespace": "Email"
  }'
```

| パラメーター | 説明 |
| --- | --- |
| `@type` | 定義する記述子の型。 参照記述子の場合、値は&quot;xdm:descriptorReferenceIdentity&quot;でなければなりません。 |
| `xdm:sourceSchema` | リンク先スキーマの `$id` URL。 |
| `xdm:sourceVersion` | 宛先スキーマのバージョン番号。 |
| `sourceProperty` | 宛先スキーマのプライマリIDフィールドへのパス。 |
| `xdm:identityNamespace` | 参照フィールドのID名前空間。 この名前空間は、フィールドをスキーマのプライマリIDとして定義する場合と同じにする必要があります。 See the [identity namespace overview](../../identity-service/home.md) for more information. |

**応答**

正常な応答は、新たに作成された宛先スキーマの参照記述子の詳細を返します。

```json
{
    "@type": "xdm:descriptorReferenceIdentity",
    "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/d4ad4b8463a67f6755f2aabbeb9e02c7",
    "xdm:sourceVersion": 1,
    "xdm:sourceProperty": "/_{TENANT_ID}/email",
    "xdm:identityNamespace": "Email",
    "meta:containerId": "tenant",
    "@id": "53180e9f86eed731f6bf8bf42af4f59d81949ba6"
}
```

## 関係記述子の作成 {#create-descriptor}

関係記述子は、ソース・スキーマと宛先スキーマの間に1対1の関係を確立します。 宛先スキーマの参照記述子を定義すると、エンドポイントにPOSTリクエストを作成して、新しい関係記述子を作成でき `/tenant/descriptors` ます。

**API形式**

```http
POST /tenant/descriptors
```

**リクエスト**

次のリクエストは、新しい関係記述子を作成します。この関係記述子は、ソーススキーマ[!DNL Loyalty Members]として「」を、宛先スキーマとして「[!DNL Legacy Loyalty Members]」を持ちます。

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
    "xdm:destinationProperty": "/_{TENANT_ID}/email"
  }'
```

| パラメーター | 説明 |
| --- | --- |
| `@type` | 作成する記述子の型です。 関係記述子の `@type` 値は&quot;xdm:descriptorOneToOne&quot;です。 |
| `xdm:sourceSchema` | ソーススキーマの `$id` URL。 |
| `xdm:sourceVersion` | ソーススキーマのバージョン番号。 |
| `xdm:sourceProperty` | ソーススキーマの参照フィールドへのパス。 |
| `xdm:destinationSchema` | リンク先スキーマの `$id` URL。 |
| `xdm:destinationVersion` | 宛先スキーマのバージョン番号。 |
| `xdm:destinationProperty` | コピー先スキーマの参照フィールドへのパス。 |

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

このチュートリアルに従うと、2つのスキーマ間に1対1の関係が正しく作成されます。 APIを使用した記述子の操作について詳しくは、『 [!DNL Schema Registry] スキーマレジストリ開発者ガイド [](../api/descriptors.md)』を参照してください。 UIでスキーマの関係を定義する手順については、スキーマエディタを使用したスキーマの関係の [定義に関するチュートリアルを参照してください](relationship-ui.md)。
