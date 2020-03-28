---
title: Oracle Eloqua の宛先
seo-title: Oracle Eloqua の宛先
description: Oracle Eloqua は、Oracle が提供するマーケティング自動処理向けの SaaS（サービスとしてのソフトウェア）プラットフォームで、B2B マーケターや組織がマーケティングキャンペーンや販売リードジェネレーションを管理するのを支援します。
seo-description: Oracle Eloqua は、Oracle が提供するマーケティング自動処理向けの SaaS（サービスとしてのソフトウェア）プラットフォームで、B2B マーケターや組織がマーケティングキャンペーンや販売リードジェネレーションを管理するのを支援します。
translation-type: tm+mt
source-git-commit: 336aa90cf1e059a92a36dd0ef3222ef6a6f5123b

---


# Oracle Eloqua

## 概要

[Eloqua は](https://www.oracle.com/marketingcloud/products/marketing-automation/)、Oracle が提供するマーケティング自動処理向けの SaaS（サービスとしてのソフトウェア）プラットフォームで、B2B マーケターや組織がマーケティングキャンペーンや販売リードジェネレーションを管理するのを支援します。

セグメント・データを Oracle Eloqua に送信するには、まず、アドビリアルタイム顧客データプラットフォームで[宛先に接続](#connect-destination)してから、ストレージの場所から Oracle Eloqua に[データインポートを設定](#import-data-into-eloqua)する必要があります。

## 宛先に接続 {#connect-destination}

1. で、「 **[!UICONTROL Connections > Destinations]** Oracle Eloqua」を選択し、「」を選択しま **[!UICONTROL Connect destination]**&#x200B;す。

   ![Eloqua に接続](/help/rtcdp/destinations/assets/connect-oracle-eloqua.png)

2. クラウド **[!UICONTROL Authentication]** ストレージの宛先への接続を設定済みの場合は、手順で、既存の接続の1 **[!UICONTROL Existing Account]** つを選択して選択します。 または、新しい接続を設 **[!UICONTROL New Account]** 定するように選択できます。 アカウント認証資格情報を入力し、を選択しま **[!UICONTROL Connect to destination]**&#x200B;す。 Oracle Eloquaの場合は、とから選択でき **[!UICONTROL SFTP with Password]** ます **[!UICONTROL SFTP with SSH Key]**。 Fill in the information below, depending on your connection type, and select **[!UICONTROL Connect to destination]**.

   For **[!UICONTROL SFTP with Password]** connections, you must provide Domain, Port, Username, and Password.
接続の **[!UICONTROL SFTP with SSH Key]** 場合は、ドメイン、ポート、ユーザー名、SSHキーを指定する必要があります。

   ![Eloqua ウィザードの設定](/help/rtcdp/destinations/assets/eloqua-authentication.png)

3. In the **[!UICONTROL Setup]** step, fill in the relevant information for your destination as shown below:
   * **[!UICONTROL Name]**:目的の名前を選択します。
   * **[!UICONTROL Description]**:宛先の説明を入力します。
   * **[!UICONTROL Folder Path]**:リアルタイムCDPがエクスポートデータをCSVまたはタブ区切りのファイルとしてデポジットするストレージの場所のパスを指定します。
   * **[!UICONTROL File Format]**: **CSV** 、 **TAB_DELIMITED**。 ストレージの場所に書き出すファイル形式を選択します。
   ![Eloqua の基本情報](/help/rtcdp/destinations/assets/eloqua-basic-information.png)

4. 上記のフ **[!UICONTROL Create destination]** ィールドに入力した後、をクリックします。 Your destination is now created and you can [activate segments](/help/rtcdp/destinations/activate-destinations.md) to the destination.

## 宛先属性

Oracle Eloqua の宛先に対して[セグメントをアクティブ化](/help/rtcdp/destinations/activate-destinations.md)する場合は、[ユニオンスキーマ](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!api-specification/markdown/narrative/technical_overview/unified_profile_architectural_overview/unified_profile_architectural_overview.md)から一意の識別子を選択することをお勧めします。宛先に書き出す一意の識別子およびその他の XDM フィールドを選択します。詳しくは、「電子メールマーケティングの宛先」の「[書き出したファイルの宛先属性として使用するスキーマフィールドの選択](/help/rtcdp/destinations/email-marketing-destinations.md#destination-attributes)」を参照してください。

## Oracle Eloqua へのデータインポートの設定 {#import-data-into-eloqua}

Real-time CDP を Amazon S3 または SFTP ストレージに接続した後、ストレージの場所から Oracle Eloqua へのデータインポートを設定する必要があります。これをおこなう方法については、Oracle Eloqua ヘルプセンターの「[連絡先またはアカウントのインポート](https://docs.oracle.com/cloud/latest/marketingcs_gs/OMCAA/Help/DataImportExport/Tasks/ImportingContactsOrAccounts.htm)」を参照してください。