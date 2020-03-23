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

1. 接続/ **[!UICONTROL 宛先で]**、目的のクラウドストレージの宛先を選択し、「宛先に接続」を **[!UICONTROL 選択します]**。

   ![クラウドストレージの接続先](/help/rtcdp/destinations/assets/connect-cloud-destination.png)

2. クラウド **ストレージ** の宛先への接続を設定済みの場合は、認証手順で「既存のアカウント」を選択 **[!UICONTROL し、既存の接続を選択します]** 。 または、「新規アカウント」 **[!UICONTROL を選択して]** 、クラウドストレージの宛先への新しい接続を設定できます。 アカウント認証資格情報を入力し、「宛先に接続」 **[!UICONTROL を選択します]**。

   >[!NOTE]
   >
   >Adobe Real-time CDPは、認証プロセスでの資格情報の検証をサポートし、クラウドのストレージの場所に誤った資格情報を入力した場合にエラーメッセージを表示します。 これにより、正しくない資格情報でワークフローが完了しなくなります。

   ![クラウドストレージの接続先 — 認証手順](/help/rtcdp/destinations/assets/cloud-destinations-authentication-step.png)

3. 設定手順 **[!UICONTROL で]** 、アクティベ **[!UICONTROL ートフ]** ローの名前と **** 説明を入力します。
   1. Amazon S3の宛先の場合は、ファイルの配 **[!UICONTROL 信先に]** Bucket名と **[!UICONTROL Folderパスをクラウドストレージ]** の宛先に挿入します。 上記のフ **[!UICONTROL ィールドに入力し]** 、「宛先を作成」を選択します。
   2. SFTPの宛先に対して、フォルダーパスを **[!UICONTROL 挿入します]**
   ![クラウドストレージの接続先 — 認証手順](/help/rtcdp/destinations/assets/cloud-destinations-setup-step.png)

4. これで宛先が作成されます。 後でセグメントをア **[!UICONTROL クティブにする場合は]** 、「保存して終了」を選択できます。または、「次へ **** 」を選択してワークフローを続行し、アクティブにするセグメントを選択することもできます。 どちらの場合も、残りのデータをエクスポートするワークフ [ローについては](#activate-segments)、次の「セグメントをアクティブ化」の節を参照してください。

## セグメントのアクティブ化 {#activate-segments}

セグメン [トのアクティブ化ワークフローについて詳しくは](/help/rtcdp/destinations/activate-destinations.md) 、「宛先へのプロファイルとセグメントのアクティブ化」を参照してください。