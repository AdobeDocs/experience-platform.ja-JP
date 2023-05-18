---
keywords: Experience Platform;データガバナンス;顧客 AI;人気のトピック
solution: Experience Platform
feature: Customer AI
title: 顧客 AI におけるデータガバナンス
description: Adobe Experience Platform には、ビジネスプラクティス、法的義務および開発プロセスに準拠するために、収集されたエクスペリエンスデータを確信を持って制御できるサービスやツールがいくつか用意されています。
exl-id: de0836a4-7bc2-4f9c-95a9-c01dd9e2b03f
source-git-commit: 0fcdb358882fba7f7923e5d6fc1a947699276e18
workflow-type: tm+mt
source-wordcount: '302'
ht-degree: 76%

---

# 顧客 AI とデータガバナンス

顧客 AI におけるデータガバナンス関連の設定はすべて、Adobe Experience Platform から継承されます。

## データガバナンス {#governance}

顧客 AI と Adobe Experience Platform データガバナンスの統合により、Platform からジャーニー全体を通してデータを制御し把握することができます。これには、データ品質、データ系列、データのカタログ化などを維持管理することが必要になります。

Platform で使用されるデータセットに関して作成されたデータ使用ラベルおよびポリシーは、顧客 AI 設定ワークフローで表示できます。 これらのラベルは、ラベル付きフィールドを使用するユーザーの操作を防いだり、ユーザーに警告したりします。

この統合により、コンプライアンスをより効率的に管理できるようになります。組織のデータ管理人は、使用を制限するポリシーを設定できます。その結果、データ管理人が定義したポリシーに準拠するデータを使用できます。 詳しくは、[ラベルとポリシー](https://experienceleague.adobe.com/docs/analytics-platform/using/cja-dataviews/data-governance.html?lang=ja)に関するドキュメントを参照してください。

## 同意ポリシー {#consent-policy}

顧客 AI は、お客様の同意設定に従います。 一度 [同意ポリシーをセットアップして有効にします](https://experienceleague.adobe.com/docs/experience-platform/data-governance/policies/user-guide.html?lang=ja#consent-policy)に設定されている場合、顧客 AI は、お客様から収集された同意データに従います。 モデルの後続の実行で、モデルのスコアリングに同意されたデータのみが使用されます。 新しいスコアは、古いスコアに代わり、セグメント化で使用できます。 この機能は、現在、HealthCare Shield のお客様と、Privacy and Security shield のお客様のみが利用できます。

この機能について詳しくは、以下を参照してください。

[顧客 AI の基本を学ぶ](../../customer-ai/getting-started.md)
[Adobe Experience Platform およびアプリケーション](https://experienceleague.adobe.com/docs/blueprints-learn/architecture/architecture-overview/platform-applications.html?lang=ja)
[Adobe Experience Cloud のアーキテクチャ図](https://experienceleague.adobe.com/docs/blueprints-learn/architecture/architecture-overview/experience-cloud.html?lang=ja)
