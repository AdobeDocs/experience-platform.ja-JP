---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；クエリ
solution: Experience Platform
title: クエリサービスの概要
topic-legacy: overview
description: このドキュメントでは、Experience Platform 内でのクエリサービスの役割を概説します。
exl-id: fdaefc12-a97d-4e4e-9aed-d3dbd0f43ea0
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '635'
ht-degree: 25%

---

# [!DNL Query Service] の概要

Adobe Experience Platform は様々なソースからデータを取得します。マーケターにとっての主要な課題は、このデータの意味を理解して、顧客に関するインサイトを得ることです。Adobe Experience Platform[!DNL Query Service]は、[!DNL Platform]のクエリデータに標準のSQLを使えるようにすることで、それを容易にします。 [!DNL Query Service]を使用すると、[!DNL Data Lake]内の任意のデータセットに参加し、クエリ結果を新しいデータセットとして取り込み、レポート、機械学習、または[!DNL Real-time Customer Profile]への取り込みに使用できます。 このドキュメントでは、[!DNL Experience Platform]内の[!DNL Query Service]の役割の概要を説明します。

[!DNL Query Service] ブランドがオンラインからオフラインへの顧客ジャーニーを結び付け、オムニチャネルアトリビューションを理解できるようになります。次のビデオでは、エクスペリエンスビジネスが[!DNL Query Service]を活用して主要な使用例や[!DNL Query Service]の仕組みに対処する方法を示します。

>[!VIDEO](https://video.tv.adobe.com/v/29795?quality=12&learn=on)

## 使用 [!DNL Query Service]

[!DNL Query Service] には、データをより良く分析するためのSQLクエリを作成できるユーザーインターフェイスとRESTful APIが用意されています。ユーザーインターフェイスを使用すると、クエリの記述と実行、以前に実行されたクエリの表示、IMS 組織内のユーザーによって保存されたクエリへのアクセスをおこなうことができます。ユーザーインターフェイスは、より広範なデータセットに対してクエリを実行する前に、クエリをテストするためのサンドボックスとして使用されます。[!DNL Platform]内でのインタラクティブサービスの使用について詳しくは、[クエリサービスユーザーインターフェイスガイド](ui/overview.md)を参照してください。 RESTful API も同様のエクスペリエンスを提供し、クエリの記述と実行、将来の使用と繰り返しに備えたクエリのスケジュール、記述するクエリのテンプレートの作成をプログラムで行えるようにします。[!DNL Query Service] APIの使用について詳しくは、[クエリサービス開発者ガイド](api/getting-started.md)を参照してください。

## [!DNL Query Service] および [!DNL Experience Platform] サービス

[!DNL Query Service] が相互に作用し、複数の [!DNL Experience Platform] サービスと組み合わせて使用できます。[!DNL Query Service's]の機能を最大限に活用するために、これらのサービスと[!DNL Query Service]とのやり取り方を理解することをお勧めします。

### [!DNL Data Science Workspace]

Adobe Experience Platform[!DNL Data Science Workspace]は、機械学習と人工知能を使って[!DNL Experience Platform]に保存されたデータから洞察を得ています。 [!DNL Data Science Workspace] を使用すると、データサイエンティストは、顧客とそのアクティビティに関するレコードと時系列データに基づいてレシピを作成できるため、購入傾向やユーザーが評価して使用する可能性の高い推奨オファーなどの予測が容易になります。[!DNL Query Service]を[!DNL JupyterLab]に統合することで、[!DNL Data Science Workspace]内でSQLを使用できます。これにより、Adobe Analyticsデータの調査、変換、分析が可能になります。 [!DNL Data Science Workspace]の詳細については[!DNL Data Science Workspace]の概要を、[!DNL Data Science Workspace]と[!DNL Query Service]との相互作用については[!DNL Query Service]統合ガイドをお読みください。

### [!DNL Segmentation Service]

Adobe Experience Platform[!DNL Segmentation Service]は、同様の特徴を持つ小さなグループに顧客を分けることをユーザーに許可します。 その後、これらのセグメントを評価して、[!DNL Real-time Customer Profile]データの分析を改善できます。 [!DNL Query Service] を使用して、内のこのセグメントデータに対してクエリを実行することで、この分析を提供でき [!DNL Data Lake]ます。セグメント化の詳細については、[!DNL Segmentation Service]の概要をお読みください。セグメントの分析方法の詳細については、[!DNL Profile Query Language] (PQL)のガイドを参照してください。

### Looker BIの使用例

Adobe Experience Platformでは、行動、CRM、POS（販売時点情報）データを含むすべての保存済みデータセットを取り込み、保存、構造化、取り込みできます。 [!DNL Experience Platform's Query Service]を使用すると、これらのデータセットにクエリし、ビジネスに関する特定の質問に答え、開始がインパクトのあるインサイトを生成できます。 次のビデオでは、[!DNL Query Service]を使用してビジネスインテリジェンス(BI)ツールでダッシュボードを作成する価値を示します。

>[!VIDEO](https://video.tv.adobe.com/v/28981?quality=12&learn=on)

## 次の手順とその他のリソース

このドキュメントを読むことで、[!DNL Query Service]に紹介され、[!DNL Experience Platform]の範囲内での機能がどのように機能するかを知ることができます。 [!DNL Query Service] API内の様々なエンドポイントとの対話について詳しくは、[クエリサービス開発者ガイド](api/getting-started.md)をお読みください。 [!DNL Platform]内でのインタラクティブサービスの使用について詳しくは、[クエリサービスユーザーインターフェイスガイド](ui/overview.md)を参照してください。 外部クライアントと[!DNL Query Service]の接続に関する包括的なリストについては、[クエリサービスクライアントの概要](clients/overview.md)を参照してください。

クエリを実行する準備を整えるために、次のビデオをご覧ください。 このビデオでは、クエリエディタインターフェイス、PSQLクライアント、ビジネスインテリジェンス(BI)ソリューション、およびHTTP APIでのクエリの実行に関するヒントとベストプラクティスを紹介します。

>[!VIDEO](https://video.tv.adobe.com/v/29811?quality=12&learn=on)
