---
keywords: email;Email;e-mail;email destinations;adobe campaign;campaign
title: Adobe Campaign
seo-title: Adobe Campaign
description: Adobe Campaign は、オンラインおよびオフラインのすべてのチャネルにまたがるキャンペーンをカスタマイズし、実施するのに役立つソリューションセットです。
seo-description: Adobe Campaign は、オンラインおよびオフラインのすべてのチャネルにまたがるキャンペーンをカスタマイズし、実施するのに役立つソリューションセットです。
translation-type: tm+mt
source-git-commit: 0bb1622895b1e0f97fc47b5c61d456bc369746c8
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 60%

---


# Adobe Campaign

## 概要

Adobe Campaign は、オンラインおよびオフラインのすべてのチャネルにまたがるキャンペーンをカスタマイズし、実施するのに役立つソリューションセットです。詳しくは、「[Adobe Campaign Classic について](https://experienceleague.adobe.com/docs/campaign-classic/using/getting-started/starting-with-adobe-campaign/about-adobe-campaign-classic.html)」を参照してください。

セグメントデータを Campaign に送信するには、まずアドビのリアルタイム顧客データプラットフォームで[宛先に接続](#connect-destination)して、ストレージの場所から Adobe Campaign に[データインポートを設定](#import-data-into-campaign)する必要があります。

## 書き出しタイプ {#export-type}

**プロファイルベース** — セグメントのすべてのメンバーを、必要なスキーマフィールド(例：電子メールアドレス、電話番号、姓)。 [宛先アクティベーションワークフローの属性を選択画面で選択](../../ui/activate-destinations.md#select-attributes)。

## 宛先の接続 {#connect-destination}

In **[!UICONTROL Connections]** > **[!UICONTROL Destinations]**, select Adobe Campaign, then select **[!UICONTROL Connect destination]**.

![Adobe Campaign への接続](../../assets/catalog/email-marketing/adobe-campaign/catalog.png)

「宛先の接続」ワークフローで、ストレージの場所の&#x200B;**[!UICONTROL 接続タイプ]**&#x200B;を選択します。Adobe Campaign の場合は、**[!UICONTROL Amazon S3]**、**[!UICONTROL SFTP（パスワード）]**、**[!UICONTROL SFTP（SSH キー）]**&#x200B;のいずれかを選択できます。接続タイプに応じて以下の情報を入力し、「**[!UICONTROL 接続]**」を選択します。

![Campaign ウィザードの設定](../../assets/catalog/email-marketing/adobe-campaign/connection-type.png)

- **[!UICONTROL AmazonS3 接続]**&#x200B;の場合、アクセスキー ID とシークレットアクセスキーを指定する必要があります。
- **[!UICONTROL SFTP（パスワード）]** で接続する場合は、ドメイン、ポート、ユーザー名、パスワードを指定する必要があります。
- **[!UICONTROL SFTP（SSH キー）]** で接続する場合は、ドメイン、ポート、ユーザー名、SSH キーを指定する必要があります。

必要に応じて、RSA形式の公開鍵を添付し、PGP/GPGを使用した暗号化を **[!UICONTROL Key]** セクションのエクスポートファイルに追加できます。 この公開鍵は、Base64エンコードされた文字列 **として書き込む** 必要があります。

![Campaign 情報の入力](../../assets/catalog/email-marketing/adobe-campaign/account-info.png)

「**[!UICONTROL 基本情報]**」で、目的の宛先に関する情報を次のように入力します。
- **[!UICONTROL 名前]**：宛先の名前を選択します。
- **[!UICONTROL 説明]**：宛先の説明を入力します。
- **[!UICONTROL バケット名]**：*S3 接続用*。リアルタイム CDP が書き出しデータを CSV またはタブ区切りファイルとして収納する S3 バケットの場所を入力します。
- **[!UICONTROL フォルダーパス]**：リアルタイム CDP が書き出しデータを CSV またはタブ区切りファイルとして格納するストレージの場所へのパスを指定します。
- **[!UICONTROL ファイル形式]**：**CSV** または **TAB_DELIMITED**。ストレージの場所に書き出すファイル形式を選択します。

![Campaign の基本情報](../../assets/catalog/email-marketing/adobe-campaign/basic-information.png)

上記のフィールドに入力した後、「**[!UICONTROL 作成]**」をクリックします。これで宛先が接続され、宛先への[セグメントをアクティブ化](../../ui/activate-destinations.md)できます。

## セグメントのアクティブ化 {#activate-segments}

セグメントのアクティベーションワークフローについて詳しくは、「[宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-destinations.md)」を参照してください。

## 宛先属性 {#destination-attributes}

Adobe Campaign の宛先に対して[セグメントをアクティブ化する](../../ui/activate-destinations.md)場合は、[ユニオンスキーマー](../../../profile/home.md#profile-fragments-and-union-schemas)から一意の識別子を選択することをお勧めします。宛先に書き出す一意の識別子およびその他の XDM フィールドを選択します。For more information, see [Select which schema fields to use as destination attributes in your exported files](./overview.md#destination-attributes) in email marketing destinations documentation.

## 書き出されたデータ {#exported-data}

For [!DNL Adobe Campaign] destinations, Real-time CDP creates a tab-delimited `.txt` or `.csv` file in the storage location that you provided. これらのファイルについて詳しくは、セグメントアクティベーションチュートリアルの [電子メールマーケティングの宛先とクラウドストレージの宛先](../../ui/activate-destinations.md#esp-and-cloud-storage) （英語）を参照してください。

<!--

Expect a new file to be created in your storage location every day. The file format is:

`Adobe_Campaign_segment<segmentID>_<timestamp-yyyymmddhhmmss>.csv`

```
Adobe_Campaign_segment12341e18-abcd-49c2-836d-123c88e76c39_20200408061804.csv
Adobe_Campaign_segment12341e18-abcd-49c2-836d-123c88e76c39_20200409052200.csv
Adobe_Campaign_segment12341e18-abcd-49c2-836d-123c88e76c39_20200410061130.csv
```

The presence of these files in your storage location is confirmation of successful activation. To understand how the exported files are structured, you can [download a sample .csv file](/help/rtcdp/destinations/assets/sample_export_file_segment12341e18-abcd-49c2-836d-123c88e76c39_20200408061804.csv). This sample file includes the profile attributes `person.firstname`, `person.lastname`, `person.gender`, `person.birthyear`, and `personalEmail.address`.

-->

## Adobe Campaign へのデータインポートの設定 {#import-data-into-campaign}

>[!IMPORTANT]
>
>- この統合を実行する際は、Adobe Campaign契約に従って、SFTPストレージの制限、データベースストレージの制限およびアクティブなプロファイルの制限に注意してください。
>- ワークフローを使用して、Adobe Campaignでエクスポートしたセグメントのスケジュール、インポート、およびマッピングを行う必要があり [!DNL Campaign] ます。 Adobe Campaignドキュメントの [定期インポートの](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/general-operation/importing-data.html#automating-with-workflows) 設定を参照してください。



After connecting Real-time CDP to your [!DNL Amazon S3] or SFTP storage, you must set up the data import from your storage location into Adobe Campaign. To learn how to accomplish this, refer to [Importing data](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/general-operation/importing-data.html) in the Adobe Campaign documentation.