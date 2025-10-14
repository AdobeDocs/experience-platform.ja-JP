---
keywords: Experience Platform;ホーム;人気のトピック;データフロー;データフロー;データ;監視;データフロー監視;データフロー監視;監視;データフロー監視;データフロー監視;フロー;フローサービス;
solution: Experience Platform
title: データフローの概要
description: このドキュメントでは、Adobe Experience Platform でのデータの使い方を示すデータフローを紹介します。
exl-id: 8fe08ffa-f095-4e9f-8bab-d060985f0236
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 69%

---

# データフローの概要

Adobe Experience Platform では、様々なソースからデータを取り込み、Experience Platform 内で分析し、様々な目的でアクティブ化します。Experience Platformでは、データフローに透明性を提供することで、この非線形の可能性があるデータフローのトラッキングプロセスを容易にします。

## データフローの使用

データフローは、Experience Platform間でデータを移動するデータジョブを表します。 これらのデータフローは様々なサービスを対象に設定され、ソースコネクタからターゲットデータセットにデータを移動できます。こうしたデータは、ID サービスとリアルタイム顧客プロファイルで利用されてから、最終的に宛先に対してアクティブ化されます。

ソースコネクタでのデータフローの使用について詳しくは、[ソースの概要](../sources/home.md)を参照してください。

## データの準備

Data Prep を使用すると、データエンジニアはエクスペリエンスデータモデル（XDM）との間でデータのマッピング、変換、検証をおこなうことができます。

データの取り込み後にデータを準備する方法の詳細については、[Data Prep の概要](../data-prep/home.md)を参照してください。

## データフローの監視

データフローの監視は、Experience Platform API またはExperience Platform UI を使用して実行できます。 API を使用してデータフローを監視する方法について詳しくは、[データフロー API の監視チュートリアル](./api/monitor.md)をお読みください。Experience Platform UI 内のデータフローを監視する方法を学ぶには、[&#x200B; ソースのデータフロー監視 &#x200B;](./ui/monitor-sources.md) および [&#x200B; 宛先のデータフロー監視 &#x200B;](./ui/monitor-destinations.md) のチュートリアルをお読みください。
