---
title: クラウドストレージの宛先ワークフロー
seo-title: クラウドストレージの宛先ワークフロー
description: クラウドストレージの場所への接続手順
seo-description: クラウドストレージの場所への接続手順
translation-type: tm+mt
source-git-commit: 3c598454a868139b7604c5c7ca2b98fa0f1bb961
workflow-type: tm+mt
source-wordcount: '503'
ht-degree: 49%

---


# クラウドストレージの宛先を作成するためのワークフロー

## 概要

このページでは、Adobe Real-time Customer Data Platform でクラウドストレージの場所に接続する方法について説明します。

1. **[!UICONTROL 接続／宛先]**&#x200B;で、目的のクラウドストレージの宛先を選択してから、「**[!UICONTROL 宛先に接続]**」を選択します。

   ![クラウドストレージの宛先に接続](/help/rtcdp/destinations/assets/connect-cloud-destination.png)

2. クラウドストレージの宛先への接続を既に設定している場合は、**[!UICONTROL 認証]**&#x200B;手順で「**[!UICONTROL 既存のアカウント]**」を選択し、既存の接続を選択します。または、「**[!UICONTROL 新しいアカウント]**」を選択して、クラウドストレージの宛先への新しい接続を設定できます。アカウント認証資格情報を入力し、「**[!UICONTROL 宛先に接続]**」を選択します。<br> Amazon S3 [](/help/rtcdp/destinations/amazon-s3-destination.md) , [Amazon S](/help/rtcdp/destinations/amazon-kinesis-destination.md) , [Azureイベントハブ](/help/rtcdp/destinations/azure-event-hubs-destination.md) , SFTP宛先詳細は、Amazon S3 [,](/help/rtcdp/destinations/sftp-destination.md) Amazon S3 **, Azure Authentication Hubs** , and SSFTP宛先を参照してください。

   >[!NOTE]
   >
   >Adobe Real-time CDP は、認証プロセスでの資格情報の検証をサポートし、クラウドのストレージの場所に誤った資格情報が入力されるとエラーメッセージを表示します。これにより、間違った資格情報を使用してワークフローを完了できなくします。

   ![クラウドストレージの宛先 - 認証手順](/help/rtcdp/destinations/assets/cloud-destinations-authentication-step.png)

3. 「 **[!UICONTROL 設定]** 」( **[!UICONTROL Setup]** )手順で、アクティベーションフローの **[!UICONTROL 名前]** (Name)と説明(Description)を入力します。 <br>
また、この手順では、この宛先に適用する **[!UICONTROL マーケティングの使用例]** を選択できます。 マーケティングの使用例は、データがエクスポート先にエクスポートされる意図を示します。 アドビ定義のマーケティングの使用例から選択するか、独自のマーケティングの使用例を作成することができます。 マーケティングの使用例の詳細については、Real-time CDP [（リアルタイムCDP）ページの「](/help/rtcdp/privacy/data-governance-overview.md#destinations) Data Governance（データ・ガバナンス）」を参照してください。 アドビが定義した個々のマーケティングの使用例について詳しくは、 [データ使用ポリシーの概要を参照してください](/help/data-governance/policies/overview.md#core-actions)。 <br>
Amazon S3の宛先の場合は、ファイルが配信されるクラウドストレージーの宛先に **[!UICONTROL バケット名]** と **[!UICONTROL フォルダーパス]** を挿入します。 上記のフィールドに入力した後、「**[!UICONTROL 宛先を作成]**」を選択します。

   ![Amazon S3クラウドのストレージ先への接続 — 認証手順](/help/rtcdp/destinations/assets/amazon-s3-setup-step.png)

   SFTPの送信先の場合は、ファイルが配信される **[!UICONTROL フォルダーパス]** を挿入します。 上記のフィールドに入力した後、「**[!UICONTROL 宛先を作成]**」を選択します。

   ![SFTPクラウドのストレージ先に接続 — 認証手順](/help/rtcdp/destinations/assets/sftp-destinations-setup-step.png)

   Amazon Kinesisの宛先の場合は、ア [!DNL Amazon Kinesis] カウント内の既存のデータストリームの名前を指定します。 Adobe Real-time CDPは、このストリームにデータをエクスポートします。 上記のフィールドに入力した後、「**[!UICONTROL 宛先を作成]**」を選択します。

   ![Kinesisクラウドストレージの宛先への接続 — 認証手順](/help/rtcdp/destinations/assets/kinesis-destinations-setup-step.png)

   Azureイベントハブの宛先の場合は、ア [!DNL Amazon Kinesis] カウント内の既存のデータストリームの名前を指定します。 Adobe Real-time CDPは、このストリームにデータをエクスポートします。 上記のフィールドに入力した後、「**[!UICONTROL 宛先を作成]**」を選択します。

   ![Kinesisクラウドストレージの宛先への接続 — 認証手順](/help/rtcdp/destinations/assets/eventhubs-destinations-setup-step.png)

4. これで宛先が作成されました。後でセグメントをアクティブにする場合は、「**[!UICONTROL 保存して終了]**」を選択できます。または、「**[!UICONTROL 次へ]**」を選択してワークフローを続行し、アクティブ化するセグメントを選択することもできます。In either case, see the next section, [Activate segments](#activate-segments), for the rest of the workflow to export data.

## セグメントのアクティブ化 {#activate-segments}

セグメントのアクティベーションワークフローについて詳しくは、「[宛先へのプロファイルとセグメントのアクティブ化](/help/rtcdp/destinations/activate-destinations.md)」を参照してください。