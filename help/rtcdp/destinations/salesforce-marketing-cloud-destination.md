---
title: Salesforce Marketing Cloud
seo-title: Salesforce Marketing Cloud
description: Salesforce Marketing Cloud（旧称 ExactTarget）は、訪問者や顧客がエクスペリエンスをパーソナライズするためのジャーニーを構築し、カスタマイズできるデジタルマーケティングスイートです。
seo-description: Salesforce Marketing Cloud（旧称 ExactTarget）は、訪問者や顧客がエクスペリエンスをパーソナライズするためのジャーニーを構築し、カスタマイズできるデジタルマーケティングスイートです。
translation-type: tm+mt
source-git-commit: 50e6b39c1eb0bda4f3b30991515fb1c13fa9ff87

---


# Salesforce Marketing Cloud

## 概要

[Salesforce Marketing Cloud は、訪問者や顧客がエクスペリエンスをパーソナライズするためのジャーニーを構築し、カスタマイズできるデジタルマーケティングスイートです（旧称 ExactTarget）。](https://www.salesforce.com/products/marketing-cloud/email-marketing/)

セグメントデータを Salesforce Marketing Cloud に送信するには、まず Adobe Real-time CDP で[宛先に接続](#connect-destination)して、ストレージの場所から Salesforce Marketing Cloud への[データインポートを設定する](#import-data-into-salesforce)必要があります。

## 宛先の接続 {#connect-destination}

1. **[!UICONTROL 接続／宛先]**&#x200B;で、「Salesforce Marketing Cloud」を選択し、「**[!UICONTROL 宛先の接続]**」を選択します。

   ![Salesforce への接続](/help/rtcdp/destinations/assets/connect-salesforce.png)

2. In the **[!UICONTROL Authentication]** step, if you had previously set up a connection to your cloud storage destination, select **[!UICONTROL Existing Account]** and select one of your existing connections. または、「新規アカウント」を **[!UICONTROL 選択して]** 、新しい接続を設定できます。 アカウント認証資格情報を入力し、「**[!UICONTROL 宛先に接続]**」を選択します。Salesforce Marketing Cloud の場合は、「**[!UICONTROL SFTP（パスワード）]**」と「**[!UICONTROL SFTP（SSH キー）]**」を選択できます。Fill in the information below, depending on your connection type, and select **[!UICONTROL Connect to destination]**.

   **[!UICONTROL SFTP（パスワード）]** で接続する場合は、ドメイン、ポート、ユーザー名、パスワードを指定する必要があります。
**[!UICONTROL SFTP（SSH キー）]** で接続する場合は、ドメイン、ポート、ユーザー名、SSH キーを指定する必要があります。

   ![Salesforce 情報の入力](/help/rtcdp/destinations/assets/salesforce-authenticate.png)

3. In the **[!UICONTROL Setup]** step, fill in the relevant information for your destination as shown below:
   * **[!UICONTROL 名前]**：宛先の名前を選択します。
   * **[!UICONTROL 説明]**：宛先の説明を入力します。
   * **[!UICONTROL フォルダーパス]**：Real-time CDP が書き出しデータを CSV またはタブ区切りファイルとして格納するストレージの場所へのパスを指定します。
   * **[!UICONTROL ファイル形式]**：**[!UICONTROL CSV]** または **[!UICONTROL TAB_DELIMITED]**。ストレージの場所に書き出すファイル形式を選択します。
   ![Salesforce の基本情報](/help/rtcdp/destinations/assets/salesforce-basic-information.png)

4. 上記のフ **[!UICONTROL ィールドに入力し]** 、「宛先を作成」をクリックします。 これで宛先が接続され、宛先への[セグメントをアクティブ化](/help/rtcdp/destinations/activate-destinations.md)できます。

## 宛先属性 {#destination-attributes}

Salesforce Marketing Cloud の宛先に対して[セグメントをアクティブ化する](/help/rtcdp/destinations/activate-destinations.md)場合は、[ユニオンスキーマ](../../profile/home.md#profile-fragments-and-union-schemas)から一意の識別子を選択することをお勧めします。宛先に書き出す一意の識別子およびその他の XDM フィールドを選択します。詳しくは、「電子メールマーケティングの宛先」の「[書き出したファイルの宛先属性として使用するスキーマフィールドの選択](/help/rtcdp/destinations/email-marketing-destinations.md#destination-attributes)」を参照してください。

## Salesforce Marketing Cloud へのデータインポートの設定 {#import-data-into-salesforce}

Real-time CDP を Amazon S3 または SFTP ストレージに接続した後、ストレージの場所から Salesforce Marketing Cloud へのデータインポートを設定する必要があります。これを達成する方法については、Salesforce ヘルプセンターの「[ファイルから Marketing Cloud に購読者をインポートする方法](https://help.salesforce.com/articleView?id=mc_es_import_subscribers_from_file.htm&amp;type=5)」を参照してください。