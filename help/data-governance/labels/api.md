---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 'APIを使用したデータ使用ラベルの管理 '
topic: developer guide
translation-type: tm+mt
source-git-commit: 1fce86193bc1660d0f16408ed1b9217368549f6c
workflow-type: tm+mt
source-wordcount: '610'
ht-degree: 3%

---


# APIを使用したデータ使用ラベルの管理

Dataset Service APIを使用すると、データセットの使用ラベルをプログラムで管理できます。 これは、Adobe Experience Platformのデータカタログ機能の一部ですが、データセットメタデータを管理するCatalog Service APIとは別のものです。

このドキュメントでは、Dataset Service APIを使用して、データセットレベルおよびフィールドレベルでデータ使用量ラベルを管理する方法について手順を説明します。

## はじめに

このガイドを読む前に、カタログ開発者ガイドの [はじめに節に説明されている手順に従って](../../catalog/api/getting-started.md) 、APIを呼び出すために必要な資格情報を収集し [!DNL Platform] ます。

以下の節で説明するエンドポイントを呼び出すには、特定のデータセットに固有の `id` 値を持つ必要があります。 この値がない場合は、カタログオブジェクトの [一覧表示に関するガイドを参照して](../../catalog/api/list-objects.md) 、既存のデータセットのIDを確認してください。

## データセットのラベルを検索する {#lookup}

GETリクエストを作成して、既存のデータセットに適用されているデータ使用量ラベルを調べることができます。

**API形式**

```http
GET /datasets/{DATASET_ID}/labels
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | ラベルを調べるデータセットの固有 `id` 値。 |

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
| `optionalLabels` | データセット内の個々のフィールドのリストで、データ使用ラベルが適用されています。 |

## データセットへのラベルの適用

POSTまたはPUTリクエストのペイロードにラベルを提供することで、データセット用の一連のラベルを作成できます。 これらのいずれかの方法を使用すると、既存のラベルが上書きされ、ペイロードに指定されたラベルに置き換えられます。

**API形式**

```http
POST /datasets/{DATASET_ID}/labels
PUT /datasets/{DATASET_ID}/labels
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | ラベルを作成するデータセットの固有 `id` 値。 |

**リクエスト**

次のPOSTリクエストでは、一連のラベルと、そのデータセット内の特定のフィールドを追加します。 ペイロードに指定されるフィールドは、PUT要求に必要なフィールドと同じです。

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
| `optionalLabels` | データセット内でラベルを追加する個々のフィールドのリスト。 この配列の各アイテムは、次のプロパティを持つ必要があります。 <br/><br/>`option`: フィールドのエクスペリエンスデータモデル(XDM)属性を含むオブジェクトです。 次の3つのプロパティが必要です。<ul><li>id</code>: フィールドに関連付けられているスキーマのURI $id</code> 。</li><li>contentType</code>: スキーマのコンテンツタイプとバージョン番号。 これは、XDMルックアップ要求に対して有効な <a href="../../xdm/api/look-up-resource.md">Acceptヘッダーの1つの形式にする必要があります</a> 。</li><li>schemaPath</code>: データセットのスキーマ内のフィールドへのパス。</li></ul>`labels`: フィールドに追加するデータ使用ラベルのリストです。 |

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

## データセットのラベルの削除

DELETEリクエストを行うと、データセットに適用されたラベルを削除できます。

**API形式**

```http
DELETE /datasets/{DATASET_ID}/labels
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | ラベルを削除するデータセットの固有 `id` 値。 |

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

成功した応答HTTPステータス200 (OK)。ラベルが削除されたことを示します。 別の呼び出しで、データセットの既存のラベルを [調べて](#lookup) 、これを確認できます。

## 次の手順

データセットレベルとフィールドレベルでデータ使用量ラベルを追加したら、Experience Platformにデータを取り込み始めます。 詳しくは、 [データ取り込みに関するドキュメントを参照して開始](../../ingestion/home.md)。

適用したラベルに基づいてデータ使用ポリシーを定義できるようになりました。 詳しくは、「 [データ使用ポリシーの概要](../policies/overview.md)」を参照してください。

でのデータセットの管理について詳し [!DNL Experience Platform]くは、「 [データセットの概要](../../catalog/datasets/overview.md)」を参照してください。