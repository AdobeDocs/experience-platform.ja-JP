---
keywords: Experience Platform;ホーム;人気のトピック;データガバナンス;データ使用ポリシーユーザーガイド
solution: Experience Platform
title: UI でのデータ使用ポリシーの管理
topic-legacy: policies
description: Adobe Experience Platform データガバナンスは、データ使用ポリシーを作成および管理できるユーザーインターフェイスを提供します。このドキュメントでは、Experience Platform ユーザーインターフェイスのポリシーワークスペースで実行できるアクションの概要について説明します。
exl-id: 29434dc1-02c2-4267-a1f1-9f73833e76a0
source-git-commit: 03e7863f38b882a2fbf6ba0de1755e1924e8e228
workflow-type: tm+mt
source-wordcount: '776'
ht-degree: 100%

---

# UI でのデータ使用ポリシーの管理

Adobe Experience Platform データガバナンスは、データ使用ポリシーを作成および管理できるユーザーインターフェイスを提供します。このドキュメントでは、[!DNL Experience Platform] ユーザーインターフェイスの&#x200B;**ポリシー**&#x200B;ワークスペースで実行できるアクションの概要について説明します。

>[!IMPORTANT]
>
>すべてのデータ使用ポリシー（アドビが提供するコアポリシーを含む）は、デフォルトで無効になっています。個々のポリシーを適用対象と見なすには、そのポリシーを手動で有効にする必要があります。UI でこれをおこなう手順については、[ポリシーの有効化](#enable)に関する節を参照してください。

## 前提条件

このガイドでは、次の [!DNL Experience Platform] の概念に関する十分な知識が必要です。

- [データガバナンス](../home.md)
- [データ使用ポリシー](./overview.md)

## 既存のポリシーの表示 {#view-policies}

[!DNL Experience Platform] UI で、「**[!UICONTROL ポリシー]**」を選択して、「**[!UICONTROL ポリシー]**」ワークスペースを開きます。「**[!UICONTROL 参照]**」タブには、使用可能なポリシー（関連するラベル、マーケティングアクション、ステータスなど）が一覧表示されます。

![](../images/policies/browse-policies.png)

リストされているポリシーを選択すると、説明と種類が表示されます。カスタムポリシーを選択すると、ポリシーを編集、削除、[有効／無効にする](#enable)ための追加のコントロールが表示されます。

![](../images/policies/policy-details.png)

## カスタムポリシーの作成 {#create-policy}

新しいカスタムデータ使用ポリシーを作成するには、「**[!UICONTROL ポリシー]**」ワークスペースの「 **[!UICONTROL 参照]**」タブの右上にある「**[!UICONTROL ポリシーの作成]**」をクリックします。

![](../images/policies/create-policy-button.png)

**[!UICONTROL ポリシーの作成]**&#x200B;ワークフローが表示されます。まず、新しいポリシーの名前と説明を指定します。

![](../images/policies/create-policy-description.png)

次に、ポリシーの基にするデータ使用ラベルを選択します。複数のラベルを選択する場合は、ポリシーが適用するために、データにすべてのラベルを含めるか、1 つのラベルのみを含めるかを選択できます。完了したら、「**[!UICONTROL 次へ]**」をクリックします。

![](../images/policies/add-labels.png)

「**[!UICONTROL マーケティングアクションの選択]**」手順が表示されます。表示されたリストから適切なマーケティングアクションを選択してから、「**[!UICONTROL 次へ]**」を選択して続行します。

>[!NOTE]
>
> 複数のマーケティングアクションを選択する場合、ポリシーはそれらを「OR」ルールとして解釈します。つまり、選択したマーケティングアクションの&#x200B;**いずれか**&#x200B;が実行された場合に、ポリシーが適用されます。

![](../images/policies/add-marketing-actions.png)

「**[!UICONTROL 確認]**」手順が表示され、新しいポリシーを作成する前にその詳細を確認できます。確認したら、「**[!UICONTROL 完了]**」を選択して、ポリシーを作成します。

![](../images/policies/policy-review.png)

「**[!UICONTROL 参照]**」タブが再び表示され 、新しく作成したポリシーが「草案」ステータスでリストに表示されるようになります。ポリシーを有効にするには、次の節を参照してください。

![](../images/policies/created-policy.png)

## ポリシーの有効化または無効化 {#enable}

すべてのデータ使用ポリシー（アドビが提供するコアポリシーを含む）は、デフォルトで無効になっています。個々のポリシーの適用が考慮されるようにするには、API または UI を使用して手動でそのポリシーを有効にする必要があります。

「**[!UICONTROL ポリシー]**」ワークスペースの「**[!UICONTROL 参照]**」タブで、ポリシーを有効または無効にできます。リストからカスタムポリシーを選択して、右側に詳細を表示します。「**[!UICONTROL ステータス]**」で、ポリシーを有効または無効にする切り替えボタンを選択します。

![](../images/policies/enable-policy.png)

## マーケティングアクションの表示 {#view-marketing-actions}

「**[!UICONTROL ポリシー]**」ワークスペースで、「**[!UICONTROL マーケティングアクション]**」タブを選択し、アドビおよび自分の組織で定義されている使用可能なマーケティングアクションのリストを表示します。

![](../images/policies/marketing-actions.png)

## マーケティングアクションの作成 {#create-marketing-action}

新しいカスタムマーケティングアクションを作成するには、「**[!UICONTROL ポリシー]**」ワークスペースの「**[!UICONTROL マーケティングアクション]**」タブの右上隅にある「**[!UICONTROL マーケティングアクションを作成]**」を選択します。

![](../images/policies/create-marketing-action.png)

「**[!UICONTROL マーケティングアクションを作成]**」ダイアログが表示されます。マーケティングアクションの名前と説明を入力し、「**[!UICONTROL 作成]**」を選択します。

![](../images/policies/create-marketing-action-details.png)

新しく作成したアクションは、「**[!UICONTROL マーケティングアクション]**」タブに表示されます。これで、[新しいデータ使用ポリシーを作成](#create-policy)するときに、マーケティングアクションを使用できるようになりました。

![](../images/policies/created-marketing-action.png)

## マーケティングアクションの編集または削除 {#edit-delete-marketing-action}

>[!NOTE]
>
>編集できるのは、組織で定義されたカスタムマーケティングアクションのみです。アドビによって定義されたマーケティングアクションは、変更または削除できません。

「**[!UICONTROL ポリシー]**」ワークスペースで、「**[!UICONTROL マーケティングアクション]**」タブを選択し、アドビおよび自分の組織で定義されている使用可能なマーケティングアクションのリストを表示します。リストからカスタムマーケティングアクションを選択し、右側のセクションに表示されるフィールドを使用して、マーケティングアクションの詳細を編集します。

![](../images/policies/edit-marketing-action.png)

既存の使用ポリシーでマーケティングアクションが使用されていない場合は、「**[!UICONTROL マーケティングアクションを削除]**」を選択して削除できます。

>[!NOTE]
>
>既存のポリシーで使用されているマーケティングアクションを削除しようとすると、削除の試行に失敗したことを示すエラーメッセージが表示されます。

![](../images/policies/delete-marketing-action.png)

## 次の手順

このドキュメントでは、[!DNL Experience Platform] UI でデータ使用ポリシーを管理する方法の概要を説明しました。[!DNL Policy Service API] を使用してポリシーを管理する手順については、[デベロッパーガイド](../api/getting-started.md)を参照してください。データ使用ポリシーの適用方法について詳しくは、「[ポリシー実施の概要](../enforcement/overview.md)」を参照してください。

次のビデオでは、[!DNL Experience Platform] UI で使用ポリシーを操作する方法のデモを示します。

>[!VIDEO](https://video.tv.adobe.com/v/32977?quality=12&learn=on)
