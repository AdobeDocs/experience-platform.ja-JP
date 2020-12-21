---
keywords: Experience Platform;home;popular topics;Policy enforcement;Automatic enforcement;API-based enforcement;data governance
solution: Experience Platform
title: ポリシー適用の概要
topic: enforcement
description: データ使用ラベルがAdobe Experience Platformのデータセットに適用され、これらのラベルに対するマーケティング活動に対してデータ使用ポリシーが定義されると、データ管理機能により、これらのポリシーを適用し、ポリシー違反を構成するデータ操作を防ぐことができます。 Data Governance機能によって提供されるプラットフォームのポリシー適用には、APIベースの適用と自動適用の2つの方法があります。
translation-type: tm+mt
source-git-commit: 30733f2274ff8cb9ae73cf2b9f7f0219fefbd393
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 12%

---


# ポリシー適用の概要

[!DNL Platform]データセットにデータ使用ラベルが適用され、これらのラベルに対するマーケティングアクションに対してデータ使用ポリシーが定義されると、[!DNL Data Governance]機能により、これらのポリシーを適用し、ポリシー違反を構成するデータ操作を防ぐことができます。

[!DNL Platform]上の[!DNL Data Governance]機能によって提供されるポリシー適用には、次の2つの方法があります。APIベースの強制および自動強制。

## APIベースの強制

[!DNL Policy Service] APIはエンドポイントを提供します。エンドポイントを使用すると、データセットやデータ使用ラベルの任意の組み合わせに対するマーケティング操作をテストし、ポリシー違反が発生したかどうかを確認できます。 その後、API 応答に基づいて、エクスペリエンスアプリケーション内でプロトコルを設定し、データ使用ポリシーのコンプライアンスを適切に実施できます。

APIを使用してポリシーを評価する手順については、[APIベースの強制](./api-enforcement.md)のチュートリアルを参照してください。

## 自動強制

Experience Platformでは、データ・ライング、データ・クラス分け、ポリシー管理機能を活用して、ポリシー違反を自動的に評価し、表面化します。 詳しくは、[自動ポリシー適用](./auto-enforcement.md)の概要を参照してください。