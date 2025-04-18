---
solution: Experience Platform
title: Segmentation Service API を使用したセグメント定義の作成
type: Tutorial
description: このチュートリアルでは、Adobe Experience Platform Segmentation Service API を使用してセグメント定義を作成、テスト、プレビューおよび保存する方法について説明します。
exl-id: 78684ae0-3721-4736-99f1-a7d1660dc849
source-git-commit: f6d700087241fb3a467934ae8e64d04f5c1d98fa
workflow-type: tm+mt
source-wordcount: '1067'
ht-degree: 44%

---

# Segmentation Service API を使用したセグメント定義の作成

このドキュメントでは、[[!DNL Adobe Experience Platform Segmentation Service API]](../api/getting-started.md) を使用したセグメント定義の開発、テスト、プレビューおよび保存に関するチュートリアルを提供します。

ユーザーインターフェイスを使用してセグメント定義を作成する方法については、『 [ セグメントビルダーガイド ](../ui/segment-builder.md) を参照してください。

## はじめに

このチュートリアルでは、セグメント定義の作成に関連する様々な [!DNL Adobe Experience Platform] サービスについて、実際に理解している必要があります。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-Time Customer Profile]](../../profile/home.md)：複数のソースから集計したデータに基づいて、統合されたリアルタイム顧客プロファイルを提供します。
- [[!DNL Adobe Experience Platform Segmentation Service]](../home.md): セグメント定義またはリアルタイム顧客プロファイルデータのその他の外部ソースを使用して、オーディエンスを作成できます。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：[!DNL Experience Platform] が、カスタマーエクスペリエンスデータを整理する際に使用する、標準化されたフレームワーク。セグメント化を最大限に活用するには、[データモデリングのベストプラクティス](../../xdm/schema/best-practices.md)に従って、データがプロファイルとイベントとして取り込まれていることを確認してください。

次の節では、[!DNL Experience Platform] API を正しく呼び出すために知っておく必要がある追加情報を示します。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Experience Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- Authorization： Bearer `{ACCESS_TOKEN}`
- x-api-key： `{API_KEY}`
- x-gw-ims-org-id： `{ORG_ID}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Experience Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name：`{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Experience Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、次のような追加ヘッダーが必要です。

- Content-Type: application/json

## セグメント定義の作成

セグメント化の最初の手順は、セグメント定義を定義することです。 セグメント定義は、[!DNL Profile Query Language] （PQL）で記述されたクエリをカプセル化するオブジェクトです。 このオブジェクトは、PQL述語とも呼ばれます。 PQLの述語は、[!DNL Real-Time Customer Profile] に提供するレコードまたは時系列データに関連する条件に基づいて、セグメント定義のルールを定義します。 PQL クエリの記述について詳しくは、[PQL ガイド](../pql/overview.md)を参照してください。

[!DNL Segmentation] API の `/segment/definitions` エンドポイントに POST リクエストをおこなうことで、新しいセグメント定義を作成できます。 次の例では、セグメント定義を正常に定義するために必要な情報など、定義リクエストのフォーマット方法の概要を説明します。

セグメント定義の定義方法について詳しくは、『 [ セグメント定義開発者ガイド ](../api/segment-definitions.md#create) 』を参照してください。

## オーディエンスの推定とプレビュー {#estimate-and-preview-an-audience}

セグメント定義を作成する際には、[!DNL Real-Time Customer Profile] 内の推定ツールとプレビューツールを使用して概要レベルの情報を表示し、期待されるオーディエンスを確実に分離するのに役立ちます。 推定を通じて、予想されるオーディエンスサイズや信頼区間など、セグメント定義の統計情報が得られます。プレビューは、セグメント定義に適格なプロファイルのページ分割リストを表示するので、結果を予想と比較できます。

オーディエンスの推定とプレビューにより、望ましい結果が得られるまで PQL 述語をテストし最適化することができます。最終的な PQL 述語は更新したセグメント定義で使用できます。

セグメント定義をプレビューまたは推定するには、次の 2 つの必須ステップがあります。

1. [プレビュージョブの作成](#create-a-preview-job)
2. [推定またはプレビューの表示](#view-an-estimate-or-preview)（プレビュージョブの ID を使用）

### 推定の生成方法

リアルタイム顧客プロファイルに対して有効にされたデータはExperience Platformに取り込まれるので、プロファイルデータストア内に保存されます。 プロファイルストアへのレコードの取り込みが、合計プロファイル数を 5% 以上増加または減少させると、サンプリングジョブがトリガーされて数が更新されます。 プロファイル数が 5% を超えて変化しない場合、サンプリングジョブは毎週自動的に実行されます。

サンプルをトリガーする方法は、使用する取り込みのタイプによって異なります。

- ストリーミングデータワークフローの場合は、5% の増加または減少のしきい値に達したかどうかを判断するために、1 時間ごとにチェックが行われます。 このしきい値に達すると、サンプルジョブが自動的にトリガーされ、カウントが更新されます。
- バッチ取り込みの場合、プロファイルストアにバッチを正常に取り込んでから 15 分以内に、5% の増減しきい値に達した場合は、ジョブを実行してカウントを更新します。 プロファイル API を使用すると、最新の成功したサンプルジョブをプレビューできるほか、データセット別および ID 名前空間別のプロファイル配布をリストできます。

サンプルサイズは、プロファイルストア内のエンティティの合計数によって異なります。 これらのサンプルサイズを次の表に示します。

| プロファイルストアのエンティティ | サンプルサイズ |
| ------------------------- | ----------- |
| 100 万未満 | フルデータセット |
| 100 万～2000 万 | 100 万 |
| 2000 万以上 | 全体の 5% |

推定は通常、10～15 秒間実行されます。最初は大まかな推定ですが、読み取るレコードが増えるにつれて精度が高くなります。

### プレビュージョブの作成

新しいプレビュージョブを作成するには、`/preview` エンドポイントに POST リクエストを送信します。

プレビュージョブの作成に関する詳細な手順については、[ プレビューと予測エンドポイントガイド ](../api/previews-and-estimates.md#create-preview) を参照してください。

### 推定またはプレビューの表示

クエリが異なると完了するまでの時間が異なる可能性があるので、推定プロセスとプレビュープロセスは非同期で実行されます。クエリが開始されたら、API 呼び出しを使用して、推定またはプレビューの現在の状態を進行に応じて取得できます（GET リクエストを使用）。

[!DNL Segmentation Service] API を使用すると、プレビュージョブの現在の状態を ID で検索できます。 状態が「RESULT_READY」の場合は、結果を表示できます。プレビュージョブの現在の状態を検索するには、プレビューと予測エンドポイントガイドの [ プレビュージョブの取得 ](../api/previews-and-estimates.md#get-preview) セクションを参照してください。 推定ジョブの現在の状態を調べるには、プレビューと推定エンドポイントガイドの [ 推定ジョブの取得 ](../api/previews-and-estimates.md#get-estimate) の節を参照してください。


## 次の手順

セグメント定義を開発、テスト、保存したら、[!DNL Segmentation Service] API を使用してオーディエンスを作成するセグメントジョブを作成できます。 その詳しい手順については、[セグメント結果の評価とアクセス](./evaluate-a-segment.md)に関するチュートリアルを参照してください。
