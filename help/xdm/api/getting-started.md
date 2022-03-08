---
keywords: Experience Platform；ホーム；人気のトピック；API;XDM;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；スキーマレジストリ；スキーマレジストリ；
solution: Experience Platform
title: スキーマレジストリ API の概要
description: このドキュメントでは、スキーマレジストリ API を呼び出す前に知っておく必要があるコア概念の概要を示します。
topic-legacy: developer guide
exl-id: 7daebb7d-72d2-4967-b4f7-1886736db69f
source-git-commit: a26c8d43ff7874bcedd2adb3d6da995986198c96
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# [!DNL Schema Registry] API の概要

この [!DNL Schema Registry] API を使用すると、様々な Experience Data Model(XDM) リソースを作成および管理できます。 このドキュメントでは、[!DNL Schema Registry] API を呼び出す前に知っておく必要があるコア概念の概要を説明します。

## 前提条件

開発者ガイドを使用するには、Adobe Experience Platformの次のコンポーネントに関する十分な知識が必要です。

* [[!DNL Experience Data Model (XDM) System]](../home.md)：顧客体験データを編成する際に [!DNL Experience Platform] に使用される標準化されたフレームワーク。
   * [スキーマ構成の基本](../schema/composition.md)：XDM スキーマの基本的な構成要素について説明します。
* [[!DNL Real-time Customer Profile]](../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
* [[!DNL Sandboxes]](../../sandboxes/home.md): [!DNL Experience Platform] は、単一を分割する仮想サンドボックスを提供します [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立てます。

XDM では、JSON スキーマの形式を使用して、取り込んだ顧客体験データの構造を記述し、検証します。 したがって、 [公式の JSON スキーマドキュメント](https://json-schema.org/) この基盤となる技術をより深く理解するために

## API 呼び出し例の読み取り

[!DNL Schema Registry] API ドキュメントには、API 呼び出しの例とリクエストの形式を指定する方法が示されています。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

## 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

のすべてのリソース [!DNL Experience Platform]( [!DNL Schema Registry]は、特定の仮想サンドボックスに分離されています。 [!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

>[!NOTE]
>
>でのサンドボックスについて詳しくは、 [!DNL Platform]を参照し、 [サンドボックスドキュメント](../../sandboxes/home.md).

に対するすべての検索 (GET) リクエスト [!DNL Schema Registry] 追加の `Accept` ヘッダー。この値は、API から返される情報の形式を決定します。 詳しくは、この後の [Accept ヘッダー](#accept)の節を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 使用する TENANT_ID {#know-your-tenant_id}

API ガイド全体を通して、 `TENANT_ID`. この ID は、作成したリソースに名前空間が適切に設定されていること、またそのリソースが IMS 組織内に格納されていることを確認するために使用されます。ID が不明な場合は、次の GET リクエストを実行して ID にアクセスします。

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

成功時の応答は、組織の [!DNL Schema Registry]. この中には `TENANT_ID` の値である `tenantId` 属性が含まれています。

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
      "title": "Sample Field Group",
      "description": "New Sample Field Group.",
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
      "title": "Sample Field Group",
      "description": "New Sample Field Group.",
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

## `CONTAINER_ID` について  {#container}

への呼び出し [!DNL Schema Registry] API では `CONTAINER_ID`. API 呼び出しを実行できるコンテナは 2 つあります。の `global` コンテナと `tenant` コンテナ。

### グローバルコンテナ

この `global` コンテナにはすべての標準Adobeと [!DNL Experience Platform] パートナーが提供したクラス、スキーマフィールドグループ、データタイプ、スキーマ。 リストリクエストと参照 (GET) リクエストは、 `global` コンテナ。

を使用する呼び出しの例 `global` コンテナは次のようになります。

```http
GET /global/classes
```

### テナントコンテナ

ユニークな `TENANT_ID`、 `tenant` コンテナには、IMS 組織で定義されたすべてのクラス、フィールドグループ、データ型、スキーマ、および記述子が格納されます。 これらは各組織に固有のもので、他の IMS 組織では表示も管理もできません。で作成したリソースに対して、すべての CRUD 操作 (GET、POST、PUT、PATCH、DELETE) を実行できます。 `tenant` コンテナ。

を使用する呼び出しの例 `tenant` コンテナは次のようになります。

```http
POST /tenant/fieldgroups
```

クラス、フィールドグループ、スキーマ、データ型を `tenant` コンテナの場合は、 [!DNL Schema Registry] そして割り当てられた `$id` を含む URI `TENANT_ID`. この `$id` は、API 全体で特定のリソースを参照する際に使用されます。`$id` 値の例については、次の節で説明します。

## リソースの識別 {#resource-identification}

XDM リソースは、 `$id` 属性を URI 形式で記述します。次に例を示します。

* `https://ns.adobe.com/xdm/context/profile`
* `https://ns.adobe.com/{TENANT_ID}/schemas/7442343-abs2343-21232421`

URI をより REST に適したものにするために、スキーマでは、`meta:altId` と呼ばれるプロパティで、ドット表記でエンコードされた URI を使用できます。

* `_xdm.context.profile`
* `_{TENANT_ID}.schemas.7442343-abs2343-21232421`

への呼び出し [!DNL Schema Registry] API は、URL エンコードされた `$id` URI または `meta:altId` （ドット表記形式）。 API への REST 呼び出しを実行する際には、URL エンコードされた `$id` URI を使用することをお勧めします。

* `https%3A%2F%2Fns.adobe.com%2Fxdm%2Fcontext%2Fprofile`
* `https%3A%2F%2Fns.adobe.com%2F{TENANT_ID}%2Fschemas%2F7442343-abs2343-21232421`

## Accept ヘッダー {#accept}

リスト操作およびルックアップ (GET) 操作を [!DNL Schema Registry] API、 `Accept` API から返されるデータの形式を決定するには、ヘッダーが必要です。 特定のリソースを検索する場合は、バージョン番号も `Accept` ヘッダー。

次の表に、互換性のあるリストを示します `Accept` ヘッダー値（バージョン番号を持つヘッダー値を含む）と、それらを使用したときに API が返す内容の説明。

| Accept | 説明 |
| ------- | ------------ |
| `application/vnd.adobe.xed-id+json` | ID のリストのみを返します。これは、リソースを一覧表示する際に最も使用される値です。 |
| `application/vnd.adobe.xed+json` | 元の `$ref` および `allOf` を含むフル JSON スキーマのリストを返します。これは、全リソースのリストを返す際に使用されます。 |
| `application/vnd.adobe.xed+json; version=1` | `$ref` と `allOf` を含む未処理の XDM です。タイトルと説明があります。 |
| `application/vnd.adobe.xed-full+json; version=1` | `$ref` 属性と解決された `allOf`。タイトルと説明があります。 |
| `application/vnd.adobe.xed-notext+json; version=1` | `$ref` と `allOf` を含む未処理の XDM です。タイトルや説明はありません。 |
| `application/vnd.adobe.xed-full-notext+json; version=1` | `$ref` 属性と解決された `allOf`。タイトルや説明はありません。 |
| `application/vnd.adobe.xed-full-desc+json; version=1` | `$ref` 属性と解決された `allOf`。記述子が含まれます。 |

{style=&quot;table-layout:auto&quot;}

>[!NOTE]
>
>Platform は現在、各スキーマ (`1`) をクリックします。 したがって、 `version` は常に `1` 検索リクエストを実行して、スキーマの最新のマイナーバージョンを返す場合。 スキーマのバージョン管理について詳しくは、以下のサブセクションを参照してください。

### スキーマのバージョン管理 {#versioning}

スキーマのバージョンはで参照されています `Accept` スキーマレジストリ API および `schemaRef.contentType` ダウンストリーム Platform サービス API ペイロードのプロパティ。

現在、Platform は 1 つのメジャーバージョン (`1`) を使用します。 以下に従って： [スキーマ進化のルール](../schema/composition.md#evolution)に設定する場合、スキーマを更新するたびに非破壊的にする必要があります。つまり、スキーマの新しいマイナーバージョン (`1.2`, `1.3`など ) は、常に以前のマイナーバージョンとの後方互換性を維持します。 したがって、 `version=1`に値を指定しない場合、スキーマレジストリは常に **latest** メジャーバージョン `1` スキーマの。つまり、以前のマイナーバージョンは返されません。

>[!NOTE]
>
>スキーマの進化に対する非破壊的な要件は、スキーマがデータセットによって参照され、次のいずれかの場合に該当するときにのみ適用されます。
>
>* データがデータセットに取り込まれました。
>* データセットは、（データが取り込まれていない場合でも）リアルタイム顧客プロファイルで使用できるようになっています。
>
>上記の条件の 1 つを満たすデータセットにスキーマが関連付けられていない場合は、スキーマに対して任意の変更を加えることができます。 ただし、どの場合でも `version` コンポーネントは、まだ次の場所に残っています： `1`.

## XDM フィールドの制約とベストプラクティス

スキーマのフィールドは、その `properties` オブジェクト内にリストされます。各フィールド自体はオブジェクトで、フィールドに格納できるデータを記述および制約する属性を含みます。に関するガイドを参照してください。 [API でのカスタムフィールドの定義](../tutorials/custom-fields-api.md) 最も一般的に使用されるデータ型のコードサンプルとオプションの制約を参照してください。

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
   * フィールドの種類は、レジストリで任意のデータタイプを使用して定義することもできます。詳しくは、 [データ型の作成](./data-types.md#create) （データタイプエンドポイントガイド）を参照してください。
* `description` では、フィールドとフィールドデータについての関連情報を表します。スキーマにアクセスした人が誰でもフィールドの意図を理解できるように、明確な言葉で記述する必要があります。

## 次の手順

[!DNL Schema Registry] API を使用した呼び出しを開始するには、使用可能なエンドポイントガイドの 1 つを選択します。
