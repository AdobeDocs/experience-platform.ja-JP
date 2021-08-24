---
keywords: Experience Platform；ホーム；人気のあるトピック；セグメント化；セグメント化；セグメント化サービス；エッジセグメント化；エッジセグメント化；ストリーミングエッジ；
solution: Experience Platform
title: 'APIを使用したエッジのセグメント化 '
topic-legacy: developer guide
description: このドキュメントでは、Adobe Experience Platform Segmentation Service APIでエッジセグメントを使用する方法の例を示します。
exl-id: effce253-3d9b-43ab-b330-943fb196180f
source-git-commit: af1eee8787d7fa2ae2d56e541823100d2620dd2d
workflow-type: tm+mt
source-wordcount: '633'
ht-degree: 17%

---

# エッジのセグメント化（ベータ版）

>[!NOTE]
>
>次のドキュメントでは、APIを使用してエッジセグメント化を実行する方法について説明します。 UIを使用したエッジセグメント化の実行について詳しくは、『[エッジセグメント化UIガイド](../ui/edge-segmentation.md)』を参照してください。 さらに、エッジセグメント化は現在ベータ版です。 ドキュメントと機能は変更される場合があります。

エッジセグメント化とは、Adobe Experience Platformのセグメントを瞬時にエッジ上で評価し、同じページおよび次のページのパーソナライゼーションの使用例を可能にする機能です。

## はじめに

この開発者ガイドでは、エッジセグメント化に関連する様々な[!DNL Adobe Experience Platform]サービスに関する十分な知識が必要です。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、統合された消費者プロファイルをリアルタイムで提供します。
- [[!DNL Segmentation]](../home.md):データからセグメントやオーディエンスを作成する機能を提 [!DNL Real-time Customer Profile] 供します。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：顧客体験データを編成する際に [!DNL Platform] に使用される標準化されたフレームワーク。

[!DNL Data Prep] API エンドポイントへの呼び出しを正常におこなうには、[Platform API の概要](../../landing/api-guide.md)を読み、必要なヘッダーとサンプル API 呼び出しの読み込み方法を確認してください。

## エッジセグメント化のクエリのタイプ {#query-types}

エッジセグメント化を使用してセグメントを評価するには、クエリが次のガイドラインに従う必要があります。

| クエリタイプ | 詳細 |
| ---------- | ------- |
| 受信ヒット | 時間制限のない単一の受信イベントを参照するセグメント定義。 |
| プロファイルを参照する受信ヒット | 時間制限のない単一の受信イベントを参照するセグメント定義、1つ以上のプロファイル属性。 |
| 24時間の時間枠で受信ヒット | 24時間以内に発生する単一のイベントを参照するセグメント定義 |
| 時間枠が24時間のプロファイルを参照する受信ヒット | 24時間以内の単一の受信イベントを参照するセグメント定義、および1つ以上のプロファイル属性 |

{style=&quot;table-layout:auto&quot;}

次のクエリタイプは、エッジセグメント化で現在サポートされている&#x200B;**ではありません**。

| クエリタイプ | 詳細 |
| ---------- | ------- |
| 複数のイベント | クエリに複数のイベントが含まれる場合、エッジセグメント化を使用して評価することはできません。 |
| 頻度クエリ | 少なくとも一定回数発生するイベントを参照するセグメント定義。 |
| プロファイルを参照する頻度クエリ | 少なくとも一定回数発生するイベントを参照し、1つ以上のプロファイル属性を持つセグメント定義。 |

{style=&quot;table-layout:auto&quot;}

## エッジセグメント化が有効なすべてのセグメントの取得

IMS組織内のエッジセグメント化で有効になっているすべてのセグメントのリストを取得するには、`/segment/definitions`エンドポイントにGETリクエストを送信します。

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

正常な応答は、エッジセグメント化が有効なIMS組織内のセグメントの配列を返します。 返されるセグメント定義に関する詳細については、『[セグメント定義エンドポイントガイド](./segment-definitions.md)』を参照してください。

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

## エッジセグメント化が有効なセグメントの作成

上記の](#query-types)に示した[エッジセグメント化クエリタイプの1つに一致する`/segment/definitions`エンドポイントにPOSTリクエストを送信して、エッジセグメント化が有効なセグメントを作成できます。

**API 形式**

```http
POST /segment/definitions
```

**リクエスト**

>[!NOTE]
>
>次の例は、セグメントを作成するための標準的なリクエストです。 セグメント定義の作成の詳細については、[セグメント](../tutorials/create-a-segment.md)の作成に関するチュートリアルを参照してください。

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
    }
}'
```

**応答**

正常な応答は、新しく作成され、エッジセグメント化が有効になっているセグメント定義の詳細を返します。

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

エッジセグメント化が有効なセグメントの作成方法がわかったので、それらを使用して同じページおよび次のページのパーソナライゼーションの使用例を有効にできます。

Adobe Experience Platform ユーザーインターフェイスを使用して同様のアクションを実行し、セグメントを操作する方法については、『[セグメントビルダーユーザーガイド](../ui/segment-builder.md)』を参照してください。
