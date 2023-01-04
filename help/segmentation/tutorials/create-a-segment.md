---
keywords: Experience Platform；ホーム；人気の高いトピック；セグメント；セグメント；セグメントの作成；セグメントの作成；セグメントの作成；セグメント化サービス；
solution: Experience Platform
title: セグメント化サービス API を使用したセグメントの作成
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルでは、Adobe Experience Platform Segmentation Service API を使用してセグメント定義を開発、テスト、プレビュー、保存する方法について説明します。
exl-id: 78684ae0-3721-4736-99f1-a7d1660dc849
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '948'
ht-degree: 63%

---

# セグメント化サービス API を使用してセグメントを作成する

このドキュメントでは、 [[!DNL Adobe Experience Platform Segmentation Service API]](../api/getting-started.md).

ユーザーインターフェイスを使用したセグメントの作成方法について詳しくは、[セグメントビルダーガイド](../ui/overview.md)を参照してください。

## はじめに

このチュートリアルでは、 [!DNL Adobe Experience Platform] オーディエンスセグメントの作成に関係するサービス。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-Time Customer Profile]](../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
- [[!DNL Adobe Experience Platform Segmentation Service]](../home.md):リアルタイム顧客プロファイルデータからオーディエンスセグメントを作成できます。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：[!DNL Platform] が、カスタマーエクスペリエンスデータを整理する際に使用する、標準化されたフレームワーク。セグメント化を最適に利用するには、 [データモデリングのベストプラクティス](../../xdm/schema/best-practices.md).

以下の節では、 [!DNL Platform] API

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- Authorization： Bearer `{ACCESS_TOKEN}`
- x-api-key： `{API_KEY}`
- x-gw-ims-org-id： `{ORG_ID}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name：`{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、次のような追加ヘッダーが必要です。

- Content-Type: application/json

## セグメント定義の作成

セグメント化の最初の手順は、セグメントを定義することです。セグメントは、セグメント定義と呼ばれる構成体で表されます。セグメント定義は、 [!DNL Profile Query Language] (PQL) を参照してください。 このオブジェクトは PQL 述語とも呼ばれます。PQL 述語は、指定するレコードまたは時系列データに関連する条件に基づいて、セグメントのルールを定義します [!DNL Real-Time Customer Profile]. PQL クエリの記述について詳しくは、[PQL ガイド](../pql/overview.md)を参照してください。

新しいセグメント定義を作成するには、 `/segment/definitions` エンドポイント [!DNL Segmentation] API 次の例では、セグメントを正しく定義するために必要な情報など、定義リクエストの形式について説明します。

セグメントの定義方法について詳しくは、 [セグメント定義開発者ガイド](../api/segment-definitions.md#create).

## オーディエンスの推定とプレビュー {#estimate-and-preview-an-audience}

セグメント定義を作成する際に、内の推定ツールとプレビューツールを使用できます [!DNL Real-Time Customer Profile] を参照して、期待されるオーディエンスを確実に特定するのに役立つ概要レベルの情報を表示します。 推定を通じて、予想されるオーディエンスサイズや信頼区間など、セグメント定義の統計情報が得られます。プレビューは、セグメント定義に適格なプロファイルのページ分割リストを表示するので、結果を予想と比較できます。

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

プレビュージョブの作成に関する詳しい手順については、 [プレビューおよび予測エンドポイントガイド](../api/previews-and-estimates.md#create-preview).

### 推定またはプレビューの表示

クエリが異なると完了するまでの時間が異なる可能性があるので、推定プロセスとプレビュープロセスは非同期で実行されます。クエリが開始されたら、API 呼び出しを使用して、推定またはプレビューの現在の状態を進行に応じて取得できます（GET リクエストを使用）。

の使用 [!DNL Segmentation Service] API では、ID でプレビュージョブの現在の状態を検索できます。 状態が「RESULT_READY」の場合は、結果を表示できます。プレビュージョブの現在の状態を検索するには、 [プレビュージョブセクションの取得](../api/previews-and-estimates.md#get-preview) を参照してください。 見積ジョブの現在の状態を検索するには、 [見積ジョブの取得](../api/previews-and-estimates.md#get-estimate) を参照してください。


## 次の手順

セグメント定義を開発、テスト、保存したら、セグメントジョブを作成して、 [!DNL Segmentation Service] API その詳しい手順については、[セグメント結果の評価とアクセス](./evaluate-a-segment.md)に関するチュートリアルを参照してください。
