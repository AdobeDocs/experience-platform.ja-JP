---
keywords: 電子メール；電子メール；電子メールの宛先；oracleresponsys の宛先
title: OracleResponsys 接続
description: Responsys は、Oracle が提供するクロスチャネルマーケティングキャンペーン用の大規模法人向け電子メールマーケティングツールで、電子メール、モバイル、ディスプレイおよびソーシャルでのインタラクションをパーソナライズします。
exl-id: 70f2f601-afee-4315-bf7a-ed2c92397ebe
source-git-commit: b0d6e02c67f2a62971332acb224c7422ea467e6c
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 20%

---

# [!DNL Oracle Responsys] 接続

## 概要 {#overview}

[Responsys](https://www.oracle.com/cx/marketing/campaign-management/)[!DNL Oracle] は、 が提供するクロスチャネルマーケティングキャンペーン用の大規模法人向け電子メールマーケティングツールで、電子メール、モバイル、ディスプレイおよびソーシャルでのインタラクションをパーソナライズします。

セグメントデータをに送信するには、以下を実行します。 [!DNL Oracle Responsys]、最初に [宛先に接続](#connect-destination) Adobe Experience Platformで [データインポートの設定](#import-data-into-responsys) ストレージの場所から [!DNL Oracle Responsys].

## 書き出しタイプ {#export-type}

**プロファイルベース**  — セグメントのすべてのメンバーを、目的のスキーマフィールド ( 例：電子メールアドレス、電話番号、姓 )。 [オーディエンスのアクティベーションワークフロー](../../ui/activate-batch-profile-destinations.md#select-attributes).

## IP アドレスの許可リスト {#allow-list}

SFTP ストレージを使用した電子メールマーケティングの宛先を設定する場合、Adobeでは、特定の IP 範囲を許可リストに追加することをお勧めします。

参照： [クラウドストレージの宛先の IP アドレス許可リスト](../cloud-storage/ip-address-allow-list.md) 許可リストにAdobeIP を追加する必要がある場合。

## 宛先に接続 {#connect}

この宛先に接続するには、 [宛先設定のチュートリアル](../../ui/connect-destination.md).

この宛先では、次の接続タイプをサポートしています。

* **[!UICONTROL パスワード付き SFTP]**
* **[!UICONTROL SSH キーを使用した SFTP]**

### 接続パラメーター {#parameters}

While [設定](../../ui/connect-destination.md) この宛先には、次の情報を指定する必要があります。

* の場合 **[!UICONTROL パスワード付き SFTP]** 接続には、次を指定する必要があります。
   * [!UICONTROL ドメイン]
   * [!UICONTROL ポート]
   * [!UICONTROL ユーザー名]
   * [!UICONTROL パスワード]
* の場合 **[!UICONTROL SSH キーを使用した SFTP]** 接続には、次を指定する必要があります。
   * [!UICONTROL ドメイン]
   * [!UICONTROL ポート]
   * [!UICONTROL ユーザー名]
   * [!UICONTROL SSH キー]
* 必要に応じて、RSA 形式の公開鍵を添付し、PGP/GPG を使用して、 **[!UICONTROL キー]** 」セクションに入力します。 公開鍵は、 [!DNL Base64] エンコードされた文字列。
* **[!UICONTROL 名前]**：宛先の名前を選択します。
* **[!UICONTROL 説明]**：宛先の説明を入力します。
* **[!UICONTROL フォルダーパス]**:Platform が書き出しデータを CSV ファイルとして格納するストレージの場所のパスを指定します。
* **[!UICONTROL ファイル形式]**:選択 **CSV** を使用して、CSV ファイルをストレージの場所に書き出します。

<!--

Commenting out Amazon S3 bucket part for now until support is clarified

- **[!UICONTROL Bucket name]**: Your Amazon S3 bucket, where Platform will deposit the data export. Your input must be between 3 and 63 characters long. Must begin and end with a letter or number. Must contain only lowercase letters, numbers, or hyphens ( - ). Must not be formatted as an IP address (for example, 192.100.1.1).

-->

## この宛先へのセグメントのアクティブ化 {#activate}

詳しくは、 [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md) を参照してください。

### 宛先属性 {#destination-attributes}

この宛先に対してセグメントをアクティブ化する場合は、Adobeでは、 [和集合スキーマ](../../../profile/home.md#profile-fragments-and-union-schemas). 宛先に書き出す一意の ID およびその他の XDM フィールドを選択します。詳しくは、 [電子メールマーケティングの宛先に対してオーディエンスをアクティブ化する際のベストプラクティス](overview.md#best-practices).

## 書き出されたデータ {#exported-data}

の場合 [!DNL Oracle Responsys] 宛先、Platform は、 `.csv` ファイルを指定したストレージの場所に保存します。 ファイルの詳細については、 [セグメントのアクティベーションを検証](../../ui/activate-batch-profile-destinations.md#verify) （セグメントのアクティベーションに関するチュートリアル）。

## へのデータインポートの設定 [!DNL Oracle Responsys] {#import-data-into-responsys}

接続後 [!DNL Platform] を [!DNL SFTP] ストレージの場合は、ストレージの場所から次の場所へのデータインポートを設定する必要があります。 [!DNL Oracle Responsys]. これを実現する方法については、「 [連絡先またはアカウントのインポート](https://docs.oracle.com/cloud/latest/marketingcs_gs/OMCEA/Connect_WizardUpload.htm) 内 [!DNL Oracle Responsys Help Center].
