---
keywords: Experience Platform;home;popular topics;data governance;data usage policy user guide
solution: Experience Platform
title: データ使用ポリシーユーザガイド
topic: policies
description: Adobe Experience Platform データガバナンスは、データ使用ポリシーを作成および管理できるユーザーインターフェイスを提供します。このドキュメントでは、Experience Platformユーザーインターフェイスのポリシーワークスペースで実行できるアクションの概要を説明します。
translation-type: tm+mt
source-git-commit: a362b67cec1e760687abb0c22dc8c46f47e766b7
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 39%

---


# データ使用ポリシーユーザガイド

Adobe Experience Platform [!DNL Data Governance] provides a user interface that allows you to create and manage data usage policies. This document provides an overview of the actions you can perform in the **Policies** workspace in the [!DNL Experience Platform] user interface.

>[!IMPORTANT]
>
>すべてのデータ使用ポリシー(Adobeが提供するコアポリシーを含む)は、デフォルトで無効になっています。 個々のポリシーを適用対象と見なすには、そのポリシーを手動で有効にする必要があります。 UIでこれを行う方法については、ポリシー [の有効化の節を参照してください](#enable) 。

## 前提条件

This guide requires a working understanding of the following [!DNL Experience Platform] concepts:

- [[!DNL Data Governance]](../home.md)
- [データ使用ポリシー](./overview.md)

## データ使用ポリシーの表示 {#view-policies}

In the [!DNL Experience Platform] UI, click **[!UICONTROL Policies]** to open the **[!UICONTROL Policies]** workspace. 「**[!UICONTROL 参照]**」タブには、使用可能なポリシー（関連するラベル、マーケティングアクション、ステータスなど）が一覧表示されます。

![](../images/policies/browse-policies.png)

ポリシーをクリックすると、説明と種類が表示されます。カスタムポリシーを選択すると、ポリシーを編集、削除、[有効／無効にする](#enable)ための追加のコントロールが表示されます。

![](../images/policies/policy-details.png)

## カスタムデータ使用ポリシーの作成 {#create-policy}

To create a new custom data usage policy, click **[!UICONTROL Create policy]** in the top-right corner of the **[!UICONTROL Browse]** tab in the **[!UICONTROL Policies]** workspace.

![](../images/policies/create-policy-button.png)

**[!UICONTROL ポリシーの作成]**&#x200B;ワークフローが表示されます。まず、新しいポリシーの名前と説明を指定します。

![](../images/policies/create-policy-description.png)

次に、ポリシーの基にするデータ使用ラベルを選択します。複数のラベルを選択する場合は、ポリシーが適用するために、データにすべてのラベルを含めるか、1 つのラベルのみを含めるかを選択できます。終了したら「**[!UICONTROL 次へ]**」をクリックします。

![](../images/policies/add-labels.png)

「**[!UICONTROL マーケティングアクションの選択]**」手順が表示されます。表示されたリストから適切なマーケティングアクションを選択し、「**[!UICONTROL 次へ]**」をクリックして続行します。

>[!NOTE]
>
> 複数のマーケティングアクションを選択する場合、ポリシーはそれらを「OR」ルールとして解釈します。つまり、選択したマーケティングアクションの&#x200B;**いずれか**&#x200B;が実行された場合に、ポリシーが適用されます。

![](../images/policies/add-marketing-actions.png)

「**[!UICONTROL 確認]**」手順が表示され、新しいポリシーを作成する前にその詳細を確認できます。確認したら、「**[!UICONTROL 完了]**」をクリックして、ポリシーを作成します。

![](../images/policies/policy-review.png)

「**[!UICONTROL 参照]**」タブが再び表示され 、新しく作成したポリシーが「草案」ステータスでリストに表示されるようになります。ポリシーを有効にするには、次の節を参照してください。

![](../images/policies/created-policy.png)

## データ使用ポリシーの有効化または無効化 {#enable}

すべてのデータ使用ポリシー(Adobeが提供するコアポリシーを含む)は、デフォルトで無効になっています。 個々のポリシーの適用が考慮されるようにするには、APIまたはUIを使用して手動でそのポリシーを有効にする必要があります。

You can enable or disable policies from the **[!UICONTROL Browse]** tab in the **[!UICONTROL Policies]** workspace. リストからカスタムポリシーを選択して、右側に詳細を表示します。「**[!UICONTROL ステータス]**」で、ポリシーを有効または無効にする切り替えボタンを選択します。

![](../images/policies/enable-policy.png)

## 表示マーケティングのアクション {#view-marketing-actions}

「 **[!UICONTROL ポリシー]** 」ワークスペースで「 **[!UICONTROL マーケティングアクション]** 」タブを選択し、Adobeや自分の組織が定義した、使用可能なマーケティングアクションのリストを表示します。

![](../images/policies/marketing-actions.png)

## Create a marketing action {#create-marketing-action}

新しいカスタムマーケティングアクションを作成するには、「 **[!UICONTROL ポリシー]****[!UICONTROL 」ワークスペースの「マーケティングアクション]****** 」タブの右上隅にある「マーケティングアクションを作成」をクリックします。

![](../images/policies/create-marketing-action.png)

マーケティングアクション **[!UICONTROL の作成]** ダイアログが表示されます。 マーケティングアクションの名前と説明を入力し、「 **[!UICONTROL 作成]**」をクリックします。

![](../images/policies/create-marketing-action-details.png)

新しく作成したアクションが「 **[!UICONTROL マーケティングアクション]** 」タブに表示されます。 新しいデータ使用ポリシーを [作成する際に、マーケティングアクションを使用できるようになりました](#create-policy)。

![](../images/policies/created-marketing-action.png)

## マーケティングアクションの編集または削除 {#edit-delete-marketing-action}

>[!NOTE]
>
>編集できるのは、組織で定義されたカスタムマーケティングアクションのみです。 Adobeによって定義されたマーケティングアクションは、変更または削除できません。

「 **[!UICONTROL ポリシー]** 」ワークスペースで「 **[!UICONTROL マーケティングアクション]** 」タブを選択し、Adobeや自分の組織が定義した、使用可能なマーケティングアクションのリストを表示します。 リストからカスタムマーケティングアクションを選択し、右側のセクションに表示されるフィールドを使用して、マーケティングアクションの詳細を編集します。

![](../images/policies/edit-marketing-action.png)

既存の使用ポリシーでマーケティングアクションが使用されていない場合は、「マーケティングアクションを **[!UICONTROL 削除]**」をクリックして削除できます。

>[!NOTE]
>
>既存のポリシーで使用されているマーケティングアクションを削除しようとすると、削除の試行に失敗したことを示すエラーメッセージが表示されます。

![](../images/policies/delete-marketing-action.png)

## 次の手順

This document provided an overview of how to manage data usage policies in [!DNL Experience Platform] UI. を使用してポリシーを管理する手順については、『 [!DNL Policy Service API]開発者ガイド [](../api/getting-started.md)』を参照してください。 データ使用ポリシーの適用方法について詳しくは、「[ポリシー実施の概要](../enforcement/overview.md)」を参照してください。

次のビデオでは、 [!DNL Experience Platform] UIで使用ポリシーを使用する方法のデモを示します。

>[!VIDEO](https://video.tv.adobe.com/v/32977?quality=12&learn=on)