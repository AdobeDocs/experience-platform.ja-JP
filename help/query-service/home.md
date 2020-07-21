---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platformクエリサービス
topic: overview
translation-type: tm+mt
source-git-commit: 3b710e7a20975880376f7e434ea4d79c01fa0ce5
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---


# [!DNL Query Service]概要

Adobe Experience Platformは様々なソースからデータを取り込みます。 マーケターにとっての大きな課題は、顧客に関するインサイトを得るために、このデータを理解することです。 Adobe Experience Platform [!DNL Query Service] は、でのクエリデータに標準のSQLを使用できるようにすることで、それを容易に [!DNL Platform]します。 を使用 [!DNL Query Service]すると、任意のデータセットをに組み込んで、レポート、機械学習、またはに取り込むための新しいクエリセットとして結果を取り込むことができ [!DNL Data Lake][!DNL Real-time Customer Profile]ます。 このドキュメントでは、の役割の概要を説明 [!DNL Query Service] し [!DNL Experience Platform]ます。

[!DNL Query Service] ブランドがオンラインからオフラインへの顧客の遍歴を結びつけ、オムニチャネルアトリビューションを理解できるようになります。 次のビデオでは、エクスペリエンスビジネスが主要な使用例 [!DNL Query Service] にどのように対処し、どのように [!DNL Query Service] 機能するかを示します。

>[!VIDEO](https://video.tv.adobe.com/v/29795?quality=12&learn=on)

## 使用 [!DNL Query Service]

[!DNL Query Service] には、データをより良く分析するためのSQLクエリを作成できるユーザーインターフェイスとRESTful APIが用意されています。 ユーザーインターフェイスを使用して、クエリの書き込みと実行、表示が以前に実行したクエリの作成と実行、およびIMS組織内のユーザーによって保存されたクエリへのアクセスを行うことができます。 ユーザーインターフェイスは、より広いデータセットでクエリを実行する前にユーザーをテストするサンドボックスとして使用することを目的としています。 内でのインタラクティブサービスの使用について詳し [!DNL Platform] くは、『 [クエリサービスユーザーインターフェイスガイド](ui/overview.md)』を参照してください。 RESTful APIも同様の経験を提供し、クエリの書き込みと実行、将来の使用と繰り返しに関するクエリのスケジュール設定、および書き込むクエリ用のテンプレートの作成をプログラムによって行うことができます。 この [!DNL Query Service] APIの使用について詳しくは、 [クエリサービス開発者ガイドを参照してください](api/getting-started.md)。

## [!DNL Query Service] および [!DNL Experience Platform] サービス

[!DNL Query Service] が相互に作用し、複数のサー [!DNL Experience Platform] ビスと組み合わせて使用できます。 機能を最大限に活用するために、これらのサービスとのやり取りについて理解することをお勧めし [!DNL Query Service's][!DNL Query Service]ます。

### [!DNL Data Science Workspace]

Adobe Experience Platform [!DNL Data Science Workspace] は、機械学習と人工知能を使用して、に保存されたデータからの洞察を得 [!DNL Experience Platform]ます。 [!DNL Data Science Workspace] データ科学者は、顧客とそのアクティビティに関する記録と時系列のデータに基づいてレシピを作成でき、購入傾向や、個人が認識し、使用する可能性が高い推奨オファーなどの予測を容易にできます。 に統合するこ [!DNL Data Science Workspace] とで、内でSQLを使用 [!DNL Query Service] でき [!DNL JupyterLab]ます。これにより、アドビのAnalyticsデータを調査、変換、分析できます。 との対話方法の詳細については、 [!DNL Data Science Workspace] 概要をお読みください。詳しく [!DNL Data Science Workspace]は、 [!DNL Query Service] 統合ガイドを参照し [!DNL Data Science Workspace] てくだ [!DNL Query Service]さい。

### [!DNL Segmentation Service]

Adobe Experience Platform [!DNL Segmentation Service] により、同様の特性を共有する小さなグループに顧客を分割できます。 その後、これらのセグメントを評価して、 [!DNL Real-time Customer Profile] データの分析を改善できます。 [!DNL Query Service] を使用して、内のこのセグメントデータに対してクエリを実行することで、この分析を提供でき [!DNL Data Lake]ます。 セグメント化の詳細については、 [!DNL Segmentation Service] 概要をお読みください。セグメントの分析方法の詳細については、 [!DNL Profile Query Language] (PQL)ガイドを参照してください。

### Looker BIの使用例

Adobe Experience Platformにより、行動、CRM、POS（販売時点管理）データを含むすべての保存済みデータセットを取り込み、保存、構造化、取り込みできます。 を使用 [!DNL Experience Platform's Query Service]すると、これらのデータセットにクエリを付け、ビジネスに関する特定の質問に答え、開始がインパクトなインサイトを生み出すことができます。 次のビデオでは、を使用してビジネスインテリジェンス(BI)ツールでダッシュボードを作成する価値を示し [!DNL Query Service]ます。

>[!VIDEO](https://video.tv.adobe.com/v/28981?quality=12&learn=on)

## 次の手順とその他のリソース

このドキュメントを読むことで、様々な範囲での機能 [!DNL Query Service] と機能についての説明を受け [!DNL Experience Platform]ました。 API内の様々なエンドポイントとの対話について詳しくは、『 [!DNL Query Service] クエリサービス開発者ガイド [](api/getting-started.md)』を参照してください。 内でのインタラクティブサービスの使用について詳し [!DNL Platform]くは、『 [クエリサービスユーザーインターフェイスガイド](ui/overview.md)』を参照してください。 外部クライアントとの接続に関する包括的なリストにつ [!DNL Query Service]いては、 [クエリサービスクライアントの概要を参照してください](clients/overview.md)。

クエリを実行する準備を整えるために、次のビデオをご覧ください。 このビデオでは、クエリエディタインターフェイス、PSQLクライアント、ビジネスインテリジェンス(BI)ソリューション、およびHTTP APIでのクエリの実行に関するヒントとベストプラクティスを紹介します。

>[!VIDEO](https://video.tv.adobe.com/v/29811?quality=12&learn=on)
