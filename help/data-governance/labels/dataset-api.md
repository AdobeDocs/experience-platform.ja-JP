---
keywords: Experience Platform；ホーム；人気の高いトピック；データセットapi；データ使用の管理；データ使用api
solution: Experience Platform
title: 'APIを使用したデータセットのデータ使用ラベルの管理 '
topic-legacy: developer guide
description: Dataset Service APIを使用すると、データセットの使用ラベルを適用および編集できます。 これはAdobe Experience Platformのデータカタログ機能の一部ですが、データセットメタデータを管理するCatalog Service APIとは別のものです。
exl-id: 24a8d870-eb81-4255-8e47-09ae7ad7a721
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '961'
ht-degree: 6%

---

# APIを使用したデータセットのデータ使用ラベルの管理

[[!DNL Dataset Service API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dataset-service.yaml)では、データセットの使用ラベルを適用および編集できます。 これはAdobe Experience Platformのデータカタログ機能の一部ですが、データセットメタデータを管理する[!DNL Catalog Service] APIとは別のものです。

このドキュメントでは、[!DNL Dataset Service API]を使用してデータセットとフィールドのラベルを管理する方法を説明します。 API呼び出しを使用してデータ使用ラベル自体を管理する手順については、[!DNL Policy Service API]の[ラベルエンドポイントガイド](../api/labels.md)を参照してください。

## はじめに

このガイドを読む前に、カタログ開発者ガイドの「[はじめに](../../catalog/api/getting-started.md)」に説明されている手順に従って、[!DNL Platform] APIを呼び出すために必要な資格情報を収集します。

このドキュメントで概要を説明しているエンドポイントを呼び出すには、特定のデータセットに対して一意の`id`値を持つ必要があります。 この値がない場合は、[カタログオブジェクト](../../catalog/api/list-objects.md)のリストを参照し、既存のデータセットのIDを確認してください。

## データセット{#look-up}のラベルを検索します

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
| `optionalLabels` | データセット内の個々のフィールドのリストで、データ使用ラベルが適用されています。 |

## データセット{#apply}にラベルを適用

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

次のPUTリクエストでは、データセットの既存のラベルと、そのデータセット内の特定のフィールドを更新します。 ペイロードに指定されるフィールドは、POST要求に必要なフィールドと同じです。

>[!IMPORTANT]
>
>`/datasets/{DATASET_ID}/labels`エンドポイントにPUTリクエストを行う場合は、有効な`If-Match`ヘッダーを指定する必要があります。 必要なヘッダの使い方の詳細については、[付録](#if-match)を参照してください。

```shell
curl -X PUT \
  'https://platform.adobe.io/data/foundation/dataset/datasets/5abd49645591445e1ba04f87/labels' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -H 'If-Match: 8f00d38e-0000-0200-0000-5ef4fc6d0000' \
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
| `optionalLabels` | データセット内でラベルを追加する個々のフィールドのリスト。 この配列の各アイテムは、次のプロパティを持つ必要があります。<br/><br/>`option`:フィールドの[!DNL Experience Data Model] (XDM)属性を含むオブジェクト。 次の3つのプロパティが必要です。<ul><li>id</code>:フィールドに関連付けられているスキーマのURI $id</code>値。</li><li>contentType</code>:スキーマのコンテンツタイプとバージョン番号。 XDMルックアップ要求に対しては、有効な<a href="../../xdm/api/getting-started.md#accept">Accept headers</a>のいずれかの形式で行う必要があります。</li><li>schemaPath</code>:データセットのスキーマ内のフィールドへのパス。</li></ul>`labels`:フィールドに追加するデータ使用ラベルのリストです。 |

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

## データセット{#remove}からラベルを削除

[!DNL Dataset Service] APIにDELETEリクエストを行うと、データセットに適用されたラベルを削除できます。

**API 形式**

```http
DELETE /datasets/{DATASET_ID}/labels
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | ラベルを削除するデータセットの一意の`id`値。 |

**リクエスト**

次のリクエストでは、パスで指定されたデータセットのラベルを削除します。

>[!IMPORTANT]
>
>`/datasets/{DATASET_ID}/labels`エンドポイントにDELETEリクエストを行う場合は、有効な`If-Match`ヘッダーを指定する必要があります。 必要なヘッダの使い方の詳細については、[付録](#if-match)を参照してください。

```shell
curl -X DELETE \
  'https://platform.adobe.io/data/foundation/dataset/datasets/5abd49645591445e1ba04f87/labels' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'If-Match: 8f00d38e-0000-0200-0000-5ef4fc6d0000'
```

**応答**

成功した応答HTTPステータス200 (OK)。ラベルが削除されたことを示します。 別の呼び出しで、データセットの既存のラベル[を参照し、これを確認できます。](#look-up)

## 次の手順

このドキュメントを読むことで、[!DNL Dataset Service] APIを使用してデータセットとフィールドのデータ使用ラベルを管理する方法を学びました。

データセットレベルとフィールドレベルでデータ使用量ラベルを追加すると、[!DNL Experience Platform]にデータを取り込み始めることができます。 詳しくは、[データ取得ドキュメント](../../ingestion/home.md)を参照してください。

適用したラベルに基づいてデータ使用状況ポリシーを定義することもできます。詳しくは、「[データ使用状況ポリシーの概要](../policies/overview.md)」を参照してください。

[!DNL Experience Platform]でのデータセットの管理について詳しくは、[データセットの概要](../../catalog/datasets/overview.md)を参照してください。

## 付録 {#appendix}

Dataset Service APIを使用したラベルの操作について詳しくは、次の節に説明します。

### [!DNL If-Match] ヘッダー {#if-match}

データセットの既存のラベル(PUTとDELETE)を更新するAPI呼び出しを行う場合は、Dataset Serviceのデータセットラベルエンティティの現在のバージョンを示す`If-Match`ヘッダーを含める必要があります。 データの競合を防ぐため、含まれる`If-Match`文字列が、そのデータセットのシステムで生成される最新のバージョンタグと一致する場合にのみ、データセットエンティティが更新されます。

>[!NOTE]
>
>現在、該当するデータセットにラベルが存在しない場合、新しいラベルはPOSTリクエストを通じてのみ追加でき、`If-Match`ヘッダーは不要です。 ラベルがデータセットに追加されると、`etag`値が割り当てられ、後でラベルの更新や削除に使用できます。

dataset-labelエンティティの最新バージョンを取得するには、[GETリクエスト](#look-up)を`/datasets/{DATASET_ID}/labels`エンドポイントに送信します。 現在の値は、応答の`etag`ヘッダーの下に返されます。 既存のデータセットラベルを更新する場合、最新の`etag`値を取得する前に、その値を後続のPUTまたはDELETEリクエストの`If-Match`ヘッダーで使用するために、まずデータセットのルックアップリクエストを実行することをお勧めします。
