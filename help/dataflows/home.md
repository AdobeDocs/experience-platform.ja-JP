---
keywords: Experience Platform；ホーム；人気のあるトピック；データフロー；データフロー；データフロー；監視；監視データフロー；監視；監視データフロー；監視データフロー；監視データフロー；フロー；フローサービス；
solution: Experience Platform
title: データフローの概要
topic-legacy: overview
description: このドキュメントでは、Adobe Experience Platformでのデータの使い方を表すデータフローを紹介します。
exl-id: 8fe08ffa-f095-4e9f-8bab-d060985f0236
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 5%

---

# データフローの概要

Adobe Experience Platformでは、様々なソースからデータを取り込み、Experience Platform内で分析し、様々な目的地に活性化する。 プラットフォームでは、データフローに透明性を提供することで、この非線形の可能性があるデータフローの追跡プロセスを容易にします。

## データフローの使用

データフローは、Platform 間でデータを移動するデータジョブを表します。これらのデータフローは様々なサービスで構成され、ソース・コネクタからターゲット・データセットにデータを移動できます。このデータは、宛先に対して最終的にアクティブ化される前に、IDサービスとリアルタイム顧客プロファイルによって利用されます。

ソースコネクタでのデータフローの使用について詳しくは、[ソースの概要](../sources/home.md)を参照してください。

## データの準備

データ準備を使用すると、データエンジニアはエクスペリエンスデータモデル(XDM)との間でデータのマッピング、変換、検証を行うことができます。

データの取り込み後にデータを準備する方法の詳細については、[データ準備の概要](../data-prep/home.md)を参照してください。

## データフローの監視

データフローの監視は、プラットフォームAPIまたはプラットフォームUIを使用して行うことができます。 APIを使用してデータフローを監視する方法を学ぶには、[モニタリングデータフローAPIチュートリアル](./api/monitor.md)をお読みください。 プラットフォームUI内のデータフローを監視する方法を学ぶには、[sources](./ui/monitor-sources.md)の監視データフロー[およびdestinations](./ui/monitor-destinations.md)の監視データフローのチュートリアルをお読みください。
