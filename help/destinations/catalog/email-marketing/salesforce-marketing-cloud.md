---
title: Salesforce Marketing Cloud 接続
description: Salesforce Marketing Cloud（旧称 ExactTarget）は、訪問者や顧客がエクスペリエンスをパーソナライズするためのジャーニーを構築し、カスタマイズできるデジタルマーケティングスイートです。
exl-id: e85049a7-eaed-4f8a-b670-9999d56928f8
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '801'
ht-degree: 48%

---

# [!DNL (Files) Salesforce Marketing Cloud] 接続

## 概要 {#overview}

[[!DNL Salesforce Marketing Cloud]](https://www.salesforce.com/jp/products/marketing-cloud/email-marketing/) は、以前は ExactTarget と呼ばれていたデジタルマーケティングスイートで、訪問者や顧客がエクスペリエンスをパーソナライズできるよう、ジャーニーを作成およびカスタマイズできます。

オーディエンスデータを [!DNL Salesforce Marketing Cloud] に送信するには、まずExperience Platformで [ 宛先に接続 ](#connect-destination) してから、ストレージの場所から [!DNL Salesforce Marketing Cloud] に [ データの読み込みを設定 ](#import-data-into-salesforce) する必要があります。

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
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | [宛先のアクティベーションワークフロー](../../ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性選択画面で選択した目的のスキーマフィールド（例：メールアドレス、電話番号、姓）と共に、セグメントのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## IP アドレスの許可リスト {#allow-list}

SFTP ストレージを使用してメールマーケティングの宛先を設定する場合、Adobeでは、特定の IP 範囲を許可リストに追加することをお勧めします。

許可リストにAdobe IP を追加する必要がある場合は [&#128279;](../cloud-storage/ip-address-allow-list.md)IP アドレス^SFTP 宛先の許可リスト^を参照してください。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

この宛先は、次の接続タイプをサポートしています。

* **[!UICONTROL パスワードを使用した SFTP]**
* **[!UICONTROL SSH キーを使用した SFTP]**

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL パスワード付き SFTP]** 接続の場合は、次を指定する必要があります。
   * **[!UICONTROL ドメイン]**:SFTP アカウントの IP アドレスまたはドメイン名
   * **[!UICONTROL ポート]**：SFTP ストレージの場所で使用されるポート
   * **[!UICONTROL ユーザー名]**：SFTP ストレージの場所にログインするためのユーザー名
   * **[!UICONTROL パスワード]**：SFTP ストレージの場所にログインするためのパスワード
* SSH キーを使用した **[!UICONTROL SFTP]** 接続の場合は、次を指定する必要があります。
   * **[!UICONTROL ドメイン]**:SFTP アカウントの IP アドレスまたはドメイン名
   * **[!UICONTROL ポート]**：SFTP ストレージの場所で使用されるポート
   * **[!UICONTROL ユーザー名]**：SFTP ストレージの場所にログインするためのユーザー名
   * **[!UICONTROL SSH キー]**：SFTP ストレージの場所へのログインに使用する SSH 秘密鍵。 秘密鍵は、Base64 でエンコードされた文字列の形式にする必要があり、パスワードで保護しないでください。

* 必要に応じて、RSA 形式の公開鍵を添付して、「**[!UICONTROL キー]**」セクションで書き出したファイルに PGP/GPG による暗号化を追加できます。 公開鍵は、[!DNL Base64] でエンコードされた文字列として記述する必要があります。
* **[!UICONTROL 名前]**：宛先の名前を選択します。
* **[!UICONTROL 説明]**：宛先の説明を入力します。
* **[!UICONTROL フォルダーパス]**:Experience Platformが書き出しデータを CSV ファイルとして保存するストレージの場所のパスを指定します。
* **[!UICONTROL ファイル形式]**: **CSV** を選択して、CSV ファイルをストレージの場所に書き出します。

<!--

Commenting out Amazon S3 bucket part for now until support is clarified

- **[!UICONTROL Bucket name]**: Your Amazon S3 bucket, where Experience Platform will deposit the data export. Your input must be between 3 and 63 characters long. Must begin and end with a letter or number. Must contain only lowercase letters, numbers, or hyphens ( - ). Must not be formatted as an IP address (for example, 192.100.1.1).

-->

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先に対してオーディエンスをアクティブ化する手順については、[ プロファイル書き出しのバッチ宛先に対するオーディエンスデータのアクティブ化 ](../../ui/activate-batch-profile-destinations.md) を参照してください。

### 宛先属性 {#destination-attributes}

この宛先に対してオーディエンスをアクティブ化する場合、Adobeでは [ 和集合スキーマ ](../../../profile/home.md#profile-fragments-and-union-schemas) から一意の ID を選択することをお勧めします。 宛先に書き出す一意の ID およびその他の XDM フィールドを選択します。詳しくは、[ メールマーケティングの宛先に対してオーディエンスをアクティブ化する際のベストプラクティス ](overview.md#best-practices) を参照してください。

## 書き出したデータ {#exported-data}

[!DNL Salesforce Marketing Cloud] の宛先の場合、Experience Platformは、指定されたストレージの場所に `.csv` ファイルを保存します。 ファイルについて詳しくは、Audience Activation チュートリアルの [Audience Activation の検証 ](../../ui/activate-batch-profile-destinations.md#verify) を参照してください。

## [!DNL Salesforce Marketing Cloud] へのデータの読み込みの設定 {#import-data-into-salesforce}

[!DNL Experience Platform] を [!DNL SFTP] ストレージに接続した後、ストレージの場所から [!DNL Salesforce Marketing Cloud] へのデータの読み込みを設定する必要があります。 これを実現する方法については、[!DNL Salesforce Help Center] の [ ファイルからのMarketing Cloudへのサブスクライバーの読み込み ](https://help.salesforce.com/articleView?id=mc_es_import_subscribers_from_file.htm&amp;type=5) を参照してください。
