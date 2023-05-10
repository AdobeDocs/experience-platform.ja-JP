---
title: 高速クエリの送信
description: 高速クエリ API の紹介です。
exl-id: c6cd1182-d3a9-457f-81d5-18027e47c3f9
source-git-commit: 05a7b73da610a30119b4719ae6b6d85f93cdc2ae
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 14%

---

# 高速クエリの送信

Data Distiller SKU の一部である [Query Service API](https://developer.adobe.com/experience-platform-apis/references/query-service/) では、高速ストアに対してステートレスにクエリを実行できます。この [高速クエリエンドポイント](https://developer.adobe.com/experience-platform-apis/references/query-service/#tag/Accelerated-Queries) は、集計されたデータに基づいて結果を返し、結果の待ち時間を短縮し、よりインタラクティブな情報交換を提供します。

詳しくは、 [Accelerated Queries エンドポイント](../../api/accelerated-queries.md) accelerated store に対するクエリ方法の説明については、ドキュメントを参照してください。

Accelerated Store を使用すると、カスタムデータモデルを作成したり、既存のAdobe Real-time Customer Data Platformデータモデルを拡張したりできます。 レポートインサイトをレポート/ビジュアライゼーションフレームワークに関与させる、または組み込むには、 [query accelerated store reporting insights ガイド](./reporting-insights-data-model.md). また、Real-time Customer Data Platform Insights データモデルのドキュメントを読んで、 [SQL クエリテンプレートをカスタマイズして、マーケティングおよび KPI（主要業績評価指標）の使用例に関するReal-Time CDPレポートを作成する](../../../dashboards/cdp-insights-data-model.md). 以下を使用して、 [属性ベースのアクセス制御機能](../../../access-control/abac/overview.md):accelerated ストア内のデータセットに対する制限のレベルを制御します。 詳しくは、 [クエリサービスでのデータガバナンス](../../data-governance/overview.md#create-field-based-access-restrictions-on-accelerated-datasets)
ドキュメントを参照してください。
