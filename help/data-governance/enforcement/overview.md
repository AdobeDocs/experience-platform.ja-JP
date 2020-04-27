---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ポリシーの適用の概要
topic: enforcement
translation-type: tm+mt
source-git-commit: d1659bbdd40cf1e598713f1fe1a6eeae8e8249cc

---


# ポリシーの適用の概要

データ使用ラベルがプラットフォームデータセットに適用され、これらのラベルに対するマーケティングアクションに対してデータ使用ポリシーが定義されると、データガバナンス機能を使用して、これらのポリシーを適用し、ポリシー違反を構成するデータ操作を防ぐことができます。

プラットフォームのデータガバナンス機能によって提供されるポリシーを適用する方法は2つあります。 **APIベースの強制** と自 **動強制**。

## APIベースの強制

Policy Service APIは、マーケティングアクションをデータセットや任意のデータ使用ラベルの組み合わせに対してテストし、ポリシー違反が発生したかどうかを確認できるエンドポイントを提供します。 その後、API応答に基づいて、エクスペリエンスアプリケーション内でプロトコルを設定し、データ使用ポリシーのコンプライアンスを適切に実施できます。

APIを使用してポリシーを評価す [る手順については](api-enforcement.md) 、ポリシーの適用に関するチュートリアルを参照してください。

## 自動強制

エクスペリエンスプラットフォーム（Real-time Customer Data Platformなど）上に構築される特定のアプリケーションは、データ使用ポリシーを自動的に適用します。 各アプリケーションは、ポリシー違反を表示する独自の方法を維持し、問題を解決する手順を提供します。

自動データ使用ポリシーの適用の詳細については、使用しているプラットフォームベースのアプリケーションのドキュメントを参照してください。 リアルタイムCDPでの自動ポリシー適用の詳細については、「 [Real-time CDP Data Governance overview」を参照してください](../../rtcdp/privacy/data-governance-overview.md#enforce-data-usage-compliance)。