---
keywords: Experience Platform;home;popular topics;api;API;XDM;XDM system;;experience data model;Experience data model;Experience Data Model;data model;Data Model;schema registry;Schema Registry;
solution: Experience Platform
title: スキーマレジストリAPIの使用の手引き
description: このドキュメントでは、スキーマレジストリAPIを呼び出す前に知っておく必要があるコア概念の概要を説明します。
topic: developer guide
translation-type: tm+mt
source-git-commit: b79482635d87efd5b79cf4df781fc0a3a6eb1b56
workflow-type: tm+mt
source-wordcount: '1132'
ht-degree: 45%

---


# Getting started with the [!DNL Schema Registry] API

この [!DNL Schema Registry] APIを使用すると、様々なエクスペリエンスデータモデル(XDM)リソースを作成および管理できます。 This document provides an introduction to the core concepts you need to know before attempting to make calls to the [!DNL Schema Registry] API.

## 前提条件

開発者ガイドを使用するには、Adobe Experience Platformの次のコンポーネントについて作業的に理解する必要があります。

* [[!DNL Experience Data Model (XDM) System]](../home.md):顧客体験データを [!DNL Experience Platform] 整理する際に使用される標準化されたフレームワーク。
   * [Basics of schema composition](../schema/composition.md)：XDM スキーマの基本的な構成要素について説明しています。
* [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [[!DNL Sandboxes]](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

XDMは、JSONスキーマ形式を使用して、取り込むカスタマーエクスペリエンスデータの構造を記述し、検証します。 したがって、この基盤となる技術をより深く理解するために、 [公式のJSONスキーマドキュメント](https://json-schema.org/) を確認することを強くお勧めします。

## API 呼び出し例の読み取り

The [!DNL Schema Registry] API documentation provides example API calls to demonstrate how to format your requests. この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

## 必須ヘッダーの値の収集

In order to make calls to [!DNL Platform] APIs, you must first complete the [authentication tutorial](../../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

All resources in [!DNL Experience Platform], including those belonging to the [!DNL Schema Registry], are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in:

* `x-sandbox-name: {SANDBOX_NAME}`

>[!NOTE]
>
>For more information on sandboxes in [!DNL Platform], see the [sandbox documentation](../../sandboxes/home.md).

All lookup (GET) requests to the [!DNL Schema Registry] require an additional `Accept` header, whose value determines the format of information returned by the API. 詳しくは、この後の [Accept ヘッダー](#accept)の節を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 使用する TENANT_ID {#know-your-tenant_id}

APIガイド全体で、APIの概要に関する情報を確認でき `TENANT_ID`ます。 この ID は、作成したリソースに名前空間が適切に設定されていること、またそのリソースが IMS 組織内に格納されていることを確認するために使用されます。ID が不明な場合は、次の GET リクエストを実行して ID にアクセスします。

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

## `CONTAINER_ID` について {#container}

Calls to the [!DNL Schema Registry] API require the use of a `CONTAINER_ID`. There are two containers against which API calls can be made: the `global` container and the `tenant` container.

### グローバルコンテナ

The `global` container holds all standard Adobe and [!DNL Experience Platform] partner provided classes, mixins, data types, and schemas. You may only perform list and lookup (GET) requests against the `global` container.

この `global` コンテナを使用した呼び出しの例を次に示します。

```http
GET /global/classes
```

### テナントコンテナ

Not to be confused with your unique `TENANT_ID`, the `tenant` container holds all classes, mixins, data types, schemas, and descriptors defined by an IMS Organization. これらは各組織に固有のもので、他の IMS 組織では表示も管理もできません。You may perform all CRUD operations (GET, POST, PUT, PATCH, DELETE) against resources that you create in the `tenant` container.

この `tenant` コンテナを使用した呼び出しの例を次に示します。

```http
POST /tenant/mixins
```

When you create a class, mixin, schema or data type in the `tenant` container, it is saved to the [!DNL Schema Registry] and assigned an `$id` URI that includes your `TENANT_ID`. この `$id` は、API 全体で特定のリソースを参照する際に使用されます。`$id` 値の例については、次の節で説明します。

## リソースの識別 {#resource-identification}

XDMリソースは、次の例のように、URIの形式で `$id` 属性と共に識別されます。

* `https://ns.adobe.com/xdm/context/profile`
* `https://ns.adobe.com/{TENANT_ID}/schemas/7442343-abs2343-21232421`

URI をより REST に適したものにするために、スキーマでは、`meta:altId` と呼ばれるプロパティで、ドット表記でエンコードされた URI を使用できます。

* `_xdm.context.profile`
* `_{TENANT_ID}.schemas.7442343-abs2343-21232421`

Calls to the [!DNL Schema Registry] API will support either the URL-encoded `$id` URI or the `meta:altId` (dot-notation format). API への REST 呼び出しを実行する際には、URL エンコードされた `$id` URI を使用することをお勧めします。

* `https%3A%2F%2Fns.adobe.com%2Fxdm%2Fcontext%2Fprofile`
* `https%3A%2F%2Fns.adobe.com%2F{TENANT_ID}%2Fschemas%2F7442343-abs2343-21232421`

## Accept ヘッダー {#accept}

When performing list and lookup (GET) operations in the [!DNL Schema Registry] API, an `Accept` header is required to determine the format of the data returned by the API. When looking up specific resources, a version number must also be included in the `Accept` header.

The following table lists compatible `Accept` header values, including those with version numbers, along with descriptions of what the API will return when they are used.

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
>メジャーバージョン（1、2、3など）のみを指定した場合、レジストリは最新のマイナーバージョン(例：.1、.2、.3)が自動的に更新されます。

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
   * フィールドの種類は、レジストリで任意のデータタイプを使用して定義することもできます。See the section on [creating a data type](./data-types.md#create) in the data types endpoint guide for more information.
* `description` では、フィールドとフィールドデータについての関連情報を表します。スキーマにアクセスした人が誰でもフィールドの意図を理解できるように、明確な言葉で記述する必要があります。

APIで異なるフィールドタイプを定義する方法について詳しくは、 [フィールド制約のドキュメント](../schema/field-constraints.md) （英語）を参照してください。

## 次の手順

APIを使用した呼び出しを開始するには、使用可能なエンドポイントガイドの1つを選択します。 [!DNL Schema Registry]
