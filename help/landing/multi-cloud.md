---
solution: Experience Platform
title: Adobe Experience Platform マルチクラウドの概要
description: Microsoft Azure とAmazon Web ServicesでのExperience Platform実行の違いを説明します。
source-git-commit: d3654573cec338f173d151fd5e62ef5c8b893c11
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 5%

---


# Adobe Experience Platform マルチクラウドの概要

Adobe Experience Platformはマルチクラウド製品であり、[[!DNL Microsoft Azure]](https://azure.microsoft.com/en-us) 上で実行するか [[!DNL Amazon Web Services (AWS)]](https://aws.amazon.com/jp/) 上で実行するかを選択できます。 この柔軟性により、ビジネス要件と技術要件に最適なものを選択できます。

>[!AVAILABILITY]
>
>Amazon Web Services（AWS）上で実行されるAdobe Experience Platformは、現在、限られた数のお客様が利用できます。 AWSでのExperience Platformについて詳しくは、Adobeアカウントチームにお問い合わせください。

このページでは、使用可能な 2 つのクラウドインフラストラクチャの概要を説明し、ビジネスに合わせて適切なクラウドインフラストラクチャを選択する方法に関するガイダンスも含まれています。

## どのクラウド実装が適切ですか？ {#which-cloud-is-right}

Azure またはAWSのどちらでExperience Platformを選択するかは、ビジネスに固有のいくつかの要因によって異なります。

* **ビジネスニーズと技術ニーズ**：組織の要件と長期的なクラウド戦略を評価します。
* **既存のインフラストラクチャ**：現在のクラウドインフラストラクチャと統合のニーズを考慮します。
* **クラウドテクノロジーへの依存**：企業がMicrosoft テクノロジーに大きく依存している場合は、Azure が適している可能性があります。 Amazon サービスへの依存度が高い場合は、AWSが適している可能性があります。
* **データレジデンシーに関する考慮事項**：組織のデータレジデンシー要件を評価し、選択したクラウドプラットフォームがこれらの規制に準拠する地域を提供することを確認します。

上記の要因を考慮して、このシンプルなデシジョンツリーを使用すると、ビジネスニーズに合った適切なクラウド実装の決定に役立ちます。

![ ホスティング場所の地理的分布を示す画像 ](assets/multi-cloud/diagram-cloud.png){align="center" zoomable="yes"}

## ホスティングの場所 {#available-cloud-regions}

適切なクラウド地域の選択は、データ常駐サービスの要件を満たし、最適なパフォーマンスを確保するために重要です。

![ ホスティング場所の地理的分布を示す画像 ](assets/multi-cloud/hosting-locations-map.png){align="center" zoomable="yes"}

Microsoft Azure の 6 つのホスティング場所および 1 つのAmazon Web Services（AWS）のホスティング場所でExperience Platformを利用でき、世界中に分散する 7 つの [Edge Networkノードを介してAdobe サービスにデータをルーティング ](../collection/home.md#edge) きます。

### Microsoft Azure の地域 {#azure-regions}

次の表は、Experience PlatformがホストされているMicrosoft Azure リージョンを示しています。

| 国 | 地域コード | ロケーション |
|---------|-------------|----------|
| アメリカ合衆国 | VA7 | バージニア |
| 英国 | GBR9 | ロンドン |
| オランダ | NDL2 | アムステルダム |
| カナダ | CAN2 | トロント |
| インド | IND2 | マハラシュトラ |
| オーストラリア | AUS5 | ニューサウスウェールズ州 |

{style="table-layout:auto"}

### Amazon Web Services（AWS）地域 {#aws-regions}

次の表は、Experience PlatformがホストされているAWS地域を示しています。 さらに場所が追加されたかどうかを定期的に確認してください。

| 国 | 地域コード | ロケーション |
|---------|-------------|----------|
| アメリカ合衆国 | VA6 | バージニア |

{style="table-layout:auto"}

## 機能パリティ {#feature-parity}

Adobeは、Experience Platform上で稼働するすべてのアプリケーションに対して、クラウドプラットフォーム間で同等の機能を提供するよう取り組んでいます。例えば、次のようなアプリケーションが挙げられます。

* [Real-Time Customer Data Platform](../rtcdp/home.md)
* [Adobe Journey Optimizer](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/ajo-home)
* [Customer Journey Analytics](https://experienceleague.adobe.com/ja/docs/analytics-platform/using/cja-landing)

ただし、Azure とAWSの実装では、一部の機能が異なる場合があります。 これらの違いについては、以下のセクションと、製品ドキュメントの他の部分（該当する場合）で概要を説明します。

### Microsoft Azure とAWSでのExperience Platform実行の違い {#azure-aws-differences}

次の表に、Microsoft Azure とAWSでExperience Platformを実行する場合の主な違いを示します。

| 機能 | Microsoft Azure | Amazon Web Services |
| --- | --- | --- |
| [HIPAA コンプライアンス ](https://www.adobe.com/trust/compliance/hipaa-ready.html) | サポートあり | サポートなし |
| [ ソースコネクタのカタログ ](/help/sources/home.md) | ソースカタログ内のすべてのコネクタがサポートされています | 使用できるソースコネクタの数は限られています。 AWS実装に使用できるソースコネクタは、それぞれのドキュメントページのページ上部のメモで呼び出されます。 |

{style="table-layout:auto"}

<!-- To be determined if we need to add this part about the AI Assistant 

| [Experience Platform AI Assistant](/help/ai-assistant/home.md) | Supported | Not supported |

-->

## まとめ {#conclusion}

Experience Platformは、Microsoft Azure またはAmazon Web Servicesで実行するオプションを提供することで、柔軟性と選択肢を提供します。 ビジネスニーズと既存のインフラストラクチャを評価し、使用するクラウドプラットフォームについて十分な情報に基づいた決定を行います。
