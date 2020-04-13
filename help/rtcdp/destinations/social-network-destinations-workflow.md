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

## クラウドワークフローのストレージ先の作成

このチュートリアルではFacebookを例として使用しますが、Adobe Real-time Customer Data Platformのワークフローはすべてのソーシャルネットワークの宛先で同じになり、もう一度製品に追加されます。

1. で、 **[!UICONTROL Connections > Destinations]**&#x200B;スクロールして **[!UICONTROL Social]** カテゴリ 優先するソーシャルネットワークの宛先を選択し、を選択しま **[!UICONTROL Connect destination]**&#x200B;す。

   ![ソーシャルネットワークの宛先に接続](/help/rtcdp/destinations/assets/facebook-catalog-view.png)

2. 認証手順 **で** 、ソーシャルネットワークの宛先への接続を設定済みの場合は、既存の接続を **[!UICONTROL Existing Account]** 選択して選択します。 または、ソーシャルネットワー **[!UICONTROL New Account]** クの宛先への新しい接続を設定するように選択できます。 選択す **[!UICONTROL Connect to destination]** ると、選択したソーシャルネットワークの移動先に移動し、Adobe Experience Cloudをソーシャルネットワーク広告アカウントに接続します。

   >[!NOTE]
   >
   >Adobe Real-time CDPは、認証プロセスでの資格情報の検証をサポートし、ソーシャルネットワークアカウントIDに誤った資格情報を入力した場合にエラーメッセージを表示します。 これにより、正しくない資格情報でワークフローが完了しなくなります。

   ![ソーシャルネットワークの宛先に接続 — 認証手順](/help/rtcdp/destinations/assets/facebook-pre-connect-view.png)

3. 資格情報が確認され、Adobe Experience Cloudがソーシャルネットワークに接続されたら、手順に進 **[!UICONTROL Next]** むことを選択でき **[!UICONTROL Setup]** ます。

   ![認証情報の確認](/help/rtcdp/destinations/assets/facebook-post-connection-view.png)

4. 手順で、 **[!UICONTROL Setup]** アクティベーションフロ **[!UICONTROL Name]** ーのa **[!UICONTROL Description]** とaを入力し、ソーシャルネットワ **[!UICONTROL Account ID]** ーク広告アカウントのに入力します。 上記のフ **[!UICONTROL Create Destination]** ィールドに入力した後に選択します。

   >[!IMPORTANT]
   >
   >Facebookの宛先の場合。 **[!UICONTROL Account ID]** は、Facebook広告アカウントIDです。 このIDは、Facebook広告マネージャーで確認できます。 IDのプレフィックスを次のよ `act_` うに付加します。

   ![ソーシャルネットワークの宛先に接続 — 設定手順](/help/rtcdp/destinations/assets/social-network-step.png)

5. これで宛先が作成されます。 後でセグメントをア **[!UICONTROL Save & Exit]** クティブにするか、ワークフローを続行してアクティブにするセグ **[!UICONTROL Next]** メントを選択するかを選択できます。 どちらの場合も、残りのワークフローについては、次の「 [ソーシャルネットワークに対するセグメントの](#activate-segments)アクティブ化」の節を参照してください。

## ソーシャルネットワークへのセグメントのアクティブ化 {#activate-segments}

For instructions on how to activate segments to social networks, see [Activate Data to Destinations](/help/rtcdp/destinations/activate-destinations.md).