---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ使用量のコンプライアンスをオーディエンスセグメントに適用
topic: tutorial
translation-type: tm+mt
source-git-commit: 7f61cee8fb5160d0f393f8392b4ce2462d602981

---


# APIを使用したデータセグメントのオーディエンス使用コンプライアンスの実施

このチュートリアルでは、APIを使用したリアルタイム顧客プロファイルオーディエンスセグメントのデータ使用法の準拠を適用する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分な理解を得る必要があります。

- [リアルタイム顧客プロファイル](../../profile/home.md):リアルタイム顧客プロファイルは、汎用の参照エンティティストアで、プラットフォーム内のExperience Data Model(XDM)データの管理に使用されます。 プロファイルは、様々な企業データアセット間でデータを結合し、統合されたプレゼンテーションでそのデータへのアクセスを提供します。
   - [ポリシーの結合](../../profile/api/merge-policies.md):リアルタイム顧客プロファイルが、特定の条件下で統合表示に結合できるデータを決定するために使用するルール。 マージポリシーは、データガバナンスの目的で設定できます。
- [セグメント](../home.md):リアルタイム顧客プロファイルは、プロファイルストアに含まれる大きな個人グループを、類似した特性を共有し、マーケティング戦略と同様に対応する小さなグループに分割する方法を示します。
- [データガバナンス](../../data-governance/home.md):データガバナンスは、次のコンポーネントを使用して、データ使用のラベル付けと実施(DULE)のインフラストラクチャを提供します。
   - [データ使用ラベル](../../data-governance/labels/user-guide.md):データセットとフィールドを、それぞれのデータを処理する際に使用する機密性のレベルの観点から記述するために使用されるラベル。
   - [データ使用ポリシー](../../data-governance/api/getting-started.md):特定のデータ使用ラベルで分類されたデータで、どのマーケティングアクションが許可されるかを示す設定。

以下の節では、プラットフォームAPIを正しく呼び出すために知っておく必要がある追加情報を示します。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を示します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) （英語のみ）を参照してください。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず認証チュートリアルを完了する必要 [があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

- 認証：無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

> [!NOTE] プラットフォームのサンドボックスについて詳しくは、サンドボックスの概要ドキュメ [ントを参照してくださ](../../sandboxes/home.md)い。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次のヘッダーが追加で必要です。

- コンテンツタイプ：application/json

## セグメント定義の結合ポリシーの参照

このワークフローは、既知のワークフローセグメントにアクセスすることでオーディエンスを開始します。 リアルタイム顧客プロファイルで使用できるセグメントには、セグメント定義内にマージポリシーIDが含まれます。 このマージポリシーには、セグメントに含めるデータセットに関する情報が含まれ、さらに該当するデータ使用ラベルも含まれます。

セグメント [APIを使用して](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/segmentation.yaml)、IDでセグメント定義を参照し、関連する結合ポリシーを見つけることができます。

**API形式**

```http
GET /segment/definitions/{SEGMENT_DEFINITION_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SEGMENT_DEFINITION_ID}` | 参照するセグメント定義のID。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/segment/definitions/24379cae-726a-4987-b7b9-79c32cddb5c1 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**レポンス**

成功した応答は、セグメント定義の詳細を返します。

```json
{
    "id": "24379cae-726a-4987-b7b9-79c32cddb5c1",
    "schema": { 
        "name": "_xdm.context.profile"
    },
    "ttlInDays": 90,
    "imsOrgId": "{IMS_ORG}",
    "name": "Cart abandons in CA",
    "description": "",
    "expression": {
        "type": "PQL", 
        "format": "pql/text", 
        "value": "homeAddress.countryISO = 'US'"
    },
    "mergePolicyId": "2b43d78d-0ad4-4c1e-ac2d-574c09b01119",
    "evaluationInfo": {
        "batch": {
            "enabled": true
        },
        "continuous": {
            "enabled": false
        },
        "synchronous": {
            "enabled": false
        }
    },
    "creationTime": 1556094486000,
    "updateEpoch": 1556094486000,
    "updateTime": 1556094486000
  }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `mergePolicyId` | セグメント定義に使用されるマージポリシーのID。 これは次の手順で使用します。 |

## マージポリシーからソースデータセットを検索します

マージ・ポリシーには、ソース・データセットに関する情報が含まれ、DULEラベルが含まれます。 マージポリシーの詳細を参照するには、プロファイルAPIへのGET要求でマージポリシーIDを指定します。

**API形式**

```http
GET /config/mergePolicies/{MERGE_POLICY_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{MERGE_POLICY_ID}` | 前の手順で取得したマージポリシ [ーのID](#lookup-a-merge-policy-for-a-segment-definition)。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/config/mergePolicies/2b43d78d-0ad4-4c1e-ac2d-574c09b01119 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**レポンス**

成功した応答は、マージポリシーの詳細を返します。

```json
{
    "id": "2b43d78d-0ad4-4c1e-ac2d-574c09b01119",
    "imsOrgId": "{IMS_ORG}",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "version": 1,
    "identityGraph": {
        "type": "none"
    },
    "attributeMerge": {
        "type":"dataSetPrecedence", 
        "data": {
            "order" : ["5b95b155419ec801e6eee780", "5b7c86968f7b6501e21ba9df"]
        }
    },
    "default": false,
    "updateEpoch": 1551127597
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `schema.name` | マージポリシーに関連付けられているスキーマの名前です。 |
| `attributeMerge.type` | マージポリシーのデータ優先順位の構成タイプ。 値がの場合、このマ `dataSetPrecedence`ージポリシーに関連付けられたデータセットがに表示されま `attributeMerge > data > order`す。 値がの場合、で参照さ `timestampOrdered`れているスキーマに関連付けられているすべてのデ `schema.name` ータセットがマージポリシーで使用されます。 |
| `attributeMerge.data.order` | がの場合、 `attributeMerge.type` この属 `dataSetPrecedence`性は、このマージポリシーで使用されるデータセットのIDを含む配列になります。 これらのIDは次の手順で使用します。 |

## ソースデータセットの参照データ使用ラベル

マージポリシーのソースデータセットのIDを収集したら、これらのIDを使用して、データセット自体およびデータセットに含まれる特定のデータフィールド用に設定されたデータ使用ラベルを参照できます。

次の [Catalog Service APIの呼び出しは](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml) 、リクエストパスにIDを指定することで、1つのデータセットに関連付けられたデータ使用ラベルを取得します。

**API形式**

```http
GET /dataSets/{DATASET_ID}/dule
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{DATASET_ID}` | 参照するデータ使用ラベルのデータセットのID。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/catalog/dataSets/5b95b155419ec801e6eee780/dule \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**レポンス**

成功した場合は、データセット全体に関連付けられたデータ使用量ラベルのリストと、ソーススキーマに関連付けられた特定のデータフィールドが返されます。

```json
{
    "connection": {},
    "dataset": {
        "identity": [],
        "contract": [
            "C3"
        ],
        "sensitive": [],
        "contracts": [
            "C3"
        ],
        "identifiability": [],
        "specialTypes": []
    },
    "fields": [],
    "schemaFields": [
        {
            "path": "/properties/personalEmail/properties/address",
            "identity": [
                "I1"
            ],
            "contract": [
                "C2",
                "C9"
            ],
            "sensitive": [],
            "contracts": [
                "C2",
                "C9"
            ],
            "identifiability": [
                "I1"
            ],
            "specialTypes": []
        }
    ]
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `dataset` | データセット全体に適用されるデータ使用ラベルを含むオブジェクトです。 |
| `schemaFields` | データ使用ラベルが適用された特定のスキーマフィールドを表すオブジェクトの配列です。 |
| `schemaFields.path` | データ使用ラベルが同じスキーマにリストされているオブジェクトフィールドのパス。 |

## データフィールドのフィルタ

> [!NOTE] この手順はオプションです。 データ使用ラベルを調べる前の手順の結果に基づいてセグメントに含まれるデータを調整しない場合 [、ポリシー違反のデータを評価する最終手順に進む](#lookup-data-usage-labels-for-the-source-datasets)ことができます [](#evaluate-data-for-policy-violations)。

オーディエンスセグメントに含まれるデータを調整する場合は、次の2つの方法のいずれかを使用します。

### セグメント定義のマージポリシーの更新

セグメント定義のマージポリシーを更新すると、セグメントジョブの実行時に含まれるデータセットとフィールドが調整されます。 詳しくは、マージポリシ [ーのチュートリアルで](../../profile/api/merge-policies.md) 、既存のマージポリシーの更新に関する節を参照してください。

### セグメントの書き出し時に特定のデータフィールドを制限する

リアルタイム顧客プロファイルAPIを使用してセグメントをデータセットに書き出す場合は、パラメーターを使用して、書き出しに含まれるデータをフィルターでき `fields` ます。 このパラメーターに追加されたデータフィールドはエクスポートに含まれ、その他のデータフィールドはすべて除外されます。

「A」、「B」および「C」という名前のデータフィールドを持つセグメントについて考えてみます。 フィールド「C」のみを書き出す場合、パラメーターにはフィール `fields` ド「C」のみが含まれます。 これにより、セグメントを書き出す際に「A」と「B」のフィールドが除外されます。

詳しくは、セグメントチュ [ートリアルの](./evaluate-a-segment.md#export-a-segment) 、セグメントの書き出しに関する節を参照してください。

## ポリシー違反のデータを評価します

これで、オーディエンスセグメントに関連付けられたデータ使用ラベルを収集したので、これらのラベルをマーケティングアクションに対してテストし、データ使用ポリシー違反を評価できます。 [DULE Policy Service APIを使用してポリシー評価を実行する方法の詳細な手順については](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml)、「ポリシー評価に関するドキュメント」を参 [照してください](../../data-governance/enforcement/overview.md)。

## 次の手順

このチュートリアルに従うことで、オーディエンスセグメントに関連付けられたデータ使用ラベルを調べ、特定のマーケティングアクションに対するポリシー違反の有無をテストしました。 エクスペリエンスプラットフォームのデータガバナンスについて詳しくは、データガバナンスの概 [要を参照してくださ](../../data-governance/home.md)い。