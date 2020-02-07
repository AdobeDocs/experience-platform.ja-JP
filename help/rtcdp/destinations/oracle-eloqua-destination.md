---
title: Oracle Eloqua宛先
seo-title: Oracle Eloqua宛先
description: Oracle Eloquaは、B2Bマーケターや組織がマーケティングキャンペーンや販売リードジェネレーションを管理するのを支援する、Oracleが提供するマーケティング自動化向けのSaaS（サービスとしてのソフトウェア）プラットフォームです。
seo-description: Oracle Eloquaは、B2Bマーケターや組織がマーケティングキャンペーンや販売リードジェネレーションを管理するのを支援する、Oracleが提供するマーケティング自動化向けのSaaS（サービスとしてのソフトウェア）プラットフォームです。
translation-type: tm+mt
source-git-commit: 3b9584cca8943c52bb3d8e4512d327d3dbeb9e04

---


# Oracle Eloqua

## 概要

[Eloquaは](https://www.oracle.com/marketingcloud/products/marketing-automation/) 、Oracleが提供するマーケティング自動化向けのSaaS（サービスとしてのソフトウェア）プラットフォームで、B2Bマーケターや組織がマーケティングキャンペーンや販売リードジェネレーションを管理するのを支援します。

セグメント・データをOracle Eloquaに送信するには、まず [Adobe Real-time Customer Data Platformの宛先に接続し、次にストレージ場所からOracle Eloquaへのデータ・インポ](#connect-destination) ート [](#import-data-into-eloqua) を設定する必要があります。

## 宛先に接続 {#connect-destination}

1. 「接 **[!UICONTROL 続」>「宛先]**」で、「Oracle Eloqua」を選択し、「接続先」を **[!UICONTROL 選択します]**。

   ![Eloquaに接続](/help/rtcdp/destinations/assets/connect-oracle-eloqua.png)

1. [接続先]ウィザードで、保存場所の **[!UICONTROL 接続の種類]** を選択します。 Oracle Eloquaの場合は、「 **SFTP with Password** 」と「 **SSH Key」を使用した** SFTPのどちらかを選択できます。 接続タイプに応じて、以下の情報を入力し、「接続」を選択 **[!UICONTROL します]**。

   ![Eloquaウィザードの設定](/help/rtcdp/destinations/assets/eloqua-wizard.png)

   パスワ **ード接続を使用するSFTPの場合は** 、ドメイン、ポート、ユーザー名およびパスワードを指定する必要があります。
SSHキー **接続を使用するSFTPの場合は** 、ドメイン、ポート、ユーザー名、SSHキーを指定する必要があります。

   ![Eloqua情報の入力](/help/rtcdp/destinations/assets/eloqua-step2.png)

1. 「基 **本情報**」に、次に示す宛先に関連する情報を入力します。
   * **名前**:目的の名前を選択します。
   * **説明**:宛先の説明を入力します。
   * **Folder Path**:リアルタイムCDPがエクスポートデータをCSVまたはタブ区切りファイルとしてデポジットするストレージの場所にパスを指定します。
   * **File Format**: **CSV** 、 **TAB_DELIMITED**。 保存場所に書き出すファイル形式を選択します。
   ![Eloquaの基本情報](/help/rtcdp/destinations/assets/eloqua-basic-information.png)

1. 「基本情 **報** 」のフィールドに入力した後、「作成」を **クリックします**。 これで宛先が接続され、宛先へのセグメ [ントをアクティブ](/help/rtcdp/destinations/activate-destinations.md) にできます。

## 宛先属性

Oracle Eloqua宛 [先に対して](/help/rtcdp/destinations/activate-destinations.md) 、セグメントをアクティブ化する場合は、ユニオン・スキーマから一意の識別子を選択することを [お勧めします](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!api-specification/markdown/narrative/technical_overview/unified_profile_architectural_overview/unified_profile_architectural_overview.md)。 宛先に書き出す一意の識別子およびその他のXDMフィールドを選択します。 詳しくは、「電子メールマーケテ [ィングの宛先」で、書き出したファイルの宛先属性として使用するスキーマフィールドの選択](/help/rtcdp/destinations/email-marketing-destinations.md#destination-attributes) 」を参照してください。

## Oracle Eloquaへのデータ・インポートの設定 {#import-data-into-eloqua}

Real-time CDPをAmazon S3またはSFTPストレージに接続した後、ストレージの場所からOracle Eloquaへのデータインポートを設定する必要があります。 この達成方法は、『Oracle Eloquaヘルプ・センター [』の「連絡先またはアカウントの](https://docs.oracle.com/cloud/latest/marketingcs_gs/OMCAA/Help/DataImportExport/Tasks/ImportingContactsOrAccounts.htm) ・インポート」を参照してください。