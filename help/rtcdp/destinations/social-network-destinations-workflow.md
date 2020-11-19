---
keywords: Facebook;facebook;Social network;Social Network;social network authentication;Social network authentication
title: ソーシャルネットワークの宛先のワークフロー
type: Tutorial
seo-title: ソーシャルネットワークの宛先のワークフロー
description: ソーシャルネットワーク広告アカウントに接続する手順
seo-description: ソーシャルネットワーク広告アカウントに接続する手順
translation-type: tm+mt
source-git-commit: 0232acdc64019b9d93888e8137ef9bc8e114779b
workflow-type: tm+mt
source-wordcount: '500'
ht-degree: 58%

---


# Social Network destinations authentication workflow {#social-network-destinations-workflow}

## ソーシャルネットワークの宛先を作成するためのワークフロー

This tutorial uses [!DNL Facebook] as an example, but the workflow in Real-time Customer Data Platform will be the same for all social network destinations, once more are added to the product.

1. In **[!UICONTROL Destinations]** > **[!UICONTROL Catalog]**, scroll to the **[!UICONTROL Social]** category. Select your preferred social network destination, then select **[!UICONTROL Configure]**.

   ![ソーシャルネットワークの宛先に接続する](/help/rtcdp/destinations/assets/facebook-catalog-view.png)

   >[!NOTE]
   >
   >この宛先との接続が既に存在する場合は、宛先カードに **[!UICONTROL 「アクティブ化]** 」ボタンが表示されます。 「 **[!UICONTROL アクティブ化]** 」と「 **[!UICONTROL 設定]**」の違いについて詳しくは、表示先ワークスペースのドキュメントの「 [カタログ](/help/rtcdp/destinations/destinations-workspace.md#catalog) 」セクションを参照してください。

2. ソーシャルネットワークの宛先への接続を既に設定している場合は、**認証**&#x200B;手順で「**[!UICONTROL 既存のアカウント]**」を選択し、既存の接続を選択します。または、「**[!UICONTROL 新規アカウント]**」を選択して、ソーシャルネットワークの宛先への新しい接続を設定できます。「**[!UICONTROL 宛先に接続]**」を選択すると、選択したソーシャルネットワークの宛先に移動するので、ログインして Adobe Experience Cloud をソーシャルネットワーク広告アカウントに接続します。

   >[!NOTE]
   >
   >アドビのリアルタイム CDP は、認証プロセスでの資格情報の検証をサポートし、ソーシャルネットワークアカウント ID に誤った資格情報が入力されるとエラーメッセージを表示します。このため、間違った資格情報を使用すると、ワークフローを完了することができません。

   ![ソーシャルネットワークの宛先に接続 - 認証手順](/help/rtcdp/destinations/assets/facebook-pre-connect-view.png)

3. 資格情報が確認され、Adobe Experience Cloud がソーシャルネットワークに接続されたら、「**[!UICONTROL 次へ]**」を選択して&#x200B;**[!UICONTROL 設定]**&#x200B;手順に進むことができます。

   ![資格情報の確認](/help/rtcdp/destinations/assets/facebook-post-connection-view.png)

4. **[!UICONTROL 設定]**&#x200B;手順で、アクティベーションフローの[!UICONTROL 名前]と[!UICONTROL 説明]を入力し、ソーシャルネットワーク広告アカウントの[!UICONTROL アカウント ID] を入力します。<br> また、この手順では、この宛先に適用する **[!UICONTROL マーケティングの使用例]** を選択できます。 マーケティングの使用例は、データがエクスポート先にエクスポートされる意図を示します。 Adobe定義のマーケティングの使用例から選択するか、独自のマーケティングの使用例を作成することができます。 マーケティングの使用例の詳細については、Real-time CDP [（リアルタイムCDP）ページの「](/help/rtcdp/privacy/data-governance-overview.md#destinations) Data Governance（データ・ガバナンス）」を参照してください。 個々のAdobe定義マーケティングの使用例について詳しくは、 [データ使用ポリシーの概要を参照してください](/help/data-governance/policies/overview.md#core-actions)。 <br>上記のフィールドに入力した後、「**[!UICONTROL 宛先を作成]**」を選択します。

   >[!IMPORTANT]
   >
   > * ソーシャルネットワークの宛先に対しては、 *単一IDパーソナライゼーション* マーケティングの使用例がデフォルトで選択されており、削除できません。
   > * 目的 [!DNL Facebook] 地用。 **[!UICONTROL アカウントID]** がお客様のアカウント [!DNL Facebook Ad Account ID]です。 このIDは、で確認でき [!DNL Facebook Ads Manager]ます。 以下に示すように、ID の前に `act_` を追加します。


   ![ソーシャルネットワークの宛先に接続 — 設定手順](/help/rtcdp/destinations/assets/social-networks-setup-step.png)

5. これで宛先が作成されました。後でセグメントをアクティブにする場合は、「**[!UICONTROL 保存して終了]**」を選択します。また、「**[!UICONTROL 次へ]**」を選択してワークフローを続行し、アクティブ化するセグメントを選択することもできます。いずれの場合も、ワークフローでのこの後の操作については、次の「[ソーシャルネットワークに対してセグメントをアクティブ化する](#activate-segments)」の節を参照してください。

## ソーシャルネットワークに対してセグメントをアクティブ化する {#activate-segments}

ソーシャルネットワークに対してセグメントをアクティブ化する方法については、「[宛先へのデータのアクティブ化](/help/rtcdp/destinations/activate-destinations.md)」を参照してください。