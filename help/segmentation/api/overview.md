---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
solution: Adobe Experience Platform
title: Adobe Experience Platformセグメントサービス開発ガイド
topic: guide
translation-type: tm+mt
source-git-commit: 995fadef9abacf22d0561e0590dfbe172adf0a43
workflow-type: tm+mt
source-wordcount: '467'
ht-degree: 6%

---


# Adobe Experience Platform [!DNL Segmentation Service] API開発ガイド

[!DNL Adobe Experience Platform Segmentation Service] セグメントを作成し、データ [!DNL Adobe Experience Platform] からでオーディエンスを生成でき [!DNL Real-time Customer Profile] ます。

この [!DNL Segmentation Service] APIは複数のエンドポイントを提供し、でのセグメント化操作をプログラムで管理できるようにし [!DNL Experience Platform]ます。 この概要ドキュメントでは、これらの各エンドポイントの概要を説明し、詳しくは、関連するエンドポイントガイドへのリンクを示します。 個々のエンドポイントのガイドを読む前に、 [はじめに](./getting-started.md) 、必要なヘッダーに関する重要な情報、サンプルAPI呼び出しの読み方などについてのガイドを参照してください。

使用可能なすべてのエンドポイントとCRUD操作を表示するには、 [Segmentation Service APIリファレンスを参照してください](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/segmentation.yaml)。

## ジョブの書き出し

ジョブの書き出し は、オーディエンスセグメントメンバーをデータセットに永続化するために使用される非同期プロセスです。 エンドポイントを使用して、すべての書き出しジョブの取得、新しい書き出しジョブの作成、特定の書き出しジョブの詳細の取得または特定の書き出しジョブのキャンセルを行うことができます。 `/export/jobs`

For more information on using this endpoint, please read the [export jobs endpoint guide](./export-jobs.md).

## プレビューと予測

プレビューには、セグメント定義の条件を満たすプロファイルのページ分割リストが用意されており、期待どおりの結果と比較できます。 エンドポイントを使用して、新しいプレビュージョブを作成したり、特定のプレビュージョブの結果を検索したりできます。 `/preview`

予測には、予測されるオーディエンスサイズ、信頼区間、誤差の標準偏差など、セグメント定義の統計情報が含まれます。 エンドポイントを使用して、セグメント定義の予測を表示でき `/estimate` ます。

これらのエンドポイントの使用に関する詳細は、『 [プレビューと予測エンドポイントガイド』を参照してください](./previews-and-estimates.md)。

## スケジュール

スケジュールは、バッチセグメントジョブを1日1回自動的に実行するために使用できるツールです。 エンドポイントを使用して、スケジュールのリストの取得、新しいスケジュールの作成、特定のスケジュールの詳細の取得、特定のスケジュールの更新または特定のスケジュールの削除を行うことができます。 `/config/schedules`

For more information on using this endpoint, please read the [schedules endpoint guide](./schedules.md).

## セグメントの定義

セグメント定義は、どのプロファイルがどのオーディエンスセグメントに含まれるかを定義します。 エンドポイントを使用して、セグメント定義を管理でき `/segment/definitions` ます。

For more information on using this endpoint, please read the [segment definitions endpoint guide](./segment-definitions.md).

## セグメントジョブ

セグメントジョブでは、以前に確立されたセグメント定義が処理され、オーディエンスセグメントが生成されます。エンドポイントを使用して、セグメントジョブを管理でき `/segment/jobs` ます。

For more information on using this endpoint, please read the [segment jobs endpoint guide](./segment-jobs.md).

## セグメント検索

セグメント検索は、様々なデータソースに含まれるフィールドを検索し、ほぼリアルタイムで返す場合に使用します。 セグメント検索の操作を開始するには、 [検索エンドポイントガイドを参照してください](segment-search.md)

## 次の手順

APIの使用を開始するには、様々なエンドポイントガイドを参照して、サービスの様々なエンドポイントを呼び出す方法に関する詳細な手順を確認してください。 [!DNL Segmentation Service] To learn more about working with segments using the [!DNL Platform] UI, see the [Segmentation user guide](../ui/overview.md).