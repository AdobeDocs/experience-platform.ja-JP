---
title: 拡張アプリレポートのためのカスタマイズ可能なインサイト
description: SQL クエリを使用して、カスタムダッシュボードのインサイトを生成する方法を説明します。
exl-id: c60a9218-4ac0-4638-833b-bdbded36ddf5
source-git-commit: 5bb954da7c1e05922a4e0f8d0bc7d3ab5c8e0e58
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 0%

---

# 拡張アプリレポート用のカスタマイズ可能なインサイト

カスタム SQL クエリを使用すると、様々な構造化データセットからインサイトを効果的に抽出できます。 技術者は、query pro モードを使用して SQL で複雑な分析を実行し、カスタムダッシュボードのグラフを使用してこの分析を技術者以外のユーザーと共有したり、CSV ファイルで書き出したりできます。 このインサイトの作成方法は、関係が明確なテーブルに適しており、ニッチなユースケースに適したインサイトやフィルター内のカスタマイズを強化できます。

>[!IMPORTANT]
>
>Query pro モードは、[Data Distiller SKU](../../../query-service/data-distiller/overview.md) を購入したユーザーのみが利用できます。

SQL からインサイトを生成するには、まずダッシュボードを作成する必要があります。

## カスタムダッシュボードの作成 {#create-custom-dashboard}

カスタムダッシュボードを作成するには、左側のナビゲーションパネルから **[!UICONTROL ダッシュボード]** を選択して、ダッシュボード ワークスペースを開きます。 次に、「**[!UICONTROL ダッシュボードを作成]**」を選択します。

![ 作成ダッシュボードがハイライト表示されたダッシュボードインベントリ。](../../images/customizable-insights/create-dashboard.png)

**[!UICONTROL ダッシュボードを作成]** ダイアログが表示されます。 ダッシュボードの作成方法を選択する方法は 2 つあります。 インサイトを作成するには、[[!UICONTROL  ガイド付き設計モード ]](../../user-defined-dashboards.md) で既存のデータモデルを使用するか、[!UICONTROL Query pro モード ] で独自の SQL を使用します。

<!-- Maybe reference Guided design mode in other places on UDD doc. -->

既存のデータモデルを使用すると、特定のビジネスニーズに合わせて構造化された、効率的かつスケーラブルなフレームワークを提供できるというメリットがあります。 [ 既存のデータモデルからインサイトを作成する ](../../user-defined-dashboards.md#create-widget) 方法については、カスタムダッシュボードガイドを参照してください。

SQL クエリから生成されたインサイトは、はるかに高い柔軟性とカスタマイズを提供します。 技術者は、query pro モードを使用して SQL で複雑な分析を実行し、このダッシュボード機能を通じて技術者以外のユーザーとこの分析を共有できます。 **[!UICONTROL クエリプロモード]** を選択してから **[!UICONTROL 保存]** を選択します。

>[!NOTE]
>
>選択を行った後は、そのダッシュボード内でこの選択を変更することはできません。 代わりに、別のダッシュボード作成方法で新しいダッシュボードを作成する必要があります。

![Query pro モードと「保存」がハイライト表示された [!UICONTROL  ダッシュボードを作成 ] ダイアログ ](../../images/customizable-insights/query-pro-mode.png)

## グラフの作成 {#create-a-chart}

SQL を使用してグラフを作成する手順については、[query pro mode guide](./query-pro-mode.md) を参照してください。
