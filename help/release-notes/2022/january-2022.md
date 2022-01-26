---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の最新のリリースノートです。
source-git-commit: 9cd9307d54d0950d4f67d5d8cee9c6412a558275
workflow-type: tm+mt
source-wordcount: '708'
ht-degree: 27%

---

# Adobe Experience Platform リリースノート

**リリース日：2021 年 1 月 26 日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [アラート {#alerts}](#alerts-alerts)
- [[!DNL Data Prep] {#data-prep}](#dnl-data-prep-data-prep)
- [[!DNL Dashboards] {#dashboards}](#dnl-dashboards-dashboards)
- [クエリサービス {#query-service}](#query-service-query-service)
- [サンドボックス {#sandboxes}](#sandboxes-sandboxes)
- [セグメント化サービス {#segmentation}](#segmentation-service-segmentation)

## アラート {#alerts}

Experience Platformを使用すると、様々な Platform アクティビティに関するイベントベースのアラートを購読できます。 を使用して、様々なアラートルールを購読できます [!UICONTROL アラート] 」タブをクリックし、UI 内または電子メール通知を通じてアラートメッセージを受け取るように選択できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 新しいアラートルール | データの取り込み、ID、プロファイル、セグメント化、アクティブ化に関連するワークフローで、いくつかの新しいアラートルールを使用できるようになりました。 概要については、 [アラートルール](../../observability/alerts/rules.md) を参照してください。 |

Platform のアラートについて詳しくは、 [アラートの概要](../../observability/alerts/overview.md).

## [!DNL Data Prep] {#data-prep}

[!DNL Data Prep] を使用すると、データエンジニアは Experience Data Model（XDM）との間でデータをマッピング、変換および検証できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 統合マッピングエクスペリエンス | Platform UI の新しいマッピングインターフェイスでは、一貫したマッピング操作を使用して、インテリジェントマッピングレコメンデーションを活用し、マッピングルールを手動で設定し、マッピングセットに発生したエラーをデバッグできます。 詳しくは、 [[!DNL Data Prep] UI ガイド](../../data-prep/home.md). |

詳しくは、 [!DNL Data Prep]詳しくは、 [[!DNL Data Prep] 概要](../../data-prep/home.md).

## [!DNL Dashboards] {#dashboards}

[!DNL Dashboards] きれいな事をする。

| 機能 | 説明 |
|---------|-------------|
| インテリジェントキャプション | 機械学習アルゴリズムは、プロファイルとオーディエンスデータに関するインサイトを自動的に提供し、30 ～ 90 日（12 か月）のパターンとトレンドを示します。 キャプションには、 <ul><li>全体的な形状と統計</li><li>トレンドと急激な変化</li><li>季節柄</li><li>予期しない異常値</li></ul> 詳しくは、 [プロファイルダッシュボード](../../dashboards/guides/profiles.md#profiles-count-trend) および [セグメントダッシュボード](../../dashboards/guides/segments.md#audience-size-trend) ドキュメント。 |
| ダッシュボードの在庫 | PowerBI などのインストール済み統合を含む、プロファイル、セグメント、宛先ダッシュボードの事前設定済みレポートに、一元的にアクセスします。 詳しくは、 [[!DNL Dashboards] 概要](../../dashboards/home.md). |
| PowerBI レポートテンプレート | 新しい PowerBI グラフを使用して、プロファイル、セグメントおよび宛先レポートのデータモデルから指標を作成、カスタマイズ、拡張します。 自動インストールワークフローを使用すると、PowerBI 環境内から組織全体でマーケティングインサイトを共有できます。 詳しくは、 [[!DNL Dashboards] 概要](../../dashboards/home.md). |

詳しくは、 [!DNL Dashboards]詳しくは、 [[!DNL Dashboards] 概要](../../dashboards/home.md).

## クエリサービス {#query-service}

[!DNL Query Service] では、標準 SQL を使用してAdobe Experience Platformでデータに対してクエリを実行できます [!DNL Data Lake]. 任意のデータセットを [!DNL Data Lake] クエリの結果を新しいデータセットとして取り込み、レポートや Data Science Workspace で使用したり、リアルタイム顧客プロファイルに取り込んだりできます。

| 機能 | 説明 |
|----------------------|-----------------------|
| 匿名ブロック | 匿名ブロック SQL 構成を使用すると、クエリサービス内の大規模なデータ準備ジョブを小さなタスクに分類し、それらを再利用して順に実行し、増分データの読み込みをおこなうことができます。 詳しくは、 [クエリサービスの概要](../../query-service/home.md). |
| データセットの構成 | サンドボックス内のデータアセットの量が増えるにつれ、クエリサービスで使用するデータアセットを整理するための一貫性のある論理的なデータ構造を提供します。 詳しくは、 [クエリサービスの概要](../../query-service/home.md). |

詳しくは、 [!DNL Query Service]詳しくは、 [[!DNL Query Service] 概要](../../query-service/home.md).

## サンドボックス {#sandboxes}

Adobe Experience Platform は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。このニーズに対応するために、Experience Platform は、サンドボックスを提供します。サンドボックスでは、単一の Platform インスタンスを別々の仮想環境に分割することができ、デジタルエクスペリエンスアプリケーションの開発と発展に役立ちます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| サンドボックス UI の強化 | サンドボックスインジケーターが、すべての Platform UI アプリケーションのヘッダー内に統合されるようになりました。 サンドボックスインジケーターは、サンドボックスの名前、地域、タイプを表示し、ドロップダウンメニューにアクセスしてサンドボックス間を切り替えることもできます。 詳しくは、 [サンドボックス UI ガイド](../../sandboxes/ui/user-guide.md). |

サンドボックスについて詳しくは、 [サンドボックスの概要](../../sandboxes/home.md).

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| Segment Match | セグメントマッチは、2 人以上の Platform ユーザーが、共通の識別子に基づいて、安全で管理され、プライバシーに優しい方法でデータを交換できるデータコラボレーションサービスです。 詳しくは、 [セグメントマッチの概要](../../segmentation/ui/segment-match/overview.md). |

[!DNL Segmentation Service] について詳しくは、[セグメント化の概要](../../segmentation/home.md)を参照してください。
