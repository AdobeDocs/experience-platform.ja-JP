---
title: 実行と操作の概要
description: 実行ツールと操作ツールを使用して、Experience Platform実装を調べ、トラブルシューティングし、最適化します。 スケジュールされたバッチのアクティベーションを可視化し、設定の問題を特定して、システムの信頼性を向上させます。
hide: true
exl-id: 7f44cdf3-4db1-47f9-bcde-401f6dcfc551
source-git-commit: a36f984e56f37e4769e54eab182a8c54e891e32f
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 1%

---

# 実行と操作の概要

>[!AVAILABILITY]
>
>実行および操作機能は、現在、限定リリースとして使用できます。

バッチジョブが失敗したり不完全なデータが配信されたりしたら、問題の原因をすばやく把握する必要があります。 データの可用性の問題、タイミングの誤り、設定の問題、システム容量の制約などが根本原因である可能性があります。 明確な可視性がないと、答えを見つける前に複数のシステムを調査するのに何時間も費やすことがあります。

[!UICONTROL Run and Operate] のツールを使用すると、次のことができます。

* **データ操作の検査**：すべてのワークフローにわたるジョブ実行ステータスとヘルスの完全な表示を取得します。
* **迅速なトラブルシューティング**：詳細な診断情報と実行履歴にアクセスして、根本原因をすばやく特定し、解決までの平均時間を短縮します。
* **問題を事前に防ぐ**：ジョブパターンを分析し、エラーを引き起こす前に設定の問題を検出し、データ操作を最適化します。

## ターゲットオーディエンス {#target-audiences}

[!UICONTROL Run and Operate] のツールは、組織全体で複数のオーディエンスにサービスを提供するように設計されています。

* **データおよび IT チーム**：信頼性の高いデータパイプラインを維持し、技術的な問題をトラブルシューティングするシステム管理者およびデータエンジニア。
* **マーケティング業務**：マーケティングプラットフォームへのデータ配信を調査し、アクティベーションの問題を解決するマーケティング技術者。
* **実装者**：実装の効率や信頼性を検証し、技術的な問題をトラブルシューティングする実務担当者。

## 前提条件 {#prerequisites}

実行ツールと操作ツールにアクセスするには、**[!UICONTROL View Job Schedules]** と **[!UICONTROL View Profile Management]** [ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。
[!UICONTROL Job Schedules] のページには、スケジュールされたすべてのバッチ処理ジョブの概要が表示されます。
システム管理者に問い合わせて、適切な権限があることを確認します。

## はじめに {#getting-started}

Experience Platform UI から実行ツールおよび操作ツールにアクセスするには：

1. Experience Platform アカウントにログインし、左側のナビゲーションから「**[!UICONTROL Run and Operate]**」を選択します。
2. 検査やトラブルシューティングのニーズに合ったツールを選択します。

   >[!NOTE]
   >
   >現在、使用可能な機能は、[ ジョブスケジュール ](job-schedules.md) および [ ヘルスチェック ](health-checks.md) です。

![ 左ナビゲーションの実行と操作を示すExperience Platform UI。](assets/overview/run-and-operate.png)

## 使用可能なツール {#available-tools}

次のツールは、データ操作を検査し最適化するのに役立ちます。

### ジョブスケジュール {#job-schedules}

>[!IMPORTANT]
>
>[!UICONTROL Job schedules] は現在、次のReal-Time CDP ジョブでのみ使用できます。
>
> * バッチデータレイクの取り込み
> * バッチプロファイル取り込み
> * バッチ送信
> * バッチ宛先のアクティベーション。

[ ジョブスケジュール ](job-schedules.md) を使用すると、データレイクの取り込み、プロファイルの取り込み、セグメント化、宛先のアクティベーションなど、サンドボックスごとに、組織全体でスケジュールされたすべてのバッチ操作を検査できます。 ジョブの実行ステータス、パフォーマンス指標および実行履歴を表示して、パターンを特定し、信頼性に影響を与える設定の問題を診断します。

![ ジョブスケジュール画面を示すExperience Platform UI。](assets/overview/job-schedules-interface.png)

ジョブ・スケジュールには、次の 3 つのレベルの調査が用意されています。

* **[ジョブスケジュールの検査](job-schedules.md)**：すべてのデータセットとそのスケジュールされたジョブをタイムラインに表示して、パイプライン全体でのパターンとスケジュールの競合を特定します。
* **[アンチパターンの特定](job-schedules-anti-patterns.md)**：スケジュールの重複、複雑なバッチスタッキング、パフォーマンスに影響を与える過度のバッチ処理など、一般的な設定の問題を特定し、解決する方法を説明します。
* **[ジョブの詳細の表示](job-schedules-details.md)**：特定のデータセットと個々のジョブ実行にドリルダウンして、失敗の調査、タイミングの確認、処理されたレコードの検証を行います。

また、データ処理ステージ間の依存関係を理解し、Experience Platform ワークフロー全体で信頼性の高いデータフローを確保するのに役立ちます。

### ヘルスチェック {#health-checks}

>[!IMPORTANT]
>
>[!UICONTROL Health checks] は現在、限定リリースとして利用できます。

[ ヘルスチェック ](health-checks.md) を使用すると、ビジネス運用に影響を与える前に、スキーマと ID の設定の問題をプロアクティブに検出できます。 現在、ヘルスチェックは、スキーマと ID 名前空間にわたって毎日の静的スキャンを実行し、ベストプラクティスの欠落、設定の誤り、ダウンストリームのエラーにつながるパターンを明らかにします。

ヘルスチェックでは、現在、次の 5 つの基本的な領域を評価しています。

* **[ID フィールドの検証](health-checks.md#identity-field-validation)**:ID フィールドに適切な長さとパターンの制約があることを確認します。
* **[ID グラフリンクルール](health-checks.md#identity-graph-linking-rules)**：プロファイルが折りたたまれないようにリンクルールが設定されていることを確認します。
* **[人物および人物以外の ID 設定](health-checks.md#people-non-people-identity)**：スキーマクラスをまたいで正しい ID タイプ使用を検証します。
* **[カスタム ID 名前空間の説明](health-checks.md#namespace-missing-description)**：名前空間メタデータが完了していることを確認します。
* **[非推奨の ID 名前空間](health-checks.md#deprecated-namespace)**：クリーンアップ用に古い名前空間を検出します。

## 次の手順 {#next-steps}

[!UICONTROL Run and Operate] ツールの目的と機能を理解したら、次のリソースを参照して知識を深めます。

* [ ヘルスチェック ](health-checks.md) を使用して、スキーマと ID の設定の問題を検出する方法を説明します
* バッチの取り込みとアクティベーションのために [ ジョブスケジュールを検査 ](job-schedules.md) する方法を説明します
* データがExperience Platformにどのように取り込まれるかを理解するための [ バッチ取り込み ](../ingestion/batch-ingestion/overview.md) について説明します
* バッチ宛先の [ スケジュールされたアクティベーション ](../destinations/ui/activate-batch-profile-destinations.md) 設定方法を理解します
* 宛先の [ データフロー監視 ](../dataflows/ui/monitor-destinations.md) を探索
