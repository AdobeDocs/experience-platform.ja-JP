---
keywords: Experience Platform;ホーム人気のトピック;データセット api;データ使用の管理;データ使用 api
solution: Experience Platform
title: API を使用したデータセットのデータ使用ラベルの管理
description: Dataset Service API を使用すると、データセットの使用ラベルを適用および編集できます。これは Adobe Experience Platform のデータカタログ機能の一部ですが、データセットメタデータを管理する Catalog Service API とは別のものです。
exl-id: 24a8d870-eb81-4255-8e47-09ae7ad7a721
source-git-commit: 8db484e4a65516058d701ca972fcbcb6b73abb31
workflow-type: tm+mt
source-wordcount: '1318'
ht-degree: 93%

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

[!DNL Dataset Service] API への POST または PUT リクエストのペイロードにラベルのセットを指定することで、データセット全体にラベルのセットを適用できます。リクエスト本文は、両方の呼び出しで同じです。個々のデータセットフィールドにラベルを追加することはできません。

**API 形式**

```http
POST /datasets/{DATASET_ID}/labels
PUT /datasets/{DATASET_ID}/labels
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | ラベルを作成するデータセットの一意の `id` 値。 |

**リクエスト**

以下の POST リクエストの例では、データセット全体を `C1` ラベルで更新します。ペイロードに指定されるフィールドは、PUT リクエストに必要なフィールドと同じです。

データセット（PUT）の既存のラベルを更新する API 呼び出しを行う場合、データセットサービスのデータセットラベルエンティティの現在のバージョンを示す `If-Match` ヘッダーを含める必要があります。データの競合を防ぐため、含まれる `If-Match` 文字列が、そのデータセットのシステムで生成される最新のバージョンタグと一致する場合にのみ、データセットエンティティが更新されます。

>[!NOTE]
>
>現在、該当するデータセットにラベルが存在する場合、新しいラベルは PUT リクエストを通じてのみ追加でき、これには `If-Match` ヘッダーが必要です。データセットにラベルが追加されると、最新の `etag` 後でラベルを更新または削除するには、値が必要です<br>PUTメソッドを実行する前に、GETセットラベルに対してデータリクエストを実行する必要があります。 リクエストでの変更を目的とした特定のフィールドのみを更新し、残りは変更しないでください。 さらに、PUT呼び出しで、GET呼び出しと同じ親エンティティが保持されていることを確認します。 不一致が生じると、お客様にエラーが発生します。

データセットラベルエンティティの最新バージョンを取得するには、`/datasets/{DATASET_ID}/labels` エンドポイントに対して [GET リクエスト](#look-up)を実行します。現在の値は、応答の `etag` ヘッダーの下で返されます。既存のデータセットラベルを更新する場合、ベストプラクティスは、最新の `etag` 値を取得するために、最初にデータセットのルックアップリクエストを実行してから、後続の PUT リクエストの `If-Match` ヘッダーでその値を使用することです。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/dataset/datasets/5abd49645591445e1ba04f87/labels' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
        "entityId": {
          "namespace": "AEP",
          "id": "test-ds-id",
          "type": "dataset"
        },
        "labels": [
          "C1"
        ],
        "parents": []
      } '
```

| プロパティ | 説明 |
| --- | --- |
| `entityId` | これにより、更新対象の特定のデータセットエンティティが識別されます。 |
| `entityId.namespace` | これは ID の競合を回避するために使用されます。`namespace` は `AEP` です。 |
| `entityId.id` | 更新するリソースの ID。これは `datasetId` を指します。 |
| `entityId.type` | 更新するリソースのタイプ。これは常に `dataset` になります。 |
| `labels` | データセット全体に追加するデータ使用ラベルのリスト。 |
| `parents` | `parents` 配列には、このデータセットがラベルを継承する `entityId` のリストが含まれています。データセットは、スキーマやデータセットからラベルを継承できます。 |

**応答**

応答が成功すると、データセットの更新されたラベルのセットが返されます。

>[!IMPORTANT]
>
>`optionalLabels` プロパティは、POST リクエストでの使用が非推奨（廃止予定）になりました。データセットフィールドにデータラベルを追加することはできなくなりました。`optionalLabel` 値が存在する場合、POST 操作はエラーをスローします。ただし、PUT リクエストと `optionalLabels` プロパティを使用して、個々のフィールドからラベルを削除できます。詳しくは、[データセットからのラベルの削除](#remove)に関する節を参照してください。

```json
{
  "parents": [
      {
        "id": "_ddgduleint.schemas.4a95cdba7d560e3bca7d8c5c7b58f00ca543e2bb1e4137d6",
        "type": "schema",
        "namespace": "AEP"
      }
  ],
  "optionalLabels": [],
  "labels": [
      "C1"
  ],
  "code": "PES-201",
  "message": "POST Successful"
} 
```

## データセットからラベルを削除する {#remove}

以前に適用されたフィールドラベルを削除するには、既存のフィールドラベルのサブセットで既存の `optionalLabels` 値を更新するか、空のリストを更新して完全に削除します。[!DNL Dataset Service] API に PUT リクエストを送信して、以前に適用されたラベルを更新または削除します。

**API 形式**

```http
PUT /datasets/{DATASET_ID}/labels
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | ラベルを作成するデータセットの一意の `id` 値。 |

**リクエスト**

PUT 操作が適用される以下のデータセットには、properties/person/properties/address フィールドに C1 optionalLabel があり、/properties/person/properties/name/properties/fullName フィールドに C1、C2 optionalLabels がありました。PUT 操作の後、最初のフィールドにはラベルがなくなり（C1 ラベルが削除されました）、2 番目のフィールドには C1 ラベルのみが含まれます（C2 ラベルが削除されました）。

 以下のサンプルシナリオでは、PUT リクエストを使用して、個々のフィールドに追加されたラベルを削除します。リクエストを行う前に、`fullName` フィールドには `C1` および `C2` ラベルが適用され、`address` フィールドには既に `C1` ラベルが適用されていました。PUT リクエストは、`optionalLabels.labels` パラメーターを使用して、`fullName` フィールドの既存のラベルである `C1, C2` ラベルを `C1` ラベルで上書きします。また、このリクエストは、`address` フィールドの `C1` ラベルを空のフィールドラベルのセットで上書きします。

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
        "entityId": {
          "namespace": "AEP",
          "id": "646b814d52e1691c07b41032",
          "type": "dataset"
        },
        "labels": [
          "C1"
        ],
        "parents": [
          {
            "id": "_xdm.context.identity-graph-flattened-export",
            "type": "schema",
            "namespace": "AEP"
          }
        ],
        "optionalLabels": [
          {
            "option": {
              "id": "https://ns.adobe.com/xdm/context/identity-graph-flattened-export",
              "contentType": "application/vnd.adobe.xed-full+json;version=1",
              "schemaPath": "/properties/person/properties/name/properties/fullName"
            },
            "labels": [
              "C1"
            ]
          },
          {
            "option": {
              "id": "https://ns.adobe.com/xdm/context/identity-graph-flattened-export",
              "contentType": "application/vnd.adobe.xed-full+json;version=1",
              "schemaPath": "/properties/person/properties/address"
            },
            "labels": []
          }
        ]
      }'
```

| パラメーター | 説明 |
| --- | --- |
| `entityId` | これにより、更新対象の特定のデータセットエンティティが識別されます。`entityId` には次の 3 つの値を含める必要があります。<br/><br/>`namespace`：これは ID の競合を避けるために使用されます。`namespace` は `AEP` です。<br/>`id`：更新するリソースの ID。これは `datasetId` を指します。<br/>`type`：更新するリソースのタイプ。これは常に `dataset` になります。 |
| `labels` | データセット全体に追加するデータ使用ラベルのリスト。 |
| `parents` | `parents` 配列には、このデータセットがラベルを継承する `entityId` のリストが含まれています。データセットは、スキーマやデータセットからラベルを継承できます。 |
| `optionalLabels` | このパラメーターは、データセットフィールドに以前に適用されたラベルを削除するために使用されます。ラベルを削除するデータセット内の個々のフィールドのリスト。この配列の各アイテムは、次のプロパティを持つ必要があります。<br/><br/>`option`：フィールドの [!DNL Experience Data Model]（XDM）属性を含むオブジェクト。次の 3 つのプロパティが必要です。<ul><li><code>id</code>：URI <code>$id</code> フィールドに関連付けられているスキーマの値。</li><li><code>contentType</code>：スキーマのコンテンツタイプとバージョン番号。XDM ルックアップリクエストに対しては、有効な <a href="../../xdm/api/getting-started.md#accept">Accept ヘッダー</a>のいずれかの形式でおこなう必要があります。</li><li><code>schemaPath</code>：データセットのスキーマ内のフィールドへのパス。</li></ul>`labels`：この値には、適用されている既存のフィールドラベルのサブセットを含めるか、空にして既存のフィールドラベルをすべて削除する必要があります。`optionalLabels` フィールドに新しいラベルまたは変更されたラベルがある場合、PUT メソッドまたは POST メソッドはエラーを返すようになりました。 |

**応答**

応答が成功すると、データセットの更新されたラベルのセットが返されます。

```json
{
  "parents": [
      {
        "id": "_xdm.context.identity-graph-flattened-export",
        "type": "schema",
        "namespace": "AEP"
      }
  ],
  "optionalLabels": [],
  "labels": [
      "C1"
  ],
  "code": "PES-200",
  "message": "PUT Successful"
} 
```

## 次の手順

このドキュメントでは、[!DNL Dataset Service] APIを使用してデータセットとフィールドのデータ使用ラベルを管理する方法について説明しました。適用したラベルに基づいて、[データ使用ポリシー](../policies/overview.md)と[アクセス制御ポリシー](../../access-control/abac/ui/policies.md)を定義できるようになりました。

[!DNL Experience Platform] でのデータセットの管理について詳しくは、[データセットの概要](../../catalog/datasets/overview.md)を参照してください。
