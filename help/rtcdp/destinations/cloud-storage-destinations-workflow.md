---
title: クラウドストレージの宛先ワークフロー
seo-title: クラウドストレージの宛先ワークフロー
description: クラウドストレージの場所への接続手順
seo-description: クラウドストレージの場所への接続手順
translation-type: tm+mt
source-git-commit: 336aa90cf1e059a92a36dd0ef3222ef6a6f5123b

---


# クラウドストレージの宛先を作成するためのワークフロー

## 概要

このページでは、Adobe Real-time Customer Data Platform でクラウドストレージの場所に接続する方法について説明します。

1. **[!UICONTROL 接続／宛先]**&#x200B;で、目的のクラウドストレージの宛先を選択してから、「**[!UICONTROL 宛先に接続]**」を選択します。

   ![クラウドストレージの宛先に接続](/help/rtcdp/destinations/assets/connect-cloud-destination.png)

2. クラウドストレージの宛先への接続を既に設定している場合は、**[!UICONTROL 認証]**&#x200B;手順で「**[!UICONTROL 既存のアカウント]**」を選択し、既存の接続を選択します。または、「**[!UICONTROL 新しいアカウント]**」を選択して、クラウドストレージの宛先への新しい接続を設定できます。アカウント認証資格情報を入力し、「**[!UICONTROL 宛先に接続]**」を選択します。認証手順での [秘密鍵証明書の入力について詳しくは、](/help/rtcdp/destinations/amazon-s3-destination.md) Amazon S3の宛先 [/](/help/rtcdp/destinations/sftp-destination.md) SFTPの宛先を参照して **** ください。

   >[!NOTE]
   >
   >Adobe Real-time CDP は、認証プロセスでの資格情報の検証をサポートし、クラウドのストレージの場所に誤った資格情報が入力されるとエラーメッセージを表示します。これにより、間違った資格情報を使用してワークフローを完了できなくします。

   ![クラウドストレージの宛先 - 認証手順](/help/rtcdp/destinations/assets/cloud-destinations-authentication-step.png)

3. 設定手順 **[!UICONTROL で]** 、 **[!UICONTROL アクティベーションフ]****** ローの名前と説明を入力します。 <br>
Amazon S3の宛先の場合は、ファイルの配 **[!UICONTROL 信先となるクラウドストレージ]****** の宛先に、バケット名とフォルダーパスを挿入します。 上記のフィールドに入力した後、「**[!UICONTROL 宛先を作成]**」を選択します。

   ![Amazon S3クラウドのストレージ先への接続 — 認証手順](/help/rtcdp/destinations/assets/cloud-destinations-setup-step.png)

   SFTPの宛先に対して、ファイルが配 **[!UICONTROL 信される]** Folderパスを挿入します。

   ![SFTPクラウドの接続先ストレージ — 認証手順](/help/rtcdp/destinations/assets/sftp-destinations-setup-step.png)

4. これで宛先が作成されました。後でセグメントをアクティブにする場合は、「**[!UICONTROL 保存して終了]**」を選択できます。または、「**[!UICONTROL 次へ]**」を選択してワークフローを続行し、アクティブ化するセグメントを選択することもできます。In either case, see the next section, [Activate segments](#activate-segments), for the rest of the workflow to export data.

## セグメントのアクティブ化 {#activate-segments}

セグメントのアクティベーションワークフローについて詳しくは、「[宛先へのプロファイルとセグメントのアクティブ化](/help/rtcdp/destinations/activate-destinations.md)」を参照してください。