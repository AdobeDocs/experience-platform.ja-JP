---
title: Oracle Eloqua の宛先
seo-title: Oracle Eloqua の宛先
description: Oracle Eloqua は、Oracle が提供するマーケティング自動処理向けの SaaS（サービスとしてのソフトウェア）プラットフォームで、B2B マーケターや組織がマーケティングキャンペーンや販売リードジェネレーションを管理するのを支援します。
seo-description: Oracle Eloqua は、Oracle が提供するマーケティング自動処理向けの SaaS（サービスとしてのソフトウェア）プラットフォームで、B2B マーケターや組織がマーケティングキャンペーンや販売リードジェネレーションを管理するのを支援します。
translation-type: ht
source-git-commit: 3b9584cca8943c52bb3d8e4512d327d3dbeb9e04

---


# Oracle Eloqua

## 概要

[Eloqua は](https://www.oracle.com/marketingcloud/products/marketing-automation/)、Oracle が提供するマーケティング自動処理向けの SaaS（サービスとしてのソフトウェア）プラットフォームで、B2B マーケターや組織がマーケティングキャンペーンや販売リードジェネレーションを管理するのを支援します。

セグメント・データを Oracle Eloqua に送信するには、まず、アドビリアルタイム顧客データプラットフォームで[宛先に接続](#connect-destination)してから、ストレージの場所から Oracle Eloqua に[データインポートを設定](#import-data-into-eloqua)する必要があります。

## 宛先に接続 {#connect-destination}

1. **[!UICONTROL 接続／宛先]**&#x200B;で、「Oracle Eloqua」を選択し、「**[!UICONTROL 宛先の接続]**」を選択します。

   ![Eloqua に接続](/help/rtcdp/destinations/assets/connect-oracle-eloqua.png)

1. 「宛先の接続」ウィザードで、ストレージの場所の&#x200B;**[!UICONTROL 接続タイプ]**&#x200B;を選択します。Oracle Eloqua の場合は、「**SFTP（パスワード）**」と「**SFTP（SSH キー）」**&#x200B;のどちらかを選択できます。接続タイプに応じて、以下の情報を入力し、「**[!UICONTROL 接続]**」を選択します。

   ![Eloqua ウィザードの設定](/help/rtcdp/destinations/assets/eloqua-wizard.png)

   **SFTP（パスワード）** で接続する場合は、ドメイン、ポート、ユーザー名、パスワードを指定する必要があります。
**SFTP（SSH キー）** で接続する場合は、ドメイン、ポート、ユーザー名、SSH キーを指定する必要があります。

   ![Eloqua 情報の入力](/help/rtcdp/destinations/assets/eloqua-step2.png)

1. 「**基本情報**」で、目的の宛先に関する情報を次のように入力します。
   * **名前**：宛先の名前を選択します。
   * **説明**：宛先の説明を入力します。
   * **フォルダーパス**：Real-time CDP が書き出しデータを CSV またはタブ区切りファイルとして格納するストレージの場所へのパスを指定します。
   * **ファイル形式**：**CSV** または **TAB_DELIMITED**。ストレージの場所に書き出すファイル形式を選択します。
   ![Eloqua の基本情報](/help/rtcdp/destinations/assets/eloqua-basic-information.png)

1. 「**基本情報**」フィールドに入力した後、「**作成**」をクリックします。これで宛先が接続され、宛先への[セグメントをアクティブ化](/help/rtcdp/destinations/activate-destinations.md)できます。

## 宛先属性

Oracle Eloqua の宛先に対して[セグメントをアクティブ化](/help/rtcdp/destinations/activate-destinations.md)する場合は、[ユニオンスキーマ](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!api-specification/markdown/narrative/technical_overview/unified_profile_architectural_overview/unified_profile_architectural_overview.md)から一意の識別子を選択することをお勧めします。宛先に書き出す一意の識別子およびその他の XDM フィールドを選択します。詳しくは、「電子メールマーケティングの宛先」の「[書き出したファイルの宛先属性として使用するスキーマフィールドの選択](/help/rtcdp/destinations/email-marketing-destinations.md#destination-attributes)」を参照してください。

## Oracle Eloqua へのデータインポートの設定 {#import-data-into-eloqua}

Real-time CDP を Amazon S3 または SFTP ストレージに接続した後、ストレージの場所から Oracle Eloqua へのデータインポートを設定する必要があります。これをおこなう方法については、Oracle Eloqua ヘルプセンターの「[連絡先またはアカウントのインポート](https://docs.oracle.com/cloud/latest/marketingcs_gs/OMCAA/Help/DataImportExport/Tasks/ImportingContactsOrAccounts.htm)」を参照してください。