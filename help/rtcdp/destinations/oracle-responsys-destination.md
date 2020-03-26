---
title: Oracle Responsys の宛先
seo-title: Oracle Responsys の宛先
description: Responsys は、Oracle が提供するクロスチャネルマーケティングキャンペーン用の大規模法人向け電子メールマーケティングツールで、メール、モバイル、ディスプレイ、ソーシャルでのインタラクションをパーソナライズします。
seo-description: Responsys は、Oracle が提供するクロスチャネルマーケティングキャンペーン用の大規模法人向け電子メールマーケティングツールで、メール、モバイル、ディスプレイ、ソーシャルでのインタラクションをパーソナライズします。
translation-type: tm+mt
source-git-commit: c3fe5753fb23f99076f9c85b4e07af2d25a121a9

---


# Oracle Responsys

## 概要

[Responsys](https://www.oracle.com/marketingcloud/products/cross-channel-orchestration/) は、Oracle が提供するクロスチャネルマーケティングキャンペーン用の大規模法人向け電子メールマーケティングツールで、電子メール、モバイル、表示およびソーシャルでのインタラクションをパーソナライズします。

To send segment data to Oracle Responsys, you must first [connect to the destination](#connect-destination) in Adobe Real-time Customer Data Platform, and then [set up a data import](#import-data-into-responsys) from your storage location into Oracle Responsys.

## 宛先の接続 {#connect-destination}

1. で、「Oracle **[!UICONTROL Connections > Destinations]** Responsys」を選択し、を選択しま **[!UICONTROL Connect destination]**&#x200B;す。

   ![Responsys に接続](/help/rtcdp/destinations/assets/connect-oracle-responsys.png)

2. クラウドス **トレージ** の宛先への接続を設定済みの場合は、認証手順で、既存の接続の1 **[!UICONTROL Existing Account]** つを選択します。 または、新しい接続を設 **[!UICONTROL New Account]** 定するように選択できます。 アカウント認証資格情報を入力し、を選択しま **[!UICONTROL Connect to destination]**&#x200B;す。 Oracle Responsys の場合は、「**SFTP（パスワード）**」と「**SFTP（SSH キー）**」を選択できます。Fill in the information below, depending on your connection type, and select **[!UICONTROL Connect to destination]**.

   **SFTP（パスワード）** で接続する場合は、ドメイン、ポート、ユーザー名、パスワードを指定する必要があります。
**SFTP（SSH キー）** で接続する場合は、ドメイン、ポート、ユーザー名、SSH キーを指定する必要があります。

   ![Responsys 情報の入力](/help/rtcdp/destinations/assets/responsys-authentication.png)

3. In the **Setup** step, fill in the relevant information for your destination as shown below:
   * **名前**：宛先の名前を選択します。
   * **説明**：宛先の説明を入力します。
   * **フォルダーパス**：Real-time CDP が書き出しデータを CSV またはタブ区切りファイルとして格納するストレージの場所へのパスを指定します。
   * **ファイル形式**：**CSV** または **TAB_DELIMITED**。ストレージの場所に書き出すファイル形式を選択します。
   ![Responsys 基本情報](/help/rtcdp/destinations/assets/responsys-basic-information.png)

4. 上記のフ **ィールドに入力し** 、「宛先を作成」をクリックします。 これで宛先が接続され、宛先への[セグメントをアクティブ化](/help/rtcdp/destinations/activate-destinations.md)できます。

## 宛先属性 {#destination-attributes}

Oracle Responsys の宛先に対して[セグメントをアクティブ化](/help/rtcdp/destinations/activate-destinations.md)する場合は、[ユニオンスキーマ](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!api-specification/markdown/narrative/technical_overview/unified_profile_architectural_overview/unified_profile_architectural_overview.md)から一意の識別子を選択することをお勧めします。宛先に書き出す一意の識別子およびその他の XDM フィールドを選択します。詳しくは、「電子メールマーケティングの宛先」の「[書き出したファイルの宛先属性として使用するスキーマフィールドの選択](/help/rtcdp/destinations/email-marketing-destinations.md#destination-attributes)」を参照してください。

## Oracle Responsys へのデータインポートの設定 {#import-data-into-responsys}

Real-time CDP を Amazon S3 または SFTP ストレージに接続した後、ストレージの場所から Oracle Responsys へのデータインポートを設定する必要があります。これをおこなう方法については、Oracle Responsys ヘルプセンターの 「[連絡先またはアカウントのインポート](https://docs.oracle.com/cloud/latest/marketingcs_gs/OMCEA/Connect_WizardUpload.htm)」を参照してください。