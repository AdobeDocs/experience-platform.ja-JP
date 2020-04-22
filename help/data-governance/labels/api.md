---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 'APIを使用したデータ使用ラベルの管理 '
topic: developer guide
translation-type: tm+mt
source-git-commit: d685f1851badf54ce1d1ac3cbacd69d62894c33f

---


# APIを使用したデータ使用ラベルの管理

このドキュメントでは、Catalog Service APIを使用して、データセットレベルおよびフィールドレベルでデータ使用ラベルを管理する手順を説明します。

## はじめに

このガイドを読む前に、Catalog Serviceの概要を読んで、サ [ービスのより強力な概要を](../../catalog/home.md) 「お読みになることをお勧めします。 また、カタログAPIを呼び出すために必要な資格情報を収集するには、 [Catalog開発者ガイドの](../../catalog/api/getting-started.md) 「はじめに」の節に記載されている手順に従う必要があります。

以下の節で説明するエンドポイントを呼び出すには、特定のデータセットに固有の値を `id` 持つ必要があります。 この値がない場合は、Catalogオブジェクトのリスト表示に関する開発者ガイドの [節を参照し](../../catalog/api/list-objects.md) 、既存のデータセットのIDを確認してください。

## データセットのラベルの検索 {#lookup}

GETリクエストを作成することで、既存のデータセットに適用されたデータ使用ラベルを調べることができます。

**API形式**

```http
GET /dataSets/{DATASET_ID}/labels
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | ラベル `id` を検索するデータセットの固有値。 |

**リクエスト**

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets/5abd49645591445e1ba04f87/labels' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した場合、データセットに適用されたデータ使用ラベルが返されます。

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
| `labels` | リストセットに適用されたデータ使用ラベルのデータです。 |
| `optionalLabels` | データセット内の個々のフィールドのリストで、データ使用ラベルが適用されています。 |

## データセットへのラベルの適用

POSTまたはPUTリクエストのペイロードにラベルを指定することで、データセットのラベルのセットを作成できます。 これらのいずれかの方法を使用すると、既存のラベルは上書きされ、ペイロードで提供されたラベルに置き換えられます。

**API形式**

```http
POST /dataSets/{DATASET_ID}/labels
PUT /dataSets/{DATASET_ID}/labels
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | ラベルを `id` 作成するデータセットの固有値。 |

**リクエスト**

次のPOSTリクエストでは、一連のラベルと、そのデータセット内の特定のフィールドをデータセットに追加します。 ペイロードに指定されるフィールドは、PUT要求に必要なフィールドと同じです。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/catalog/dataSets/5abd49645591445e1ba04f87/labels' \
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
| `labels` | リストセットに追加するデータ使用ラベルのデータです。 |
| `optionalLabels` | リストセット内でラベルを追加する個々のフィールドのデータ。 この配列の各アイテムには、次のプロパティが必要です。 <br/><br/>`option`:フィールドのエクスペリエンスデータモデル(XDM)属性を含むオブジェクトです。 次の3つのプロパティが必要です。<ul><li>id</code>:フィールドに関連付けられているスキーマのURI $id</code> 。</li><li>contentType</code>:コンテンツタイプとバージョン番号(スキーマ)。 これは、XDM参照要求に対して有効な <a href="../../xdm/api/look-up-resource.md">Acceptヘッダの</a> 1つの形式をとる必要があります。</li><li>schemaPath</code>:データセット内のフィールドへのパスです。スキーマ</li></ul>`labels`:フィールドにリストするデータ使用ラベルの追加。 |

**応答**

成功すると、データセットに追加されたラベルが返されます。

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

## データセットのラベルの削除

DELETEリクエストを行うことで、データセットに適用されたラベルを削除できます。

**API形式**

```http
DELETE /dataSets/{DATASET_ID}/labels
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | ラベルを `id` 削除するデータセットの一意の値。 |

**リクエスト**

```shell
curl -X DELETE \
  'https://platform.adobe.io/data/foundation/catalog/dataSets/5abd49645591445e1ba04f87/labels' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答HTTPステータス200(OK)。ラベルが削除されたことを示します。 データセッ [トの既存のラベルを別の呼び出しで](#lookup) 、調べて確認することができます。

## 次の手順

データセットレベルとフィールドレベルでデータ使用ラベルを追加したら、データをExperience Platformに取り込み始めます。 詳しくは、開始の取り込みに関するドキュメ [ントを参照してくださ](../../ingestion/home.md)い。

適用したラベルに基づいてデータ使用ポリシーを定義することもできます。 詳しくは、データ使用ポリシーの概 [要を参照してください](../policies/overview.md)。