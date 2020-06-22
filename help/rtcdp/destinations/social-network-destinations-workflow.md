---
title: ソーシャルネットワーク宛先のワークフロー
seo-title: ソーシャルネットワーク宛先のワークフロー
description: ソーシャルネットワーク広告アカウントに接続する手順
seo-description: ソーシャルネットワーク広告アカウントに接続する手順
translation-type: tm+mt
source-git-commit: 3c598454a868139b7604c5c7ca2b98fa0f1bb961
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 11%

---


# ソーシャルネットワーク宛先の認証ワークフロー {#social-network-destinations-workflow}

## ソーシャルネットワークの宛先を作成するためのワークフロー

このチュートリアルではFacebookを例として使用しますが、Adobeリアルタイム顧客データPlatformのワークフローは、すべてのソーシャルネットワークの宛先に対して同じになり、もう一度製品に追加されます。

1. 「 **[!UICONTROL Destinations/Catalog]**」で、「 **[!UICONTROL Social]** 」カテゴリまでスクロールします。 希望するソーシャルネットワークの宛先を選択し、「 **[!UICONTROL 宛先に接続]**」を選択します。

   ![ソーシャルネットワークの宛先に接続](/help/rtcdp/destinations/assets/facebook-catalog-view.png)

2. In the **Authentication** step, if you had previously set up a connection to your social network destination, select **[!UICONTROL Existing Account]** and select your existing connection. Or, you can select **[!UICONTROL New Account]** to set up a new connection to your social network destination. 「 **[!UICONTROL 接続先に接続]** 」を選択すると、選択したソーシャルネットワークの接続先にログインし、Adobe Experience Cloudをソーシャルネットワーク広告アカウントに接続します。

   >[!NOTE]
   >
   >Adobe Real-time CDPは、認証プロセスでの秘密鍵証明書の検証をサポートし、ソーシャルネットワークアカウントIDに誤った秘密鍵証明書を入力した場合にエラーメッセージを表示します。 これにより、間違った資格情報を使用してワークフローを完了できなくします。

   ![ソーシャルネットワークの宛先に接続 — 認証手順](/help/rtcdp/destinations/assets/facebook-pre-connect-view.png)

3. 資格情報が確認され、Adobe Experience Cloudがソーシャルネットワークに接続されたら、「 **[!UICONTROL 次へ]** 」を選択して **[!UICONTROL 設定]** 手順に進むことができます。

   ![資格情報が確認されました](/help/rtcdp/destinations/assets/facebook-post-connection-view.png)

4. 「 **[!UICONTROL 設定]** 」の手順で、アクティベーションフローの **[!UICONTROL 名前]** と説明を入力し、Social Network Adアカウントの ******** アカウントIDを入力します。 <br> また、この手順では、この宛先に適用する **[!UICONTROL マーケティングの使用例]** を選択できます。 マーケティングの使用例は、データがエクスポート先にエクスポートされる意図を示します。 アドビ定義のマーケティングの使用例から選択するか、独自のマーケティングの使用例を作成することができます。 マーケティングの使用例の詳細については、Real-time CDP [（リアルタイムCDP）ページの「](/help/rtcdp/privacy/data-governance-overview.md#destinations) Data Governance（データ・ガバナンス）」を参照してください。 アドビが定義した個々のマーケティングの使用例について詳しくは、 [データ使用ポリシーの概要を参照してください](/help/data-governance/policies/overview.md#core-actions)。 <br>上記のフィールドに入力した後、「**[!UICONTROL 宛先を作成]**」を選択します。

   >[!IMPORTANT]
   >
   > * ソーシャルネットワークの宛先に対しては、 *単一IDパーソナライゼーション* (Single Identity Personalization)マーケティングの使用例がデフォルトで選択されており、削除できません。
   > * Facebookのリンク先の場合。 **[!UICONTROL アカウントID]** は、Facebook広告のアカウントIDです。 このIDはFacebook広告マネージャーで確認できます。 IDの先頭に、次のよう `act_` に付けます。


   ![ソーシャルネットワークの宛先に接続 — 設定手順](/help/rtcdp/destinations/assets/social-networks-setup-step.png)

5. これで宛先が作成されました。後でセグメントをアクティブにする場合は、「**[!UICONTROL 保存して終了]**」を選択できます。または、「**[!UICONTROL 次へ]**」を選択してワークフローを続行し、アクティブ化するセグメントを選択することもできます。In either case, see the next section, [Activate segments to social networks](#activate-segments), for the rest of the workflow.

## ソーシャルネットワークへのセグメントのアクティブ化 {#activate-segments}

For instructions on how to activate segments to social networks, see [Activate Data to Destinations](/help/rtcdp/destinations/activate-destinations.md).