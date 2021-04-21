---
keywords: Experience Platform；ホーム；人気の高いトピック；データ使用の準拠；適用；データ使用の準拠；適用；Segmentation Service；セグメント化；セグメント化；
solution: Experience Platform
title: APIを使用したオーディエンスセグメントに対するデータ使用量コンプライアンスの実施
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルでは、API を使用して、リアルタイム顧客プロファイルのオーディエンスセグメントでデータ使用のコンプライアンスを徹底する方法について説明します。
exl-id: 2299328c-d41a-4fdc-b7ed-72891569eaf2
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1362'
ht-degree: 45%

---

# API を使用して、オーディエンスセグメントでデータ使用のコンプライアンスを徹底する

このチュートリアルでは、APIを使用して[!DNL Real-time Customer Profile]オーディエンスセグメントのデータ使用に関するコンプライアンスを適用する手順を説明します。

## はじめに

このチュートリアルでは、[!DNL Adobe Experience Platform]の次のコンポーネントについて十分に理解している必要があります。

- [[!DNL Real-time Customer Profile]](../../profile/home.md): [!DNL Real-time Customer Profile] は汎用の参照エンティティストアで、内の [!DNL Experience Data Model (XDM)] データの管理に使用され [!DNL Platform]ます。プロファイルでは、様々な企業データアセットのデータが結合され、統合されたプレゼンテーションでそのデータにアクセスできます。
   - [ポリシーの結合](../../profile/api/merge-policies.md):特定の条件下で統合表示 [!DNL Real-time Customer Profile] に統合できるデータを決定するためにに使用されるルール。マージポリシーは[!DNL Data Governance]の目的で設定できます。
- [[!DNL Segmentation]](../home.md):プロファイルストアに含まれる多数の個人グループを、同様の特性を共有し、マーケティング戦略と同様に対応する小さなグループに [!DNL Real-time Customer Profile] 分割する方法。
- [[!DNL Data Governance]](../../data-governance/home.md): [!DNL Data Governance] は、次のコンポーネントを使用して、データ使用のラベル付けと実行のためのインフラストラクチャを提供します。
   - [データ使用ラベル](../../data-governance/labels/user-guide.md)：データセットとフィールドを、それぞれのデータを処理する際に適用する機密性のレベルの観点から説明する際に使用されるラベルです。
   - [データ使用ポリシー](../../data-governance/policies/overview.md)：特定のデータ使用ラベルで分類されたデータで、どのマーケティングアクションが許可されるかを示す設定です。
   - [ポリシーの適用](../../data-governance/enforcement/overview.md):データ使用ポリシーを適用し、ポリシー違反を構成するデータ操作を防止できます。
- [サンドボックス](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

以下の節では、[!DNL Platform] APIを正しく呼び出すために知っておく必要がある追加情報について説明します。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。サンプル API 呼び出しのドキュメントで使用されている規則については、[!DNL Experience Platform] トラブルシューテングガイドの[サンプル API 呼び出しの読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- Authorization: Bearer `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソースは、特定の仮想サンドボックスに分離されています。 [!DNL Platform] APIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform]のサンドボックスについて詳しくは、[サンドボックスの概要ドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

- Content-Type: application/json

## セグメント定義{#merge-policy}の結合ポリシーを検索します

このワークフローでは、最初に既知のオーディエンスセグメントにアクセスします。[!DNL Real-time Customer Profile]で使用できるセグメントは、セグメント定義内にマージポリシーIDを含みます。 この結合ポリシーには、セグメントに含めるデータセットに関する情報があります。さらに、データセットには適用可能なデータ使用ラベルが含まれています。

[!DNL Segmentation] APIを使用して、IDでセグメント定義を検索し、関連付けられている結合ポリシーを見つけることができます。

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
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
| `mergePolicyId` | セグメント定義に使用される結合ポリシーの ID。これは次の手順で使用します。 |

## 結合ポリシーからソースデータセットを検索する  {#datasets}

マージ・ポリシーには、ソース・データセットに関する情報が含まれ、ソース・データセットにはデータ使用ラベルが含まれます。 [!DNL Profile] APIにGETリクエストでマージポリシーIDを指定すると、マージポリシーの詳細を参照できます。 結合ポリシーの詳細については、[結合ポリシーエンドポイントガイド](../../profile/api/merge-policies.md)を参照してください。

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
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、結合ポリシーの詳細を返します。

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
| `schema.name` | 結合ポリシーに関連付けられているスキーマの名前。 |
| `attributeMerge.type` | 結合ポリシーのデータ優先の設定タイプ。値が `dataSetPrecedence` の場合、この結合ポリシーに関連付けられているデータセットが `attributeMerge > data > order` の下に表示されます。値が `timestampOrdered` の場合、`schema.name` で参照されているスキーマに関連付けられているすべてのデータセットが結合ポリシーで使用されます。 |
| `attributeMerge.data.order` | `attributeMerge.type` が `dataSetPrecedence` の場合、この属性は、結合ポリシーで使用されるデータセットの IDを含む配列になります。これらの ID は次の手順で使用します。 |

## ポリシー違反のデータセットを評価

>[!NOTE]
>
> この手順では、特定のラベルを含むデータに対して特定のマーケティング操作が実行されないようにする、アクティブなデータ使用ポリシーが少なくとも1つあることを前提としています。 評価対象のデータセットに対して適切な使用ポリシーがない場合は、[ポリシー作成チュートリアル](../../data-governance/policies/create.md)に従って作成し、次の手順に進んでください。

マージポリシーのソースデータセットのIDを取得したら、[Policy Service API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml)を使用して、データ使用ポリシーの違反を確認するために、特定のマーケティング操作に対するデータセットを評価できます。

データセットを評価するには、次の例に示すように、POSTリクエストのパスにマーケティングアクションの名前を指定し、リクエスト本文内にデータセットIDを指定する必要があります。

**API 形式**

```http
POST /marketingActions/core/{MARKETING_ACTION_NAME}/constraints
POST /marketingActions/custom/{MARKETING_ACTION_NAME}/constraints
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | データセットを評価するデータ使用ポリシーに関連付けられたマーケティングアクションの名前。 ポリシーがAdobeによって定義されたか組織で定義されたかに応じて、それぞれ`/marketingActions/core`または`/marketingActions/custom`を使用する必要があります。 |

**リクエスト**

次のリクエストは、[前の手順](#datasets)で取得したデータセットに対して`exportToThirdParty`マーケティングアクションをテストします。 リクエストペイロードは、各データセットのIDを含む配列です。

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
| `entityType` | ペイロード配列の各項目は、定義するエンティティのタイプを示す必要があります。この使用例では、値は常に「dataSet」になります。 |
| `entityID` | ペイロード配列の各項目は、データセットの一意の ID を提供する必要があります。 |

**応答**

成功した応答は、マーケティングアクションのURI、指定されたデータセットから収集されたデータ使用ラベル、およびこれらのラベルに対するアクションのテストの結果として違反されたデータ使用ポリシーのリストを返します。 この例では、`violatedPolicies`配列に「データをサードパーティにエクスポート」ポリシーが表示され、マーケティング操作がポリシー違反をトリガーしたことを示しています。

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
| `violatedPolicies` | 指定された`duleLabels`に対するマーケティングアクション（`marketingActionRef`で指定）のテストで違反したデータ使用ポリシーをリストした配列。 |

API応答で返されるデータを使用して、エクスペリエンスアプリケーション内でプロトコルを設定し、ポリシー違反が発生した場合に適切にポリシー違反を適用することができます。

## データフィールドをフィルタリングする

オーディエンスセグメントが評価に合格しない場合は、次に示す2つの方法のいずれかを使用して、セグメントに含まれるデータを調整できます。

### セグメント定義の結合ポリシーを更新する

セグメント定義の結合ポリシーを更新すると、セグメントジョブの実行時に含まれるデータセットとフィールドが調整されます。詳しくは、APIマージポリシーチュートリアルの既存のマージポリシー](../../profile/api/merge-policies.md#update)の更新に関する節を参照してください。[

### セグメントの書き出し時に特定のデータフィールドを制限する

[!DNL Segmentation] APIを使用してセグメントをデータセットにエクスポートする場合、`fields`パラメーターを使用して、エクスポートに含まれるデータをフィルターできます。 このパラメーターに追加したデータフィールドは書き出しに含まれ、その他のデータフィールドはすべて除外されます。

「A」、「B」および「C」という名前のデータフィールドを持つセグメントについて考えてみます。フィールド「C」のみを書き出す場合、`fields` パラメーターにはフィールド「C」だけを指定します。こうすることで、セグメントを書き出す際に、「A」と「B」のフィールドが除外されます。

詳しくは、セグメント化のチュートリアルで、[セグメントの書き出し](./evaluate-a-segment.md#export)に関する節を参照してください。

## 次の手順

このチュートリアルでは、オーディエンスセグメントに関連付けられているデータ使用ラベルを検索し、特定のマーケティングアクションに対してポリシー違反の有無をテストしました。[!DNL Experience Platform]の[!DNL Data Governance]についての詳細は、[[!DNL Data Governance]](../../data-governance/home.md)の概要をお読みください。
