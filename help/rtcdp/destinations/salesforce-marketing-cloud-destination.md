---
title: Salesforce Marketing Cloud
seo-title: Salesforce Marketing Cloud
description: Salesforce Marketing Cloud（旧称 ExactTarget）は、訪問者や顧客がエクスペリエンスをパーソナライズするためのジャーニーを構築し、カスタマイズできるデジタルマーケティングスイートです。
seo-description: Salesforce Marketing Cloud（旧称 ExactTarget）は、訪問者や顧客がエクスペリエンスをパーソナライズするためのジャーニーを構築し、カスタマイズできるデジタルマーケティングスイートです。
translation-type: tm+mt
source-git-commit: 336aa90cf1e059a92a36dd0ef3222ef6a6f5123b

---


# Salesforce Marketing Cloud

## 概要

[Salesforce Marketing Cloud は、訪問者や顧客がエクスペリエンスをパーソナライズするためのジャーニーを構築し、カスタマイズできるデジタルマーケティングスイートです（旧称 ExactTarget）。](https://www.salesforce.com/products/marketing-cloud/email-marketing/)

セグメントデータを Salesforce Marketing Cloud に送信するには、まず Adobe Real-time CDP で[宛先に接続](#connect-destination)して、ストレージの場所から Salesforce Marketing Cloud への[データインポートを設定する](#import-data-into-salesforce)必要があります。

## 宛先の接続 {#connect-destination}

1. で、「Salesforce **[!UICONTROL Connections > Destinations]** Marketing Cloud」を選択し、「」を選択しま **[!UICONTROL Connect destination]**&#x200B;す。

   ![Salesforce への接続](/help/rtcdp/destinations/assets/connect-salesforce.png)

2. クラウド **[!UICONTROL Authentication]** ストレージの宛先への接続を設定済みの場合は、手順で、既存の接続の1 **[!UICONTROL Existing Account]** つを選択して選択します。 または、新しい接続を設 **[!UICONTROL New Account]** 定するように選択できます。 アカウント認証資格情報を入力し、を選択しま **[!UICONTROL Connect to destination]**&#x200B;す。 Salesforce Marketing Cloudの場合は、とから選択でき **[!UICONTROL SFTP with Password]** ます **[!UICONTROL SFTP with SSH Key]**。 Fill in the information below, depending on your connection type, and select **[!UICONTROL Connect to destination]**.

   For **[!UICONTROL SFTP with Password]** connections, you must provide Domain, Port, Username, and Password.
接続の **[!UICONTROL SFTP with SSH Key]** 場合は、ドメイン、ポート、ユーザー名、SSHキーを指定する必要があります。

   ![Salesforce 情報の入力](/help/rtcdp/destinations/assets/salesforce-authenticate.png)

3. In the **[!UICONTROL Setup]** step, fill in the relevant information for your destination as shown below:
   * **[!UICONTROL Name]**:目的の名前を選択します。
   * **[!UICONTROL Description]**:宛先の説明を入力します。
   * **[!UICONTROL Folder Path]**:リアルタイムCDPがエクスポートデータをCSVまたはタブ区切りのファイルとしてデポジットするストレージの場所のパスを指定します。
   * **[!UICONTROL File Format]**: **[!UICONTROL CSV]** または **[!UICONTROL TAB_DELIMITED]**. ストレージの場所に書き出すファイル形式を選択します。
   ![Salesforce の基本情報](/help/rtcdp/destinations/assets/salesforce-basic-information.png)

4. 上記のフ **[!UICONTROL Create destination]** ィールドに入力した後、をクリックします。 これで宛先が接続され、宛先への[セグメントをアクティブ化](/help/rtcdp/destinations/activate-destinations.md)できます。

## 宛先属性 {#destination-attributes}

Salesforce Marketing Cloud の宛先に対して[セグメントをアクティブ化する](/help/rtcdp/destinations/activate-destinations.md)場合は、[ユニオンスキーマ](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!api-specification/markdown/narrative/technical_overview/unified_profile_architectural_overview/unified_profile_architectural_overview.md)から一意の識別子を選択することをお勧めします。宛先に書き出す一意の識別子およびその他の XDM フィールドを選択します。詳しくは、「電子メールマーケティングの宛先」の「[書き出したファイルの宛先属性として使用するスキーマフィールドの選択](/help/rtcdp/destinations/email-marketing-destinations.md#destination-attributes)」を参照してください。

## Salesforce Marketing Cloud へのデータインポートの設定 {#import-data-into-salesforce}

Real-time CDP を Amazon S3 または SFTP ストレージに接続した後、ストレージの場所から Salesforce Marketing Cloud へのデータインポートを設定する必要があります。これを達成する方法については、Salesforce ヘルプセンターの「[ファイルから Marketing Cloud に購読者をインポートする方法](https://help.salesforce.com/articleView?id=mc_es_import_subscribers_from_file.htm&type=5)」を参照してください。