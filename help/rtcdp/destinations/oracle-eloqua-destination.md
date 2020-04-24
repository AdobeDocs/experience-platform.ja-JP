---
title: Oracle Eloqua の宛先
seo-title: Oracle Eloqua の宛先
description: Oracle Eloqua は、Oracle が提供するマーケティング自動処理向けの SaaS（サービスとしてのソフトウェア）プラットフォームで、B2B マーケターや組織がマーケティングキャンペーンや販売リードジェネレーションを管理するのを支援します。
seo-description: Oracle Eloqua は、Oracle が提供するマーケティング自動処理向けの SaaS（サービスとしてのソフトウェア）プラットフォームで、B2B マーケターや組織がマーケティングキャンペーンや販売リードジェネレーションを管理するのを支援します。
translation-type: tm+mt
source-git-commit: 50e6b39c1eb0bda4f3b30991515fb1c13fa9ff87

---


# Oracle Eloqua

## 概要

[Eloqua は](https://www.oracle.com/marketingcloud/products/marketing-automation/)、Oracle が提供するマーケティング自動処理向けの SaaS（サービスとしてのソフトウェア）プラットフォームで、B2B マーケターや組織がマーケティングキャンペーンや販売リードジェネレーションを管理するのを支援します。

セグメント・データを Oracle Eloqua に送信するには、まず、アドビリアルタイム顧客データプラットフォームで[宛先に接続](#connect-destination)してから、ストレージの場所から Oracle Eloqua に[データインポートを設定](#import-data-into-eloqua)する必要があります。

## 宛先に接続 {#connect-destination}

1. **[!UICONTROL 接続／宛先]**&#x200B;で、「Oracle Eloqua」を選択し、「**[!UICONTROL 宛先の接続]**」を選択します。

   ![Eloqua に接続](/help/rtcdp/destinations/assets/connect-oracle-eloqua.png)

2. In the **[!UICONTROL Authentication]** step, if you had previously set up a connection to your cloud storage destination, select **[!UICONTROL Existing Account]** and select one of your existing connections. または、「新規アカウント」を **[!UICONTROL 選択して]** 、新しい接続を設定できます。 アカウント認証資格情報を入力し、「**[!UICONTROL 宛先に接続]**」を選択します。Oracle Eloqua の場合は、「**[!UICONTROL SFTP（パスワード）]**」と「**[!UICONTROL SFTP（SSH キー）」]**&#x200B;のどちらかを選択できます。Fill in the information below, depending on your connection type, and select **[!UICONTROL Connect to destination]**.

   **[!UICONTROL SFTP（パスワード）]** で接続する場合は、ドメイン、ポート、ユーザー名、パスワードを指定する必要があります。
**[!UICONTROL SFTP（SSH キー）]** で接続する場合は、ドメイン、ポート、ユーザー名、SSH キーを指定する必要があります。

   ![Eloqua ウィザードの設定](/help/rtcdp/destinations/assets/eloqua-authentication.png)

3. In the **[!UICONTROL Setup]** step, fill in the relevant information for your destination as shown below:
   * **[!UICONTROL 名前]**：宛先の名前を選択します。
   * **[!UICONTROL 説明]**：宛先の説明を入力します。
   * **[!UICONTROL フォルダーパス]**：Real-time CDP が書き出しデータを CSV またはタブ区切りファイルとして格納するストレージの場所へのパスを指定します。
   * **[!UICONTROL ファイル形式]**：**CSV** または **TAB_DELIMITED**。ストレージの場所に書き出すファイル形式を選択します。
   ![Eloqua の基本情報](/help/rtcdp/destinations/assets/eloqua-basic-information.png)

4. 上記のフ **[!UICONTROL ィールドに入力し]** 、「宛先を作成」をクリックします。 Your destination is now created and you can [activate segments](/help/rtcdp/destinations/activate-destinations.md) to the destination.

## 宛先属性

Oracle Eloqua の宛先に対して[セグメントをアクティブ化](/help/rtcdp/destinations/activate-destinations.md)する場合は、[ユニオンスキーマ](../../profile/home.md#profile-fragments-and-union-schemas)から一意の識別子を選択することをお勧めします。宛先に書き出す一意の識別子およびその他の XDM フィールドを選択します。詳しくは、「電子メールマーケティングの宛先」の「[書き出したファイルの宛先属性として使用するスキーマフィールドの選択](/help/rtcdp/destinations/email-marketing-destinations.md#destination-attributes)」を参照してください。

## Oracle Eloqua へのデータインポートの設定 {#import-data-into-eloqua}

Real-time CDP を Amazon S3 または SFTP ストレージに接続した後、ストレージの場所から Oracle Eloqua へのデータインポートを設定する必要があります。これをおこなう方法については、Oracle Eloqua ヘルプセンターの「[連絡先またはアカウントのインポート](https://docs.oracle.com/cloud/latest/marketingcs_gs/OMCAA/Help/DataImportExport/Tasks/ImportingContactsOrAccounts.htm)」を参照してください。