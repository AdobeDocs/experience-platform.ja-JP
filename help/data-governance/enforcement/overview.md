---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ポリシー適用の概要
topic: enforcement
translation-type: tm+mt
source-git-commit: 1a835c6c20c70bf03d956c601e2704b68d4f90fa
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---


# ポリシー適用の概要

データ使用ラベルがデータ [!DNL Platform] セットに適用され、これらのラベルに対するマーケティングアクションに対してデータ使用ポリシーが定義されると、 [!DNL Data Governance] 機能により、これらのポリシーを適用し、ポリシー違反を構成するデータ操作を防ぐことができます。

ポリシーの適用には、次の2つの方法があります。この2つの方法は、 [!DNL Data Governance] 機能によって提供され [!DNL Platform]ます。 **APIベースの強制** と **自動強制**。

## APIベースの強制

この [!DNL Policy Service] APIは、ポリシー違反が発生したかどうかを確認するために、データセットやデータ使用ラベルの任意の組み合わせに対するマーケティングアクションをテストできるエンドポイントを提供します。 API応答に基づいて、エクスペリエンスアプリケーション内でプロトコルを設定し、データ使用ポリシーのコンプライアンスを適切に実施できます。

APIを使用してポリシーを評価する手順については、 [ポリシーの適用に関するチュートリアルを参照してください](api-enforcement.md) 。

## 自動強制

の上に構築されたアプリケーション [!DNL Experience Platform] (など [!DNL Real-time Customer Data Platform])には、データ使用ポリシーを自動的に適用するものもあります。 各アプリケーションは、ポリシー違反の提示方法を維持し、問題の解決手順を提供します。

自動データ使用ポリシーの適用の詳細については、使用している [!DNL Platform]ベースのアプリケーションのドキュメントを参照してください。 リアルタイムCDPでの自動ポリシー適用の詳細については、 [Real-time CDP Data Governanceの概要を参照してください](../../rtcdp/privacy/data-governance-overview.md#enforce-data-usage-compliance)。