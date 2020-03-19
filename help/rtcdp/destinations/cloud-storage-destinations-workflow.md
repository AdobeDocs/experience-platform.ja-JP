---
title: クラウドストレージの宛先のワークフロー
seo-title: クラウドストレージの宛先のワークフロー
description: クラウドストレージの場所への接続手順
seo-description: クラウドストレージの場所への接続手順
translation-type: tm+mt
source-git-commit: 225f18bd0d092bdae7b4cc99c278e850be9cfd56

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

3. 設定手順で **** 、アクティブ化フローの **[!UICONTROL Name]** （名前）とDestination **[!UICONTROL （保存先）を入力し、ファイルを配信するクラウドの保存先に]****** Folder path（フォルダパス）を挿入します。 上記のフ **[!UICONTROL ィールドに入力し]** 、「宛先を作成」を選択します。

   ![クラウドストレージの接続先 — 認証手順](/help/rtcdp/destinations/assets/cloud-destinations-setup-step.png)

4. これで宛先が作成されます。 後でセグメントをア **[!UICONTROL クティブにする場合は]** 、「保存して終了」を選択できます。または、「次へ **** 」を選択してワークフローを続行し、アクティブにするセグメントを選択することもできます。 いずれの場合も、残りのワークフローについては、次の「 [セグメントのアクティブ化](#activate-segments)」の節を参照してください。

## セグメントのアクティブ化 {#activate-segments}

セグメン [トのアクティブ化ワークフローについて詳しくは](/help/rtcdp/destinations/activate-destinations.md) 、「宛先へのプロファイルとセグメントのアクティブ化」を参照してください。