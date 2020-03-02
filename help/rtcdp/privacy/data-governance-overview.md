---
title: データガバナンスの概要
seo-title: リアルタイム顧客データプラットフォームにおけるデータガバナンス
description: 'データガバナンスを使用すると、顧客データを管理し、データの使用に適した規制、制限、ポリシーへのコンプライアンスを確保できます。 '
seo-description: 'データガバナンスを使用すると、顧客データを管理し、データの使用に適した規制、制限、ポリシーへのコンプライアンスを確保できます。 '
translation-type: ht
source-git-commit: c583d0ab89dad61e00ba117f7f2e0ec5ab427745

---


# Real-time CDP におけるデータガバナンス

リアルタイム顧客データプラットフォーム（Real-time CDP）を使用すると、マーケティング担当者は複数の大規模法人システムからデータを統合し、顧客の特定、理解、関与を促進できます。このデータは、組織または法規制によって定義された使用制限の対象となる場合があります。したがって、Real-time CDP が使用ポリシーに準拠していることを確認し、データを処理することが重要です。

Adobe Experience Platform データガバナンスを使用すると、顧客データを管理し、データの使用に適した規制、制限、ポリシーへのコンプライアンスを確保できます。データガバナンスは Real-time CDP 内で重要な役割を果たし、使用ポリシーの定義、それらのポリシーに基づくデータの分類、特定のマーケティングアクションの実行時のポリシー違反の確認をおこなうことができるようになります。

Real-time CDP は Adobe Experience Platform をベースに構築されているので、データガバナンス機能の大部分は Experience Platform のドキュメントで説明されています。本書は、Experience Platform の『[データガバナンスの概要](https://www.adobe.io/apis/experienceplatform/home/dule/duleservices.html#!api-specification/markdown/narrative/technical_overview/data_governance/dule_overview.md)』を補完するものであり、Real-time CDP で利用可能なガバナンス機能の概要を説明しています。以下のトピックを取り上げます。

* [データへの使用状況ラベルの適用](#apply-usage-labels-to-your-data)
* [データ使用ポリシーの管理](#manage-data-usage-policies)
* [データ使用コンプライアンスの実施](#enforce-data-usage-compliance)

## データへの使用状況ラベルの適用

データガバナンスを使用すると、データセットレベルまたはデータセットフィールドレベルで、使用状況ラベルをデータに適用できます。データ使用状況ラベルを使用すると、データに適用される使用ポリシーに従ってデータを分類できます。

データ使用状況ラベルの使用について詳しくは、Adobe Experience Platform の『[データ使用ラベルユーザーガイド](https://www.adobe.io/apis/experienceplatform/home/dule/duleservices.html#!api-specification/markdown/narrative/tutorials/dule/dule_working_with_labels.md)』を参照してください。

<!-- (To be included after destinations support is available -- January 2020)
## Set restrictions on destinations

You can set data usage restrictions on a destination by defining the marketing use cases for that destination. Defining use cases for destinations allows you to check for usage policy violations and ensure that any profiles or segments sent to that destination are compatible with Data Governance rules.

Marketing use cases can be defined during the _Setup_ phase for the _Edit Destination_ workflow. See the destination documentation for more information. 
-->


## データ使用ポリシーの管理

データ使用状況ラベルがデータのコンプライアンスを効果的にサポートするためには、データ使用ポリシーを定義し、有効にする必要があります。データ使用ポリシーは、Real-time CDP 内のデータに対して実行を許可（／制限）するマーケティングアクションの種類を記述するルールです詳しくは、Experience Platform で『[データガバナンスの概要](https://www.adobe.io/apis/experienceplatform/home/dule/duleservices.html#!api-specification/markdown/narrative/technical_overview/data_governance/dule_overview.md)』の「データ使用ポリシー」の節を参照してください。

Adobe Experience Platform では、一般的な顧客体験の使用例に対して、いくつかの&#x200B;**コアポリシー**&#x200B;があります。これらのポリシーは、[DULE Policy Service API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml) に要求ことで表示できます（『[ポリシーサービス開発者ガイド](https://www.adobe.io/apis/experienceplatform/home/dule/duleservices.html#!api-specification/markdown/narrative/technical_overview/data_governance/dule_policy_service_developer_guide.md)』の「すべてのポリシーの一覧表示」節参照）。独自の&#x200B;**カスタムポリシー**&#x200B;を作成して、カスタムの使用制限をモデル化することもできます（開発者ガイドの「ポリシーの作成」節参照）。

## データ使用コンプライアンスの実施

データにラベルが付けられ、使用ポリシーが定義されたら、データ使用に対するポリシーのコンプライアンスを適用できます。詳しくは、[オーディエンスセグメントのデータ使用に対する準拠の適用](https://www.adobe.io/apis/experienceplatform/home/tutorials/alltutorials.html#!api-specification/markdown/narrative/tutorials/dule/data_governance_and_segmentation.md)に関するチュートリアルを参照してください。

## 次の手順

Real-time CDP の主要なデータガバナンス機能と、Experience Platform による機能の紹介が完了したので、[Adobe Experience Platform のデータガバナンスに関するドキュメント](https://www.adobe.io/apis/experienceplatform/home/dule/duleservices.html)を引き続きご覧ください。このドキュメントには、データガバナンスの基本概念の概要と、データ使用ラベルとポリシーを管理するためのワークフローが順を追って記載されています。