---
keywords: Experience Platform;ホーム;人気のトピック;query service;Query service;クエリ
solution: Experience Platform
title: クエリサービスの概要
description: このドキュメントでは、Experience Platform 内でのクエリサービスの役割を概説します。
exl-id: fdaefc12-a97d-4e4e-9aed-d3dbd0f43ea0
source-git-commit: e59def7a05862ad880d0b6ada13b1c69c655ff90
workflow-type: tm+mt
source-wordcount: '721'
ht-degree: 89%

---

# [!DNL Query Service] の概要

Adobe Experience Platform は様々なソースからデータを取得します。マーケターにとっての主要な課題は、このデータの意味を理解して、顧客に関するインサイトを得ることです。Adobe Experience Platform [!DNL Query Service] では、標準の SQL を使用して [!DNL Platform] のデータに対してクエリを実行することによってこれを促進します。[!DNL Query Service] を使用すると、[!DNL Data Lake] 内のデータセットを結合して、クエリ結果を新しいデータセットとして取得し、レポートやマシンラーニングで使用したり、[!DNL Real-Time Customer Profile] に取り込んだりできます。このドキュメントでは、[!DNL Experience Platform] 内での[!DNL Query Service]の役割を概説します。

[!DNL Query Service] を使用すると、企業はオンラインからオフラインへのカスタマージャーニーを結び付け、オムニチャネル属性を把握することが可能です。次のビデオでは、エクスペリエンスビジネスが [!DNL Query Service] を活用して主要なユースケースに対応する方法や[!DNL Query Service]の仕組みについて説明します。

>[!VIDEO](https://video.tv.adobe.com/v/29795?quality=12&learn=on)

## 使用 [!DNL Query Service]

[!DNL Query Service] は、データを詳細に分析するための SQL クエリを作成できるユーザーインターフェイスと RESTful API を提供します。ユーザーインターフェイスを使用すると、クエリを作成して実行したり、以前に実行したクエリを表示したり、組織内のユーザーが保存したクエリにアクセスしたりできます。このユーザーインターフェイスは、より広範なデータセットに対してクエリを実行する前にクエリをテストするためのサンドボックスとして使用するためのものです。[!DNL Platform] 内でのインタラクティブサービスの使用に関する詳細については、[クエリサービスのユーザーインターフェイスのガイド](ui/overview.md)を参照してください。RESTful API も同様のエクスペリエンスを提供し、クエリの記述と実行、将来の使用と繰り返しに備えたクエリのスケジュール、記述するクエリのテンプレートの作成をプログラムで行えるようにします。[!DNL Query Service] API の使用に関する詳細については、[クエリサービスデベロッパーガイド](api/getting-started.md)を参照してください。

## [!DNL Query Service] および [!DNL Experience Platform] サービス

[!DNL Query Service] は複数の [!DNL Experience Platform] サービスで動作し、これらのサービスと連携して使用できます。[!DNL Query Service's] の機能を最大限に活用するために、これらのサービスと、[!DNL Query Service] とのやり取りについて理解することをお勧めします。

### [!DNL Data Science Workspace]

Adobe Experience Platform [!DNL Data Science Workspace] は、機械学習と人工知能を使用して、[!DNL Experience Platform] 内に保存されたデータからインサイトを取得します。[!DNL Data Science Workspace] を使用すると、データサイエンティストは、顧客とそのアクティビティに関するレコードと時系列データに基づいてレシピを作成できるため、購入傾向やユーザーが評価して使用する可能性の高い推奨オファーなどの予測が容易になります。[!DNL Query Service] を [!DNL JupyterLab] に統合すると、[!DNL Data Science Workspace] 内で SQL を使用できます。これにより、Adobe Analytics データの調査、変換、分析をおこなうことができます。[!DNL Data Science Workspace] の詳細については [!DNL Data Science Workspace] の概要を、[!DNL Data Science Workspace] と [!DNL Query Service] のやり取りについては [!DNL Query Service] 統合ガイドをお読みください。

### [!DNL Segmentation Service]

Adobe Experience Platform [!DNL Segmentation Service] を使用すると、ユーザーは顧客を、類似特性を共有する小さなグループに分割することができます。その後、これらのオーディエンスを評価して、 [!DNL Real-Time Customer Profile] データ。 [!DNL Query Service] を使用して、 [!DNL Data Lake]. 詳しくは、 [!DNL Segmentation Service] の概要を参照してください。 [!DNL Profile Query Language] (PQL) ガイドを参照してください。

## ユースケース

[!DNL Query Service] では、多くの目的に役立つ柔軟なデータ処理アプローチを提供します。 特に、マーケターによるセグメント化の負担を軽減し、実用的なオーディエンスや意味のあるビジネスインサイトを生成するのに役立ちます。次のユースケースでは、[!DNL Query Service] の機能のさらに詳しい例を示しています。

### Adobe Analytics の閲覧放棄

この[閲覧放棄の例では、Adobe [!DNL Analytics]](./use-cases/abandoned-browse.md) データを使用して、特定の実用的なオーディエンスを作成することに重点を置いています。[!DNL Query Service] は、セグメント化の複雑なロジックに対応して、ダウンストリームで使用するためにパーソナライズされた様々な属性を計算したり、オーディエンスの構築方法を大幅に簡略化したりします。

### Looker BI ダッシュボード

Adobe Experience Platform では、行動データ、CRM データ、POS データなどのすべての保存済みデータセットの取得、保存、構造化および取り込みをおこなうことができます。[!DNL Experience Platform's Query Service] を使用すると、これらのデータセットに対してクエリを実行して、ビジネスに関する特定の質問に回答し、重要なインサイトの生成を開始できます。次のビデオでは、[!DNL Query Service] を使用して Business Intelligence（BI）ツールでダッシュボードを構築する意味を紹介します。

>[!VIDEO](https://video.tv.adobe.com/v/28981?quality=12&learn=on)

## 次の手順とその他のリソース

このドキュメントでは、[!DNL Query Service] の概要と、[!DNL Experience Platform] の範囲内でどのように機能するかについて説明しました。[!DNL Query Service] API 内の様々なエンドポイントとの相互作用に関する詳細については、『[クエリサービスのデベロッパーガイド](api/getting-started.md)』を参照してください。[!DNL Platform] 内でのインタラクティブサービスの使用に関する詳細については、[クエリサービスのユーザーインターフェイスガイド](ui/overview.md)を参照してください。外部クライアントと [!DNL Query Service] の接続に関する包括的なリストについては、[クエリサービスクライアントの概要](clients/overview.md)を参照してください。

クエリを実行する準備を整えるために、次のビデオをご覧ください。このビデオでは、クエリエディターインターフェイス、PSQL クライアント、Business Intelligence（BI）ソリューション、および HTTP API でのクエリの実行に関するヒントとベストプラクティスを紹介します。

>[!VIDEO](https://video.tv.adobe.com/v/29811?quality=12&learn=on)
