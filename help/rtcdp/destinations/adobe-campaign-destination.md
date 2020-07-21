---
title: Adobe Campaign
seo-title: Adobe Campaign
description: Adobe Campaign は、オンラインおよびオフラインのすべてのチャネルにまたがるキャンペーンをカスタマイズし、実施するのに役立つソリューションセットです。
seo-description: Adobe Campaign は、オンラインおよびオフラインのすべてのチャネルにまたがるキャンペーンをカスタマイズし、実施するのに役立つソリューションセットです。
translation-type: tm+mt
source-git-commit: b96286f6a06f0583b45343a513ee64f0025d79a7
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 87%

---


# Adobe Campaign

## 概要

Adobe Campaign は、オンラインおよびオフラインのすべてのチャネルにまたがるキャンペーンをカスタマイズし、実施するのに役立つソリューションセットです。詳しくは、「[Adobe Campaign Classic について](https://docs.adobe.com/content/help/ja-JP/campaign-classic/using/getting-started/starting-with-adobe-campaign/about-adobe-campaign-classic.html)」を参照してください。

セグメントデータを Adobe Campaign に送信するには、まずアドビのリアルタイム顧客データプラットフォームで[宛先に接続](#connect-destination)して、ストレージの場所から Adobe Campaign に[データインポートを設定](#import-data-into-campaign)する必要があります。

## 宛先の接続 {#connect-destination}

1. **[!UICONTROL 接続／宛先]**&#x200B;で、「Adobe Campaign」を選択し、「**[!UICONTROL 宛先の接続]**」を選択します。

   ![Adobe Campaign への接続](/help/rtcdp/destinations/assets/connect-adobe-campaign.png)

1. In the Connect destination workflow, select the **[!UICONTROL Connection type]** for your storage location. Adobe Campaign の場合は、**[!UICONTROL Amazon S3]**、**[!UICONTROL SFTP（パスワード）]**、**[!UICONTROL SFTP（SSH キー）]**&#x200B;のいずれかを選択できます。接続タイプに応じて以下の情報を入力し、「**[!UICONTROL 接続]**」を選択します。

   ![Campaign ウィザードの設定](/help/rtcdp/destinations/assets/adobe-campaign-wizard.png)

   **[!UICONTROL Amazon S3]** 接続の場合は、アクセスキーIDとシークレットアクセスキーを指定する必要があります。
**[!UICONTROL SFTP（パスワード）]** で接続する場合は、ドメイン、ポート、ユーザー名、パスワードを指定する必要があります。
**[!UICONTROL SFTP（SSH キー）]** で接続する場合は、ドメイン、ポート、ユーザー名、SSH キーを指定する必要があります。

   ![Campaign 情報の入力](/help/rtcdp/destinations/assets/adobe-campaign-step2.png)

1. 「**[!UICONTROL 基本情報]**」で、目的の宛先に関する情報を次のように入力します。
   * **[!UICONTROL 名前]**：宛先の名前を選択します。
   * **[!UICONTROL 説明]**：宛先の説明を入力します。
   * **[!UICONTROL バケット名]**：*S3 接続用*。Real-time CDP が書き出しデータを CSV またはタブ区切りファイルとして収納する S3 バケットの場所を入力します。
   * **[!UICONTROL フォルダーパス]**：Real-time CDP が書き出しデータを CSV またはタブ区切りファイルとして格納するストレージの場所へのパスを指定します。
   * **[!UICONTROL ファイル形式]**：**CSV** または **TAB_DELIMITED**。ストレージの場所に書き出すファイル形式を選択します。

   ![Campaign の基本情報](/help/rtcdp/destinations/assets/adobe-campaign-basic-information.png)

1. 上のフィールドに入力したら、 **[!UICONTROL 「作成]** 」をクリックします。 これで宛先が接続され、宛先への[セグメントをアクティブ化](/help/rtcdp/destinations/activate-destinations.md)できます。

## 宛先属性 {#destination-attributes}

Adobe Campaign の宛先に対して[セグメントをアクティブ化する](/help/rtcdp/destinations/activate-destinations.md)場合は、[ユニオンスキーマー](../../profile/home.md#profile-fragments-and-union-schemas)から一意の識別子を選択することをお勧めします。宛先に書き出す一意の識別子およびその他の XDM フィールドを選択します。詳しくは、「電子メールマーケティングの宛先」の「[書き出したファイルの宛先属性として使用するスキーマフィールドの選択](/help/rtcdp/destinations/email-marketing-destinations.md#destination-attributes)」を参照してください。


## Adobe Campaign へのデータインポートの設定 {#import-data-into-campaign}

After connecting Real-time CDP to your [!DNL Amazon S3] or SFTP storage, you must set up the data import from your storage location into Adobe Campaign. これをおこなう方法については、Adobe Campaign ヘルプドキュメントの「[データのインポート](https://docs.adobe.com/content/help/en/campaign-classic/using/automating-with-workflows/general-operation/importing-data.html)」を参照してください。