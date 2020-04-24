---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: スキーマレジストリAPI開発者ガイド
topic: developer guide
translation-type: tm+mt
source-git-commit: 387cbdebccb9ae54a2907d1afe220e9711927ca6

---


# スキーマレジストリAPI開発者ガイド

スキーマレジストリは、Adobe Experience Platform内のスキーマライブラリにアクセスするために使用され、使用可能なすべてのライブラリリソースにアクセスできるユーザーインターフェイスとRESTful APIを提供します。

スキーマレジストリAPIを使用すると、Adobe Experience Platform内で利用できるすべてのスキーマおよび関連リソースを表示および管理するために、基本的なCRUD操作を実行できます。 これには、アドビ、エクスペリエンスプラットフォームパートナー、および使用するアプリケーションのベンダーが定義するものも含まれます。 また、API呼び出しを使用して、組織の新しいスキーマやリソースを作成したり、既に定義した表示やリソースを編集したりすることもできます。

この開発者ガイドでは、スキーマレジストリAPIを使用した開始を支援する手順を説明します。 次に、このガイドは、キー操作を実行するためのサンプルAPI呼び出しを、スキーマレジストリを使用して提供します。

## 前提条件

このガイドでは、Adobe Experience Platformの次のコンポーネントに関する作業を理解している必要があります。

* [Experience Data Model(XDM)System](../home.md):エクスペリエンスプラットフォームが顧客エクスペリエンスデータを整理するための標準化されたフレームワーク。
   * [スキーマ構成の基本](../schema/composition.md):XDMの基本的な構成要素について説明します。スキーマ
* [リアルタイム顧客プロファイル](../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムのプロファイルを顧客に提供します。
* [サンドボックス](../../sandboxes/home.md):Experience Platformは、デジタルエクスペリエンスアプリケーションの開発と発展を支援するために、単一のプラットフォームインスタンスを別々の仮想環境に分割する仮想サンドボックスを提供します。

以降の節では、スキーマレジストリAPIを正しく呼び出すために知っておく必要がある追加情報を示します。

## サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) （英語のみ）を参照してください。

## 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず認証チュートリアルを完了する必要 [があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

* 認証：無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォーム内のすべてのリソース(スキーマレジストリに属するリソースを含む)は、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、サンドボックスの概要ドキュメ [ントを参照してくださ](../../sandboxes/home.md)い。

スキーマレジストリへのすべての参照(GET)要求には、追加のAcceptヘッダーが必要です。このヘッダーの値によって、APIから返される情報の形式が決まります。 詳しくは、下の [「受け入れ](#accept) 」ヘッダーの節を参照してください。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次のヘッダーが追加で必要です。

* コンテンツタイプ：application/json

## TENANT_IDの把握 {#know-your-tenant_id}

このガイド全体で、に関するリファレンスを確認しま `TENANT_ID`す。 このIDは、作成したリソースの名前が適切に付けられ、IMS組織内に含まれていることを確認するために使用されます。 IDが不明な場合は、次のGETリクエストを実行してIDにアクセスできます。

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

成功した場合、組織のスキーマレジストリ使用に関する情報が返されます。 これには属性が含 `tenantId` まれ、その値が自分の属性の値となりま `TENANT_ID`す。

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

* `tenantId`:IMS組 `TENANT_ID` 織の値。

## 詳し `CONTAINER_ID` く {#container}

スキーマレジストリAPIの呼び出しには、を使用する必要がありま `CONTAINER_ID`す。 API呼び出しを行うコンテナは2つあります。グローバル **コンテナ** とテナ **ントコンテナ**。

### グローバルコンテナ

グローバルコンテナには、すべての標準のAdobeおよびExperience Platformパートナーが提供するクラス、ミックスイン、データ型およびスキーマが含まれます。 グローバルリストに対してのみ、GET(コンテナおよび参照)要求を実行できます。

### テナントコンテナ

固有のクラスと混同しないでください。テナ `TENANT_ID`ントコンテナには、IMS組織で定義されているすべてのクラス、ミックスイン、データ型、スキーマ、および記述子が含まれます。 これらは各組織に固有のもので、他のIMS組織では表示も管理もされません。 テナントコンテナで作成したリソースに対して、すべてのCRUD操作(GET、POST、PUT、PATCH、DELETE)を実行できます。

テナントコンテナでクラス、ミックスイン、スキーマ、またはデータ型を作成すると、スキーマレジストリに保存され、ユーザーのを含む `$id` URIが割り当てられま `TENANT_ID`す。 これは、API `$id` 全体で特定のリソースを参照するために使用されます。 値の例につ `$id` いては、次の節で説明します。

## スキーマ同定 {#schema-identification}

スキーマは、次のよ `$id` うなURIの形式で属性で識別されます。
* `https://ns.adobe.com/xdm/context/profile`
* `https://ns.adobe.com/{TENANT_ID}/schemas/7442343-abs2343-21232421`

URIをよりRESTに適したものにするために、スキーマは、次のようなプロパティにURIのドット表記エンコードを持ちま `meta:altId`す。
* `_xdm.context.profile`
* `_{TENANT_ID}.schemas.7442343-abs2343-21232421`

スキーマレジストリAPIの呼び出しは、URLエンコードされた `$id` URIまたは `meta:altId` （ドット表記の形式）をサポートします。 ベストプラクティスは、APIに対するREST呼び出しを行う際に、 `$id` URLエンコードされたURIを使用することです。例えば、次のようになります。
* `https%3A%2F%2Fns.adobe.com%2Fxdm%2Fcontext%2Fprofile`
* `https%3A%2F%2Fns.adobe.com%2F{TENANT_ID}%2Fschemas%2F7442343-abs2343-21232421`

## ヘッダーを受け入れる {#accept}

スキーマレジストリAPIでリストと参照(GET)操作を実行する場合、APIから返されるデータの形式を決定するには、Acceptヘッダーが必要です。 特定のリソースを検索する場合は、Acceptヘッダーにバージョン番号も含める必要があります。

次の表に、互換性のあるAcceptヘッダー値（バージョン番号を持つヘッダー値を含む）と、APIが使用されたときに返す内容の説明を示します。

| 承認 | 説明 |
| ------- | ------------ |
| `application/vnd.adobe.xed-id+json` | IDのみのリストを返します。 これは、リソースの一覧表示に最も一般的に使用されます。 |
| `application/vnd.adobe.xed+json` | 元のJSONリストを含む完全なJSONスキーマを `$ref` 返し `allOf` ます。 これは、全リソースのリストを返すのに使用します。 |
| `application/vnd.adobe.xed+json; version={MAJOR_VERSION}` | とを含む生のXDM `$ref` です `allOf`。 タイトルと説明があります。 |
| `application/vnd.adobe.xed-full+json; version={MAJOR_VERSION}` | `$ref` 属性と解決さ `allOf` れます。 タイトルと説明があります。 |
| `application/vnd.adobe.xed-notext+json; version={MAJOR_VERSION}` | とを含む生のXDM `$ref` です `allOf`。 タイトルや説明はありません。 |
| `application/vnd.adobe.xed-full-notext+json; version={MAJOR_VERSION}` | `$ref` 属性と解決さ `allOf` れます。 タイトルや説明はありません。 |
| `application/vnd.adobe.xed-full-desc+json; version={MAJOR_VERSION}` | `$ref` 属性と解決さ `allOf` れます。 記述子が含まれます。 |

>[!NOTE] バージョンの `major` みを指定した場合（例： 1、2、3）、レジストリは最新のバージ `minor` ョン(例：.1、.2、.3)が自動的に追加されます。

## XDMフィールドの制約とベストプラクティス

オブジェクト内にスキーマのフィールドが表示さ `properties` れます。 各フィールド自体はオブジェクトで、フィールドに格納できるデータを記述し、制約する属性を含みます。

APIでのフィールドタイプの定義について詳しくは、このガイドの付録 [](appendix.md) （最も一般的に使用されるデータ型のコードサンプルやオプションの制約など）を参照してください。

以下のサンプルフィールドは、適切に形式設定されたXDMフィールドを示しています。命名の制約とベストプラクティスの詳細については、以下に説明します。 これらの慣行は、類似の属性を含む他のリソースを定義する際にも適用できます。

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

* フィールドオブジェクトの名前には、英数字、ダッシュ、アンダースコア文字を使用できますが、アンダースコア **を含む** 開始は使用できません。
   * **正しい：**`fieldName`、 `field_name2``Field-Name`、 `field-name_3`
   * **誤った例：** `_fieldName`
* フィールドオブジェクトの名前には、camelCaseをお勧めします。 例: `fieldName`
* フィールドには、大文字と小文字の区 `title`別が付いた文字が含まれている必要があります。 例: `Field Name`
* フィールドにはが必要で `type`す。
   * 特定の型の定義にはオプションが必要な場合がありま `format`す。
   * 特定のデータの形式設定が必要な場合は、 `examples` 配列として追加できます。
   * フィールドタイプは、レジストリ内の任意のデータタイプを使用して定義することもできます。 詳しくは、このガイドの [データ型の作成に関する](create-data-type.md) 「 」の節を参照してください。
* は、フィ `description` ールドおよびフィールドデータに関する関連情報を説明します。 スキーマにアクセスした人が誰でもフィールドの意図を理解できるように、明確な言葉で全文書に書く必要があります。

APIでフィールド [タイプを定義する方法について詳しくは](appendix.md) 、付録を参照してください。

## 次の手順

このドキュメントでは、必要な認証資格情報を含む、スキーマレジストリAPIを呼び出すために必要な前提条件に関する知識を説明しました。 これで、この開発者ガイドに記載されているサンプル呼び出しに進み、その手順に従うことができます。 APIでのスキーマの作成方法に関する詳細な手順のチュートリアルについては、次のチュートリアルを参照してく [ださい](../tutorials/create-schema-api.md)。
