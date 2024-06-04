---
title: Edge Network Server API のパフォーマンスガードレール
description: 最適なパフォーマンスガードレール内で Server API を使用する方法を説明します。
exl-id: 063d0fbb-26d1-4727-9dea-8e7223b2173d
source-git-commit: 5d6b70e397a252e037589c3200053ebcb7eb8291
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 5%

---


# Edge Network Server API のパフォーマンスガードレール

## 概要 {#overview}

パフォーマンスガードレールは、Server API のユースケースに関連する使用制限を定義します。 この記事で説明しているパフォーマンスガードレールを超えると、パフォーマンスが低下する可能性があります。

Adobeは、使用量の制限を超えたことによるパフォーマンスの低下については責任を負いません。 パフォーマンスのガードレールを常に超えるお客様は、パフォーマンスの低下を避けるために追加の処理能力をリクエストできます。

>[!IMPORTANT]
>
>販売注文のライセンスの使用権限と対応するを確認します [製品の説明](https://helpx.adobe.com/jp/legal/product-descriptions.html) 実際の使用制限に関して、このガードレール ページに加えて説明します。

## 定義

* **対象** は、5 分間隔ごとに、エラーで失敗せず、プロビジョニングされたEdge NetworkAPI にのみ関連するExperience PlatformEdge Networkによって処理されたリクエストの割合として計算されます。 指定された 5 分間隔でテナントがリクエストを行わなかった場合、その間隔は 100% 使用可能と見なされます。
* **月間稼動率** 特定の地域について、1 か月におけるすべての 5 分間の利用可能時間の平均として計算されます。
* An **上流** は、Edge Networkの背後にあるサービスで、Adobeサーバーサイド転送、Adobe Edge セグメント化、Adobe Targetなどの特定のデータストリームに対して有効になります。
* A **リクエスト単位** リクエストの 8 KB フラグメントと、データストリーム用に設定された 1 つのアップストリームに対応します。
* A **リクエスト** は、顧客が所有するアプリケーションから [!DNL Server API]. リクエストには、1 つ以上のリクエストユニットを含めることができます。
* An **エラー** は、Edge Networkが原因で失敗したリクエストです [内部サービスエラー](error-handling.md).

## サービス制限

すべてのデータストリームは特定の使用制限を適用します。これは主に、同時に送信できるイベントの数、そのサイズ、およびそれらのリクエストのルーティング先となるアップストリームサービスの数を制御します。

### リクエスト単位

すべての制限が適用され、 **要求単位（RU）**、として定義 **8 KB フラグメント** データストリームで設定された 1 つのアップストリームサービスに送信されるリクエストの。

#### 例

| データストリームごとに設定されたアップストリーム | 平均リクエストサイズ | リクエスト単位 |
| --- | --- | --- |
| 1 （Adobe基盤） | 8 KB （1 個のフラグメント） | 1 |
| 2 （Adobeプラットフォーム、Adobe Target） | 8 KB （1 個のフラグメント） | 2 |
| 2 （Adobeプラットフォーム、Adobe Target） | 16 KB （2 個のフラグメント） | 4 |
| 2 （Adobeプラットフォーム、Adobe Target） | 64 KB （8 個のフラグメント） | 16 |

### リクエスト単位の制限

次の表に、デフォルトの制限値を示します。 より高いリクエスト単位数の制限が必要な場合は、アカウント担当者にお問い合わせください。

| エンドポイント | 1 秒あたりのリクエスト数 |
| --- | --- |
| `/v2/interact` | 4000 |
| `/v2/collect` | 6000 |


### HTTP リクエストのサイズ制限

| ペイロード形式 | リクエストの最大サイズ | 最大 8 KB のリクエストフラグメント |
| --- | --- | --- |
| JSON プレーンテキスト | 64 KB | 8 |


>[!NOTE]
>
>ペイロード自体に応じて、バイナリ形式は通常、プレーンテキスト JSON よりも多くのデータをプッシュできるので、20～40% コンパクトです。 データストリームにより高い処理能力が必要な場合は、カスタマーケア担当者にお問い合わせください。

## 次の手順

他のExperience Platformサービスのガードレール、エンドツーエンドの待ち時間の情報およびReal-Time CDP Product Description のドキュメントからのライセンス情報について詳しくは、次のドキュメントを参照してください。

* [Real-Time CDP ガードレール](/help/rtcdp/guardrails/overview.md)
* [エンドツーエンドの待ち時間図](https://experienceleague.adobe.com/docs/blueprints-learn/architecture/architecture-overview/deployment/guardrails.html?lang=en#end-to-end-latency-diagrams) （様々なExperience Platformサービス用）
* [Real-time Customer Data Platform（B2C Edition - Prime および Ultimate パッケージ）](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2c-edition-prime-and-ultimate-packages.html)
* [Real-time Customer Data Platform（B2P - Prime および Ultimate パッケージ）](https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2p-edition-prime-and-ultimate-packages.html)
* [Real-time Customer Data Platform（B2B - Prime および Ultimate パッケージ）](https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html)
