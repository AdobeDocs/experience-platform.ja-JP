---
keywords: Experience Platform;ホーム;人気のトピック;query service;Query service;クエリ
solution: Experience Platform
title: クエリサービスの概要
description: クエリサービスの役割 (Experience Platform内 ) を説明します。
exl-id: fdaefc12-a97d-4e4e-9aed-d3dbd0f43ea0
source-git-commit: 5bf54374773fd95ae1c40dd00b5dbe633031b70e
workflow-type: tm+mt
source-wordcount: '860'
ht-degree: 20%

---

# クエリサービスの概要

Adobe Experience Platform は様々なソースからデータを取得します。マーケターにとっての主な課題は、このデータを把握して、顧客に関するインサイトを得ることです。 Platform でデータをクエリするには、標準の SQL およびAdobe Experience Platformクエリサービスを使用できます。 クエリサービスを使用して、データレイク内の任意のデータセットを結合し、クエリ結果を新しいデータセットとして取り込み、レポート、機械学習、またはに取り込むことができます。 [!DNL Real-Time Customer Profile]. このドキュメントでは、Experience Platform 内でのクエリサービスの役割を概説します。

クエリサービスを使用して、オンラインからオフラインのカスタマージャーニーを結び付け、ブランドのオムニチャネルアトリビューションを理解できます。 次のビデオでは、エクスペリエンスビジネスがクエリサービスを使用して主な使用例に対処する方法、およびクエリサービスの仕組みを示します。

>[!VIDEO](https://video.tv.adobe.com/v/29795?quality=12&learn=on)

## クエリサービスの使用 {#usage}

データを分析するには、クエリサービスのユーザーインターフェイスまたは RESTful API を使用して SQL クエリを作成し、実行します。
クエリサービス UI を使用すると、クエリの書き込み、実行、スケジュール設定、以前に実行したクエリの表示、組織内のユーザーが保存したクエリへのアクセスをおこなうことができます。 クエリエディターを使用して、より広いデータセットに対してクエリを実行する前に、クエリをテストすることもできます。 詳しくは、 [クエリサービス UI ガイド](ui/overview.md) :UI 機能の概要を示します。

RESTful API も同様のエクスペリエンスを提供します。 クエリサービス API を使用すると、クエリの書き込みと実行、適応させるクエリのテンプレートの作成と保存、クエリの自動実行スケジュールの設定をプログラムで実行できます。 詳しくは、 [クエリサービス開発者ガイド](api/getting-started.md) クエリサービス API の使用に関する詳細。

クエリサービス機能の使用をすぐに開始するには、次のドキュメントを読むことをお勧めします。

- [クエリ実行の一般的なガイダンス](./best-practices/writing-queries.md)
- [クエリサービスの SQL 構文](./sql/syntax.md)
- [SQL を使用した派生データセットの作成](./data-distiller/derived-datasets/create-derived-datasets-with-sql.md)

## クエリサービスと Experience Platform サービス {#experience-platform-services}

クエリサービスは相互に作用し、複数のクエリサービスと共に使用できますExperience Platform。 クエリサービスの機能を最大限に活用するには、これらのサービスと、それらのクエリサービスとのやり取りについて理解する必要があります。 The [Experience Platformドキュメントのランディングページ](https://experienceleague.adobe.com/docs/experience-platform.html?lang=ja) には、プラットフォームの機能の概要とリンクが記載されています。

### [!DNL Data Science Workspace] {#data-science-workspace}

Adobe Experience Platform [!DNL Data Science Workspace] は、機械学習と人工知能を使用して、Experience Platform内に保存されたデータからインサイトを得ます。 データサイエンティストは、 [!DNL Data Science Workspace] を使用して、顧客とそのアクティビティに関するレコードと時系列のデータに基づいてレシピを作成します。 これらのレシピは、購入傾向や、個人が喜んで受け入れる可能性が高い推奨オファーなどの予測を促進します。 SQL は、 [!DNL Data Science Workspace] クエリサービスを [!DNL JupyterLab] Adobe Analyticsデータを調査、変換、分析する。 詳しくは、 [[!DNL Data Science Workspace] 概要](../data-science-workspace/home.md) そして [Jupyter Notebook 接続ガイド](./clients/jupyter-notebook.md) 詳しくは、 [!DNL Data Science Workspace] はクエリサービスとやり取りする

### [!DNL Segmentation Service] {#segmentation}

Adobe Experience Platformセグメント化サービスを使用すると、顧客を、類似した特性を共有する小さなグループに分割できます。 これらのオーディエンスを評価して、リアルタイム顧客プロファイルデータの分析を改善できます。 クエリサービスを使用して、データレイク内でこのオーディエンスデータに対してクエリを実行し、分析を提供できます。 詳しくは、 [セグメント化サービスの概要](../segmentation/home.md) そして [[!DNL Profile Query Language] (PQL) ガイド](../segmentation/pql/overview.md) オーディエンスの分析方法の詳細については、を参照してください。

## ユースケース {#use-cases}

クエリサービスは、様々な目的に対応する柔軟なデータ処理アプローチを提供します。 特に、マーケターからのセグメント化の負担を軽減し、実用的なオーディエンスや意味のあるビジネスインサイトを生み出すのに役立ちます。 次の使用例では、クエリサービスの機能の詳細な例を示しています。

### Adobe Analytics の閲覧放棄 {#abandon-browse}

この[閲覧放棄の例では、Adobe [!DNL Analytics]](./use-cases/abandoned-browse.md) データを使用して、特定の実用的なオーディエンスを作成することに重点を置いています。クエリサービスは、セグメント化の複雑なロジックに対応し、ダウンストリームで使用するためにパーソナライズされた様々な属性を計算したり、オーディエンスの構築方法を大幅に簡略化したりします。

## カスタムダッシュボードでインサイトを生成 {#custom-dashboards}

Adobe Experience Platform では、行動データ、CRM データ、POS データなどのすべての保存済みデータセットの取得、保存、構造化および取り込みをおこなうことができます。[!DNL Experience Platform's Query Service] を使用すると、これらのデータセットに対してクエリを実行して、ビジネスに関する特定の質問に回答し、重要なインサイトの生成を開始できます。カスタムダッシュボードを作成および管理する方法について説明します。カスタムダッシュボードでは、カスタムウィジェットを作成、追加および編集して、主要指標を視覚化します。 [ユーザ定義のダッシュボード](../dashboards/user-defined-dashboards.md). 次のことが可能です。 [独自のReal-Time CDPレポートのカスタマイズ](../dashboards/cdp-insights-data-model.md) Real-time Customer Data Platform Insights データモデルで SQL クエリを使用することで、マーケティングおよび KPI の使用例に応じて変更できます。

## 次の手順とその他のリソース

このドキュメントでは、クエリサービスと、Experience Platform の範囲内でクエリサービスがどのように機能するかについて説明しました。クエリサービス機能の学習を続けるには、次のドキュメントを読むことをお勧めします。

- The [クエリサービス開発者ガイド](api/getting-started.md)：クエリサービス API 内の様々なエンドポイントとのやり取りに関する詳細。
- The [クエリサービスユーザーインターフェイスガイド](ui/overview.md)：クエリエディターと UI の使用に関する詳細。
- The [クエリサービスクライアントの概要](clients/overview.md)：クエリサービスに接続する外部クライアントの包括的なリスト。

クエリを実行する準備を整えるために、次のビデオをご覧ください。このビデオでは、クエリエディターインターフェイス、PSQL クライアント、Business Intelligence（BI）ソリューション、および HTTP API でのクエリの実行に関するヒントとベストプラクティスを紹介します。

>[!VIDEO](https://video.tv.adobe.com/v/29811?quality=12&learn=on)
