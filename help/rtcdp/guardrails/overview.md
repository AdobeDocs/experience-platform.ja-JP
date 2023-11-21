---
title: Real-Time CDP Guardrails
description: Real-Time CDPの様々なサービスや領域にわたるデータガードレールについて説明します。
feature: Guardrails, Data Management, Data Ingestion, Data Export
source-git-commit: db57fa753a3980dca671d476521f9849147880f1
workflow-type: tm+mt
source-wordcount: '391'
ht-degree: 11%

---

# Real-Time CDP Guardrails

Guardrail は、データとシステムの使用状況、パフォーマンスの最適化、Real-Time CDPでのエラーや予期しない結果の回避に関するガイダンスを提供するしきい値です。 ガードレールは、データの使用状況や消費量、ライセンスのエンタイトルメントに関連する処理方法を参照できます。

ここから始め、以下のリンクに従って、Real-Time CDPの様々なサービスおよび領域にわたるすべてのガードレールを理解します。

* [データ取り込みのガードレール](/help/ingestion/guardrails.md)
* [のガードレール [!DNL Edge Network Server API]](/help/server-api/guardrails.md)
* [のガードレール [!DNL Real-Time Customer Profile] データとセグメント化](/help/profile/guardrails.md)
* [ [!DNL Identity Service] データのガードレール](/help/identity-service/guardrails.md)
* [のガードレール [!DNL Query Service]](/help/query-service/guardrails.md)
* [宛先を介したデータのアクティベーションのガードレール](/help/destinations/guardrails.md)

>[!TIP]
>
>また、 [このページ](https://experienceleague.adobe.com/docs/blueprints-learn/architecture/architecture-overview/deployment/guardrails.html) を参照してください。 [Adobe Journey Optimizer](https://experienceleague.adobe.com/docs/journey-optimizer.html?lang=ja) および [Customer Journey Analytics](https://experienceleague.adobe.com/docs/customer-journey-analytics.html?lang=ja)、および [エンドツーエンドの待ち時間図](https://experienceleague.adobe.com/docs/blueprints-learn/architecture/architecture-overview/deployment/guardrails.html?lang=en#end-to-end-latency-diagrams) 様々なExperience Platformサービス。

## ガードレールのタイプ {#guardrail-types}

すべてのReal-Time CDP領域およびサービスにわたって、次の 2 つのガードレールタイプがあることに注意してください。

| ガードレールのタイプ | 説明 |
|----------|---------|
| **パフォーマンスガードレール（ソフト制限）** | パフォーマンスガードレールは、使用例の範囲に関連する使用制限です。 パフォーマンスガードレールを超えると、パフォーマンスが低下し、遅延が発生する場合があります。 Adobeは、そのようなパフォーマンスの低下に対して責任を負いません。 パフォーマンスガードレールを常に上回るお客様は、パフォーマンスの低下を避けるために、追加の容量をライセンスすることを選択できます。 |
| **システムが適用するガードレール（ハード制限）** | システムが適用するガードレールは、Real-Time CDP UI または API によって適用されます。 UI や API によってブロックされたり、エラーが返されたりするので、上限を超えることはできません。 |

{style="table-layout:auto"}

## Real-Time CDPのライセンスと権利に関する情報 {#product-descriptions}

さらに、購入したReal-Time CDPエディションと層に基づくライセンスと使用権限の情報については、以下の製品説明リンクを参照してください。

* [すべてのAdobe製品の説明](https://helpx.adobe.com/jp/legal/product-descriptions.html)
* [Real-time Customer Data Platform（B2C 版 — プライムパッケージおよび究極パッケージ）](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2c-edition-prime-and-ultimate-packages.html)
* [Real-time Customer Data Platform（B2P エディション — プライムパッケージおよび最終パッケージ）](https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2p-edition-prime-and-ultimate-packages.html)
* [Real-time Customer Data Platform（B2B エディション — プライムパッケージおよび究極パッケージ）](https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html)

## 次の手順

Real-Time CDPの様々な領域やサービスに適用されるガードレールについて理解したら、 [Real-Time CDP実装の使用例](/help/rtcdp/get-started.md).
