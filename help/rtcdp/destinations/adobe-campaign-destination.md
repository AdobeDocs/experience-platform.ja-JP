---
title: Adobe Campaign
seo-title: Adobe Campaign
description: Adobe Campaign は、オンラインおよびオフラインのすべてのチャネルにまたがるキャンペーンをカスタマイズし、実施するのに役立つソリューションセットです。
seo-description: Adobe Campaign は、オンラインおよびオフラインのすべてのチャネルにまたがるキャンペーンをカスタマイズし、実施するのに役立つソリューションセットです。
translation-type: tm+mt
source-git-commit: 336aa90cf1e059a92a36dd0ef3222ef6a6f5123b

---


# Adobe Campaign

## 概要

Adobe Campaign は、オンラインおよびオフラインのすべてのチャネルにまたがるキャンペーンをカスタマイズし、実施するのに役立つソリューションセットです。詳しくは、「[Adobe Campaign Classic について](https://docs.adobe.com/content/help/en/campaign-classic/using/getting-started/starting-with-adobe-campaign/about-adobe-campaign-classic.html)」を参照してください。

セグメントデータを Adobe Campaign に送信するには、まずアドビのリアルタイム顧客データプラットフォームで[宛先に接続](#connect-destination)して、ストレージの場所から Adobe Campaign に[データインポートを設定](#import-data-into-campaign)する必要があります。

## 宛先の接続 {#connect-destination}

1. で、「Adobe **[!UICONTROL Connections > Destinations]** Campaign」を選択し、を選択しま **[!UICONTROL Connect destination]**&#x200B;す。

   ![Adobe Campaign への接続](/help/rtcdp/destinations/assets/connect-adobe-campaign.png)

1. In the Connect destination workflow, select the **[!UICONTROL Connection type]** for your storage location. Adobe Campaignの場合は、とから選択で **[!UICONTROL Amazon S3]**&#x200B;きま **[!UICONTROL SFTP with Password]** す **[!UICONTROL SFTP with SSH Key]**。 Fill in the information below, depending on your connection type, then select **[!UICONTROL Connect]**.

   ![Campaign ウィザードの設定](/help/rtcdp/destinations/assets/adobe-campaign-wizard.png)

   **[!UICONTROL Amazon S3]** 接続の場合、アクセスキー ID とシークレットアクセスキーを指定する必要があります。
For **[!UICONTROL SFTP with Password]** connections, you must provide Domain, Port, Username, and Password.
接続の **[!UICONTROL SFTP with SSH Key]** 場合は、ドメイン、ポート、ユーザー名、SSHキーを指定する必要があります。

   ![Campaign 情報の入力](/help/rtcdp/destinations/assets/adobe-campaign-step2.png)

1. In **[!UICONTROL Basic Information]**, fill in the relevant information for your destination, as shown below:
   * **[!UICONTROL Name]**:目的の名前を選択します。
   * **[!UICONTROL Description]**:宛先の説明を入力します。
   * **[!UICONTROL Bucket Name]**: *S3接続の場合*。 Real-time CDP が書き出しデータを CSV またはタブ区切りファイルとして収納する S3 バケットの場所を入力します。
   * **[!UICONTROL Folder Path]**:リアルタイムCDPがエクスポートデータをCSVまたはタブ区切りのファイルとしてデポジットするストレージの場所のパスを指定します。
   * **[!UICONTROL File Format]**: **CSV** 、 **TAB_DELIMITED**。 ストレージの場所に書き出すファイル形式を選択します。
   ![Campaign の基本情報](/help/rtcdp/destinations/assets/adobe-campaign-basic-information.png)

1. 上記のフ **[!UICONTROL Create]** ィールドに入力した後、をクリックします。 これで宛先が接続され、宛先への[セグメントをアクティブ化](/help/rtcdp/destinations/activate-destinations.md)できます。

## 宛先属性 {#destination-attributes}

Adobe Campaign の宛先に対して[セグメントをアクティブ化する](/help/rtcdp/destinations/activate-destinations.md)場合は、[ユニオンスキーマー](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!api-specification/markdown/narrative/technical_overview/unified_profile_architectural_overview/unified_profile_architectural_overview.md)から一意の識別子を選択することをお勧めします。宛先に書き出す一意の識別子およびその他の XDM フィールドを選択します。詳しくは、「電子メールマーケティングの宛先」の「[書き出したファイルの宛先属性として使用するスキーマフィールドの選択](/help/rtcdp/destinations/email-marketing-destinations.md#destination-attributes)」を参照してください。


## Adobe Campaign へのデータインポートの設定 {#import-data-into-campaign}

Real-time CDP を Amazon S3 または SFTP ストレージに接続した後、ストレージの場所から Adobe Campaign へのデータインポートを設定する必要があります。これをおこなう方法については、Adobe Campaign ヘルプドキュメントの「[データのインポート](https://docs.adobe.com/content/help/en/campaign-classic/using/automating-with-workflows/general-operation/importing-data.html)」を参照してください。