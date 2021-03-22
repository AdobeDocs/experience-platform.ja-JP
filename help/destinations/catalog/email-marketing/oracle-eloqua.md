---
keywords: 電子メール；電子メール；電子メール；電子メールの送信先；oracleの検索；oracle
title: OracleEloqua接続
description: Oracle Eloqua は、Oracle が提供するマーケティング自動処理向けの SaaS（サービスとしてのソフトウェア）プラットフォームで、B2B マーケターや組織がマーケティングキャンペーンや販売リードジェネレーションを管理するのを支援します。
translation-type: tm+mt
source-git-commit: 709908196bb5df665c7e7df10dc58ee9f3b0edbf
workflow-type: tm+mt
source-wordcount: '629'
ht-degree: 49%

---


# [!DNL Oracle Eloqua] connection

## 概要 {#overview}

[[!DNL Oracle Eloqua] は](https://www.oracle.com/marketingcloud/products/marketing-automation/)[!DNL Oracle]、 が提供するマーケティング自動処理向けの SaaS（サービスとしてのソフトウェア）プラットフォームで、B2B マーケターや組織がマーケティングキャンペーンや販売リードジェネレーションを管理するのを支援します。

セグメントデータを[!DNL Oracle Eloqua]に送信するには、まずAdobe Experience Platformの宛先](#connect-destination)を[接続し、次に[ストレージの場所から[!DNL Oracle Eloqua]にデータインポート](#import-data-into-eloqua)を設定する必要があります。

## エクスポートの種類{#export-type}

**プロファイルベース**  — セグメントのすべてのメンバーを、必要なスキーマフィールド(例：電子メールアドレス、電話番号、姓)。 [宛先アクティベーションワークフローの属性を選択画面で選択](../../ui/activate-destinations.md#select-attributes)。

## 宛先に接続 {#connect-destination}

**[!UICONTROL 接続]**/**[!UICONTROL 宛先]**&#x200B;で、[!DNL Oracle Eloqua]を選択し、**[!UICONTROL 宛先]**&#x200B;を接続します。

[Eloqua に接続](../../assets/catalog/email-marketing/oracle-eloqua/catalog.png)

クラウドストレージの宛先への接続を既に設定している場合は、**[!UICONTROL 認証]**&#x200B;手順で「**[!UICONTROL 既存のアカウント]**」を選択し、既存の接続の 1 つを選択します。または、「**[!UICONTROL 新しいアカウント]**」を選択して、新しい接続を設定できます。アカウント認証資格情報を入力し、「**[!UICONTROL 宛先に接続]**」を選択します。[!DNL Oracle Eloqua]の場合は、**[!UICONTROL パスワード]**&#x200B;のSFTPと&#x200B;**[!UICONTROL SSHキー]**&#x200B;のSFTPのどちらかを選択できます。 接続タイプに応じて、以下の情報を入力し、「**[!UICONTROL 宛先への接続]**」を選択します。

**[!UICONTROL SFTP（パスワード）]** で接続する場合は、ドメイン、ポート、ユーザー名、パスワードを指定する必要があります。
**[!UICONTROL SFTP（SSH キー）]** で接続する場合は、ドメイン、ポート、ユーザー名、SSH キーを指定する必要があります。

![Eloqua ウィザードの設定](../../assets/catalog/email-marketing/oracle-eloqua/account-info.png)

「**[!UICONTROL 設定]**」手順で、目的の宛先に関する情報を次のように入力します。
- **[!UICONTROL 名前]**：宛先の名前を選択します。
- **[!UICONTROL 説明]**：宛先の説明を入力します。
- **[!UICONTROL グループ名]**:プラットフォームがデータエクスポートを預け入れる、AmazonS3バケット。入力する文字は3 ～ 63文字の長さにする必要があります。 先頭と末尾は文字または数字にする必要があります。 小文字、数字、ハイフン(-)のみを含める必要があります。 IPアドレス（192.100.1.1など）の形式を設定しないでください。
- **[!UICONTROL Folder Path]**:ストレージー上の場所に、PlatformがエクスポートデータをCSVまたはタブ区切りファイルとしてデポジットするパスを指定します。
- **[!UICONTROL ファイル形式]**：**CSV** または **TAB_DELIMITED**。ストレージの場所に書き出すファイル形式を選択します。
- **[!UICONTROL マーケティングアクション]**:マーケティングアクションは、データをエクスポート先にエクスポートする意図を示します。Adobe定義のマーケティングアクションから選択するか、独自のマーケティングアクションを作成することができます。 マーケティング活動の詳細については、「[Adobe Experience Platform](../../../data-governance/policies/overview.md)のデータガバナンス」ページを参照してください。 Adobe定義の個々のマーケティングアクションについて詳しくは、[データ使用ポリシーの概要](../../../data-governance/policies/overview.md)を参照してください。

![Eloqua の基本情報](../../assets/catalog/email-marketing/oracle-eloqua/basic-information.png)

上記のフィールドに入力した後、「**[!UICONTROL 宛先を作成]**」をクリックします。これで宛先が作成され、宛先への[セグメントをアクティブ化](../../ui/activate-destinations.md)できます。

## セグメントのアクティブ化 {#activate-segments}

セグメントのアクティベーションワークフローについて詳しくは、「[宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-destinations.md)」を参照してください。

## 宛先属性 {#destination-attributes}

[セグメント](../../ui/activate-destinations.md)を[!DNL Oracle Eloqua]宛先に対してアクティブ化する場合、[和集合スキーマ](../../../profile/home.md#profile-fragments-and-union-schemas)から一意の識別子を選択することをお勧めします。 宛先に書き出す一意の ID およびその他の XDM フィールドを選択します。詳しくは、「電子メールマーケティングの宛先」の「[書き出したファイルの宛先属性として使用するスキーマフィールドの選択](./overview.md#destination-attributes)」を参照してください。

## エクスポートされたデータ{#exported-data}

[!DNL Oracle Eloqua]宛先の場合、Platformは、指定したストレージーの場所にタブ区切りの`.txt`ファイルまたは`.csv`ファイルを作成します。 ファイルについて詳しくは、セグメントアクティベーションチュートリアルの「電子メールマーケティングの宛先とクラウドストレージの宛先」[を参照してください。](../../ui/activate-destinations.md#esp-and-cloud-storage)

## [!DNL Oracle Eloqua] {#import-data-into-eloqua}へのデータインポートの設定

プラットフォームをAmazonS3またはSFTPストレージに接続した後、ストレージの場所から[!DNL Oracle Eloqua]にデータインポートを設定する必要があります。 これを達成する方法については、[!DNL Oracle Eloqua Help Center]の[連絡先またはアカウントのインポート](https://docs.oracle.com/cloud/latest/marketingcs_gs/OMCAA/Help/DataImportExport/Tasks/ImportingContactsOrAccounts.htm)を参照してください。