---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 記述子
topic: developer guide
translation-type: tm+mt
source-git-commit: 1b398e479137a12bcfc3208d37472aae3d6721e1
workflow-type: tm+mt
source-wordcount: '1481'
ht-degree: 81%

---


# 記述子

スキーマは、データエンティティの静的表示を定義しますが、これらのスキーマ（データセットなど）に基づくデータが相互にどのように関連付けられるかに関する具体的な詳細は提供しません。Adobe Experience Platform では、記述子を使用して、これらの関係や、記述子に関するその他の解釈的なメタスキーマを記述できます。

スキーマ記述子はテナントレベルのメタデータです。つまり、スキーマ記述子は IMS 組織に固有で、すべての記述子操作はテナントコンテナで行われます。

各スキーマには、1 つ以上のスキーマ記述子エンティティを適用できます。各スキーマ記述子エンティティは、記述子 `@type` と、それが適用される `sourceSchema` を含みます。適用されると、これらの記述子はスキーマを使用して作成されたすべてのデータセットに適用されます。

このドキュメントは、記述子の API 呼び出しの例と、使用可能な記述子の完全なリストと、各型の定義に必要なフィールドを提供します。

>[!NOTE]
>
>Descriptors require unique Accept headers that replace `xed` with `xdm`, but otherwise look very similar to Accept headers used elsewhere in the [!DNL Schema Registry]. 以下の呼び出し例には、適切な Accept ヘッダーが含まれていますが、正しいヘッダーが使用されていることを確認するために、十分に注意が必要です。

## リスト記述子

単一の GET リクエストを使用して、組織で定義されたすべてのリストの記述子を返すことができます。

**API 形式**

```http
GET /tenant/descriptors
```

**リクエスト**

```SHELL
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Accept: application/vnd.adobe.xdm-link+json'
```

応答の形式は、リクエストで送信される Accept ヘッダーに依存します。Notice that the `/descriptors` endpoint uses Accept headers that are different than all other endpoints in the [!DNL Schema Registry] API.

記述子 Accept ヘッダーは、`xed` を `xdm` で置き換え、記述子に固有の `link` オプションを指定します。

| Accept | 説明 |
| -------|------------ |
| `application/vnd.adobe.xdm-id+json` | 記述子 ID の配列を返します。 |
| `application/vnd.adobe.xdm-link+json` | 記述子 API パスの配列を返します。 |
| `application/vnd.adobe.xdm+json` | 拡張された記述子オブジェクトの配列を返します。 |

**応答** 

応答には、定義された記述子を持つ各記述子タイプの配列が含まれます。つまり、ある種の `@type` 記述子が定義されていない場合、その記述子型の空の配列は返されません。

`link` Accept ヘッダーを使用する場合、各記述子は `/{CONTAINER}/descriptors/{DESCRIPTOR_ID}` 形式を持つ配列項目として示されます。

```JSON
{
  "xdm:alternateDisplayInfo": [
    "/tenant/descriptors/85dc1bc8b91516ac41163365318e38a9f1e4f351",
    "/tenant/descriptors/49bd5abb5a1310ee80ebc1848eb508d383a462cf",
    "/tenant/descriptors/b3b3e548f1c653326bcf5459ceac4140fc0b9e08"
  ],
  "xdm:descriptorIdentity": [
    "/tenant/descriptors/f7a4bc25429496c4740f8f9a7a49ba96862c5379"
  ],
  "xdm:descriptorOneToOne": [
    "/tenant/descriptors/cb509fd6f8ab6304e346905441a34b58a0cd481a"
  ]
}
```

## 記述子の検索

特定の記述子の詳細を表示したい場合は、個々の記述子をその `@id` を使って検索（GET）できます。

**API 形式**

```http
GET /tenant/descriptors/{DESCRIPTOR_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DESCRIPTOR_ID}` | 参照する記述子の `@id`。 |

**リクエスト**

記述子はバージョン管理されないので、参照リクエストに Accept ヘッダーは不要です。

```SHELL
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors/f3a1dfa38a4871cf4442a33074c1f9406a593407 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、記述子の詳細（`@type` および `sourceSchema` を含む）と、記述子の種類に応じて変化する追加情報を返します。返される `@id` は、リクエストで指定された記述子 `@id` と一致する必要があります。

```JSON
{
  "@type": "xdm:descriptorIdentity",
  "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/fbc52b243d04b5d4f41eaa72a8ba58be",
  "xdm:sourceVersion": 1,
  "xdm:sourceProperty": "/personalEmail/address",
  "xdm:namespace": "Email",
  "xdm:property": "xdm:code",
  "xdm:isPrimary": false,
  "createdUser": "{CREATED_USER}",
  "imsOrg": "{IMS_ORG}",
  "createdClient": "{CREATED_CLIENT}",
  "updatedUser": "{UPDATED_USER}",
  "created": 1548899346989,
  "updated": 1548899346989,
  "meta:containerId": "tenant",
  "@id": "f3a1dfa38a4871cf4442a33074c1f9406a593407"
}
```

## 記述子の作成

The [!DNL Schema Registry] allows you to define several different descriptor types. 各記述子タイプは、POST リクエストで送信する固有のフィールドを必要とします。記述子の完全なリストと、それらを定義するために必要なフィールドは、[記述子の定義](#defining-descriptors)に関する付録の節に記載されています。

**API 形式**

```http
POST /tenant/descriptors
```

**リクエスト**

次のリクエストでは、スキーマ例の「email address」フィールドに ID 記述子を定義します。This tells [!DNL Experience Platform] to use the email address as an identifier to help stitch together information about the individual.

```SHELL
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '
      {
        "@type": "xdm:descriptorIdentity",
        "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/fbc52b243d04b5d4f41eaa72a8ba58be",
        "xdm:sourceVersion": 1,
        "xdm:sourceProperty": "/personalEmail/address",
        "xdm:namespace": "Email",
        "xdm:property": "xdm:code",
        "xdm:isPrimary": false
      }'
```

**応答** 

正常な応答は、新しく作成された記述子の詳細（`@id` を含む）と共に HTTP ステータス 201（作成済み）を返します。The `@id` is a read-only field assigned by the [!DNL Schema Registry] and used for referencing the descriptor in the API.

```JSON
{
  "@type": "xdm:descriptorIdentity",
  "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/fbc52b243d04b5d4f41eaa72a8ba58be",
  "xdm:sourceVersion": 1,
  "xdm:sourceProperty": "/personalEmail/address",
  "xdm:namespace": "Email",
  "xdm:property": "xdm:code",
  "xdm:isPrimary": false,
  "meta:containerId": "tenant",
  "@id": "f3a1dfa38a4871cf4442a33074c1f9406a593407"
}
```

## 記述子の更新

記述子を更新するには、リクエストパスで、更新する記述子の `@id` に対する PUT リクエストを作成します。

**API 形式**

```http
PUT /tenant/descriptors/{DESCRIPTOR_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DESCRIPTOR_ID}` | 更新する記述子の `@id`。 |

**リクエスト**

このリクエストは基本的に記述子を&#x200B;_書き換え_&#x200B;るので、リクエスト本体には、その型の記述子を定義するために必要なすべてのフィールドが含まれている必要があります。つまり、記述子を更新（PUT）するリクエストペイロードは、同じタイプの記述子を作成（POST）するペイロードと同じです。

この例では、ID 記述子は別の `xdm:sourceProperty`（「携帯電話」）を参照するように更新され、`xdm:namespace` を「Phone」に変更しています。

```SHELL
curl -X PUT \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors/f3a1dfa38a4871cf4442a33074c1f9406a593407 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "@type": "xdm:descriptorIdentity",
        "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/fbc52b243d04b5d4f41eaa72a8ba58be",
        "xdm:sourceVersion": 1,
        "xdm:sourceProperty": "/mobilePhone/number",
        "xdm:namespace": "Phone",
        "xdm:property": "xdm:code",
        "xdm:isPrimary": false
      }'
```

`xdm:namespace` および `xdm:property` プロパティと、そのアクセス方法などに関する詳細は、付録の[記述子の定義](#defining-descriptors)に関する節に記載されています。

**応答** 

正常な応答は、更新された記述子の `@id`（リクエストで送信された `@id` と一致する必要がある）と共に、HTTP ステータス 201（作成済み）を返します。

```JSON
{
    "@id": "f3a1dfa38a4871cf4442a33074c1f9406a593407"
}
```

表示に対して検索（GET）リクエストを実行すると、PUT リクエストで送信された変更を反映するようにフィールドが更新されたことが記述子に示されます。

## 記述子の削除

Occasionally you may need to remove a descriptor that you have defined from the [!DNL Schema Registry]. これは、削除する記述子の `@id` を参照する DELETE リクエストを作成することで行われます。

**API 形式**

```http
DELETE /tenant/descriptors/{DESCRIPTOR_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DESCRIPTOR_ID}` | 削除する記述子の `@id`。 |

**リクエスト**

記述子を削除する場合、Accept ヘッダーは必要ありません。

```SHELL
curl -X DELETE \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors/ca921946fb5281cbdb8ba5e07087486ce531a1f2  \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、空白の本文とともに HTTP ステータス 204（コンテンツなし）を返します。

記述子が削除されたことを確認するには、記述子 `@id` に対して検索リクエストを実行できます。The response returns HTTP status 404 (Not Found) because the descriptor has been removed from the [!DNL Schema Registry].

## 付録

The following section provides additional information regarding working with descriptors in the [!DNL Schema Registry] API.

### 記述子の定義

以下の節では、使用可能な記述子の型の概要を示します。各型の記述子を定義するための必須フィールドも含まれます。

#### ID 記述子

An identity descriptor signals that the &quot;[!UICONTROL sourceProperty]&quot; of the &quot;[!UICONTROL sourceSchema]&quot; is an [!DNL Identity] field as described by [Adobe Experience Platform Identity Service](../../identity-service/home.md).

```json
{
  "@type": "xdm:descriptorIdentity",
  "xdm:sourceSchema":
    "https://ns.adobe.com/{TENANT_ID}/schemas/fbc52b243d04b5d4f41eaa72a8ba58be",
  "xdm:sourceVersion": 1,
  "xdm:sourceProperty": "/personalEmail/address",
  "xdm:namespace": "Email",
  "xdm:property": "xdm:code",
  "xdm:isPrimary": false
}
```

| プロパティ | 説明 |
| --- | --- |
| `@type` | 定義する記述子のタイプ。 |
| `xdm:sourceSchema` | 記述子を定義するスキーマの `$id` URI。 |
| `xdm:sourceVersion` | ソーススキーマのメジャーバージョン。 |
| `xdm:sourceProperty` | ID となる特定のプロパティへのパス。パスは、「/」で始まる必要がありますが、「/」で終わらない必要があります。パスに「プロパティ」を含めてはいけません（例：「/properties/personalEmail/properties/address」の代わりに「/personalEmail/address」を使用）。 |
| `xdm:namespace` | ID 名前空間の `id` または `code`。A list of namespaces can be found using the [[!DNL Identity Service API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/id-service-api.yaml). |
| `xdm:property` | `xdm:id` または `xdm:code`（使用されている `xdm:namespace` に応じる）。 |
| `xdm:isPrimary` | オプションのブール値。true の場合、フィールドがプライマリ ID であることを示します。スキーマには、1 つのプライマリ ID のみを含めることができます。 |

#### わかりやすい名前記述子

Friendly name descriptors allow a user to modify the `title`, `description`, and `meta:enum` values of the core library schema fields. 特に、「eVar」および組織に固有の情報を含むとしてラベル付けする他の「汎用」フィールドを扱う場合に役立ちます。UI は、これらを使用して、わかりやすい名前を表示したり、わかりやすい名前を持つフィールドのみを表示したりできます。

```json
{
  "@type": "xdm:alternateDisplayInfo",
  "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/274f17bc5807ff307a046bab1489fb18",
  "xdm:sourceVersion": 1,
  "xdm:sourceProperty": "/xdm:eventType",
  "xdm:title": {
    "en_us": "Event Type"
  },
  "xdm:description": {
    "en_us": "The type of experience event detected by the system."
  },
  "meta:enum": {
    "click": "Mouse Click",
    "addCart": "Add to Cart",
    "checkout": "Cart Checkout"
  }
}
```

| プロパティ | 説明 |
| --- | --- |
| `@type` | 定義する記述子のタイプ。 |
| `xdm:sourceSchema` | 記述子を定義するスキーマの `$id` URI。 |
| `xdm:sourceVersion` | ソーススキーマのメジャーバージョン。 |
| `xdm:sourceProperty` | ID となる特定のプロパティへのパス。パスは、「/」で始まる必要がありますが、「/」で終わらない必要があります。パスに「プロパティ」を含めてはいけません（例：「/properties/personalEmail/properties/address」の代わりに「/personalEmail/address」を使用）。 |
| `xdm:title` | このフィールドに表示する新しいタイトル（タイトルケースで記述）。 |
| `xdm:description` | オプションで、タイトルに説明を追加できます。 |
| `meta:enum` | に示すフィールド `xdm:sourceProperty` が文字列フィールドの場合、 `meta:enum` UI内のフィールドに推奨される値のリストを [!DNL Experience Platform] 決定します。 定義済みリストを宣言しないこと、またはXDMフィールドに対してデータの検証を行わないことに注意して `meta:enum` ください。<br><br>これは、Adobeが定義するコアXDMフィールドに対してのみ使用する必要があります。 ソースプロパティが組織で定義されたカスタムフィールドの場合は、 `meta:enum` PATCH要求を介してフィールドの [プロパティを直接編集する必要があります](./update-resource.md)。 |

#### 関係記述子

関係記述子は、`sourceProperty` と `destinationProperty` で説明されているプロパティに基づいて、2 つの異なるスキーマ間の関係を記述します。詳しくは、[2 つのスキーマ間の関係の定義](../tutorials/relationship-api.md)に関するチュートリアルを参照してください。

```json
{
  "@type": "xdm:descriptorOneToOne",
  "xdm:sourceSchema":
    "https://ns.adobe.com/{TENANT_ID}/schemas/fbc52b243d04b5d4f41eaa72a8ba58be",
  "xdm:sourceVersion": 1,
  "xdm:sourceProperty": "/parentField/subField",
  "xdm:destinationSchema": 
    "https://ns.adobe.com/{TENANT_ID}/schemas/78bab6346b9c5102b60591e15e75d254",
  "xdm:destinationVersion": 1,
  "xdm:destinationProperty": "/parentField/subField"
}
```

| プロパティ | 説明 |
| --- | --- |
| `@type` | 定義する記述子のタイプ。 |
| `xdm:sourceSchema` | 記述子を定義するスキーマの `$id` URI。 |
| `xdm:sourceVersion` | ソーススキーマのメジャーバージョン。 |
| `xdm:sourceProperty` | 関係を定義するソーススキーマ内のフィールドのパス。「/」で始まる必要がありますが、「/」で終わらない必要があります。パスに「プロパティ」を含めてはいけません（例：「/properties/personalEmail/properties/address」の代わりに「/personalEmail/address」を使用）。。 |
| `xdm:destinationSchema` | この記述子が関係を定義する宛先スキーマの `$id` URI。 |
| `xdm:destinationVersion` | 宛先スキーマのメジャーバージョン。 |
| `xdm:destinationProperty` | 宛先スキーマ内のターゲットフィールドへの最適パス。このプロパティを省略すると、ターゲットフィールドは、対応する参照 ID 記述子（以下を参照）を含むフィールドによって推論されます。 |


#### 参照 ID 記述子

参照ID記述子は、スキーマフィールドの主要なIDへの参照コンテキストを提供し、他のスキーマのフィールドから参照できるようにします。 参照記述子を適用する前に、フィールドに ID 記述子のラベルを付けておく必要があります。

```json
{
  "@type": "xdm:descriptorReferenceIdentity",
  "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/78bab6346b9c5102b60591e15e75d254",
  "xdm:sourceVersion": 1,
  "xdm:sourceProperty": "/parentField/subField",
  "xdm:identityNamespace": "Email"
}
```

| プロパティ | 説明 |
| --- | --- |
| `@type` | 定義する記述子のタイプ。 |
| `xdm:sourceSchema` | 記述子を定義するスキーマの `$id` URI。 |
| `xdm:sourceVersion` | ソーススキーマのメジャーバージョン。 |
| `xdm:sourceProperty` | 記述子を定義するソーススキーマ内のフィールドのパス。「/」で始まる必要がありますが、「/」で終わらない必要があります。パスに「プロパティ」を含めてはいけません（例：「/properties/personalEmail/properties/address」の代わりに「/personalEmail/address」を使用）。。 |
| `xdm:identityNamespace` | ソースプロパティの ID 名前空間コード。 |