---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 記述子
topic: developer guide
translation-type: tm+mt
source-git-commit: 599991af774e283d9fb60216e3d3bd5b17cf8193

---


# 記述子

スキーマは、データエンティティの静的表示を定義しますが、これらのスキーマ（データセットなど）に基づくデータが相互にどのように関連付けられるかに関する具体的な詳細は提供しません。 Adobe Experience Platformでは、記述子を使用して、これらの関係や、記述子に関するその他の解釈的なメタスキーマを記述できます。

スキーマ記述子はテナントレベルのメタデータで、IMS組織に固有で、すべての記述子操作がテナントコンテナで行われます。

各スキーマには、1つ以上のスキーマ記述子エンティティを適用できます。 各スキーマ記述子エンティティは、記述子 `@type` と、それが適 `sourceSchema` 用される記述子を含みます。 これらの記述子は、適用されると、その記述子を使用して作成されたすべてのデータセットにスキーマされます。

このドキュメントは、記述子のAPI呼び出しの例と、使用可能な記述子の完全なリストと、各型の定義に必要なフィールドを提供します。

>[!NOTE] 記述子は一意のAcceptヘッダを必要とします。このヘッダは `xed` で置き換えら `xdm`れますが、それ以外の場合は、Acceptヘッダがスキーマレジストリ内の他の場所で使用されているヘッダと非常に似ています。 以下のサンプル呼び出しには、適切なAcceptヘッダが含まれていますが、正しいヘッダが使用されていることを確認するために、十分に注意が必要です。

## リスト記述子

1つのGET要求を使用して、組織で定義されたすべてのリストの記述子を返すことができます。

**API形式**

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

応答の形式は、リクエストで送信されるAcceptヘッダーに依存します。 エンドポイントでは、 `/descriptors` スキーマレジストリAPIの他のすべてのエンドポイントとは異なるAcceptヘッダーが使用されます。

記述子受け付けヘッダは、 `xed` 記述子に `xdm`固有のオプシ `link` ョンで置き換えられ、オファーを指定します。

| 承認 | 説明 |
| -------|------------ |
| `application/vnd.adobe.xdm-id+json` | 記述子IDの配列を返します。 |
| `application/vnd.adobe.xdm-link+json` | 記述子APIパスの配列を返します。 |
| `application/vnd.adobe.xdm+json` | 拡張された記述子オブジェクトの配列を返します。 |

**応答**

応答には、定義された記述子を持つ各記述子タイプの配列が含まれます。 つまり、ある種の記述子が定義されていない場合、そ `@type` の記述子型の空の配列は返されません。

Acceptヘッダを使 `link` 用する場合、各記述子は配列項目として、 `/{CONTAINER}/descriptors/{DESCRIPTOR_ID}`

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

特定の記述子の詳細を表示したい場合は、個々の記述子をその記述子を使って検索(GET)できま `@id`す。

**API形式**

```http
GET /tenant/descriptors/{DESCRIPTOR_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DESCRIPTOR_ID}` | 参照 `@id` する記述子の名前。 |

**リクエスト**

記述子はバージョン管理されないので、参照要求にAcceptヘッダーは不要です。

```SHELL
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors/f3a1dfa38a4871cf4442a33074c1f9406a593407 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、記述子の詳細（およびを含む）と、 `@type` 記述 `sourceSchema`子の種類に応じて変化する追加情報を返します。 返される値は、 `@id` リクエストで指定された記述子 `@id` と一致する必要があります。

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

スキーマレジストリでは、複数の異なる記述子の種類を定義できます。 各記述子タイプは、POSTリクエストで送信する固有のフィールドを必要とします。 記述子の完全なリストと、それらを定義するために必要なフィールドは、記述子の定義に関する付録の節に記載さ [れています](#defining-descriptors)。

**API形式**

```http
POST /tenant/descriptors
```

**リクエスト**

次のリクエストは、サンプルディスクリプタの「電子メールアドレス」フィールドにID記述子を定義します。スキーマ これにより、Experience Platformは、電子メールアドレスを識別子として使用し、個人に関する情報を組み合わせるのに役立ちます。

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

成功した応答は、HTTPステータス201（作成済み）と、新しく作成された記述子の詳細（記述子を含む）を返しま `@id`す。 は読み取 `@id` り専用のフィールドで、スキーマレジストリによって割り当てられ、API内の記述子を参照するために使用されます。

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

記述子を更新するには、リクエストパスで、更新する記述子 `@id` のPUTリクエストを作成します。

**API形式**

```http
PUT /tenant/descriptors/{DESCRIPTOR_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DESCRIPTOR_ID}` | 更新 `@id` する記述子の。 |

**リクエスト**

このリクエストは基本的 _に記述子を書き換え_ るので、リクエスト本体には、その型の記述子を定義するために必要なすべてのフィールドが含まれている必要があります。 つまり、記述子を更新(PUT)する要求ペイロードは、同じタイプの記述子を作成(POST)するペイロードと同じです。

この例では、ID記述子は別の（「携帯電話」）を参照するよ `xdm:sourceProperty` うに更新され、を「電話」 `xdm:namespace` に変更しています。

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

プロパティと、そ `xdm:namespace` のア `xdm:property`クセス方法などに関する詳細は、記述子の定義に関する付録の節に記載さ [れています](#defining-descriptors)。

**応答**

成功した応答は、HTTPステータス201（作成済み）と、更新さ `@id` れた記述子(リクエストで送信された記述子と一致 `@id` する必要がある)を返します。

```JSON
{
    "@id": "f3a1dfa38a4871cf4442a33074c1f9406a593407"
}
```

表示に対してルックアップ(GET)リクエストを実行すると、PUTリクエストで送信された変更を反映するようにフィールドが更新されたことが記述子に示されます。

## 記述子の削除

場合によっては、定義した記述子をスキーマレジストリから削除する必要があります。 これは、削除する記述子を参照するDELETE `@id` リクエストを作成することで行われます。

**API形式**

```http
DELETE /tenant/descriptors/{DESCRIPTOR_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DESCRIPTOR_ID}` | 削除 `@id` する記述子の。 |

**リクエスト**

記述子を削除する場合、ヘッダーの受け入れは必要ありません。

```SHELL
curl -X DELETE \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors/ca921946fb5281cbdb8ba5e07087486ce531a1f2  \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、HTTPステータス204（コンテンツなし）と空白の本文を返します。

記述子が削除されたことを確認するために、記述子に対してルックアップリクエストを実行できま `@id`す。 ディスクリプタがディスクリプタレジストリから削除されたので、応答はHTTPステータス404 （見つかりません）をスキーマします。

## 付録

次の節では、ディスクリプタレジストリAPIでの記述子の操作に関する追加スキーマを示します。

### 記述子の定義

以下の節では、使用可能な記述子の型の概要を示します。各型の記述子を定義するための必須フィールドも含まれます。

#### ID記述子

ID記述子は、「sourceSchema」の「sourceProperty」が [Adobe Experience Platform Identity Serviceの説明に従うIdentityフィールドであることを通知します](../../identity-service/home.md)。

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
| `@type` | 定義する記述子の型。 |
| `xdm:sourceSchema` | 記述子を `$id` 定義するスキーマのURIです。 |
| `xdm:sourceVersion` | ソース・バージョンのメジャー・スキーマ。 |
| `xdm:sourceProperty` | IDとなる特定のプロパティへのパス。 パスは、末尾が「/」ではなく「/」で始まる必要があります。 パスに「プロパティ」を含めない（例：「/properties/personalEmail/properties/address」の代わりに「/personalEmail/address」を使用） |
| `xdm:namespace` | ID `id` の `code` 名前空間。 名前空間のリストは、 [Identity Service APIを使用して見つかります](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/id-service-api.yaml)。 |
| `xdm:property` | また `xdm:id` は( `xdm:code`使用に応じて) `xdm:namespace` 。 |
| `xdm:isPrimary` | オプションのboolean値。 trueの場合、フィールドがプライマリIDであることを示します。 スキーマには、1つのプライマリIDのみを含めることができます。 |

#### わかりやすい名前記述子

わかりやすい名前記述子を使用すると、ユーザーはコアライブラリ `title` スキーマフ `description` ィールドの値と値を変更できます。 特に、「eVar」および組織に固有の情報を含むとしてラベル付けする他の「汎用」フィールドを扱う場合に役立ちます。 UIは、これらを使用して、わかりやすい名前を表示したり、わかりやすい名前を持つフィールドのみを表示したりできます。

```json
{
  "@type": "xdm:alternateDisplayInfo",
  "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/274f17bc5807ff307a046bab1489fb18",
  "xdm:sourceVersion": 1
  "xdm:sourceProperty": "/eVars/eVar1",
  "xdm:title": {
    "en_us":{"Loyalty ID"}
  },
  "xdm:description": {
    "en_us":{"Unique ID of loyalty program member."}
  },
}
```

| プロパティ | 説明 |
| --- | --- |
| `@type` | 定義する記述子の型。 |
| `xdm:sourceSchema` | 記述子を `$id` 定義するスキーマのURIです。 |
| `xdm:sourceVersion` | ソース・バージョンのメジャー・スキーマ。 |
| `xdm:sourceProperty` | IDとなる特定のプロパティへのパス。 パスは、末尾が「/」ではなく「/」で始まる必要があります。 パスに「プロパティ」を含めない（例：「/properties/personalEmail/properties/address」の代わりに「/personalEmail/address」を使用） |
| `xdm:title` | このフィールドに表示する新しいタイトル（大文字と小文字を区別）。 |
| `xdm:description` | オプションで、タイトルと共に説明を追加できます。 |

#### 関係記述子

関係記述子は、とで説明されているプロパティに基づいて、2つの異なるスキーマ間の関係を記述 `sourceProperty` しま `destinationProperty`す。 詳しくは、2つの関係の定義に関 [するチュートリアルを参照してください](../tutorials/relationship-api.md) 。スキーマを参照してください。

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
| `@type` | 定義する記述子の型。 |
| `xdm:sourceSchema` | 記述子を `$id` 定義するスキーマのURIです。 |
| `xdm:sourceVersion` | ソース・バージョンのメジャー・スキーマ。 |
| `xdm:sourceProperty` | 関係を定義するソーススキーマ内のフィールドのパス。 末尾が「/」ではなく「/」で始まる必要があります。 パスに「プロパティ」を含めないでください（例：「/properties/personalEmail/properties/address」の代わりに「/personalEmail/address」）。 |
| `xdm:destinationSchema` | このデ `$id` ィスクリプタが関係を定義する宛先スキーマのURI。 |
| `xdm:destinationVersion` | 宛先のメジャースキーマ。 |
| `xdm:destinationProperty` | 宛先のオプション内のターゲットフィールドへのパス(スキーマ)。 このプロパティを省略すると、ターゲットフィールドは、対応する参照ID記述子（以下を参照）を含むフィールドによって推論されます。 |


#### 参照ID記述子

参照ID記述子は、スキーマフィールドへの参照コンテキストを提供し、宛先ディスクリプタのプライマリIDフィールドにリンクできるようにします。スキーマ 参照記述子を適用する前に、フィールドにID記述子のラベルを付けておく必要があります。

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
| `@type` | 定義する記述子の型。 |
| `xdm:sourceSchema` | 記述子を `$id` 定義するスキーマのURIです。 |
| `xdm:sourceVersion` | ソース・バージョンのメジャー・スキーマ。 |
| `xdm:sourceProperty` | 記述子を定義するソーススキーマ内のフィールドのパス。 末尾が「/」ではなく「/」で始まる必要があります。 パスに「プロパティ」を含めないでください（例：「/properties/personalEmail/properties/address」の代わりに「/personalEmail/address」）。 |
| `xdm:identityNamespace` | ソース名前空間のIDプロパティコード。 |