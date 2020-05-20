---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: スキーマレジストリAPI開発者ガイド
topic: developer guide
translation-type: tm+mt
source-git-commit: 387cbdebccb9ae54a2907d1afe220e9711927ca6
workflow-type: tm+mt
source-wordcount: '1246'
ht-degree: 0%

---


# スキーマレジストリAPI開発者ガイド

スキーマレジストリは、Adobe Experience Platform内のスキーマライブラリにアクセスするために使用され、使用可能なすべてのライブラリリソースにアクセスできるユーザーインターフェイスとRESTful APIを提供します。

スキーマレジストリAPIを使用すると、Adobe Experience Platform内で利用可能なすべてのスキーマおよび関連リソースを表示および管理するために、基本的なCRUD操作を実行できます。 これには、アドビ、エクスペリエンスプラットフォームパートナー、および使用するアプリケーションのベンダーが定義するものも含まれます。 また、API呼び出しを使用して、組織の新しいスキーマやリソース、既に定義した表示や編集のリソースを作成することもできます。

この開発者ガイドでは、スキーマレジストリAPIを使用して開始を行う際に役立つ手順を説明します。 次に、スキーマレジストリを使用してキー操作を実行するためのサンプルAPI呼び出しを提供します。

## 前提条件

このガイドでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [Experience Data Model(XDM)System](../home.md): エクスペリエンスプラットフォームが顧客エクスペリエンスデータを編成する際に使用する標準化されたフレームワークです。
   * [スキーマ構成の基本](../schema/composition.md): XDMスキーマの基本的な構成要素について説明します。
* [リアルタイム顧客プロファイル](../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [サンドボックス](../../sandboxes/home.md): Experience Platformは、1つのプラットフォームインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

以下の節では、スキーマレジストリAPIを正しく呼び出すために知っておく必要がある追加情報について説明します。

## サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例の読み [方に関する節を参照してください](../../landing/troubleshooting.md#how-do-i-format-an-api-request) 。

## 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのExperience Platform API呼び出しに必要な各ヘッダーの値を指定します。

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

スキーマレジストリに属するリソースを含む、エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、「 [サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)」を参照してください。

スキーマレジストリへのすべてのルックアップ(GET)リクエストには、追加のAcceptヘッダーが必要です。このヘッダーの値は、APIから返される情報の形式を決定します。 詳細については、下の「 [承認ヘッダー](#accept) 」セクションを参照してください。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

* Content-Type: application/json

## TENANT_IDの確認 {#know-your-tenant_id}

このガイド全体で、に関するリファレンスを確認でき `TENANT_ID`ます。 このIDは、作成するリソースの名前が適切に指定され、IMS組織内に含まれていることを確認するために使用されます。 IDがわからない場合は、次のGETリクエストを実行してIDにアクセスできます。

**API形式**

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

正常に応答すると、組織のスキーマレジストリの使用に関する情報が返されます。 属性が含まれ、その値がお客様の `tenantId` 属性で `TENANT_ID`す。

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

* `tenantId`: IMS組織の `TENANT_ID` 値。

## 詳しくは、 `CONTAINER_ID` {#container}

スキーマレジストリAPIの呼び出しには、を使用する必要があり `CONTAINER_ID`ます。 API呼び出しは次の2つのコンテナに対して実行できます。 グ **ローバルコンテナ** と **テナントコンテナ**。

### グローバルコンテナ

グローバルコンテナには、標準のAdobeおよびExperience Platformパートナーが提供するクラス、ミックスイン、データ型、スキーマがすべて含まれます。 リストおよびルックアップ(GET)リクエストは、グローバルコンテナに対してのみ実行できます。

### テナントコンテナ

独自のクラス、ミックスイン、データ型、スキーマ `TENANT_ID`、およびIMS組織で定義されている記述子は、テナントコンテナにすべて保持されています。 これらは各組織に固有のものであり、他のIMS組織では表示されず、管理もできません。 テナントコンテナで作成するリソースに対して、すべてのCRUD操作(GET、POST、PUT、PATCH、DELETE)を実行できます。

テナントコンテナでクラス、ミックスイン、スキーマ、またはデータ型を作成すると、そのクラスはスキーマレジストリに保存され、ユーザーを含む `$id` URIが割り当てられ `TENANT_ID`ます。 これ `$id` は、特定のリソースを参照するためにAPI全体で使用されます。 値の例については、次の節で説明します。 `$id`

## スキーマ同定 {#schema-identification}

スキーマは、次のようなURIの形式で `$id` 属性によって識別されます。
* `https://ns.adobe.com/xdm/context/profile`
* `https://ns.adobe.com/{TENANT_ID}/schemas/7442343-abs2343-21232421`

URIをRESTに対応させるために、スキーマは、次のようなプロパティにURIのドット表記エンコードを含めることもでき `meta:altId`ます。
* `_xdm.context.profile`
* `_{TENANT_ID}.schemas.7442343-abs2343-21232421`

スキーマレジストリAPIの呼び出しは、URLエンコードされた `$id` URIまたは `meta:altId` （ドット表記の形式）をサポートします。 ベストプラクティスは、APIに対して次のようにREST呼び出しを行う場合に、URLエンコードされた `$id` URIを使用することです。
* `https%3A%2F%2Fns.adobe.com%2Fxdm%2Fcontext%2Fprofile`
* `https%3A%2F%2Fns.adobe.com%2F{TENANT_ID}%2Fschemas%2F7442343-abs2343-21232421`

## ヘッダーを受け入れる {#accept}

スキーマレジストリAPIでリストと参照(GET)の操作を実行する場合、APIから返されるデータの形式を決定するためにAcceptヘッダーが必要です。 特定のリソースを検索する場合は、Acceptヘッダーにバージョン番号も含める必要があります。

次の表に示す互換性のあるリストーのうち、バージョン番号を持つヘッダー値を受け入れる方法と、ヘッダー値を使用した場合にAPIが返す内容を説明します。

| 同意 | 説明 |
| ------- | ------------ |
| `application/vnd.adobe.xed-id+json` | IDのみのリストを返します。 これは、リソースのリスト表示に最も一般的に使用されます。 |
| `application/vnd.adobe.xed+json` | 元の値を含め、完全なJSONスキーマのリスト `$ref` を返 `allOf` します。 これは、完全なリソースのリストを返すために使用されます。 |
| `application/vnd.adobe.xed+json; version={MAJOR_VERSION}` | ANDを使用したRaw XDM `$ref` で `allOf`す。 タイトルと説明があります。 |
| `application/vnd.adobe.xed-full+json; version={MAJOR_VERSION}` | `$ref` 属性を `allOf` 解決します。 タイトルと説明があります。 |
| `application/vnd.adobe.xed-notext+json; version={MAJOR_VERSION}` | ANDを使用したRaw XDM `$ref` で `allOf`す。 タイトルや説明はありません。 |
| `application/vnd.adobe.xed-full-notext+json; version={MAJOR_VERSION}` | `$ref` 属性を `allOf` 解決します。 タイトルや説明はありません。 |
| `application/vnd.adobe.xed-full-desc+json; version={MAJOR_VERSION}` | `$ref` 属性を `allOf` 解決します。 記述子が含まれます。 |

>[!NOTE] バージョンのみを指定する場合( `major` 例： 1、2、3)、レジストリは最新 `minor` バージョン(例： .1、.2、.3)が自動的に更新されます。

## XDMフィールドの制約とベストプラクティス

スキーマのフィールドは、その `properties` オブジェクト内に一覧表示されます。 各フィールドはオブジェクトであり、フィールドに格納できるデータを記述し、制約する属性を含みます。

APIでのフィールドタイプの定義について詳しくは、 [付録](appendix.md) （最も一般的に使用されるデータタイプのコードサンプルやオプションの制約事項を含む）を参照してください。

以下のサンプルフィールドは、適切に形式設定されたXDMフィールドを示しています。このフィールドには、命名の制約とベストプラクティスに関する詳細が以下に示されています。 これらの慣行は、類似の属性を含む他のリソースを定義する場合にも適用できます。

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

* フィールドオブジェクトの名前には、英数字、ダッシュ、アンダースコアを含めることができますが、アンダースコアを含む **開始は使用できません** 。
   * **正しい構文：** `fieldName`, `field_name2`, `Field-Name``field-name_3`
   * **誤った構文：**`_fieldName`
* fieldオブジェクトの名前にはcamelCaseをお勧めします。 例: `fieldName`
* フィールドには、「単語の先頭のみ大文字」 `title`で記述された「名称」を含める必要があります。 例: `Field Name`
* フィールドにはが必要で `type`す。
   * 特定の型を定義するには、オプションが必要な場合があり `format`ます。
   * 特定のデータのフォーマットが必要な場合は、配列として追加 `examples` できます。
   * フィールドの種類は、レジストリ内の任意のデータ型を使用して定義することもできます。 詳しくは、このガイドのデータ型の [作成に関する節](create-data-type.md) を参照してください。
* は、フィールド `description` とフィールドデータに関する関連情報を説明します。 スキーマにアクセスした人がフィールドの意図を理解できるように、明確な言葉で全文書に記述する必要があります。

APIでフィールドタイプを定義する方法について詳しくは、 [付録](appendix.md) を参照してください。

## 次の手順

このドキュメントでは、必要な認証資格情報を含む、スキーマレジストリAPIを呼び出すために必要な前提条件に関する知識を説明します。 これで、この開発者ガイドに記載されているサンプル呼び出しに進み、それらの指示に従うことができます。 APIでのスキーマの作成方法に関する詳しい手順説明については、次の [チュートリアルを参照してください](../tutorials/create-schema-api.md)。
