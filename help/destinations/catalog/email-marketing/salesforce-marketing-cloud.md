---
keywords: 電子メール；電子メール；電子メール；電子メールの宛先；salesforce;salesforceの宛先
title: SalesforceMarketing Cloud接続
seo-description: Salesforce Marketing Cloud（旧称 ExactTarget）は、訪問者や顧客がエクスペリエンスをパーソナライズするためのジャーニーを構築し、カスタマイズできるデジタルマーケティングスイートです。
exl-id: e85049a7-eaed-4f8a-b670-9999d56928f8
source-git-commit: 70be44e919070df910d618af4507b600ad51123c
workflow-type: tm+mt
source-wordcount: '629'
ht-degree: 30%

---

# [!DNL Salesforce Marketing Cloud] connection

## 概要 {#overview}

[[!DNL Salesforce Marketing Cloud]](https://www.salesforce.com/jp/products/marketing-cloud/email-marketing/) は、訪問者や顧客がエクスペリエンスをパーソナライズするためのジャーニーを構築し、カスタマイズできるデジタルマーケティングスイートです（旧称 ExactTarget）。

セグメントデータを[!DNL Salesforce Marketing Cloud]に送信するには、まずPlatformの宛先](#connect-destination)を[接続し、次に[ストレージの場所から[!DNL Salesforce Marketing Cloud]にデータインポート](#import-data-into-salesforce)を設定する必要があります。

## エクスポートの種類{#export-type}

**プロファイルベース**  — セグメントのすべてのメンバーを、必要なスキーマフィールド(例：電子メールアドレス、電話番号、姓)。 [宛先アクティベーションワークフローの属性を選択画面で選択](../../ui/activate-destinations.md#select-attributes)。

## IPアドレス許可リスト{#allow-list}

SFTPストレージを使用して電子メールマーケティングの宛先を設定する場合、Adobeでは許可リストに特定のIP範囲を追加することを推奨します。

許可リストにAdobeIPを追加する必要がある場合は、[IPアドレスの許可リストを参照して、クラウドストレージの宛先](../cloud-storage/ip-address-allow-list.md)を確認してください。

## 宛先の接続 {#connect-destination}

**[!UICONTROL 接続]**/**[!UICONTROL 宛先]**&#x200B;で、[!DNL Salesforce Marketing Cloud]を選択し、**[!UICONTROL 設定]**&#x200B;を選択します。

![Salesforce への接続](../../assets/catalog/email-marketing/salesforce/catalog.png)

**[!UICONTROL アカウント]**&#x200B;の手順で、クラウドストレージの接続先への接続を以前に設定した場合は、「**[!UICONTROL 既存のアカウント]**」を選択し、既存の接続の1つを選択します。 または、「**[!UICONTROL 新しいアカウント]**」を選択して、新しい接続を設定できます。アカウント認証資格情報を入力し、「**[!UICONTROL 宛先に接続]**」を選択します。[!DNL Salesforce Marketing Cloud]の場合は、**[!UICONTROL パスワード]**&#x200B;のSFTPと&#x200B;**[!UICONTROL SSHキー]**&#x200B;のSFTPのどちらかを選択できます。

![SalesforceMarketing Cloudアカウントの接続](../../assets/catalog/email-marketing/salesforce/connection-type.png)

接続タイプに応じて、以下の情報を入力し、**[!UICONTROL 設定]**&#x200B;を選択します。

- **[!UICONTROL SFTPとパスワード]**&#x200B;接続の場合、[!UICONTROL ドメイン]、[!UICONTROL ポート]、[!UICONTROL ユーザー名]、[!UICONTROL パスワード]を指定する必要があります。
- **[!UICONTROL SSHキー]**&#x200B;接続を使用するSFTPの場合、[!UICONTROL ドメイン]、[!UICONTROL ポート]、[!UICONTROL ユーザー名]、[!UICONTROL SSHキー]を指定する必要があります。

必要に応じて、RSA形式の公開鍵を添付し、PGP/GPGを使用した暗号化を&#x200B;**[!UICONTROL Key]**&#x200B;セクションのエクスポートファイルに追加することができます。 公開鍵は、[!DNL Base64]エンコードされた文字列として書き込む必要があります。

![Salesforce 情報の入力](../../assets/catalog/email-marketing/salesforce/account-info.png)

**[!UICONTROL 認証]**&#x200B;の手順で、宛先に関する関連情報を次のように入力します。
- **[!UICONTROL 名前]**：宛先の名前を選択します。
- **[!UICONTROL 説明]**：宛先の説明を入力します。
- **[!UICONTROL Folder Path]**:ストレージー上の場所に、PlatformがエクスポートデータをCSVまたはタブ区切りファイルとしてデポジットするパスを指定します。
- **[!UICONTROL ファイル形式]**：**CSV** または **TAB_DELIMITED**。ストレージの場所に書き出すファイル形式を選択します。
- **[!UICONTROL マーケティングアクション]**:マーケティングアクションは、データをエクスポート先にエクスポートする意図を示します。Adobe定義のマーケティングアクションから選択するか、独自のマーケティングアクションを作成することができます。 マーケティングアクションについて詳しくは、[データ使用ポリシーの概要](../../../data-governance/policies/overview.md)を参照してください。

<!--

Commenting out Amazon S3 bucket part for now until support is clarified

- **[!UICONTROL Bucket name]**: Your Amazon S3 bucket, where Platform will deposit the data export. Your input must be between 3 and 63 characters long. Must begin and end with a letter or number. Must contain only lowercase letters, numbers, or hyphens ( - ). Must not be formatted as an IP address (for example, 192.100.1.1).

-->

![Salesforce の基本情報](../../assets/catalog/email-marketing/salesforce/basic-information.png)

上記のフィールドに入力し、「**[!UICONTROL 宛先を作成]**」をクリックします。これで宛先が接続され、宛先への[セグメントをアクティブ化](../../ui/activate-destinations.md)できます。

## セグメントのアクティブ化 {#activate-segments}

セグメントのアクティベーションワークフローについて詳しくは、「[宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-destinations.md)」を参照してください。

## 宛先属性 {#destination-attributes}

[セグメント](../../ui/activate-destinations.md)を[!DNL Salesforce Marketing Cloud]宛先にアクティブ化する場合、Adobeでは[和集合スキーマ](../../../profile/home.md#profile-fragments-and-union-schemas)から一意の識別子を選択することを推奨します。 宛先に書き出す一意の ID およびその他の XDM フィールドを選択します。詳細については、[書き出したファイルの保存先属性として使用するスキーマフィールドの選択](./overview.md#destination-attributes)を参照してください。

## エクスポートされたデータ{#exported-data}

[!DNL Salesforce Marketing Cloud]宛先の場合、Platformは、指定したストレージーの場所にタブ区切りの`.csv`ファイルを作成します。 ファイルについて詳しくは、セグメントアクティベーションチュートリアルの「電子メールマーケティングの宛先とクラウドストレージの宛先」[を参照してください。](../../ui/activate-destinations.md#esp-and-cloud-storage)

## [!DNL Salesforce Marketing Cloud] {#import-data-into-salesforce}へのデータインポートの設定

[!DNL Platform]を[!DNL SFTP]ストレージに接続した後、ストレージの場所から[!DNL Salesforce Marketing Cloud]にデータインポートを設定する必要があります。 これを達成する方法については、[[!DNL Salesforce Help Center]の「ファイル](https://help.salesforce.com/articleView?id=mc_es_import_subscribers_from_file.htm&amp;type=5)からMarketing Cloudにサブスクライバをインポートする」を参照してください。
