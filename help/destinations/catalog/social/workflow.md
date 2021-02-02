---
keywords: Facebook;Facebook；ソーシャルネットワーク；ソーシャルネットワーク；ソーシャルネットワーク認証；ソーシャルネットワーク認証
title: ソーシャルネットワークの宛先のワークフロー
type: Tutorial
seo-title: ソーシャルネットワークの宛先のワークフロー
description: ソーシャルネットワーク広告アカウントに接続する手順
seo-description: ソーシャルネットワーク広告アカウントに接続する手順
translation-type: tm+mt
source-git-commit: d1aa2c825cd679d593cf97d84506058482a7fe8f
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 56%

---


# ソーシャルネットワーク宛先の認証ワークフロー{#social-network-destinations-workflow}

## ソーシャルネットワークの宛先を作成するためのワークフロー

このチュートリアルでは例として[!DNL Facebook]を使用しますが、Adobe Experience Platformのワークフローはすべてのソーシャルネットワークの宛先に対して同じになり、もう一度製品に追加されます。

**[!UICONTROL 宛先]**/**[!UICONTROL カタログ]**&#x200B;で、**[!UICONTROL ソーシャル]**&#x200B;カテゴリまでスクロールします。 希望するソーシャルネットワークの宛先を選択し、**[!UICONTROL 設定]**&#x200B;を選択します。

![ソーシャルネットワークの宛先に接続する](../../assets/catalog/social/workflow/catalog.png)

>[!NOTE]
>
>この宛先との接続が既に存在する場合は、宛先カードに「**[!UICONTROL Activate]**」ボタンが表示されます。 「**[!UICONTROL アクティブ化]**」と「**[!UICONTROL 設定]**」の違いについて詳しくは、保存先のワークスペースドキュメントの「[カタログ](../../ui/destinations-workspace.md#catalog)」の節を参照してください。

ソーシャルネットワークの宛先への接続を既に設定している場合は、**認証**&#x200B;手順で「**[!UICONTROL 既存のアカウント]**」を選択し、既存の接続を選択します。または、「**[!UICONTROL 新規アカウント]**」を選択して、ソーシャルネットワークの宛先への新しい接続を設定できます。「**[!UICONTROL 宛先に接続]**」を選択すると、選択したソーシャルネットワークの宛先に移動するので、ログインして Adobe Experience Cloud をソーシャルネットワーク広告アカウントに接続します。

>[!NOTE]
>
>プラットフォームは、認証プロセスで資格情報の検証をサポートしており、ソーシャルネットワークアカウントIDに正しくない資格情報を入力すると、エラーメッセージを表示します。 このため、間違った資格情報を使用すると、ワークフローを完了することができません。

![ソーシャルネットワークの宛先に接続 - 認証手順](../../assets/catalog/social/workflow/pre-connect.png)

資格情報が確認され、Adobe Experience Cloud がソーシャルネットワークに接続されたら、「**[!UICONTROL 次へ]**」を選択して&#x200B;**[!UICONTROL 設定]**&#x200B;手順に進むことができます。

![資格情報の確認](../../assets/catalog/social/workflow/post-connect.png)

**[!UICONTROL 設定]**&#x200B;手順で、アクティベーションフローの[!UICONTROL 名前]と[!UICONTROL 説明]を入力し、ソーシャルネットワーク広告アカウントの[!UICONTROL アカウント ID] を入力します。

また、この手順では、この宛先に適用する&#x200B;**[!UICONTROL マーケティングの使用例]**&#x200B;を選択できます。 マーケティングの使用例は、データがエクスポート先にエクスポートされる意図を示します。 Adobe定義のマーケティングの使用例から選択するか、独自のマーケティングの使用例を作成することができます。 マーケティングの使用例について詳しくは、[データ使用ポリシーの概要](../../../data-governance/policies/overview.md)を参照してください。

上記のフィールドに入力した後、「**[!UICONTROL 宛先を作成]**」を選択します。

>[!IMPORTANT]
>
> * [!DNL Facebook]宛先用。 **[!UICONTROL アカウント]** IDはお客様 [!DNL Facebook Ad Account ID]の。このIDは[!DNL Facebook Ads Manager]にあります。 以下に示すように、ID の前に `act_` を追加します。


![ソーシャルネットワークの宛先に接続 — 設定手順](../../assets/catalog/social/workflow/setup.png)

これで宛先が作成されました。後でセグメントをアクティブにする場合は、「**[!UICONTROL 保存して終了]**」を選択します。また、「**[!UICONTROL 次へ]**」を選択してワークフローを続行し、アクティブ化するセグメントを選択することもできます。いずれの場合も、ワークフローでのこの後の操作については、次の「[ソーシャルネットワークに対してセグメントをアクティブ化する](#activate-segments)」の節を参照してください。

## ソーシャルネットワークに対してセグメントをアクティブ化する {#activate-segments}

ソーシャルネットワークに対してセグメントをアクティブ化する方法については、「[宛先へのデータのアクティブ化](../../ui/activate-destinations.md)」を参照してください。