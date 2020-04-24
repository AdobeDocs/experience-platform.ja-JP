---
title: ソーシャルネットワークの宛先のワークフロー
seo-title: ソーシャルネットワークの宛先のワークフロー
description: ソーシャルネットワーク広告アカウントに接続する手順
seo-description: ソーシャルネットワーク広告アカウントに接続する手順
translation-type: tm+mt
source-git-commit: bfcbc56f05fa1c3b5fafd57b1166e50130b6007d

---


# （ベータ）ソーシャルネットワークの宛先の認証ワークフロー {#social-network-destinations-workflow}


>[!IMPORTANT]
>
>Adobe Real-time CDPでソーシャルネットワークの宛先を作成するワークフローは現在ベータ版で、すべてのユーザーが利用できるわけではありません。 ドキュメントと機能は変更される場合があります。

## クラウドストレージの宛先を作成するためのワークフロー

このチュートリアルではFacebookを例として使用しますが、Adobe Real-time Customer Data Platformのワークフローはすべてのソーシャルネットワークの宛先で同じになり、もう一度製品に追加されます。

1. 接続/ **[!UICONTROL 宛先で]**、 **[!UICONTROL Social]** カテゴリまでスクロールします。 優先するソーシャルネットワークの宛先を選択し、「宛先に接続」 **[!UICONTROL を選択しま]**&#x200B;す。

   ![ソーシャルネットワークの宛先に接続](/help/rtcdp/destinations/assets/facebook-catalog-view.png)

2. In the **Authentication** step, if you had previously set up a connection to your social network destination, select **[!UICONTROL Existing Account]** and select your existing connection. Or, you can select **[!UICONTROL New Account]** to set up a new connection to your social network destination. 「宛先 **[!UICONTROL に接続」を選択すると]** 、選択したソーシャルネットワークの宛先に移動し、ログインしてAdobe Experience Cloudをソーシャルネットワーク広告アカウントに接続します。

   >[!NOTE]
   >
   >Adobe Real-time CDPは、認証プロセスでの資格情報の検証をサポートし、ソーシャルネットワークアカウントIDに誤った資格情報を入力した場合にエラーメッセージを表示します。 これにより、間違った資格情報を使用してワークフローを完了できなくします。

   ![ソーシャルネットワークの宛先に接続 — 認証手順](/help/rtcdp/destinations/assets/facebook-pre-connect-view.png)

3. 資格情報が確認され、Adobe Experience Cloudがソーシャルネットワークに接続されたら、「次へ」を選択し **[!UICONTROL て]** 、設定手順に進むことがで **[!UICONTROL きます]** 。

   ![認証情報の確認](/help/rtcdp/destinations/assets/facebook-post-connection-view.png)

4. 設定手順で **[!UICONTROL 、]** アクティベーションフ **[!UICONTROL ローの名前]** と説明を入力し、Social Network Ad Account ******** のアカウントIDを入力します。 上記のフィールドに入力した後、「**[!UICONTROL 宛先を作成]**」を選択します。

   >[!IMPORTANT]
   >
   >Facebookの宛先の場合。 **[!UICONTROL アカウントIDは]** 、Facebook広告アカウントIDです。 このIDは、Facebook広告マネージャーで確認できます。 IDのプレフィックスを次のよ `act_` うに付加します。

   ![ソーシャルネットワークの宛先に接続 — 設定手順](/help/rtcdp/destinations/assets/social-network-step.png)

5. これで宛先が作成されました。後でセグメントをアクティブにする場合は、「**[!UICONTROL 保存して終了]**」を選択できます。または、「**[!UICONTROL 次へ]**」を選択してワークフローを続行し、アクティブ化するセグメントを選択することもできます。In either case, see the next section, [Activate segments to social networks](#activate-segments), for the rest of the workflow.

## ソーシャルネットワークへのセグメントのアクティブ化 {#activate-segments}

For instructions on how to activate segments to social networks, see [Activate Data to Destinations](/help/rtcdp/destinations/activate-destinations.md).