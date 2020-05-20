---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ポリシー適用の概要
topic: enforcement
translation-type: tm+mt
source-git-commit: d1659bbdd40cf1e598713f1fe1a6eeae8e8249cc
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 0%

---


# ポリシー適用の概要

データ使用ラベルがプラットフォームデータセットに適用され、これらのラベルに対するマーケティングアクションに対してデータ使用ポリシーが定義されると、Data Governance機能により、これらのポリシーを適用し、ポリシー違反を構成するデータ操作を回避できます。

プラットフォームのData Governance機能でポリシーを適用する方法は2つあります。 **APIベースの強制** と **自動強制**。

## APIベースの強制

Policy Service APIは、ポリシー違反が発生したかどうかを確認するために、データセットやデータ使用ラベルの任意の組み合わせに対するマーケティングアクションをテストできるエンドポイントを提供します。 API応答に基づいて、エクスペリエンスアプリケーション内でプロトコルを設定し、データ使用ポリシーのコンプライアンスを適切に実施できます。

APIを使用してポリシーを評価する手順については、 [ポリシーの適用に関するチュートリアルを参照してください](api-enforcement.md) 。

## 自動強制

エクスペリエンスプラットフォーム上に構築される特定のアプリケーション（リアルタイム顧客データプラットフォームなど）では、データ使用ポリシーが自動的に適用されます。 各アプリケーションは、ポリシー違反の提示方法を維持し、問題の解決手順を提供します。

自動データ使用ポリシーの適用の詳細については、使用しているプラットフォームベースのアプリケーションのドキュメントを参照してください。 リアルタイムCDPでの自動ポリシー適用の詳細については、 [Real-time CDP Data Governanceの概要を参照してください](../../rtcdp/privacy/data-governance-overview.md#enforce-data-usage-compliance)。