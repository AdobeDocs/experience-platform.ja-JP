---
keywords: 電子メール；電子メール；電子メール；電子メールの宛先；salesforce;salesforceの宛先
title: SalesforceMarketing Cloud接続
seo-description: Salesforce Marketing Cloud（旧称 ExactTarget）は、訪問者や顧客がエクスペリエンスをパーソナライズするためのジャーニーを構築し、カスタマイズできるデジタルマーケティングスイートです。
translation-type: tm+mt
source-git-commit: 709908196bb5df665c7e7df10dc58ee9f3b0edbf
workflow-type: tm+mt
source-wordcount: '621'
ht-degree: 48%

---


# [!DNL Salesforce Marketing Cloud] connection

## 概要 {#overview}

[[!DNL Salesforce Marketing Cloud]](https://www.salesforce.com/jp/products/marketing-cloud/email-marketing/) は、訪問者や顧客がエクスペリエンスをパーソナライズするためのジャーニーを構築し、カスタマイズできるデジタルマーケティングスイートです（旧称 ExactTarget）。

セグメントデータを[!DNL Salesforce Marketing Cloud]に送信するには、まずPlatformの宛先](#connect-destination)を[接続し、次に[ストレージの場所から[!DNL Salesforce Marketing Cloud]にデータインポート](#import-data-into-salesforce)を設定する必要があります。

## エクスポートの種類{#export-type}

**プロファイルベース**  — セグメントのすべてのメンバーを、必要なスキーマフィールド(例：電子メールアドレス、電話番号、姓)。 [宛先アクティベーションワークフローの属性を選択画面で選択](../../ui/activate-destinations.md#select-attributes)。

## 宛先の接続 {#connect-destination}

**[!UICONTROL 接続]**/**[!UICONTROL 宛先]**&#x200B;で、[!DNL Salesforce Marketing Cloud]を選択し、**[!UICONTROL 宛先]**&#x200B;を接続します。

![Salesforce への接続](../../assets/catalog/email-marketing/salesforce/catalog.png)

クラウドストレージの宛先への接続を既に設定している場合は、**[!UICONTROL 認証]**&#x200B;手順で「**[!UICONTROL 既存のアカウント]**」を選択し、既存の接続のいずれかを選択します。または、「**[!UICONTROL 新しいアカウント]**」を選択して、新しい接続を設定できます。アカウント認証資格情報を入力し、「**[!UICONTROL 宛先に接続]**」を選択します。[!DNL Salesforce Marketing Cloud]の場合は、**[!UICONTROL パスワード]**&#x200B;のSFTPと&#x200B;**[!UICONTROL SSHキー]**&#x200B;のSFTPのどちらかを選択できます。 接続タイプに応じて、以下の情報を入力し、「**[!UICONTROL 宛先への接続]**」を選択します。

**[!UICONTROL SFTP（パスワード）]** で接続する場合は、ドメイン、ポート、ユーザー名、パスワードを指定する必要があります。

**[!UICONTROL SFTP（SSH キー）]** で接続する場合は、ドメイン、ポート、ユーザー名、SSH キーを指定する必要があります。

![Salesforce 情報の入力](../../assets/catalog/email-marketing/salesforce/account-info.png)

「**[!UICONTROL 設定]**」で、目的の宛先に関する情報を次のように入力します。
- **[!UICONTROL 名前]**：宛先の名前を選択します。
- **[!UICONTROL 説明]**：宛先の説明を入力します。
- **[!UICONTROL グループ名]**:プラットフォームがデータエクスポートを預け入れる、AmazonS3バケット。入力する文字は3 ～ 63文字の長さにする必要があります。 先頭と末尾は文字または数字にする必要があります。 小文字、数字、ハイフン(-)のみを含める必要があります。 IPアドレス（192.100.1.1など）の形式を設定しないでください。
- **[!UICONTROL Folder Path]**:ストレージー上の場所に、PlatformがエクスポートデータをCSVまたはタブ区切りファイルとしてデポジットするパスを指定します。
- **[!UICONTROL ファイル形式]**：**CSV** または **TAB_DELIMITED**。ストレージの場所に書き出すファイル形式を選択します。
- **[!UICONTROL マーケティングアクション]**:マーケティングアクションは、データをエクスポート先にエクスポートする意図を示します。Adobe定義のマーケティングアクションから選択するか、独自のマーケティングアクションを作成することができます。 マーケティング活動の詳細については、「[Adobe Experience Platform](../../../data-governance/policies/overview.md)のデータガバナンス」ページを参照してください。 Adobe定義の個々のマーケティングアクションについて詳しくは、[データ使用ポリシーの概要](../../../data-governance/policies/overview.md)を参照してください。

![Salesforce の基本情報](../../assets/catalog/email-marketing/salesforce/basic-information.png)

上記のフィールドに入力し、「**[!UICONTROL 宛先を作成]**」をクリックします。これで宛先が接続され、宛先への[セグメントをアクティブ化](../../ui/activate-destinations.md)できます。

## セグメントのアクティブ化 {#activate-segments}

セグメントのアクティベーションワークフローについて詳しくは、「[宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-destinations.md)」を参照してください。

## 宛先属性 {#destination-attributes}

[セグメント](../../ui/activate-destinations.md)を[!DNL Salesforce Marketing Cloud]宛先に対してアクティブ化する場合、[和集合スキーマ](../../../profile/home.md#profile-fragments-and-union-schemas)から一意の識別子を選択することをお勧めします。 宛先に書き出す一意の ID およびその他の XDM フィールドを選択します。詳しくは、「電子メールマーケティングの宛先」の「[書き出したファイルの宛先属性として使用するスキーマフィールドの選択](./overview.md#destination-attributes)」を参照してください。

## エクスポートされたデータ{#exported-data}

[!DNL Salesforce Marketing Cloud]宛先の場合、Platformは、指定したストレージーの場所にタブ区切りの`.txt`ファイルまたは`.csv`ファイルを作成します。 ファイルについて詳しくは、セグメントアクティベーションチュートリアルの「電子メールマーケティングの宛先とクラウドストレージの宛先」[を参照してください。](../../ui/activate-destinations.md#esp-and-cloud-storage)

## [!DNL Salesforce Marketing Cloud] {#import-data-into-salesforce}へのデータインポートの設定

プラットフォームを[!DNL Amazon S3]またはSFTPストレージに接続した後、ストレージの場所から[!DNL Salesforce Marketing Cloud]にデータインポートを設定する必要があります。 これを達成する方法については、[[!DNL Salesforce Help Center]の「ファイル](https://help.salesforce.com/articleView?id=mc_es_import_subscribers_from_file.htm&amp;type=5)からMarketing Cloudにサブスクライバをインポートする」を参照してください。