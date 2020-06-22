---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Data usage policiesユーザーガイド
topic: policies
translation-type: tm+mt
source-git-commit: c4554e3fbc0dd527606b81e2767cb5777b6e81e7
workflow-type: tm+mt
source-wordcount: '739'
ht-degree: 1%

---


# Data usage policiesユーザーガイド

Adobe Experience Platformデータガバナンスは、データ使用ポリシーの作成と管理を可能にするユーザーインターフェイスを提供します。 このドキュメントでは、Experience Platformユーザーインターフェイスの _ポリシー_ ワークスペースで実行できるアクションの概要を説明します。

>[!IMPORTANT] すべてのデータ使用ポリシー（アドビが提供するコアポリシーを含む）は、デフォルトで無効になっています。 個々のポリシーを適用対象と見なすには、そのポリシーを手動で有効にする必要があります。 UIでこれを行う方法については、ポリシー [の有効化の節を参照してください](#enable) 。

## 前提条件

このガイドでは、以下のExperience Platformの概念について、十分に理解している必要があります。

- [データガバナンス](../home.md)
- [データ使用ポリシー](./overview.md)

## 表示データ使用ポリシー {#view-policies}

Experience PlatformUIで、「 **[!UICONTROL ポリシー]** 」をクリックして、 *[!UICONTROL ポリシー]* ワークスペースを開きます。 「 **[!UICONTROL 参照]** 」タブには、関連付けられたラベル、マーケティングアクション、ステータスなど、使用可能なポリシーのリストが表示されます。

![](../images/policies/browse-policies.png)

表示されたポリシーをクリックして、その説明とタイプを表示します。 カスタムポリシーを選択すると、そのポリシーを編集、削除、 [有効/無効にするための追加のコントロールが表示されます](#enable)。

![](../images/policies/policy-details.png)

## カスタムデータ使用ポリシーの作成 {#create-policy}

新しいカスタムデータ使用ポリシーを作成するには、ポリシー **[!UICONTROL ワークスペースの「]** 参照 ****** 」タブの右上隅にある「ポリシーを作成」をクリックします。

![](../images/policies/create-policy-button.png)

「 *[!UICONTROL ポリシーの作成]* 」ワークフローが表示されます。 新しいポリシーの名前と説明を入力して開始します。

![](../images/policies/create-policy-description.png)

次に、ポリシーの基にするデータ使用ラベルを選択します。 複数のラベルを選択する場合、ポリシーを適用するために、データにすべてのラベルを含めるか、ラベルの1つだけを含めるかを選択できます。 終了したら「**[!UICONTROL 次へ]**」をクリックします。

![](../images/policies/add-labels.png)

「マーケティングアクション *[!UICONTROL の選択]* 」手順が表示されます。 表示されたリストから適切なマーケティングアクションを選択し、「 **[!UICONTROL 次へ]** 」をクリックして続行します。

>[!NOTE] 複数のマーケティングアクションを選択する場合、ポリシーはそれらを「OR」ルールとして解釈します。 つまり、選択したい _ずれかのマーケティング操作が実行された場合_ 、ポリシーが適用されます。

![](../images/policies/add-marketing-actions.png)

「 *[!UICONTROL レビュー]* 」手順が表示され、新しいポリシーを作成する前にその詳細を確認できます。 問題が解決したら、「 **[!UICONTROL 完了]** 」をクリックしてポリシーを作成します。

![](../images/policies/policy-review.png)

「 *[!UICONTROL 参照]* 」タブが再び表示され、新しく作成したポリシーが「ドラフト」ステータスでリストされます。 ポリシーを有効にするには、次のセクションを参照してください。

![](../images/policies/created-policy.png)

## データ使用ポリシーの有効化または無効化 {#enable}

すべてのデータ使用ポリシー（アドビが提供するコアポリシーを含む）は、デフォルトで無効になっています。 個々のポリシーの適用が考慮されるようにするには、APIまたはUIを使用して手動でそのポリシーを有効にする必要があります。

ポリシーは、ポリシーワークス *[!UICONTROL ペースの「]* 参照 ** 」タブで有効または無効にできます。 リストからカスタムポリシーを選択し、詳細を右側に表示します。 「 *[!UICONTROL ステータス]*」で、切り替えボタンを選択してポリシーを有効または無効にします。

![](../images/policies/enable-policy.png)

## 表示マーケティングのアクション {#view-marketing-actions}

「 **[!UICONTROL ポリシー]** 」ワークスペースで、「 **[!UICONTROL マーケティングアクション]** 」タブを選択して、アドビおよび自分の組織が定義した使用可能なマーケティングアクションのリストを表示します。

![](../images/policies/marketing-actions.png)

## マーケティングアクションの作成 {#create-marketing-action}

新しいカスタムマーケティングアクションを作成するには、「 **[!UICONTROL ポリシー]****[!UICONTROL 」ワークスペースの「マーケティングアクション]**** 」タブの右上隅にある「マーケティングアクションを作成」をクリックします。

![](../images/policies/create-marketing-action.png)

マーケティングアクション *[!UICONTROL の作成]* ダイアログが表示されます。 マーケティングアクションの名前と説明を入力し、「 **[!UICONTROL 作成]**」をクリックします。

![](../images/policies/create-marketing-action-details.png)

新しく作成したアクションが「 *[!UICONTROL マーケティングアクション]* 」タブに表示されます。 新しいデータ使用ポリシーを [作成する際に、マーケティングアクションを使用できるようになりました](#create-policy)。

![](../images/policies/created-marketing-action.png)

## マーケティングアクションの編集または削除 {#edit-delete-marketing-action}

>[!NOTE] 編集できるのは、組織で定義されたカスタムマーケティングアクションのみです。 アドビが定義するマーケティングアクションは、変更または削除できません。

「 **[!UICONTROL ポリシー]** 」ワークスペースで、「 **[!UICONTROL マーケティングアクション]** 」タブを選択して、アドビおよび自分の組織が定義した使用可能なマーケティングアクションのリストを表示します。 リストからカスタムマーケティングアクションを選択し、右側のセクションに表示されるフィールドを使用して、マーケティングアクションの詳細を編集します。

![](../images/policies/edit-marketing-action.png)

既存の使用ポリシーでマーケティングアクションが使用されていない場合は、「マーケティングアクションを **[!UICONTROL 削除]**」をクリックして削除できます。

>[!NOTE] 既存のポリシーで使用されているマーケティングアクションを削除しようとすると、削除の試行に失敗したことを示すエラーメッセージが表示されます。

![](../images/policies/delete-marketing-action.png)

## 次の手順

このドキュメントでは、Experience PlatformUIでデータ使用ポリシーを管理する方法の概要を説明しました。 DULE Policy APIを使用してポリシーを管理する手順については、 [開発者ガイドを参照してください](../api/getting-started.md)。 データ使用ポリシーの適用方法について詳しくは、「 [ポリシー適用の概要](../enforcement/overview.md)」を参照してください。

次のビデオでは、Experience PlatformUIで使用ポリシーを使用する方法のデモを示します。

>[!VIDEO](https://video.tv.adobe.com/v/32977?quality=12&learn=on)