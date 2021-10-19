---
keywords: 経験のあるプラットホーム、home、ポピュラーな話題、セグメンテーション、セグメンテーション、分類セグメンテーションサービス、エッジセグメンテーション、Edge セグメンテーション。ストリーミングエッジ;
solution: Experience Platform
title: 'API を使用した Edge セグメンテーション '
topic-legacy: developer guide
description: このドキュメントでは、Adobe エクスペリエンスプラットフォームセグメンテーションサービス API でのエッジセグメンテーションの使用方法の例について説明しています。
exl-id: effce253-3d9b-43ab-b330-943fb196180f
source-git-commit: c89971668839555347e9b84c7c0a4ff54a394c1a
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 8%

---

# Edge セグメンテーション (ベータ版)

>[!NOTE]
>
>次のドキュメントでは、API を使用して edge セグメンテーションを実行する方法について説明します。 UI を使用して edge セグメンテーションを実行する方法について詳しくは、 [ エッジセグメンテーション UI ガイドを参照してください ](../ui/edge-segmentation.md) 。 さらに、現在のところ、エッジセグメンテーションがベータに追加されています。 ドキュメントと機能は変更される場合があります。

Edge segment は、Adobe Experience Platform のセグメントを即座にエッジ上に評価する機能を備えており、同じページおよび次のページのパーソナライズ用途に使用できます。

## はじめに

この開発ガイドでは、 [!DNL Adobe Experience Platform] エッジのセグメンテーションに関係する様々なサービスについて、理解を深める必要があります。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-time Customer Profile]](../../profile/home.md): 複数のソースから集められたデータに基づいて、統合されたコンシューマープロファイルがリアルタイムで提供されます。
- [[!DNL Segmentation]](../home.md): データからセグメントや対象ユーザーを作成する機能を提供し [!DNL Real-time Customer Profile] ます。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：顧客体験データを編成する際に [!DNL Platform] に使用される標準化されたフレームワーク。

経験のあるプラットフォーム API エンドポイントに対して適切な呼び出しを行うには、Platform Api 入門のガイドを参照して、 [ ](../../landing/api-guide.md) 必要なヘッダーと、サンプル API 呼び出しの読み取り方法について説明します。

## エッジのセグメンテーションクエリータイプ {#query-types}

エッジのセグメンテーションを使用してセグメントを評価するには、クエリーが次のガイドラインに従う必要があります。

| クエリタイプ | 詳細 | 例 |
| ---------- | ------- | ------- |
| 1つのイベント | 時間制限のない、単一の受信イベントを参照するセグメント定義。 | カートにアイテムを追加した人です。 |
| 1つのプロファイルを参照する単一イベント | 1つ以上のプロファイル属性を参照するセグメント定義と、時間制限のない1つの受信イベント | 「米国在住」によって、ホームページが表示されていました。 |
| 単一イベントとプロファイル属性の反転 | 符号が負の単一受信イベントと1つまたは複数のプロファイル属性を参照するセグメント定義 | 米国に住んでいて、 **** ホームページにアクセスしていない人物 |
| 24時間ウィンドウ内の1つのイベント | これは、24時間以内に1つの受信イベントを参照するセグメント定義です。 | 最近、24時間以内にホームページにアクセスした人です。 |
| 1つのイベントでは、プロファイル属性が24時間ウィンドウになります。 | 1つ以上のプロファイル属性を参照するセグメント定義と、24時間以内に反転した1つの受信イベントを表します。 | 過去24時間にホームページにアクセスしたことを米国在住にしていました。 |
| 1つのプロファイル属性を24時間ウィンドウ内に指定した場合の単一イベントの反転 | 1つ以上のプロファイル属性を参照するセグメント定義と、24時間以内に反転した1つの受信イベントを表します。 | **** 過去24時間にホームページにアクセスしていない場合に、米国在住の社員 |
| 24時間ウィンドウ内の Frequency イベント | 1つのタイムウィンドウ内に一定回数のイベントがある場合は、そのすべてのセグメントが24時間内に出現することを示します。 | これまでにホームページにアクセスしていた人物は **** 、過去24時間以内に5回表示されていました。 |
| 「24時間制」ウィンドウ内の profile 属性を含む Frequency イベント | 1つ以上のプロファイル属性を参照するセグメント定義と、24時間のタイムウィンドウ内で一定の回数のイベントが発生します。 | 過去24時間に、ホームページにアクセスした米国のユーザーが **5 回以上移動したと** します。 |
| 24時間ウィンドウ内のプロファイルに対する否定の frequency イベント | 1つ以上のプロファイル属性を参照するセグメント定義と、24時間のタイムウィンドウ内で指定された回数だけ反映される、負のイベント。 | これまでにホームページにアクセスしていない人物に対しては **** 、24時間以内に5回以上訪れていました。 |
| 24時間の時間プロファイル内に複数の受信ヒットがある場合 | 1つのタイムウィンドウの中で発生した複数のイベントを表すセグメント定義。 | このホームページを参照したり、またはその他のユーザーとの **** 間で、過去24時間に閲覧したページを参照していた |
| 24時間ウィンドウ内のプロファイルを使用した複数のイベント | 1つ以上のプロファイル属性を参照するセグメント定義と、24時間のタイムウィンドウ内で発生する複数のイベント | 米国から訪問したホームページにアクセスし、その後 **** 24 時間以内にチェックアウトページにアクセスした人物 |

{style=&quot;table-layout:auto&quot;}

## エッジのセグメンテーションが有効になっているすべてのセグメントを取得する

エンドポイントに GET 要求を作成することで、IMS 組織内のエッジセグメントに対して有効になっているすべてのセグメントのリストを取得することができ `/segment/definitions` ます。

**API 形式**

エッジセグメンテーションが有効になっているセグメントを取得するには、要求パスにクエリパラメーターを含める必要があり `evaluationInfo.synchronous.enabled=true` ます。

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

応答が成功した場合は、IMS 組織内のセグメントの配列が返されます。これにより、エッジセグメンテーションが有効になります。 返されるセグメント定義について詳しくは、 [ segment definition エンドポイントガイドを参照 ](./segment-definitions.md) してください。

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

## エッジのセグメンテーションが有効になっているセグメントの作成

`/segment/definitions` [ 上記のエッジセグメンテーションクエリータイプのいずれかと一致する POST 要求をエンドポイントに対して行うことにより、エッジセグメント化が有効になっているセグメントを作成することができ ](#query-types) ます。

**API 形式**

```http
POST /segment/definitions
```

**リクエスト**

>[!NOTE]
>
>次の例では、セグメントを作成する標準的な要求について説明します。 セグメントの定義を作成する方法について詳しくは、「セグメントの作成」のチュートリアルを参照してください [ ](../tutorials/create-a-segment.md) 。

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

応答が成功すると、エッジのセグメンテーションが有効になっている、新しく作成されたセグメントの定義の詳細が返されます。

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

これで、エッジセグメントが有効になっているセグメントを作成する方法を理解できたので、この2つのセグメントを使用して、同じページおよび次ページの個人設定を行うことができます。

Adobe Experience Platform ユーザーインターフェイスを使用して同様のアクションを実行し、セグメントを操作する方法については、『[セグメントビルダーユーザーガイド](../ui/segment-builder.md)』を参照してください。
