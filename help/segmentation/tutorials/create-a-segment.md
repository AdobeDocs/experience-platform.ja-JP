---
keywords: エクスペリエンス Platform、home、人気のある話題; segment;セグメント、セグメントの作成、セグメンテーション、segment。セグメンテーションサービス。
solution: Experience Platform
title: セグメンテーションサービス API を使用したセグメントの作成
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルでは、Adobe エクスペリエンスプラットフォームセグメンテーションサービス API を使用してセグメント定義を開発、テスト、プレビューおよび保存する方法について説明します。
exl-id: 78684ae0-3721-4736-99f1-a7d1660dc849
source-git-commit: 8325ae6fd7d0013979e80d56eccd05b6ed6f5108
workflow-type: tm+mt
source-wordcount: '948'
ht-degree: 63%

---

# セグメンテーションサービス API を使用したセグメントの作成

このドキュメントでは、を使用してセグメント定義の開発、テスト、プレビューおよび保存を行う方法についてのチュートリアルを提供して [[!DNL Adobe Experience Platform Segmentation Service API]](../api/getting-started.md) います。

ユーザーインターフェイスを使用したセグメントの作成方法について詳しくは、[セグメントビルダーガイド](../ui/overview.md)を参照してください。

## はじめに

このチュートリアルでは、対象となるセグメントを作成するための様々なサービスについて、十分に理解しておく必要があり [!DNL Adobe Experience Platform] ます。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-time Customer Profile]](../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
- [[!DNL Adobe Experience Platform Segmentation Service]](../home.md): リアルタイムのカスタマープロファイルデータから参加者を作成することができます。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：顧客体験データを編成する際に [!DNL Platform] に使用される標準化されたフレームワーク。セグメンテーションを最大限に活用するには、データモデリングのベストプラクティスに従い、データをプロファイルとイベントとして ingested にしておく必要が [ ](../../xdm/schema/best-practices.md) あります。

以下の各セクションでは、api を正常に呼び出すために必要な追加情報を示し [!DNL Platform] ます。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- Authorization： Bearer `{ACCESS_TOKEN}`
- x-api-key： `{API_KEY}`
- x-gw-ims-org-id： `{IMS_ORG}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name： `{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

- Content-Type: application/json

## セグメント定義の作成

セグメント化の最初の手順は、セグメントを定義することです。セグメントは、セグメント定義と呼ばれる構成体で表されます。セグメント定義は、(pql) に記述されているクエリーをカプセル化するオブジェクトです [!DNL Profile Query Language] 。 このオブジェクトは PQL 述語とも呼ばれます。PQL 述語は、指定したすべてのレコードまたはタイムシリーズのデータに関連する条件に基づいて、セグメントのルールを定義 [!DNL Real-time Customer Profile] します。 PQL クエリの記述について詳しくは、[PQL ガイド](../pql/overview.md)を参照してください。

API のエンドポイントに POST 要求を行うことにより、新しいセグメント定義を作成することができ `/segment/definitions` [!DNL Segmentation] ます。 次の例では、セグメントを正しく定義するために必要な情報など、定義リクエストの形式について説明します。

セグメントの定義方法について詳しくは、『 [ segment definition developer guide 』を参照してください ](../api/segment-definitions.md#create) 。

## オーディエンスの推定とプレビュー {#estimate-and-preview-an-audience}

セグメントの定義を作成する際に、内の見積とプレビューツールを使用して [!DNL Real-time Customer Profile] 要約レベルの情報を表示し、予想される視聴ユーザーを絞り込むことができます。 推定を通じて、予想されるオーディエンスサイズや信頼区間など、セグメント定義の統計情報が得られます。プレビューは、セグメント定義に適格なプロファイルのページ分割リストを表示するので、結果を予想と比較できます。

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

プレビュージョブを作成する手順の詳細については、プレビューおよび見積もりの終了ガイドを参照して [ ](../api/previews-and-estimates.md#create-preview) ください。

### 推定またはプレビューの表示

クエリが異なると完了するまでの時間が異なる可能性があるので、推定プロセスとプレビュープロセスは非同期で実行されます。クエリが開始されたら、API 呼び出しを使用して、推定またはプレビューの現在の状態を進行に応じて取得できます（GET リクエストを使用）。

この API を使用して [!DNL Segmentation Service] 、プレビュージョブの現在の状態を ID によって検索できます。 状態が「RESULT_READY」の場合は、結果を表示できます。プレビュージョブの現在の状態を確認するには、プレビュー [ および見積もりの設定ガイドの「プレビュージョブ」セクションを参照してください ](../api/previews-and-estimates.md#get-preview) 。 見積ジョブの現在の状態を確認するには、 [ ](../api/previews-and-estimates.md#get-estimate) プレビューおよび見積もりのエンドポイントガイドの見積ジョブの取得に関するセクションを参照してください。


## 次の手順

セグメントの定義を開発してテストし、保存した後で、この API を使用して対象ユーザーを構築するためのセグメントジョブを作成でき [!DNL Segmentation Service] ます。 その詳しい手順については、[セグメント結果の評価とアクセス](./evaluate-a-segment.md)に関するチュートリアルを参照してください。
