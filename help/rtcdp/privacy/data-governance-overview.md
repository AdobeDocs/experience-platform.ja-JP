---
keywords: データガバナンス rtcdp、rtcdp データガバナンス、リアルタイム顧客データプロファイルデータガバナンス
title: データガバナンスの概要
seo-title: Data Governance in Real-time Customer Data Platform
description: 'データガバナンスを使用すると、顧客データを管理し、データの使用に適した規制、制限、ポリシーへのコンプライアンスを確保できます。 '
seo-description: Data Governance allows you to manage customer data and ensure compliance with regulations, restrictions, and policies applicable to data use.
exl-id: eb501d85-cabd-4667-a1cd-2210ec83fb71
source-git-commit: 0085306a2f5172eb19590cc12bc9645278bd2b42
workflow-type: tm+mt
source-wordcount: '674'
ht-degree: 43%

---

# [!DNL Data Governance] リアルタイム CDP の

[!DNL Real-time Customer Data Platform] （リアルタイム CDP）は、複数の企業システムからのデータを統合し、マーケターが顧客をより深く識別、理解し、惹きつけられるようにします。このデータは、組織または法規制によって定義された使用制限の対象となる場合があります。したがって、リアルタイム CDP が使用ポリシーに準拠していることを確認し、データを処理することが重要です。

Adobe Experience Platform [!DNL Data Governance]を使用すると、顧客データを管理し、データの使用に適用される規制、制限、ポリシーへのコンプライアンスを確保できます。データガバナンスは リアルタイム CDP 内で重要な役割を果たし、使用ポリシーの定義、それらのポリシーに基づくデータの分類、特定のマーケティングアクションの実行時のポリシー違反を確認できるようになります。

リアルタイム CDP はAdobe Experience Platformをベースに構築されているので、[!DNL Data Governance] 機能の大部分は [!DNL Experience Platform] のドキュメントで説明されています。 本書は、 の『[データガバナンスの概要](../../data-governance/home.md)』を補完するものであり、リアルタイム CDP で利用可能なガバナンス機能の概要を説明しています。[!DNL Experience Platform]以下のトピックを取り上げます。

* [データへの使用状況ラベルの適用](#labels)
* [データ使用ポリシーの管理](#policies)
* [データ使用コンプライアンスの実施](#enforce)

## データへの使用状況ラベルの適用 {#labels}

[!DNL Data Governance] では、データセットレベルまたはデータセットフィールドレベルで、使用状況ラベルをデータに適用できます。データ使用状況ラベルを使用すると、データに適用される使用ポリシーに従ってデータを分類できます。

データ使用状況ラベルの使用について詳しくは、Adobe Experience Platform の『[データ使用ラベルユーザーガイド](../../data-governance/labels/overview.md)』を参照してください。

## 宛先のマーケティングアクションの設定 {#destinations}

宛先のマーケティングアクション（マーケティングユースケースとも呼ばれます）を定義することで、その宛先に対するデータの使用制限を設定できます。 宛先のマーケティングアクションは、その宛先に書き出されるデータの意図を示します。

>[!NOTE]
>
>マーケティングアクションとデータ使用ポリシーでの使用について詳しくは、[!DNL Experience Platform] ドキュメントの [ データ使用ポリシーの概要 ](../../data-governance/policies/overview.md) を参照してください。

宛先に対するマーケティングアクションを定義すると、それらの宛先に送信されたプロファイルやセグメントがデータ使用ポリシーに準拠していることを確認できます。 したがって、組織のアクティベーションに対するポリシー制限を実施する必要に応じて、適切なマーケティングアクションを宛先に追加する必要があります。

マーケティングアクションは、宛先を初めて設定する場合にのみ選択できます。 使用する宛先のタイプに応じて、マーケティングアクションを設定する機会は、セットアップワークフローの様々なポイントに表示されます。 特定の宛先を設定する手順については、[ 宛先のドキュメント ](../destinations/overview.md) を参照してください。

## データ使用ポリシーの管理 {#policies}

データ使用状況ラベルがデータのコンプライアンスを効果的にサポートするためには、データ使用ポリシーを定義し、有効にする必要があります。データ使用ポリシーは、リアルタイム CDP 内のデータに対して実行を許可（／制限）するマーケティングアクションの種類を記述するルールです詳しくは、 で『[!DNL Experience Platform][データガバナンスの概要](../../data-governance/home.md)』の「データ使用ポリシー」の節を参照してください。

Adobe Experience Platform では、一般的な顧客体験の使用例に対して、いくつかのコアポリシーがあります。これらのポリシーは、UI で表示できます。それには、**[!UICONTROL ポリシー]** ワークスペースに移動し、「**[!UICONTROL 参照]**」タブを選択します。 独自のカスタムポリシーを作成する方法など、UI でポリシーを操作する詳細な手順については、[!DNL Experience Platform] ドキュメントの [ ポリシーユーザーガイド ](../../data-governance/policies/user-guide.md) を参照してください。

## データ使用コンプライアンスの実施 {#enforce}

データにラベルが付けられ、使用ポリシーが定義されたら、データ使用に対するポリシーのコンプライアンスを適用できます。リアルタイム CDP で宛先に対するオーディエンスセグメントをアクティブ化すると、[!DNL Data Governance] は、違反が発生した場合に使用ポリシーを自動的に適用します。

詳しくは、[ 自動ポリシー適用 ](../../data-governance/enforcement/auto-enforcement.md) のドキュメントを参照してください。

## 次の手順

リアルタイム CDP の主要な [!DNL Data Governance] 機能と、[!DNL Experience Platform] 機能を使用したので、Adobe Experience Platform](../../data-governance/home.md) のデータガバナンスに関する [ ドキュメントを引き続きご覧ください。 このドキュメントでは、基本的な [!DNL Data Governance] 概念の概要と、データ使用ラベルおよびポリシーを管理するためのワークフローを順を追って説明します。

次のビデオでは、宛先でのマーケティングの使用例の使用や、様々なシナリオでのワークフローの例など、リアルタイム CDP の [!DNL Data Governance] の概要を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/33631?quality=12&learn=on)
