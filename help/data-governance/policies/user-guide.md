---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ使用ポリシーユーザーガイド
topic: policies
translation-type: tm+mt
source-git-commit: 304a84b81758b2f2a343977533f9120857a1fb33

---


# データ使用ポリシーユーザーガイド

Adobe Experience Platform Data Governanceは、データ使用ポリシーを作成および管理できるユーザーインターフェイスを提供します。 このドキュメントでは、Experience Platform UIのポリシーワークスペースで実行で _きる_ 、アクションの概要を示します。

## 前提条件

このガイドでは、次のエクスペリエンスプラットフォームの概念に関する作業を理解する必要があります。

- [データガバナンス](../home.md)
- [データ使用ポリシー](./overview.md)

## 表示データ使用ポリシー

Experience Platform UIで、「ポリシー」をクリックし **[!UICONTROL て]** 、ポリシーワークスペ *[!UICONTROL ースを開きます]* 。 「参照」タ **[!UICONTROL ブには]** 、使用可能なポリシーのリスト（関連するラベル、マーケティングアクション、ステータスなど）が表示されます。

![](../images/policies/browse-policies.png)

一覧のポリシーをクリックして、その説明と表示の種類を選択します。 カスタムポリシーを選択すると、ポリシーを編集、削除、または有効/無効にするた [めの追加のコントロールが表示されます](#enable)。

![](../images/policies/policy-details.png)

## カスタムデータ使用ポリシーの作成

新しいカスタムデータ使用ポリシーを作成するには、 **[!UICONTROL ポリシー]** ( *Policies* )ワークスペースの右上隅にある「ポリシーを作成」をクリックします。

![](../images/policies/create-policy-button.png)

ポリシーの *[!UICONTROL 作成ワークフロー]* が表示されます。 開始。新しいポリシーの名前と説明を指定します。

![](../images/policies/create-policy-description.png)

次に、ポリシーの基にするデータ使用ラベルを選択します。 複数のラベルを選択する場合は、ポリシーを適用するために、データにすべてのラベルを含めるか、1つのラベルのみを含めるかを選択できます。 終了したら「**[!UICONTROL 次へ]**」をクリックします。

![](../images/policies/add-labels.png)

「マーケテ *[!UICONTROL ィングアクションの選択]* 」手順が表示されます。 表示されたアクションから適切なマーケティングリストを選択し、「 **[!UICONTROL Next]** 」をクリックして続行します。

>[!NOTE] 複数のマーケティングアクションを選択する場合、ポリシーはそれらを「OR」ルールとして解釈します。 つまり、選択したマーケティングアクションのいず _れかが_ 実行された場合に、ポリシーが適用されます。

![](../images/policies/add-marketing-actions.png)

[ *[!UICONTROL Review]* ]ステップが表示され、新しいポリシーを作成する前にその詳細を確認できます。 問題が解決したら、「完了」をクリ **[!UICONTROL ックし]** 、ポリシーを作成します。

![](../images/policies/policy-review.png)

「参照」 *[!UICONTROL タブが再び表示され]* 、新しく作成したポリシーが「ドラフト」ステータスでリストされるようになりました。 ポリシーを有効にするには、次のセクションを参照してください。

![](../images/policies/created-policy.png)

## データ使用ポリシーの有効化または無効化 {#enable}

ポリシーワークスペースの「参照」タブで、カスタムデータ使用ポリシ *[!UICONTROL ーを]* 有効または無効に *[!UICONTROL できます]* 。 右側に詳細を表示するカスタムリストを選択します。 「ステー *[!UICONTROL タス]*」で、ポリシーを有効または無効にする切り替えボタンを選択します。

![](../images/policies/enable-policy.png)

## 次の手順

このドキュメントでは、Experience Platform UIでデータ使用ポリシーを管理する方法の概要を説明しました。 DULE Policy APIを使用してポリシーを管理する手順については、 [APIポリシー作成のチュートリアルを参照してください](./create.md)。 データ使用ポリシーの適用方法について詳しくは、「ポリシーの適用の概要」 [を参照してくださ](../enforcement/overview.md)い。