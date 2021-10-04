---
keywords: 電子メール；電子メール；電子メールの宛先；Salesforce;Salesforce の宛先
title: SalesforceMarketing Cloud接続
seo-description: Salesforce Marketing Cloud is a digital marketing suite formerly known as ExactTarget that allows you to build and customize journeys for visitors and customers to personalize their experience.
exl-id: e85049a7-eaed-4f8a-b670-9999d56928f8
source-git-commit: 3aac1e7c7fe838201368379da8504efc8e316e1c
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 21%

---

# [!DNL Salesforce Marketing Cloud] 接続

## 概要 {#overview}

[[!DNL Salesforce Marketing Cloud]](https://www.salesforce.com/jp/products/marketing-cloud/email-marketing/) は、訪問者や顧客がエクスペリエンスをパーソナライズするためのジャーニーを構築し、カスタマイズできるデジタルマーケティングスイートです（旧称 ExactTarget）。

セグメントデータを [!DNL Salesforce Marketing Cloud] に送信するには、まず Platform で宛先 ](#connect-destination) に接続し、次にストレージの場所から [ データインポート ](#import-data-into-salesforce) を [!DNL Salesforce Marketing Cloud] に設定する必要があります。[

## 書き出しタイプ {#export-type}

**プロファイルベース**  — セグメントのすべてのメンバーを、目的のスキーマフィールド ( 例：電子メールアドレス、電話番号、姓 )。 [オーディエンスアクティベーションワークフローの属性を選択画面](../../ui/activate-batch-profile-destinations.md#select-attributes)から選択します。

## IP アドレス許可リスト {#allow-list}

SFTP ストレージを使用した電子メールマーケティングの宛先を設定する場合、Adobeでは、特定の IP 範囲を許可リストに追加することをお勧めします。

許可リストにAdobeIP を追加する必要がある場合は、[ クラウドストレージの宛先 ](../cloud-storage/ip-address-allow-list.md) の IP アドレス許可リストを参照してください。

## 宛先に接続 {#connect}

この宛先に接続するには、[ 宛先の設定に関するチュートリアル ](../../ui/connect-destination.md) で説明されている手順に従います。

この宛先では、次の接続タイプをサポートしています。

* **[!UICONTROL SFTP（パスワード）]**
* **[!UICONTROL SFTP（SSH キー）]**

### 接続パラメーター {#parameters}

[ この宛先を設定 ](../../ui/connect-destination.md) する際は、次の情報を指定する必要があります。

* **[!UICONTROL パスワード]** を使用した SFTP 接続の場合は、次を指定する必要があります。
   * [!UICONTROL ドメイン]
   * [!UICONTROL ポート]
   * [!UICONTROL ユーザー名]
   * [!UICONTROL パスワード]
* **[!UICONTROL SFTP（SSH キー]**）で接続する場合は、次を指定する必要があります。
   * [!UICONTROL ドメイン]
   * [!UICONTROL ポート]
   * [!UICONTROL ユーザー名]
   * [!UICONTROL SSH キー]

* 必要に応じて、RSA 形式の公開鍵を添付し、**[!UICONTROL Key]** セクションで、PGP/GPG を使用した暗号化を書き出したファイルに追加できます。 公開鍵は、[!DNL Base64] エンコードされた文字列として書き込む必要があります。
* **[!UICONTROL 名前]**：宛先の名前を選択します。
* **[!UICONTROL 説明]**：宛先の説明を入力します。
* **[!UICONTROL フォルダーパス]**:Platform が書き出しデータを CSV またはタブ区切りファイルとして格納するストレージの場所へのパスを指定します。
* **[!UICONTROL ファイル形式]**：**CSV** または **TAB_DELIMITED**。ストレージの場所に書き出すファイル形式を選択します。

<!--

Commenting out Amazon S3 bucket part for now until support is clarified

- **[!UICONTROL Bucket name]**: Your Amazon S3 bucket, where Platform will deposit the data export. Your input must be between 3 and 63 characters long. Must begin and end with a letter or number. Must contain only lowercase letters, numbers, or hyphens ( - ). Must not be formatted as an IP address (for example, 192.100.1.1).

-->

## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、[ プロファイルの一括書き出し先へのオーディエンスデータのアクティブ化 ](../../ui/activate-batch-profile-destinations.md) を参照してください。

### 宛先属性 {#destination-attributes}

この宛先に対してセグメントをアクティブ化する場合は、[ 和集合スキーマ ](../../../profile/home.md#profile-fragments-and-union-schemas) から一意の識別子を選択することをAdobeにお勧めします。 宛先に書き出す一意の ID およびその他の XDM フィールドを選択します。詳しくは、電子メールマーケティングの宛先にオーディエンスをアクティブ化する際のベストプラクティス [ を参照してください。](overview.md#best-practices)

## エクスポートされたデータ {#exported-data}

[!DNL Salesforce Marketing Cloud] の宛先の場合、Platform は指定したストレージの場所に、タブ区切りの `.csv` ファイルを作成します。 ファイルの詳細については、『セグメントのアクティベーションのチュートリアル』の [ セグメントのアクティベーションの検証 ](../../ui/activate-batch-profile-destinations.md#verify) を参照してください。

## [!DNL Salesforce Marketing Cloud] へのデータインポートの設定 {#import-data-into-salesforce}

[!DNL Platform] を [!DNL SFTP] ストレージに接続した後、ストレージの場所から [!DNL Salesforce Marketing Cloud] へのデータインポートを設定する必要があります。 これをおこなう方法については、[[!DNL Salesforce Help Center] の「ファイル ](https://help.salesforce.com/articleView?id=mc_es_import_subscribers_from_file.htm&amp;type=5) からMarketing Cloudに購読者をインポートする」を参照してください。
