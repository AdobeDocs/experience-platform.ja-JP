---
title: 実行と操作の概要
description: 実行ツールと運用ツールを使用して、Experience Platformの実装を調査、トラブルシューティング、最適化します。 スケジュールされたバッチアクティベーションを可視化し、設定の問題を特定して、システムの信頼性を向上させます。
solution: Experience Platform
type: Documentation
role: Admin, User
exl-id: 7f44cdf3-4db1-47f9-bcde-401f6dcfc551
source-git-commit: 41abc542b11dcd9c295d29cdfad68720ad50129d
workflow-type: tm+mt
source-wordcount: '636'
ht-degree: 2%

---

# 実行と操作の概要

バッチジョブが失敗したり、不完全なデータを配信したりした場合は、問題の原因をすばやく把握する必要があります。 根本原因は、データ可用性の問題、タイミングの誤り、設定の問題、システム容量の制約などが考えられます。 明確に可視化できなければ、回答を見つける前に、複数のシステムを調査するのに何時間もかかってしまう可能性があります。

[!UICONTROL Run and Operate] ツールを使用すると、次のことが可能になります。

* **データ操作を調べる**：すべてのワークフローでジョブ実行の状態と正常性を包括的に把握します。
* **トラブルシューティングを迅速化**：詳細な診断情報と実行履歴にアクセスして、根本原因をすばやく特定し、解決までの平均時間を短縮します。
* **問題をプロアクティブに防止**：ジョブ パターンを分析し、エラーが発生する前に構成の問題を検出し、データ操作を最適化します。

## ターゲットオーディエンス {#target-audiences}

[!UICONTROL Run and Operate]個のツールは、組織全体で複数のオーディエンスに対応するように設計されています。

* **データおよびIT チーム**：信頼性の高いデータパイプラインを維持し、技術的な問題をトラブルシューティングするシステム管理者およびデータエンジニア。
* **マーケティングオペレーション**: マーケティングプラットフォームへのデータ配信を調査し、アクティベーションの問題を解決するマーケティングテクノロジスト。
* **実装者**：実装の効率性と信頼性を検証し、技術的な問題をトラブルシューティングする実務担当者。

## 前提条件 {#prerequisites}

実行および操作ツールにアクセスするには、**[!UICONTROL View Job Schedules]**&#x200B;および&#x200B;**[!UICONTROL View Profile Management]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 適切な権限を持っていることを確認するには、システム管理者にお問い合わせください。

## はじめに {#getting-started}

Experience Platform UIから実行ツールと操作ツールにアクセスするには：

1. Experience Platform アカウントにログインし、左側のナビゲーションから「**[!UICONTROL Run and Operate]**」を選択します。
2. 検査またはトラブルシューティングのニーズに合ったツールを選択します。

![左ナビゲーションの実行と操作を示すExperience Platform UI。](assets/overview/run-and-operate.png)

## 利用可能なツール {#available-tools}

次のツールは、データ運用の検査と最適化に役立ちます。

### ジョブスケジュール {#job-schedules}

>[!IMPORTANT]
>
>[!UICONTROL Job schedules]は現在、次のReal-Time CDP ジョブでのみ使用できます。
>
> * バッチデータレイクの取得
> * プロファイルのバッチインジェスト
> * バッチセグメント化
> * バッチ宛先のアクティベーション

[&#x200B; ジョブスケジュール &#x200B;](job-schedules.md)を使用すると、サンドボックスごとに、データレイクの取り込み、プロファイルの取り込み、セグメント化、宛先アクティベーションなど、組織全体でスケジュールされているすべてのバッチ操作を調べることができます。 ジョブの実行ステータス、パフォーマンス指標、実行履歴を表示して、パターンを特定し、信頼性に影響を与える設定の問題を診断します。

![&#x200B; ジョブスケジュール画面を表示するExperience Platform UI。](assets/overview/job-schedules-interface.png)

ジョブスケジュールには、次の3つのレベルがあります。

* **[ジョブスケジュールの調査](job-schedules.md)**：すべてのデータセットとスケジュールされたジョブをタイムラインで表示して、パイプライン全体のパターンとスケジュールの競合を特定します。
* **[アンチパターンの特定](job-schedules-anti-patterns.md)**：スケジュールの重複、密なバッチスタック、パフォーマンスに影響を与える過度なバッチ処理など、一般的な設定の問題を特定して解決する方法について説明します。
* **[ジョブの詳細を表示](job-schedules-details.md)**：特定のデータセットと個々のジョブの実行をドリルダウンして、エラーを調査し、タイミングを確認し、処理されたレコードを検証します。

また、データ処理ステージ間の依存関係を把握できるため、Experience Platformワークフロー全体で信頼性の高いデータフローを確保できます。

### ヘルスチェック {#health-checks}

[&#x200B; ヘルスチェック &#x200B;](health-checks.md)を使用すると、スキーマとID設定の問題がビジネス運営に影響を与える前に、先見的に検出できます。 現在、ヘルスチェックでは、スキーマとID名前空間をまたいで毎日の静的スキャンを実行し、欠けているベストプラクティス、設定のミス、パターンを表面化して、下流での失敗につながっています。

ヘルスチェックでは、現在、次の5つの基本分野を評価しています。

* **[ID フィールドの検証](health-checks.md#identity-field-validation)**:ID フィールドの長さとパターンの制約が適切であることを確認します。
* **[ID グラフ リンク ルール](health-checks.md#identity-graph-linking-rules)**: プロファイルの折りたたみを防ぐためにリンク ルールが設定されていることを確認します。
* **[ユーザーとユーザー以外のID設定](health-checks.md#people-non-people-identity)**：スキーマクラス全体で正しいID タイプの使用状況を検証します。
* **[カスタム ID名前空間の説明](health-checks.md#namespace-missing-description)**：名前空間メタデータが完了していることを確認します。
* **[非推奨のID名前空間](health-checks.md#deprecated-namespace)**：クリーンアップ用に古い名前空間を検出します。

## 次の手順 {#next-steps}

[!UICONTROL Run and Operate] ツールの目的と機能について理解したところで、次のリソースを確認して、知識を深めてください。

* [&#x200B; ヘルスチェック &#x200B;](health-checks.md)を使用して、スキーマとID設定の問題を検出する方法を説明します
* バッチ取り込みとアクティベーション用にジョブスケジュール [を](job-schedules.md)検査する方法について説明します
* [&#x200B; バッチ取り込み](../ingestion/batch-ingestion/overview.md)について説明し、Experience Platformにデータを取り込む方法を理解します
* バッチ宛先に対して[&#x200B; スケジュールされたアクティベーションを設定する方法](../destinations/ui/activate-batch-profile-destinations.md)について説明します
* 宛先の[&#x200B; データフロー監視](../dataflows/ui/monitor-destinations.md)を探索
