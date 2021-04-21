---
keywords: Experience Platform；ホーム；人気の高いトピック；データガバナンス；データ使用ポリシーユーザーガイド
solution: Experience Platform
title: UIでのデータ使用ポリシーの管理
topic-legacy: policies
description: Adobe Experience Platform データガバナンスは、データ使用ポリシーを作成および管理できるユーザーインターフェイスを提供します。このドキュメントでは、Experience Platformユーザーインターフェイスのポリシーワークスペースで実行できるアクションの概要を説明します。
exl-id: 29434dc1-02c2-4267-a1f1-9f73833e76a0
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '772'
ht-degree: 31%

---

# UI でのデータ使用ポリシーの管理

Adobe Experience Platform[!DNL Data Governance]は、データ使用ポリシーの作成と管理を可能にするユーザーインターフェイスを提供します。 このドキュメントでは、[!DNL Experience Platform]ユーザーインターフェイスの&#x200B;**ポリシー**&#x200B;ワークスペースで実行できるアクションの概要を説明します。

>[!IMPORTANT]
>
>すべてのデータ使用ポリシー(Adobeが提供するコアポリシーを含む)は、デフォルトで無効になっています。 個々のポリシーを適用対象と見なすには、そのポリシーを手動で有効にする必要があります。 UIでこれを行う手順については、[ポリシー](#enable)の有効化の節を参照してください。

## 前提条件

このガイドでは、以下の[!DNL Experience Platform]概念に関する作業的な理解が必要です。

- [[!DNL Data Governance]](../home.md)
- [データ使用ポリシー](./overview.md)

## 表示の既存ポリシー{#view-policies}

[!DNL Experience Platform] UIで「**[!UICONTROL ポリシー]**」を選択し、**[!UICONTROL ポリシー]**&#x200B;ワークスペースを開きます。 「**[!UICONTROL 参照]**」タブには、使用可能なポリシー（関連するラベル、マーケティングアクション、ステータスなど）が一覧表示されます。

![](../images/policies/browse-policies.png)

説明とタイプを表示するために、一覧からポリシーを選択します。 カスタムポリシーを選択すると、ポリシーを編集、削除、[有効／無効にする](#enable)ための追加のコントロールが表示されます。

![](../images/policies/policy-details.png)

## カスタムポリシー{#create-policy}の作成

新しいカスタムデータ使用ポリシーを作成するには、**[!UICONTROL ポリシー]**&#x200B;ワークスペースの&#x200B;**[!UICONTROL 参照]**&#x200B;タブの右上隅にある「ポリシー&#x200B;**[!UICONTROL 作成]**」を選択します。

![](../images/policies/create-policy-button.png)

**[!UICONTROL ポリシーの作成]**&#x200B;ワークフローが表示されます。まず、新しいポリシーの名前と説明を指定します。

![](../images/policies/create-policy-description.png)

次に、ポリシーの基にするデータ使用ラベルを選択します。複数のラベルを選択する場合は、ポリシーが適用するために、データにすべてのラベルを含めるか、1 つのラベルのみを含めるかを選択できます。終了したら「**[!UICONTROL 次へ]**」を選択します。

![](../images/policies/add-labels.png)

「**[!UICONTROL マーケティングアクションの選択]**」手順が表示されます。提供されたリストから適切なマーケティングアクションを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

>[!NOTE]
>
> 複数のマーケティングアクションを選択する場合、ポリシーはそれらを「OR」ルールとして解釈します。つまり、選択したマーケティングアクションの&#x200B;**いずれか**&#x200B;が実行された場合に、ポリシーが適用されます。

![](../images/policies/add-marketing-actions.png)

「**[!UICONTROL 確認]**」手順が表示され、新しいポリシーを作成する前にその詳細を確認できます。問題が解決したら、「**[!UICONTROL 完了]**」を選択してポリシーを作成します。

![](../images/policies/policy-review.png)

「**[!UICONTROL 参照]**」タブが再び表示され 、新しく作成したポリシーが「草案」ステータスでリストに表示されるようになります。ポリシーを有効にするには、次の節を参照してください。

![](../images/policies/created-policy.png)

## ポリシー{#enable}を有効または無効にする

すべてのデータ使用ポリシー(Adobeが提供するコアポリシーを含む)は、デフォルトで無効になっています。 個々のポリシーの適用が考慮されるようにするには、APIまたはUIを使用して手動でそのポリシーを有効にする必要があります。

ポリシーは、**[!UICONTROL ポリシー]**&#x200B;ワークスペースの「**[!UICONTROL 参照]**」タブで有効または無効にできます。 リストからカスタムポリシーを選択して、右側に詳細を表示します。「**[!UICONTROL ステータス]**」で、ポリシーを有効または無効にする切り替えボタンを選択します。

![](../images/policies/enable-policy.png)

## 表示マーケティングアクション{#view-marketing-actions}

「**[!UICONTROL ポリシー]**」ワークスペースで、「**[!UICONTROL マーケティングアクション]**」タブを選択し、Adobeおよび自分の組織で定義されている使用可能なマーケティングアクションのリストを表示します。

![](../images/policies/marketing-actions.png)

## マーケティングアクションの作成{#create-marketing-action}

新しいカスタムマーケティングアクションを作成するには、**[!UICONTROL ポリシー]**&#x200B;ワークスペースの&#x200B;**[!UICONTROL マーケティングアクション]**&#x200B;タブの右上隅にある「マーケティングアクション&#x200B;]**を作成」を選択します。**[!UICONTROL 

![](../images/policies/create-marketing-action.png)

**[!UICONTROL マーケティングアクションの作成]**&#x200B;ダイアログが表示されます。 マーケティングアクションの名前と説明を入力し、「**[!UICONTROL 作成]**」を選択します。

![](../images/policies/create-marketing-action-details.png)

新しく作成したアクションは、「**[!UICONTROL マーケティングアクション]**」タブに表示されます。 [新しいデータ使用ポリシー](#create-policy)の作成時に、マーケティングアクションを使用できるようになりました。

![](../images/policies/created-marketing-action.png)

## マーケティングアクションの編集または削除{#edit-delete-marketing-action}

>[!NOTE]
>
>編集できるのは、組織で定義されたカスタムマーケティングアクションのみです。 Adobeによって定義されたマーケティングアクションは、変更または削除できません。

「**[!UICONTROL ポリシー]**」ワークスペースで、「**[!UICONTROL マーケティングアクション]**」タブを選択し、Adobeおよび自分の組織で定義されている使用可能なマーケティングアクションのリストを表示します。 リストからカスタムマーケティングアクションを選択し、右側のセクションに表示されるフィールドを使用して、マーケティングアクションの詳細を編集します。

![](../images/policies/edit-marketing-action.png)

既存の使用ポリシーでマーケティングアクションが使用されていない場合は、「**[!UICONTROL マーケティングアクションを削除]**」を選択して削除できます。

>[!NOTE]
>
>既存のポリシーで使用されているマーケティングアクションを削除しようとすると、削除の試行に失敗したことを示すエラーメッセージが表示されます。

![](../images/policies/delete-marketing-action.png)

## 次の手順

このドキュメントでは、[!DNL Experience Platform] UIでデータ使用ポリシーを管理する方法の概要を説明しました。 [!DNL Policy Service API]を使用してポリシーを管理する手順については、[開発者ガイド](../api/getting-started.md)を参照してください。 データ使用ポリシーの適用方法について詳しくは、「[ポリシー実施の概要](../enforcement/overview.md)」を参照してください。

次のビデオでは、[!DNL Experience Platform] UIでの使用ポリシーの使用方法のデモを示します。

>[!VIDEO](https://video.tv.adobe.com/v/32977?quality=12&learn=on)
