---
title: クラウドストレージの宛先のワークフロー
seo-title: クラウドストレージの宛先のワークフロー
description: クラウドストレージの場所への接続手順
seo-description: クラウドストレージの場所への接続手順
translation-type: tm+mt
source-git-commit: 9221c11a30bda3a155d73afec16be55ef8f5d133

---


# クラウドストレージの宛先を作成するためのワークフロー

## 概要

このページでは、Adobe Real-time Customer Data Platformでクラウドストレージの場所に接続する方法を説明します。

1. で、目 **[!UICONTROL Connections > Destinations]**&#x200B;的のクラウドストレージの保存先を選択し、を選択しま **[!UICONTROL Connect destination]**&#x200B;す。

   ![クラウドストレージの接続先](/help/rtcdp/destinations/assets/connect-cloud-destination.png)

2. クラウド **ストレージ** の宛先への接続を設定済みの場合は、認証手順で、既存の接続を選択 **[!UICONTROL Existing Account]** して選択します。 または、クラウドストレージ **[!UICONTROL New Account]** の宛先への新しい接続を設定するように選択できます。 アカウント認証資格情報を入力し、を選択しま **[!UICONTROL Connect to destination]**&#x200B;す。

   >[!NOTE]
   >
   >Adobe Real-time CDPは、認証プロセスでの資格情報の検証をサポートし、クラウドのストレージの場所に誤った資格情報を入力した場合にエラーメッセージを表示します。 これにより、正しくない資格情報でワークフローが完了しなくなります。

   ![クラウドストレージの接続先 — 認証手順](/help/rtcdp/destinations/assets/cloud-destinations-authentication-step.png)

3. 手順で、ア **[!UICONTROL Setup]** クティブ化フ **[!UICONTROL Name]** ローのaと **[!UICONTROL Description]** aを入力します。
   1. Amazon S3の宛先の場合は、ファイルが配 **[!UICONTROL Bucket name]** 信されるク **[!UICONTROL Folder path]** ラウドストレージの宛先に、とを挿入します。 上記のフ **[!UICONTROL Create Destination]** ィールドに入力した後に選択します。
   2. SFTPの送信先に対して、 **[!UICONTROL Folder path]**
   ![クラウドストレージの接続先 — 認証手順](/help/rtcdp/destinations/assets/cloud-destinations-setup-step.png)

4. これで宛先が作成されます。 後でセグメントをア **[!UICONTROL Save & Exit]** クティブにするか、ワークフローを続行してアクティブにするセグ **[!UICONTROL Next]** メントを選択するかを選択できます。 どちらの場合も、残りのデータをエクスポートするワークフ [ローについては](#activate-segments)、次の「セグメントをアクティブ化」の節を参照してください。

## セグメントのアクティブ化 {#activate-segments}

セグメン [トのアクティブ化ワークフローについて詳しくは](/help/rtcdp/destinations/activate-destinations.md) 、「宛先へのプロファイルとセグメントのアクティブ化」を参照してください。