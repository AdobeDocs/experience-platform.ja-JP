---
keywords: Experience Platform;ホーム;人気のトピック;query service;Query service;クエリ
solution: Experience Platform
title: クエリサービスの概要
description: Experience Platform内でのクエリサービスの役割について説明します。
exl-id: fdaefc12-a97d-4e4e-9aed-d3dbd0f43ea0
source-git-commit: e0af1f0110ceb514a5b249c42a05bf780ea834c6
workflow-type: tm+mt
source-wordcount: '860'
ht-degree: 20%

---

# クエリサービスの概要

Adobe Experience Platform は様々なソースからデータを取得します。マーケターにとって主な課題は、このデータを理解して顧客に関するインサイトを得ることです。 Platform でデータに対してクエリを実行する場合は、標準の SQL およびAdobe Experience Platform クエリサービスを使用できます。 クエリサービスを使用すると、データレイク内のデータセットを結合したり、クエリ結果を新しいデータセットとして取得したりして、レポートやマシンラーニングで使用したり、に取り込んだりできます [!DNL Real-Time Customer Profile]. このドキュメントでは、Experience Platform 内でのクエリサービスの役割を概説します。

クエリサービスを使用してオンラインからオフラインへのカスタマージャーニーを接続し、ブランドのオムニチャネル属性を把握できます。 次のビデオでは、エクスペリエンスビジネスがクエリサービスを使用して主要なユースケースに対応する方法や、クエリサービスの仕組みについて説明します。

>[!VIDEO](https://video.tv.adobe.com/v/29795?quality=12&learn=on)

## クエリサービスの使用 {#usage}

データを分析するには、クエリサービスのユーザーインターフェイスまたは RESTful API を使用して SQL クエリを作成し、実行します。
クエリサービス UI を使用すると、クエリの書き込み、実行、スケジュール設定、以前に実行したクエリの表示、組織内のユーザーが保存したクエリへのアクセスを行うことができます。 また、クエリエディターを使用して、より広いデータセットに対してクエリを実行する前に、クエリをテストすることもできます。 を参照してください。 [クエリサービス UI ガイド](ui/overview.md) の説明を参照してください。

RESTful API も同様のエクスペリエンスを提供します。 Query Service API を使用すると、クエリをプログラムで記述して実行したり、適応させるクエリのテンプレートを作成して保存したり、自動実行のためにクエリをスケジュールしたりできます。 を参照してください。 [クエリサービスデベロッパーガイド](api/getting-started.md) query Service API の使用に関する詳細を参照してください。

クエリサービスの機能をすぐに使い始めるには、次のドキュメントを読むことをお勧めします。

- [クエリ実行の一般的なガイダンス](./best-practices/writing-queries.md)
- [クエリサービスの SQL 構文](./sql/syntax.md)
- [SQL を使用した派生データセットの作成](./data-distiller/derived-datasets/create-derived-datasets-with-sql.md)

## クエリサービスと Experience Platform サービス {#experience-platform-services}

クエリサービスはやり取りし、複数のExperience Platformサービスで使用できます。 クエリサービスの機能を最大限に活用するには、これらのサービスと、クエリサービスとのやり取りについて理解する必要があります。 この [Experience Platformドキュメントのランディングページ](https://experienceleague.adobe.com/docs/experience-platform.html?lang=ja) プラットフォームの機能の概要とリンクを示します。

### [!DNL Data Science Workspace] {#data-science-workspace}

Adobe Experience Platform [!DNL Data Science Workspace] は、機械学習と人工知能を使用して、Experience Platform内に保存されたデータからインサイトを取得します。 データサイエンティストは、 [!DNL Data Science Workspace] 顧客とそのアクティビティに関するレコードと時系列データに基づいてレシピを作成する。 これらのレシピは、購入傾向や、個人が評価して使用する可能性が高い推奨オファーなどの予測を容易にします。 内で SQL を使用できます [!DNL Data Science Workspace] にクエリサービスを統合する [!DNL JupyterLab] を使用して、Adobe Analytics データを調査、変換、分析します。 を読み取る [[!DNL Data Science Workspace] の概要](../data-science-workspace/home.md) および [Jupyter Notebook 接続ガイド](./clients/jupyter-notebook.md) の方法について詳しくは、 [!DNL Data Science Workspace] クエリサービスとやり取りします。

### [!DNL Segmentation Service] {#segmentation}

Adobe Experience Platform Segmentation Service を使用して、顧客を類似の特性を共有する小さなグループに分割します。 その後、これらのオーディエンスを評価して、リアルタイム顧客プロファイルデータをより詳細に分析できます。 クエリサービスを使用して、データレイク内でこのオーディエンスデータに対してクエリを実行し、分析を提供できます。 を読み取る [セグメント化サービスの概要](../segmentation/home.md) および [[!DNL Profile Query Language] （PQL）ガイド](../segmentation/pql/overview.md) オーディエンスの分析方法について詳しくは、こちらを参照してください。

## ユースケース {#use-cases}

クエリサービスは、多くの目的に役立つ柔軟なデータ処理アプローチを提供します。 特に、マーケターによるセグメント化の負担を軽減し、実用的なオーディエンスや意味のあるビジネスインサイトを生成するのに役立ちます。 次のユースケースでは、クエリサービスの機能のより詳細な例を示しています。

### Adobe Analytics の閲覧放棄 {#abandon-browse}

この[閲覧放棄の例では、Adobe [!DNL Analytics]](./use-cases/abandoned-browse.md) データを使用して、特定の実用的なオーディエンスを作成することに重点を置いています。クエリサービスは、セグメント化の複雑なロジックを調整して、ダウンストリームで使用するためのパーソナライズされた様々な属性を計算したり、オーディエンスの作成方法を大幅に簡略化したりできます。

## カスタムダッシュボードを使用したインサイトの生成 {#custom-dashboards}

Adobe Experience Platform では、行動データ、CRM データ、POS データなどのすべての保存済みデータセットの取得、保存、構造化および取り込みをおこなうことができます。[!DNL Experience Platform's Query Service] を使用すると、これらのデータセットに対してクエリを実行して、ビジネスに関する特定の質問に回答し、重要なインサイトの生成を開始できます。カスタムダッシュボードを作成および管理する方法を説明します。カスタムダッシュボードでは、カスタムウィジェットを作成、追加および編集して、主要指標を視覚化できます [ユーザー定義ダッシュボード](../dashboards/user-defined-dashboards.md). 次の操作も可能 [独自のReal-Time CDP レポートのカスタマイズ](../dashboards/data-models/cdp-insights-data-model-b2c.md) Real-time Customer Data Platform インサイトデータモデルで SQL クエリを使用した、マーケティングおよび KPI のユースケースの場合。

## 次の手順とその他のリソース

このドキュメントでは、クエリサービスと、Experience Platform の範囲内でクエリサービスがどのように機能するかについて説明しました。クエリサービスの機能について引き続き学習するには、次のドキュメントを読むことをお勧めします。

- この [クエリサービスデベロッパーガイド](api/getting-started.md):Query Service API 内の様々なエンドポイントとの相互作用に関する詳細を説明します。
- この [クエリサービスのユーザーインターフェイスのガイド](ui/overview.md)：クエリエディターと UI の使用に関する詳細
- この [クエリサービスクライアントの概要](clients/overview.md)：クエリサービスに接続する外部クライアントの包括的なリスト用。

クエリを実行する準備を整えるために、次のビデオをご覧ください。このビデオでは、クエリエディターインターフェイス、PSQL クライアント、Business Intelligence（BI）ソリューション、および HTTP API でのクエリの実行に関するヒントとベストプラクティスを紹介します。

>[!VIDEO](https://video.tv.adobe.com/v/29811?quality=12&learn=on)
