---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 記述子
topic: developer guide
translation-type: tm+mt
source-git-commit: d04bf35e49488ab7d5e07de91eb77d0d9921b6fa
workflow-type: tm+mt
source-wordcount: '1477'
ht-degree: 1%

---


# 記述子

スキーマは、データエンティティの静的な表示を定義しますが、これらのスキーマ（データセットなど）に基づくデータが相互にどのように関連付けられるかに関する具体的な詳細は提供しません。 Adobe Experience Platformを使用すると、これらの関係と、記述子を使用したスキーマに関する他の解釈メタデータを記述できます。

スキーマ記述子はテナントレベルのメタデータで、IMS組織に固有で、すべての記述子コンテナがテナント操作で実行されます。

各スキーマには、1つ以上のスキーマ記述子エンティティを適用できます。 各スキーマ記述子エンティティは、記述子 `@type` と、それが適用さ `sourceSchema` れる記述子を含む。 これらの記述子は、スキーマを使用して作成されたすべてのデータセットに適用されます。

このドキュメントは、記述子のAPI呼び出しの例と、使用可能な記述子の完全なリストと、各型の定義に必要なフィールドを提供します。

>[!NOTE]
>
>記述子には一意のAcceptヘッダが必要です。このヘッダはで置き換えら `xed` れ `xdm`ますが、それ以外の場合は、Acceptヘッダが内の他の場所で使用されているのと非常に似ていま [!DNL Schema Registry]す。 以下のサンプル呼び出しには、適切なAcceptヘッダが含まれていますが、正しいヘッダが使用されていることを確認するために十分に注意してください。

## リスト記述子

1つのGET要求を使用して、組織で定義されているすべての記述子のリストを返すことができます。

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

応答の形式は、要求で送信されるAcceptヘッダーによって異なります。 エンドポイントでは、API内の他のすべてのエンドポイントとは異なるAcceptヘッダーが `/descriptors`[!DNL Schema Registry] 使用されていることに注意してください。

記述子受け付けヘッダーは、 `xed` に置き換え `xdm`られ、記述子に固有の `link` オプションをオファーします。

| 同意 | 説明 |
| -------|------------ |
| `application/vnd.adobe.xdm-id+json` | 記述子IDの配列を返します |
| `application/vnd.adobe.xdm-link+json` | 記述子APIパスの配列を返します |
| `application/vnd.adobe.xdm+json` | 拡張された記述子オブジェクトの配列を返します |

**応答**

応答には、定義された記述子を持つ各記述子の型の配列が含まれます。 つまり、ある種の定義されたディスクリプタが存在しない場合、レジストリはそのディスクリプタ型の空の配列を返さない `@type` 。

Acceptヘッダを使用する場合、各 `link` 記述子は配列項目として形式で表示されます `/{CONTAINER}/descriptors/{DESCRIPTOR_ID}`

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

## 記述子を検索します

特定のディスクリプタの詳細を表示したい場合は、個々のディスクリプタをそのディスクリプタを使って調べる(GET)ことができ `@id`ます。

**API形式**

```http
GET /tenant/descriptors/{DESCRIPTOR_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DESCRIPTOR_ID}` | 参照 `@id` する記述子の名前。 |

**リクエスト**

記述子はバージョン管理されないため、参照要求にAcceptヘッダーは必要ありません。

```SHELL
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors/f3a1dfa38a4871cf4442a33074c1f9406a593407 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、記述子の詳細( `@type` およびを含む)と、記述子の種類に応じて変化する追加情報を返し `sourceSchema`ます。 返される値 `@id` は、要求で指定された記述子と一致する必要 `@id` があります。

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

では、異なる複数の種類の記述子を定義で [!DNL Schema Registry] きます。 各記述子の型は、POST要求で送信する固有のフィールドを必要とします。 記述子の完全なリストと、それらを定義するのに必要なフィールドは、記述子の [定義に関する付録の節で説明](#defining-descriptors)。

**API形式**

```http
POST /tenant/descriptors
```

**リクエスト**

次のリクエストは、サンプルスキーマの「電子メールアドレス」フィールドにID記述子を定義します。 これにより、電子メールアドレス [!DNL Experience Platform] を識別子として使用し、個々の人に関する情報を結合するのに役立ちます。

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

応答が成功すると、HTTPステータス201（作成済み）と、新たに作成された記述子の詳細（その記述子を含む）が返され `@id`ます。 は、によって割り当てら `@id`[!DNL Schema Registry] れ、APIで記述子を参照するために使用される読み取り専用のフィールドです。

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

記述子を更新するには、更新したい記述子 `@id` のPUTリクエストをリクエストパスで指定します。

**API形式**

```http
PUT /tenant/descriptors/{DESCRIPTOR_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DESCRIPTOR_ID}` | 更新 `@id` する記述子の名前。 |

**リクエスト**

このリクエストは基本的に記述子を _書き直すので_ 、この型の記述子を定義するのに必要なすべてのフィールドをリクエスト本体に含める必要があります。 つまり、記述子を更新(PUT)する要求ペイロードは、同じタイプの記述子を作成(POST)するペイロードと同じです。

この例では、ID記述子が別の `xdm:sourceProperty` （「携帯電話」）を参照し、を「電話」に変更するように更新 `xdm:namespace` されています。

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

プロパティ `xdm:namespace` とプロパティに関する詳細（アクセス方法など）は、記述子の `xdm:property`定義に関する付録の節に記載されています [](#defining-descriptors)。

**応答**

応答が成功すると、HTTPステータス201（作成済み）と、更新された記述子(要求で `@id``@id` 送信されたものと一致する必要がある)が返されます。

```JSON
{
    "@id": "f3a1dfa38a4871cf4442a33074c1f9406a593407"
}
```

表示へのルックアップ(GET)リクエストを実行すると、PUTリクエストで送信された変更を反映するようにフィールドが更新されたことを示します。

## 記述子の削除

定義した記述子をから削除しなければならない場合があり [!DNL Schema Registry]ます。 これは、削除したいディスクリプタ `@id` の内部を参照するDELETEリクエストを行うことで行われます。

**API形式**

```http
DELETE /tenant/descriptors/{DESCRIPTOR_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DESCRIPTOR_ID}` | 削除 `@id` する記述子の名前。 |

**リクエスト**

記述子を削除する場合、ヘッダーを受け入れる必要はありません。

```SHELL
curl -X DELETE \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors/ca921946fb5281cbdb8ba5e07087486ce531a1f2  \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、HTTPステータス204（コンテンツなし）と空白の本文が返されます。

記述子が削除されたことを確認するために、記述子に対してルックアップリクエストを実行でき `@id`ます。 記述子がから削除されたので、応答はHTTPステータス404 （見つかりません）を返し [!DNL Schema Registry]ます。

## 付録

次の節では、 [!DNL Schema Registry] APIの記述子の操作に関する追加情報について説明します。

### 記述子の定義

以下の節では、各型の記述子を定義するために必要なフィールドを含む、使用可能な記述子型の概要を説明します。

#### ID記述子

ID記述子は、「[!UICONTROL sourceSchema]」の「[!UICONTROL sourceProperty]」が、 [!DNL Identity] Adobe Experience PlatformIDサービスの記述に従うフィールドであることを通知します [](../../identity-service/home.md)。

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
| `xdm:sourceSchema` | 記述子を定義するスキーマの `$id` URI。 |
| `xdm:sourceVersion` | ソーススキーマのメジャーバージョン。 |
| `xdm:sourceProperty` | IDとなる特定のプロパティへのパスです。 パスは「/」で始まり、1で終わらないようにしてください。 パスに「プロパティ」を含めない（例：「/properties/personalEmail/properties/address」ではなく「/personalEmail/address」を使用） |
| `xdm:namespace` | ID名前空間 `id` のまたは `code` 値。 名前空間のリストは、を使用して確認でき [!DNL Identity Service API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/id-service-api.yaml)ます。 |
| `xdm:property` | ま `xdm:id` た `xdm:code`は( `xdm:namespace` 使用されるデータに応じて) |
| `xdm:isPrimary` | オプションのboolean値。 trueの場合、フィールドを主IDとして示します。 スキーマには、1つのプライマリIDのみを含めることができます。 |

#### フレンドリ名記述子

わかりやすい名前記述子を使用して、コアライブラリスキーマフィールドの `title`、 `description`、および `meta:enum` 値を変更できます。 組織に固有の情報を含むとしてラベル付けする「eVar」や他の「汎用」フィールドを扱う場合に特に便利です。 UIは、これらを使用して、わかりやすい名前を表示したり、わかりやすい名前を持つフィールドのみを表示したりできます。

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
| `@type` | 定義する記述子の型。 |
| `xdm:sourceSchema` | 記述子を定義するスキーマの `$id` URI。 |
| `xdm:sourceVersion` | ソーススキーマのメジャーバージョン。 |
| `xdm:sourceProperty` | IDとなる特定のプロパティへのパスです。 パスは「/」で始まり、1で終わらないようにしてください。 パスに「プロパティ」を含めない（例：「/properties/personalEmail/properties/address」ではなく「/personalEmail/address」を使用） |
| `xdm:title` | このフィールドに表示する新しいタイトル。タイトルの大文字と小文字で示されます。 |
| `xdm:description` | オプションで、タイトルと共に説明を追加できます。 |
| `meta:enum` | に示すフィールド `xdm:sourceProperty` が文字列フィールドの場合、 `meta:enum` UI内のフィールドに推奨される値のリストを [!DNL Experience Platform] 決定します。 定義済みリストを宣言しないこと、またはXDMフィールドに対してデータの検証を行わないことに注意して `meta:enum` ください。<br><br>これは、アドビが定義するコアXDMフィールドに対してのみ使用する必要があります。 ソースプロパティが組織で定義されたカスタムフィールドの場合は、 `meta:enum` PATCH要求を介して、フィールドの [プロパティを直接編集する必要があります](./update-resource.md)。 |

#### 関係記述子

関係記述子は、とで説明されているプロパティに基づいてキー設定された2つの異なるスキーマ間の関係を記述 `sourceProperty` し `destinationProperty`ます。 詳細については、2つのスキーマ間の関係の [定義に関するチュートリアル](../tutorials/relationship-api.md) を参照してください。

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
| `xdm:sourceSchema` | 記述子を定義するスキーマの `$id` URI。 |
| `xdm:sourceVersion` | ソーススキーマのメジャーバージョン。 |
| `xdm:sourceProperty` | リレーションシップが定義されているソーススキーマのフィールドへのパス。 先頭は「/」で、末尾は「/」ではありません。 パスに「プロパティ」を含めないでください（例えば、「/properties/personalEmail/properties/address」ではなく「/personalEmail/address」）。 |
| `xdm:destinationSchema` | この記述子が関係を定義する宛先スキーマの `$id` URI。 |
| `xdm:destinationVersion` | 宛先スキーマのメジャーバージョン。 |
| `xdm:destinationProperty` | 宛先スキーマ内のターゲットフィールドへのパス（オプション）。 このプロパティを省略すると、ターゲットフィールドは、対応する参照ID記述子を含む任意のフィールドによって推論されます（以下を参照）。 |


#### 参照ID記述子

参照ID記述子は、スキーマフィールドへの参照コンテキストを提供し、この参照コンテキストを宛先スキーマのプライマリIDフィールドにリンクできます。 フィールドに参照記述子を適用する前に、ID記述子のラベルを付けておく必要があります。

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
| `xdm:sourceSchema` | 記述子を定義するスキーマの `$id` URI。 |
| `xdm:sourceVersion` | ソーススキーマのメジャーバージョン。 |
| `xdm:sourceProperty` | 記述子を定義するソーススキーマ内のフィールドのパス。 先頭は「/」で、末尾は「/」ではありません。 パスに「プロパティ」を含めないでください（例えば、「/properties/personalEmail/properties/address」ではなく「/personalEmail/address」）。 |
| `xdm:identityNamespace` | ソースプロパティのID名前空間コード。 |