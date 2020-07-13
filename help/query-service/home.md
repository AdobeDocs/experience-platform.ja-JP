---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platformクエリサービス
topic: overview
translation-type: tm+mt
source-git-commit: cfd767a05ad4c1dd43ac2bdde1966f9c89f5d219
workflow-type: tm+mt
source-wordcount: '709'
ht-degree: 0%

---


# クエリサービスの概要

Adobe Experience Platformは様々なソースからデータを取り込みます。 マーケターにとっての大きな課題は、顧客に関するインサイトを得るために、このデータを理解することです。 Adobe Experience Platformクエリサービスは、標準のSQLからクエリデータをPlatformで使用できるようにすることで、これを容易にします。 クエリサービスを使用すると、Data Lakeの任意のデータセットに参加し、クエリ結果を新しいデータセットとして取り込み、レポート、機械学習、リアルタイム顧客プロファイルへの取り込みに使用できます。 このドキュメントでは、Experience Platform内のクエリサービスの役割の概要を説明します。

クエリサービスを使用すると、ブランドがオンラインからオフラインへの顧客の遍歴を結びつけ、オムニチャネルアトリビューションを理解できます。 次のビデオでは、エクスペリエンスのビジネスでクエリサービスを利用して、主な使用例やクエリサービスの仕組みに対処する方法を示します。

>[!VIDEO](https://video.tv.adobe.com/v/29795?quality=12&learn=on)

## クエリサービスの使用

クエリサービスは、SQLクエリを作成してデータをより良く分析できるユーザーインターフェイスとRESTful APIを提供します。 ユーザーインターフェイスを使用して、クエリの書き込みと実行、表示が以前に実行したクエリの作成と実行、およびIMS組織内のユーザーによって保存されたクエリへのアクセスを行うことができます。 ユーザーインターフェイスは、より広いデータセットでクエリを実行する前にユーザーをテストするサンドボックスとして使用することを目的としています。 Platform内でのインタラクティブサービスの使用について詳しくは、『 [クエリサービスユーザーインターフェイスガイド](ui/overview.md)』を参照してください。 RESTful APIも同様の経験を提供し、クエリの書き込みと実行、将来の使用と繰り返しに関するクエリのスケジュール設定、および書き込むクエリ用のテンプレートの作成をプログラムによって行うことができます。 クエリサービスAPIの使用について詳しくは、 [クエリサービス開発ガイドを参照してください](api/getting-started.md)。

## クエリサービスとExperience Platformサービス

クエリサービスは、対話的で、複数のExperience Platformサービスと組み合わせて使用できます。 クエリサービスの機能を最大限に活用するために、これらのサービスとクエリサービスとのやり取りについて理解することをお勧めします。

### Data Science Workspace

Adobe Experience Platformデータサイエンスワークスペースでは、機械学習と人工知能を使用して、Experience Platform内に保存されたデータからの洞察を得ます。 Data Science Workspaceを使用すると、データ科学者は顧客とそのアクティビティに関する記録と時系列のデータに基づいてレシピを作成でき、購入傾向や推奨オファーなど、個人が認識し、使用する可能性が高い予測を容易にできます。 クエリサービスをJupterLabに統合することで、Data Science Workspace内でSQLを使用できます。これにより、AdobeAnalyticsのデータを調査、変換、分析できます。 Data Science Workspaceの詳細については、「Data Science Workspaceの概要」をお読みください。Data Science Workspaceがクエリサービスと相互作用する方法の詳細については、『クエリサービス統合ガイド』を参照してください。

### Segmentation Service

Adobe Experience Platformセグメントサービスを使用すると、類似の特性を共有する小さなグループに顧客を分割できます。 その後、これらのセグメントを評価して、リアルタイム顧客プロファイルデータの分析を改善できます。 クエリサービスを使用してこの分析を提供するには、データレーク内でこのセグメントデータに対してクエリを実行します。 セグメント化の詳細については、Segmentation Serviceの概要を参照し、セグメントの分析方法の詳細については、『プロファイルクエリ言語(PQL)ガイド』を参照してください。

### Looker BIの使用例

Adobe Experience Platformにより、行動、CRM、POS（販売時点管理）データを含むすべての保存済みデータセットを取り込み、保存、構造化、取り込みできます。 を使用 [!DNL Experience Platform's Query Service]すると、これらのデータセットにクエリを付け、ビジネスに関する特定の質問に答え、開始がインパクトなインサイトを生み出すことができます。 次のビデオでは、を使用してビジネスインテリジェンス(BI)ツールでダッシュボードを作成する価値を示し [!DNL Query Service]ます。

>[!VIDEO](https://video.tv.adobe.com/v/28981?quality=12&learn=on)

## 次の手順とその他のリソース

このドキュメントを読むことで、クエリサービスと、Experience Platformの範囲内でのその機能についての紹介を受けました。 クエリサービスAPI内の様々なエンドポイントとの対話について詳しくは、 [クエリサービス開発ガイドを参照してください](api/getting-started.md)。 Platform内でのインタラクティブサービスの使用について詳しくは、『 [クエリサービスユーザーインターフェイスガイド](ui/overview.md)』を参照してください。 外部クライアントとクエリサービスを接続する際の詳細なリストについては、 [クエリサービスクライアントの概要を参照してください](clients/overview.md)。

クエリを実行する準備を整えるために、次のビデオをご覧ください。 このビデオでは、クエリエディタインターフェイス、PSQLクライアント、ビジネスインテリジェンス(BI)ソリューション、およびHTTP APIでのクエリの実行に関するヒントとベストプラクティスを紹介します。

>[!VIDEO](https://video.tv.adobe.com/v/29811?quality=12&learn=on)
