---
keywords: Experience Platform；ホーム；人気のトピック；API;XDM;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；スキーマレジストリ；スキーマ；スキーマ；スキーマ；作成
solution: Experience Platform
title: スキーマレジストリ API を使用したスキーマの作成
type: Tutorial
description: このチュートリアルでは、スキーマレジストリ API を使用して、標準クラスを使用してスキーマを作成する手順を説明します。
exl-id: fa487a5f-d914-48f6-8d1b-001a60303f3d
source-git-commit: 030874e91b88b18f7de0cc2d12200243b7ed1d31
workflow-type: tm+mt
source-wordcount: '2556'
ht-degree: 38%

---

# を使用してスキーマを作成する [!DNL Schema Registry] API

この [!DNL Schema Registry] は、 [!DNL Schema Library] Adobe Experience Platformの この [!DNL Schema Library] には、Adobeが作成したリソースが含まれます。 [!DNL Experience Platform] パートナー、およびお客様が使用するアプリケーションのベンダー レジストリは、使用可能なすべてのライブラリリソースにアクセスできるユーザーインターフェイスと RESTful API を提供します。

このチュートリアルでは、 [!DNL Schema Registry] 標準クラスを使用してスキーマを作成する手順を説明する API です。 のユーザーインターフェイスを使用する場合は、 [!DNL Experience Platform]、 [スキーマエディターのチュートリアル](create-schema-ui.md) には、スキーマエディターで同様の操作を実行する手順が記載されています。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [[!DNL Experience Data Model (XDM) System]](../home.md)：[!DNL Experience Platform] がカスタマーエクスペリエンスのデータの整理に使用する、標準化されたフレームワーク。
   * [スキーマ構成の基本](../schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について説明します。
* [[!DNL Real-Time Customer Profile]](../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
* [[!DNL Sandboxes]](../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

このチュートリアルを開始する前に、 [開発者ガイド](../api/getting-started.md) を正しく呼び出すために知っておく必要がある重要な情報については、を参照してください。 [!DNL Schema Registry] API これには、`{TENANT_ID}`、「コンテナ」の概念、リクエストを行うのに必要なヘッダー（ ヘッダーと使用可能な値には特に注意を払う）が含まれます。`Accept`

このチュートリアルでは、小売ロイヤルティプログラムのメンバーに関連するデータを記述する「ロイヤルティメンバー」スキーマを作成する手順について説明します。開始する前に、付録にある[完全なロイヤルティメンバースキーマ](#complete-schema)をプレビューできます。

## 標準クラスでのスキーマを構成

スキーマは、に取り込むデータのブループリントと見なすことができます。 [!DNL Experience Platform]. 各スキーマは、クラスと、0 個以上のスキーマフィールドグループで構成されます。 つまり、スキーマを定義するためにフィールドグループを追加する必要はありませんが、ほとんどの場合、少なくとも 1 つのフィールドグループが使用されます。

### クラスの割り当て

スキーマ構成プロセスは、クラスの選択から始まります。このクラスは、データの主要な動作面（レコードと時系列）、および取得されるデータを説明するために必要な最小フィールドを定義します。

このチュートリアルで作成するスキーマでは、 [!DNL XDM Individual Profile] クラス。 [!DNL XDM Individual Profile] は、レコードの動作を定義するためにAdobeが提供する標準クラスです。 動作に関する詳細については、「[スキーマ合成の基本](../schema/composition.md)」を参照してください。

クラスを割り当てるために、API 呼び出しが行われ、テナントコンテナ内に新しいスキーマが作成（POST）されます。この呼び出しには、スキーマが実装するクラスが含まれます。各スキーマは、1 つのクラスのみを実装できます。

**API 形式**

```http
POST /tenant/schemas
```

**リクエスト**

リクエストには、クラスの `$id` を参照する `allOf` 属性を含める必要があります。この属性は、スキーマが実装する「基本クラス」を定義します。この例では、基本クラスは [!DNL XDM Individual Profile] クラス。  クラスの `$id`[!DNL XDM Individual Profile] は、以下の `$ref` 配列内の `allOf` フィールドの値として使用されます。

```SHELL
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
        "type": "object",
        "title": "Loyalty Members",
        "description": "Information for all members of the loyalty program",
        "allOf": [
            {
              "$ref": "https://ns.adobe.com/xdm/context/profile"
            }
        ]
      }'
```

**応答** 

リクエストが成功すると、HTTP 応答ステータス 201（作成済み）が返され、応答本文に、`$id`、`meta:altIt`、および`version` を含む新しく作成されたスキーマの詳細が含まれています。これらの値は読み取り専用で、 [!DNL Schema Registry].

```JSON
{
  "$id": "https://ns.adobe.com/tenantId/schemas/ee56b80adc7e03b8214e135a28538fe83c7f85bf87f565b3",
  "meta:altId": "_tenantId.schemas.ee56b80adc7e03b8214e135a28538fe83c7f85bf87f565b3",
  "meta:resourceType": "schemas",
  "version": "1.0",
  "title": "Loyalty Members",
  "type": "object",
  "description": "Information for all members of the loyalty program",
  "allOf": [
    {
      "$ref": "https://ns.adobe.com/xdm/context/profile",
      "type": "object",
      "meta:xdmType": "object"
    }
  ],
  "refs": [
    "https://ns.adobe.com/xdm/context/profile"
  ],
  "imsOrg": "{ORG_ID}",
  "meta:extensible": false,
  "meta:abstract": false,
  "meta:extends": [
    "https://ns.adobe.com/xdm/common/auditable",
    "https://ns.adobe.com/xdm/data/record",
    "https://ns.adobe.com/xdm/context/profile"
  ],
  "meta:xdmType": "object",
  "meta:registryMetadata": {
    "repo:createdDate": 1673310304048,
    "repo:lastModifiedDate": 1673310304048,
    "xdm:createdClientId": "{CLIENT_ID}",
    "xdm:lastModifiedClientId": "{CLIENT_ID}",
    "xdm:createdUserId": "{USER_ID}",
    "xdm:lastModifiedUserId": "{USER_ID}",
    "eTag": "6d2ed8acd9c3b768a44de29e069fc6f71329d2550f708381d22fa8bf8c192366",
    "meta:globalLibVersion": "1.38.2"
  },
  "meta:class": "https://ns.adobe.com/xdm/context/profile",
  "meta:containerId": "tenant",
  "meta:sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
  "meta:sandboxType": "production",
  "meta:tenantNamespace": "_tenantId"
}
```

### スキーマの検索

新しく作成したスキーマを表示するには、`meta:altId` またはスキーマの URL エンコードされた `$id` URI を使用して、参照（GET）リクエストを行します。

**API 形式**

```http
GET /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | この `meta:altId` または URL エンコード済み `$id` 検索するスキーマの数を指定します。 |

**リクエスト**

```SHELL
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/https%3A%2F%2Fns.adobe.com%2F{TENANT_ID}%2Fschemas%2F533ca5da28087c44344810891b0f03d9\
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Accept: application/vnd.adobe.xed+json; version=1'
```

**応答**

応答の形式は、 `Accept` ヘッダーがリクエストと共に送信されます。 異なる `Accept` ヘッダーを使用して、ニーズに最も適したヘッダーを確認できます。

```JSON
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/ee56b80adc7e03b8214e135a28538fe83c7f85bf87f565b3",
  "meta:altId": "_{TENANT_ID}.schemas.ee56b80adc7e03b8214e135a28538fe83c7f85bf87f565b3",
  "meta:resourceType": "schemas",
  "version": "1.0",
  "title": "Loyalty Members",
  "type": "object",
  "description": "Information for all members of the loyalty program",
  "allOf": [
    {
      "$ref": "https://ns.adobe.com/xdm/context/profile",
      "type": "object",
      "meta:xdmType": "object"
    }
  ],
  "imsOrg": "{ORG_ID}",
  "meta:extensible": false,
  "meta:abstract": false,
  "meta:extends": [
    "https://ns.adobe.com/xdm/common/auditable",
    "https://ns.adobe.com/xdm/data/record",
    "https://ns.adobe.com/xdm/context/profile"
  ],
  "meta:xdmType": "object",
  "meta:registryMetadata": {
    "repo:createdDate": 1673310304048,
    "repo:lastModifiedDate": 1673310304048,
    "xdm:createdClientId": "{CLIENT_ID}",
    "xdm:lastModifiedClientId": "{CLIENT_ID}",
    "xdm:createdUserId": "{USER_ID}",
    "xdm:lastModifiedUserId": "{USER_ID}",
    "eTag": "6d2ed8acd9c3b768a44de29e069fc6f71329d2550f708381d22fa8bf8c192366",
    "meta:globalLibVersion": "1.38.2"
  },
  "meta:class": "https://ns.adobe.com/xdm/context/profile",
  "meta:containerId": "tenant",
  "meta:sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
  "meta:sandboxType": "production",
  "meta:tenantNamespace": "_{TENANT_ID}",
  "meta:allFieldAccess": true
}
```

### フィールドグループを追加 {#add-a-field-group}

「ロイヤルティメンバー」スキーマが作成され、確認されたので、フィールドグループを追加できます。

選択したスキーマのクラスに応じて、使用できる標準フィールドグループが異なります。 各フィールドグループには、 `intendedToExtend` そのフィールドグループと互換性があるクラスを定義するフィールド。

フィールドグループは、「名前」や「住所」など、同じ情報を取り込む必要のあるスキーマで再利用できる概念を定義します。

**API 形式**

```http
PATCH /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | この `meta:altId` または URL エンコード済み `$id` フィールドグループを追加するスキーマの。 |

**リクエスト**

このリクエストは、「ロイヤルティメンバー」スキーマを更新し、 [[!UICONTROL 人口統計の詳細] フィールドグループ](../field-groups/profile/demographic-details.md) (`profile-person-details`) をクリックします。

この `profile-person-details` 「ロイヤルティメンバー」スキーマでは、ロイヤルティプログラムメンバーの人口統計情報（名、姓、誕生日など）が取り込まれるようになりました。

```SHELL
curl -X PATCH \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.533ca5da28087c44344810891b0f03d9 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '[
        { "op": "add", "path": "/allOf/-", "value":  {"$ref": "https://ns.adobe.com/xdm/context/profile-person-details"}}
      ]'
```

**応答**

応答には、 `meta:extends` 配列であり、その中に `$ref` を `allOf` 属性。

```JSON
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/ee56b80adc7e03b8214e135a28538fe83c7f85bf87f565b3",
  "meta:altId": "_{TENANT_ID}.schemas.ee56b80adc7e03b8214e135a28538fe83c7f85bf87f565b3",
  "meta:resourceType": "schemas",
  "version": "1.1",
  "title": "Loyalty Members",
  "type": "object",
  "description": "Information for all members of the loyalty program",
  "allOf": [
    {
      "$ref": "https://ns.adobe.com/xdm/context/profile",
      "type": "object",
      "meta:xdmType": "object"
    },
    {
      "$ref": "https://ns.adobe.com/xdm/context/profile-person-details",
      "type": "object",
      "meta:xdmType": "object"
    }
  ],
  "refs": [
    "https://ns.adobe.com/xdm/context/profile-person-details",
    "https://ns.adobe.com/xdm/context/profile"
  ],
  "imsOrg": "{ORG_ID}",
  "meta:extensible": false,
  "meta:abstract": false,
  "meta:extends": [
    "https://ns.adobe.com/xdm/context/profile-person-details",
    "https://ns.adobe.com/xdm/common/auditable",
    "https://ns.adobe.com/xdm/data/record",
    "https://ns.adobe.com/xdm/context/profile"
  ],
  "meta:xdmType": "object",
  "meta:registryMetadata": {
    "repo:createdDate": 1673310304048,
    "repo:lastModifiedDate": 1673310912096,
    "xdm:createdClientId": "{CLIENT_ID}",
    "xdm:lastModifiedClientId": "{CLIENT_ID}",
    "xdm:createdUserId": "{USER_ID}",
    "xdm:lastModifiedUserId": "{USER_ID}",
    "eTag": "b480f28a35f356b237fc129e796074a3f33a7a67df273f6a8beaee1ec6465540",
    "meta:globalLibVersion": "1.38.2"
  },
  "meta:class": "https://ns.adobe.com/xdm/context/profile",
  "meta:containerId": "tenant",
  "meta:sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
  "meta:sandboxType": "production",
  "meta:tenantNamespace": "_{TENANT_ID}",
  "meta:descriptorStatus": {
    "result": []
  }
}
```

### さらにフィールドグループを追加

「ロイヤルティメンバー」スキーマには、さらに 2 つの標準フィールドグループが必要です。追加するには、別のフィールドグループを使用して手順を繰り返します。

>[!TIP]
>
>使用可能なすべてのフィールドグループを確認して、各グループに含まれるフィールドに慣れておくことをお勧めします。 「グローバル」コンテナと「テナント」コンテナのそれぞれに対してリクエストを実行し、「meta:intendedToExtend」フィールドが使用しているクラスと一致するフィールドグループのみを返すことで、特定のクラスで使用できるすべてのフィールドグループをリスト (GET) できます。 この場合、 [!DNL XDM Individual Profile] クラス、 [!DNL XDM Individual Profile] `$id` は次の場合に使用されます。
>
>
```http
>GET /global/fieldgroups?property=meta:intendedToExtend==https://ns.adobe.com/xdm/context/profile
>GET /tenant/fieldgroups?property=meta:intendedToExtend==https://ns.adobe.com/xdm/context/profile
>```

**API 形式**

```http
PATCH /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | この `meta:altId` または URL エンコード済み `$id` 更新するスキーマの |

**リクエスト**

このリクエストは、「ロイヤルティメンバー」スキーマを更新して、次の標準フィールドグループ内のフィールドを含めます。

* [[!UICONTROL 個人の連絡先の詳細]](../field-groups/profile/personal-contact-details.md) (`profile-personal-details`):住所、電子メールアドレス、自宅電話などの連絡先情報を追加します。
* [[!UICONTROL ロイヤルティの詳細]](../field-groups/profile/loyalty-details.md) (`profile-loyalty-details`):住所、電子メールアドレス、自宅電話などの連絡先情報を追加します。

```SHELL
curl -X PATCH \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.533ca5da28087c44344810891b0f03d9 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '[
        { "op": "add", "path": "/allOf/-", "value":  {"$ref": "https://ns.adobe.com/xdm/context/profile-personal-details"}},
        { "op": "add", "path": "/allOf/-", "value":  {"$ref": "https://ns.adobe.com/xdm/mixins/profile/profile-loyalty-details"}}
      ]'  
```

**応答**

応答には、 `meta:extends` 配列であり、その中に `$ref` を `allOf` 属性。

「ロイヤルティメンバー」スキーマには、4 つの `$ref` 値 `allOf` 配列： `profile`, `profile-person-details`, `profile-personal-details`、および `profile-loyalty-details` 以下に示すように。

```JSON
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/ee56b80adc7e03b8214e135a28538fe83c7f85bf87f565b3",
  "meta:altId": "_{TENANT_ID}.schemas.ee56b80adc7e03b8214e135a28538fe83c7f85bf87f565b3",
  "meta:resourceType": "schemas",
  "version": "1.2",
  "title": "Loyalty Members",
  "type": "object",
  "description": "Information for all members of the loyalty program",
  "allOf": [
    {
      "$ref": "https://ns.adobe.com/xdm/context/profile",
      "type": "object",
      "meta:xdmType": "object"
    },
    {
      "$ref": "https://ns.adobe.com/xdm/context/profile-person-details",
      "type": "object",
      "meta:xdmType": "object"
    },
    {
      "$ref": "https://ns.adobe.com/xdm/context/profile-personal-details",
      "type": "object",
      "meta:xdmType": "object"
    },
    {
      "$ref": "https://ns.adobe.com/xdm/mixins/profile/profile-loyalty-details",
      "type": "object",
      "meta:xdmType": "object"
    }
  ],
  "refs": [
    "https://ns.adobe.com/xdm/context/profile-person-details",
    "https://ns.adobe.com/xdm/mixins/profile/profile-loyalty-details",
    "https://ns.adobe.com/xdm/context/profile-personal-details",
    "https://ns.adobe.com/xdm/context/profile"
  ],
  "imsOrg": "{ORG_ID}",
  "meta:extensible": false,
  "meta:abstract": false,
  "meta:extends": [
    "https://ns.adobe.com/xdm/context/profile-person-details",
    "https://ns.adobe.com/xdm/mixins/profile/profile-loyalty-details",
    "https://ns.adobe.com/xdm/context/profile-personal-details",
    "https://ns.adobe.com/xdm/common/auditable",
    "https://ns.adobe.com/xdm/data/record",
    "https://ns.adobe.com/xdm/context/profile"
  ],
  "meta:xdmType": "object",
  "meta:registryMetadata": {
    "repo:createdDate": 1673310304048,
    "repo:lastModifiedDate": 1673311559934,
    "xdm:createdClientId": "{CLIENT_ID}",
    "xdm:lastModifiedClientId": "{CLIENT_ID}",
    "xdm:createdUserId": "{USER_ID}",
    "xdm:lastModifiedUserId": "{USER_ID}",
    "eTag": "1de5ed1a07e3478719952f0a8c94d5e5390d5a9a998761adb4cf1989137fd6ea",
    "meta:globalLibVersion": "1.38.2"
  },
  "meta:class": "https://ns.adobe.com/xdm/context/profile",
  "meta:containerId": "tenant",
  "meta:sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
  "meta:sandboxType": "production",
  "meta:tenantNamespace": "_{TENANT_ID}",
  "meta:descriptorStatus": {
    "result": []
  }
}
```

### 新しいフィールドグループを定義

標準 [!UICONTROL ロイヤルティの詳細] フィールドグループは、スキーマに役立つロイヤルティ関連のフィールドを提供します。標準フィールドグループには、追加のロイヤルティフィールドは含まれません。

これらのフィールドを追加するには、 `tenant` コンテナ。 これらのフィールドグループは組織に固有のもので、組織外のユーザーは表示したり編集したりすることはできません。

新しいフィールドグループを作成 (POST) するには、リクエストに `meta:intendedToExtend` 次を含むフィールド： `$id` フィールドグループと互換性がある基本クラスの場合、およびフィールドグループに含めるプロパティ。

カスタムプロパティは、 `TENANT_ID` 他のフィールドグループやフィールドとの衝突を回避するため。

**API 形式**

```http
POST /tenant/fieldgroups
```

**リクエスト**

このリクエストでは、 `loyaltyTier` 会社の特定のロイヤルティプログラムに固有の 4 つのフィールドを含むオブジェクト。 `id`, `effectiveDate`, `currentThreshold`、および `nextThreshold`.

```SHELL
curl -X POST\
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/fieldgroups\
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'content-type: application/json' \
  -d '{
        "type": "object",
        "title": "Loyalty Tier",
        "meta:intendedToExtend": [
          "https://ns.adobe.com/xdm/context/profile"
        ],
        "description": "Captures info about the current loyalty tier of a customer.",
        "definitions": {
          "loyaltyTier": {
            "properties": {
              "_{TENANT_ID}": {
                "type": "object",
                "properties": {
                  "loyaltyTier": {
                    "type": "object",
                    "properties": {
                      "id": {
                        "title": "Loyalty Tier Identifier",
                        "type": "string",
                        "description": "Loyalty Tier Identifier."
                      },
                      "effectiveDate": {
                        "title": "Effective Date",
                        "type": "string",
                        "format": "date-time",
                        "description": "Date the member joined their current loyalty tier."
                      },
                      "currentThreshold": {
                        "title": "Current Point Threshold",
                        "type": "integer",
                        "description": "The minimum number of loyalty points the member must maintain to remain in the current tier."
                      },
                      "nextThreshold": {
                        "title": "Next Point Threshold",
                        "type": "integer",
                        "description": "The number of loyalty points the member must accrue to graduate to the next tier."
                      }
                    }
                  }
                }
              }
            }
          }
        },
        "allOf": [
          {
            "$ref": "#/definitions/loyaltyTier"
          }
        ]
      }'
```

**応答**

リクエストが成功すると、HTTP 応答ステータス 201（作成済み）が返され、応答本文に、新しく作成されたフィールドグループの詳細 ( `$id`, `meta:altIt`、および `version`. これらの値は読み取り専用で、 [!DNL Schema Registry].

```JSON
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/mixins/9068fd4ea2abf813f4fd2fc9c8b413ae453ff0efc7636691",
  "meta:altId": "_{TENANT_ID}.mixins.9068fd4ea2abf813f4fd2fc9c8b413ae453ff0efc7636691",
  "meta:resourceType": "mixins",
  "version": "1.0",
  "title": "Loyalty Tier",
  "type": "object",
  "description": "Captures info about the current loyalty tier of a customer.",
  "definitions": {
    "loyaltyTier": {
      "properties": {
        "_{TENANT_ID}": {
          "type": "object",
          "properties": {
            "loyaltyTier": {
              "type": "object",
              "properties": {
                "id": {
                  "title": "Loyalty Tier Identifier",
                  "type": "string",
                  "description": "Loyalty Tier Identifier.",
                  "meta:xdmType": "string"
                },
                "effectiveDate": {
                  "title": "Effective Date",
                  "type": "string",
                  "format": "date-time",
                  "description": "Date the member joined their current loyalty tier.",
                  "meta:xdmType": "date-time"
                },
                "currentThreshold": {
                  "title": "Current Point Threshold",
                  "type": "integer",
                  "description": "The minimum number of loyalty points the member must maintain to remain in the current tier.",
                  "meta:xdmType": "int"
                },
                "nextThreshold": {
                  "title": "Next Point Threshold",
                  "type": "integer",
                  "description": "The number of loyalty points the member must accrue to graduate to the next tier.",
                  "meta:xdmType": "int"
                }
              },
              "meta:xdmType": "object"
            }
          },
          "meta:xdmType": "object"
        }
      }
    }
  },
  "allOf": [
    {
      "$ref": "#/definitions/loyaltyTier",
      "type": "object",
      "meta:xdmType": "object"
    }
  ],
  "refs": [],
  "imsOrg": "{ORG_ID}",
  "meta:extensible": true,
  "meta:abstract": true,
  "meta:intendedToExtend": [
    "https://ns.adobe.com/xdm/context/profile"
  ],
  "meta:xdmType": "object",
  "meta:registryMetadata": {
    "repo:createdDate": 1673313004645,
    "repo:lastModifiedDate": 1673313004645,
    "xdm:createdClientId": "{CLIENT_ID}",
    "xdm:lastModifiedClientId": "{CLIENT_ID}",
    "xdm:createdUserId": "{USER_ID}",
    "xdm:lastModifiedUserId": "{USER_ID}",
    "eTag": "98e5d48808f5a4d9655493777389568a2581cfce013351ab9e1595d82f698dd6",
    "meta:globalLibVersion": "1.38.2"
  },
  "meta:containerId": "tenant",
  "meta:sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
  "meta:sandboxType": "production",
  "meta:tenantNamespace": "_{TENANT_ID}"
}
```

### スキーマにカスタムフィールドグループを追加

同じ手順で [標準フィールドグループの追加](#add-a-field-group) この新しく作成したフィールドグループをスキーマに追加します。

**API 形式**

```http
PATCH /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | この `meta:altId` または URL エンコード済み `$id` スキーマの。 |

**リクエスト**

このリクエストは、「ロイヤルティメンバー」スキーマを更新 (PATCH) し、新しい「ロイヤルティ層」フィールドグループ内のフィールドを含めます。

```SHELL
curl -X PATCH \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.533ca5da28087c44344810891b0f03d9 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '[
        { "op": "add", "path": "/allOf/-", "value":  {"$ref": "https://ns.adobe.com/{TENANT_ID}/mixins/9068fd4ea2abf813f4fd2fc9c8b413ae453ff0efc7636691"}}
      ]'
```

**応答**

応答には、新しく追加されたフィールドグループが `meta:extends` 配列を含み、 `$ref` を `allOf` 属性。

```JSON
{
    "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/ee56b80adc7e03b8214e135a28538fe83c7f85bf87f565b3",
    "meta:altId": "_{TENANT_ID}.schemas.ee56b80adc7e03b8214e135a28538fe83c7f85bf87f565b3",
    "meta:resourceType": "schemas",
    "version": "1.3",
    "title": "Loyalty Members",
    "type": "object",
    "description": "Information for all members of the loyalty program",
    "allOf": [
        {
            "$ref": "https://ns.adobe.com/xdm/context/profile",
            "type": "object",
            "meta:xdmType": "object"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/profile-person-details",
            "type": "object",
            "meta:xdmType": "object"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/profile-personal-details",
            "type": "object",
            "meta:xdmType": "object"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/mixins/profile/profile-loyalty-details",
            "type": "object",
            "meta:xdmType": "object"
        },
        {
            "$ref": "https://ns.adobe.com/{TENANT_ID}/mixins/9068fd4ea2abf813f4fd2fc9c8b413ae453ff0efc7636691",
            "type": "object",
            "meta:xdmType": "object"
        }
    ],
    "refs": [
        "https://ns.adobe.com/xdm/context/profile-person-details",
        "https://ns.adobe.com/xdm/mixins/profile/profile-loyalty-details",
        "https://ns.adobe.com/xdm/context/profile-personal-details",
        "https://ns.adobe.com/xdm/context/profile",
        "https://ns.adobe.com/{TENANT_ID}/mixins/9068fd4ea2abf813f4fd2fc9c8b413ae453ff0efc7636691"
    ],
    "imsOrg": "{ORG_ID}",
    "meta:extensible": false,
    "meta:abstract": false,
    "meta:extends": [
        "https://ns.adobe.com/xdm/context/profile-person-details",
        "https://ns.adobe.com/xdm/mixins/profile/profile-loyalty-details",
        "https://ns.adobe.com/xdm/context/profile-personal-details",
        "https://ns.adobe.com/xdm/common/auditable",
        "https://ns.adobe.com/xdm/data/record",
        "https://ns.adobe.com/xdm/context/profile",
        "https://ns.adobe.com/{TENANT_ID}/mixins/9068fd4ea2abf813f4fd2fc9c8b413ae453ff0efc7636691"
    ],
    "meta:xdmType": "object",
    "meta:registryMetadata": {
        "repo:createdDate": 1673310304048,
        "repo:lastModifiedDate": 1673313118938,
        "xdm:createdClientId": "{CLIENT_ID}",
        "xdm:lastModifiedClientId": "{CLIENT_ID}",
        "xdm:createdUserId": "{USER_ID}",
        "xdm:lastModifiedUserId": "{USER_ID}",
        "eTag": "6559b197a04bb3fda5bc80bf383a260cfbe32539d528f0139c5750711eebfba2",
        "meta:globalLibVersion": "1.38.2"
    },
    "meta:class": "https://ns.adobe.com/xdm/context/profile",
    "meta:containerId": "tenant",
    "meta:sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
    "meta:sandboxType": "production",
    "meta:tenantNamespace": "_{TENANT_ID}",
    "meta:descriptorStatus": {
        "result": []
    }
}
```

### 現在のスキーマの表示

GETリクエストを実行して、現在のスキーマを表示し、追加したフィールドグループがスキーマの全体的な構造にどのように貢献したかを確認できるようになりました。

**API 形式**

```http
GET /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | この `meta:altId` または URL エンコード済み `$id` スキーマの。 |

**リクエスト**

```SHELL
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.ee56b80adc7e03b8214e135a28538fe83c7f85bf87f565b3 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Accept: application/vnd.adobe.xed-full+json; version=1'
```

**応答**

`application/vnd.adobe.xed-full+json; version=1` ヘッダーを使用すると、すべてのプロパティを含む完全なスキーマを表示できます。`Accept`これらのプロパティは、スキーマの作成に使用されたクラスおよびフィールドグループが提供するフィールドです。 以下の応答例では、スペースを節約するために、最近追加したフィールドのみが表示されます。 ドキュメント末尾の[付録](#appendix)では、すべてのプロパティとその属性を含む完全なスキーマを参照できます。

の下 `"properties"`に設定されている場合、 `_{TENANT_ID}` カスタムフィールドグループを追加したときに作成された名前空間。 その名前空間内に、 `loyaltyTier` オブジェクトおよびフィールドグループの作成時に定義されたフィールド。

```JSON
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/mixins/9068fd4ea2abf813f4fd2fc9c8b413ae453ff0efc7636691",
  "meta:altId": "_{TENANT_ID}.mixins.9068fd4ea2abf813f4fd2fc9c8b413ae453ff0efc7636691",
  "meta:resourceType": "mixins",
  "version": "1.0",
  "title": "Loyalty Tier",
  "type": "object",
  "description": "Captures info about the current loyalty tier of a customer.",
  "definitions": {
    "loyaltyTier": {
      "properties": {
        "_{TENANT_ID}": {
          "type": "object",
          "properties": {
            "loyaltyTier": {
              "type": "object",
              "properties": {
                "id": {
                  "title": "Loyalty Tier Identifier",
                  "type": "string",
                  "description": "Loyalty Tier Identifier.",
                  "meta:xdmType": "string"
                },
                "effectiveDate": {
                  "title": "Effective Date",
                  "type": "string",
                  "format": "date-time",
                  "description": "Date the member joined their current loyalty tier.",
                  "meta:xdmType": "date-time"
                },
                "currentThreshold": {
                  "title": "Current Point Threshold",
                  "type": "integer",
                  "description": "The minimum number of loyalty points the member must maintain to remain in the current tier.",
                  "meta:xdmType": "int"
                },
                "nextThreshold": {
                  "title": "Next Point Threshold",
                  "type": "integer",
                  "description": "The number of loyalty points the member must accrue to graduate to the next tier.",
                  "meta:xdmType": "int"
                }
              },
              "meta:xdmType": "object"
            }
          },
          "meta:xdmType": "object"
        }
      }
    }
  },
  "allOf": [
    {
      "$ref": "#/definitions/loyaltyTier",
      "type": "object",
      "meta:xdmType": "object"
    }
  ],
  "refs": [],
  "imsOrg": "{ORG_ID}",
  "meta:extensible": true,
  "meta:abstract": true,
  "meta:intendedToExtend": [
    "https://ns.adobe.com/xdm/context/profile"
  ],
  "meta:xdmType": "object",
  "meta:registryMetadata": {
    "repo:createdDate": 1673313004645,
    "repo:lastModifiedDate": 1673313004645,
    "xdm:createdClientId": "{CLIENT_ID}",
    "xdm:lastModifiedClientId": "{CLIENT_ID}",
    "xdm:createdUserId": "{USER_ID}",
    "xdm:lastModifiedUserId": "{USER_ID}",
    "eTag": "98e5d48808f5a4d9655493777389568a2581cfce013351ab9e1595d82f698dd6",
    "meta:globalLibVersion": "1.38.2"
  },
  "meta:containerId": "tenant",
  "meta:sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
  "meta:sandboxType": "production",
  "meta:tenantNamespace": "_{TENANT_ID}"
}
```

### データタイプの作成

作成した「ロイヤルティ層」フィールドグループには、他のスキーマで役立つ特定のプロパティが含まれています。 例えば、データはエクスペリエンスイベントの一部として取得されたり、別のクラスを実装するスキーマによって使用されたりします。この場合、オブジェクト階層をデータ型として保存し、別の場所で簡単に定義を再利用できるようにすると効果的です。

データ型を使用すると、1 つのオブジェクト階層を 1 回定義し、他のスカラ型と同様にフィールド内で参照できます。

つまり、データ型を使用すると、フィールドの「型」として追加してスキーマの任意の場所に含めることができるので、フィールドグループよりも柔軟にマルチフィールド構造を一貫して使用できます。

**API 形式**

```http
POST /tenant/datatypes
```

**リクエスト**

データ型を定義する必要はありません `meta:extends` または `meta:intendedToExtend` 競合を避けるために、フィールドやフィールドをテナント ID の下にネストする必要はありません。

```SHELL
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/datatypes \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "title": "Loyalty Tier",
        "type": "object",
        "description": "Loyalty Tier data type",
        "definitions": {
          "loyaltyTier": {
            "type": "object",
            "properties": {
              "id": {
                "title": "Loyalty Tier Identifier",
                "type": "string",
                "description": "Loyalty Tier Identifier."
              },
              "effectiveDate": {
                "title": "Effective Date",
                "type": "string",
                "format": "date-time",
                "description": "Date the member joined their current loyalty tier."
              },
              "currentThreshold": {
                "title": "Current Point Threshold",
                "type": "integer",
                "description": "The minimum number of loyalty points the member must maintain to remain in the       current tier."
              },
              "nextThreshold": {
                "title": "Next Point Threshold",
                "type": "integer",
                "description": "The number of loyalty points the member must accrue to graduate to the next tier."
              }
            }
          }
        },
        "allOf": [
          {
            "$ref": "#/definitions/loyaltyTier"
          }
        ]
      }'
```

**応答** 

リクエストが成功すると、HTTP 応答ステータス 201（作成済み）が返され、応答本文に `$id`、`meta:altIt`、および `version` を含む新しく作成されたデータ型の詳細が含まれています。す。これらの値は読み取り専用で、 [!DNL Schema Registry].

```JSON
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/datatypes/c069d13ebfaaa5980d988e7694d794b37b1784fe11d754cb",
  "meta:altId": "_{TENANT_ID}.datatypes.c069d13ebfaaa5980d988e7694d794b37b1784fe11d754cb",
  "meta:resourceType": "datatypes",
  "version": "1.0",
  "title": "Loyalty Tier",
  "type": "object",
  "description": "Loyalty Tier data type",
  "definitions": {
    "loyaltyTier": {
      "type": "object",
      "properties": {
        "id": {
          "title": "Loyalty Tier Identifier",
          "type": "string",
          "description": "Loyalty Tier Identifier.",
          "meta:xdmType": "string"
        },
        "effectiveDate": {
          "title": "Effective Date",
          "type": "string",
          "format": "date-time",
          "description": "Date the member joined their current loyalty tier.",
          "meta:xdmType": "date-time"
        },
        "currentThreshold": {
          "title": "Current Point Threshold",
          "type": "integer",
          "description": "The minimum number of loyalty points the member must maintain to remain in the current tier.",
          "meta:xdmType": "int"
        },
        "nextThreshold": {
          "title": "Next Point Threshold",
          "type": "integer",
          "description": "The number of loyalty points the member must accrue to graduate to the next tier.",
          "meta:xdmType": "int"
        }
      },
      "meta:xdmType": "object"
    }
  },
  "allOf": [
    {
      "$ref": "#/definitions/loyaltyTier",
      "type": "object",
      "meta:xdmType": "object"
    }
  ],
  "refs": [],
  "imsOrg": "{ORG_ID}",
  "meta:extensible": true,
  "meta:abstract": true,
  "meta:xdmType": "object",
  "meta:registryMetadata": {
    "repo:createdDate": 1673378256699,
    "repo:lastModifiedDate": 1673378256699,
    "xdm:createdClientId": "{CLIENT_ID}",
    "xdm:lastModifiedClientId": "{CLIENT_ID}",
    "xdm:createdUserId": "{USER_ID}",
    "xdm:lastModifiedUserId": "{USER_ID}",
    "eTag": "8afba84c0c9a68126a7a1389f8523a1112bdf4405badc6dcddbbb4a0e18f5cdb",
    "meta:globalLibVersion": "1.38.2"
  },
  "meta:containerId": "tenant",
  "meta:sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
  "meta:sandboxType": "production",
  "meta:tenantNamespace": "_{TENANT_ID}"
}
```

URL エンコードされた `$id` URI を使用して参照（GET）リクエストを実行し、新しいデータ型を直接表示することができます。参照リクエストでは ヘッダーに `version` を必ず含めてください。`Accept`

### スキーマでのデータ型の使用

これで、Loyalty Tier のデータタイプが作成されたので、 `loyaltyTier` 作成したフィールドグループのフィールドを使用して、以前のフィールドの代わりにデータ型を参照します。

**API 形式**

```http
PATCH /tenant/fieldgroups/{FIELD_GROUP_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{FIELD_GROUP_ID}` | この `meta:altId` または URL エンコード済み `$id` 更新するフィールドグループの |

**リクエスト**

```SHELL
curl -X PATCH \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/fieldgroups/_{TENANT_ID}.mixins.9068fd4ea2abf813f4fd2fc9c8b413ae453ff0efc7636691 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '[
        {
          "op": "replace",
          "path": "/definitions/loyaltyTier/properties/_{TENANT_ID}/properties",
          "value": {
            "loyaltyTier": {
              "title": "Loyalty Tier",
              "$ref": "https://ns.adobe.com/{TENANT_ID}/datatypes/c069d13ebfaaa5980d988e7694d794b37b1784fe11d754cb",
              "description": "Loyalty tier info"
            }
          }
        }
      ]'
```

**応答**

応答に参照 (`$ref`) を `loyaltyTier` オブジェクトに置き換えます。

```JSON
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/mixins/9068fd4ea2abf813f4fd2fc9c8b413ae453ff0efc7636691",
  "meta:altId": "_{TENANT_ID}.mixins.9068fd4ea2abf813f4fd2fc9c8b413ae453ff0efc7636691",
  "meta:resourceType": "mixins",
  "version": "1.1",
  "title": "Loyalty Tier",
  "type": "object",
  "description": "Captures info about the current loyalty tier of a customer.",
  "definitions": {
    "loyaltyTier": {
      "properties": {
        "_{TENANT_ID}": {
          "type": "object",
          "properties": {
            "loyaltyTier": {
              "title": "Loyalty Tier",
              "$ref": "https://ns.adobe.com/{TENANT_ID}/datatypes/c069d13ebfaaa5980d988e7694d794b37b1784fe11d754cb",
              "description": "Loyalty tier info",
              "type": "object",
              "meta:xdmType": "object"
            }
          },
          "meta:xdmType": "object"
        }
      }
    }
  },
  "allOf": [
    {
      "$ref": "#/definitions/loyaltyTier",
      "type": "object",
      "meta:xdmType": "object"
    }
  ],
  "refs": [
    "https://ns.adobe.com/{TENANT_ID}/datatypes/c069d13ebfaaa5980d988e7694d794b37b1784fe11d754cb"
  ],
  "imsOrg": "{ORG_ID}",
  "meta:extensible": true,
  "meta:abstract": true,
  "meta:intendedToExtend": [
    "https://ns.adobe.com/xdm/context/profile"
  ],
  "meta:xdmType": "object",
  "meta:registryMetadata": {
    "repo:createdDate": 1673313004645,
    "repo:lastModifiedDate": 1673378970276,
    "xdm:createdClientId": "{CLIENT_ID}",
    "xdm:lastModifiedClientId": "{CLIENT_ID}",
    "xdm:createdUserId": "{USER_ID}",
    "xdm:lastModifiedUserId": "{USER_ID}",
    "eTag": "4df8fa56d00991590364606bb2e219e1ea8f5717a51c0e6ae57ca956830b6a27",
    "meta:globalLibVersion": "1.38.2"
  },
  "meta:containerId": "tenant",
  "meta:sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
  "meta:sandboxType": "production",
  "meta:tenantNamespace": "_{TENANT_ID}",
  "meta:descriptorStatus": {
    "result": []
  }
}
```

今すぐGETリクエストを実行してスキーマを参照する場合、 `loyaltyTier` プロパティは、以下のデータ型への参照を表示します。 `meta:referencedFrom`:

```JSON
"_{TENANT_ID}": {
  "type": "object",
  "meta:xdmType": "object",
  "properties": {
    "loyaltyTier": {
      "title": "Loyalty Tier",
      "description": "Loyalty tier info",
      "type": "object",
      "meta:xdmType": "object",
      "properties": {
        "id": {
          "title": "Loyalty Tier Identifier",
          "type": "string",
          "description": "Loyalty Tier Identifier.",
          "meta:xdmType": "string"
        },
        "effectiveDate": {
          "title": "Effective Date",
          "type": "string",
          "format": "date-time",
          "description": "Date the member joined their current loyalty tier.",
          "meta:xdmType": "date-time"
        },
        "currentThreshold": {
          "title": "Current Point Threshold",
          "type": "integer",
          "description": "The minimum number of loyalty points the member must maintain to remain in the current tier.",
          "meta:xdmType": "int"
        },
        "nextThreshold": {
          "title": "Next Point Threshold",
          "type": "integer",
          "description": "The number of loyalty points the member must accrue to graduate to the next tier.",
          "meta:xdmType": "int"
        }
      },
      "meta:referencedFrom": "https://ns.adobe.com/{TENANT_ID}/datatypes/c069d13ebfaaa5980d988e7694d794b37b1784fe11d754cb"
    }
  }
}
```

### ID 記述子の定義

スキーマは、データをに取り込むために使用されます [!DNL Experience Platform]. このデータは、最終的に複数のサービスで使用され、個人の単一の統合表示を作成します。この処理を支援するために、主要フィールドを「ID」としてマークし、データ取得時に、これらのフィールド内のデータをその個人の「ID グラフ」に挿入できます。その後、グラフデータには、 [[!DNL Real-Time Customer Profile]](../../profile/home.md) その他 [!DNL Experience Platform] 各顧客の関連付けられたビューを提供するサービス。

一般的に「ID」とマークされるフィールドは次のとおりです。メールアドレス、電話番号、 [[!DNL Experience Cloud ID (ECID)]](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=ja)、CRM ID またはその他の一意の ID フィールド。 組織に固有の識別子は、適切な ID フィールドである場合もあるので、それらを考慮します。

ID 記述子は、 `sourceProperty` の `sourceSchema` は、ID と見なす必要がある一意の識別子です。

記述子の操作について詳しくは、『[スキーマレジストリ開発者ガイド](../api/getting-started.md)』を参照してください。

**API 形式**

```http
POST /tenant/descriptors
```

**リクエスト**

次のリクエストは、 `personalEmail.address` 「ロイヤルティメンバー」スキーマのフィールド。 これは [!DNL Experience Platform] ロイヤリティメンバーの電子メールアドレスを識別子として使用し、個人に関する情報を組み合わせるのに役立てます。 また、この呼び出しでは、このフィールドを、 `xdm:isPrimary` から `true`は、 [リアルタイム顧客プロファイルでのスキーマ使用の有効化](#profile).

```SHELL
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "@type": "xdm:descriptorIdentity",
        "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/533ca5da28087c44344810891b0f03d9",
        "xdm:sourceVersion": 1,
        "xdm:sourceProperty": "/_{TENANT_ID}/loyalty/loyaltyId",
        "xdm:namespace": "Email",
        "xdm:property": "xdm:code",
        "xdm:isPrimary": false
      }'
```

>[!NOTE]
>
>使用可能な「xdm:namespace」値をリストしたり、新しい値を作成したりするには、 [[!DNL Identity Service API]](https://www.adobe.io/experience-platform-apis/references/identity-service). 「xdm:property」の値は、使用する「xdm:namespace」によって「xdm:code」または「xdm:id」になります。

**応答** 

正常な応答では、新しく作成された記述子の詳細（`@id` を含む）とと共に、HTTP ステータス 201（作成済み）が返されます。この `@id` は、 [!DNL Schema Registry] API で記述子を参照するために使用されます。

```JSON
{
  "@id": "719a4391897c097786efbaa6d4262bb928a1cd88540344c6",
  "@type": "xdm:descriptorIdentity",
  "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/ee56b80adc7e03b8214e135a28538fe83c7f85bf87f565b3",
  "xdm:sourceVersion": 1,
  "xdm:sourceProperty": "/personalEmail/address",
  "imsOrg": "{ORG_ID}",
  "version": "1",
  "xdm:namespace": "Email",
  "xdm:property": "xdm:code",
  "xdm:isPrimary": true,
  "meta:containerId": "tenant",
  "meta:sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
  "meta:sandboxType": "production"
}
```

## でのスキーマ使用の有効化 [!DNL Real-Time Customer Profile] {#profile}

スキーマにプライマリ ID 記述子が適用されたら、「ロイヤルティメンバー」スキーマを有効にして、 [!DNL Real-Time Customer Profile] を `union` タグを `meta:immutableTags` 属性。

>[!NOTE]
>
>和集合表示の操作について詳しくは、 [和集合](../api/unions.md) 内 [!DNL Schema Registry] 開発者ガイド。

### を追加します。 `union` タグ

結合された和集合表示にスキーマを含めるには、 `union` タグを `meta:immutableTags` スキーマの属性。 これは、スキーマを更新し、 `meta:immutableTags` 値が `union`.

**API 形式**

```http
PATCH /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | この `meta:altId` または URL エンコード済み `$id` プロファイルに対して有効にするスキーマの |

**リクエスト**

```SHELL
curl -X PATCH \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.533ca5da28087c44344810891b0f03d9 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '[
        { "op": "add", "path": "/meta:immutableTags", "value": ["union"]}
      ]'
```

**応答** 

応答は操作が正常に実行されたことを示し、スキーマには最上位の属性（「和集合」という値を含む `meta:immutableTags` 列）が含まれています。

```JSON
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/ee56b80adc7e03b8214e135a28538fe83c7f85bf87f565b3",
  "meta:altId": "_{TENANT_ID}.schemas.ee56b80adc7e03b8214e135a28538fe83c7f85bf87f565b3",
  "meta:resourceType": "schemas",
  "version": "1.4",
  "title": "Loyalty Members",
  "type": "object",
  "description": "Information for all members of the loyalty program",
  "allOf": [
    {
      "$ref": "https://ns.adobe.com/xdm/context/profile",
      "type": "object",
      "meta:xdmType": "object"
    },
    {
      "$ref": "https://ns.adobe.com/xdm/context/profile-person-details",
      "type": "object",
      "meta:xdmType": "object"
    },
    {
      "$ref": "https://ns.adobe.com/xdm/context/profile-personal-details",
      "type": "object",
      "meta:xdmType": "object"
    },
    {
      "$ref": "https://ns.adobe.com/xdm/mixins/profile/profile-loyalty-details",
      "type": "object",
      "meta:xdmType": "object"
    },
    {
      "$ref": "https://ns.adobe.com/{TENANT_ID}/mixins/9068fd4ea2abf813f4fd2fc9c8b413ae453ff0efc7636691",
      "type": "object",
      "meta:xdmType": "object"
    }
  ],
  "refs": [
    "https://ns.adobe.com/xdm/context/profile-person-details",
    "https://ns.adobe.com/xdm/mixins/profile/profile-loyalty-details",
    "https://ns.adobe.com/xdm/context/profile-personal-details",
    "https://ns.adobe.com/xdm/context/profile",
    "https://ns.adobe.com/{TENANT_ID}/mixins/9068fd4ea2abf813f4fd2fc9c8b413ae453ff0efc7636691"
  ],
  "imsOrg": "{ORG_ID}",
  "meta:extensible": false,
  "meta:abstract": false,
  "meta:extends": [
    "https://ns.adobe.com/xdm/context/profile-person-details",
    "https://ns.adobe.com/xdm/mixins/profile/profile-loyalty-details",
    "https://ns.adobe.com/xdm/context/profile-personal-details",
    "https://ns.adobe.com/xdm/common/auditable",
    "https://ns.adobe.com/xdm/data/record",
    "https://ns.adobe.com/xdm/context/profile",
    "https://ns.adobe.com/{TENANT_ID}/mixins/9068fd4ea2abf813f4fd2fc9c8b413ae453ff0efc7636691"
  ],
  "meta:xdmType": "object",
  "meta:registryMetadata": {
    "repo:createdDate": 1673310304048,
    "repo:lastModifiedDate": 1673380280074,
    "xdm:createdClientId": "{CLIENT_ID}",
    "xdm:lastModifiedClientId": "{CLIENT_ID}",
    "xdm:createdUserId": "{USER_ID}",
    "xdm:lastModifiedUserId": "{USER_ID}",
    "eTag": "c590ccc7a293040d85c2b7d93276480ef4b4aa9a4fcd6991f50fbb47f58bced2",
    "meta:globalLibVersion": "1.38.2"
  },
  "meta:class": "https://ns.adobe.com/xdm/context/profile",
  "meta:containerId": "tenant",
  "meta:sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
  "meta:sandboxType": "production",
  "meta:tenantNamespace": "_{TENANT_ID}",
  "meta:immutableTags": [
    "union"
  ],
  "meta:descriptorStatus": {
    "result": []
  }
}
```

### 和集合でのスキーマのリスト

これで、スキーマがに正常に追加されました [!DNL XDM Individual Profile] 和集合。 同じ和集合に属するすべてのスキーマのリストを表示するには、クエリーパラメーターを使用して GET 要求を実行し、応答をフィルタリングします。

`property` クエリーパラメーターを使用して、 クラスの `meta:immutableTags` と等しい `meta:class` を持つ `$id` フィールドを含むプロファイルのみが返されるように指定できます。[!DNL XDM Individual Profile]

**API 形式**

```http
GET /tenant/schemas?property=meta:immutableTags==union&property=meta:class=={CLASS_ID}
```

**リクエスト**

次の例のリクエストは、 [!DNL XDM Individual Profile] 和集合。

```SHELL
curl -X GET \
  'https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas?property=meta:immutableTags==union&property=meta:class==https://ns.adobe.com/xdm/context/profile' \
  -H 'Accept: application/vnd.adobe.xed-id+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

応答はフィルターされたリストのスキーマで、両方の要件を満たすもののみが含まれます。複数のクエリーパラメーターを使用する場合は、AND が想定されることに注意してください。リスト応答の形式は、 `Accept` ヘッダーがリクエストで送信されました。

```JSON
{
  "results": [
    {
      "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/d29a200b5deb6cfb55d3b865ef627f33",
      "meta:altId": "_{TENANT_ID}.schemas.d29a200b5deb6cfb55d3b865ef627f33",
      "version": "1.2",
      "title": "Profile Schema"
    },
    {
      "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/5d70026f5522fc60b3c81f6523b83c86",
      "meta:altId": "_{TENANT_ID}.schemas.5d70026f5522fc60b3c81f6523b83c86",
      "version": "1.3",
      "title": "CRM Onboarding"
    },
    {
      "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/653e53eb04341d09453c9b6a5fb43d1b4ca9526ec274856d",
      "meta:altId": "_{TENANT_ID}.schemas.653e53eb04341d09453c9b6a5fb43d1b4ca9526ec274856d",
      "version": "1.1",
      "title": "Profile consents"
    },
    {
      "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/ee56b80adc7e03b8214e135a28538fe83c7f85bf87f565b3",
      "meta:altId": "_{TENANT_ID}.schemas.ee56b80adc7e03b8214e135a28538fe83c7f85bf87f565b3",
      "version": "1.4",
      "title": "Loyalty Members"
    }
  ],
  "_page": {
    "orderby": "updated",
    "next": null,
    "count": 4
  },
  "_links": {
    "next": null,
    "global_schemas": {
      "href": "https://platform.adobe.io/data/foundation/schemaregistry/global/schemas?property=meta:immutableTags==union&property=meta:class==https://ns.adobe.com/xdm/context/profile"
    }
  }
}
```

## 次の手順

このチュートリアルでは、標準フィールドグループと定義したフィールドグループの両方を使用してスキーマを正常に構成しました。 これで、このスキーマを使用して、データセットを作成し、レコードデータを Adobe Experience Platform に取得することができます。

このチュートリアル全体で作成した「ロイヤルティメンバー」スキーマは、次の付録で完全に参照できます。スキーマを見ると、フィールドグループが全体的な構造にどのように貢献しているか、およびデータ取り込みに使用できるフィールドを確認できます。

複数のリレーションを作成したら、関係記述子を使用して、スキーマ間の関係を定義できます。詳しくは、[2 つのスキーマの関係を定義する](relationship-api.md)ためのチュートリアルを参照してください 。レジストリ内のすべての操作（GET、POST、PUT、PATCH、DELETE）を実行する方法の詳細な例については、API を使用する際に、『[スキーマレジストリ開発者ガイド](../api/getting-started.md)』を参照してください。

## 付録 {#appendix}

次の情報は、API チュートリアルを補完するものです。

## 完全なロイヤルティメンバースキーマ {#complete-schema}

このチュートリアル全体で、スキーマは小売ロイヤルティメンバープログラムを説明するために構成されています。

スキーマがを実装します。 [!DNL XDM Individual Profile] クラスと、複数のフィールドグループを組み合わせます。 標準を使用して、ロイヤルティメンバーに関する情報を取り込みます [!DNL Demographic Details], [!UICONTROL 個人の連絡先の詳細]、および [!UICONTROL ロイヤルティの詳細] フィールドグループ、およびチュートリアルで定義されたカスタムのロイヤルティ層フィールドグループを通じて。

以下に、完全な「ロイヤルティメンバー」スキーマを JSON 形式で示します。

+++完全なスキーマを表示

```JSON
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/ee56b80adc7e03b8214e135a28538fe83c7f85bf87f565b3",
  "meta:altId": "_{TENANT_ID}.schemas.ee56b80adc7e03b8214e135a28538fe83c7f85bf87f565b3",
  "meta:resourceType": "schemas",
  "version": "1.4",
  "title": "Loyalty Members",
  "type": "object",
  "description": "Information for all members of the loyalty program",
  "properties": {
    "_{TENANT_ID}": {
      "type": "object",
      "properties": {
        "loyaltyTier": {
          "title": "Loyalty Tier",
          "description": "Loyalty tier info",
          "type": "object",
          "meta:xdmType": "object",
          "properties": {
            "id": {
              "title": "Loyalty Tier Identifier",
              "type": "string",
              "description": "Loyalty Tier Identifier.",
              "meta:xdmType": "string"
            },
            "effectiveDate": {
              "title": "Effective Date",
              "type": "string",
              "format": "date-time",
              "description": "Date the member joined their current loyalty tier.",
              "meta:xdmType": "date-time"
            },
            "currentThreshold": {
              "title": "Current Point Threshold",
              "type": "integer",
              "description": "The minimum number of loyalty points the member must maintain to remain in the current tier.",
              "meta:xdmType": "int"
            },
            "nextThreshold": {
              "title": "Next Point Threshold",
              "type": "integer",
              "description": "The number of loyalty points the member must accrue to graduate to the next tier.",
              "meta:xdmType": "int"
            }
          },
          "meta:referencedFrom": "https://ns.adobe.com/{TENANT_ID}/datatypes/c069d13ebfaaa5980d988e7694d794b37b1784fe11d754cb"
        }
      },
      "meta:xdmType": "object"
    },
    "_id": {
      "title": "Identifier",
      "type": "string",
      "format": "uri-reference",
      "description": "A unique identifier for the record.",
      "meta:xdmType": "string",
      "meta:xdmField": "@id"
    },
    "_repo": {
      "properties": {
        "createDate": {
          "type": "string",
          "format": "date-time",
          "meta:immutable": true,
          "meta:usereditable": false,
          "examples": [
            "2004-10-23T12:00:00-06:00"
          ],
          "description": "The server date and time when the resource was created in the repository, such as when an asset file is first uploaded or a directory is created by the server as the parent of a new asset. The date time property should conform to ISO 8601 standard. An example form is \"2004-10-23T12:00:00-06:00\".",
          "meta:xdmType": "date-time",
          "meta:xdmField": "repo:createDate"
        },
        "modifyDate": {
          "type": "string",
          "format": "date-time",
          "meta:usereditable": false,
          "examples": [
            "2004-10-23T12:00:00-06:00"
          ],
          "description": "The server date and time when the resource was last modified in the repository, such as when a new version of an asset is uploaded or a directory's child resource is added or removed. The date time property should conform to ISO 8601 standard. An example form is \"2004-10-23T12:00:00-06:00\".",
          "meta:xdmType": "date-time",
          "meta:xdmField": "repo:modifyDate"
        }
      },
      "type": "object",
      "meta:xdmType": "object",
      "meta:xedConverted": true
    },
    "billingAddress": {
      "title": "Billing Address",
      "description": "Billing postal address.",
      "type": "object",
      "meta:xdmType": "object",
      "properties": {
        "_id": {
          "title": "Coordinates ID",
          "type": "string",
          "format": "uri-reference",
          "description": "The unique identifier of the coordinates.",
          "meta:xdmType": "string",
          "meta:xdmField": "@id"
        },
        "_repo": {
          "properties": {
            "createDate": {
              "type": "string",
              "format": "date-time",
              "meta:immutable": true,
              "meta:usereditable": false,
              "examples": [
                "2004-10-23T12:00:00-06:00"
              ],
              "description": "The server date and time when the resource was created in the repository, such as when an asset file is first uploaded or a directory is created by the server as the parent of a new asset. The date time property should conform to ISO 8601 standard. An example form is \"2004-10-23T12:00:00-06:00\".",
              "meta:xdmType": "date-time",
              "meta:xdmField": "repo:createDate"
            },
            "modifyDate": {
              "type": "string",
              "format": "date-time",
              "meta:usereditable": false,
              "examples": [
                "2004-10-23T12:00:00-06:00"
              ],
              "description": "The server date and time when the resource was last modified in the repository, such as when a new version of an asset is uploaded or a directory's child resource is added or removed. The date time property should conform to ISO 8601 standard. An example form is \"2004-10-23T12:00:00-06:00\".",
              "meta:xdmType": "date-time",
              "meta:xdmField": "repo:modifyDate"
            }
          },
          "type": "object",
          "meta:xdmType": "object",
          "meta:xedConverted": true
        },
        "_schema": {
          "properties": {
            "description": {
              "title": "Description",
              "type": "string",
              "description": "A description of what the coordinates identify.",
              "meta:xdmType": "string",
              "meta:xdmField": "schema:description"
            },
            "elevation": {
              "title": "Elevation",
              "type": "number",
              "description": "The specific elevation of the defined coordinate. The value conforms to the [WGS84](http://gisgeography.com/wgs84-world-geodetic-system/) datum and is measured in meters.",
              "meta:xdmType": "number",
              "meta:xdmField": "schema:elevation"
            },
            "latitude": {
              "title": "Latitude",
              "type": "number",
              "minimum": -90,
              "maximum": 90,
              "description": "The signed vertical coordinate of a geographic point.",
              "meta:xdmType": "number",
              "meta:xdmField": "schema:latitude"
            },
            "longitude": {
              "title": "Longitude",
              "type": "number",
              "minimum": -180,
              "maximum": 180,
              "description": "The signed horizontal coordinate of a geographic point.",
              "meta:xdmType": "number",
              "meta:xdmField": "schema:longitude"
            }
          },
          "type": "object",
          "meta:xdmType": "object",
          "meta:xedConverted": true
        },
        "city": {
          "title": "City",
          "type": "string",
          "description": "The name of the city.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:city"
        },
        "country": {
          "title": "Country",
          "type": "string",
          "description": "The name of the government-administered territory. Other than `xdm:countryCode`, this is a free-form field that can have the country name in any language.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:country"
        },
        "countryCode": {
          "title": "Country code",
          "type": "string",
          "pattern": "^[A-Z]{2}$",
          "description": "The two-character [ISO 3166-1 alpha-2](https://datahub.io/core/country-list) code for the country.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:countryCode"
        },
        "createdByBatchID": {
          "title": "Created by batch identifier",
          "type": "string",
          "format": "uri-reference",
          "description": "The dataset files in Catalog which has been originating the creation of the record.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:createdByBatchID"
        },
        "dmaID": {
          "title": "Designated market area",
          "type": "integer",
          "description": "The Nielsen media research designated market area.",
          "meta:xdmType": "int",
          "meta:xdmField": "xdm:dmaID"
        },
        "label": {
          "title": "Label",
          "type": "string",
          "description": "Free form name of the address.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:label"
        },
        "lastVerifiedDate": {
          "title": "Last verified date",
          "type": "string",
          "format": "date",
          "description": "The date that the address was last verified as still associated to the person.",
          "meta:xdmType": "date",
          "meta:xdmField": "xdm:lastVerifiedDate"
        },
        "modifiedByBatchID": {
          "title": "Modified by batch identifier",
          "type": "string",
          "format": "uri-reference",
          "description": "The last dataset files in Catalog which has modified the record. At creation time, `modifiedByBatchID` is set as `createdByBatchID`.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:modifiedByBatchID"
        },
        "msaID": {
          "title": "Metropolitan statistical area",
          "type": "integer",
          "description": "The metropolitan statistical area in the United States where the observation occurred.",
          "meta:xdmType": "int",
          "meta:xdmField": "xdm:msaID"
        },
        "postOfficeBox": {
          "title": "Post office box",
          "type": "string",
          "description": "Post office box of the address.",
          "maxLength": 20,
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:postOfficeBox"
        },
        "postalCode": {
          "title": "Postal code",
          "type": "string",
          "description": "The postal code of the location. Postal codes are not available for all countries. In some countries, this will only contain part of the postal code.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:postalCode"
        },
        "primary": {
          "title": "Primary",
          "type": "boolean",
          "description": "Primary address indicator. A profile can have only one `primary` address at a given point of time.",
          "meta:xdmType": "boolean",
          "meta:xdmField": "xdm:primary"
        },
        "region": {
          "title": "Region",
          "type": "string",
          "description": "The region, county, or district portion of the address.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:region"
        },
        "repositoryCreatedBy": {
          "title": "Created by user identifier",
          "type": "string",
          "description": "User ID of who created the record.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:repositoryCreatedBy"
        },
        "repositoryLastModifiedBy": {
          "title": "Modified by user identifier",
          "type": "string",
          "description": "User ID of who last modified the record. At creation time, `modifiedByUser` is set as `createdByUser`.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:repositoryLastModifiedBy"
        },
        "state": {
          "title": "State",
          "type": "string",
          "description": "The name of the State. This is a free-form field.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:state"
        },
        "stateProvince": {
          "title": "State or province",
          "type": "string",
          "description": "The state, or province portion of the observation. The format follows the [ISO 3166-2 (country and subdivision)][http://www.unece.org/cefact/locode/subdivisions.html] standard.",
          "examples": [
            "US-CA",
            "DE-BB",
            "JP-13"
          ],
          "pattern": "([A-Z]{2}-[A-Z0-9]{1,3}|)",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:stateProvince"
        },
        "status": {
          "title": "Status",
          "type": "string",
          "description": "An indication as to the ability to use the address.",
          "default": "active",
          "meta:enum": {
            "active": "Active",
            "incomplete": "Incomplete",
            "pending_verification": "Pending verification",
            "blacklisted": "Blacklisted",
            "blocked": "Blocked"
          },
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:status"
        },
        "statusReason": {
          "title": "Status reason",
          "type": "string",
          "description": "A description of the current status.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:statusReason"
        },
        "street1": {
          "title": "Street 1",
          "type": "string",
          "description": "Primary street level information, apartment number, street number, and street name.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:street1"
        },
        "street2": {
          "title": "Street 2",
          "type": "string",
          "description": "Optional street information second line.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:street2"
        },
        "street3": {
          "title": "Street 3",
          "type": "string",
          "description": "Optional street information third line.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:street3"
        },
        "street4": {
          "title": "Street 4",
          "type": "string",
          "description": "Optional street information fourth line.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:street4"
        }
      },
      "meta:referencedFrom": "https://ns.adobe.com/xdm/common/address",
      "meta:xdmField": "xdm:billingAddress"
    },
    "createdByBatchID": {
      "title": "Created by batch identifier",
      "type": "string",
      "format": "uri-reference",
      "description": "The dataset files in Catalog which has been originating the creation of the record.",
      "meta:xdmType": "string",
      "meta:xdmField": "xdm:createdByBatchID"
    },
    "faxPhone": {
      "title": "Fax Phone",
      "description": "Fax phone number.",
      "type": "object",
      "meta:xdmType": "object",
      "properties": {
        "countryCode": {
          "title": "Country Calling Code",
          "type": "string",
          "description": "Country calling code (CC) as defined by E.164.",
          "minLength": 1,
          "maxLength": 3,
          "pattern": "^[0-9]{1,3}?$",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:countryCode"
        },
        "extension": {
          "title": "Extension",
          "type": "string",
          "description": "The internal dialing number used to call from a private exchange, operator, or switchboard.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:extension"
        },
        "number": {
          "title": "Number",
          "type": "string",
          "description": "The phone number. Note the phone number is a string and may include meaningful characters such as brackets '()', hyphens '-', or characters to indicate sub-dialing identifiers like extensions 'x' for example,  1-353(0)18391111 or +613 9403600x1234.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:number"
        },
        "primary": {
          "title": "Primary",
          "type": "boolean",
          "description": "Primary phone number indicator. Unlike address or email address, there can be multiple primary phone numbers; one per communication channel. The communication channel is defined by the type: `textMessaging`, `mobile`, `phone`, `home`, `work`, `unknown`, and `fax`.",
          "meta:xdmType": "boolean",
          "meta:xdmField": "xdm:primary"
        },
        "status": {
          "title": "Status",
          "type": "string",
          "description": "An indication as to the ability to use the phone number.",
          "default": "active",
          "meta:enum": {
            "active": "Active",
            "incomplete": "Incomplete",
            "blacklisted": "Blacklisted",
            "blocked": "Blocked"
          },
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:status"
        },
        "statusReason": {
          "title": "Status reason",
          "type": "string",
          "description": "A description of the current status.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:statusReason"
        },
        "validity": {
          "title": "Validity",
          "type": "string",
          "description": "A level of technical correctness of the phone number.",
          "meta:enum": {
            "consistent": "Consistent",
            "inconsistent": "Inconsistent",
            "incomplete": "Incomplete",
            "successfullyUsed": "Successfully used"
          },
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:validity"
        }
      },
      "meta:referencedFrom": "https://ns.adobe.com/xdm/context/phonenumber",
      "meta:xdmField": "xdm:faxPhone"
    },
    "homeAddress": {
      "title": "Home Address",
      "description": "A home postal address.",
      "type": "object",
      "meta:xdmType": "object",
      "properties": {
        "_id": {
          "title": "Coordinates ID",
          "type": "string",
          "format": "uri-reference",
          "description": "The unique identifier of the coordinates.",
          "meta:xdmType": "string",
          "meta:xdmField": "@id"
        },
        "_repo": {
          "properties": {
            "createDate": {
              "type": "string",
              "format": "date-time",
              "meta:immutable": true,
              "meta:usereditable": false,
              "examples": [
                "2004-10-23T12:00:00-06:00"
              ],
              "description": "The server date and time when the resource was created in the repository, such as when an asset file is first uploaded or a directory is created by the server as the parent of a new asset. The date time property should conform to ISO 8601 standard. An example form is \"2004-10-23T12:00:00-06:00\".",
              "meta:xdmType": "date-time",
              "meta:xdmField": "repo:createDate"
            },
            "modifyDate": {
              "type": "string",
              "format": "date-time",
              "meta:usereditable": false,
              "examples": [
                "2004-10-23T12:00:00-06:00"
              ],
              "description": "The server date and time when the resource was last modified in the repository, such as when a new version of an asset is uploaded or a directory's child resource is added or removed. The date time property should conform to ISO 8601 standard. An example form is \"2004-10-23T12:00:00-06:00\".",
              "meta:xdmType": "date-time",
              "meta:xdmField": "repo:modifyDate"
            }
          },
          "type": "object",
          "meta:xdmType": "object",
          "meta:xedConverted": true
        },
        "_schema": {
          "properties": {
            "description": {
              "title": "Description",
              "type": "string",
              "description": "A description of what the coordinates identify.",
              "meta:xdmType": "string",
              "meta:xdmField": "schema:description"
            },
            "elevation": {
              "title": "Elevation",
              "type": "number",
              "description": "The specific elevation of the defined coordinate. The value conforms to the [WGS84](http://gisgeography.com/wgs84-world-geodetic-system/) datum and is measured in meters.",
              "meta:xdmType": "number",
              "meta:xdmField": "schema:elevation"
            },
            "latitude": {
              "title": "Latitude",
              "type": "number",
              "minimum": -90,
              "maximum": 90,
              "description": "The signed vertical coordinate of a geographic point.",
              "meta:xdmType": "number",
              "meta:xdmField": "schema:latitude"
            },
            "longitude": {
              "title": "Longitude",
              "type": "number",
              "minimum": -180,
              "maximum": 180,
              "description": "The signed horizontal coordinate of a geographic point.",
              "meta:xdmType": "number",
              "meta:xdmField": "schema:longitude"
            }
          },
          "type": "object",
          "meta:xdmType": "object",
          "meta:xedConverted": true
        },
        "city": {
          "title": "City",
          "type": "string",
          "description": "The name of the city.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:city"
        },
        "country": {
          "title": "Country",
          "type": "string",
          "description": "The name of the government-administered territory. Other than `xdm:countryCode`, this is a free-form field that can have the country name in any language.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:country"
        },
        "countryCode": {
          "title": "Country code",
          "type": "string",
          "pattern": "^[A-Z]{2}$",
          "description": "The two-character [ISO 3166-1 alpha-2](https://datahub.io/core/country-list) code for the country.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:countryCode"
        },
        "createdByBatchID": {
          "title": "Created by batch identifier",
          "type": "string",
          "format": "uri-reference",
          "description": "The dataset files in Catalog which has been originating the creation of the record.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:createdByBatchID"
        },
        "dmaID": {
          "title": "Designated market area",
          "type": "integer",
          "description": "The Nielsen media research designated market area.",
          "meta:xdmType": "int",
          "meta:xdmField": "xdm:dmaID"
        },
        "label": {
          "title": "Label",
          "type": "string",
          "description": "Free form name of the address.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:label"
        },
        "lastVerifiedDate": {
          "title": "Last verified date",
          "type": "string",
          "format": "date",
          "description": "The date that the address was last verified as still associated to the person.",
          "meta:xdmType": "date",
          "meta:xdmField": "xdm:lastVerifiedDate"
        },
        "modifiedByBatchID": {
          "title": "Modified by batch identifier",
          "type": "string",
          "format": "uri-reference",
          "description": "The last dataset files in Catalog which has modified the record. At creation time, `modifiedByBatchID` is set as `createdByBatchID`.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:modifiedByBatchID"
        },
        "msaID": {
          "title": "Metropolitan statistical area",
          "type": "integer",
          "description": "The metropolitan statistical area in the United States where the observation occurred.",
          "meta:xdmType": "int",
          "meta:xdmField": "xdm:msaID"
        },
        "postOfficeBox": {
          "title": "Post office box",
          "type": "string",
          "description": "Post office box of the address.",
          "maxLength": 20,
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:postOfficeBox"
        },
        "postalCode": {
          "title": "Postal code",
          "type": "string",
          "description": "The postal code of the location. Postal codes are not available for all countries. In some countries, this will only contain part of the postal code.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:postalCode"
        },
        "primary": {
          "title": "Primary",
          "type": "boolean",
          "description": "Primary address indicator. A profile can have only one `primary` address at a given point of time.",
          "meta:xdmType": "boolean",
          "meta:xdmField": "xdm:primary"
        },
        "region": {
          "title": "Region",
          "type": "string",
          "description": "The region, county, or district portion of the address.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:region"
        },
        "repositoryCreatedBy": {
          "title": "Created by user identifier",
          "type": "string",
          "description": "User ID of who created the record.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:repositoryCreatedBy"
        },
        "repositoryLastModifiedBy": {
          "title": "Modified by user identifier",
          "type": "string",
          "description": "User ID of who last modified the record. At creation time, `modifiedByUser` is set as `createdByUser`.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:repositoryLastModifiedBy"
        },
        "state": {
          "title": "State",
          "type": "string",
          "description": "The name of the State. This is a free-form field.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:state"
        },
        "stateProvince": {
          "title": "State or province",
          "type": "string",
          "description": "The state, or province portion of the observation. The format follows the [ISO 3166-2 (country and subdivision)][http://www.unece.org/cefact/locode/subdivisions.html] standard.",
          "examples": [
            "US-CA",
            "DE-BB",
            "JP-13"
          ],
          "pattern": "([A-Z]{2}-[A-Z0-9]{1,3}|)",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:stateProvince"
        },
        "status": {
          "title": "Status",
          "type": "string",
          "description": "An indication as to the ability to use the address.",
          "default": "active",
          "meta:enum": {
            "active": "Active",
            "incomplete": "Incomplete",
            "pending_verification": "Pending verification",
            "blacklisted": "Blacklisted",
            "blocked": "Blocked"
          },
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:status"
        },
        "statusReason": {
          "title": "Status reason",
          "type": "string",
          "description": "A description of the current status.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:statusReason"
        },
        "street1": {
          "title": "Street 1",
          "type": "string",
          "description": "Primary street level information, apartment number, street number, and street name.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:street1"
        },
        "street2": {
          "title": "Street 2",
          "type": "string",
          "description": "Optional street information second line.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:street2"
        },
        "street3": {
          "title": "Street 3",
          "type": "string",
          "description": "Optional street information third line.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:street3"
        },
        "street4": {
          "title": "Street 4",
          "type": "string",
          "description": "Optional street information fourth line.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:street4"
        }
      },
      "meta:referencedFrom": "https://ns.adobe.com/xdm/common/address",
      "meta:xdmField": "xdm:homeAddress"
    },
    "homePhone": {
      "title": "Home Phone",
      "description": "Home phone number.",
      "type": "object",
      "meta:xdmType": "object",
      "properties": {
        "countryCode": {
          "title": "Country Calling Code",
          "type": "string",
          "description": "Country calling code (CC) as defined by E.164.",
          "minLength": 1,
          "maxLength": 3,
          "pattern": "^[0-9]{1,3}?$",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:countryCode"
        },
        "extension": {
          "title": "Extension",
          "type": "string",
          "description": "The internal dialing number used to call from a private exchange, operator, or switchboard.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:extension"
        },
        "number": {
          "title": "Number",
          "type": "string",
          "description": "The phone number. Note the phone number is a string and may include meaningful characters such as brackets '()', hyphens '-', or characters to indicate sub-dialing identifiers like extensions 'x' for example,  1-353(0)18391111 or +613 9403600x1234.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:number"
        },
        "primary": {
          "title": "Primary",
          "type": "boolean",
          "description": "Primary phone number indicator. Unlike address or email address, there can be multiple primary phone numbers; one per communication channel. The communication channel is defined by the type: `textMessaging`, `mobile`, `phone`, `home`, `work`, `unknown`, and `fax`.",
          "meta:xdmType": "boolean",
          "meta:xdmField": "xdm:primary"
        },
        "status": {
          "title": "Status",
          "type": "string",
          "description": "An indication as to the ability to use the phone number.",
          "default": "active",
          "meta:enum": {
            "active": "Active",
            "incomplete": "Incomplete",
            "blacklisted": "Blacklisted",
            "blocked": "Blocked"
          },
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:status"
        },
        "statusReason": {
          "title": "Status reason",
          "type": "string",
          "description": "A description of the current status.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:statusReason"
        },
        "validity": {
          "title": "Validity",
          "type": "string",
          "description": "A level of technical correctness of the phone number.",
          "meta:enum": {
            "consistent": "Consistent",
            "inconsistent": "Inconsistent",
            "incomplete": "Incomplete",
            "successfullyUsed": "Successfully used"
          },
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:validity"
        }
      },
      "meta:referencedFrom": "https://ns.adobe.com/xdm/context/phonenumber",
      "meta:xdmField": "xdm:homePhone"
    },
    "loyalty": {
      "type": "object",
      "description": "Captures details related to the customer's loyalty rewards.",
      "properties": {
        "joinDate": {
          "title": "Program Join Date",
          "type": "string",
          "format": "date-time",
          "description": "Date which the visitor registered for the loyalty program.",
          "meta:xdmType": "date-time",
          "meta:xdmField": "xdm:joinDate"
        },
        "loyaltyID": {
          "title": "Program ID",
          "type": "array",
          "items": {
            "type": "string",
            "meta:xdmType": "string"
          },
          "description": "The loyalty program ID(s) associated with a specific user, if they are enrolled in the client's loyalty program.",
          "meta:xdmType": "array",
          "meta:xdmField": "xdm:loyaltyID"
        },
        "points": {
          "title": "Program Points Balance",
          "type": "number",
          "description": "Current balance of the loyalty points/awards in a visitor's loyalty account.",
          "meta:xdmType": "number",
          "meta:xdmField": "xdm:points"
        },
        "pointsExpiration": {
          "title": "Points Expiration",
          "type": "array",
          "items": {
            "type": "object",
            "properties": {
              "pointsExpirationDate": {
                "type": "string",
                "format": "date-time",
                "description": "Date on which the given portion of the loyalty points expire.",
                "meta:xdmType": "date-time",
                "meta:xdmField": "xdm:pointsExpirationDate"
              },
              "pointsExpiring": {
                "title": "Points Expiring",
                "type": "number",
                "description": "Point balance expiring as of the associated expiration date.",
                "meta:xdmType": "number",
                "meta:xdmField": "xdm:pointsExpiring"
              }
            },
            "meta:xdmType": "object"
          },
          "meta:xdmType": "array",
          "meta:xdmField": "xdm:pointsExpiration"
        },
        "pointsRedeemed": {
          "title": "Points Redeemed",
          "type": "number",
          "description": "Amount of points applied toward a purchase or otherwise redeemed.",
          "meta:xdmType": "number",
          "meta:xdmField": "xdm:pointsRedeemed"
        },
        "program": {
          "title": "Program Name",
          "type": "string",
          "description": "This should define the loyalty progam in which a visitor is enrolled.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:program"
        },
        "status": {
          "title": "Status",
          "type": "string",
          "description": "Captures the visitor's loyalty progam status, such as active, disabled, or suspended.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:status"
        },
        "tier": {
          "title": "Tier",
          "type": "string",
          "description": "Captures the loyalty progam tier in which a visitor is enrolled.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:tier"
        },
        "upgradeDate": {
          "title": "Program Name",
          "type": "string",
          "description": "Date which the customer was upgraded to the next tier level.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:upgradeDate"
        }
      },
      "meta:xdmType": "object",
      "meta:xdmField": "xdm:loyalty"
    },
    "mailingAddress": {
      "title": "Mailing Address",
      "description": "Mailing postal address.",
      "type": "object",
      "meta:xdmType": "object",
      "properties": {
        "_id": {
          "title": "Coordinates ID",
          "type": "string",
          "format": "uri-reference",
          "description": "The unique identifier of the coordinates.",
          "meta:xdmType": "string",
          "meta:xdmField": "@id"
        },
        "_repo": {
          "properties": {
            "createDate": {
              "type": "string",
              "format": "date-time",
              "meta:immutable": true,
              "meta:usereditable": false,
              "examples": [
                "2004-10-23T12:00:00-06:00"
              ],
              "description": "The server date and time when the resource was created in the repository, such as when an asset file is first uploaded or a directory is created by the server as the parent of a new asset. The date time property should conform to ISO 8601 standard. An example form is \"2004-10-23T12:00:00-06:00\".",
              "meta:xdmType": "date-time",
              "meta:xdmField": "repo:createDate"
            },
            "modifyDate": {
              "type": "string",
              "format": "date-time",
              "meta:usereditable": false,
              "examples": [
                "2004-10-23T12:00:00-06:00"
              ],
              "description": "The server date and time when the resource was last modified in the repository, such as when a new version of an asset is uploaded or a directory's child resource is added or removed. The date time property should conform to ISO 8601 standard. An example form is \"2004-10-23T12:00:00-06:00\".",
              "meta:xdmType": "date-time",
              "meta:xdmField": "repo:modifyDate"
            }
          },
          "type": "object",
          "meta:xdmType": "object",
          "meta:xedConverted": true
        },
        "_schema": {
          "properties": {
            "description": {
              "title": "Description",
              "type": "string",
              "description": "A description of what the coordinates identify.",
              "meta:xdmType": "string",
              "meta:xdmField": "schema:description"
            },
            "elevation": {
              "title": "Elevation",
              "type": "number",
              "description": "The specific elevation of the defined coordinate. The value conforms to the [WGS84](http://gisgeography.com/wgs84-world-geodetic-system/) datum and is measured in meters.",
              "meta:xdmType": "number",
              "meta:xdmField": "schema:elevation"
            },
            "latitude": {
              "title": "Latitude",
              "type": "number",
              "minimum": -90,
              "maximum": 90,
              "description": "The signed vertical coordinate of a geographic point.",
              "meta:xdmType": "number",
              "meta:xdmField": "schema:latitude"
            },
            "longitude": {
              "title": "Longitude",
              "type": "number",
              "minimum": -180,
              "maximum": 180,
              "description": "The signed horizontal coordinate of a geographic point.",
              "meta:xdmType": "number",
              "meta:xdmField": "schema:longitude"
            }
          },
          "type": "object",
          "meta:xdmType": "object",
          "meta:xedConverted": true
        },
        "city": {
          "title": "City",
          "type": "string",
          "description": "The name of the city.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:city"
        },
        "country": {
          "title": "Country",
          "type": "string",
          "description": "The name of the government-administered territory. Other than `xdm:countryCode`, this is a free-form field that can have the country name in any language.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:country"
        },
        "countryCode": {
          "title": "Country code",
          "type": "string",
          "pattern": "^[A-Z]{2}$",
          "description": "The two-character [ISO 3166-1 alpha-2](https://datahub.io/core/country-list) code for the country.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:countryCode"
        },
        "createdByBatchID": {
          "title": "Created by batch identifier",
          "type": "string",
          "format": "uri-reference",
          "description": "The dataset files in Catalog which has been originating the creation of the record.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:createdByBatchID"
        },
        "dmaID": {
          "title": "Designated market area",
          "type": "integer",
          "description": "The Nielsen media research designated market area.",
          "meta:xdmType": "int",
          "meta:xdmField": "xdm:dmaID"
        },
        "label": {
          "title": "Label",
          "type": "string",
          "description": "Free form name of the address.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:label"
        },
        "lastVerifiedDate": {
          "title": "Last verified date",
          "type": "string",
          "format": "date",
          "description": "The date that the address was last verified as still associated to the person.",
          "meta:xdmType": "date",
          "meta:xdmField": "xdm:lastVerifiedDate"
        },
        "modifiedByBatchID": {
          "title": "Modified by batch identifier",
          "type": "string",
          "format": "uri-reference",
          "description": "The last dataset files in Catalog which has modified the record. At creation time, `modifiedByBatchID` is set as `createdByBatchID`.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:modifiedByBatchID"
        },
        "msaID": {
          "title": "Metropolitan statistical area",
          "type": "integer",
          "description": "The metropolitan statistical area in the United States where the observation occurred.",
          "meta:xdmType": "int",
          "meta:xdmField": "xdm:msaID"
        },
        "postOfficeBox": {
          "title": "Post office box",
          "type": "string",
          "description": "Post office box of the address.",
          "maxLength": 20,
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:postOfficeBox"
        },
        "postalCode": {
          "title": "Postal code",
          "type": "string",
          "description": "The postal code of the location. Postal codes are not available for all countries. In some countries, this will only contain part of the postal code.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:postalCode"
        },
        "primary": {
          "title": "Primary",
          "type": "boolean",
          "description": "Primary address indicator. A profile can have only one `primary` address at a given point of time.",
          "meta:xdmType": "boolean",
          "meta:xdmField": "xdm:primary"
        },
        "region": {
          "title": "Region",
          "type": "string",
          "description": "The region, county, or district portion of the address.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:region"
        },
        "repositoryCreatedBy": {
          "title": "Created by user identifier",
          "type": "string",
          "description": "User ID of who created the record.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:repositoryCreatedBy"
        },
        "repositoryLastModifiedBy": {
          "title": "Modified by user identifier",
          "type": "string",
          "description": "User ID of who last modified the record. At creation time, `modifiedByUser` is set as `createdByUser`.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:repositoryLastModifiedBy"
        },
        "state": {
          "title": "State",
          "type": "string",
          "description": "The name of the State. This is a free-form field.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:state"
        },
        "stateProvince": {
          "title": "State or province",
          "type": "string",
          "description": "The state, or province portion of the observation. The format follows the [ISO 3166-2 (country and subdivision)][http://www.unece.org/cefact/locode/subdivisions.html] standard.",
          "examples": [
            "US-CA",
            "DE-BB",
            "JP-13"
          ],
          "pattern": "([A-Z]{2}-[A-Z0-9]{1,3}|)",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:stateProvince"
        },
        "status": {
          "title": "Status",
          "type": "string",
          "description": "An indication as to the ability to use the address.",
          "default": "active",
          "meta:enum": {
            "active": "Active",
            "incomplete": "Incomplete",
            "pending_verification": "Pending verification",
            "blacklisted": "Blacklisted",
            "blocked": "Blocked"
          },
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:status"
        },
        "statusReason": {
          "title": "Status reason",
          "type": "string",
          "description": "A description of the current status.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:statusReason"
        },
        "street1": {
          "title": "Street 1",
          "type": "string",
          "description": "Primary street level information, apartment number, street number, and street name.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:street1"
        },
        "street2": {
          "title": "Street 2",
          "type": "string",
          "description": "Optional street information second line.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:street2"
        },
        "street3": {
          "title": "Street 3",
          "type": "string",
          "description": "Optional street information third line.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:street3"
        },
        "street4": {
          "title": "Street 4",
          "type": "string",
          "description": "Optional street information fourth line.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:street4"
        }
      },
      "meta:referencedFrom": "https://ns.adobe.com/xdm/common/address",
      "meta:xdmField": "xdm:mailingAddress"
    },
    "mobilePhone": {
      "title": "Mobile Phone",
      "description": "Mobile phone number.",
      "type": "object",
      "meta:xdmType": "object",
      "properties": {
        "countryCode": {
          "title": "Country Calling Code",
          "type": "string",
          "description": "Country calling code (CC) as defined by E.164.",
          "minLength": 1,
          "maxLength": 3,
          "pattern": "^[0-9]{1,3}?$",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:countryCode"
        },
        "extension": {
          "title": "Extension",
          "type": "string",
          "description": "The internal dialing number used to call from a private exchange, operator, or switchboard.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:extension"
        },
        "number": {
          "title": "Number",
          "type": "string",
          "description": "The phone number. Note the phone number is a string and may include meaningful characters such as brackets '()', hyphens '-', or characters to indicate sub-dialing identifiers like extensions 'x' for example,  1-353(0)18391111 or +613 9403600x1234.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:number"
        },
        "primary": {
          "title": "Primary",
          "type": "boolean",
          "description": "Primary phone number indicator. Unlike address or email address, there can be multiple primary phone numbers; one per communication channel. The communication channel is defined by the type: `textMessaging`, `mobile`, `phone`, `home`, `work`, `unknown`, and `fax`.",
          "meta:xdmType": "boolean",
          "meta:xdmField": "xdm:primary"
        },
        "status": {
          "title": "Status",
          "type": "string",
          "description": "An indication as to the ability to use the phone number.",
          "default": "active",
          "meta:enum": {
            "active": "Active",
            "incomplete": "Incomplete",
            "blacklisted": "Blacklisted",
            "blocked": "Blocked"
          },
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:status"
        },
        "statusReason": {
          "title": "Status reason",
          "type": "string",
          "description": "A description of the current status.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:statusReason"
        },
        "validity": {
          "title": "Validity",
          "type": "string",
          "description": "A level of technical correctness of the phone number.",
          "meta:enum": {
            "consistent": "Consistent",
            "inconsistent": "Inconsistent",
            "incomplete": "Incomplete",
            "successfullyUsed": "Successfully used"
          },
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:validity"
        }
      },
      "meta:referencedFrom": "https://ns.adobe.com/xdm/context/phonenumber",
      "meta:xdmField": "xdm:mobilePhone"
    },
    "modifiedByBatchID": {
      "title": "Modified by batch identifier",
      "type": "string",
      "format": "uri-reference",
      "description": "The last dataset files in Catalog which has modified the record. At creation time, `modifiedByBatchID` is set as `createdByBatchID`.",
      "meta:xdmType": "string",
      "meta:xdmField": "xdm:modifiedByBatchID"
    },
    "person": {
      "title": "Person",
      "description": "An individual actor, contact, or owner.",
      "type": "object",
      "meta:xdmType": "object",
      "properties": {
        "birthDate": {
          "title": "Birth date(YYYY-MM-DD)",
          "type": "string",
          "format": "date",
          "description": "The full date a person was born.",
          "meta:xdmType": "date",
          "meta:xdmField": "xdm:birthDate"
        },
        "birthDayAndMonth": {
          "title": "Birth date (MM-DD)",
          "type": "string",
          "pattern": "[0-1][0-9]-[0-9][0-9]",
          "description": "The day and month a person was born, in the format MM-DD. This field should be used when the day and month of a person's birth is known, but not the year.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:birthDayAndMonth"
        },
        "birthYear": {
          "title": "Birth year",
          "type": "integer",
          "description": "The year a person was born including the century, for example, 1983.  This field should be used when only the person's age is known, not the full birth date.",
          "minimum": 1,
          "maximum": 32767,
          "meta:xdmType": "short",
          "meta:xdmField": "xdm:birthYear"
        },
        "gender": {
          "title": "Gender",
          "type": "string",
          "enum": [
            "male",
            "female",
            "not_specified",
            "non_specific"
          ],
          "meta:enum": {
            "male": "Male",
            "female": "Female",
            "not_specified": "Not Specified",
            "non_specific": "Non-specific"
          },
          "description": "Gender identity of the person.\n",
          "default": "not_specified",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:gender"
        },
        "maritalStatus": {
          "title": "Marital Status",
          "type": "string",
          "enum": [
            "married",
            "single",
            "divorced",
            "widowed",
            "not_specified"
          ],
          "meta:enum": {
            "married": "Married",
            "single": "Single",
            "divorced": "Divorced",
            "widowed": "Widowed",
            "not_specified": "Not Specified"
          },
          "description": "Describes a person's relationship with a significant other.",
          "default": "not_specified",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:maritalStatus"
        },
        "name": {
          "title": "Full name",
          "description": "The person's full name.",
          "type": "object",
          "meta:xdmType": "object",
          "properties": {
            "courtesyTitle": {
              "title": "Courtesy title",
              "type": "string",
              "description": "Normally an abbreviation of a persons title, honorific, or salutation. The `courtesyTitle` is used in front of full or last name in opening texts. For example, Mr. Miss. or Dr.",
              "meta:xdmType": "string",
              "meta:xdmField": "xdm:courtesyTitle"
            },
            "firstName": {
              "title": "First name",
              "type": "string",
              "description": "The first segment of the name in the writing order most commonly accepted in the language of the name. In many cultures this is the preferred personal or given name. The `firstName` and `lastName` properties have been introduced to maintain compatibility with existing systems that model names in a simplified, non-semantic, and non-internationalizable way. Using `xdm:fullName` is always preferable.",
              "meta:xdmType": "string",
              "meta:xdmField": "xdm:firstName"
            },
            "fullName": {
              "title": "Full name",
              "type": "string",
              "description": "The full name of the person, in writing order most commonly accepted in the language of the name.",
              "meta:xdmType": "string",
              "meta:xdmField": "xdm:fullName"
            },
            "lastName": {
              "title": "Last name",
              "type": "string",
              "description": "The last segment of the name in the writing order most commonly accepted in the language of the name. In many cultures this is the inherited family name, surname, patronymic, or matronymic name. The `firstName` and `lastName` properties have been introduced to maintain compatibility with existing systems that model names in a simplified, non-semantic, and non-internationalizable way. Using `xdm:fullName` is always preferable.",
              "meta:xdmType": "string",
              "meta:xdmField": "xdm:lastName"
            },
            "middleName": {
              "title": "Middle name",
              "type": "string",
              "description": "Middle, alternative, or additional names supplied between the first name and last name.",
              "meta:xdmType": "string",
              "meta:xdmField": "xdm:middleName"
            },
            "suffix": {
              "title": "Suffix",
              "type": "string",
              "description": "A group of letters provided after a person's name to provide additional information. The `suffix` is used at the end of someones name. For example Jr., Sr., M.D., PhD, I, II, III, etc.",
              "meta:xdmType": "string",
              "meta:xdmField": "xdm:suffix"
            }
          },
          "meta:referencedFrom": "https://ns.adobe.com/xdm/context/person-name",
          "meta:xdmField": "xdm:name"
        },
        "nationality": {
          "title": "Nationality",
          "type": "string",
          "pattern": "^[A-Z]{2}$",
          "description": "The legal relationship between a person and their state represented using the ISO 3166-1 Alpha-2 code.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:nationality"
        },
        "taxId": {
          "title": "Tax ID",
          "type": "string",
          "description": "The Tax / Fiscal ID of the person, e.g. the TIN in the US or the CIF/NIF in Spain.",
          "meta:status": "deprecated",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:taxId"
        },
        "type": {
          "title": "Type",
          "type": "string",
          "description": "The type of individual in different business contexts like B2C.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:type"
        }
      },
      "meta:referencedFrom": "https://ns.adobe.com/xdm/context/person",
      "meta:xdmField": "xdm:person"
    },
    "personID": {
      "title": "Person ID",
      "description": "Unique identifier of Person/Profile fragment.",
      "type": "string",
      "meta:xdmType": "string",
      "meta:xdmField": "xdm:personID"
    },
    "personalEmail": {
      "title": "Personal Email",
      "description": "A personal email address.",
      "type": "object",
      "meta:xdmType": "object",
      "properties": {
        "address": {
          "title": "Address",
          "type": "string",
          "format": "email",
          "description": "The technical address, for example, 'name@domain.com' as commonly defined in RFC2822 and subsequent standards.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:address",
          "minLength": 1
        },
        "label": {
          "title": "Label",
          "type": "string",
          "description": "Additional display information that maybe available, for example, Microsoft Outlook rich address controls display 'John Smith smithjr@company.uk', 'John Smith' part is data that would be placed in the label.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:label"
        },
        "primary": {
          "title": "Primary",
          "type": "boolean",
          "description": "Primary email indicator. A profile can have only one `primary` email address at a given point of time.",
          "meta:xdmType": "boolean",
          "meta:xdmField": "xdm:primary"
        },
        "status": {
          "title": "Status",
          "type": "string",
          "description": "An indication as to the ability to use the email address.",
          "default": "active",
          "meta:enum": {
            "active": "Active",
            "incomplete": "Incomplete",
            "pending_verification": "Pending verification",
            "blacklisted": "Blacklisted",
            "blocked": "Blocked"
          },
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:status"
        },
        "statusReason": {
          "title": "Status reason",
          "type": "string",
          "description": "A description of the current status.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:statusReason"
        },
        "type": {
          "title": "Type",
          "type": "string",
          "description": "The way the account relates to the person for example 'work' or 'personal'.",
          "meta:enum": {
            "unknown": "Unknown",
            "personal": "Personal",
            "work": "Work",
            "education": "Education"
          },
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:type"
        }
      },
      "meta:referencedFrom": "https://ns.adobe.com/xdm/context/emailaddress",
      "meta:xdmField": "xdm:personalEmail",
      "required": [
        "address"
      ]
    },
    "repositoryCreatedBy": {
      "title": "Created by user identifier",
      "type": "string",
      "description": "User ID of who created the record.",
      "meta:xdmType": "string",
      "meta:xdmField": "xdm:repositoryCreatedBy"
    },
    "repositoryLastModifiedBy": {
      "title": "Modified by user identifier",
      "type": "string",
      "description": "User ID of who last modified the record. At creation time, `modifiedByUser` is set as `createdByUser`.",
      "meta:xdmType": "string",
      "meta:xdmField": "xdm:repositoryLastModifiedBy"
    },
    "shippingAddress": {
      "title": "Shipping Address",
      "description": "Shipping postal address.",
      "type": "object",
      "meta:xdmType": "object",
      "properties": {
        "_id": {
          "title": "Coordinates ID",
          "type": "string",
          "format": "uri-reference",
          "description": "The unique identifier of the coordinates.",
          "meta:xdmType": "string",
          "meta:xdmField": "@id"
        },
        "_repo": {
          "properties": {
            "createDate": {
              "type": "string",
              "format": "date-time",
              "meta:immutable": true,
              "meta:usereditable": false,
              "examples": [
                "2004-10-23T12:00:00-06:00"
              ],
              "description": "The server date and time when the resource was created in the repository, such as when an asset file is first uploaded or a directory is created by the server as the parent of a new asset. The date time property should conform to ISO 8601 standard. An example form is \"2004-10-23T12:00:00-06:00\".",
              "meta:xdmType": "date-time",
              "meta:xdmField": "repo:createDate"
            },
            "modifyDate": {
              "type": "string",
              "format": "date-time",
              "meta:usereditable": false,
              "examples": [
                "2004-10-23T12:00:00-06:00"
              ],
              "description": "The server date and time when the resource was last modified in the repository, such as when a new version of an asset is uploaded or a directory's child resource is added or removed. The date time property should conform to ISO 8601 standard. An example form is \"2004-10-23T12:00:00-06:00\".",
              "meta:xdmType": "date-time",
              "meta:xdmField": "repo:modifyDate"
            }
          },
          "type": "object",
          "meta:xdmType": "object",
          "meta:xedConverted": true
        },
        "_schema": {
          "properties": {
            "description": {
              "title": "Description",
              "type": "string",
              "description": "A description of what the coordinates identify.",
              "meta:xdmType": "string",
              "meta:xdmField": "schema:description"
            },
            "elevation": {
              "title": "Elevation",
              "type": "number",
              "description": "The specific elevation of the defined coordinate. The value conforms to the [WGS84](http://gisgeography.com/wgs84-world-geodetic-system/) datum and is measured in meters.",
              "meta:xdmType": "number",
              "meta:xdmField": "schema:elevation"
            },
            "latitude": {
              "title": "Latitude",
              "type": "number",
              "minimum": -90,
              "maximum": 90,
              "description": "The signed vertical coordinate of a geographic point.",
              "meta:xdmType": "number",
              "meta:xdmField": "schema:latitude"
            },
            "longitude": {
              "title": "Longitude",
              "type": "number",
              "minimum": -180,
              "maximum": 180,
              "description": "The signed horizontal coordinate of a geographic point.",
              "meta:xdmType": "number",
              "meta:xdmField": "schema:longitude"
            }
          },
          "type": "object",
          "meta:xdmType": "object",
          "meta:xedConverted": true
        },
        "city": {
          "title": "City",
          "type": "string",
          "description": "The name of the city.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:city"
        },
        "country": {
          "title": "Country",
          "type": "string",
          "description": "The name of the government-administered territory. Other than `xdm:countryCode`, this is a free-form field that can have the country name in any language.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:country"
        },
        "countryCode": {
          "title": "Country code",
          "type": "string",
          "pattern": "^[A-Z]{2}$",
          "description": "The two-character [ISO 3166-1 alpha-2](https://datahub.io/core/country-list) code for the country.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:countryCode"
        },
        "createdByBatchID": {
          "title": "Created by batch identifier",
          "type": "string",
          "format": "uri-reference",
          "description": "The dataset files in Catalog which has been originating the creation of the record.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:createdByBatchID"
        },
        "dmaID": {
          "title": "Designated market area",
          "type": "integer",
          "description": "The Nielsen media research designated market area.",
          "meta:xdmType": "int",
          "meta:xdmField": "xdm:dmaID"
        },
        "label": {
          "title": "Label",
          "type": "string",
          "description": "Free form name of the address.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:label"
        },
        "lastVerifiedDate": {
          "title": "Last verified date",
          "type": "string",
          "format": "date",
          "description": "The date that the address was last verified as still associated to the person.",
          "meta:xdmType": "date",
          "meta:xdmField": "xdm:lastVerifiedDate"
        },
        "modifiedByBatchID": {
          "title": "Modified by batch identifier",
          "type": "string",
          "format": "uri-reference",
          "description": "The last dataset files in Catalog which has modified the record. At creation time, `modifiedByBatchID` is set as `createdByBatchID`.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:modifiedByBatchID"
        },
        "msaID": {
          "title": "Metropolitan statistical area",
          "type": "integer",
          "description": "The metropolitan statistical area in the United States where the observation occurred.",
          "meta:xdmType": "int",
          "meta:xdmField": "xdm:msaID"
        },
        "postOfficeBox": {
          "title": "Post office box",
          "type": "string",
          "description": "Post office box of the address.",
          "maxLength": 20,
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:postOfficeBox"
        },
        "postalCode": {
          "title": "Postal code",
          "type": "string",
          "description": "The postal code of the location. Postal codes are not available for all countries. In some countries, this will only contain part of the postal code.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:postalCode"
        },
        "primary": {
          "title": "Primary",
          "type": "boolean",
          "description": "Primary address indicator. A profile can have only one `primary` address at a given point of time.",
          "meta:xdmType": "boolean",
          "meta:xdmField": "xdm:primary"
        },
        "region": {
          "title": "Region",
          "type": "string",
          "description": "The region, county, or district portion of the address.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:region"
        },
        "repositoryCreatedBy": {
          "title": "Created by user identifier",
          "type": "string",
          "description": "User ID of who created the record.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:repositoryCreatedBy"
        },
        "repositoryLastModifiedBy": {
          "title": "Modified by user identifier",
          "type": "string",
          "description": "User ID of who last modified the record. At creation time, `modifiedByUser` is set as `createdByUser`.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:repositoryLastModifiedBy"
        },
        "state": {
          "title": "State",
          "type": "string",
          "description": "The name of the State. This is a free-form field.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:state"
        },
        "stateProvince": {
          "title": "State or province",
          "type": "string",
          "description": "The state, or province portion of the observation. The format follows the [ISO 3166-2 (country and subdivision)][http://www.unece.org/cefact/locode/subdivisions.html] standard.",
          "examples": [
            "US-CA",
            "DE-BB",
            "JP-13"
          ],
          "pattern": "([A-Z]{2}-[A-Z0-9]{1,3}|)",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:stateProvince"
        },
        "status": {
          "title": "Status",
          "type": "string",
          "description": "An indication as to the ability to use the address.",
          "default": "active",
          "meta:enum": {
            "active": "Active",
            "incomplete": "Incomplete",
            "pending_verification": "Pending verification",
            "blacklisted": "Blacklisted",
            "blocked": "Blocked"
          },
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:status"
        },
        "statusReason": {
          "title": "Status reason",
          "type": "string",
          "description": "A description of the current status.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:statusReason"
        },
        "street1": {
          "title": "Street 1",
          "type": "string",
          "description": "Primary street level information, apartment number, street number, and street name.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:street1"
        },
        "street2": {
          "title": "Street 2",
          "type": "string",
          "description": "Optional street information second line.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:street2"
        },
        "street3": {
          "title": "Street 3",
          "type": "string",
          "description": "Optional street information third line.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:street3"
        },
        "street4": {
          "title": "Street 4",
          "type": "string",
          "description": "Optional street information fourth line.",
          "meta:xdmType": "string",
          "meta:xdmField": "xdm:street4"
        }
      },
      "meta:referencedFrom": "https://ns.adobe.com/xdm/common/address",
      "meta:xdmField": "xdm:shippingAddress"
    }
  },
  "required": [
    "personalEmail"
  ],
  "imsOrg": "{ORG_ID}",
  "meta:extensible": false,
  "meta:abstract": false,
  "meta:extends": [
    "https://ns.adobe.com/xdm/context/profile-person-details",
    "https://ns.adobe.com/xdm/mixins/profile/profile-loyalty-details",
    "https://ns.adobe.com/xdm/context/profile-personal-details",
    "https://ns.adobe.com/xdm/common/auditable",
    "https://ns.adobe.com/xdm/data/record",
    "https://ns.adobe.com/xdm/context/profile",
    "https://ns.adobe.com/{TENANT_ID}/mixins/9068fd4ea2abf813f4fd2fc9c8b413ae453ff0efc7636691"
  ],
  "meta:xdmType": "object",
  "meta:registryMetadata": {
    "repo:createdDate": 1673310304048,
    "repo:lastModifiedDate": 1673380280074,
    "xdm:createdClientId": "{CLIENT_ID}",
    "xdm:lastModifiedClientId": "{CLIENT_ID}",
    "xdm:createdUserId": "{USER_ID}",
    "xdm:lastModifiedUserId": "{USER_ID}",
    "eTag": "c590ccc7a293040d85c2b7d93276480ef4b4aa9a4fcd6991f50fbb47f58bced2",
    "meta:globalLibVersion": "1.38.2"
  },
  "meta:class": "https://ns.adobe.com/xdm/context/profile",
  "meta:containerId": "tenant",
  "meta:sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
  "meta:sandboxType": "production",
  "meta:tenantNamespace": "_{TENANT_ID}",
  "meta:immutableTags": [
    "union"
  ],
  "meta:allFieldAccess": true
}
```

+++
