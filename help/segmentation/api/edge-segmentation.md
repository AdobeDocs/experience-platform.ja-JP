---
keywords: Experience Platform；ホーム；人気のあるトピック；セグメント化；セグメント化；セグメント化サービス；エッジセグメント化；エッジセグメント化；ストリーミングエッジ；
solution: Experience Platform
title: 'APIを使用したエッジセグメント '
topic-legacy: developer guide
description: このドキュメントでは、Adobe Experience PlatformセグメントサービスAPIでエッジセグメントを使用する方法の例を示します。
exl-id: effce253-3d9b-43ab-b330-943fb196180f
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '651'
ht-degree: 11%

---

# エッジセグメント（ベータ）

>[!NOTE]
>
>次のドキュメントでは、APIを使用したエッジセグメント化の実行方法を示します。 UIを使用したエッジセグメント化の実行について詳しくは、[エッジセグメント化UIガイド](../ui/edge-segmentation.md)を参照してください。 また、エッジセグメントは現在ベータ版です。 ドキュメントと機能は変更される場合があります。

エッジセグメント化は、エッジ上でAdobe Experience Platformのセグメントを瞬時に評価する機能で、同じページや次のページのパーソナライゼーションの使用例を実現します。

## はじめに

この開発者ガイドでは、エッジのセグメント化に関連する様々な[!DNL Adobe Experience Platform]サービスについて、十分に理解している必要があります。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、リアルタイムで統一された消費者プロファイルを提供します。
- [[!DNL Segmentation]](../home.md):デー [!DNL Real-time Customer Profile] タからセグメントやオーディエンスを作成できます。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：顧客体験データを編成する際に [!DNL Platform] に使用される標準化されたフレームワーク。

[!DNL Data Prep] APIエンドポイントの呼び出しを正しく行うために、必要なヘッダーとサンプルAPI呼び出しの読み方については、[プラットフォームAPIの使い始めに](../../landing/api-guide.md)のガイドを読んでください。

## エッジセグメントクエリタイプ{#query-types}

エッジセグメントを使用してセグメントを評価するには、クエリが次のガイドラインに従う必要があります。

| クエリタイプ | 詳細 |
| ---------- | ------- |
| 受信ヒット | 時間制限のない、単一の着信イベントを参照するセグメント定義。 |
| プロファイルを参照する着信ヒット | 時間制限のない、1つの着信イベント、および1つ以上のプロファイル属性を参照するセグメント定義。 |
| 周波数クエリ | 少なくとも一定回数発生するイベントを参照するセグメント定義。 |
| プロファイルを参照する頻度クエリ | 少なくとも一定回数発生するイベントを参照し、1つ以上のプロファイル属性を持つセグメント定義。 |

{style=&quot;table-layout:auto&quot;}

次のクエリタイプは、エッジセグメントでは現在&#x200B;****&#x200B;サポートされていません。

| クエリタイプ | 詳細 |
| ---------- | ------- |
| 相対時間枠 | クエリが時間枠を参照する場合、エッジセグメントを使用して評価することはできません。 |
| 否定 | クエリに負の値または`not`イベントが含まれる場合、エッジセグメントを使用して評価することはできません。 |
| 複数のイベント | クエリに複数のイベントが含まれる場合、エッジセグメントを使用して評価することはできません。 |

{style=&quot;table-layout:auto&quot;}

## エッジセグメント化で有効になっているすべてのセグメントを取得

`/segment/definitions`エンドポイントにGETリクエストを行うことで、IMS組織内のエッジセグメント化が有効になっているすべてのセグメントのリストを取得できます。

**API 形式**

エッジセグメント化が有効なセグメントを取得するには、リクエストパスにクエリパラメーター`evaluationInfo.synchronous.enabled=true`を含める必要があります。

```http
GET /segment/definitions?evaluationInfo.synchronous.enabled=true
```

**リクエスト**

```shell
curl -X GET \
  'https://platform.adobe.io/data/core/ups/segment/definitions?evaluationInfo.synchronous.enabled=true' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答が返されると、IMS組織内の、エッジセグメントが有効なセグメントの配列が返されます。 返されるセグメント定義に関する詳細は、『[セグメント定義エンドポイントガイド](./segment-definitions.md)』を参照してください。

```json
{
    "segments": [
        {
            "id": "15063cb-2da8-4851-a2e2-bf59ddd2f004",
            "schema": {
                "name": "_xdm.context.profile"
            },
            "ttlInDays": 30,
            "imsOrgId": "{IMS_ORG_ID}",
            "sandbox": {
                "sandboxId": "",
                "sandboxName": "",
                "type": "production",
                "default": true
            },
            "name": " People who are NOT on their homepage ",
            "expression": {
                "type": "PQL",
                "format": "pql/text",
                "value": "select var1 from xEvent where var1._experience.analytics.endUser.firstWeb.webPageDetails.isHomePage = false"
            },
            "evaluationInfo": {
                "batch": {
                    "enabled": false
                },
                "continuous": {
                    "enabled": false
                },
                "synchronous": {
                    "enabled": true
                }
            },
            "creationTime": 1572029711000,
            "updateEpoch": 1572029712000,
            "updateTime": 1572029712000
        },
        {
            "id": "f15063cb-2da8-4851-a2e2-bf59ddd2f004",
            "schema": {
                "name": "_xdm.context.profile"
            },
            "ttlInDays": 30,
            "imsOrgId": "{IMS_ORG_ID}",
            "sandbox": {
                "sandboxId": "",
                "sandboxName": "",
                "type": "production",
                "default": true
            },
            "name": "Homepage_continuous",
            "description": "People who are on their homepage - continuous",
            "expression": {
                "type": "PQL",
                "format": "pql/text",
                "value": "select var1 from xEvent where var1._experience.analytics.endUser.firstWeb.webPageDetails.isHomePage = true"
            },
            "evaluationInfo": {
                "batch": {
                    "enabled": false
                },
                "continuous": {
                    "enabled": false
                },
                "synchronous": {
                    "enabled": true
                }
            },
            "creationTime": 1572021085000,
            "updateEpoch": 1572021086000,
            "updateTime": 1572021086000
        }
    ],
    "page": {
        "totalCount": 2,
        "totalPages": 1,
        "sortField": "creationTime",
        "sort": "desc",
        "pageSize": 2,
        "limit": 100
    },
    "link": {}
}
```

## エッジセグメントを有効にするセグメントの作成

`/segment/definitions`エンドポイントにPOSTリクエストを行うことで、エッジセグメント化を有効にするセグメントを作成できます。 ](#query-types)の上に示した[エッジセグメント化クエリのタイプの1つと一致するほか、ペイロードの`evaluationInfo.synchronous.enabled`フラグをtrueに設定する必要があります。

**API 形式**

```http
POST /segment/definitions
```

**リクエスト**

>[!NOTE]
>
>次の例は、セグメントを作成するための標準的なリクエストです。 セグメント定義の作成についての詳細は、[セグメント](../tutorials/create-a-segment.md)の作成のチュートリアルをお読みください。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/segment/definitions \
  -H 'Authorization: Bearer {ACCESS_TOKEN}'  \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "schema": {
        "name": "_xdm.context.profile"
    },
    "ttlInDays": 30,
    "name": "Homepage_continuous",
    "description": "People who are on their homepage - continuous",
    "expression": {
        "type": "PQL",
        "format": "pql/text",
        "value": "select var1 from xEvent where var1._experience.analytics.endUser.firstWeb.webPageDetails.isHomePage = true"
    },
    "evaluationInfo": {
        "synchronous": {
            "enabled": true
        }
    }
}'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `evaluationInfo.synchronous.enabled` | `evaluationInfo`オブジェクトは、セグメント定義が受ける評価のタイプを決定します。 エッジセグメントを使用するには、`evaluationInfo.synchronous.enabled`を`true`の値で設定します。 |

**応答**

正常に完了すると、エッジセグメント化が有効な、新しく作成されたセグメント定義の詳細が返されます。

```json
{
    "id": "f15063cb-2da8-4851-a2e2-bf59ddd2f004",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "ttlInDays": 30,
    "imsOrgId": "{IMS_ORG}",
    "sandbox": {
        "sandboxId": "{SANDBOX_ID}",
        "sandboxName": "{SANDBOX_NAME}",
        "type": "production",
        "default": true
    },
    "name": "Homepage_continuous",
    "description": "People who are on their homepage - continuous",
    "expression": {
        "type": "PQL",
        "format": "pql/text",
        "value": "select var1 from xEvent where var1._experience.analytics.endUser.firstWeb.webPageDetails.isHomePage = true"
    },
    "evaluationInfo": {
        "batch": {
            "enabled": false
        },
        "continuous": {
            "enabled": false
        },
        "synchronous": {
            "enabled": true
        }
    },
    "creationTime": 1572021085000,
    "updateEpoch": 1572021086000,
    "updateTime": 1572021086000
}
```

## 次の手順

エッジセグメント化が有効なセグメントの作成方法がわかったので、これらのセグメントを使用して、同じページと次のページのパーソナライズの使用例を有効にできます。

Adobe Experience Platform ユーザーインターフェイスを使用して同様のアクションを実行し、セグメントを操作する方法については、『[セグメントビルダーユーザーガイド](../ui/segment-builder.md)』を参照してください。
