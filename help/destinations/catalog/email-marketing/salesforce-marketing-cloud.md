---
title: Salesforce Marketing Cloud 接続
description: Salesforce Marketing Cloud（旧称 ExactTarget）は、訪問者や顧客がエクスペリエンスをパーソナライズするためのジャーニーを構築し、カスタマイズできるデジタルマーケティングスイートです。
exl-id: e85049a7-eaed-4f8a-b670-9999d56928f8
source-git-commit: c35b43654d31f0f112258e577a1bb95e72f0a971
workflow-type: tm+mt
source-wordcount: '798'
ht-degree: 50%

---

# [!DNL (Files) Salesforce Marketing Cloud] 接続

## 概要 {#overview}

[[!DNL Salesforce Marketing Cloud]](https://www.salesforce.com/jp/products/marketing-cloud/email-marketing/) は、以前は ExactTarget と呼ばれていたデジタルマーケティングスイートで、訪問者や顧客がエクスペリエンスをパーソナライズできるよう、ジャーニーを作成およびカスタマイズできます。

オーディエンスデータをに送信するには [!DNL Salesforce Marketing Cloud]、最初にする必要があります [宛先への接続](#connect-destination) Platform で、次に [データインポートの設定](#import-data-into-salesforce) ストレージの場所から [!DNL Salesforce Marketing Cloud].

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platformを通じて生成されたオーディエンス [セグメント化サービス](../../../segmentation/home.md). |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | [宛先のアクティベーションワークフロー](../../ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性選択画面で選択した目的のスキーマフィールド（例：メールアドレス、電話番号、姓）と共に、セグメントのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## IP アドレスの許可リスト {#allow-list}

SFTP ストレージを使用してメールマーケティングの宛先を設定する場合は、特定の IP 範囲をAdobe許可リストに加えるに追加することをお勧めします。

こちらを参照してください [SFTP の宛先の IP アドレス許可リスト](../cloud-storage/ip-address-allow-list.md) AdobeIP を許可リストに追加する必要がある場合。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

この宛先は、次の接続タイプをサポートしています。

* **[!UICONTROL パスワード付き SFTP]**
* **[!UICONTROL SSH キーを使用した SFTP]**

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* の場合 **[!UICONTROL パスワード付き SFTP]** 接続。次を指定する必要があります。
   * **[!UICONTROL ドメイン]**:SFTP アカウントの IP アドレスまたはドメイン名
   * **[!UICONTROL ポート]**：SFTP ストレージの場所で使用されるポート
   * **[!UICONTROL ユーザー名]**：SFTP ストレージの場所にログインするためのユーザー名
   * **[!UICONTROL パスワード]**：SFTP ストレージの場所にログインするためのパスワード
* の場合 **[!UICONTROL SSH キーを使用した SFTP]** 接続。次を指定する必要があります。
   * **[!UICONTROL ドメイン]**:SFTP アカウントの IP アドレスまたはドメイン名
   * **[!UICONTROL ポート]**：SFTP ストレージの場所で使用されるポート
   * **[!UICONTROL ユーザー名]**：SFTP ストレージの場所にログインするためのユーザー名
   * **[!UICONTROL SSH キー]**：SFTP ストレージの場所へのログインに使用する SSH 秘密鍵。 秘密鍵は、Base64 でエンコードされた文字列の形式にする必要があり、パスワードで保護しないでください。

* 必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに PGP/GPG による暗号化をの下に追加できます。 **[!UICONTROL キー]** セクション。 公開鍵は、[!DNL Base64] でエンコードされた文字列として記述する必要があります。
* **[!UICONTROL 名前]**：宛先の名前を選択します。
* **[!UICONTROL 説明]**：宛先の説明を入力します。
* **[!UICONTROL フォルダーパス]**:Platform が書き出しデータを CSV ファイルとして保存するストレージの場所のパスを指定します。
* **[!UICONTROL ファイル形式]**：を選択 **CSV** CSV ファイルをストレージの場所に書き出す。

<!--

Commenting out Amazon S3 bucket part for now until support is clarified

- **[!UICONTROL Bucket name]**: Your Amazon S3 bucket, where Platform will deposit the data export. Your input must be between 3 and 63 characters long. Must begin and end with a letter or number. Must contain only lowercase letters, numbers, or hyphens ( - ). Must not be formatted as an IP address (for example, 192.100.1.1).

-->

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* エクスポートする *id*、が必要です **[!UICONTROL ID グラフの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![宛先に対してオーディエンスをアクティブ化するために、ワークフローで強調表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png "宛先に対してオーディエンスをアクティブ化するために、ワークフローで強調表示されている ID 名前空間を選択します。"){width="100" zoomable="yes"}

参照： [プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](../../ui/activate-batch-profile-destinations.md) この宛先に対してオーディエンスをアクティブ化する手順については、を参照してください。

### 宛先属性 {#destination-attributes}

この宛先に対してオーディエンスをアクティブ化する場合、Adobeでは、 [結合スキーマ](../../../profile/home.md#profile-fragments-and-union-schemas). 宛先に書き出す一意の ID およびその他の XDM フィールドを選択します。詳しくは、次を参照してください。 [メールマーケティングの宛先に対してオーディエンスをアクティブ化する際のベストプラクティス](overview.md#best-practices).

## 書き出したデータ {#exported-data}

[!DNL Salesforce Marketing Cloud] 宛先の場合、Platform は `.csv` ファイルを指定したストレージの場所に保存します。 ファイルの詳細については、を参照してください [audience activation の検証](../../ui/activate-batch-profile-destinations.md#verify) オーディエンスのアクティベーションのチュートリアルの

## へのデータの読み込みを設定 [!DNL Salesforce Marketing Cloud] {#import-data-into-salesforce}

接続後 [!DNL Platform] 宛先： [!DNL SFTP] ストレージ。ストレージの場所からへのデータの読み込みを設定する必要があります [!DNL Salesforce Marketing Cloud]. これを実行する方法については、を参照してください。 [ファイルからのMarketing Cloudへのサブスクライバのインポート](https://help.salesforce.com/articleView?id=mc_es_import_subscribers_from_file.htm&amp;type=5) が含まれる [!DNL Salesforce Help Center].
