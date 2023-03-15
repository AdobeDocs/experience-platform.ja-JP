---
keywords: Experience Platform;ホーム;人気のトピック;データフロー;データフロー;データ;監視;データフロー監視;データフロー監視;監視;データフロー監視;データフロー監視;フロー;フローサービス;
solution: Experience Platform
title: データフローの概要
description: このドキュメントでは、Adobe Experience Platform でのデータの使い方を示すデータフローを紹介します。
exl-id: 8fe08ffa-f095-4e9f-8bab-d060985f0236
source-git-commit: 1a7ba52b48460d77d0b7695aa0ab2d5be127d921
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 100%

---

# データフローの概要

Adobe Experience Platform では、様々なソースからデータを取り込み、Experience Platform 内で分析し、様々な目的でアクティブ化します。Platform では、データフローに透明性を提供することで、この非線形の可能性があるデータフローのトラッキングプロセスを容易にします。

## データフローの使用

データフローは、Platform 間でデータを移動するデータジョブを表します。これらのデータフローは様々なサービスを対象に設定され、ソースコネクタからターゲットデータセットにデータを移動できます。こうしたデータは、ID サービスとリアルタイム顧客プロファイルで利用されてから、最終的に宛先に対してアクティブ化されます。

ソースコネクタでのデータフローの使用について詳しくは、[ソースの概要](../sources/home.md)を参照してください。

## データの準備

Data Prep を使用すると、データエンジニアはエクスペリエンスデータモデル（XDM）との間でデータのマッピング、変換、検証をおこなうことができます。

データの取り込み後にデータを準備する方法の詳細については、[Data Prep の概要](../data-prep/home.md)を参照してください。

## データフローの監視

データフローの監視は、Platform API または Platform UI を使用しておこなうことができます。API を使用してデータフローを監視する方法について詳しくは、[データフロー API の監視チュートリアル](./api/monitor.md)をお読みください。Platform UI 内のデータフローを監視する方法を学ぶには、[ソースのデータフロー監視](./ui/monitor-sources.md)および[出力先のデータフロー監視](./ui/monitor-destinations.md)のチュートリアルをお読みください。
