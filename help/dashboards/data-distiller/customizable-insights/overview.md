---
title: 拡張アプリレポートのためのカスタマイズ可能なインサイト
description: SQL クエリを使用して、カスタムダッシュボードのインサイトを生成する方法を説明します。
source-git-commit: 17ad52864bbca09844c0241b6451e6811bd8f413
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 0%

---

# 拡張アプリレポート用のカスタマイズ可能なインサイト

カスタム SQL クエリを使用すると、様々な構造化データセットからインサイトを効果的に抽出できます。 技術者は、query pro モードを使用して SQL で複雑な分析を実行し、カスタムダッシュボードのグラフを使用してこの分析を技術者以外のユーザーと共有したり、CSV ファイルで書き出したりできます。 このインサイトの作成方法は、関係が明確なテーブルに適しており、ニッチなユースケースに適したインサイトやフィルター内のカスタマイズを強化できます。

>[!IMPORTANT]
>
>Query pro モードは、を購入したユーザーのみが使用できます [Data Distiller SKU](../../../query-service/data-distiller/overview.md).

SQL からインサイトを生成するには、まずダッシュボードを作成する必要があります。

## カスタムダッシュボードの作成 {#create-custom-dashboard}

カスタムダッシュボードを作成するには、次を選択します **[!UICONTROL ダッシュボード]** 左側のナビゲーションパネルからダッシュボード ワークスペースを開きます。 次に、を選択します **[!UICONTROL ダッシュボードを作成]**.

![作成ダッシュボードがハイライト表示されたダッシュボードインベントリ。](../../images/customizable-insights/create-dashboard.png)

この **[!UICONTROL ダッシュボードを作成]** ダイアログが表示されます。 ダッシュボードの作成方法を選択する方法は 2 つあります。 インサイトを作成するには、で既存のデータモデルを使用します [[!UICONTROL ガイド付きデザインモード]](../../user-defined-dashboards.md) または、を使用した独自の SQL [!UICONTROL Query pro モード].

<!-- Maybe reference Guided design mode in other places on UDD doc. -->

既存のデータモデルを使用すると、特定のビジネスニーズに合わせて構造化された、効率的かつスケーラブルなフレームワークを提供できるというメリットがあります。 方法を説明します [既存のデータモデルからのインサイトの作成](../../user-defined-dashboards.md#create-widget)詳しくは、カスタムダッシュボードガイドを参照してください。

SQL クエリから生成されたインサイトは、はるかに高い柔軟性とカスタマイズを提供します。 技術者は、query pro モードを使用して SQL で複雑な分析を実行し、このダッシュボード機能を通じて技術者以外のユーザーとこの分析を共有できます。 を選択 **[!UICONTROL Query pro モード]** 続いて **[!UICONTROL 保存]**.

>[!NOTE]
>
>選択を行った後は、そのダッシュボード内でこの選択を変更することはできません。 代わりに、別のダッシュボード作成方法で新しいダッシュボードを作成する必要があります。

![この [!UICONTROL ダッシュボードを作成] 「Query pro」モードと「保存」がハイライト表示されたダイアログ](../../images/customizable-insights/query-pro-mode.png)

## グラフの作成 {#create-a-chart}

を参照してください。 [query pro モードガイド](./query-pro-mode.md) SQL でグラフを作成する手順を説明します。
