---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ使用量のコンプライアンスをオーディエンスセグメントに適用
topic: tutorial
translation-type: tm+mt
source-git-commit: 97ba7aeb8a67735bd65af372fbcba5e71aee6aae

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
   - [データ使用ポリシー](../../data-governance/policies/overview.md):特定のデータ使用ラベルで分類されたデータで、どのマーケティングアクションが許可されるかを示す設定。
   - [ポリシーの適用](../../data-governance/enforcement/overview.md):データ使用ポリシーを適用し、ポリシー違反を構成するデータ操作を防ぐことができます。
- [サンドボックス](../../sandboxes/home.md):Experience Platformは、デジタルエクスペリエンスアプリケーションの開発と発展を支援するために、単一のプラットフォームインスタンスを別々の仮想環境に分割する仮想サンドボックスを提供します。

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

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、サンドボックスの概要ドキュメ [ントを参照してくださ](../../sandboxes/home.md)い。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次のヘッダーが追加で必要です。

- コンテンツタイプ：application/json

## セグメント定義のマージポリシーを検索します {#merge-policy}

このワークフローは、既知のワークフローセグメントにアクセスすることでオーディエンスを開始します。 リアルタイム顧客プロファイルで使用できるセグメントには、セグメント定義内にマージポリシーIDが含まれます。 このマージポリシーには、セグメントに含めるデータセットに関する情報が含まれ、さらに該当するデータ使用ラベルも含まれます。

セグメント [APIを使用して](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/segmentation.yaml)、IDでセグメント定義を検索し、関連する結合ポリシーを見つけることができます。

**API形式**

```http
GET /segment/definitions/{SEGMENT_DEFINITION_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SEGMENT_DEFINITION_ID}` | 検索するセグメント定義のID。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/segment/definitions/24379cae-726a-4987-b7b9-79c32cddb5c1 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

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

## マージポリシーからソースデータセットを検索します {#datasets}

マージ・ポリシーには、ソース・データセットに関する情報が含まれ、その情報にはデータ使用ラベルが含まれます。 マージポリシーの詳細を参照するには、プロファイルAPIへのGET要求でマージポリシーIDを指定します。

**API形式**

```http
GET /config/mergePolicies/{MERGE_POLICY_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{MERGE_POLICY_ID}` | 前の手順で取得したマージポリシ [ーのID](#merge-policy)。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/config/mergePolicies/2b43d78d-0ad4-4c1e-ac2d-574c09b01119 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

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

## データセットのポリシー違反の評価

>[!NOTE]  この手順では、特定のラベルを含むデータに対して特定のマーケティングアクションが実行されないようにする、アクティブなデータ使用ポリシーが1つ以上あることを前提としています。 評価するデータセットに適用可能な使用ポリシーがない場合は、ポリシー作成のチュートリアルに従って [作成し](../../data-governance/policies/create.md) 、この手順に進む前に作成してください。

マージポリシーのソースデータセットのIDを取得したら、 [](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml) DULE Policy Service APIを使用して、データ使用ポリシーの違反を確認するために、特定のマーケティングアクションに対してこれらのデータセットを評価できます。

データセットを評価するには、次の例に示すように、リクエスト本文内にデータセットIDを指定しながら、POSTリクエストのパスにマーケティングアクションの名前を指定する必要があります。

**API形式**

```http
POST /marketingActions/core/{MARKETING_ACTION_NAME}/constraints
POST /marketingActions/custom/{MARKETING_ACTION_NAME}/constraints
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | データセットを評価するデータ使用ポリシーに関連付けられたマーケティングアクションの名前。 ポリシーがアドビによって定義されたか、組織によって定義されたかに応じて、それぞれまたはを使用 `/marketingActions/core` する必要が `/marketingActions/custom`あります。 |

**リクエスト**

次のリクエストは、前の手順で取得 `exportToThirdParty` したデータセットに対してマーケティングアクション [をテストしま](#datasets)す。 リクエストペイロードは、各データセットのIDを含む配列です。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/exportToThirdParty/constraints
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '[
    {
      "entityType": "dataSet",
      "entityId": "5b95b155419ec801e6eee780"
    },
    {
      "entityType": "dataSet",
      "entityId": "5b7c86968f7b6501e21ba9df"
    }
  ]'
```

| プロパティ | 説明 |
| --- | --- |
| `entityType` | ペイロード配列の各項目は、定義するエンティティのタイプを示す必要があります。 この使用例では、値は常に「dataSet」になります。 |
| `entityID` | ペイロード配列の各項目は、データセットの一意のIDを提供する必要があります。 |

**応答**

成功した応答は、マーケティングアクションのURI、提供されたデータセットから収集されたデータ使用ラベル、およびこれらのラベルに対するアクションのテストの結果として違反されたデータ使用ポリシーのリストを返します。 この例では、「データをサードパーティにエクスポート」ポリシーが配列に表示され、マーケティングアクシ `violatedPolicies` ョンがポリシー違反をトリガーしたことを示しています。

```json
{
  "timestamp": 1556324277895,
  "clientId": "{CLIENT_ID}",
  "userId": "{USER_ID}",
  "imsOrg": "{IMS_ORG}",
  "marketingActionRef": "https://platform.adobe.io:443/data/foundation/dulepolicy/marketingActions/custom/exportToThirdParty",
  "duleLabels": [
    "C1",
    "C2",
    "C4",
    "C5"
  ],
  "discoveredLabels": [
    {
      "entityType": "dataSet",
      "entityId": "5b95b155419ec801e6eee780",
      "dataSetLabels": {
        "connection": {
          "labels": []
        },
        "dataSet": {
          "labels": [
            "C5"
          ]
        },
        "fields": [
          {
            "labels": [
              "C2",
            ],
            "path": "/properties/_customer"
          },
          {
            "labels": [
              "C5"
            ],
            "path": "/properties/geoUnit"
          },
          {
            "labels": [
              "C1"
            ],
            "path": "/properties/identityMap"
          }
        ]
      }
    },
    {
      "entityType": "dataSet",
      "entityId": "5b7c86968f7b6501e21ba9df",
      "dataSetLabels": {
        "connection": {
          "labels": []
        },
        "dataSet": {
          "labels": [
            "C5"
          ]
        },
        "fields": [
          {
            "labels": [
              "C5"
            ],
            "path": "/properties/createdByBatchID"
          },
          {
            "labels": [
              "C5"
            ],
            "path": "/properties/faxPhone"
          }
        ]
      }
    }
  ],
  "violatedPolicies": [
    {
      "name": "Export Data to Third Party",
      "status": "ENABLED",
      "marketingActionRefs": [
        "https://platform-stage.adobe.io:443/data/foundation/dulepolicy/marketingActions/custom/exportToThirdParty"
      ],
      "description": "Conditions under which data cannot be exported to a third party",
      "deny": {
        "operator": "OR",
        "operands": [
          {
            "label": "C1"
          },
          {
            "operator": "AND",
            "operands": [
              {
                "label": "C3"
              },
              {
                "label": "C7"
              }
            ]
          }
        ]
      },
      "imsOrg": "{IMS_ORG}",
      "created": 1565651746693,
      "createdClient": "{CREATED_CLIENT}",
      "createdUser": "{CREATED_USER",
      "updated": 1565723012139,
      "updatedClient": "{UPDATED_CLIENT}",
      "updatedUser": "{UPDATED_USER}",
      "_links": {
        "self": {
          "href": "https://platform-stage.adobe.io/data/foundation/dulepolicy/policies/custom/5d51f322e553c814e67af1a3"
        }
      },
      "id": "5d51f322e553c814e67af1a3"
    }
  ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `duleLabels` | 指定されたリストセットから抽出されたデータ使用ラベルのデータ。 |
| `discoveredLabels` | リクエストペイロードで提供されたデータセットのリスト。各データセット内のデータセットレベルとフィールドレベルのラベルが表示されます。 |
| `violatedPolicies` | 提供されたに対する（で指定された）マーケティングアクションのテストに違反したデータ使用ポリシーを `marketingActionRef`リストする配列で `duleLabels`す。 |

API応答で返されたデータを使用して、エクスペリエンスアプリケーション内でプロトコルを設定し、ポリシー違反が発生した場合に適切にポリシー違反を適用することができます。

## データフィールドのフィルタ

オーディエンスセグメントが評価に合格しない場合は、次に示す2つの方法のいずれかを使用して、セグメントに含まれるデータを調整できます。

### セグメント定義のマージポリシーの更新

セグメント定義のマージポリシーを更新すると、セグメントジョブの実行時に含まれるデータセットとフィールドが調整されます。 詳しくは、APIマージポ [リシーチュートリアルの](../../profile/api/merge-policies.md#update) 、既存のマージポリシーの更新に関する節を参照してください。

### セグメントの書き出し時に特定のデータフィールドを制限する

リアルタイム顧客プロファイルAPIを使用してセグメントをデータセットに書き出す場合は、パラメーターを使用して、書き出しに含まれるデータをフィルターでき `fields` ます。 このパラメーターに追加されたデータフィールドはエクスポートに含まれ、その他のデータフィールドはすべて除外されます。

「A」、「B」および「C」という名前のデータフィールドを持つセグメントについて考えてみます。 フィールド「C」のみを書き出す場合、パラメーターにはフィール `fields` ド「C」のみが含まれます。 これにより、セグメントを書き出す際に「A」と「B」のフィールドが除外されます。

詳しくは、セグメントチュ [ートリアルの](./evaluate-a-segment.md#export) 、セグメントの書き出しに関する節を参照してください。

## 次の手順

このチュートリアルに従うことで、オーディエンスセグメントに関連付けられたデータ使用ラベルを調べ、特定のマーケティングアクションに対するポリシー違反の有無をテストしました。 エクスペリエンスプラットフォームのデータガバナンスについて詳しくは、データガバナンスの概 [要を参照してくださ](../../data-governance/home.md)い。
