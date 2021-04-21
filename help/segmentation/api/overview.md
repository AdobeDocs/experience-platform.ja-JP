---
keywords: Experience Platform；ホーム；人気のあるトピック；セグメント化；セグメント化；セグメント化サービス；API;API;
title: Segmentation Service APIガイド
topic-legacy: guide
description: Segmentation Service APIを使用すると、開発者はAdobe Experience Platformでのセグメント化操作をプログラムで管理できます。 このガイドに従って、APIを使用した主な操作の実行方法を学習します。
exl-id: cebecaf3-9746-4b0b-9c50-11789fba66c3
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '501'
ht-degree: 6%

---

# Segmentation Service APIガイド

[!DNL Adobe Experience Platform Segmentation Service] セグメントを作成し、 [!DNL Adobe Experience Platform] データ [!DNL Real-time Customer Profile] からでオーディエンスを生成できます。

[!DNL Segmentation Service] APIは複数のエンドポイントを提供し、[!DNL Experience Platform]でのセグメント化操作をプログラム的に管理できるようにします。 この概要ドキュメントでは、これらの各エンドポイントの概要を説明し、詳しくは、関連するエンドポイントガイドへのリンクを示します。 個々のエンドポイントガイドを読む前に、必要なヘッダー、サンプルAPI呼び出しの読み取りなどに関する重要な情報については、[はじめに](./getting-started.md)を参照してください。

使用可能なすべてのエンドポイントとCRUD操作を表示するには、[Segmentation Service APIリファレンス](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/segmentation.yaml)を参照してください。

## ジョブの書き出し

ジョブの書き出し は、オーディエンスセグメントメンバーをデータセットに永続化するために使用される非同期プロセスです。 `/export/jobs`エンドポイントを使用して、すべての書き出しジョブの取得、新しい書き出しジョブの作成、特定の書き出しジョブの詳細の取得、または特定の書き出しジョブのキャンセルを行うことができます。

このエンドポイントの使用に関する詳細は、[書き出しジョブのエンドポイントガイド](./export-jobs.md)を参照してください。

## プレビューと予測

プレビューには、セグメント定義の条件を満たすプロファイルのページ分割リストが用意されており、期待どおりの結果と比較できます。 `/preview`エンドポイントを使用して、新しいプレビュージョブを作成したり、特定のプレビュージョブの結果を検索したりできます。

予測には、予測されるオーディエンスサイズ、信頼区間、誤差の標準偏差など、セグメント定義の統計情報が含まれます。 `/estimate`エンドポイントを使用して、セグメント定義の予測を表示できます。

これらのエンドポイントの使用に関する詳細は、[プレビューおよび推定エンドポイントガイド](./previews-and-estimates.md)を読んでください。

## スケジュール

スケジュールは、バッチセグメントジョブを1日1回自動的に実行するために使用できるツールです。 `/config/schedules`エンドポイントを使用して、スケジュールのリストの取得、新しいスケジュールの作成、特定のスケジュールの詳細の取得、特定のスケジュールの更新または特定のスケジュールの削除を行うことができます。

このエンドポイントの使用に関する詳細は、[スケジュールエンドポイントガイド](./schedules.md)を参照してください。

## セグメントの定義

セグメント定義は、どのプロファイルがどのオーディエンスセグメントに含まれるかを定義します。 `/segment/definitions`エンドポイントを使用して、セグメント定義を管理できます。

このエンドポイントの使用に関する詳細は、『[セグメント定義エンドポイントガイド](./segment-definitions.md)』を参照してください。

## セグメントジョブ

セグメントジョブでは、以前に確立されたセグメント定義が処理され、オーディエンスセグメントが生成されます。`/segment/jobs`エンドポイントを使用して、セグメントジョブを管理できます。

このエンドポイントの使用に関する詳細は、[セグメントジョブエンドポイントガイド](./segment-jobs.md)を参照してください。

## セグメント検索

セグメント検索は、様々なデータソースに含まれるフィールドを検索し、ほぼリアルタイムで返す場合に使用します。 セグメント検索の使用を開始するには、[検索エンドポイントガイド](segment-search.md)を参照してください

## 次の手順

[!DNL Segmentation Service] APIの使用を開始するには、様々なエンドポイントガイドを参照して、サービスの様々なエンドポイントを呼び出す方法に関する詳細な手順を確認してください。 [!DNL Platform] UIを使用したセグメントの操作について詳しくは、[セグメント化ユーザーガイド](../ui/overview.md)を参照してください。
