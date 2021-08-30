---
keywords: Experience Platform；ホーム；人気のあるトピック；API;API;XDM;XDMシステム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；スキーマレジストリ；スキーマレジストリ；記述子；記述子；ID;ID；わかりやすい名前；ID;ADI；参照；関係；関係
solution: Experience Platform
title: 記述子APIエンドポイント
description: スキーマレジストリAPIの/descriptorsエンドポイントを使用すると、エクスペリエンスアプリケーション内のXDM記述子をプログラムで管理できます。
topic-legacy: developer guide
exl-id: bda1aabd-5e6c-454f-a039-ec22c5d878d2
source-git-commit: f269a7b1584a6e4a0e1820a0c587a647c0c8f7b5
workflow-type: tm+mt
source-wordcount: '1630'
ht-degree: 56%

---

# 記述子エンドポイント

スキーマは、データエンティティの静的表示を定義しますが、これらのスキーマ（データセットなど）に基づくデータが相互にどのように関連付けられるかに関する具体的な詳細は提供しません。Adobe Experience Platform では、記述子を使用して、これらの関係や、記述子に関するその他の解釈的なメタスキーマを記述できます。

スキーマ記述子はテナントレベルのメタデータです。つまり、スキーマ記述子は IMS 組織に固有で、すべての記述子操作はテナントコンテナで行われます。

各スキーマには、1 つ以上のスキーマ記述子エンティティを適用できます。各スキーマ記述子エンティティは、記述子 `@type` と、それが適用される `sourceSchema` を含みます。適用されると、これらの記述子はスキーマを使用して作成されたすべてのデータセットに適用されます。

[!DNL Schema Registry] APIの`/descriptors`エンドポイントを使用すると、エクスペリエンスアプリケーション内の記述子をプログラムで管理できます。

## はじめに

このガイドで使用するエンドポイントは、[[!DNL Schema Registry]  API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/class-registry.yaml) の一部です。続行する前に、[はじめにのガイド](./getting-started.md)を参照して、関連ドキュメントへのリンク、このドキュメントのAPI呼び出し例の読み方、およびExperience PlatformAPIを正しく呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## 記述子のリストの取得 {#list}

`/tenant/descriptors`に対してGETリクエストを実行すると、組織で定義されたすべての記述子をリストできます。

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

応答の形式は、リクエストで送信される`Accept`ヘッダーに応じて異なります。 `/descriptors`エンドポイントは、[!DNL Schema Registry] APIの他のすべてのエンドポイントとは異なる`Accept`ヘッダーを使用します。

>[!IMPORTANT]
>
>記述子には、`xed`を`xdm`に置き換える一意の`Accept`ヘッダーが必要です。また、記述子に固有の`link`オプションも指定します。 以下の呼び出し例には、適切な`Accept`ヘッダーが含まれていますが、記述子を操作する際に正しいヘッダーが使用されていることを確認するために、十分に注意が必要です。

| `Accept` ヘッダー | 説明 |
| -------|------------ |
| `application/vnd.adobe.xdm-id+json` | 記述子 ID の配列を返します。 |
| `application/vnd.adobe.xdm-link+json` | 記述子 API パスの配列を返します。 |
| `application/vnd.adobe.xdm+json` | 拡張された記述子オブジェクトの配列を返します。 |
| `application/vnd.adobe.xdm-v2+json` | この`Accept`ヘッダーは、ページング機能を利用するために使用する必要があります。 |

{style=&quot;table-layout:auto&quot;}

**応答**

応答には、定義された記述子を持つ各記述子タイプの配列が含まれます。つまり、ある種の `@type` 記述子が定義されていない場合、その記述子型の空の配列は返されません。

`link``Accept` ヘッダーを使用する場合、各記述子は  形式を持つ配列項目として示されます。`/{CONTAINER}/descriptors/{DESCRIPTOR_ID}`

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

## 記述子の検索 {#lookup}

特定の記述子の詳細を表示したい場合は、個々の記述子をその `@id` を使って検索（GET）できます。

**API 形式**

```http
GET /tenant/descriptors/{DESCRIPTOR_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DESCRIPTOR_ID}` | 検索する記述子の`@id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、記述子を`@id`値で取得します。 記述子はバージョン管理されないので、参照リクエストに`Accept`ヘッダーは必要ありません。

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

## 記述子の作成 {#create}

`/tenant/descriptors`エンドポイントにPOSTリクエストを送信することで、新しい記述子を作成できます。

>[!IMPORTANT]
>
>[!DNL Schema Registry]を使用すると、複数の異なる記述子型を定義できます。 各記述子タイプは、リクエスト本文で送信する固有のフィールドを必要とします。 記述子の完全なリストと、それらを定義するために必要なフィールドについては、[付録](#defining-descriptors)を参照してください。

**API 形式**

```http
POST /tenant/descriptors
```

**リクエスト**

次のリクエストでは、スキーマ例の「email address」フィールドに ID 記述子を定義します。これにより、[!DNL Experience Platform]は、電子メールアドレスを識別子として使用し、個人に関する情報を組み合わせるのに役立ちます。

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

正常な応答は、新しく作成された記述子の詳細（`@id` を含む）と共に HTTP ステータス 201（作成済み）を返します。`@id`は読み取り専用のフィールドで、[!DNL Schema Registry]によって割り当てられ、APIで記述子を参照するために使用されます。

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

## 記述子の更新 {#put}

記述子を更新するには、記述子リクエストのパスに`@id`を含めます。PUT

**API 形式**

```http
PUT /tenant/descriptors/{DESCRIPTOR_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DESCRIPTOR_ID}` | 更新する記述子の `@id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

このリクエストは基本的に記述子を書き換えるので、リクエスト本体には、その型の記述子を定義するために必要なすべてのフィールドが含まれている必要があります。つまり、記述子を更新(PUT)するリクエストペイロードは、同じ型の記述子](#create)を[作成(POST)するペイロードと同じです。

>[!IMPORTANT]
>
>POSTリクエストを使用して記述子を作成する場合と同様に、各記述子タイプは、PUTリクエストペイロードで送信する固有のフィールドを必要とします。 記述子の完全なリストと、それらを定義するために必要なフィールドについては、[付録](#defining-descriptors)を参照してください。

次の例では、ID記述子を更新して、別の`xdm:sourceProperty`(`mobile phone`)を参照し、`xdm:namespace`を`Phone`に変更します。

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

**応答** 

正常な応答は、更新された記述子の `@id`（リクエストで送信された `@id` と一致する必要がある）と共に、HTTP ステータス 201（作成済み）を返します。

```JSON
{
    "@id": "f3a1dfa38a4871cf4442a33074c1f9406a593407"
}
```

[参照(GET)リクエスト](#lookup)を実行して記述子を表示すると、PUTリクエストで送信された変更を反映するようにフィールドが更新されたことが示されます。

## 記述子の削除 {#delete}

場合によっては、定義した記述子を[!DNL Schema Registry]から削除する必要があります。 これは、削除する記述子の `@id` を参照する DELETE リクエストを作成することで行われます。

**API 形式**

```http
DELETE /tenant/descriptors/{DESCRIPTOR_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DESCRIPTOR_ID}` | 削除する記述子の `@id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

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

記述子が削除されたことを確認するには、記述子`@id`に対して[ルックアップリクエスト](#lookup)を実行します。 記述子が[!DNL Schema Registry]から削除されたので、応答はHTTPステータス404(Not Found)を返します。

## 付録

次の節では、[!DNL Schema Registry] APIでの記述子の操作に関する追加情報を示します。

### 記述子の定義 {#defining-descriptors}

以下の節では、使用可能な記述子の型の概要を示します。各型の記述子を定義するための必須フィールドも含まれます。

#### ID 記述子

ID記述子は、「[!UICONTROL sourceSchema]」の「[!UICONTROL sourceProperty]」が[Adobe Experience Platform IDサービス](../../identity-service/home.md)の説明に従う[!DNL Identity]フィールドであることを通知します。

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
| `xdm:namespace` | ID 名前空間の `id` または `code`。名前空間のリストは、[[!DNL Identity Service API]](https://www.adobe.io/experience-platform-apis/references/identity-service)を使用して見つかります。 |
| `xdm:property` | `xdm:id` または `xdm:code`（使用されている `xdm:namespace` に応じる）。 |
| `xdm:isPrimary` | オプションのブール値。true の場合、フィールドがプライマリ ID であることを示します。スキーマには、1 つのプライマリ ID のみを含めることができます。 |

{style=&quot;table-layout:auto&quot;}

#### わかりやすい名前記述子

わかりやすい名前記述子を使用すると、ユーザーはコアライブラリスキーマフィールドの`title`、`description`および`meta:enum`の値を変更できます。 特に、「eVar」および組織に固有の情報を含むとしてラベル付けする他の「汎用」フィールドを扱う場合に役立ちます。UI は、これらを使用して、わかりやすい名前を表示したり、わかりやすい名前を持つフィールドのみを表示したりできます。

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
| `meta:enum` | `xdm:sourceProperty`が示すフィールドが文字列フィールドの場合、`meta:enum`は[!DNL Experience Platform] UIでそのフィールドに対して推奨される値のリストを決定します。 `meta:enum`は列挙を宣言したり、XDMフィールドのデータ検証を行ったりしないことに注意してください。<br><br>これは、Adobeで定義されたコアXDMフィールドにのみ使用する必要があります。ソースプロパティが組織で定義されたカスタムフィールドの場合は、フィールドの親リソースに対するPATCHリクエストを使用して、フィールドの`meta:enum`プロパティを直接編集する必要があります。 |

{style=&quot;table-layout:auto&quot;}

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

{style=&quot;table-layout:auto&quot;}


#### 参照 ID 記述子

参照ID記述子は、スキーマフィールドのプライマリIDへの参照コンテキストを提供し、他のスキーマのフィールドで参照できるようにします。 参照記述子を適用する前に、フィールドに ID 記述子のラベルを付けておく必要があります。

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
