---
keywords: Experience Platform；ホーム；人気の高いトピック；データガバナンス；データ使用ラベルapi；ポリシーサービスapi
solution: Experience Platform
title: 'APIを使用したデータ使用ラベルの管理 '
topic: 開発者ガイド
description: Dataset Service APIを使用すると、データセットの使用ラベルを適用および編集できます。 これはAdobe Experience Platformのデータカタログ機能の一部ですが、データセットメタデータを管理するCatalog Service APIとは別のものです。
translation-type: tm+mt
source-git-commit: 4e75e3fbdcd480c384411c2f33bad5b2cdcc5c42
workflow-type: tm+mt
source-wordcount: '1147'
ht-degree: 7%

---


# APIを使用したデータ使用ラベルの管理

このドキュメントでは、[!DNL Policy Service] APIと[!DNL Dataset Service] APIを使用してデータ使用ラベルを管理する手順を説明します。

[[!DNL Policy Service API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml)には、組織のデータ使用ラベルを作成および管理できるエンドポイントがいくつか用意されています。

[!DNL Dataset Service] APIを使用すると、データセットの使用ラベルを適用および編集できます。 これはAdobe Experience Platformのデータカタログ機能の一部ですが、データセットメタデータを管理する[!DNL Catalog Service] APIとは別のものです。

## はじめに

このガイドを読む前に、カタログ開発者ガイドの「[はじめに](../../catalog/api/getting-started.md)」に説明されている手順に従って、[!DNL Platform] APIを呼び出すために必要な資格情報を収集します。

このドキュメントで説明する[!DNL Dataset Service]エンドポイントを呼び出すには、特定のデータセットに対して一意の`id`値を持つ必要があります。 この値がない場合は、[カタログオブジェクト](../../catalog/api/list-objects.md)のリストを参照し、既存のデータセットのIDを確認してください。

## すべてのラベル{#list-labels}をリスト

[!DNL Policy Service] APIを使用すると、`/labels/core`または`/labels/custom`に対してGETリクエストを行うことで、すべての`core`ラベルまたは`custom`ラベルをリストできます。

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

正常に応答すると、システムから取得されたカスタムラベルのリストが返されます。 上記のリクエスト例は`/labels/custom`に対して行われたので、以下の応答はカスタムラベルのみを示しています。

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

## ラベルを検索{#look-up-label}

特定のラベルを検索するには、そのラベルの`name`プロパティを[!DNL Policy Service] APIへのGET要求のパスに含めます。

**API 形式**

```http
GET /labels/core/{LABEL_NAME}
GET /labels/custom/{LABEL_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{LABEL_NAME}` | 検索するカスタムラベルの`name`プロパティ。 |

**リクエスト**

次のリクエストは、パスに示されているカスタムラベル`L2`を取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/dulepolicy/labels/custom/L2' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常に応答すると、カスタムラベルの詳細が返されます。

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

## カスタムラベル{#create-update-label}の作成または更新

カスタムラベルを作成または更新するには、[!DNL Policy Service] APIにPUTリクエストを行う必要があります。

**API 形式**

```http
PUT /labels/custom/{LABEL_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{LABEL_NAME}` | カスタムラベルの`name`プロパティ。 この名前のカスタムラベルが存在しない場合は、新しいラベルが作成されます。 存在する場合は、そのラベルが更新されます。 |

**リクエスト**

次のリクエストは、顧客が選択した支払計画に関する情報を含むデータを記述するための新しいラベル`L3`を作成します。

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
| `name` | ラベルの一意の文字列識別子。 この値は参照目的で使用され、ラベルをデータセットやフィールドに適用するので、短く簡潔にすることをお勧めします。 |
| `category` | ラベルのカテゴリ。 カスタムラベル用に独自のカテゴリを作成することもできますが、ラベルをUIに表示する場合は`Custom`を使用することを強くお勧めします。 |
| `friendlyName` | ラベルのわかりやすい名前。表示目的で使用されます。 |
| `description` | （オプション）詳細なコンテキストを提供するラベルの説明です。 |

**応答**

正常な応答は、カスタムラベルの詳細を返します。既存のラベルが更新された場合はHTTPコード200(OK)、新しいラベルが作成された場合は201（作成済み）を返します。

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

## データセット{#look-up-dataset-labels}のラベルを検索します

[!DNL Dataset Service] APIにGETリクエストを行うことで、既存のデータセットに適用されているデータ使用ラベルを調べることができます。

**API 形式**

```http
GET /datasets/{DATASET_ID}/labels
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | 検索するラベルのデータセットの一意の`id`値。 |

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

成功した場合は、データセットに適用されたデータ使用量ラベルが返されます。

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
| `labels` | データセットに適用されたデータ使用量ラベルのリスト。 |
| `optionalLabels` | データセット内の個々のフィールドのリストで、データ使用ラベルが適用されています。 次のサブプロパティが必要です：<br/><br/>`option`:フィールドの[!DNL Experience Data Model] (XDM)属性を含むオブジェクト。 次の3つのプロパティが必要です。<ul><li>`id`:フィールドに関連付けられているスキーマのURI `$id` 値。</li><li>`contentType`:スキーマの形式とバージョンを示します。詳しくは、XDM APIガイドの[スキーマのバージョン管理](../../xdm/api/getting-started.md#versioning)の節を参照してください。</li><li>`schemaPath`:問題のスキーマプロパティへのパス。 [JSONポインター](../../landing/api-fundamentals.md#json-pointer) 構文で記述されています。</li></ul>`labels`:フィールドに追加するデータ使用ラベルのリストです。 |

- id:データセットの基となるXDMスキーマのURI $id値。
- contentType:スキーマの形式とバージョンを示します。 詳しくは、XDM APIガイドの[スキーマのバージョン管理](../../xdm/api/getting-started.md#versioning)の節を参照してください。
- schemaPath:問題のスキーマプロパティへのパス。[JSONポインター](../../landing/api-fundamentals.md#json-pointer)構文で記述されています。

## データセット{#apply-dataset-labels}にラベルを適用

[!DNL Dataset Service] APIへのPOSTまたはPUTリクエストのペイロードにラベルを提供することで、データセット用の一連のラベルを作成できます。 これらのいずれかの方法を使用すると、既存のラベルが上書きされ、ペイロードに指定されたラベルに置き換えられます。

**API 形式**

```http
POST /datasets/{DATASET_ID}/labels
PUT /datasets/{DATASET_ID}/labels
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | ラベルを作成するデータセットの一意の`id`値。 |

**リクエスト**

次のPOSTリクエストでは、一連のラベルと、そのデータセット内の特定のフィールドをデータセットに追加します。 ペイロードに指定されるフィールドは、PUT要求に必要なフィールドと同じです。

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
| `labels` | データセットに追加するデータ使用ラベルのリストです。 |
| `optionalLabels` | データセット内でラベルを追加する個々のフィールドのリスト。 この配列の各項目は、次のプロパティを持つ必要があります：<br/><br/>`option`:フィールドの[!DNL Experience Data Model] (XDM)属性を含むオブジェクト。 次の3つのプロパティが必要です。<ul><li>`id`:フィールドに関連付けられているスキーマのURI `$id` 値。</li><li>`contentType`:スキーマの形式とバージョンを示します。詳しくは、XDM APIガイドの[スキーマのバージョン管理](../../xdm/api/getting-started.md#versioning)の節を参照してください。</li><li>`schemaPath`:問題のスキーマプロパティへのパス。 [JSONポインター](../../landing/api-fundamentals.md#json-pointer) 構文で記述されています。</li></ul>`labels`:フィールドに追加するデータ使用ラベルのリストです。 |

**応答**

正常に完了すると、データセットに追加されたラベルが返されます。

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

## データセット{#remove-dataset-labels}からラベルを削除

[!DNL Dataset Service] APIにDELETEリクエストを行うと、データセットに適用されたラベルを削除できます。

**API 形式**

```http
DELETE /datasets/{DATASET_ID}/labels
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | ラベルを削除するデータセットの一意の`id`値。 |

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

成功した応答HTTPステータス200 (OK)。ラベルが削除されたことを示します。 別の呼び出しで、データセットの既存のラベル[を参照し、これを確認できます。](#look-up-dataset-labels)

## 次の手順

このドキュメントを読むことで、APIを使用したデータ使用ラベルの管理方法を学びました。

データセットレベルとフィールドレベルでデータ使用量ラベルを追加すると、[!DNL Experience Platform]にデータを取り込み始めることができます。 詳しくは、[データ取得ドキュメント](../../ingestion/home.md)を参照してください。

適用したラベルに基づいてデータ使用状況ポリシーを定義することもできます。詳しくは、「[データ使用状況ポリシーの概要](../policies/overview.md)」を参照してください。

[!DNL Experience Platform]でのデータセットの管理について詳しくは、[データセットの概要](../../catalog/datasets/overview.md)を参照してください。