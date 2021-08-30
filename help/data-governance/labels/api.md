---
keywords: Experience Platform;ホーム;人気のトピック;データガバナンス;データ使用ラベル API;Policy Service API
solution: Experience Platform
title: 'API を使用したデータ使用ラベルの管理 '
topic-legacy: developer guide
description: Dataset Service API を使用すると、データセットの使用ラベルを適用および編集できます。これは Adobe Experience Platform のデータカタログ機能の一部ですが、データセットメタデータを管理する Catalog Service API とは別のものです。
source-git-commit: 8133804076b1c0adf2eae5b748e86a35f3186d14
workflow-type: tm+mt
source-wordcount: '1141'
ht-degree: 99%

---


# API を使用したデータ使用ラベルの管理

このドキュメントでは、[!DNL Policy Service] API と [!DNL Dataset Service] APIを使用してデータ使用ラベルを管理する手順を説明します。

[[!DNL Policy Service API]](https://www.adobe.io/experience-platform-apis/references/policy-service/) には、組織のデータ使用ラベルを作成および管理できるエンドポイントがいくつか用意されています。

[!DNL Dataset Service] API を使用すると、データセットの使用ラベルを適用および編集できます。これは Adobe Experience Platform のデータカタログ機能の一部ですが、データセットメタデータを管理する [!DNL Catalog Service] API とは別のものです。

## はじめに

このガイドを読む前に、カタログ開発者ガイドの「[はじめに](../../catalog/api/getting-started.md)」で説明されている手順に従って、[!DNL Platform] API を呼び出すために必要な資格情報を収集します。

このドキュメントで説明する [!DNL Dataset Service] エンドポイントを呼び出すには、特定のデータセットに対する一意の `id` 値が必要です。この値がない場合は、[カタログオブジェクトのリスト](../../catalog/api/list-objects.md)に関するガイドを参照し、既存のデータセットの ID を確認してください。

## すべてのラベルをリストする {#list-labels}

[!DNL Policy Service] API を使用し、`/labels/core` または `/labels/custom` に対して GET リクエストをおこなうことで、すべての `core` ラベルまたは `custom` ラベルをリストできます。

**API 形式**

```http
GET /labels/core
GET /labels/custom
```

**リクエスト**

次のリクエストでは、組織で作成されたすべてのカスタムラベルをリストします。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/dulepolicy/labels/custom' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

応答が成功すると、システムから取得したカスタムラベルのリストが返されます。上記のリクエスト例は `/labels/custom` に対しておこなわれたので、以下の応答はカスタムラベルのみを示しています。

```json
{
    "_page": {
        "count": 2
    },
    "_links": {
        "page": {
            "href": "https://platform.adobe.io:443/data/foundation/dulepolicy/labels/custom?{?limit,start,property}",
            "templated": true
        }
    },
    "children": [
        {
            "name": "L1",
            "category": "Custom",
            "friendlyName": "Banking Information",
            "description": "Data containing banking information for a customer.",
            "imsOrg": "{IMS_ORG}",
            "sandboxName": "{SANDBOX_NAME}",
            "created": 1594396718731,
            "createdClient": "{CLIENT_ID}",
            "createdUser": "{USER_ID}",
            "updated": 1594396718731,
            "updatedClient": "{CLIENT_ID}",
            "updatedUser": "{USER_ID}",
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io:443/data/foundation/dulepolicy/labels/custom/L1"
                }
            }
        },
        {
            "name": "L2",
            "category": "Custom",
            "friendlyName": "Purchase History Data",
            "description": "Data containing information on past transactions",
            "imsOrg": "{IMS_ORG}",
            "sandboxName": "{SANDBOX_NAME}",
            "created": 1594397415663,
            "createdClient": "{CLIENT_ID}",
            "createdUser": "{USER_ID}",
            "updated": 1594397728708,
            "updatedClient": "{CLIENT_ID}",
            "updatedUser": "{USER_ID}",
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io:443/data/foundation/dulepolicy/labels/custom/L2"
                }
            }
        }
    ]
}
```

## ラベルの検索 {#look-up-label}

特定のラベルを検索するには、そのラベルの `name` プロパティを [!DNL Policy Service] API への GET リクエストのパスに含めます。

**API 形式**

```http
GET /labels/core/{LABEL_NAME}
GET /labels/custom/{LABEL_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{LABEL_NAME}` | 検索するサンドボックスの `name` プロパティ。 |

**リクエスト**

次のリクエストは、パスに示されているカスタムラベル `L2` を取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/dulepolicy/labels/custom/L2' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

応答が成功すると、カスタムラベルの詳細が返されます。

```json
{
    "name": "L2",
    "category": "Custom",
    "friendlyName": "Purchase History Data",
    "description": "Data containing information on past transactions",
    "imsOrg": "{IMS_ORG}",
    "sandboxName": "{SANDBOX_NAME}",
    "created": 1594397415663,
    "createdClient": "{CLIENT_ID}",
    "createdUser": "{USER_ID}",
    "updated": 1594397728708,
    "updatedClient": "{CLIENT_ID}",
    "updatedUser": "{USER_ID}",
    "_links": {
        "self": {
            "href": "https://platform.adobe.io:443/data/foundation/dulepolicy/labels/custom/L2"
        }
    }
}
```

## カスタムラベル の作成または更新 {#create-update-label}

カスタムラベルを作成または更新するには、[!DNL Policy Service] API に PUT リクエストをおこなう必要があります。

**API 形式**

```http
PUT /labels/custom/{LABEL_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{LABEL_NAME}` | カスタムラベルの `name` プロパティ。この名前のカスタムラベルが存在しない場合は、新しいラベルが作成されます。存在する場合は、そのラベルが更新されます。 |

**リクエスト**

次のリクエストは、顧客が選択した支払計画に関する情報を含むデータを記述するための新しいラベル `L3` を作成します。

```shell
curl -X PUT \
  'https://platform.adobe.io/data/foundation/dulepolicy/labels/custom/L3' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "name": "L3",
        "category": "Custom",
        "friendlyName": "Payment Plan",
        "description": "Data containing information on selected payment plans."
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | ラベルの一意の文字列識別子。この値は検索目的で使用され、ラベルをデータセットとフィールドに適用します。そのため、短く簡潔にすることをお勧めします。 |
| `category` | ラベルのカテゴリ。カスタムラベル用に独自のカテゴリを作成することもできますが、ラベルを UI に表示する場合は `Custom` を使用することを強くお勧めします。 |
| `friendlyName` | 表示目的で使用される、ラベルのわかりやすい名前。 |
| `description` | （オプション）詳細なコンテキストを提供するラベルの説明です。 |

**応答**

応答が成功すると、カスタムラベルの詳細が返されます。既存のラベルが更新された場合は HTTP コード 200（OK）、新しいラベルが作成された場合は 201（作成済み）が返されます。

```json
{
  "name": "L3",
  "category": "Custom",
  "friendlyName": "Payment Plan",
  "description": "Data containing information on selected payment plans.",
  "imsOrg": "{IMS_ORG}",
  "sandboxName": "{SANDBOX_NAME}",
  "created": 1529696681413,
  "createdClient": "{CLIENT_ID}",
  "createdUser": "{USER_ID}",
  "updated": 1529697651972,
  "updatedClient": "{CLIENT_ID}",
  "updatedUser": "{USER_ID}",
  "_links": {
    "self": {
      "href": "https://platform.adobe.io:443/data/foundation/dulepolicy/labels/custom/L3"
    }
  }
}
```

## データセットのラベルの検索 {#look-up-dataset-labels}

[!DNL Dataset Service] API に GET リクエストをおこなうことで、既存のデータセットに適用されているデータ使用ラベルを調べることができます。

**API 形式**

```http
GET /datasets/{DATASET_ID}/labels
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | 検索するラベルのデータセットの一意の `id` 値。 |

**リクエスト**

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/dataset/datasets/5abd49645591445e1ba04f87/labels' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、データセットに適用されたデータ使用ラベルを返します。

```json
{
  "AEP:dataset:5abd49645591445e1ba04f87": {
    "imsOrg": "{IMS_ORG}",
    "labels": [ "C1", "C2", "C3", "I1", "I2" ],
    "optionalLabels": [
      {
        "option": {
          "id": "https://ns.adobe.com/{TENANT_ID}/schemas/c6b1b09bc3f2ad2627c1ecc719826836",
          "contentType": "application/vnd.adobe.xed-full+json;version=1",
          "schemaPath": "/properties/repositoryCreatedBy"
        },
        "labels": [ "S1", "S2" ]
      }
    ]
  }
}
```

| プロパティ | 説明 |
| --- | --- |
| `labels` | データセットに適用されたデータ使用ラベルのリスト。 |
| `optionalLabels` | データ使用ラベルが適用されたデータセット内の個々のフィールドのリスト。次のサブプロパティが必要です。<br/><br/>`option`：フィールドの[!DNL Experience Data Model] （XDM）属性を含むオブジェクト。次の 3 つのプロパティが必要です。<ul><li>`id`：フィールドに関連付けられているスキーマの URI `$id` 値。</li><li>`contentType`：スキーマの形式とバージョンを示します。詳しくは、XDM API ガイドの[スキーマのバージョン管理](../../xdm/api/getting-started.md#versioning)の節を参照してください。</li><li>`schemaPath`：問題のスキーマプロパティへのパス（[JSON ポインター](../../landing/api-fundamentals.md#json-pointer)構文で記述）。</li></ul>`labels`：フィールドに追加するデータ使用ラベルのリスト。 |

- id：データセットの基となる XDM スキーマの URI $id 値。
- contentType：スキーマの形式とバージョンを示します。詳しくは、XDM API ガイドの[スキーマのバージョン管理](../../xdm/api/getting-started.md#versioning)の節を参照してください。
- schemaPath：問題のスキーマプロパティへのパス（[JSON ポインター](../../landing/api-fundamentals.md#json-pointer)構文で記述）。

## データセットにラベルを適用する {#apply-dataset-labels}

[!DNL Dataset Service] API への POST または PUT リクエストのペイロードにラベルを提供することで、データセット用の一連のラベルを作成できます。これらのいずれかの方法を使用すると、既存のラベルが上書きされ、ペイロードに指定されたラベルに置き換えられます。

**API 形式**

```http
POST /datasets/{DATASET_ID}/labels
PUT /datasets/{DATASET_ID}/labels
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | ラベルを作成するデータセットの一意の `id` 値。 |

**リクエスト**

次の POST リクエストでは、一連のラベルと、そのデータセット内の特定のフィールドをデータセットに追加します。ペイロードに指定されるフィールドは、PUT リクエストに必要なフィールドと同じです。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/dataset/datasets/5abd49645591445e1ba04f87/labels' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
        "labels": [ "C1", "C2", "C3", "I1", "I2" ],
        "optionalLabels": [
          {
            "option": {
              "id": "https://ns.adobe.com/{TENANT_ID}/schemas/c6b1b09bc3f2ad2627c1ecc719826836",
              "contentType": "application/vnd.adobe.xed-full+json;version=1",
              "schemaPath": "/properties/repositoryCreatedBy"
            },
            "labels": [ "S1", "S2" ]
          }
        ]
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `labels` | データセットに追加するデータ使用ラベルのリスト。 |
| `optionalLabels` | ラベルを追加するデータセット内の個々のフィールドのリスト。この配列の各アイテムは、次のプロパティを持つ必要があります。<br/><br/>`option`：フィールドの [!DNL Experience Data Model]（XDM）属性を含むオブジェクト。次の 3 つのプロパティが必要です。<ul><li>`id`：フィールドに関連付けられているスキーマの URI `$id` 値。</li><li>`contentType`：スキーマの形式とバージョンを示します。詳しくは、XDM API ガイドの[スキーマのバージョン管理](../../xdm/api/getting-started.md#versioning)の節を参照してください。</li><li>`schemaPath`：問題のスキーマプロパティへのパス（[JSON ポインター](../../landing/api-fundamentals.md#json-pointer)構文で記述）。</li></ul>`labels`：フィールドに追加するデータ使用ラベルのリスト。 |

**応答** 

応答が成功すると、データセットに追加されたラベルが返されます。

```json
{
  "labels": [ "C1", "C2", "C3", "I1", "I2" ],
  "optionalLabels": [
    {
      "option": {
        "id": "https://ns.adobe.com/{TENANT_ID}/schemas/c6b1b09bc3f2ad2627c1ecc719826836",
        "contentType": "application/vnd.adobe.xed-full+json;version=1",
        "schemaPath": "/properties/repositoryCreatedBy"
      },
      "labels": [ "S1", "S2" ]
    }
  ]
}
```

## データセットからラベルを削除する {#remove-dataset-labels}

[!DNL Dataset Service] API に DELETE リクエストをおこなうと、データセットに適用されたラベルを削除できます。

**API 形式**

```http
DELETE /datasets/{DATASET_ID}/labels
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | ラベルを削除するデータセットの一意の `id` 値。 |

**リクエスト**

```shell
curl -X DELETE \
  'https://platform.adobe.io/data/foundation/dataset/datasets/5abd49645591445e1ba04f87/labels' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、ラベルが削除されたことを示す HTTP ステータス 200（OK）が返されます。別の呼び出しで、データセットの[既存のラベルを参照](#look-up-dataset-labels)し、これを確認できます。

## 次の手順

このドキュメントでは、API を使用したデータ使用ラベルの管理方法を学びました。

データセットレベルとフィールドレベルでデータ使用ラベルを追加したら、データを [!DNL Experience Platform] に取り込み始めることができます。詳しくは、[データ取得ドキュメント](../../ingestion/home.md)を参照してください。

適用したラベルに基づいてデータ使用状況ポリシーを定義することもできます。詳しくは、「[データ使用状況ポリシーの概要](../policies/overview.md)」を参照してください。

[!DNL Experience Platform] でのデータセットの管理について詳しくは、[データセットの概要](../../catalog/datasets/overview.md)を参照してください。