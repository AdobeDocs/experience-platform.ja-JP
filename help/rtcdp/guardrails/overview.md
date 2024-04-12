---
title: Real-Time CDP ガードレール
description: Real-Time CDPの様々なサービスおよび領域にまたがるデータガードレールについて説明します。
feature: Guardrails, Data Management, Data Ingestion, Data Export
exl-id: 377499b4-5707-4d50-94e3-02f88ad5bf2c
source-git-commit: 2704184446f7945c744e7e2d2a8c3cda3fc12527
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 7%

---

# Real-Time CDP ガードレール

ガードレールとは、Real-Time CDPでのデータやシステムの使用状況、パフォーマンスの最適化、エラーや予期しない結果の回避に関するガイダンスを提供するしきい値のことです。 ガードレールは、データの使用状況や消費量、ライセンスのエンタイトルメントに関連する処理方法を参照できます。

ここから始めて、以下のリンクに従って、Real-Time CDPの様々なサービスやエリアにわたるすべてのガードレールを理解します。

* [データ取り込みのガードレール](/help/ingestion/guardrails.md)
* [のガードレール [!DNL Edge Network Server API]](/help/server-api/guardrails.md)
* [のガードレール [!DNL Real-Time Customer Profile] データとセグメント化](/help/profile/guardrails.md)
* [のガードレール [!DNL Identity Service] データ](/help/identity-service/guardrails.md)
* [のガードレール [!DNL Query Service]](/help/query-service/guardrails.md)
* [宛先を介したデータのアクティベーションのためのガードレール](/help/destinations/guardrails.md)
* [Real-Time CDP B2B のガードレール](/help/rtcdp/b2b-guardrails.md)

>[!TIP]
>
>さらに、次を参照してください： [デジタルエクスペリエンスブループリント](https://experienceleague.adobe.com/docs/blueprints-learn/architecture/architecture-overview/deployment/guardrails.html) 次のような詳細情報： [エンドツーエンドの待ち時間図](https://experienceleague.adobe.com/docs/blueprints-learn/architecture/architecture-overview/deployment/guardrails.html?lang=en#end-to-end-latency-diagrams) （様々なExperience Platformサービス用）

## ガードレール タイプ {#guardrail-types}

すべてのReal-Time CDP領域とサービスをまたぐ 2 つのガードレールのタイプは次のとおりです。

| ガードレール タイプ | 説明 |
|----------|---------|
| **パフォーマンスガードレール（ソフトリミット）** | パフォーマンスガードレールは、ユースケースのスコーピングに関連する使用制限です。 パフォーマンスガードレールを超えると、パフォーマンスの低下や待ち時間が発生する場合があります。 Adobeは、このようなパフォーマンス低下に対する責任を負いません。 パフォーマンスのガードレールを常に超えるお客様は、パフォーマンスの低下を避けるために、追加容量のライセンスを選択できます。 |
| **システムで適用されるガードレール（ハードリミット）** | システムに適用されたガードレールは、Real-Time CDP UI または API によって適用されます。 これらは、UI と API によってこれをブロックされるか、エラーを返すので、超えることはできない制限です。 |

{style="table-layout:auto"}

## Real-Time CDP ライセンスと使用権限に関する情報 {#product-descriptions}

さらに、購入したReal-Time CDPのエディションと階層に応じたライセンスと使用権限については、以下の製品説明のリンクを参照してください。

* [すべてのAdobe製品の説明](https://helpx.adobe.com/jp/legal/product-descriptions.html)
* [Real-time Customer Data Platform（B2C Edition - Prime および Ultimate パッケージ）](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2c-edition-prime-and-ultimate-packages.html)
* [Real-time Customer Data Platform（B2P Edition - Prime および Ultimate パッケージ）](https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2p-edition-prime-and-ultimate-packages.html)
* [Real-time Customer Data Platform（B2B Edition - Prime および Ultimate パッケージ）](https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html)

## その他のExperience Platformアプリケーション用のガードレール  {#guardrails-other-aep-apps}

他のExperience Platformアプリケーションにも同様のガードレールがあります。 詳しくは、以下のリンクを参照してください。

* [Adobe Journey Optimizer ガードレール](https://experienceleague.adobe.com/docs/journey-optimizer/using/get-started/guardrails.html?lang=en)
* [Customer Journey Analyticsガードレール](https://experienceleague.adobe.com/docs/analytics-platform/using/cja-admin/guardrails.html)

## 次の手順

Real-Time CDPの様々なエリアやサービスに適用されるガードレールを理解したら、次の手順を実行できます [Real-Time CDP実装のユースケース](/help/rtcdp/get-started.md).
