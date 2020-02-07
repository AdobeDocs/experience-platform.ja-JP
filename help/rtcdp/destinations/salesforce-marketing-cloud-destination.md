---
title: Salesforce Marketing Cloud
seo-title: Salesforce Marketing Cloud
description: Salesforce Marketing cloudは、訪問者や顧客がエクスペリエンスをパーソナライズするためのジャーニーを構築し、カスタマイズできる、以前のExactTargetと呼ばれるデジタルマーケティングスイートです。
seo-description: Salesforce Marketing cloudは、訪問者や顧客がエクスペリエンスをパーソナライズするためのジャーニーを構築し、カスタマイズできる、以前のExactTargetと呼ばれるデジタルマーケティングスイートです。
translation-type: tm+mt
source-git-commit: 3b9584cca8943c52bb3d8e4512d327d3dbeb9e04

---


# Salesforce Marketing Cloud

## 概要

[Salesforce Marketing cloudは、訪問者と顧客がエクスペリエンスをパーソナライズするためのジャーニーを構築およびカスタマイズできる、以前のExactTargetと呼ばれるデジタルマーケティングスイートです。](https://www.salesforce.com/products/marketing-cloud/email-marketing/)

セグメントデータをSalesforce Marketing cloudに送信するには、まずAdobe Real-time [CDPの宛先に接続し、次にストレージの場所からSalesforce Marketing cloudへのデータインポートを設定する](#connect-destination)[](#import-data-into-salesforce) 必要があります。

## 宛先に接続 {#connect-destination}

1. 接続/ **[!UICONTROL 宛先で]**、「Salesforce Marketing Cloud」を選択し、「宛先に接続」を **[!UICONTROL 選択します]**。

   ![Salesforceに接続](/help/rtcdp/destinations/assets/connect-salesforce.png)

1. [接続先]ウィザードで、保存場所の **[!UICONTROL 接続の種類]** を選択します。 Salesforce Marketing cloudの場合、「 **SFTP with Password** 」と「 **SSH Key」で「SFTP」を選択できます**。 接続タイプに応じて、以下の情報を入力し、「接続」を選択 **[!UICONTROL します]**。

   ![Salesforceウィザードの設定](/help/rtcdp/destinations/assets/salesforce-step1.png)

   パスワ **ード接続を使用するSFTPの場合は** 、ドメイン、ポート、ユーザー名およびパスワードを指定する必要があります。
SSHキー **接続を使用するSFTPの場合は** 、ドメイン、ポート、ユーザー名、SSHキーを指定する必要があります。

   ![Salesforce情報の入力](/help/rtcdp/destinations/assets/salesforce-wizard.png)

1. 「基 **本情報**」で、目的の宛先に関する情報を次のように入力します。
   * **名前**:目的の名前を選択します。
   * **説明**:宛先の説明を入力します。
   * **Folder Path**:リアルタイムCDPがエクスポートデータをCSVまたはタブ区切りファイルとしてデポジットするストレージの場所にパスを指定します。
   * **File Format**: **CSV** 、 **TAB_DELIMITED**。 保存場所に書き出すファイル形式を選択します。
   ![Salesforce基本情報](/help/rtcdp/destinations/assets/salesforce-basic-information.png)

1. 「基本情 **報** 」のフィールドに入力した後、「作成」を **クリックします**。 これで宛先が接続され、宛先へのセグメ [ントをアクティブ](/help/rtcdp/destinations/activate-destinations.md) にできます。

## 宛先属性 {#destination-attributes}

Salesforce Marketing cloud宛 [先に対してセグメントをアクティブ化する場合は](/help/rtcdp/destinations/activate-destinations.md) 、ユニオンスキーマから一意の識別子を選択することをお勧 [めします](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!api-specification/markdown/narrative/technical_overview/unified_profile_architectural_overview/unified_profile_architectural_overview.md)。 宛先に書き出す一意の識別子およびその他のXDMフィールドを選択します。 詳しくは、「電子メールマーケテ [ィングの宛先」で、書き出したファイルの宛先属性として使用するスキーマフィールドの選択](/help/rtcdp/destinations/email-marketing-destinations.md#destination-attributes) 」を参照してください。

## Salesforce Marketing cloudへのデータインポートの設定 {#import-data-into-salesforce}

Real-time CDPをAmazon S3またはSFTPストレージに接続した後、ストレージの場所からSalesforce Marketing cloudへのデータインポートを設定する必要があります。 これを達成する方法については、Salesforceヘルプセン [ターのファイルからMarketing cloudへの購読者のインポート](https://help.salesforce.com/articleView?id=mc_es_import_subscribers_from_file.htm&type=5) （英語のみ）を参照してください。