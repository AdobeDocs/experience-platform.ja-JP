---
keywords: Experience Platform；ホーム；人気のトピック；api;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；スキーマレジストリ；スキーマレジストリ；記述子；記述子；記述子；記述子；ID;ID；フレンドリ名；代替表示情報；参照；関係；関係
solution: Experience Platform
title: 記述子 API エンドポイント
description: Schema Registry API の/descriptors エンドポイントを使用すると、エクスペリエンスアプリケーション内の XDM 記述子をプログラムで管理できます。
exl-id: bda1aabd-5e6c-454f-a039-ec22c5d878d2
source-git-commit: 02a22362b9ecbfc5fd7fcf17dc167309a0ea45d5
workflow-type: tm+mt
source-wordcount: '2888'
ht-degree: 25%

---

# 記述子エンドポイント

スキーマはデータエンティティの構造を定義しますが、これらのスキーマから作成されたデータセットの相関関係は指定しません。 Adobe Experience Platformでは、記述子を使用してこれらの関係を記述し、スキーマに解釈可能なメタデータを追加できます。

記述子は、Adobe Experience Platformのスキーマに適用されるテナントレベルのメタデータオブジェクトです。 ダウンストリームでのデータの検証、結合、解釈の方法に影響を与える構造の関係、キー、行動フィールド（タイムスタンプやバージョン管理など）を定義します。

スキーマには、1 つ以上の記述子を含めることができます。 各記述子は、`@type` と、それが適用される `sourceSchema` を定義します。 記述子は、そのスキーマから作成されたすべてのデータセットに自動的に適用されます。

Adobe Experience Platformでは、記述子は、スキーマに動作規則や構造的意味を追加するメタデータです。
記述子には次のようないくつかのタイプがあります。

- [ID 記述子 ](#identity-descriptor) - フィールドを ID としてマークします
- [プライマリキー記述子 ](#primary-key-descriptor) – 一意性を強制します
- [ 関係記述子 ](#relationship-descriptor) – 外部キー結合を定義します
- [ 代替表示情報記述子 ](#friendly-name) - UI でフィールドの名前を変更できます
- [Version](#version-descriptor) および [timestamp](#timestamp-descriptor) 記述子 – イベントの順序および変更検出を追跡します

`/descriptors` API の [!DNL Schema Registry] エンドポイントを使用すると、エクスペリエンスアプリケーション内の記述子をプログラムで管理できます。

## はじめに

このガイドで使用するエンドポイントは、[[!DNL Schema Registry] API](https://developer.adobe.com/experience-platform-apis/references/schema-registry/) の一部です。 先に進む前に、[はじめる前に](./getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

[!DNL Schema Registry] は、標準の記述子に加えて、**プライマリキー**、**バージョン**、**タイムスタンプ** など、モデルベースのスキーマの記述子タイプをサポートしています。 これらは、一意性の強制、バージョン管理の制御、スキーマレベルでの時系列フィールドの定義を行います。 モデルベースのスキーマに慣れていない場合は、先に進む前に [Data Mirrorの概要 ](../data-mirror/overview.md) および [ モデルベースのスキーマのテクニカルリファレンス ](../schema/model-based.md) を確認してください。

>[!IMPORTANT]
>
>すべての記述子タイプについて詳しくは、[ 付録 ](#defining-descriptors) を参照してください。

## 記述子のリストの取得 {#list}

`/tenant/descriptors` に対してGET リクエストを行うことで、組織で定義されているすべての記述子をリストできます。

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
>記述子には、`Accept` を `xed` に置き換える一意の `xdm` ヘッダーが必要で、記述子に固有の `link` オプションも提供します。 以下の呼び出し例では適切な `Accept` ヘッダーが含まれていますが、記述子を操作する際には正しいヘッダーが使用されるよう、細心の注意を払ってください。

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

特定の記述子の詳細を表示するには、`@id` を使用してGET リクエストを送信します。

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

`/tenant/descriptors` エンドポイントに POST リクエストを実行することで、新しい記述子を作成できます。

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

PUT リクエストのパスに `@id` を含めることで、記述子を更新できます。

**API 形式**

```http
PUT /tenant/descriptors/{DESCRIPTOR_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DESCRIPTOR_ID}` | 更新する記述子の `@id`。 |

{style="table-layout:auto"}

**リクエスト**

このリクエストは、基本的に記述子を書き換えるので、リクエスト本文には、そのタイプの記述子を定義するために必要なすべてのフィールドを含める必要があります。 つまり、記述子を更新（PUT）するリクエストペイロードは、同じタイプの記述子を [ 作成（POST](#create) するペイロードと同じです。

>[!IMPORTANT]
>
>POST リクエストを使用して記述子を作成する場合と同様に、記述子タイプごとに専用のフィールドをPUT リクエストペイロードで送信する必要があります。 記述子の完全なリストと、記述子の定義に必要なフィールドについては、[ 付録 ](#defining-descriptors) を参照してください。

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

記述子を表示するために [ 参照（GET）リクエスト ](#lookup) を実行すると、フィールドが更新され、PUT リクエストで送信された変更が反映されていることを示します。

## 記述子の削除 {#delete}

[!DNL Schema Registry] から定義した記述子を削除する必要が生じる場合があります。 これは、削除する記述子の `@id` を参照するDELETE リクエストを実行することで行います。

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

記述子が削除されたことを確認するには、記述子 [ に対して ](#lookup) 参照リクエスト `@id` を実行します。 記述子が [!DNL Schema Registry] から削除されたので、応答は HTTP ステータス 404 （見つかりません）を返します。

## 付録 {#appendix}

次の節では、[!DNL Schema Registry] API での記述子の操作に関する追加情報を示します。

### 記述子の定義 {#defining-descriptors}

>[!NOTE]
>
>組織のサンドボックスに適用できる記述子の最大数は 4,000 です。

以下の節では、使用可能な記述子の型の概要を示します。各型の記述子を定義するための必須フィールドも含まれます。

>[!IMPORTANT]
>
>テナントの名前空間オブジェクトにラベルを付けることはできません。そのサンドボックスをまたいだすべてのカスタムフィールドにそのラベルが適用されるからです。 代わりに、ラベルを設定する必要があるオブジェクトの下にリーフノードを指定する必要があります。

#### ID 記述子 {#identity-descriptor}

ID 記述子は、[!UICONTROL Experience Platform ID サービス &#x200B;] で説明されているように、「[!UICONTROL sourceSchema]」の「[!DNL Identity]sourceProperty[」が ](../../identity-service/home.md) フィールドであることを示します。

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
| `xdm:sourceProperty` | 詳細を変更する特定のプロパティへのパス。 パスは、スラッシュ（`/`）で始める必要があり、1 で終わる必要はありません。 パスに `properties` を含めないでください（例えば、`/personalEmail/address` の代わりに `/properties/personalEmail/properties/address` を使用します）。 |
| `xdm:title` | このフィールドに表示する新しいタイトル。タイトルケースで記述されます。 |
| `xdm:description` | オプションで、タイトルに説明を追加できます。 |
| `meta:enum` | `xdm:sourceProperty` で示されるフィールドが文字列フィールドの場合、セグメント化 UI でフィールドの推奨値を追加するために `meta:enum` を使用できます。 XDM フィールドについては、定義済みリストを宣言 `meta:enum` たり、データ検証を行ったりしないことに注意してください。<br><br> これは、Adobeで定義されたコア XDM フィールドにのみ使用してください。 ソースプロパティが組織で定義されたカスタムフィールドの場合は、代わりに、PATCH リクエストを使用してフィールドの `meta:enum` プロパティをフィールドの親リソースに直接編集する必要があります。 |
| `meta:excludeMetaEnum` | `xdm:sourceProperty` で示されるフィールドが、`meta:enum` フィールドの下で提供された既存の推奨値を持つ文字列フィールドである場合、このオブジェクトをフレンドリ名記述子に含めて、これらの値の一部またはすべてをセグメント化から除外できます。 エントリを除外するには、各エントリのキーと値が、フィールドの元の `meta:enum` に含まれる値と一致する必要があります。 |

{style="table-layout:auto"}

#### 関係記述子 {#relationship-descriptor}

関係記述子は、`xdm:sourceProperty` と `xdm:destinationProperty` で説明されているプロパティに基づいて、2 つの異なるスキーマ間の関係を記述します。詳しくは、[2 つのスキーマ間の関係の定義](../tutorials/relationship-api.md)に関するチュートリアルを参照してください。

これらのプロパティを使用して、ソースフィールド（外部キー）と宛先フィールド（[ プライマリキー ](#primary-key-descriptor) または候補キー）の関係を宣言します。

>[!TIP]
>
>**外部キー** は、別のスキーマのキーフィールドを参照するソーススキーマのフィールド（`xdm:sourceProperty` で定義）です。 **候補キー** とは、レコードを一意に識別する、宛先スキーマ内の任意のフィールド（またはフィールドのセット）であり、プライマリキーの代わりに使用できます。

この API は、次の 2 つのパターンをサポートしています。

- `xdm:descriptorOneToOne`：標準 1:1 関係。
- `xdm:descriptorRelationship`：新しい作業およびモデルベースのスキーマの一般的なパターン（カーディナリティ、命名および非プライマリキーのターゲットをサポート）。

##### 1 対 1 の関係（標準スキーマ）

既に `xdm:descriptorOneToOne` に依存している既存の標準スキーマ統合を維持する場合は、これを使用します。

```json
{
  "@type": "xdm:descriptorOneToOne",
  "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/{SOURCE_SCHEMA_ID}",
  "xdm:sourceVersion": 1,
  "xdm:sourceProperty": "/parentField/subField",
  "xdm:destinationSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/{DEST_SCHEMA_ID}",
  "xdm:destinationVersion": 1,
  "xdm:destinationProperty": "/parentField/subField"
}
```

次の表に、1 対 1 関係記述子を定義するために必要なフィールドを示します。

| プロパティ | 説明 |
| --- | --- |
| `@type` | 定義する記述子のタイプ。 関係記述子の場合、Real-Time CDP B2B editionにアクセスできない限り、この値は `xdm:descriptorOneToOne` に設定される必要があります。 B2B editionでは、`xdm:descriptorOneToOne` または [`xdm:descriptorRelationship`](#b2b-relationship-descriptor) を使用できます。 |
| `xdm:sourceSchema` | 記述子を定義するスキーマの `$id` URI。 |
| `xdm:sourceVersion` | ソーススキーマのメジャーバージョン。 |
| `xdm:sourceProperty` | 関係を定義するソーススキーマ内のフィールドのパス。「/」で始まる必要があり、「/」で終わる必要はありません。 パスに「プロパティ」を含めてはいけません（例：「/properties/personalEmail/properties/address」の代わりに「/personalEmail/address」を使用）。。 |
| `xdm:destinationSchema` | この記述子が関係を定義する参照スキーマの `$id` URI。 |
| `xdm:destinationVersion` | 参照スキーマのメジャーバージョン。 |
| `xdm:destinationProperty` | （オプション）参照スキーマ内のターゲットフィールドへのパス。 このプロパティを省略すると、ターゲットフィールドは、対応する参照 ID 記述子（以下を参照）を含むフィールドによって推論されます。 |

##### 一般的な関係（モデルベースのスキーマで、新規プロジェクトに推奨）

この記述子は、すべての新規実装と、モデルベースのスキーマに使用します。 関係のカーディナリティ（1 対 1 または多対 1 など）を定義し、関係名を指定して、プライマリキー（非プライマリキー）以外の宛先フィールドにリンクできます。

次の例は、一般的な関係記述子の定義方法を示しています。

**最小限の例：**

この最小限の例には、2 つのスキーマ間の多対 1 の関係を定義するための必須フィールドのみが含まれています。

```json
{
  "@type": "xdm:descriptorRelationship",
  "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/{SOURCE_SCHEMA_ID}",
  "xdm:sourceProperty": "/customer_ref",
  "xdm:sourceVersion": 1,
  "xdm:destinationSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/{DEST_SCHEMA_ID}",
  "xdm:cardinality": "M:1"
}
```

**すべてのオプションフィールドを使用した例：**

この例には、リレーションシップ名、表示タイトル、明示的な非プライマリ キーのリンク先フィールドなど、すべてのオプション フィールドが含まれています。

```json
{
  "@type": "xdm:descriptorRelationship",
  "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/{SOURCE_SCHEMA_ID}",
  "xdm:sourceVersion": 1,
  "xdm:sourceProperty": "/customer_ref",
  "xdm:destinationSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/{DEST_SCHEMA_ID}",
  "xdm:destinationProperty": "/customer_id",
  "xdm:sourceToDestinationName": "CampaignToCustomer",
  "xdm:destinationToSourceName": "CustomerToCampaign",
  "xdm:sourceToDestinationTitle": "Customer campaigns",
  "xdm:destinationToSourceTitle": "Campaign customers",
  "xdm:cardinality": "M:1"
}
```

##### 関係記述子の選択

次のガイドラインを使用して、適用する関係記述子を決定します。

| 状況 | 使用する記述子 |
| --------------------------------------------------------------------- | ----------------------------------------- |
| 新しい作業またはモデルベースのスキーマ | `xdm:descriptorRelationship` |
| 標準スキーマの既存の 1:1 マッピング | `xdm:descriptorOneToOne` でのみサポートされる機能が必要な場合を除き、`xdm:descriptorRelationship` を使用し続けます。 |
| 多対 1 またはオプションのカーディナリティ（`1:1`、`1:0`、`M:1`、`M:0`）が必要 | `xdm:descriptorRelationship` |
| UI/ダウンストリームが読みやすいように関係名またはタイトルが必要です | `xdm:descriptorRelationship` |
| ID ではない宛先ターゲットが必要です | `xdm:descriptorRelationship` |

>[!NOTE]
>
>標準スキーマの既存の `xdm:descriptorOneToOne` 記述子の場合、非プライマリ ID 宛先ターゲット、カスタムネーミング、拡張カーディナリティなどの機能が必要な場合以外は、それらを引き続き使用してください。

##### 機能の比較

次の表では、2 つの記述子タイプの機能を比較しています。

| 機能 | `xdm:descriptorOneToOne` | `xdm:descriptorRelationship` |
| ------------------ | ------------------------ | ------------------------------------------------------------------------ |
| 基数 | 1:1 | 1:1、1:0、M:1、M:0 （情報） |
| 宛先のターゲット | ID/明示的フィールド | デフォルトではプライマリキー、または `xdm:destinationProperty` を使用した非プライマリキー |
| フィールドの命名 | サポートなし | `xdm:sourceToDestinationName`、`xdm:destinationToSourceName` およびタイトル |
| 関係適合 | 制限付き | モデルベースのスキーマのプライマリパターン |

##### 制約と検証

一般的な関係記述子を定義する際は、次の要件と推奨事項に従います。

- モデルベースのスキーマの場合は、ルートレベルのソースフィールド（外部キー）を配置します。 これは、取得に関する現在の技術的な制限で、ベストプラクティスの推奨事項だけではありません。
- ソースおよび宛先フィールドのデータタイプに互換性があることを確認します（数値、日付、ブール値、文字列）。
- カーディナリティは情報目的であることに注意してください。ストレージでは適用されません。 カーディナリティを `<source>:<destination>` 形式で指定します。 指定できる値は、`1:1`、`1:0`、`M:1`、`M:0` です。

#### プライマリキー記述子 {#primary-key-descriptor}

プライマリキー記述子（`xdm:descriptorPrimaryKey`）は、スキーマの 1 つ以上のフィールドに対して一意性と null 以外の制約を適用します。

```json
{
  "@type": "xdm:descriptorPrimaryKey",
  "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
  "xdm:sourceProperty": ["/orderId", "/orderLineId"]
}
```

| プロパティ | 説明 |
| -------------------- | ----------------------------------------------------------------------------- |
| `@type` | `xdm:descriptorPrimaryKey` である必要があります。 |
| `xdm:sourceSchema` | スキ `$id` マの URI。 |
| `xdm:sourceProperty` | プライマリキーフィールドへの JSON ポインター。 複合キーには配列を使用します。 時系列スキーマの場合、イベントレコード間で一意性を確保するために、複合キーにタイムスタンプフィールドを含める必要があります。 |

#### バージョン記述子 {#version-descriptor}

>[!NOTE]
>
>UI スキーマエディターでは、バージョン記述子が「[ !UICOTRNOL  バージョン識別子 ]」として表示されます。

バージョン記述子（`xdm:descriptorVersion`）は、順不同の変更イベントによる競合を検出して防ぐフィールドを指定します。

```json
{
  "@type": "xdm:descriptorVersion",
  "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
  "xdm:sourceProperty": "/versionNumber"
}
```

| プロパティ | 説明 |
| -------------------- | ------------------------------------------------------------- |
| `@type` | `xdm:descriptorVersion` である必要があります。 |
| `xdm:sourceSchema` | スキ `$id` マの URI。 |
| `xdm:sourceProperty` | バージョンフィールドへの JSON ポインター。 `required` とマークする必要があります。 |

#### タイムスタンプ記述子 {#timestamp-descriptor}

>[!NOTE]
>
>UI スキーマエディターでは、タイムスタンプ記述子は「[ !UICOTRNOL  タイムスタンプ識別子 ] として表示されます。

タイムスタンプ記述子（`xdm:descriptorTimestamp`）は、`"meta:behaviorType": "time-series"` を含むスキーマのタイムスタンプとして日時フィールドを指定します。

```json
{
  "@type": "xdm:descriptorTimestamp",
  "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
  "xdm:sourceProperty": "/eventTime"
}
```

| プロパティ | 説明 |
| -------------------- | ------------------------------------------------------------------------------------------ |
| `@type` | `xdm:descriptorTimestamp` である必要があります。 |
| `xdm:sourceSchema` | スキ `$id` マの URI。 |
| `xdm:sourceProperty` | タイムスタンプフィールドへの JSON ポインター。 `required` とマークされ、`date-time` タイプである必要があります。 |

##### B2B 関係記述子 {#B2B-relationship-descriptor}

Real-Time CDP B2B editionでは、スキーマ間の関係を定義する別の方法が導入されており、多対 1 の関係が可能になります。 この新しい関係のタイプは `@type: xdm:descriptorRelationship` である必要があり、ペイロードには `@type: xdm:descriptorOneToOne` の関係よりも多くのフィールドを含める必要があります。 詳しくは、[B2B editionのスキーマ関係の定義 ](../tutorials/relationship-b2b.md) に関するチュートリアルを参照してください。

```json
{
   "@type": "xdm:descriptorRelationship",
   "xdm:sourceSchema" : "https://ns.adobe.com/{TENANT_ID}/schemas/9f2b2f225ac642570a110d8fd70800ac0c0573d52974fa9a",
   "xdm:sourceVersion" : 1,
   "xdm:sourceProperty" : "/person-ref",
   "xdm:destinationSchema" : "https://ns.adobe.com/{TENANT_ID/schemas/628427680e6b09f1f5a8f63ba302ee5ce12afba8de31acd7",
   "xdm:destinationVersion" : 1,
   "xdm:destinationProperty": "/personId",
   "xdm:destinationNamespace" : "People", 
   "xdm:destinationToSourceTitle" : "Opportunity Roles",
   "xdm:sourceToDestinationTitle" : "People",
   "xdm:cardinality": "M:1"
}
```

| プロパティ | 説明 |
| --- | --- |
| `@type` | 定義する記述子のタイプ。 以下のフィールドを使用する場合、値は `xdm:descriptorRelationship` に設定する必要があります。 その他のタイプについて詳しくは、[ 関係記述子 ](#relationship-descriptor) の節を参照してください。 |
| `xdm:sourceSchema` | 記述子を定義するスキーマの `$id` URI。 |
| `xdm:sourceVersion` | ソーススキーマのメジャーバージョン。 |
| `xdm:sourceProperty` | 関係を定義するソーススキーマ内のフィールドのパス。「/」で始まる必要があり、「/」で終わる必要はありません。 パスに「プロパティ」を含めてはいけません（例：「/properties/personalEmail/properties/address」の代わりに「/personalEmail/address」を使用）。。 |
| `xdm:destinationSchema` | この記述子が関係を定義する参照スキーマの `$id` URI。 |
| `xdm:destinationVersion` | 参照スキーマのメジャーバージョン。 |
| `xdm:destinationProperty` | （オプション）参照スキーマ内のターゲットフィールドへのパス。 これは、スキーマのプライマリ ID、または `xdm:sourceProperty` と互換性のあるデータタイプを持つ別のフィールドに解決される必要があります。 省略すると、関係が期待どおりに機能しない可能性があります。 |
| `xdm:destinationNamespace` | 参照スキーマからのプライマリ ID の名前空間。 |
| `xdm:destinationToSourceTitle` | 参照スキーマからソーススキーマへの関係の表示名。 |
| `xdm:sourceToDestinationTitle` | ソーススキーマから参照スキーマへの関係の表示名。 |
| `xdm:cardinality` | スキーマ間の結合関係。 この値は、多対 1 の関係を参照して、`M:1` に設定する必要があります。 |

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
| `xdm:sourceProperty` | 参照スキーマを参照するために使用されるソーススキーマ内のフィールドへのパス。 「/」で始まる必要がありますが、「/」で終わらない必要があります。パスに「プロパティ」を含めないでください（例えば、`/personalEmail/address` の代わりに `/properties/personalEmail/properties/address`）。 |
| `xdm:identityNamespace` | ソースプロパティの ID 名前空間コード。 |

{style="table-layout:auto"}

#### 非推奨のフィールド記述子

該当するフィールドに [ に設定された ](../tutorials/field-deprecation-api.md#custom) 属性を追加することで `meta:status` カスタム XDM リソース内のフィールドを非推奨（廃止予定 `deprecated` にすることができます。 ただし、スキーマ内の標準 XDM リソースで提供されるフィールドを非推奨（廃止予定）にする場合は、当該のスキーマに非推奨（廃止予定）のフィールド記述子を割り当てて、同じ効果を得ることができます。 [correct `Accept` ヘッダー ](../tutorials/field-deprecation-api.md#verify-deprecation) を使用すると、API で検索する際に、スキーマで非推奨になっている標準フィールドを表示できます。

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
