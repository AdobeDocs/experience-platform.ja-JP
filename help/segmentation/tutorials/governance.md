---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: オーディエンスセグメントに対するデータ使用コンプライアンスの実施
topic: tutorial
translation-type: tm+mt
source-git-commit: cb6a2f91eb6c18835bd9542e5b66af4682227491
workflow-type: tm+mt
source-wordcount: '1325'
ht-degree: 2%

---


# APIを使用したオーディエンスセグメントのデータ使用コンプライアンスの実施

このチュートリアルでは、APIを使用する [!DNL Real-time Customer Profile] オーディエンスセグメントのデータ使用のコンプライアンスを適用する手順を説明します。

## はじめに

このチュートリアルでは、次のコンポーネントについて十分に理解している必要があり [!DNL Adobe Experience Platform]ます。

- [!DNL Real-time Customer Profile](../../profile/home.md): [!DNL Real-time Customer Profile] は汎用の参照エンティティストアで、内で [!DNL Experience Data Model] (XDM)データを管理するために使用され [!DNL Platform]ます。 プロファイルは、様々な企業データアセットにわたるデータを結合し、統合されたプレゼンテーションでそのデータへのアクセスを提供します。
   - [ポリシーの結合](../../profile/api/merge-policies.md): 特定の条件下で統合表示 [!DNL Real-time Customer Profile] に統合できるデータを決定するためにに使用されるルール。 マージポリシーは、データ管理の目的で設定できます。
- [!DNL Segmentation](../home.md): プロファイルストアに含まれる多数の個人を、同じ特性を共有し、マーケティング戦略と同様に対応する小さなグループに分割する方法を [!DNL Real-time Customer Profile] 説明します。
- [!DNL Data Governance](../../data-governance/home.md): [!DNL Data Governance] 次のコンポーネントを使用して、データ使用のラベル付けと実施(DULE)のインフラストラクチャを提供します。
   - [データ使用ラベル](../../data-governance/labels/user-guide.md): データセットとフィールドを、それぞれのデータを処理する際の機密性のレベルに関して記述するために使用されるラベル。
   - [データ使用ポリシー](../../data-governance/policies/overview.md): 特定のデータ使用ラベルによって分類されたデータで、どのマーケティングアクションが許可されるかを示す設定。
   - [ポリシーの適用](../../data-governance/enforcement/overview.md): データ使用ポリシーを適用し、ポリシー違反を構成するデータ操作を防止できます。
- [サンドボックス](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

以下の節では、APIを正しく呼び出すために知る必要がある追加情報について説明し [!DNL Platform] ます。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、トラブルシューティングガイドのAPI呼び出し例 [を読む方法に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照して [!DNL Experience Platform] ください。

### 必要なヘッダーの値の収集

APIを呼び出すには、まず [!DNL Platform] 認証チュートリアルを完了する必要があり [ます](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべての [!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定する

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

内のすべてのリソース [!DNL Experience Platform] は、特定の仮想サンドボックスに分離されます。 APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>のサンドボックスについて詳し [!DNL Platform]くは、 [Sandboxの概要ドキュメントを参照してください](../../sandboxes/home.md)。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

- Content-Type: application/json

## セグメント定義の結合ポリシーを検索します {#merge-policy}

このワークフローは、既知のオーディエンスセグメントにアクセスすることから開始されます。 で使用できるセグメントは、セグメント定義内にマージポリシーIDを [!DNL Real-time Customer Profile] 含みます。 このマージポリシーには、セグメントに含めるデータセットに関する情報が含まれ、そのデータセットには該当するデータ使用ラベルが含まれます。

APIを使用して、IDでセグメント定義を検索し、関連付けられているマージポリシーを見つけることができます。 [!DNL Segmentation]

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
| `mergePolicyId` | セグメント定義に使用されるマージポリシーのID。 これは、次の手順で使用します。 |

## マージポリシーからソースデータセットを検索します {#datasets}

マージ・ポリシーには、ソース・データセットに関する情報が含まれ、ソース・データセットにはデータ使用ラベルが含まれます。 APIへのGET要求でマージポリシーIDを指定すると、マージポリシーの詳細を参照でき [!DNL Profile] ます。 結合ポリシーの詳細については、 [結合ポリシーエンドポイントガイドを参照してください](../../profile/api/merge-policies.md)。

**API形式**

```http
GET /config/mergePolicies/{MERGE_POLICY_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{MERGE_POLICY_ID}` | 前の手順で取得したマージポリシーのID [です](#merge-policy)。 |

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

正常に応答すると、マージポリシーの詳細が返されます。

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
| `attributeMerge.type` | マージポリシーのデータ優先順位の構成タイプです。 値がの場合 `dataSetPrecedence`、このマージポリシーに関連付けられているデータセットがに表示され `attributeMerge > data > order`ます。 値がの場合 `timestampOrdered`、で参照されているスキーマに関連付けられているすべてのデータセット `schema.name` がマージポリシーで使用されます。 |
| `attributeMerge.data.order` | の場合、この属性 `attributeMerge.type``dataSetPrecedence`は、このマージポリシーで使用されるデータセットのIDを含む配列になります。 これらのIDは次の手順で使用します。 |

## ポリシー違反のデータセットを評価

>[!NOTE]
>
> この手順では、特定のラベルを含むデータに対して特定のマーケティング操作が実行されないようにする、アクティブなデータ使用ポリシーが少なくとも1つあることを前提としています。 評価対象のデータセットに対して適切な使用ポリシーがない場合は、 [ポリシーの作成のチュートリアル](../../data-governance/policies/create.md) に従って使用ポリシーを作成してから、この手順に進んでください。

マージポリシーのソースデータセットのIDを取得したら、 [DULE Policy Service API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml) (DOCULE Policy Service)を使用して、データ使用ポリシーの違反を確認するために、特定のマーケティングアクションに対するデータセットを評価できます。

データセットを評価するには、次の例に示すように、POSTリクエストのパスにマーケティングアクションの名前を指定し、リクエスト本文内でデータセットIDを指定する必要があります。

**API形式**

```http
POST /marketingActions/core/{MARKETING_ACTION_NAME}/constraints
POST /marketingActions/custom/{MARKETING_ACTION_NAME}/constraints
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | データセットを評価するデータ使用ポリシーに関連付けられたマーケティングアクションの名前。 ポリシーがアドビによって定義されたか、組織で定義されたかに応じて、それぞれまたはを使用する必要があ `/marketingActions/core` り `/marketingActions/custom`ます。 |

**リクエスト**

次のリクエストは、 `exportToThirdParty` 前の手順で取得したデータセットに対して、 [マーケティングアクションをテストし](#datasets)ます。 リクエストペイロードは、各データセットのIDを含む配列です。

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

成功した応答は、マーケティングアクションのURI、指定されたデータセットから収集されたデータ使用ラベル、およびこれらのラベルに対するアクションのテストの結果として違反されたデータ使用ポリシーのリストを返します。 この例では、「データをサードパーティにエクスポート」ポリシーが `violatedPolicies` 配列に表示され、マーケティング操作がポリシー違反をトリガーしたことを示しています。

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
| `duleLabels` | 提供されたデータセットから抽出されたデータ使用ラベルのリスト。 |
| `discoveredLabels` | リクエストペイロードで提供されたデータセットのリスト。各ペイロードで見つかったデータセットレベルとフィールドレベルのラベルが表示されます。 |
| `violatedPolicies` | 提供された製品に対するマーケティングアクション（で指定）のテストに違反したデータ使用ポリシーをリストした配列で `marketingActionRef``duleLabels`す。 |

API応答で返されるデータを使用して、エクスペリエンスアプリケーション内でプロトコルを設定し、ポリシー違反が発生した場合に適切にポリシー違反を適用することができます。

## データフィールドのフィルター

オーディエンスセグメントが評価に合格しない場合は、次に示す2つの方法のいずれかを使用して、セグメントに含まれるデータを調整できます。

### セグメント定義の結合ポリシーの更新

セグメント定義のマージポリシーを更新すると、セグメントジョブの実行時に含まれるデータセットとフィールドが調整されます。 詳しくは、APIマージポリシーチュートリアルの既存のマージポリシー [の](../../profile/api/merge-policies.md#update) 更新に関する節を参照してください。

### セグメントの書き出し時に特定のデータフィールドを制限する

APIを使用してセグメントをデータセットにエクスポートする場合、 [!DNL Segmentation] パラメーターを使用して、エクスポートに含まれるデータをフィルターでき `fields` ます。 このパラメーターに追加されたデータフィールドはエクスポートに含まれ、その他のデータフィールドはすべて除外されます。

セグメントに「A」、「B」、「C」という名前のデータフィールドがあるとします。 フィールド「C」のみを書き出す場合は、この `fields` パラメーターにフィールド「C」だけが含まれます。 これにより、セグメントを書き出すときにフィールド「A」と「B」が除外されます。

詳しくは、セグメントチュートリアルのセグメントの [書き出しに関する節を参照してください](./evaluate-a-segment.md#export) 。

## 次の手順

このチュートリアルに従って、オーディエンスセグメントに関連付けられたデータ使用ラベルを調べ、特定のマーケティング操作に対するポリシー違反がないかテストしました。 の詳細については、の概要 [!DNL Data Governance] を参照し [!DNL Experience Platform]てくだ [!DNL Data Governance](../../data-governance/home.md)さい。
