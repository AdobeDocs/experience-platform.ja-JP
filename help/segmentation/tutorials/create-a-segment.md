---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: セグメントの作成
topic: tutorial
translation-type: tm+mt
source-git-commit: 6a0a9b020b0dc89a829c557bdf29b66508a10333
workflow-type: tm+mt
source-wordcount: '879'
ht-degree: 58%

---


# セグメントの作成

This document provides a tutorial for developing, testing, previewing, and saving a segment definition using the [DNL Adobe Experience Platform Segmentation Service API](../api/getting-started.md).

ユーザーインターフェイスを使用したセグメントの作成方法について詳しくは、[セグメントビルダーガイド](../ui/overview.md)を参照してください。

## はじめに

This tutorial requires a working understanding of the various [!DNL Adobe Experience Platform] services involved in creating audience segments. このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [!DNL Real-time Customer Profile](../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
- [!DNL Adobe Experience Platform Segmentation Service](../home.md): リアルタイム顧客プロファイルデータからオーディエンスセグメントを作成できます。
- [!DNL Experience Data Model (XDM)](../../xdm/home.md): 顧客体験データを [!DNL Platform] 整理するための標準化されたフレームワーク。

The following sections provide additional information that you will need to know in order to successfully make calls to the [!DNL Platform] APIs.

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

In order to make calls to [!DNL Platform] APIs, you must first complete the [authentication tutorial](../../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

- Authorization: Bearer `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

All resources in [!DNL Experience Platform] are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in:

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>For more information on sandboxes in [!DNL Platform], see the [sandbox overview documentation](../../sandboxes/home.md).

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

- Content-Type: application/json

## セグメント定義の作成

セグメント化の最初の手順は、セグメントを定義することです。セグメントは、**セグメント定義**&#x200B;と呼ばれる構成体で表されます。A segment definition is an object that encapsulates a query written in [!DNL Profile Query Language] (PQL). このオブジェクトは **PQL 述語**&#x200B;とも呼ばれます。PQL predicates define the rules for the segment based on conditions related to any record or time series data you supply to [!DNL Real-time Customer Profile]. PQL クエリの記述について詳しくは、[PQL ガイド](../pql/overview.md)を参照してください。

You can create a new segment definition by making a POST request to the `/segment/definitions` endpoint in the [!DNL Segmentation] API. 次の例では、セグメントを正しく定義するために必要な情報など、定義リクエストの形式について説明します。

セグメントの定義方法の詳細については、『 [セグメント定義開発者ガイド](../api/segment-definitions.md#create)』を参照してください。

## オーディエンスの推定とプレビュー {#estimate-and-preview-an-audience}

As you develop your segment definition, you can use the estimate and preview tools within [!DNL Real-time Customer Profile] to view summary-level information to help ensure you are isolating the expected audience. 推定を通じて、予想されるオーディエンスサイズや信頼区間など、セグメント定義の統計情報が得られます。プレビューは、セグメント定義に適格なプロファイルのページ分割リストを表示するので、結果を予想と比較できます。

オーディエンスの推定とプレビューにより、望ましい結果が得られるまで PQL 述語をテストし最適化することができます。最終的な PQL 述語は更新したセグメント定義で使用できます。

セグメントをプレビューまたは推定するために必要な手順は次の 2 つです。

1. [プレビュージョブの作成](#create-a-preview-job)
2. [推定またはプレビューの表示](#view-an-estimate-or-preview)（プレビュージョブの ID を使用）

### 推定の生成方法

データサンプルを使用してセグメントを評価し、適格なプロファイルの数を推定します。毎朝（UTC の午前 7～9 時）新しいデータがメモリに読み込まれ、その日のサンプルデータを使用して、すべてのセグメント化クエリが推定されます。その結果、新しく追加されたフィールドや収集された追加データは、翌日の推定に反映されます。

サンプルサイズは、プロファイルストア内のエンティティの総数によって異なります。これらのサンプルサイズを次の表に示します。

| プロファイルストア内のエンティティ数 | サンプルサイズ |
| ------------------------- | ----------- |
| 100 万未満 | フルデータセット |
| 100 万～2000 万 | 100 万 |
| 2000 万以上 | 全体の 5% |

推定は通常、10～15 秒間実行されます。最初は大まかな推定ですが、読み取るレコードが増えるにつれて精度が高くなります。

### プレビュージョブの作成

新しいプレビュージョブを作成するには、`/preview` エンドポイントに POST リクエストを送信します。

プレビュージョブの作成に関する詳細な手順については、『 [プレビューおよび見積もりエンドポイントガイド](../api/previews-and-estimates.md#create-preview)』を参照してください。

### 推定またはプレビューの表示

クエリが異なると完了するまでの時間が異なる可能性があるので、推定プロセスとプレビュープロセスは非同期で実行されます。クエリが開始されたら、API 呼び出しを使用して、推定またはプレビューの現在の状態を進行に応じて取得できます（GET リクエストを使用）。

APIを使用して、プレビュージョブの現在の状態をIDで調べることができ [!DNL Segmentation Service] ます。 状態が「RESULT_READY」の場合は、結果を表示できます。プレビュージョブの現在の状態を調べるには、プレビューおよび推定エンドポイントガイドのプレビュージョブの [取得に関する節を読み込んでください](../api/previews-and-estimates.md#get-preview) 。 見積もりジョブの現在の状態を調べるには、プレビューおよび見積もりエンドポイントガイドの見積もりジョブ [の](../api/previews-and-estimates.md#get-estimate) 取得に関する節をお読みください。


## 次の手順

Once you have developed, tested, and saved your segment definition, you can create a segment job to build an audience using the [!DNL Segmentation Service] API. その詳しい手順については、[セグメント結果の評価とアクセス](./evaluate-a-segment.md)に関するチュートリアルを参照してください。