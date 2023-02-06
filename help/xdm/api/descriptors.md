---
keywords: Experience Platform；ホーム；人気のトピック；API;XDM;XDM;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；スキーマレジストリ；スキーマレジストリ；記述子；記述子；ID；わかりやすい名前；ID;Frendlydisplayinfo；リファレンス；リレーションシップ；リレーションシップ
solution: Experience Platform
title: 記述子 API エンドポイント
description: Schema Registry API の/descriptors エンドポイントを使用すると、エクスペリエンスアプリケーション内の XDM 記述子をプログラムで管理できます。
exl-id: bda1aabd-5e6c-454f-a039-ec22c5d878d2
source-git-commit: a3140d5216857ef41c885bbad8c69d91493b619d
workflow-type: tm+mt
source-wordcount: '1900'
ht-degree: 45%

---

# 記述子エンドポイント

スキーマは、データエンティティの静的表示を定義しますが、これらのスキーマ（データセットなど）に基づくデータが相互にどのように関連付けられるかに関する具体的な詳細は提供しません。Adobe Experience Platform では、記述子を使用して、これらの関係や、記述子に関するその他の解釈的なメタスキーマを記述できます。

スキーマ記述子はテナントレベルのメタデータです。つまり、スキーマ記述子は IMS 組織に固有で、すべての記述子操作はテナントコンテナで行われます。

各スキーマには、1 つ以上のスキーマ記述子エンティティを適用できます。各スキーマ記述子エンティティは、記述子 `@type` と、それが適用される `sourceSchema` を含みます。適用されると、これらの記述子はスキーマを使用して作成されたすべてのデータセットに適用されます。

この `/descriptors` エンドポイント [!DNL Schema Registry] API を使用すると、エクスペリエンスアプリケーション内の記述子をプログラムで管理できます。

## はじめに

このガイドで使用するエンドポイントは、[[!DNL Schema Registry]  API](https://developer.adobe.com/experience-platform-apis/references/schema-registry/) の一部です。先に進む前に、[はじめる前に](./getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## 記述子のリストの取得 {#list}

に対してGETリクエストを実行すると、組織で定義されたすべての記述子をリストできます。 `/tenant/descriptors`.

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

応答の形式は、 `Accept` ヘッダーがリクエストで送信されました。 この `/descriptors` エンドポイントの使用 `Accept` 他のすべてのエンドポイントとは異なるヘッダー [!DNL Schema Registry] API

>[!IMPORTANT]
>
>記述子には一意のが必要です `Accept` 置き換えるヘッダー `xed` と `xdm`、および `link` 記述子に固有のオプション。 適切な `Accept` 以下の呼び出し例にヘッダーが含まれていますが、記述子を操作する際に正しいヘッダーが使用されていることを確認するには、十分に注意が必要です。

| `Accept` ヘッダー | 説明 |
| -------|------------ |
| `application/vnd.adobe.xdm-id+json` | 記述子 ID の配列を返します。 |
| `application/vnd.adobe.xdm-link+json` | 記述子 API パスの配列を返します。 |
| `application/vnd.adobe.xdm+json` | 拡張された記述子オブジェクトの配列を返します。 |
| `application/vnd.adobe.xdm-v2+json` | この `Accept` ページング機能を利用するには、ヘッダーを使用する必要があります。 |

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
| `{DESCRIPTOR_ID}` | この `@id` 検索する記述子の名前を指定します。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、記述子をその `@id` の値です。 記述子はバージョン管理されないので、 `Accept` 参照リクエストにはヘッダーが必要です。

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

新しい記述子を作成するには、 `/tenant/descriptors` endpoint.

>[!IMPORTANT]
>
>この [!DNL Schema Registry] では、複数の異なる記述子タイプを定義できます。 各記述子タイプは、リクエスト本文で送信する固有のフィールドを必要とします。 詳しくは、 [付録](#defining-descriptors) を参照してください。

**API 形式**

```http
POST /tenant/descriptors
```

**リクエスト**

次のリクエストでは、スキーマ例の「email address」フィールドに ID 記述子を定義します。これは [!DNL Experience Platform] 電子メールアドレスを識別子として使用し、個人に関する情報を組み合わせるのに役立てます。

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

正常な応答は、新しく作成された記述子の詳細（`@id` を含む）と共に HTTP ステータス 201（作成済み）を返します。この `@id` は、 [!DNL Schema Registry] API で記述子を参照するために使用されます。

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

記述子を更新するには、記述子を含めます `@id` を返します。PUTリクエストのパス。

**API 形式**

```http
PUT /tenant/descriptors/{DESCRIPTOR_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DESCRIPTOR_ID}` | 更新する記述子の `@id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

このリクエストは基本的に記述子を書き換えるので、リクエスト本体には、その型の記述子を定義するために必要なすべてのフィールドが含まれている必要があります。つまり、記述子を更新 (PUT) するリクエストペイロードは、 [記述子の作成 (POST)](#create) 同じタイプの

>[!IMPORTANT]
>
>POSTリクエストを使用して記述子を作成する場合と同様に、各記述子タイプは、記述子リクエストペイロードで送信する固有のフィールドをPUTリクエストに必要とします。 詳しくは、 [付録](#defining-descriptors) を参照してください。

次の例では、別の `xdm:sourceProperty` (`mobile phone`) をクリックし、 `xdm:namespace` から `Phone`.

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

の実行 [参照 (GET) リクエスト](#lookup) を表示すると、記述子に、PUTリクエストで送信された変更を反映するようにフィールドが更新されたことが示されます。

## 記述子の削除 {#delete}

定義した記述子を [!DNL Schema Registry]. これは、削除する記述子の `@id` を参照する DELETE リクエストを作成することで行われます。

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、HTTP ステータス 204（コンテンツなし）が空白の本文とともに返されます。

記述子が削除されたことを確認するには、 [ルックアップリクエスト](#lookup) 記述子に対して `@id`. 記述子が [!DNL Schema Registry].

## 付録

次の節では、 [!DNL Schema Registry] API

### 記述子の定義 {#defining-descriptors}

以下の節では、使用可能な記述子の型の概要を示します。各型の記述子を定義するための必須フィールドも含まれます。

#### ID 記述子

ID 記述子は、[!UICONTROL sourceProperty]&quot;[!UICONTROL sourceSchema]&quot;は [!DNL Identity] 次に示すフィールド [Adobe Experience Platform Identity Service](../../identity-service/home.md).

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
| `@type` | 定義する記述子のタイプ。ID 記述子の場合、この値はに設定する必要があります。 `xdm:descriptorIdentity`. |
| `xdm:sourceSchema` | 記述子を定義するスキーマの `$id` URI。 |
| `xdm:sourceVersion` | ソーススキーマのメジャーバージョン。 |
| `xdm:sourceProperty` | ID となる特定のプロパティへのパス。パスは、「/」で始まる必要がありますが、「/」で終わらない必要があります。パスに「プロパティ」を含めてはいけません（例：「/properties/personalEmail/properties/address」の代わりに「/personalEmail/address」を使用）。 |
| `xdm:namespace` | ID 名前空間の `id` または `code`。名前空間のリストは、 [[!DNL Identity Service API]](https://www.adobe.io/experience-platform-apis/references/identity-service). |
| `xdm:property` | `xdm:id` または `xdm:code`（使用されている `xdm:namespace` に応じる）。 |
| `xdm:isPrimary` | オプションのブール値。true の場合、フィールドがプライマリ ID であることを示します。スキーマには、1 つのプライマリ ID のみを含めることができます。 |

{style=&quot;table-layout:auto&quot;}

#### わかりやすい名前記述子 {#friendly-name}

わかりやすい名前記述子を使用すると、ユーザーは `title`, `description`、および `meta:enum` コアライブラリスキーマフィールドの値。 特に、「eVar」および組織に固有の情報を含むとしてラベル付けする他の「汎用」フィールドを扱う場合に役立ちます。UI は、これらを使用して、わかりやすい名前を表示したり、わかりやすい名前を持つフィールドのみを表示したりできます。

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
| `@type` | 定義する記述子のタイプ。わかりやすい名前記述子の場合、この値をに設定する必要があります。 `xdm:alternateDisplayInfo`. |
| `xdm:sourceSchema` | 記述子を定義するスキーマの `$id` URI。 |
| `xdm:sourceVersion` | ソーススキーマのメジャーバージョン。 |
| `xdm:sourceProperty` | 詳細を変更する特定のプロパティへのパス。パスはスラッシュ (`/`) で終わらず、1 で終わる。 次を含めない `properties` パス内 ( 例： `/personalEmail/address` の代わりに `/properties/personalEmail/properties/address`) をクリックします。 |
| `xdm:title` | このフィールドに表示する新しいタイトル（タイトルケースで記述）。 |
| `xdm:description` | オプションで、タイトルに説明を追加できます。 |
| `meta:enum` | 次に示すフィールド `xdm:sourceProperty` は文字列フィールドです。 `meta:enum` を使用して、セグメント化 UI の「 」フィールドに推奨値を追加できます。 注意すべき点は `meta:enum` は列挙を宣言しないか、XDM フィールドのデータ検証を提供しません。<br><br>これは、Adobeで定義されたコア XDM フィールドにのみ使用する必要があります。 ソースプロパティが組織で定義されたカスタムフィールドの場合、代わりに、フィールドの `meta:enum` プロパティを直接、PATCHリクエストを介してフィールドの親リソースに渡します。 |
| `meta:excludeMetaEnum` | 次に示すフィールド `xdm:sourceProperty` は、 `meta:enum` フィールドに値を入力する場合、このオブジェクトをわかりやすい名前記述子に含めて、これらの値の一部またはすべてをセグメント化から除外できます。 各エントリのキーと値は、元のエントリに含まれるキーと値と一致する必要があります `meta:enum` 」フィールドの値を指定します。 |

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
| `@type` | 定義する記述子のタイプ。関係記述子の場合、この値をに設定する必要があります。 `xdm:descriptorOneToOne`. |
| `xdm:sourceSchema` | 記述子を定義するスキーマの `$id` URI。 |
| `xdm:sourceVersion` | ソーススキーマのメジャーバージョン。 |
| `xdm:sourceProperty` | 関係を定義するソーススキーマ内のフィールドのパス。「/」で始まる必要がありますが、「/」で終わらない必要があります。パスに「プロパティ」を含めてはいけません（例：「/properties/personalEmail/properties/address」の代わりに「/personalEmail/address」を使用）。。 |
| `xdm:destinationSchema` | この `$id` この記述子が関係を定義する参照スキーマの URI。 |
| `xdm:destinationVersion` | 参照スキーマのメジャーバージョン。 |
| `xdm:destinationProperty` | 参照スキーマ内のターゲットフィールドへのオプションのパス。 このプロパティを省略すると、ターゲットフィールドは、対応する参照 ID 記述子（以下を参照）を含むフィールドによって推論されます。 |

{style=&quot;table-layout:auto&quot;}

#### 参照 ID 記述子

参照 ID 記述子は、スキーマフィールドのプライマリ ID への参照コンテキストを提供し、他のスキーマのフィールドで参照できるようにします。 参照スキーマは、この記述子を介して他のスキーマから参照できるように、事前にプライマリ ID フィールドが定義されている必要があります。

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
| `@type` | 定義する記述子のタイプ。参照 ID 記述子の場合、この値をに設定する必要があります。 `xdm:descriptorReferenceIdentity`. |
| `xdm:sourceSchema` | 記述子を定義するスキーマの `$id` URI。 |
| `xdm:sourceVersion` | ソーススキーマのメジャーバージョン。 |
| `xdm:sourceProperty` | 参照スキーマを参照するために使用されるソーススキーマ内のフィールドのパス。 「/」で始まる必要がありますが、「/」で終わらない必要があります。パスに「プロパティ」を含めない ( 例： `/personalEmail/address` の代わりに `/properties/personalEmail/properties/address`) をクリックします。 |
| `xdm:identityNamespace` | ソースプロパティの ID 名前空間コード。 |

{style=&quot;table-layout:auto&quot;}

#### 廃止されたフィールド記述子

以下が可能です。 [カスタム XDM リソース内のフィールドを廃止する](../tutorials/field-deprecation.md#custom) を `meta:status` 属性を `deprecated` 問題のフィールドに ただし、スキーマ内の標準 XDM リソースで提供されるフィールドを非推奨にする場合は、非推奨のフィールド記述子を対象のスキーマに割り当てても、同じ効果を得ることができます。 の使用 [正しい `Accept` ヘッダー](../tutorials/field-deprecation.md#verify-deprecation)の場合は、API でスキーマを検索する際に、非推奨となった標準フィールドを表示できます。

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
| `@type` | 記述子のタイプ。 フィールド廃止記述子の場合、この値をに設定する必要があります。 `xdm:descriptorDeprecated`. |
| `xdm:sourceSchema` | URI `$id` 記述子を適用するスキーマの。 |
| `xdm:sourceVersion` | 記述子を適用するスキーマのバージョン。 をに設定する必要があります。 `1`. |
| `xdm:sourceProperty` | 記述子を適用するスキーマ内のプロパティへのパス。 記述子を複数のプロパティに適用する場合は、パスのリストを配列 ( 例えば、 `["/firstName", "/lastName"]`) をクリックします。 |

{style=&quot;table-layout:auto&quot;}
