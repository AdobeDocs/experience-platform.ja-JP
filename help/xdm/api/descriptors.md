---
keywords: Experience Platform；ホーム；人気のトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；スキーマレジストリ；スキーマレジストリ；記述子；記述子；記述子；記述子；記述子；ID;ID；フレンドリ名；代替表示情報；参照；関係；関係
solution: Experience Platform
title: 記述子 API エンドポイント
description: Schema Registry API の/descriptors エンドポイントを使用すると、エクスペリエンスアプリケーション内の XDM 記述子をプログラムで管理できます。
exl-id: bda1aabd-5e6c-454f-a039-ec22c5d878d2
source-git-commit: 44355aa2ddf03b20aca64c6675414b73682bc2b5
workflow-type: tm+mt
source-wordcount: '1919'
ht-degree: 40%

---

# 記述子エンドポイント

スキーマは、データエンティティの静的表示を定義しますが、これらのスキーマ（データセットなど）に基づくデータが相互にどのように関連付けられるかに関する具体的な詳細は提供しません。Adobe Experience Platform では、記述子を使用して、これらの関係や、記述子に関するその他の解釈的なメタスキーマを記述できます。

スキーマ記述子はテナントレベルのメタデータです。つまり、スキーマ記述子は組織に固有で、すべての記述子の操作はテナントコンテナで行われます。

各スキーマには、1 つ以上のスキーマ記述子エンティティを適用できます。各スキーマ記述子エンティティは、記述子 `@type` と、それが適用される `sourceSchema` を含みます。適用されると、これらの記述子はスキーマを使用して作成されたすべてのデータセットに適用されます。

[!DNL Schema Registry] API の `/descriptors` エンドポイントを使用すると、エクスペリエンスアプリケーション内の記述子をプログラムで管理できます。

## はじめに

このガイドで使用するエンドポイントは、[[!DNL Schema Registry] API](https://developer.adobe.com/experience-platform-apis/references/schema-registry/) の一部です。 先に進む前に、[はじめる前に](./getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## 記述子のリストの取得 {#list}

`/tenant/descriptors` に対してGETリクエストを行うことで、組織で定義されているすべての記述子をリストできます。

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Accept: application/vnd.adobe.xdm-link+json'
```

応答の形式は、リクエストで送信される `Accept` ヘッダーによって異なります。 `/descriptors` エンドポイントで使用される `Accept` ヘッダーが、[!DNL Schema Registry] API の他のすべてのエンドポイントとは異なることに注意してください。

>[!IMPORTANT]
>
>記述子には、`xed` を `xdm` に置き換える一意の `Accept` ヘッダーが必要で、記述子に固有の `link` オプションも提供します。 以下の呼び出し例では適切な `Accept` ヘッダーが含まれていますが、記述子を操作する際には正しいヘッダーが使用されるよう、細心の注意を払ってください。

| `Accept` ヘッダー | 説明 |
| -------|------------ |
| `application/vnd.adobe.xdm-id+json` | 記述子 ID の配列を返します。 |
| `application/vnd.adobe.xdm-link+json` | 記述子 API パスの配列を返します。 |
| `application/vnd.adobe.xdm+json` | 拡張された記述子オブジェクトの配列を返します。 |
| `application/vnd.adobe.xdm-v2+json` | ページング機能を使用するには、この `Accept` ヘッダーを使用する必要があります。 |

{style="table-layout:auto"}

**応答** 

応答には、定義された記述子を持つ各記述子タイプの配列が含まれます。つまり、ある種の `@type` 記述子が定義されていない場合、その記述子型の空の配列は返されません。

`link` `Accept` ヘッダーを使用する場合、各記述子は配列項目として `/{CONTAINER}/descriptors/{DESCRIPTOR_ID}` の形式で表示されます

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
| `{DESCRIPTOR_ID}` | 検索する記述子の `@id`。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストは、`@id` 値で記述子を取得します。 記述子はバージョン管理されないので、参照リクエストでは `Accept` ヘッダーは必要ありません。

```SHELL
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors/f3a1dfa38a4871cf4442a33074c1f9406a593407 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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
  "imsOrg": "{ORG_ID}",
  "createdClient": "{CREATED_CLIENT}",
  "updatedUser": "{UPDATED_USER}",
  "created": 1548899346989,
  "updated": 1548899346989,
  "meta:containerId": "tenant",
  "@id": "f3a1dfa38a4871cf4442a33074c1f9406a593407"
}
```

## 記述子の作成 {#create}

`/tenant/descriptors` エンドポイントに対してPOSTリクエストを実行することで、新しい記述子を作成できます。

>[!IMPORTANT]
>
>[!DNL Schema Registry] を使用すると、複数の異なる記述子タイプを定義できます。 記述子タイプごとに、リクエスト本文で送信する専用のフィールドが必要です。 記述子の完全なリストと、記述子の定義に必要なフィールドについては、[ 付録 ](#defining-descriptors) を参照してください。

**API 形式**

```http
POST /tenant/descriptors
```

**リクエスト**

次のリクエストでは、スキーマ例の「email address」フィールドに ID 記述子を定義します。この情報は、個人に関 [!DNL Experience Platform] る情報をつなぎ合わせるのに役立つ識別子として、メールアドレスを使用するようにユーザーに指示します。

```SHELL
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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

正常な応答は、新しく作成された記述子の詳細（`@id` を含む）と共に HTTP ステータス 201（作成済み）を返します。`@id` は、[!DNL Schema Registry] によって割り当てられ、API で記述子を参照するために使用される読み取り専用フィールドです。

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

記述子を更新するには、PUTリクエストのパスに `@id` を含めます。

**API 形式**

```http
PUT /tenant/descriptors/{DESCRIPTOR_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DESCRIPTOR_ID}` | 更新する記述子の `@id`。 |

{style="table-layout:auto"}

**リクエスト**

このリクエストは、基本的に記述子を書き換えるので、リクエスト本文には、そのタイプの記述子を定義するために必要なすべてのフィールドを含める必要があります。 つまり、記述子を更新（PUT）するリクエストペイロードは、同じ種類の記述子を [ 作成（POST](#create) するペイロードと同じです。

>[!IMPORTANT]
>
>POSTリクエストを使用して記述子を作成する場合と同様に、記述子タイプごとに固有のフィールドをPUTリクエストペイロードで送信する必要があります。 記述子の完全なリストと、記述子の定義に必要なフィールドについては、[ 付録 ](#defining-descriptors) を参照してください。

次の例では、別の `xdm:sourceProperty` （`mobile phone`）を参照するように ID 記述子を更新し、`xdm:namespace` を `Phone` に変更します。

```SHELL
curl -X PUT \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors/f3a1dfa38a4871cf4442a33074c1f9406a593407 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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

記述子を表示するために [ 参照（GET）リクエスト ](#lookup) を実行すると、PUTリクエストで送信された変更を反映するようにフィールドが更新されたことを示します。

## 記述子の削除 {#delete}

[!DNL Schema Registry] から定義した記述子を削除する必要が生じる場合があります。 これは、削除したい記述子の `@id` を参照するDELETEリクエストを行うことで行います。

**API 形式**

```http
DELETE /tenant/descriptors/{DESCRIPTOR_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DESCRIPTOR_ID}` | 削除する記述子の `@id`。 |

{style="table-layout:auto"}

**リクエスト**

```SHELL
curl -X DELETE \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors/ca921946fb5281cbdb8ba5e07087486ce531a1f2  \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、HTTP ステータス 204（コンテンツなし）が空白の本文とともに返されます。

記述子が削除されたことを確認するには、記述子 `@id` に対して [ 参照リクエスト ](#lookup) を実行します。 記述子が [!DNL Schema Registry] から削除されたので、応答は HTTP ステータス 404 （見つかりません）を返します。

## 付録

次の節では、[!DNL Schema Registry] API での記述子の操作に関する追加情報を示します。

### 記述子の定義 {#defining-descriptors}

>[!NOTE]
>
>スキーマに適用できる記述子の最大数は 4000 です。

以下の節では、使用可能な記述子の型の概要を示します。各型の記述子を定義するための必須フィールドも含まれます。

>[!IMPORTANT]
>
>テナントの名前空間オブジェクトにラベルを付けることはできません。そのサンドボックスをまたいだすべてのカスタムフィールドにそのラベルが適用されるからです。 代わりに、ラベルを設定する必要があるオブジェクトの下にリーフノードを指定する必要があります。

#### ID 記述子

ID 記述子は、[Adobe Experience Platform ID サービス ] で説明されているように、「[!UICONTROL sourceSchema]」の「[!UICONTROL sourceProperty](../../identity-service/home.md)」が [!DNL Identity] フィールドであることを示します。

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
| `@type` | 定義する記述子のタイプ。 ID 記述子の場合、この値は `xdm:descriptorIdentity` に設定する必要があります。 |
| `xdm:sourceSchema` | 記述子を定義するスキーマの `$id` URI。 |
| `xdm:sourceVersion` | ソーススキーマのメジャーバージョン。 |
| `xdm:sourceProperty` | ID となる特定のプロパティへのパス。パスは、「/」で始まる必要がありますが、「/」で終わらない必要があります。パスに「プロパティ」を含めないでください（例えば、「/properties/personalEmail/properties/address」ではなく「/personalEmail/address」を使用します）。 |
| `xdm:namespace` | ID 名前空間の `id` または `code`。名前空間のリストは、[[!DNL Identity Service API]](https://developer.adobe.com/experience-platform-apis/references/identity-service) を使用して見つけることができます。 |
| `xdm:property` | `xdm:id` または `xdm:code`（使用されている `xdm:namespace` に応じる）。 |
| `xdm:isPrimary` | オプションのブール値。true の場合、フィールドがプライマリ ID であることを示します。スキーマには、1 つのプライマリ ID のみを含めることができます。 |

{style="table-layout:auto"}

#### わかりやすい名前記述子 {#friendly-name}

わかりやすい名前記述子を使用すると、ユーザーは、コアライブラリスキーマフィールドの `title`、`description`、`meta:enum` の値を変更できます。 特に、「eVar」および組織に固有の情報を含むとしてラベル付けする他の「汎用」フィールドを扱う場合に役立ちます。UI は、これらを使用して、わかりやすい名前を表示したり、わかりやすい名前を持つフィールドのみを表示したりできます。

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
  },
  "xdm:excludeMetaEnum": {
    "web.formFilledOut": "Web Form Filled Out",
    "media.ping": "Media ping"
  }
}
```

| プロパティ | 説明 |
| --- | --- |
| `@type` | 定義する記述子のタイプ。 わかりやすい名前記述子の場合、この値は `xdm:alternateDisplayInfo` に設定する必要があります。 |
| `xdm:sourceSchema` | 記述子を定義するスキーマの `$id` URI。 |
| `xdm:sourceVersion` | ソーススキーマのメジャーバージョン。 |
| `xdm:sourceProperty` | 詳細を変更する特定のプロパティへのパス。 パスは、スラッシュ（`/`）で始める必要があり、1 で終わる必要はありません。 パスに `properties` を含めないでください（例えば、`/properties/personalEmail/properties/address` の代わりに `/personalEmail/address` を使用します）。 |
| `xdm:title` | このフィールドに表示する新しいタイトル。タイトルケースで記述されます。 |
| `xdm:description` | オプションで、タイトルに説明を追加できます。 |
| `meta:enum` | `xdm:sourceProperty` で示されるフィールドが文字列フィールドの場合、セグメント化 UI でフィールドの推奨値を追加するために `meta:enum` を使用できます。 XDM フィールドについては、定義済みリストを宣言 `meta:enum` たり、データ検証を行ったりしないことに注意してください。<br><br> これは、Adobeで定義されたコア XDM フィールドにのみ使用する必要があります。 ソースプロパティが組織で定義されたカスタムフィールドの場合は、代わりに、PATCHリクエストを使用してフィールドの `meta:enum` プロパティをフィールドの親リソースに直接編集する必要があります。 |
| `meta:excludeMetaEnum` | `xdm:sourceProperty` で示されるフィールドが、`meta:enum` フィールドの下で提供された既存の推奨値を持つ文字列フィールドである場合、このオブジェクトをフレンドリ名記述子に含めて、これらの値の一部またはすべてをセグメント化から除外できます。 エントリを除外するには、各エントリのキーと値が、フィールドの元の `meta:enum` に含まれる値と一致する必要があります。 |

{style="table-layout:auto"}

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
| `@type` | 定義する記述子のタイプ。 関係記述子の場合、この値は `xdm:descriptorOneToOne` に設定する必要があります。 |
| `xdm:sourceSchema` | 記述子を定義するスキーマの `$id` URI。 |
| `xdm:sourceVersion` | ソーススキーマのメジャーバージョン。 |
| `xdm:sourceProperty` | 関係を定義するソーススキーマ内のフィールドのパス。「/」で始まる必要がありますが、「/」で終わらない必要があります。パスに「プロパティ」を含めてはいけません（例：「/properties/personalEmail/properties/address」の代わりに「/personalEmail/address」を使用）。。 |
| `xdm:destinationSchema` | この記述子が関係を定義する参照スキーマの `$id` URI。 |
| `xdm:destinationVersion` | 参照スキーマのメジャーバージョン。 |
| `xdm:destinationProperty` | 参照スキーマ内のターゲットフィールドへのオプションのパス。 このプロパティを省略すると、ターゲットフィールドは、対応する参照 ID 記述子（以下を参照）を含むフィールドによって推論されます。 |

{style="table-layout:auto"}

#### 参照 ID 記述子

参照 ID 記述子は、スキーマフィールドのプライマリ ID への参照コンテキストを提供し、他のスキーマのフィールドから参照できるようにします。 この記述子を使用して他のスキーマが参照するには、参照スキーマにプライマリ ID フィールドが既に定義されている必要があります。

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
| `@type` | 定義する記述子のタイプ。 参照 ID 記述子の場合、この値は `xdm:descriptorReferenceIdentity` に設定する必要があります。 |
| `xdm:sourceSchema` | 記述子を定義するスキーマの `$id` URI。 |
| `xdm:sourceVersion` | ソーススキーマのメジャーバージョン。 |
| `xdm:sourceProperty` | 参照スキーマを参照するために使用されるソーススキーマ内のフィールドへのパス。 「/」で始まる必要がありますが、「/」で終わらない必要があります。パスに「プロパティ」を含めないでください（例えば、`/properties/personalEmail/properties/address` の代わりに `/personalEmail/address`）。 |
| `xdm:identityNamespace` | ソースプロパティの ID 名前空間コード。 |

{style="table-layout:auto"}

#### 非推奨のフィールド記述子

該当するフィールドに `deprecated` に設定された `meta:status` 属性を追加することで ](../tutorials/field-deprecation-api.md#custom) カスタム XDM リソース内のフィールドを非推奨（廃止予定 [ にすることができます。 ただし、スキーマ内の標準 XDM リソースで提供されるフィールドを非推奨（廃止予定）にする場合は、当該のスキーマに非推奨（廃止予定）のフィールド記述子を割り当てて、同じ効果を得ることができます。 [correct `Accept` ヘッダー ](../tutorials/field-deprecation-api.md#verify-deprecation) を使用すると、API で検索する際に、スキーマで非推奨になっている標準フィールドを表示できます。

```json
{
  "@type": "xdm:descriptorDeprecated",
  "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/c65ddf08cf2d4a2fe94bd06113bf4bc4c855e12a936410d5",
  "xdm:sourceVersion": 1,
  "xdm:sourceProperty": "/faxPhone"
}
```

| プロパティ | 説明 |
| --- | --- |
| `@type` | 記述子のタイプ。フィールド非推奨化記述子の場合は、この値を `xdm:descriptorDeprecated` に設定する必要があります。 |
| `xdm:sourceSchema` | 記述子の適用先となるスキーマの URI `$id`。 |
| `xdm:sourceVersion` | 記述子の適用先となるスキーマのバージョン。 `1` に設定してください。 |
| `xdm:sourceProperty` | 記述子の適用先となるスキーマ内のプロパティへのパス。 記述子を複数のプロパティに適用する場合は、パスのリストを配列の形式（例：`["/firstName", "/lastName"]`）で指定します。 |

{style="table-layout:auto"}
