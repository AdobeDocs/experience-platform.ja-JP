---
keywords: Experience Platform;home;popular topics;Policy enforcement;Automatic enforcement;API-based enforcement;data governance
solution: Experience Platform
title: ポリシー適用の概要
topic: enforcement
description: データ使用ラベルがAdobe Experience Platformのデータセットに適用され、これらのラベルに対するマーケティング活動に対してデータ使用ポリシーが定義されると、データ管理機能により、これらのポリシーを適用し、ポリシー違反を構成するデータ操作を防ぐことができます。 Data Governance機能によって提供されるプラットフォームのポリシー適用には、APIベースの適用と自動適用の2つの方法があります。
translation-type: tm+mt
source-git-commit: 83f1392ffab3571ebd91325123fbe7095ad59e28
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 17%

---


# ポリシー適用の概要

Once data usage labels have been applied to [!DNL Platform] datasets, and data usage policies have been defined for marketing actions against those labels, [!DNL Data Governance] capabilities allow you to enforce those policies and prevent data operations that constitute policy violations.

ポリシーの適用には、次の2つの方法があります。この2つの方法は、 [!DNL Data Governance] 機能によって提供され [!DNL Platform]ます。APIベースの強制および自動強制。

## APIベースの強制

The [!DNL Policy Service] API provides endpoints that allow you to test marketing actions against datasets or arbitrary combinations of data usage labels in order to check if any policy violations occur. その後、API 応答に基づいて、エクスペリエンスアプリケーション内でプロトコルを設定し、データ使用ポリシーのコンプライアンスを適切に実施できます。

API を使用してポリシーを評価する手順については、[ポリシーの適用](api-enforcement.md)に関するチュートリアルを参照してください。

## 自動強制

の上に構築されたアプリケーション [!DNL Experience Platform] (など [!DNL Real-time Customer Data Platform])には、データ使用ポリシーを自動的に適用するものもあります。 各アプリケーションは、ポリシー違反の提示方法を維持し、問題の解決手順を提供します。

リアルタイムCDPでの自動ポリシー適用では、データ系列、データ分類、ポリシー管理機能を利用して、ポリシー違反を評価し、その違反を表示します。 詳細は、 [Real-time CDP Data Governanceの概要](../../rtcdp/privacy/data-governance-overview.md#enforce-data-usage-compliance) を参照してください。