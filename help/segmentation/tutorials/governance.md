---
solution: Experience Platform
title: API を使用して、オーディエンスセグメントのデータ使用コンプライアンスを適用する
type: Tutorial
description: このチュートリアルでは、API を使用してデータ使用コンプライアンスセグメント定義を適用する手順を説明します。
exl-id: 2299328c-d41a-4fdc-b7ed-72891569eaf2
source-git-commit: e52eb90b64ae9142e714a46017cfd14156c78f8b
workflow-type: tm+mt
source-wordcount: '1348'
ht-degree: 40%

---

# API を使用して、セグメント定義のデータ使用コンプライアンスを適用する

このチュートリアルでは、API を使用して、セグメント定義のデータ使用に対する準拠を適用する手順を説明します。

## はじめに

このチュートリアルでは、[!DNL Adobe Experience Platform] の次のコンポーネントに関する十分な知識が必要です。

- [[!DNL Real-Time Customer Profile]](../../profile/home.md): [!DNL Real-Time Customer Profile] は汎用ルックアップ エンティティ ストアであり、管理に使用されます [!DNL Experience Data Model (XDM)] 内のデータ [!DNL Platform]. プロファイルでは、様々な企業データアセットのデータが結合され、統合されたプレゼンテーションでそのデータにアクセスできます。
   - [結合ポリシー](../../profile/api/merge-policies.md)：が使用するルール [!DNL Real-Time Customer Profile] 特定の条件下でどのデータを統合ビューに結合できるかを決定する。 結合ポリシーは、データガバナンスの目的で設定できます。
- [[!DNL Segmentation]](../home.md)：方法 [!DNL Real-Time Customer Profile] プロファイルストアに含まれる個人の大きなグループを、類似の特性を共有し、マーケティング戦略に同様に応答する小さなグループに分割します。
- [データガバナンス](../../data-governance/home.md)：データガバナンスは、次のコンポーネントを使用して、データ使用のラベル付けと実施に関するインフラストラクチャを提供します。
   - [データ使用ラベル](../../data-governance/labels/user-guide.md)：データセットとフィールドを、それぞれのデータを処理する際に適用する機密性のレベルの観点から説明する際に使用されるラベルです。
   - [データ使用ポリシー](../../data-governance/policies/overview.md)：特定のデータ使用ラベルで分類されたデータで、どのマーケティングアクションが許可されるかを示す設定です。
   - [ポリシーの適用](../../data-governance/enforcement/overview.md)：データ使用ポリシーを適用し、ポリシー違反を構成するデータ操作を防ぐことができます。
- [サンドボックス](../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別個の仮想環境に分割してデジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスが用意されています。

以下の節では、を正しく呼び出すために知っておく必要がある追加情報を示します [!DNL Platform] API です。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

に対して呼び出しを行うため [!DNL Platform] API を使用する場合、最初にを完了する必要があります。 [認証のチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja). 認証チュートリアルを完了すると、必要な各ヘッダーの値がすべて提供されます [!DNL Experience Platform] API 呼び出し（下図を参照）。

- Authorization： Bearer `{ACCESS_TOKEN}`
- x-api-key： `{API_KEY}`
- x-gw-ims-org-id： `{ORG_ID}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name：`{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、次のような追加ヘッダーが必要です。

- Content-Type：application/json

## セグメント定義の結合ポリシーの検索 {#merge-policy}

このワークフローは、既知のセグメント定義にアクセスすることから始まります。 で使用できるセグメント定義 [!DNL Real-Time Customer Profile] には、セグメント定義内の結合ポリシー ID が含まれます。 この結合ポリシーには、セグメント定義に含めるデータセットに関する情報が含まれ、その中に適用可能なデータ使用ラベルが含まれています。

使用， [!DNL Segmentation] API の場合、ID でセグメント定義を検索して、関連する結合ポリシーを見つけることができます。

**API 形式**

```http
GET /segment/definitions/{SEGMENT_DEFINITION_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SEGMENT_DEFINITION_ID}` | 検索するセグメント定義の ID。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/segment/definitions/24379cae-726a-4987-b7b9-79c32cddb5c1 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、セグメント定義の詳細が返されます。

```json
{
    "id": "24379cae-726a-4987-b7b9-79c32cddb5c1",
    "schema": { 
        "name": "_xdm.context.profile"
    },
    "ttlInDays": 90,
    "imsOrgId": "{ORG_ID}",
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
| `mergePolicyId` | セグメント定義に使用される結合ポリシーの ID。これは次の手順で使用します。 |

## 結合ポリシーからソースデータセットを検索する {#datasets}

結合ポリシーには、ソースデータセットに関する情報が含まれ、そのソースデータセットにはデータ使用ラベルが含まれます。 に対するGETリクエストで結合ポリシー ID を指定することで、結合ポリシーの詳細を参照できます [!DNL Profile] API です。 結合ポリシーの詳細については、を参照してください。 [結合ポリシーエンドポイントガイド](../../profile/api/merge-policies.md).

**API 形式**

```http
GET /config/mergePolicies/{MERGE_POLICY_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{MERGE_POLICY_ID}` | [前の手順](#merge-policy)で取得した結合ポリシーの ID。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/config/mergePolicies/2b43d78d-0ad4-4c1e-ac2d-574c09b01119 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、結合ポリシーの詳細を返します。

```json
{
    "id": "2b43d78d-0ad4-4c1e-ac2d-574c09b01119",
    "imsOrgId": "{ORG_ID}",
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
            "order": ["5b95b155419ec801e6eee780", "5b7c86968f7b6501e21ba9df"]
        }
    },
    "default": false,
    "updateEpoch": 1551127597
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `schema.name` | 結合ポリシーに関連付けられているスキーマの名前。 |
| `attributeMerge.type` | 結合ポリシーのデータ優先の設定タイプ。値が `dataSetPrecedence` の場合、この結合ポリシーに関連付けられているデータセットが `attributeMerge > data > order` の下に表示されます。値が `timestampOrdered` の場合、`schema.name` で参照されているスキーマに関連付けられているすべてのデータセットが結合ポリシーで使用されます。 |
| `attributeMerge.data.order` | `attributeMerge.type` が `dataSetPrecedence` の場合、この属性は、結合ポリシーで使用されるデータセットの IDを含む配列になります。これらの ID は次の手順で使用します。 |

## ポリシー違反のデータセットを評価

>[!NOTE]
>
> この手順では、特定のラベルを含んだデータに対して特定のマーケティングアクションが実行されないようにする、アクティブなデータ使用ポリシーが少なくとも 1 つ存在することを前提としています。 評価されるデータセットに適用できる使用ポリシーがない場合は、次の手順に従ってください [ポリシー作成のチュートリアル](../../data-governance/policies/create.md) を作成してから、この手順を続行します。

結合ポリシーのソースデータセットの ID を取得したら、 [Policy Service API](https://www.adobe.io/experience-platform-apis/references/policy-service/) データ使用ポリシー違反を確認するために、特定のマーケティングアクションに対してこれらのデータセットを評価する。

データセットを評価するには、次の例に示すように、POSTリクエストのパスにマーケティングアクションの名前を指定する必要があります。一方、リクエスト本文内ではデータセット ID を指定する必要があります。

**API 形式**

```http
POST /marketingActions/core/{MARKETING_ACTION_NAME}/constraints
POST /marketingActions/custom/{MARKETING_ACTION_NAME}/constraints
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | データセットの評価基準となるデータ使用ポリシーに関連付けられたマーケティングアクションの名前。 ポリシーがAdobeによって定義されたか、組織によって定義されたかに応じて、を使用する必要があります `/marketingActions/core` または `/marketingActions/custom`、それぞれ。 |

**リクエスト**

次のリクエストは、 `exportToThirdParty` で取得したデータセットに対するマーケティングアクション [前の手順](#datasets). リクエストペイロードは、各データセットの ID を含む配列です。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/exportToThirdParty/constraints
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `entityType` | ペイロード配列の各項目は、定義するエンティティのタイプを示す必要があります。この使用例では、値は常に「dataSet」になります。 |
| `entityID` | ペイロード配列の各項目は、データセットの一意の ID を提供する必要があります。 |

**応答**

正常な応答は、マーケティングアクションの URI、提供されたデータセットから収集されたデータ使用ラベル、およびこれらのラベルに対するアクションのテストの結果として違反したデータ使用ポリシーのリストを返します。 この例では、「データをサードパーティに書き出し」ポリシーが `violatedPolicies` マーケティングアクションがポリシー違反をトリガーしたことを示す配列。

```json
{
  "timestamp": 1556324277895,
  "clientId": "{CLIENT_ID}",
  "userId": "{USER_ID}",
  "imsOrg": "{ORG_ID}",
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
      "imsOrg": "{ORG_ID}",
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
| `discoveredLabels` | リクエストペイロードで提供されたデータセットのリスト。それぞれに見つかったデータセットレベルとフィールドレベルのラベルを表示します。 |
| `violatedPolicies` | マーケティングアクションのテストに違反したデータ使用ポリシーをリストする配列です（で指定） `marketingActionRef`）に対して実行します `duleLabels`. |

API 応答で返されたデータを使用して、エクスペリエンスアプリケーション内でプロトコルを設定し、ポリシー違反が発生した場合に適切にポリシー違反を実施することができます。

## データフィールドをフィルタリングする

セグメント定義が評価に合格しない場合、以下に示す 2 つの方法のいずれかを使用して、セグメント定義に含まれるデータを調整できます。

### セグメント定義の結合ポリシーを更新する

セグメント定義の結合ポリシーを更新すると、セグメントジョブの実行時に含まれるデータセットとフィールドが調整されます。の節を参照してください。 [既存の結合ポリシーの更新](../../profile/api/merge-policies.md#update) 詳しくは、API 結合ポリシーのチュートリアルを参照してください。

### セグメント定義を書き出す際の特定のデータフィールドの制限

を使用してセグメント定義をデータセットに書き出す場合 [!DNL Segmentation] API を使用する場合、次のコマンドを使用して、書き出しに含まれるデータをフィルタリングすることができます `fields` パラメーター。 このパラメーターに追加したデータフィールドは書き出しに含まれ、その他のデータフィールドはすべて除外されます。

「A」、「B」、「C」という名前のデータフィールドを持つセグメント定義について考えてみます。 フィールド「C」のみを書き出す場合、`fields` パラメーターにはフィールド「C」だけを指定します。これにより、セグメント定義を書き出す際に、「A」および「B」フィールドが除外されます。

の節を参照してください。 [セグメント定義の書き出し](./evaluate-a-segment.md#export) 詳しくは、セグメント化のチュートリアルを参照してください。

## 次の手順

このチュートリアルでは、セグメント定義に関連付けられたデータ使用ラベルを検索し、特定のマーケティングアクションに対するポリシー違反についてテストしました。 でのデータガバナンスについて詳しくは、こちらを参照してください [!DNL Experience Platform]の概要を参照してください [データガバナンス](../../data-governance/home.md).
