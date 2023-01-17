---
keywords: Experience Platform;ホーム人気のトピック;データセット api;データ使用の管理;データ使用 api
solution: Experience Platform
title: API を使用したデータセットのデータ使用ラベルの管理
description: Dataset Service API を使用すると、データセットの使用ラベルを適用および編集できます。これは Adobe Experience Platform のデータカタログ機能の一部ですが、データセットメタデータを管理する Catalog Service API とは別のものです。
exl-id: 24a8d870-eb81-4255-8e47-09ae7ad7a721
source-git-commit: 7b15166ae12d90cbcceb9f5a71730bf91d4560e6
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 100%

---

# API を使用したデータセットのデータ使用ラベルの管理

[[!DNL Dataset Service API]](https://www.adobe.io/experience-platform-apis/references/dataset-service/) では、データセットの使用ラベルを適用および編集できます。これは Adobe Experience Platform のデータカタログ機能の一部ですが、データセットメタデータを管理する [!DNL Catalog Service] API とは別のものです。

>[!IMPORTANT]
>
>データセットレベルでのラベルの適用は、データガバナンスのユースケースでのみサポートされています。データのアクセスポリシーを作成しようとしている場合は、データセットが基づいている[スキーマにラベルを適用](../../xdm/tutorials/labels.md)する必要があります。詳しくは、[属性ベースのアクセス制御](../../access-control/abac/overview.md)の概要を参照してください。

このドキュメントでは、[!DNL Dataset Service API] を使用してデータセットとフィールドのラベルを管理する方法を説明します。API 呼び出しを使用してデータ使用ラベル自体を管理する手順については、[!DNL Policy Service API] の[ラベルエンドポイントガイド](../api/labels.md)を参照してください。

## はじめに

このガイドを読む前に、カタログ開発者ガイドの「[はじめに](../../catalog/api/getting-started.md)」で説明されている手順に従って、[!DNL Platform] API を呼び出すために必要な資格情報を収集します。

このドキュメントで説明しているエンドポイントを呼び出すには、特定のデータセットに対して一意の `id` 値を持つ必要があります。この値がない場合は、[カタログオブジェクトのリスト](../../catalog/api/list-objects.md)に関するガイドを参照し、既存のデータセットの ID を確認してください。

## データセットのラベルの検索 {#look-up}

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、データセットに適用されたデータ使用ラベルを返します。

```json
{
  "AEP:dataset:5abd49645591445e1ba04f87": {
    "imsOrg": "{ORG_ID}",
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
| `optionalLabels` | データ使用ラベルが適用されたデータセット内の個々のフィールドのリスト。 |

## データセットにラベルを適用する {#apply}

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

以下の PUT リクエストの例では、データセットの既存のラベルと、そのデータセット内の特定のフィールドを更新します。ペイロードで提供されるフィールドは、POST リクエストで必要とされるものと同じです。

データセット（PUT）の既存のラベルを更新する API 呼び出しを行う場合、データセットサービスのデータセットラベルエンティティの現在のバージョンを示す `If-Match` ヘッダーを含める必要があります。データの競合を防ぐため、含まれる `If-Match` 文字列が、そのデータセットのシステムで生成される最新のバージョンタグと一致する場合にのみ、データセットエンティティが更新されます。

>[!NOTE]
>
>現在、該当するデータセットにラベルが存在しない場合、新しいラベルは POST リクエストを通じてのみ追加でき、`If-Match` ヘッダーは不要です。ラベルがデータセットに追加されると、`etag` 値が割り当てられ、後でラベルの更新や削除に使用できます。

データセットラベルエンティティの最新バージョンを取得するには、`/datasets/{DATASET_ID}/labels` エンドポイントに対して [GET リクエスト](#look-up)を実行します。現在の値は、応答の `etag` ヘッダーの下で返されます。既存のデータセットラベルを更新する場合、ベストプラクティスは、最新の `etag` 値を取得するために、最初にデータセットのルックアップリクエストを実行してから、後続の PUT リクエストの `If-Match` ヘッダーでその値を使用することです。

```shell
curl -X PUT \
  'https://platform.adobe.io/data/foundation/dataset/datasets/5abd49645591445e1ba04f87/labels' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `labels` | データセットに追加するデータ使用ラベルのリスト。 |
| `optionalLabels` | ラベルを追加するデータセット内の個々のフィールドのリスト。この配列の各アイテムは、次のプロパティを持つ必要があります。<br/><br/>`option`：フィールドの [!DNL Experience Data Model]（XDM）属性を含むオブジェクト。次の 3 つのプロパティが必要です。<ul><li>id</code>：フィールドに関連付けられているスキーマのURI $id</code>値。</li><li>contentType</code>：スキーマのコンテンツタイプとバージョン番号。XDM ルックアップリクエストに対しては、有効な <a href="../../xdm/api/getting-started.md#accept">Accept ヘッダー</a>のいずれかの形式でおこなう必要があります。</li><li>schemaPath</code>：データセットのスキーマ内のフィールドへのパス。</li></ul>`labels`：フィールドに追加するデータ使用ラベルのリスト。 |

**応答** 

応答が成功すると、データセットの更新されたラベルのセットが返されます。

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

## 次の手順

このドキュメントでは、[!DNL Dataset Service] APIを使用してデータセットとフィールドのデータ使用ラベルを管理する方法について説明しました。適用したラベルに基づいて、[データ使用ポリシー](../policies/overview.md)と[アクセス制御ポリシー](../../access-control/abac/ui/policies.md)を定義できるようになりました。

[!DNL Experience Platform] でのデータセットの管理について詳しくは、[データセットの概要](../../catalog/datasets/overview.md)を参照してください。
