---
keywords: Experience Platform；ホーム；人気の高いトピック；セグメント化；セグメント化；セグメント化サービス；API;API;
title: セグメント化サービス API ガイド
topic-legacy: guide
description: Segmentation Service API を使用すると、開発者はAdobe Experience Platformでセグメント化操作をプログラムで管理できます。 このガイドに従って、API を使用した主な操作の実行方法を学習します。
exl-id: cebecaf3-9746-4b0b-9c50-11789fba66c3
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '497'
ht-degree: 7%

---

# セグメント化サービス API ガイド

[!DNL Adobe Experience Platform Segmentation Service] では、セグメントを作成し、 [!DNL Adobe Experience Platform] から [!DNL Real-Time Customer Profile] データ。

この [!DNL Segmentation Service] API は、でのセグメント化操作をプログラムで管理できる複数のエンドポイントを提供します。 [!DNL Experience Platform]. この概要ドキュメントでは、これらの各エンドポイントの概要を説明し、詳しくは、関連するエンドポイントガイドへのリンクを示します。 個々のエンドポイントガイドを読む前に、 [入門ガイド](./getting-started.md) 必要なヘッダー、サンプル API 呼び出しなどに関する重要な情報については、を参照してください。

使用可能なすべてのエンドポイントと CRUD 操作を表示するには、 [セグメント化サービス API リファレンス](https://www.adobe.io/experience-platform-apis/references/segmentation/).

<!-- ## Audiences

Audiences are a collection of people who share similar behaviors and/or characteristics. These can be generated either by using Platform or from external sources. You can use the `/audiences` endpoint to retrieve all audiences, create a new audience, retrieve details of a specific audience, update a specific audience, or delete a specific audience.

For more information on using this endpoint, please read the [audiences endpoint guide](./audiences.md). -->

## ジョブの書き出し

ジョブの書き出し は、オーディエンスセグメントメンバーをデータセットに永続化するために使用される非同期プロセスです。 以下を使用して、 `/export/jobs` エンドポイント：すべての書き出しジョブを取得、新しい書き出しジョブを作成、特定の書き出しジョブの詳細を取得、または特定の書き出しジョブをキャンセルします。

このエンドポイントの使用方法の詳細については、 [書き出しジョブエンドポイントガイド](./export-jobs.md).

## プレビューと見積もり

プレビューは、セグメント定義に適格なプロファイルのページ分割リストを提供し、結果を期待値と比較できます。 以下を使用して、 `/preview` エンドポイント：新しいプレビュージョブを作成するか、特定のプレビュージョブの結果を検索します。

推定は、推定オーディエンスサイズ、信頼区間、エラーの標準偏差など、セグメント定義の統計情報を提供します。 以下を使用して、 `/estimate` エンドポイント：セグメント定義の推定値を表示します。

これらのエンドポイントの使用に関する詳細は、 [プレビューおよび予測エンドポイントガイド](./previews-and-estimates.md).

## スケジュール

スケジュールは、1 日 1 回バッチセグメント化ジョブを自動的に実行するために使用できるツールです。 以下を使用して、 `/config/schedules` エンドポイント：スケジュールのリストの取得、新しいスケジュールの作成、特定のスケジュールの詳細の取得、特定のスケジュールの更新または特定のスケジュールの削除をおこないます。

このエンドポイントの使用方法の詳細については、 [スケジュールエンドポイントガイド](./schedules.md).

## セグメントの定義

セグメント定義は、どのプロファイルがどのオーディエンスセグメントに含まれるかを定義します。 以下を使用して、 `/segment/definitions` エンドポイント：セグメント定義を管理します。

このエンドポイントの使用方法の詳細については、 [セグメント定義エンドポイントガイド](./segment-definitions.md).

## セグメントジョブ

セグメントジョブでは、以前に確立されたセグメント定義が処理され、オーディエンスセグメントが生成されます。以下を使用して、 `/segment/jobs` エンドポイント：セグメントジョブを管理します。

このエンドポイントの使用方法の詳細については、 [セグメントジョブエンドポイントガイド](./segment-jobs.md).

## セグメント検索

セグメント検索は、様々なデータソースに含まれるフィールドを検索し、ほぼリアルタイムで返すために使用します。 セグメント検索の使用を開始するには、 [検索エンドポイントガイド](segment-search.md)

## 次の手順

を使い始めるには、以下を実行します。 [!DNL Segmentation Service] API で、様々なエンドポイントガイドを参照して、サービスの様々なエンドポイントを呼び出す方法に関する詳細な手順を確認してください。 を使用してセグメントを操作する方法の詳細を確認するには [!DNL Platform] UI( [セグメント化ユーザーガイド](../ui/overview.md).
