---
title: Adobe Campaign
seo-title: Adobe Campaign
description: Adobe Campaignは、オンラインおよびオフラインのすべてのチャネルにわたってキャンペーンをパーソナライズし、配信するのに役立つ一連のソリューションです。
seo-description: Adobe Campaignは、オンラインおよびオフラインのすべてのチャネルにわたってキャンペーンをパーソナライズし、配信するのに役立つ一連のソリューションです。
translation-type: tm+mt
source-git-commit: 3b9584cca8943c52bb3d8e4512d327d3dbeb9e04

---


# Adobe Campaign

## 概要

Adobe Campaignは、オンラインおよびオフラインのすべてのチャネルにわたってキャンペーンをパーソナライズし、配信するのに役立つ一連のソリューションです。 詳しく [は、「Adobe Campaign Classicについて](https://docs.adobe.com/content/help/en/campaign-classic/using/getting-started/starting-with-adobe-campaign/about-adobe-campaign-classic.html) 」を参照してください。

セグメントデータをAdobe Campaignに送信するには、まずAdobe Real-time Customer Data [Platformで宛先に接続し、ストレージの場所からAdobe Campaignにデータインポ](#connect-destination) ートを設定する必要があります [](#import-data-into-campaign) 。

## 宛先に接続 {#connect-destination}

1. 接続/ **[!UICONTROL 宛先で]**、「Adobe Campaign」を選択し、「宛先に接続」を **[!UICONTROL 選択します]**。

   ![Adobe Campaignへの接続](/help/rtcdp/destinations/assets/connect-adobe-campaign.png)

1. [接続先]ウィザードで、保存場所の **[!UICONTROL 接続の種類]** を選択します。 Adobe Campaignの場合は、 **Amazon S3**、 **SFTP（パスワード付き）、** SSHキー付き **SFTPのいずれかを選択できます**。 接続タイプに応じて以下の情報を入力し、「接続」を選択し **[!UICONTROL ます]**。

   ![キャンペーンウィザードの設定](/help/rtcdp/destinations/assets/adobe-campaign-wizard.png)

   **S3接続の場合** 、アクセスキーIDとシークレットアクセスキーを指定する必要があります。
パスワ **ード接続を使用するSFTPの場合は** 、ドメイン、ポート、ユーザー名およびパスワードを指定する必要があります。
SSHキー **接続を使用するSFTPの場合は** 、ドメイン、ポート、ユーザー名、SSHキーを指定する必要があります。

   ![キャンペーン情報の入力](/help/rtcdp/destinations/assets/adobe-campaign-step2.png)

1. 「基 **本情報**」で、目的の宛先に関する情報を次のように入力します。
   * **名前**:目的の名前を選択します。
   * **説明**:宛先の説明を入力します。
   * **グループ名**: *S3接続用*。 リアルタイムCDPがエクスポートデータをCSVまたはタブ区切りファイルとしてデポジットするS3バケットの場所を入力します。
   * **Folder Path**:リアルタイムCDPがエクスポートデータをCSVまたはタブ区切りファイルとしてデポジットするストレージの場所にパスを指定します。
   * **File Format**: **CSV** 、 **TAB_DELIMITED**。 保存場所に書き出すファイル形式を選択します。
   ![キャンペーンの基本情報](/help/rtcdp/destinations/assets/adobe-campaign-basic-information.png)

1. 「基本情 **報** 」のフィールドに入力した後、「作成」を **クリックします**。 これで宛先が接続され、宛先へのセグメ [ントをアクティブ](/help/rtcdp/destinations/activate-destinations.md) にできます。

## 宛先属性 {#destination-attributes}

Adobe Campaignの [宛先に対してセグメントをアクティブ化する場合は](/help/rtcdp/destinations/activate-destinations.md) 、ユニオンスキーマから一意の識別子を選択することをお勧 [めします](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!api-specification/markdown/narrative/technical_overview/unified_profile_architectural_overview/unified_profile_architectural_overview.md)。 宛先に書き出す一意の識別子およびその他のXDMフィールドを選択します。 詳しくは、「電子メールマーケテ [ィングの宛先」で、書き出したファイルの宛先属性として使用するスキーマフィールドの選択](/help/rtcdp/destinations/email-marketing-destinations.md#destination-attributes) 」を参照してください。


## Adobe Campaignへのデータインポートの設定 {#import-data-into-campaign}

Real-time CDPをAmazon S3またはSFTPストレージに接続した後、ストレージの場所からAdobe Campaignへのデータインポートを設定する必要があります。 これを行う方法については、Adobe Campaignヘルプドキュ [メントの](https://docs.adobe.com/content/help/en/campaign-classic/using/automating-with-workflows/general-operation/importing-data.html) 「データのインポート」を参照してください。