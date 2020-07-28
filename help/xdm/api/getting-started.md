---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Schema Registry API 開発者ガイド
topic: developer guide
translation-type: tm+mt
source-git-commit: d04bf35e49488ab7d5e07de91eb77d0d9921b6fa
workflow-type: tm+mt
source-wordcount: '1195'
ht-degree: 71%

---


# [!DNL Schema Registry] API開発者ガイド

The [!DNL Schema Registry] is used to access the Schema Library within Adobe Experience Platform, providing a user interface and RESTful API from which all available library resources are accessible.

Schema Registry API を使用すると、基本的な CRUD 操作を実行して、Adobe Experience Platform 内で使用可能なすべてのスキーマと関連リソースを表示および管理できます。This includes those defined by Adobe, [!DNL Experience Platform] partners, and vendors whose applications you use. また、API 呼び出しを使用して、組織の新しいスキーマやリソースを作成したり、定義済みリソースの表示や編集をおこなうこともできます。

This developer guide provides steps to help you start using the [!DNL Schema Registry] API. The guide then provides sample API calls for performing key operations using the [!DNL Schema Registry].

## 前提条件

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [!DNL Experience Data Model (XDM) System](../home.md): 顧客体験データを [!DNL Experience Platform] 整理するための標準化されたフレームワーク。
   * [Basics of schema composition](../schema/composition.md)：XDM スキーマの基本的な構成要素について説明しています。
* [!DNL Real-time Customer Profile](../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [!DNL Sandboxes](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

The following sections provide additional information that you will need to know in order to successfully make calls to the [!DNL Schema Registry] API.

## API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切に書式設定されたリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

## 必須ヘッダーの値の収集

In order to make calls to [!DNL Platform] APIs, you must first complete the [authentication tutorial](../../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

* Authorization: Bearer `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

All resources in [!DNL Experience Platform], including those belonging to the [!DNL Schema Registry], are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in:

* x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>For more information on sandboxes in [!DNL Platform], see the [sandbox overview documentation](../../sandboxes/home.md).

All lookup (GET) requests to the [!DNL Schema Registry] require an additional Accept header, whose value determines the format of information returned by the API. 詳しくは、この後の [Accept ヘッダー](#accept)の節を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

* Content-Type: application/json

## 使用する TENANT_ID {#know-your-tenant_id}

このガイド全体を通して、`TENANT_ID` への参照が使用されます。この ID は、作成したリソースに名前空間が適切に設定されていること、またそのリソースが IMS 組織内に格納されていることを確認するために使用されます。ID が不明な場合は、次の GET リクエストを実行して ID にアクセスします。

**API 形式**

```http
GET /stats
```

**リクエスト**

```SHELL
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/stats \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

A successful response returns information regarding your organization&#39;s use of the [!DNL Schema Registry]. この中には `TENANT_ID` の値である `tenantId` 属性が含まれています。

```JSON
{
  "imsOrg":"{IMS_ORG}",
  "tenantId":"{TENANT_ID}",
  "counts": {
    "schemas": 4,
    "mixins": 3,
    "datatypes": 1,
    "classes": 2,
    "unions": 0,
  },
  "recentlyCreatedResources": [ 
    {
      "title": "Sample Mixin",
      "description": "New Sample Mixin.",
      "meta:resourceType": "mixins",
      "meta:created": "Sat Feb 02 2019 00:24:30 GMT+0000 (UTC)",
      "version": "1.1"
    },
    {
      "$id": "https://ns.adobe.com/{TENANT_ID}/classes/5bdb5582be0c0f3ebfc1c603b705764f",
      "title": "Tenant Class",
      "description": "Tenant Defined Class",
      "meta:resourceType": "classes",
      "meta:created": "Fri Feb 01 2019 22:46:21 GMT+0000 (UTC)",
      "version": "1.0"
    } 
  ],
  "recentlyUpdatedResources": [
    {
      "title": "Sample Mixin",
      "description": "New Sample Mixin.",
      "meta:resourceType": "mixins",
      "meta:updated": "Sat Feb 02 2019 00:34:06 GMT+0000 (UTC)",
      "version": "1.1"
    },
    {
      "title": "Data Schema",
      "description": "Schema for Data Information",
      "meta:resourceType": "schemas",
      "meta:updated": "Fri Feb 01 2019 23:47:43 GMT+0000 (UTC)",
      "meta:class": "https://ns.adobe.com/{TENANT_ID}/classes/47b2189fc135e03c844b4f25139d10ab",
      "meta:classTitle": "Sample Class",
      "version": "1.1"
    }
 ],
 "classUsage": {
    "https://ns.adobe.com/{TENANT_ID}/classes/47b2189fc135e03c844b4f25139d10ab": [
      {
        "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/274f17bc5807ff307a046bab1489fb18",
        "title": "Tenant Data Schema",
        "description": "Schema for tenant-specific data."
      }
    ],
    "https://ns.adobe.com/xdm/context/profile": [
      {
        "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/3ac6499f0a43618bba6b138226ae3542",
        "title": "Simple Profile",
        "description": "A simple profile schema."
      },
      {
        "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/fbc52b243d04b5d4f41eaa72a8ba58be",
        "title": "Program Schema",
        "description": "Schema for program-related data."
      },
      {
        "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/4025a705890c6d4a4a06b16f8cf6f4ca",
        "title": "Sample Schema",
        "description": "A sample schema."
      }
    ]
  }
 }
```

* `tenantId`：IMS組織の `TENANT_ID` 値。

## `CONTAINER_ID` について {#container}

Calls to the [!DNL Schema Registry] API require the use of a `CONTAINER_ID`. API 呼び出しを実行できるコンテナには、**グローバル**&#x200B;コンテナと **テナント**&#x200B;コンテナの 2 つがあります。

### グローバルコンテナ

The global container holds all standard Adobe and [!DNL Experience Platform] partner provided classes, mixins, data types, and schemas. リストリクエストと検索（GET）リクエストを実行できるのは、グローバルコンテナに対してのみです。

### テナントコンテナ

固有の `TENANT_ID` と混同しないでください。テナントコンテナには、IMS 組織が定義したすべてのクラス、Mixin、データタイプ、スキーマ、および記述子が格納されます。これらは各組織に固有のもので、他の IMS 組織では表示も管理もできません。テナントコンテナで作成したリソースに対して、すべての CRUD 操作（GET、POST、PUT、PATCH、DELETE）を実行できます。

When you create a class, mixin, schema or data type in the tenant container, it is saved to the [!DNL Schema Registry] and assigned an `$id` URI that includes your `TENANT_ID`. この `$id` は、API 全体で特定のリソースを参照する際に使用されます。`$id` 値の例については、次の節で説明します。

## スキーマの識別 {#schema-identification}

スキーマは、次のような URI 形式の `$id` 属性で識別されます。
* `https://ns.adobe.com/xdm/context/profile`
* `https://ns.adobe.com/{TENANT_ID}/schemas/7442343-abs2343-21232421`

URI をより REST に適したものにするために、スキーマでは、`meta:altId` と呼ばれるプロパティで、ドット表記でエンコードされた URI を使用できます。
* `_xdm.context.profile`
* `_{TENANT_ID}.schemas.7442343-abs2343-21232421`

Schema Registry API への呼び出しでは、URL エンコードされた `$id` URI または `meta:altId`（ドット表記の形式）がサポートされています。API への REST 呼び出しを実行する際には、URL エンコードされた `$id` URI を使用することをお勧めします。
* `https%3A%2F%2Fns.adobe.com%2Fxdm%2Fcontext%2Fprofile`
* `https%3A%2F%2Fns.adobe.com%2F{TENANT_ID}%2Fschemas%2F7442343-abs2343-21232421`

## Accept ヘッダー {#accept}

When performing list and lookup (GET) operations in the [!DNL Schema Registry] API, an Accept header is required to determine the format of the data returned by the API. 特定のリソースを検索する場合は、Accept ヘッダーにバージョン番号も含める必要があります。

次の表に、互換性のある Accept ヘッダー値（バージョン番号を持つヘッダー値を含む）と、そのヘッダー値を使用したときに API が返す内容の説明を示します。

| Accept | 説明 |
| ------- | ------------ |
| `application/vnd.adobe.xed-id+json` | ID のリストのみを返します。これは、リソースを一覧表示する際に最も使用される値です。 |
| `application/vnd.adobe.xed+json` | 元の `$ref` および `allOf` を含むフル JSON スキーマのリストを返します。これは、全リソースのリストを返す際に使用されます。 |
| `application/vnd.adobe.xed+json; version={MAJOR_VERSION}` | `$ref` と `allOf` を含む未処理の XDM です。タイトルと説明があります。 |
| `application/vnd.adobe.xed-full+json; version={MAJOR_VERSION}` | `$ref` 属性と解決された `allOf`。タイトルと説明があります。 |
| `application/vnd.adobe.xed-notext+json; version={MAJOR_VERSION}` | `$ref` と `allOf` を含む未処理の XDM です。タイトルや説明はありません。 |
| `application/vnd.adobe.xed-full-notext+json; version={MAJOR_VERSION}` | `$ref` 属性と解決された `allOf`。タイトルや説明はありません。 |
| `application/vnd.adobe.xed-full-desc+json; version={MAJOR_VERSION}` | `$ref` 属性と解決された `allOf`。記述子が含まれます。 |

>[!NOTE]
>
>`major` バージョン（1、2、3 など）のみを指定した場合、レジストリは最新の `minor` バージョン（.1、.2、.3 など）を自動的に返します。

## XDM フィールドの制約とベストプラクティス

スキーマのフィールドは、その `properties` オブジェクト内にリストされます。各フィールド自体はオブジェクトで、フィールドに格納できるデータを記述および制約する属性を含みます。

API でのフィールドの種類の定義について詳しくは、このガイドの[付録](appendix.md)を参照してください。この付録には、最も一般的に使用されるデータタイプ用のコードサンプルとオプションの制約が示されています。

以下のサンプルフィールドは、適切に形式が設定された XDM フィールドを表しています。サンプルコードの下に、命名時の制約とベストプラクティスが示されています。これらのベストプラクティスは、同様の属性を含むその他のリソースを定義する際にも適用できます。

```JSON
"fieldName": {
    "title": "Field Name",
    "type": "string",
    "format": "date-time",
    "examples": [
        "2004-10-23T12:00:00-06:00"
    ],
    "description": "Full sentence describing the field, using proper grammar and punctuation.",
}
```

* フィールドオブジェクトの名前には、英数字、ダッシュ、アンダースコアの各文字を使用できますが、先頭にアンダースコアを配置することは&#x200B;**できません**。
   * **正しい：**`fieldName`、`field_name2`、`Field-Name`、`field-name_3`
   * **誤り：**`_fieldName`
* フィールドオブジェクトの名前には、キャメルケースを使用することをお勧めします。例：`fieldName`
* フィールドには `title` が必要です。これは、単語の先頭のみ大文字で記述します。例：`Field Name`
* フィールドには `type` が必要です。
   * 特定のタイプを定義する場合、オプションの `format` が必要なことがあります。
   * データに特定の形式を設定する必要がある場合は、`examples` を配列として追加できます。
   * フィールドの種類は、レジストリで任意のデータタイプを使用して定義することもできます。詳しくは、このガイドの[データタイプの作成](create-data-type.md)に関する節を参照してください。
* `description` では、フィールドとフィールドデータについての関連情報を表します。スキーマにアクセスした人が誰でもフィールドの意図を理解できるように、明確な言葉で記述する必要があります。

API でフィールドの種類を定義する方法について詳しくは、[付録](appendix.md)を参照してください。

## 次の手順

This document covered the prerequisite knowledge required to make calls to the [!DNL Schema Registry] API, including required authentication credentials. これで、この開発者ガイドに記載されているサンプル呼び出しに進み、その手順に従うことができます。API でのスキーマの作成手順について詳しくは、[こちら](../tutorials/create-schema-api.md)のチュートリアルを参照してください。
