---
title: Real-Time CDP ガードレール
description: Real-Time CDPの様々なサービスおよび領域にまたがるデータガードレールについて説明します。
feature: Guardrails, Data Management, Data Ingestion, Data Export
exl-id: 377499b4-5707-4d50-94e3-02f88ad5bf2c
source-git-commit: 7f3459f678c74ead1d733304702309522dd0018b
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 7%

---

# Real-Time CDP ガードレール

ガードレールとは、Real-Time CDPでのデータやシステムの使用状況、パフォーマンスの最適化、エラーや予期しない結果の回避に関するガイダンスを提供するしきい値のことです。 ガードレールは、データの使用状況や消費量、ライセンスのエンタイトルメントに関連する処理方法を参照できます。

>[!IMPORTANT]
>
>このガードレール ページに加えて、販売注文と対応する [ 製品説明 ](https://helpx.adobe.com/jp/legal/product-descriptions.html) でライセンスの使用権限を確認してください。

ここから始めて、以下のリンクに従って、Real-Time CDPの様々なサービスやエリアにわたるすべてのガードレールを理解します。

* [データ取り込みのガードレール](/help/ingestion/guardrails.md)
* [ のガードレール  [!DNL Edge Network API]](https://developer.adobe.com/data-collection-apis/docs/getting-started/guardrails/)
* [データおよびセグメ  [!DNL Real-Time Customer Profile]  ト化のガードレール](/help/profile/guardrails.md)
* [データ用のガ  [!DNL Identity Service]  ドレール](/help/identity-service/guardrails.md)
* [ [!DNL Query Service] のガードレール](/help/query-service/guardrails.md)
* [宛先を介したデータのアクティベーションのためのガードレール](/help/destinations/guardrails.md)
* [Real-Time CDP B2B のガードレール](/help/rtcdp/b2b-guardrails.md)

>[!TIP]
>
>さらに、様々なExperience Platform サービスの [ エンドツーエンドの待ち時間図 ](https://experienceleague.adobe.com/docs/blueprints-learn/architecture/architecture-overview/deployment/guardrails.html?lang=ja) など [ 詳細については、](https://experienceleague.adobe.com/docs/blueprints-learn/architecture/architecture-overview/deployment/guardrails.html?lang=ja#end-to-end-latency-diagrams) デジタルエクスペリエンスブループリント」を参照してください。

## ガードレール タイプ {#guardrail-types}

すべてのReal-Time CDP領域とサービスをまたぐ 2 つのガードレールのタイプは次のとおりです。

| ガードレール タイプ | 説明 |
|----------|---------|
| **パフォーマンスガードレール（ソフトリミット）** | パフォーマンスガードレールは、ユースケースのスコーピングに関連する使用制限です。 パフォーマンスガードレールを超えると、パフォーマンスの低下や待ち時間が発生する場合があります。 Adobeは、このようなパフォーマンス低下に対する責任を負いません。 パフォーマンスのガードレールを常に超えるお客様は、パフォーマンスの低下を避けるために、追加容量のライセンスを選択できます。 |
| **システムで適用されるガードレール（ハードリミット）** | システムに適用されたガードレールは、Real-Time CDP UI または API によって適用されます。 これらは、UI と API によってこれをブロックされるか、エラーを返すので、超えることはできない制限です。 |

{style="table-layout:auto"}

## Real-Time CDP ライセンスと使用権限に関する情報 {#product-descriptions}

さらに、購入したReal-Time CDPのエディションと階層に応じたライセンスと使用権限については、以下の製品説明のリンクを参照してください。

* [ すべてのAdobe製品の説明 ](https://helpx.adobe.com/jp/legal/product-descriptions.html)
* [Real-Time Customer Data Platform（B2C Edition - PrimeおよびUltimate パッケージ） ](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2c-edition-prime-and-ultimate-packages.html)
* [Real-Time Customer Data Platform（B2P Edition - PrimeおよびUltimate パッケージ） ](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2p-edition-prime-and-ultimate-packages.html)
* [Real-Time Customer Data Platform（B2B edition - PrimeおよびUltimate パッケージ） ](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html)

## その他のExperience Platform アプリケーション用のガードレール  {#guardrails-other-aep-apps}

他のExperience Platform アプリケーションにも同様のガードレールがあります。 詳しくは、以下のリンクを参照してください。

* [Adobe Journey Optimizerガードレール ](https://experienceleague.adobe.com/docs/journey-optimizer/using/get-started/guardrails.html?lang=ja)
* [Customer Journey Analyticsガードレール ](https://experienceleague.adobe.com/docs/analytics-platform/using/cja-admin/guardrails.html?lang=ja)

## 次の手順

Real-Time CDPの様々なエリアやサービスに適用されるガードレールを理解したら、[Real-Time CDP実装のサンプルユースケース ](/help/rtcdp/get-started.md) を追うことができます。
