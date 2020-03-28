---
title: クラウドストレージの宛先のワークフロー
seo-title: クラウドストレージの宛先のワークフロー
description: クラウドストレージの場所への接続手順
seo-description: クラウドストレージの場所への接続手順
translation-type: tm+mt
source-git-commit: 336aa90cf1e059a92a36dd0ef3222ef6a6f5123b

---


# クラウドワークフローのストレージ先の作成

## 概要

このページでは、Adobe Real-time Customer Data Platformでクラウドストレージの場所に接続する方法を説明します。

1. で、希 **[!UICONTROL Connections > Destinations]**&#x200B;望のクラウドストレージの保存先を選択し、を選択しま **[!UICONTROL Connect destination]**&#x200B;す。

   ![クラウドストレージの接続先](/help/rtcdp/destinations/assets/connect-cloud-destination.png)

2. この手順で、 **[!UICONTROL Authentication]** クラウドストレージの宛先への接続を以前に設定した場合は、既存の接続を選択 **[!UICONTROL Existing Account]** して選択します。 または、クラウド接続先へ **[!UICONTROL New Account]** の新しい接続を設定するようにストレージできます。 アカウント認証資格情報を入力し、を選択しま **[!UICONTROL Connect to destination]**&#x200B;す。 認証手順での [秘密鍵証明書の入力について詳しくは、](/help/rtcdp/destinations/amazon-s3-destination.md) Amazon S3の宛先 [/](/help/rtcdp/destinations/sftp-destination.md) SFTPの宛先を参照して **** ください。

   >[!NOTE]
   >
   >Adobe Real-time CDPは、認証プロセスでの資格情報の検証をサポートし、クラウドストレージの場所に誤った資格情報を入力した場合にエラーメッセージを表示します。 これにより、正しくない資格情報でワークフローが完了しなくなります。

   ![クラウドストレージへの接続 — 認証手順](/help/rtcdp/destinations/assets/cloud-destinations-authentication-step.png)

3. 手順で、 **[!UICONTROL Setup]** アクティベーションフローのa **[!UICONTROL Name]** とa **[!UICONTROL Description]** を入力します。 <br>
Amazon S3の宛先の場合は、ファイルが配 **[!UICONTROL Bucket name]** 信されるク **[!UICONTROL Folder path]** ラウドストレージの宛先にとを挿入します。 上記のフ **[!UICONTROL Create Destination]** ィールドに入力した後に選択します。

   ![Amazon S3クラウドのストレージ先への接続 — 認証手順](/help/rtcdp/destinations/assets/cloud-destinations-setup-step.png)

   SFTPの送信先に対して、ファイル **[!UICONTROL Folder path]** の配信先を挿入します。

   ![SFTPクラウドの接続先ストレージ — 認証手順](/help/rtcdp/destinations/assets/sftp-destinations-setup-step.png)

4. これで宛先が作成されます。 後でセグメントをア **[!UICONTROL Save & Exit]** クティブにするか、ワークフローを続行してアクティブにするセグ **[!UICONTROL Next]** メントを選択するかを選択できます。 どちらの場合も、残りのデータをエクスポートするワークフ [ローについては](#activate-segments)、次の「セグメントをアクティブ化」の節を参照してください。

## セグメントのアクティブ化 {#activate-segments}

セグメント [プロファイルのワークフローについて詳しくは、](/help/rtcdp/destinations/activate-destinations.md) 「宛先へのセグメントとセグメントのアクティブ化」を参照してください。