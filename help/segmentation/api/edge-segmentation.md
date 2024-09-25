---
solution: Experience Platform
title: API を使用したエッジのセグメント化
description: このドキュメントでは、Adobe Experience Platform Segmentation Service API でエッジのセグメント化を使用する方法の例を示します。
role: Developer
exl-id: effce253-3d9b-43ab-b330-943fb196180f
source-git-commit: a1c9003a1b219325daf8fa38cda8bb1a019a55c6
workflow-type: tm+mt
source-wordcount: '783'
ht-degree: 79%

---

# エッジのセグメント化

>[!NOTE]
>
>次のドキュメントでは、API を使用してエッジのセグメント化を実行する方法を説明します。 UI を使用してエッジのセグメント化を実行する方法については、[エッジセグメント化 UI ガイド](../ui/edge-segmentation.md)を参照してください。
>
>エッジセグメント化は、すべての Platform ユーザーが一般に使用できるようになりました。ベータ版でエッジセグメント定義を作成した場合、これらのセグメント定義は引き続き動作します。

Edgeのセグメント化は、Adobe Experience Platformのセグメント定義をエッジ上で即座に評価する機能で、これにより、同じページや次のページのパーソナライゼーションのユースケースが可能になります。

>[!IMPORTANT]
>
> エッジデータは、収集された場所に最も近いエッジサーバーの場所に保存されるので、ハブ（またはプリンシパル）Adobe Experience Platform データセンターとして指定された場所以外に保存される可能性があります。
>
> さらに、エッジセグメント化エンジンは、プライマリとしてマークされた&#x200B;**単一**&#x200B;の ID（エッジベース以外のプライマリ ID と一致するもの）があるエッジ上のリクエストのみに従います。

## はじめに

この開発者ガイドでは、エッジのセグメント化に関連する様々な [!DNL Adobe Experience Platform] サービスについての十分な知識が必要です。このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-Time Customer Profile]](../../profile/home.md)：複数のソースから集約されたデータに基づいて、統合された消費者プロファイルをリアルタイムで提供します。
- [[!DNL Adobe Experience Platform Segmentation Service]](../home.md):[!DNL Real-Time Customer Profile] データからオーディエンスを作成できます。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：[!DNL Platform] が、カスタマーエクスペリエンスデータを整理する際に使用する、標準化されたフレームワーク。

Experience Platform API エンドポイントへの呼び出しを正常に行うには、[Platform API の基本を学ぶ](../../landing/api-guide.md)のガイドを読み、必要なヘッダーとサンプル API 呼び出しの読み方を確認してください。

## エッジセグメント化のクエリタイプ {#query-types}

エッジセグメント化を使用してセグメントを評価するには、次のガイドラインに従ってクエリを行う必要があります。

| クエリタイプ | 詳細 |
| ---------- | ------- |
| 24 時間未満の時間枠内での単一イベント | 24 時間未満の時間枠内に 1 つの受信イベントを参照する任意のセグメント定義。 |
| プロファイルのみ | プロファイル属性のみを参照するセグメント定義。 |
| 24 時間未満の相対時間枠内でのプロファイル属性を持つ単一のイベント | 1 つ以上のプロファイル属性を持つ 1 つの受信イベントを参照し、24 時間未満の相対時間枠内に発生するセグメント定義。 |
| セグメントのセグメント | 1 つ以上のバッチセグメントまたはストリーミングセグメントを含むセグメント定義。 **メモ：**&#x200B;セグメントのセグメントが使用される場合、**24 時間ごとに**&#x200B;プロファイルの不適合が発生します。 |
| プロファイル属性を持つ複数のイベント | **過去 24 時間以内に**&#x200B;複数のイベントを参照し、（オプションで）1 つ以上のプロファイル属性を持つ任意のセグメント定義。 |

また、セグメントが、エッジ上でアクティブな結合ポリシーに結び付けられている&#x200B;**必要があります**。 結合ポリシーの詳細については、[結合ポリシーガイド](../../profile/api/merge-policies.md)を参照してください。

次のシナリオでは、セグメント定義はエッジセグメント化に対して有効に&#x200B;**なりません**。

- セグメント定義には、単一のイベントと `inSegment` イベントの組み合わせが含まれています。
   - ただし、`inSegment` イベントに含まれるセグメントがプロファイルのみの場合、セグメント定義はエッジセグメント化に対して有効に&#x200B;**なります**。
- セグメント定義では、時間制約の一部として「年を無視」を使用します。

## エッジセグメント化で有効なすべてのセグメントの取得

`/segment/definitions` エンドポイントにGETリクエストを行うことで、組織内でエッジセグメント化が有効になっているすべてのセグメントのリストを取得できます。

**API 形式**

エッジセグメント化が有効になっているセグメントを取得するには、リクエストパスにクエリパラメーター `evaluationInfo.synchronous.enabled=true` を含める必要があります。

```http
GET /segment/definitions?evaluationInfo.synchronous.enabled=true
```

**リクエスト**

```shell
curl -X GET \
  'https://platform.adobe.io/data/core/ups/segment/definitions?evaluationInfo.synchronous.enabled=true' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、エッジセグメント化が有効になっている、組織内のセグメントの配列が返されます。 返されるセグメント定義について詳しくは、[セグメント定義エンドポイントガイド](./segment-definitions.md)を参照してください。

```json
{
    "segments": [
        {
            "id": "15063cb-2da8-4851-a2e2-bf59ddd2f004",
            "schema": {
                "name": "_xdm.context.profile"
            },
            "imsOrgId": "{ORG_ID}",
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
            "imsOrgId": "{ORG_ID}",
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

エッジセグメント化が有効なセグメントを作成するには、[上記のエッジセグメント化クエリタイプ](#query-types)のいずれかと一致する `/segment/definitions` エンドポイントに対して POST リクエストを行います。

**API 形式**

```http
POST /segment/definitions
```

**リクエスト**

>[!NOTE]
>
>次の例は、セグメントを作成するための標準的なリクエストです。 セグメント定義の作成について詳しくは、[セグメントの作成](../tutorials/create-a-segment.md)に関するチュートリアルを参照してください。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/segment/definitions \
  -H 'Authorization: Bearer {ACCESS_TOKEN}'  \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "schema": {
        "name": "_xdm.context.profile"
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
    }
}'
```

**応答**

リクエストが成功すると、エッジセグメント化が有効な新規作成のセグメント定義の詳細が応答として返されます。

```json
{
    "id": "f15063cb-2da8-4851-a2e2-bf59ddd2f004",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "imsOrgId": "{ORG_ID}",
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
        "value": "chain(xEvent, timestamp, [X: WHAT(var1._experience.analytics.endUser.firstWeb.webPageDetails.isHomePage = "true")])"
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

これで、エッジセグメント化が有効なセグメントの作成方法がわかったので、それらを使用して、同じページと次のページのパーソナライゼーションユースケースを有効にすることができます。

Adobe Experience Platform ユーザーインターフェイスを使用して同様のアクションを実行しセグメントを操作する方法については、[セグメントビルダーユーザーガイド](../ui/segment-builder.md)を参照してください。

## 付録

次の節では、エッジセグメント化に関するよくある質問を一覧表示します。

### Edge Network 上でセグメントが使用可能になるまで、どれくらいかかりますか？

Edge Network 上でセグメントが使用可能になるまで、最大 1 時間かかります。
