---
title: データガバナンスの概要
seo-title: リアルタイム顧客データプラットフォームにおけるデータガバナンス
description: 'データガバナンスを使用すると、顧客データを管理し、データの使用に適した規制、制限、ポリシーへのコンプライアンスを確保できます。 '
seo-description: 'データガバナンスを使用すると、顧客データを管理し、データの使用に適した規制、制限、ポリシーへのコンプライアンスを確保できます。 '
translation-type: tm+mt
source-git-commit: f5fbb1434b7154dcdbef12de7882ecd3d2f18d52

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

## 宛先に対する制限の設定

宛先のマーケティングの使用例を定義することで、その宛先に対するデータの使用制限を設定できます。 宛先の使用例を定義すると、使用ポリシー違反を確認し、その宛先に送信されるプロファイルまたはセグメントがData Governanceルールと互換性があることを確認できます。

マーケティングの使用例は、宛先の編集ワークフ _ローの_ 「設定」段階で _定義できます_ 。 詳しくは、宛先のドキュメントを参照してください。


## データ使用ポリシーの管理

データ使用状況ラベルがデータのコンプライアンスを効果的にサポートするためには、データ使用ポリシーを定義し、有効にする必要があります。データ使用ポリシーは、Real-time CDP 内のデータに対して実行を許可（／制限）するマーケティングアクションの種類を記述するルールです詳しくは、Experience Platform で『[データガバナンスの概要](https://www.adobe.io/apis/experienceplatform/home/dule/duleservices.html#!api-specification/markdown/narrative/technical_overview/data_governance/dule_overview.md)』の「データ使用ポリシー」の節を参照してください。

Adobe Experience Platform では、一般的な顧客体験の使用例に対して、いくつかの&#x200B;**コアポリシー**&#x200B;があります。これらのポリシーは、[DULE Policy Service API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml) に要求ことで表示できます（『[ポリシーサービス開発者ガイド](https://www.adobe.io/apis/experienceplatform/home/dule/duleservices.html#!api-specification/markdown/narrative/technical_overview/data_governance/dule_policy_service_developer_guide.md)』の「すべてのポリシーの一覧表示」節参照）。独自の&#x200B;**カスタムポリシー**&#x200B;を作成して、カスタムの使用制限をモデル化することもできます（開発者ガイドの「ポリシーの作成」節参照）。

## (Beta) Enforce data usage compliance {#enforce-data-usage-compliance}

>[!IMPORTANT]
>この機能は現在ベータ版で、一部のユーザーはご利用いただけません。 リクエストに応じて有効にできます。 ドキュメントと機能は変更される場合があります。

データにラベルが付けられ、使用ポリシーが定義されたら、データ使用に対するポリシーのコンプライアンスを適用できます。リアルタイムCDPで宛先に対するオーディエンスセグメントをアクティブ化する場合、Data Governanceは、違反が発生した場合に使用ポリシーを自動的に適用します。

次の図は、ポリシーの実施が、セグメントのアクティブ化のデータフローにどのように統合されるかを示しています。

![](assets/enforcement-flow.png)

セグメントが最初にアクティブ化されると、DULE Policy Serviceは、次の要因に基づいてポリシー違反をチェックします。

* アクティブ化するセグメント内のフィールドおよびデータセットに適用されるデータ使用ラベル。
* 宛先のマーケティングの目的。

### ポリシー違反メッセージ

ポリシー違反が発生して、セグメントをアクティブ化(または既にアクティブ化されたセグメントを編集 [](#policy-enforcement-for-activated-segments))しようとした場合、アクションは実行されず、1つ以上のポリシーに違反したことを示すポッパーが表示されます。 ポリシー違反の詳細を表示するには、その違反の左側の列でポリシー違反を選択します。

![](assets/violation-popover.png)

「プロバーの詳細」タ *ブには* 、違反をトリガーしたアクションと、違反が発生した理由が示され、問題を解決する方法に関する提案が表示されます。

「 **Data Lineage** 」をクリックして、違反をトリガーしたデータラベルを持つ宛先、セグメント、マージポリシーまたはデータセットを追跡します。

![](assets/data-lineage.png)

違反がトリガーされると、「 **Save** 」ボタンは、データ使用ポリシーに従って適切なコンポーネントが更新されるまでアクティブ化に対して無効になります。

### アクティブ化されたセグメントのポリシーの適用

ポリシーの強制は、アクティブ化された後もセグメントに適用され、ポリシー違反の原因となるセグメントまたは宛先に対する変更が制限されます。 宛先へのセグメントのアクティブ化には多くのコンポーネントが関与しているので、次のいずれかの操作を行うと違反が発生する可能性があります。

* データ使用ラベルの更新
* セグメントのデータセットの変更
* セグメント述語の変更
* 宛先設定の変更

上記のアクションのいずれかが違反をトリガーした場合、そのアクションは保存されず、ポリシー違反のメッセージが表示され、アクティブ化されたセグメントが変更時にデータ使用ポリシーに従い続けることを確認します。

## 次の手順

Real-time CDP の主要なデータガバナンス機能と、Experience Platform による機能の紹介が完了したので、[Adobe Experience Platform のデータガバナンスに関するドキュメント](https://www.adobe.io/apis/experienceplatform/home/dule/duleservices.html)を引き続きご覧ください。このドキュメントには、データガバナンスの基本概念の概要と、データ使用ラベルとポリシーを管理するためのワークフローが順を追って記載されています。