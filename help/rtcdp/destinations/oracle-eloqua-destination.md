---
title: Oracle Eloqua の宛先
seo-title: Oracle Eloqua の宛先
description: Oracle Eloqua は、Oracle が提供するマーケティング自動処理向けの SaaS（サービスとしてのソフトウェア）プラットフォームで、B2B マーケターや組織がマーケティングキャンペーンや販売リードジェネレーションを管理するのを支援します。
seo-description: Oracle Eloqua は、Oracle が提供するマーケティング自動処理向けの SaaS（サービスとしてのソフトウェア）プラットフォームで、B2B マーケターや組織がマーケティングキャンペーンや販売リードジェネレーションを管理するのを支援します。
translation-type: tm+mt
source-git-commit: 6f680a60c88bc5fee6ce9cb5a4f314c4b9d02249
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 76%

---


# [!DNL Oracle Eloqua]

## 概要

[Eloqua は](https://www.oracle.com/marketingcloud/products/marketing-automation/)[!DNL Oracle]、 が提供するマーケティング自動処理向けの SaaS（サービスとしてのソフトウェア）プラットフォームで、B2B マーケターや組織がマーケティングキャンペーンや販売リードジェネレーションを管理するのを支援します。

To send segment data to [!DNL Oracle Eloqua], you must first [connect the destination](#connect-destination) in Adobe Real-time Customer Data Platform, and then [set up a data import](#import-data-into-eloqua) from your storage location into [!DNL Oracle Eloqua].

## 宛先に接続 {#connect-destination}

1. In **[!UICONTROL Connections > Destinations]**, select [!DNL Oracle Eloqua], then select **[!UICONTROL Connect destination]**.

   ![Eloqua に接続](/help/rtcdp/destinations/assets/connect-oracle-eloqua.png)

2. クラウドストレージの宛先への接続を既に設定している場合は、**[!UICONTROL 認証]**&#x200B;手順で「**[!UICONTROL 既存のアカウント]**」を選択し、既存の接続の 1 つを選択します。または、「**[!UICONTROL 新しいアカウント]**」を選択して、新しい接続を設定できます。アカウント認証資格情報を入力し、「**[!UICONTROL 宛先に接続]**」を選択します。For [!DNL Oracle Eloqua], you can select between **[!UICONTROL SFTP with Password]** and **[!UICONTROL SFTP with SSH Key]**. 接続タイプに応じて、以下の情報を入力し、「**[!UICONTROL 宛先への接続]**」を選択します。

   **[!UICONTROL SFTP（パスワード）]** で接続する場合は、ドメイン、ポート、ユーザー名、パスワードを指定する必要があります。
**[!UICONTROL SFTP（SSH キー）]** で接続する場合は、ドメイン、ポート、ユーザー名、SSH キーを指定する必要があります。

   ![Eloqua ウィザードの設定](/help/rtcdp/destinations/assets/eloqua-authentication.png)

3. 「**[!UICONTROL 設定]**」手順で、目的の宛先に関する情報を次のように入力します。
   * **[!UICONTROL 名前]**：宛先の名前を選択します。
   * **[!UICONTROL 説明]**：宛先の説明を入力します。
   * **[!UICONTROL フォルダーパス]**：リアルタイム CDP が書き出しデータを CSV またはタブ区切りファイルとして格納するストレージの場所へのパスを指定します。
   * **[!UICONTROL ファイル形式]**：**CSV** または **TAB_DELIMITED**。ストレージの場所に書き出すファイル形式を選択します。

   ![Eloqua の基本情報](/help/rtcdp/destinations/assets/eloqua-basic-information.png)

4. 上記のフィールドに入力した後、「**[!UICONTROL 宛先を作成]**」をクリックします。これで宛先が作成され、宛先への[セグメントをアクティブ化](/help/rtcdp/destinations/activate-destinations.md)できます。

## 宛先属性

When [activating segments](/help/rtcdp/destinations/activate-destinations.md) to the [!DNL Oracle Eloqua] destination, we recommend that you select a unique identifier from your [union schema](../../profile/home.md#profile-fragments-and-union-schemas). 宛先に書き出す一意の識別子およびその他の XDM フィールドを選択します。詳しくは、「電子メールマーケティングの宛先」の「[書き出したファイルの宛先属性として使用するスキーマフィールドの選択](/help/rtcdp/destinations/email-marketing-destinations.md#destination-attributes)」を参照してください。

## Set up data import into [!DNL Oracle Eloqua] {#import-data-into-eloqua}

After connecting Real-time CDP to your Amazon S3 or SFTP storage, you must set up the data import from your storage location into [!DNL Oracle Eloqua]. To learn how to accomplish this, see [Importing contacts or accounts](https://docs.oracle.com/cloud/latest/marketingcs_gs/OMCAA/Help/DataImportExport/Tasks/ImportingContactsOrAccounts.htm) in the [!DNL Oracle Eloqua Help Center].