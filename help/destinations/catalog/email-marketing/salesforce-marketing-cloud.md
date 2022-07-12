---
keywords: 電子メール；電子メール；電子メールの宛先；Salesforce;Salesforce の宛先
title: Salesforce Marketing Cloud 接続
description: Salesforce Marketing Cloud（旧称 ExactTarget）は、訪問者や顧客がエクスペリエンスをパーソナライズするためのジャーニーを構築し、カスタマイズできるデジタルマーケティングスイートです。
exl-id: e85049a7-eaed-4f8a-b670-9999d56928f8
source-git-commit: dd18350387aa6bdeb61612f0ccf9d8d2223a8a5d
workflow-type: tm+mt
source-wordcount: '658'
ht-degree: 28%

---

# [!DNL Salesforce Marketing Cloud] 接続

## 概要 {#overview}

[[!DNL Salesforce Marketing Cloud]](https://www.salesforce.com/jp/products/marketing-cloud/email-marketing/) は、訪問者や顧客がエクスペリエンスをパーソナライズするためのジャーニーを構築し、カスタマイズできるデジタルマーケティングスイートです（旧称 ExactTarget）。

セグメントデータをに送信するには、以下を実行します。 [!DNL Salesforce Marketing Cloud]、最初に [宛先に接続](#connect-destination) プラットフォーム内で [データインポートの設定](#import-data-into-salesforce) ストレージの場所から [!DNL Salesforce Marketing Cloud].

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | セグメントのすべてのメンバーを、目的のスキーマフィールド ( 例：（電子メールアドレス、電話番号、姓）。「プロファイル属性を選択」画面で選択します。 [宛先のアクティベーションワークフロー](../../ui/activate-batch-profile-destinations.md#select-attributes). |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳細を表示 [バッチファイルベースの宛先](/help/destinations/destination-types.md#file-based). |

{style=&quot;table-layout:auto&quot;}

## IP アドレス許可リスト {#allow-list}

SFTP ストレージを使用した電子メールマーケティングの宛先を設定する場合、Adobeでは、特定の IP 範囲を許可リストに追加することをお勧めします。

参照： [クラウドストレージの宛先の IP アドレス許可リスト](../cloud-storage/ip-address-allow-list.md) 許可リストにAdobeIP を追加する必要がある場合。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

この宛先では、次の接続タイプをサポートしています。

* **[!UICONTROL パスワード付き SFTP]**
* **[!UICONTROL SSH キーを使用した SFTP]**

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

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

### アラートの有効化 {#enable-alerts}

アラートを有効にして、宛先へのデータフローのステータスに関する通知を受け取ることができます。 リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートの詳細については、 [UI を使用した宛先アラートの購読](../../ui/alerts.md).

宛先接続の詳細の指定が完了したら、 **[!UICONTROL 次へ]**.

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

詳しくは、 [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md) を参照してください。

### 宛先属性 {#destination-attributes}

この宛先に対してセグメントをアクティブ化する場合は、Adobeでは、 [和集合スキーマ](../../../profile/home.md#profile-fragments-and-union-schemas). 宛先に書き出す一意の ID およびその他の XDM フィールドを選択します。詳しくは、 [電子メールマーケティングの宛先に対してオーディエンスをアクティブ化する際のベストプラクティス](overview.md#best-practices).

## 書き出したデータ {#exported-data}

の場合 [!DNL Salesforce Marketing Cloud] 宛先、Platform は、 `.csv` ファイルを指定したストレージの場所に保存します。 ファイルの詳細については、 [セグメントのアクティベーションを検証](../../ui/activate-batch-profile-destinations.md#verify) （セグメントのアクティベーションに関するチュートリアル）。

## へのデータインポートの設定 [!DNL Salesforce Marketing Cloud] {#import-data-into-salesforce}

接続後 [!DNL Platform] を [!DNL SFTP] ストレージの場合は、ストレージの場所から次の場所へのデータインポートを設定する必要があります。 [!DNL Salesforce Marketing Cloud]. これを実現する方法については、「 [ファイルからのMarketing Cloudへの購読者のインポート](https://help.salesforce.com/articleView?id=mc_es_import_subscribers_from_file.htm&amp;type=5) 内 [!DNL Salesforce Help Center].
