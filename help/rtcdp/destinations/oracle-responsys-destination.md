---
title: Oracle Responsys宛先
seo-title: Oracle Responsys宛先
description: Responsysは、Oracleが電子メール、モバイル、表示およびソーシャル全体でインタラクションをパーソナライズするために提供する、クロスチャネルマーケティングキャンペーン用のエンタープライズ電子メールマーケティングツールです。
seo-description: Responsysは、Oracleが電子メール、モバイル、表示およびソーシャル全体でインタラクションをパーソナライズするために提供する、クロスチャネルマーケティングキャンペーン用のエンタープライズ電子メールマーケティングツールです。
translation-type: tm+mt
source-git-commit: 3b9584cca8943c52bb3d8e4512d327d3dbeb9e04

---


# Oracle Responsys

## 概要

[Responsysは](https://www.oracle.com/marketingcloud/products/cross-channel-orchestration/) 、Oracleが提供するクロスチャネルマーケティングキャンペーン用のエンタープライズ電子メールマーケティングツールで、電子メール、モバイル、表示およびソーシャルでのインタラクションをパーソナライズします。

セグメント・データをOracle Responsysに送信するには、まず [Adobe Real-time Customer Data Platformの宛先に接続し、次に、ストレージの場所からOracle Responsysにデータ・インポ](#connect-destination) ートを設定する必要があります [](#import-data-into-responsys) 。

## 宛先に接続 {#connect-destination}

1. 「接 **[!UICONTROL 続」>「宛先]**」で「Oracle Responsys」を選択し、「宛先の接続」を **[!UICONTROL 選択します]**。

   ![Responsysに接続](/help/rtcdp/destinations/assets/connect-oracle-responsys.png)

1. [接続先]ウィザードで、保存場所の **[!UICONTROL 接続の種類]** を選択します。 Oracle Responsysの場合は、「 **SFTP with Password** 」と「 **SSH Key」で「SFTP」を選択できます**。 接続タイプに応じて、以下の情報を入力し、「接続」を選択 **[!UICONTROL します]**。

   ![「応答システムの設定」ウィザード](/help/rtcdp/destinations/assets/responsys-wizard.png)

   パスワ **ード接続を使用するSFTPの場合は** 、ドメイン、ポート、ユーザー名およびパスワードを指定する必要があります。
SSHキー **接続を使用するSFTPの場合は** 、ドメイン、ポート、ユーザー名、SSHキーを指定する必要があります。

   ![Responsys情報の入力](/help/rtcdp/destinations/assets/responsys-step2.png)

1. 「基 **本情報**」で、目的の宛先に関する情報を次のように入力します。
   * **名前**:目的の名前を選択します。
   * **説明**:宛先の説明を入力します。
   * **Folder Path**:リアルタイムCDPがエクスポートデータをCSVまたはタブ区切りファイルとしてデポジットするストレージの場所にパスを指定します。
   * **File Format**: **CSV** 、 **TAB_DELIMITED**。 保存場所に書き出すファイル形式を選択します。
   ![Responsys基本情報](/help/rtcdp/destinations/assets/responsys-basic-information.png)

1. 「基本情 **報** 」のフィールドに入力した後、「作成」を **クリックします**。 これで宛先が接続され、宛先へのセグメ [ントをアクティブ](/help/rtcdp/destinations/activate-destinations.md) にできます。

## 宛先属性 {#destination-attributes}

Oracle Responsys宛 [先に対して](/help/rtcdp/destinations/activate-destinations.md) 、セグメントをアクティブ化する場合は、ユニオン・スキーマから一意の識別子を選択することをお [勧めします](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!api-specification/markdown/narrative/technical_overview/unified_profile_architectural_overview/unified_profile_architectural_overview.md)。 宛先に書き出す一意の識別子およびその他のXDMフィールドを選択します。 詳しくは、「電子メールマーケテ [ィングの宛先」で、書き出したファイルの宛先属性として使用するスキーマフィールドの選択](/help/rtcdp/destinations/email-marketing-destinations.md#destination-attributes) 」を参照してください。

## Oracle Responsysへのデータ・インポートの設定 {#import-data-into-responsys}

Real-time CDPをAmazon S3またはSFTPストレージに接続した後、ストレージの場所からOracle Responsysへのデータインポートを設定する必要があります。 この達成方法は、Oracle Responsysヘルプ・センタ [ーの](https://docs.oracle.com/cloud/latest/marketingcs_gs/OMCEA/Connect_WizardUpload.htm) 「連絡先または勘定科目のインポート」を参照してください。