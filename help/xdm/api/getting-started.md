---
keywords: Experience Platform；ホーム；人気の高いトピック；API;API;XDM;XDMシステム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；スキーマレジストリ；スキーマレジストリ；
solution: Experience Platform
title: スキーマレジストリAPIの使い始めに
description: このドキュメントでは、スキーマレジストリAPIを呼び出す前に知っておく必要があるコア概念の概要を説明します。
topic: developer guide
exl-id: 7daebb7d-72d2-4967-b4f7-1886736db69f
translation-type: tm+mt
source-git-commit: 610ce5c6dca5e7375b941e7d6f550382da10ca27
workflow-type: tm+mt
source-wordcount: '1365'
ht-degree: 39%

---

# [!DNL Schema Registry] API使用の手引き

[!DNL Schema Registry] APIを使用すると、様々なエクスペリエンスデータモデル(XDM)リソースを作成および管理できます。 このドキュメントでは、[!DNL Schema Registry] APIを呼び出す前に知っておく必要があるコア概念の紹介を行います。

## 前提条件

開発者ガイドを使用するには、Adobe Experience Platformの次のコンポーネントについて作業的に理解する必要があります。

* [[!DNL Experience Data Model (XDM) System]](../home.md)：顧客体験データを編成する際に [!DNL Experience Platform] に使用される標準化されたフレームワーク。
   * [スキーマ構成の基本](../schema/composition.md)：XDM スキーマの基本的な構成要素について説明します。
* [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [[!DNL Sandboxes]](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

XDMは、JSONスキーマ形式を使用して、取り込むカスタマーエクスペリエンスデータの構造を記述し、検証します。 したがって、この基盤となる技術の理解を深めるために、[公式のJSONスキーマドキュメント](https://json-schema.org/)を確認することを強くお勧めします。

## API 呼び出し例の読み取り

[!DNL Schema Registry] APIドキュメントには、リクエストをフォーマットする方法を示すAPI呼び出しの例が記載されています。 この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

## 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソース（[!DNL Schema Registry]に属するリソースを含む）は、特定の仮想サンドボックスに分離されます。 [!DNL Platform] APIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform]のサンドボックスについて詳しくは、[サンドボックスのドキュメント](../../sandboxes/home.md)を参照してください。

[!DNL Schema Registry]へのすべてのルックアップ(GET)リクエストには、追加の`Accept`ヘッダーが必要です。このヘッダーの値によって、APIから返される情報の形式が決まります。 詳しくは、この後の [Accept ヘッダー](#accept)の節を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 使用する TENANT_ID {#know-your-tenant_id}

APIガイド全体で、`TENANT_ID`への参照を確認できます。 この ID は、作成したリソースに名前空間が適切に設定されていること、またそのリソースが IMS 組織内に格納されていることを確認するために使用されます。ID が不明な場合は、次の GET リクエストを実行して ID にアクセスします。

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

正常に応答すると、組織の[!DNL Schema Registry]使用に関する情報が返されます。 この中には `TENANT_ID` の値である `tenantId` 属性が含まれています。

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

[!DNL Schema Registry] APIの呼び出しには`CONTAINER_ID`を使用する必要があります。 API呼び出しは次の2つのコンテナに対して実行できます。`global`コンテナと`tenant`コンテナ。

### グローバルコンテナ

`global`コンテナは、すべての標準Adobeと[!DNL Experience Platform]パートナーが提供するクラス、ミックスイン、データ型、スキーマを保持します。 `global`コンテナに対してのみリストおよび参照(GET)要求を実行できます。

`global`コンテナを使用した呼び出しの例を次に示します。

```http
GET /global/classes
```

### テナントコンテナ

`tenant`コンテナは、IMS組織が定義するすべてのクラス、ミックスイン、データ型、スキーマ、および記述子を保持し、独自の`TENANT_ID`と混同しないでください。 これらは各組織に固有のもので、他の IMS 組織では表示も管理もできません。`tenant`コンテナで作成するリソースに対して、すべてのCRUD操作(GET、POST、PUT、PATCH、DELETE)を実行できます。

`tenant`コンテナを使用した呼び出しの例を次に示します。

```http
POST /tenant/mixins
```

`tenant`コンテナーにクラス、ミックスイン、スキーマ、またはデータ型を作成すると、そのクラスは[!DNL Schema Registry]に保存され、`TENANT_ID`を含む`$id` URIが割り当てられます。 この `$id` は、API 全体で特定のリソースを参照する際に使用されます。`$id` 値の例については、次の節で説明します。

## リソースID {#resource-identification}

XDMリソースは、次の例のように、URIの形式で`$id`属性で識別されます。

* `https://ns.adobe.com/xdm/context/profile`
* `https://ns.adobe.com/{TENANT_ID}/schemas/7442343-abs2343-21232421`

URI をより REST に適したものにするために、スキーマでは、`meta:altId` と呼ばれるプロパティで、ドット表記でエンコードされた URI を使用できます。

* `_xdm.context.profile`
* `_{TENANT_ID}.schemas.7442343-abs2343-21232421`

[!DNL Schema Registry] APIの呼び出しは、URLエンコードされた`$id` URIまたは`meta:altId` （ドット表記形式）をサポートします。 API への REST 呼び出しを実行する際には、URL エンコードされた `$id` URI を使用することをお勧めします。

* `https%3A%2F%2Fns.adobe.com%2Fxdm%2Fcontext%2Fprofile`
* `https%3A%2F%2Fns.adobe.com%2F{TENANT_ID}%2Fschemas%2F7442343-abs2343-21232421`

## Accept ヘッダー {#accept}

[!DNL Schema Registry] APIでリストおよびルックアップ(GET)操作を実行する場合、APIから返されるデータの形式を決定するには`Accept`ヘッダーが必要です。 特定のリソースを検索する場合は、`Accept`ヘッダーにバージョン番号も含める必要があります。

次の表のリストは、`Accept`のヘッダー値（バージョン番号を持つヘッダー値を含む）と、それらが使用された場合にAPIが返す内容に関する説明とに互換性があります。

| Accept | 説明 |
| ------- | ------------ |
| `application/vnd.adobe.xed-id+json` | ID のリストのみを返します。これは、リソースを一覧表示する際に最も使用される値です。 |
| `application/vnd.adobe.xed+json` | 元の `$ref` および `allOf` を含むフル JSON スキーマのリストを返します。これは、全リソースのリストを返す際に使用されます。 |
| `application/vnd.adobe.xed+json; version=1` | `$ref` と `allOf` を含む未処理の XDM です。タイトルと説明があります。 |
| `application/vnd.adobe.xed-full+json; version=1` | `$ref` 属性と解決された `allOf`。タイトルと説明があります。 |
| `application/vnd.adobe.xed-notext+json; version=1` | `$ref` と `allOf` を含む未処理の XDM です。タイトルや説明はありません。 |
| `application/vnd.adobe.xed-full-notext+json; version=1` | `$ref` 属性と解決された `allOf`。タイトルや説明はありません。 |
| `application/vnd.adobe.xed-full-desc+json; version=1` | `$ref` 属性と解決された `allOf`。記述子が含まれます。 |

>[!NOTE]
>
>プラットフォームは、現在、各スキーマ(`1`)に対して1つのメジャーバージョンのみをサポートしています。 したがって、スキーマの最新のマイナーバージョンを返すためには、ルックアップ要求を実行する際に`version`の値が常に`1`である必要があります。 スキーマのバージョン付けの詳細については、次のサブセクションを参照してください。

### スキーマのバージョン管理{#versioning}

スキーマバージョンは、スキーマレジストリAPIの`Accept`ヘッダーによって参照され、ダウンストリームプラットフォームサービスAPIペイロードの`schemaRef.contentType`プロパティで参照されます。

現在、プラットフォームは各スキーマに対して1つのメジャーバージョン(`1`)のみをサポートしています。 [スキーマ展開](../schema/composition.md#evolution)の規則に従えば、スキーマの更新は、それぞれ非破壊的でなければなりません。つまり、スキーマの新しいマイナーバージョン（`1.2`、`1.3`など） は、常に以前のマイナーバージョンとの下位互換性があります。 したがって、`version=1`を指定すると、スキーマレジストリは常にスキーマの&#x200B;**最新の**&#x200B;メジャーバージョン`1`を返します。つまり、以前のマイナーバージョンは返されません。

>[!NOTE]
>
>スキーマの展開に関する非破壊的な要件は、スキーマがデータセットから参照され、次のいずれかの場合にのみ適用されます。
>
>* データがデータセットに取り込まれました。
>* データセットは、リアルタイム顧客プロファイルでの使用が可能になっています（データが取り込まれていない場合でも可）。

>
>
上記の条件の1つを満たすデータセットにスキーマが関連付けられていない場合は、そのデータセットに対して変更を加えることができます。 ただし、いずれの場合も`version`コンポーネントは`1`に残ります。

## XDM フィールドの制約とベストプラクティス

スキーマのフィールドは、その `properties` オブジェクト内にリストされます。各フィールド自体はオブジェクトで、フィールドに格納できるデータを記述および制約する属性を含みます。

APIでのフィールドタイプの定義について詳しくは、このガイドの[フィールド制約ガイド](../schema/field-constraints.md)を参照してください。最も一般的に使用されるデータタイプのコードサンプルやオプション制約などがあります。

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
   * フィールドの種類は、レジストリで任意のデータタイプを使用して定義することもできます。詳しくは、『データ型エンドポイントガイド』の[データ型](./data-types.md#create)の作成に関する節を参照してください。
* `description` では、フィールドとフィールドデータについての関連情報を表します。スキーマにアクセスした人が誰でもフィールドの意図を理解できるように、明確な言葉で記述する必要があります。

APIで異なるフィールドタイプを定義する方法について詳しくは、[フィールド制約](../schema/field-constraints.md)のドキュメントを参照してください。

## 次の手順

[!DNL Schema Registry] APIを使用した呼び出しを開始するには、使用可能なエンドポイントガイドの1つを選択します。
