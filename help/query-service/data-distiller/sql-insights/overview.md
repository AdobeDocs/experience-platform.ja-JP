---
title: SQL インサイト
description: Data Distillerを使用して SQL インサイトダッシュボードを作成するためのユースケース、基本的な機能、必要な手順について説明します。 Data Distillerの SQL Insights 機能により、プロファイル、オーディエンス、キャンペーン、ジャーニー、使用権限、同意など、様々なディメンションにわたって、透明性を高め、運用に関するインサイトを得る方法を説明します。
exl-id: f807d0fd-c8ec-42d4-96a0-5ffc5681943b
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '943'
ht-degree: 0%

---

# SQL インサイト

Data Distillerの SQL Insights を使用すると、より深いインサイトを引き出し、戦略を最適化し、分析を特定のビジネスニーズに合わせて適応させるための、カスタムのレポートデータモデルを作成できます。 SQL インサイト機能を使用すると、プロファイル、オーディエンス、キャンペーン、ジャーニー、使用権限、同意など、様々なディメンションにわたって、Adobe Experience Platform データから透明性を高め、運用に関するインサイトを得ることができます。 この機能は、特定のビジネスニーズに合わせて組織のレポートデータモデルをカスタマイズする、汎用性の高いアダプティブソリューションを提供します。

SQL Insights を [&#x200B; 視覚化 &#x200B;](../../../dashboards/sql-insights-query-pro-mode/overview.md) するには、[query pro モード &#x200B;](../../../dashboards/sql-insights-query-pro-mode/overview.md) を使用して、カスタム SQL クエリで複雑な分析を実行し、データを解釈しやすいグラフに変換します。 Query pro モードを使用して、ダッシュボードにカスタムのインサイトとビジュアライゼーションを作成し、インサイトを CSV ファイルとしてダウンロードすることで、技術オーディエンスと非技術オーディエンスの両方に対応します。

このドキュメントでは、Data Distillerを使用して SQL Insights ダッシュボードを作成するためのユースケース、基本的な機能および必要な手順について説明します。

## 前提条件

このチュートリアルでは、ユーザー定義のダッシュボードを使用して、Experience Platform UI 内のカスタムデータモデルからのデータを視覚化します。 この機能について詳しくは、[&#x200B; ユーザー定義ダッシュボードのドキュメント &#x200B;](../../../dashboards/standard-dashboards.md) を参照してください。

## はじめに

レポートインサイト用のカスタムデータモデルを作成したり、エンリッチメントされたExperience Platform データを保持するReal-Time CDP データモデルを拡張したりするには、Data Distiller SKU が必要です。 Data Distiller SKU に関連する [&#x200B; パッケージ &#x200B;](../../packaging.md)、[&#x200B; ガードレール &#x200B;](../../guardrails.md#query-accelerated-store) および [&#x200B; ライセンス &#x200B;](../../data-distiller/license-usage.md) ドキュメントを参照してください。 Data Distiller SKU をお持ちでない場合は、Adobe カスタマーサービス担当者に詳細をお問い合わせください。

## SQL Insights の使用例 {#use-cases}

Data Distillerの SQL Insights を使用して効果的に対処できる一般的なユースケースを以下に示します。

### プロファイルとオーディエンス使用の透明性 {#usage-transparency}

**課題：** 事業単位、ロイヤルティステータス、顧客のライフタイムバリュー（CLTV）などの特定の基準で主要業績評価指標（KPI）を分類する方法。

**SQL Insights ソリューション：** Data Distillerを使用すると、Adobe Experience Platformでレポートデータモデルを拡張できるので、[CLTV などのカスタムプロファイル属性の追加 &#x200B;](../../use-cases/customer-lifetime-value.md) やロイヤルティステータスの追加が容易になります。

### 同意異常値トラッキング {#consent-anomaly-tracking}

**課題：** メール、SMS、電話などのチャネルのカスタマイズされた同意属性に、オーディエンスの重複とサイズのトレンドラインレポートを適用する方法。

**SQL Insights ソリューション：** レポートデータモデルを拡張して、同意の環境設定の変化を経時的に追跡できます。 これには、同意の環境設定とスケジュール [&#x200B; 増分データ更新 &#x200B;](../../key-concepts/incremental-load.md) のトレンドを示す追加のファクトおよびディメンションテーブルの作成が含まれます。

### オーディエンスのセグメント化戦略の最適化 {#optimize-audience-segmentation-strategy}

**課題：** 機械学習（ML）モデルで生成された傾向スコアをオーディエンス KPI レポートに統合する方法。

**SQL Insights ソリューション：** Data Distillerでは、[&#x200B; カスタム ML モデルの傾向スコア &#x200B;](../../use-cases/propensity-score.md) を含めることができるので、オーディエンスレベルでの集計スコアの計算が容易になります。 その後、このデータを標準 KPI と共にレポートできます。

### オーディエンスの拡張 {#audience-expansion}

**課題：** オーディエンスの重複レポートでプロファイル数以外のデータを取得し、追加の人口統計データや環境設定を取得して、オーディエンス拡張戦略を導く方法。

**SQL Insights ソリューション：** レポートデータモデルを拡張すると、ユーザーは追加のプロファイル属性を取り込み、関連する人口統計データと環境設定でオーディエンス重複レポートを強化できます。

## SQL インサイトを生成するための主な機能 {#key-capabilities}

次の図は、SQL インサイトを生成するために不可欠ないくつかの機能を示しています。 次のような機能があります。

1. **データのビジュアライゼーション：** トレンドや棒グラフなどの視覚的要素を組み込んで、データのトレンドを包括的に表示します。
1. **ダッシュボードオーサリング：** 特定のユースケースに合わせてカスタムダッシュボードを作成できるようにすることで、よりパーソナライズされたターゲット設定の分析エクスペリエンスを提供します。
1. **柔軟な SQL データモデリング：** 汎用性の高い SQL データモデリングアプローチを使用します。ユーザーは様々なデータセットをシームレスに組み合わせて操作でき、適応性と分析深度を向上させることができます。
1. **高速ストア：** 高速ストアメカニズムを実装して、SQL で集計インサイトを効率的に提供し、貴重な情報に効率的かつ迅速にアクセスできるようにします。
1. **BI 接続性：** Power BI、Tableau、Looker、Apache Superset など、一般的なBusiness Intelligence（BI）ツールとのシームレスな統合を促進します。 この接続により、多様な BI 環境との互換性が確保され、ユーザーは詳細な分析とレポート作成に選択したツールを柔軟に使用できます。

![Data Distillerの SQL Insights を使用した主な機能の視覚的表現。](../../images/data-distiller/sql-insights/key-capabilities-of-customizable-insights.png)

## SQL Insights の作成手順 {#steps-to-create}

Data Distiller内で SQL Insights ダッシュボードを作成するには、以下のステップバイステップの手順に従います。

1. **アドホッククエリの調査：** まず、アドホッククエリを実行して、データレイク上の生データを調 `SELECT` ます。 これにより、その場で探索的なデータ分析を行い、クエリの結果がデータレイクに保存されないデータを検証できます。
1. **バッチクエリ稼働率：** バッチクエリを使用して [&#x200B; スケジュールされたジョブを作成 &#x200B;](../../api/scheduled-queries.md#create-a-new-scheduled-query) し、インサイトの集計テーブルを生成して、データ処理に対する体系的かつ自動アプローチを確保します。 バッチクエリは、`INSERT TABLE AS SELECT` および `CREATE TABLE AS SELECT` クエリを実行して、データの消去、シェイプ、操作、エンリッチメントを行います。 これらのクエリの結果は、データレイクに保存されます。
1. **集計インサイトの読み込み：** 生成された集計インサイトを高速ストアに読み込み、SQL を使用してクエリをテストし、データ取得の正確性と効率を確保します。 高速ストアに対してステートレスにクエリを実行する [&#x200B; 方法については &#x200B;](../../api/accelerated-queries.md) のドキュメントを参照してください。
1. **アクセスと統合：** Adobe Experience Platform[&#x200B; ユーザー定義ダッシュボード &#x200B;](../../../dashboards/standard-dashboards.md) またはその他の推奨されるBusiness Intelligence（BI）ツールと統合することで、高速化ストアに保存されたインサイトにシームレスにアクセスします。 これらのサードパーティクライアントとの統合により、ユーザーは直感的でまとまりのあるエクスペリエンスを容易に得ることができます。

![Data Distillerでの SQL インサイトへの 4 つの手順を示したインフォグラフィック &#x200B;](../../images/data-distiller/sql-insights/steps-to-customizable-insights.png)

## 次の手順

このドキュメントを参照することで、Data Distillerを使用した SQL インサイトダッシュボードを作成するためのユースケース、基本的な機能、必要な手順をより深く理解できます。 カスタムのレポートデータモデルの作成について詳しくは、[&#x200B; レポートインサイトデータモデルガイド &#x200B;](./reporting-insights-data-model.md) を参照してください。
