---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データレークでのプライバシー要求の処理
topic: overview
translation-type: tm+mt
source-git-commit: 409d98818888f2758258441ea2d993ced48caf9a

---


# データレークでのプライバシー要求の処理

Adobe Experience Platform Privacy Serviceは、General Data Protection Regulation(GDPR)やCalifornia Consumer Privacy Act(CCPA)などのプライバシー規制に基づいて定義された個人データのアクセス、販売、または削除を求める顧客の要求を処理します。

このドキュメントでは、Data Lakeに保存された顧客データのプライバシー要求の処理に関する基本的な概念について説明します。

## はじめに

このガイドを読む前に、次のExperience Platformサービスに関する十分な理解を得ることをお勧めします。

* [プライバシーサービス](../privacy-service/home.md):Adobe Experience Cloudアプリケーションで個人データにアクセス、オプトアウト、または削除する際の顧客の要求を管理します。
* [Catalog Service](home.md):エクスペリエンスプラットフォーム内のデータの場所と系列のレコードのシステム。 データセットメタデータの更新に使用できるAPIを提供します。
* [Experience Data Model(XDM)System](../xdm/home.md):エクスペリエンスプラットフォームが顧客エクスペリエンスデータを整理するための標準化されたフレームワーク。

## データセットへのプライバシーラベルの追加 {#privacy-labels}

データセットをData Lakeのプライバシーリクエストで処理するには、データセットにプライバシーラベルを付与する必要があります。 プライバシーラベルは、データセットに関連付けられたスキーマ内のどのフィールドが、プライバシー要求で送信されると予想される名前空間に適用されるかを示します。

この節では、Data Lakeのプライバシーリクエストで使用するプライバシーラベルをデータセットに追加する方法について説明します。 まず、以下のデータセットを考えてみましょう。

```json
{
    "5d8e9cf5872f18164763f971": {
        "name": "Loyalty Members",
        "description": "Dataset for the Loyalty Members schema",
        "imsOrg": "{IMS_ORG}",
        "tags": {
            "adobe/pqs/table": [
                "loyalty_members"
            ]
        },
        "namespace": "ACP",
        "state": "DRAFT",
        "id": "5d8e9cf5872f18164763f971",
        "dule": {
            "identity": [],
            "contract": [
                "C2",
                "C5"
            ],
            "sensitive": [],
            "contracts": [
                "C2",
                "C5"
            ],
            "identifiability": [],
            "specialTypes": []
        },
        "version": "1.0.2",
        "created": 1569627381749,
        "updated": 1569880723282,
        "createdClient": "acp_ui_platform",
        "createdUser": "{USER_ID}",
        "updatedUser": "{USER_ID}",
        "viewId": "5d8e9cf5872f18164763f972",
        "status": "enabled",
        "fileDescription": {
            "persisted": true,
            "containerFormat": "parquet",
            "format": "parquet"
        },
        "files": "@/dataSets/5d8e9cf5872f18164763f971/views/5d8e9cf5872f18164763f972/files",
        "schemaMetadata": {
            "primaryKey": [],
            "delta": [],
            "dule": [
                {
                    "path": "/properties/personalEmail/properties/address",
                    "identity": [
                        "I1"
                    ],
                    "contract": [],
                    "sensitive": [],
                    "contracts": [],
                    "identifiability": [
                        "I1"
                    ],
                    "specialTypes": []
                }
            ],
            "gdpr": []
        },
        "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/2c66c3a4323128d3701289df4468e8a6",
            "contentType": "application/vnd.adobe.xed-full+json;version=1"
        }
    }
}
```

データ `schemaMetadata` セットのプロパティには、現在空 `gdpr` の配列が含まれています。 データセットにプライバシーラベルを追加するには、 [Catalog Service APIへのPATCHリクエストを使用して、この配列を更新する必要があります](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml)。

>[!NOTE] このアレイには名前が付けられ `gdpr`ていますが、ラベルを追加することで、GDPRとCCPAの両方の規制に対するプライバシー・ジョブの要求が可能になります。

**API形式**

```http
PATCH /dataSets/{DATASET_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | 更新 `id` するデータセットの値。 |

**リクエスト**

この例では、データセットのフィールドに電子メールアドレスが含まれ `personalEmail.address` ています。 このフィールドをData Lakeのプライバシーリクエストの識別子として機能させるには、未登録の名前空間を使用するラベルを配列に追加する必要があ `gdpr` ります。

次のリクエストでは、プライバシーラベルを追加し、名前空間「email_label」をフィールドに割り当て `personalEmail.address` ます。

>[!IMPORTANT] データセットのプロパティに対してPATCH操作を実行する `schemaMetadata` 場合は、リクエストのペイロード内の既存のサブプロパティを必ずコピーしてください。 ペイロードから既存の値を除外すると、データセットから削除されます。

```shell
curl -X PATCH 'https://platform.adobe.io/data/foundation/catalog/dataSets/5d8e9cf5872f18164763f971' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Content-Type: application/json' \
  -d '{ 
    "schemaMetadata": { 
      "primaryKey": [],
      "delta": [],
      "dule": [
        {
          "path": "/properties/personalEmail/properties/address",
          "identity": [
              "I1"
          ],
          "contract": [],
          "sensitive": [],
          "contracts": [],
          "identifiability": [
              "I1"
          ],
          "specialTypes": []
        }
      ],
      "gdpr": [
          {
              "namespace": ["email_label"],
              "path": "/properties/personalEmail/properties/address"
          }
      ]
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `namespace` | で指定したフィールドに関連付ける名前空間をリストする配列 `path`。 名前空間は、プライバシーサービスAPIでアクセスまたは削除の要求を [送信する際に、プライバシー関連のフィールドを](#submit) 識別するために使用されます。 |
| `path` | に適用される、データセットに関連付けられたスキーマ内のフィールドへのパスで `namespace`す。 プライバシーラベルは、「リーフ」フィールド（サブフィールドのないフィールド）にのみ適用するのが理想的です。 |

**応答**

成功した応答は、ペイロードで提供されたデータセットのIDを持つHTTPステータス200(OK)を返します。 このIDを使用してデータセットを再度調べると、プライバシーラベルが追加されていることが明らかになります。

```json
[
    "@/dataSets/5d8e9cf5872f18164763f971"
]
```

### ネストされたマップタイプフィールドのラベル付け

プライバシーのラベル付けをサポートしないネストされたマップタイプのフィールドは、次の2種類あります。

* 配列型フィールド内のmap-typeフィールド
* 別のmap-typeフィールド内のmap-typeフィールド

上記の2つの例のいずれのプライバシージョブの処理も、最終的に失敗します。 このため、顧客のプライベートデータを保存する際に、ネストされたマップタイプのフィールドを使用しないようにすることをお勧めします。 関連するコンシューマIDは、レコードベースのデータセットの場合はフィールド（自身はマップタイプのフィールド）内の非マップデータ型、時系列ベースのデータセットの場合はフィールドとし `identityMap``endUserID` て保存する必要があります。

## 要求の送信 {#submit}

>[!NOTE] この節では、Data Lakeのプライバシー要求の形式を設定する方法について説明します。 リクエストペイロードで送信されたユーザーIDデータを適切にフォーマットする方法など、プライバシージョブの送信方法に関する完全な手順については、 [Privacy Service API](../privacy-service/api/getting-started.md) （プライバシーサービスAPI）または [Privacy Service UI](../privacy-service/ui/overview.md) （プライバシーサービスUI）のドキュメントを確認することを強くお勧めします。

次の節では、プライバシーサービスAPIまたはUIを使用してData Lakeのプライバシーリクエストを行う方法について概要を説明します。

### APIの使用

APIでジョブリクエストを作成する場合、提供され `userIDs` るすべてのユーザーは、適用するデータスト `namespace` アに応じ `type` て、特定のを使用する必要があります。 Data LakeのIDは、値に「未登録」を使用し、適用可能なデータセットに追加さ `type` れたプライバシーラベル `namespace`[](#privacy-labels) (プライバシーラベル)と一致する値を使用する必要があります。

さらに、要求ペイロ `include` ードの配列には、要求が行われる別のデータストアの製品値を含める必要があります。 データレークにリクエストを行う場合、配列には「aepDataLake」という値を含める必要があります。

次のリクエストは、未登録の「email_label」ジョブを使用して、Data Lakeの新しいプライバシージョブを作成します。名前空間 また、アレイ内のData Lakeの製品値も含まれ `include` ます。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/privacy/jobs \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -d '{
    "companyContexts": [
      {
        "namespace": "imsOrgID",
        "value": "{IMS_ORG}"
      }
    ],
    "users": [
      {
        "key": "user12345",
        "action": ["access","delete"],
        "userIDs": [
          {
            "namespace": "email_label",
            "value": "ajones@acme.com",
            "type": "unregistered"
          },
          {
            "namespace": "email_label",
            "value": "jdoe@example.com",
            "type": "unregistered"
          }
        ]
      }
    ],
    "include": ["aepDataLake"],
    "expandIds": false,
    "priority": "normal",
    "regulation": "ccpa"
}'
```

### UIの使用

UIでジョブリクエストを作成する場合は、「 **Products** 」の下の「 **AEP Data Lake** 」または「 __ プロファイル」を必ず選択し、「Data Lake」または「リアルタイム顧客プロファイル」に保存されたデータのジョブを処理します。

<img src="images/privacy/product-value.png" width="450"><br>

## リクエスト処理の削除

Experience Platformがプライバシーサービスから削除リクエストを受信すると、プラットフォームは、リクエストが受信され、影響を受けるデータが削除用にマークされたことをプライバシーサービスに確認するメッセージを送信します。 その後、7日以内にレコードがデータレークから削除されます。 この7日間の期間中は、データはソフト削除されるので、どのプラットフォームサービスからもアクセスできません。

今後のリリースでは、データが物理的に削除された後、プラットフォームはプライバシーサービスに確認を送信します。

## 次の手順

このドキュメントを読むことで、Data Lakeのプライバシーリクエストの処理に関する重要な概念を紹介します。 IDデータの管理方法とプライバシージョブの作成方法に関する理解を深めるために、このガイド全体に記載されているドキュメントを読み続けることをお勧めします。

ドキュメントストアのプライバ [シー要求を処理する手順については、リアルタイム顧客プロファイルの](../profile/privacy.md) 「プライバシー要求の処理に関するプロファイル」を参照してください。