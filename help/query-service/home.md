---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platformクエリサービス
topic: overview
translation-type: tm+mt
source-git-commit: 45da024d45b5eebdfc393ee14890e24aed6021ce

---


# クエリサービスの概要

Adobe Experience Platformは、様々なソースからデータを取り込みます。 マーケターにとっての大きな課題は、顧客に関する洞察を得るために、このデータを理解することです。 Adobe Experience Platformクエリサービスを使用すると、標準のSQLからPlatformのデータをクエリできます。 クエリサービスを使用すると、Data Lake内の任意のデータセットに加入し、クエリ結果を新しいデータセットとして取り込み、レポート、機械学習、リアルタイム顧客プロファイルへの取り込みに使用できます。 このドキュメントでは、Experience Platform内でのクエリサービスの役割の概要を説明します。

## クエリサービス

クエリサービスは、データをより良く分析するためのSQLクエリを作成できるユーザーインターフェイスとRESTful APIを提供します。 ユーザーインターフェイスを使用して、クエリの書き込みと実行、表示以前に実行されたクエリ、IMS組織内のユーザーによって保存されたクエリへのアクセスを行うことができます。 ユーザーインターフェイスは、広範なデータセットで実行する前にクエリをテストするサンドボックスとして使用されます。 プラットフォーム内でのインタラクティブサービスの使用に関する詳細は、『 [クエリサービスユーザーインターフェイス](ui/overview.md)』を参照。 RESTful APIも同様のエクスペリエンスを提供し、クエリの書き込みと実行、将来の使用と繰り返しに備えたクエリのスケジュール、および書き込むクエリのテンプレートの作成をプログラムで行うことができます。 クエリサービスAPIの使用に関する詳細は、 [クエリサービス開発ガイドを参照してください](api/getting-started.md)。

## クエリサービスとExperience Platformサービス

クエリサービスは、複数のExperience Platformサービスと相互にやり取りし、組み合わせて使用できます。 クエリサービスの機能を最大限に活用するために、これらのサービスとクエリサービスとのやり取りについて理解することをお勧めします。

### Data Science Workspace

Adobe Experience Platform Data Science Workspaceは、機械学習と人工知能を使用して、Experience Platform内に保存されたデータから洞察を得ます。 Data Science Workspaceを使用すると、データ科学者は顧客とそのアクティビティに関する記録と時系列のデータに基づいてレシピを作成でき、購入傾向や、個人が評価し、使用する可能性の高い推奨オファーなどの予測が容易になります。 クエリサービスをJupyterLabに統合することで、Data Science Workspace内でSQLを使用できます。これにより、Adobe Analyticsデータの調査、変換、分析を行うことができます。 Data Science Workspaceとクエリサービスとの相互作用の詳細については、Data Science Workspaceの概要およびクエリサービス統合ガイドをお読みください。

### Segmentation Service

Adobe Experience Platform Segmentation Serviceを使用すると、ユーザーは類似の特性を共有する小さなグループに顧客を分割できます。 これらのセグメントは、その後評価され、リアルタイム顧客分析データのプロファイルが向上します。 クエリサービスを使用して、この分析を提供できます。これは、データレーク内でこのセグメントデータに対してクエリを実行することによって行います。 セグメント化の詳細については、Segmentation Serviceの概要およびセグメントの分析方法の詳細については、プロファイルクエリ言語(PQL)ガイドをお読みください。

## 次の手順

このドキュメントを読むことで、クエリサービスと、エクスペリエンスプラットフォームの範囲内でのその機能を紹介します。 クエリサービスAPI内の様々なエンドポイントとの対話について詳しくは、 [クエリサービス開発ガイドを参照してください](api/getting-started.md)。 プラットフォーム内でのインタラクティブサービスの使用に関する詳細は、『 [クエリサービスユーザーインターフェイスガイド](ui/overview.md)』を参照。 外部クライアントとクエリサービスの接続に関する包括的なリストについては、 [クエリサービスクライアントの概要を参照してくださ](clients/overview.md)い。
