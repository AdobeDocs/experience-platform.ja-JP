---
title: データガバナンスの概要
seo-title: リアルタイム顧客データプラットフォームにおけるデータガバナンス
description: 'Data Governanceを使用すると、お客様のデータを管理し、データの使用に適した規制、制限、ポリシーへのコンプライアンスを確保できます。 '
seo-description: 'Data Governanceを使用すると、お客様のデータを管理し、データの使用に適した規制、制限、ポリシーへのコンプライアンスを確保できます。 '
translation-type: tm+mt
source-git-commit: c583d0ab89dad61e00ba117f7f2e0ec5ab427745

---


# リアルタイムCDPにおけるデータ・ガバナンス

リアルタイム顧客データプラットフォーム(Real-time CDP)は、複数のエンタープライズシステムからのデータを統合し、マーケティング担当者が顧客をより良く識別、理解、関与させるためのツールです。 このデータは、組織または法規制によって定義された使用制限に従う場合があります。 したがって、リアルタイムCDPが使用ポリシーに準拠していることを確認し、データを処理することが重要です。

Adobe Experience Platform Data Governanceを使用すると、顧客データを管理し、データの使用に適用される規制、制限、ポリシーへの準拠を確実に行うことができます。 Real-time CDP内で重要な役割を果たし、使用ポリシーの定義、それらのポリシーに基づくデータの分類、特定のマーケティングアクションの実行時のポリシー違反の確認を行うことができます。

リアルタイムCDPはAdobe Experience Platformの上に構築されているので、Data Governance機能の大部分はExperience Platformのドキュメントで説明されています。 本書は、エクスペリエンスプラットフォームの [データガバナンスの概要](https://www.adobe.io/apis/experienceplatform/home/dule/duleservices.html#!api-specification/markdown/narrative/technical_overview/data_governance/dule_overview.md) (Data Governance overview for Experience Platform)を補完するものであり、リアルタイムCDPで利用可能なガバナンス機能の概要を説明しています。 以下のトピックを取り上げます。

* [使用状況ラベルをデータに適用する](#apply-usage-labels-to-your-data)
* [データ使用ポリシーの管理](#manage-data-usage-policies)
* [データ使用コンプライアンスの実施](#enforce-data-usage-compliance)

## 使用状況ラベルをデータに適用する

Data Governanceを使用すると、データセットレベルまたはデータセットフィールドレベルで、使用状況ラベルをデータに適用できます。 データ使用量ラベルを使用すると、データに適用される使用ポリシーに従ってデータを分類できます。

データ使用ラベルの操作について詳しくは、Adobe Experience Platformのデ [ータ使用ラベルユーザーガイド](https://www.adobe.io/apis/experienceplatform/home/dule/duleservices.html#!api-specification/markdown/narrative/tutorials/dule/dule_working_with_labels.md) を参照してください。

<!-- (To be included after destinations support is available -- January 2020)
## Set restrictions on destinations

You can set data usage restrictions on a destination by defining the marketing use cases for that destination. Defining use cases for destinations allows you to check for usage policy violations and ensure that any profiles or segments sent to that destination are compatible with Data Governance rules.

Marketing use cases can be defined during the _Setup_ phase for the _Edit Destination_ workflow. See the destination documentation for more information. 
-->


## データ使用ポリシーの管理

データの使用ラベルがデータのコンプライアンスを効果的にサポートするためには、データの使用ポリシーを定義し、有効にする必要があります。 データ使用ポリシーは、リアルタイムCDP内のデータに対して実行を許可（制限）するマーケティングアクションの種類を記述するルールです。 詳しくは、エクスペリエンスプラットフォームのデータガバナンスの概要の「デ [ータ使用ポリシー](https://www.adobe.io/apis/experienceplatform/home/dule/duleservices.html#!api-specification/markdown/narrative/technical_overview/data_governance/dule_overview.md) 」の節を参照してください。

Adobe Experience Platformは、一般的な顧客体験の **使用例に対して** 、いくつかのコアポリシーを提供します。 これらのポリシーは、 [Policy Service開発者ガイドの「すべてのポリシーを一覧表示」セクションに示すように、](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml)DULE Policy Service APIにリクエストを行うことで表示 [できます](https://www.adobe.io/apis/experienceplatform/home/dule/duleservices.html#!api-specification/markdown/narrative/technical_overview/data_governance/dule_policy_service_developer_guide.md)。 独自のカスタムポリシーを作成し **て** 、カスタムの使用制限をモデル化することもできます（開発者ガイドの「ポリシーの作成」セクションを参照）。

## データ使用コンプライアンスの実施

データにラベルが付けられ、使用ポリシーが定義されたら、データ使用に対するポリシーのコンプライアンスを適用できます。 詳しくは、オーディエンスセグメ [ントのデータ使用に対する準拠の適用に関するチュートリアル](https://www.adobe.io/apis/experienceplatform/home/tutorials/alltutorials.html#!api-specification/markdown/narrative/tutorials/dule/data_governance_and_segmentation.md) を参照してください。

## 次の手順

リアルタイムCDPの主要なデータガバナンス機能と、Experience Platformによる機能の紹介が完了したので、Adobe Experience Platformのデータガバナンスに関するドキュメン [トを引き続きご覧ください](https://www.adobe.io/apis/experienceplatform/home/dule/duleservices.html)。 このドキュメントには、データガバナンスの基本概念の概要と、データ使用ラベルとポリシーを管理するためのワークフローが順を追って記載されています。