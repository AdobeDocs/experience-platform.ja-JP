---
keywords: メール；メール；メール；メールの宛先；oracle responsys の宛先
title: Oracle Responsys 接続
description: Responsys は、Oracle が提供するクロスチャネルマーケティングキャンペーン用の大規模法人向けメールマーケティングツールで、電子メール、モバイル、ディスプレイおよびソーシャルでのインタラクションをパーソナライズします。
exl-id: 70f2f601-afee-4315-bf7a-ed2c92397ebe
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '659'
ht-degree: 42%

---

# [!DNL Oracle Responsys] 接続

## 概要 {#overview}

[Responsys](https://www.oracle.com/cx/marketing/campaign-management/) は、[!DNL Oracle] がメール、モバイル、ディスプレイ、ソーシャルをまたいでインタラクションをパーソナライズするために提供する、クロスチャネルマーケティングキャンペーン用のエンタープライズ電子メールマーケティングツールです。

オーディエンスデータを [!DNL Oracle Responsys] に送信するには、まずAdobe Experience Platformで [ 宛先に接続 ](#connect-destination) してから、ストレージの場所から [ に ](#import-data-into-responsys) データの読み込みを設定 [!DNL Oracle Responsys] する必要があります。

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Profile-based]** | [宛先のアクティベーションワークフロー](../../ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性選択画面で選択した目的のスキーマフィールド（例：メールアドレス、電話番号、姓）と共に、セグメントのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Batch]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## IP アドレス許可リスト {#allow-list}

Adobeでは、SFTP ストレージを使用してメールマーケティングの宛先を設定する場合、特定の IP 範囲を許可リストに追加することをお勧めします。

許可リストにAdobe IP を許可リストする必要がある場合は [](../cloud-storage/ip-address-allow-list.md)SFTP 宛先の IP アドレス追加」を参照してください。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Manage Destinations]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

この宛先は、次の接続タイプをサポートしています。

* **[!UICONTROL SFTP with Password]**
* **[!UICONTROL SFTP with SSH Key]**

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL SFTP with Password]** 接続の場合、次を指定する必要があります。
   * [!UICONTROL Domain]
   * [!UICONTROL Port]
   * [!UICONTROL Username]
   * [!UICONTROL Password]
* **[!UICONTROL SFTP with SSH Key]** 接続の場合、次を指定する必要があります。
   * [!UICONTROL Domain]
   * [!UICONTROL Port]
   * [!UICONTROL Username]
   * [!UICONTROL SSH Key]
* 必要に応じて、RSA 形式の公開鍵を添付して、**[!UICONTROL Key]** セクションで書き出したファイルに PGP/GPG による暗号化を追加できます。 公開鍵は、[!DNL Base64] でエンコードされた文字列として記述する必要があります。
* **[!UICONTROL Name]**：宛先に関連する名前を選択します。
* **[!UICONTROL Description]**：宛先の説明を入力します。
* **[!UICONTROL Folder Path]**: Experience Platformが書き出しデータを CSV ファイルとして保存するストレージの場所のパスを指定します。
* **[!UICONTROL File Format]**: **CSV** を選択して、CSV ファイルをストレージの場所に書き出します。

<!--

Commenting out Amazon S3 bucket part for now until support is clarified

- **[!UICONTROL Bucket name]**: Your Amazon S3 bucket, where Experience Platform will deposit the data export. Your input must be between 3 and 63 characters long. Must begin and end with a letter or number. Must contain only lowercase letters, numbers, or hyphens ( - ). Must not be formatted as an IP address (for example, 192.100.1.1).

-->

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先に対してオーディエンスをアクティブ化する手順については、[ プロファイル書き出しのバッチ宛先に対するオーディエンスデータのアクティブ化 ](../../ui/activate-batch-profile-destinations.md) を参照してください。

### 宛先属性 {#destination-attributes}

この宛先に対してオーディエンスをアクティブ化する場合、Adobeでは [ 和集合スキーマ ](../../../profile/home.md#profile-fragments-and-union-schemas) から一意の ID を選択することをお勧めします。 宛先に書き出す一意の ID およびその他の XDM フィールドを選択します。詳しくは、[ メールマーケティングの宛先に対してオーディエンスをアクティブ化する際のベストプラクティス ](overview.md#best-practices) を参照してください。

## 書き出したデータ {#exported-data}

[!DNL Oracle Responsys] の宛先の場合、Experience Platformは、指定されたストレージの場所に `.csv` ファイルを保存します。 ファイルについて詳しくは、Audience Activation チュートリアルの [Audience Activation の検証 ](../../ui/activate-batch-profile-destinations.md#verify) を参照してください。

## [!DNL Oracle Responsys] へのデータの読み込みの設定 {#import-data-into-responsys}

[!DNL Experience Platform] を [!DNL SFTP] ストレージに接続した後、ストレージの場所から [!DNL Oracle Responsys] へのデータの読み込みを設定する必要があります。 これを実行する方法については、[ の ](https://docs.oracle.com/cloud/latest/marketingcs_gs/OMCEA/Connect_WizardUpload.htm) 連絡先またはアカウントの読み込み [!DNL Oracle Responsys Help Center] を参照してください。
