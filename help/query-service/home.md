---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platformクエリサービス
topic: overview
translation-type: tm+mt
source-git-commit: 45da024d45b5eebdfc393ee14890e24aed6021ce
workflow-type: tm+mt
source-wordcount: '572'
ht-degree: 0%

---


# クエリサービスの概要

Adobe Experience Platformは、様々なソースからデータを取り込みます。 マーケターにとっての大きな課題は、顧客に関するインサイトを得るために、このデータを理解することです。 Adobe Experience Platformクエリサービスは、標準のSQLを使用してPlatformでクエリデータを利用できるようにします。 クエリサービスを使用すると、Data Lakeの任意のデータセットに参加し、クエリ結果を新しいデータセットとして取り込み、レポート、機械学習、リアルタイム顧客プロファイルへの取り込みに使用できます。 このドキュメントでは、エクスペリエンスプラットフォーム内でのクエリサービスの役割の概要を説明します。

## クエリサービスの使用

クエリサービスは、SQLクエリを作成してデータをより良く分析できるユーザーインターフェイスとRESTful APIを提供します。 ユーザーインターフェイスを使用して、クエリの書き込みと実行、表示が以前に実行したクエリの作成と実行、およびIMS組織内のユーザーによって保存されたクエリへのアクセスを行うことができます。 ユーザーインターフェイスは、より広いデータセットでクエリを実行する前にユーザーをテストするサンドボックスとして使用することを目的としています。 Platform内でのインタラクティブサービスの使用について詳しくは、『 [クエリサービスユーザーインターフェイスガイド](ui/overview.md)』を参照してください。 RESTful APIも同様の経験を提供し、クエリの書き込みと実行、将来の使用と繰り返しに関するクエリのスケジュール設定、および書き込むクエリ用のテンプレートの作成をプログラムによって行うことができます。 クエリサービスAPIの使用について詳しくは、 [クエリサービス開発ガイドを参照してください](api/getting-started.md)。

## クエリサービスとExperience Platformサービス

クエリサービスは、相互にやり取りし、複数のExperience Platformサービスと組み合わせて使用できます。 クエリサービスの機能を最大限に活用するために、これらのサービスとクエリサービスとのやり取りについて理解することをお勧めします。

### Data Science Workspace

Adobe Experience Platform Data Science Workspaceは、機械学習と人工知能を使用して、Experience Platformに保存されたデータからのインサイトを得ます。 Data Science Workspaceを使用すると、データ科学者は顧客とそのアクティビティに関する記録と時系列のデータに基づいてレシピを作成でき、購入傾向や推奨オファーなど、個人が認識し、使用する可能性が高い予測を容易にできます。 クエリサービスをJupyterLabに統合することで、Data Science Workspace内でSQLを使用できます。これにより、Adobe Analyticsデータを調査、変換、分析できます。 Data Science Workspaceの詳細については、「Data Science Workspaceの概要」をお読みください。Data Science Workspaceがクエリサービスと相互作用する方法の詳細については、『クエリサービス統合ガイド』を参照してください。

### Segmentation Service

Adobe Experience Platform Segmentation Serviceを使用すると、ユーザーは類似の特性を共有する小さなグループに顧客を分割できます。 その後、これらのセグメントを評価して、リアルタイム顧客プロファイルデータの分析を改善できます。 クエリサービスを使用してこの分析を提供するには、データレーク内でこのセグメントデータに対してクエリを実行します。 セグメント化の詳細については、Segmentation Serviceの概要を参照し、セグメントの分析方法の詳細については、『プロファイルクエリ言語(PQL)ガイド』を参照してください。

## 次の手順

このドキュメントを読むことで、クエリサービスと、より広範なエクスペリエンスプラットフォーム内でのその機能についてご紹介します。 クエリサービスAPI内の様々なエンドポイントとの対話について詳しくは、 [クエリサービス開発ガイドを参照してください](api/getting-started.md)。 Platform内でのインタラクティブサービスの使用について詳しくは、『 [クエリサービスユーザーインターフェイスガイド](ui/overview.md)』を参照してください。 外部クライアントとクエリサービスを接続する際の詳細なリストについては、 [クエリサービスクライアントの概要を参照してください](clients/overview.md)。
