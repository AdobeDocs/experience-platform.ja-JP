---
title: Salesforce Marketing Cloud 接続
description: Salesforce Marketing Cloud（旧称 ExactTarget）は、訪問者や顧客がエクスペリエンスをパーソナライズするためのジャーニーを構築し、カスタマイズできるデジタルマーケティングスイートです。
exl-id: e85049a7-eaed-4f8a-b670-9999d56928f8
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '878'
ht-degree: 34%

---

# [!DNL (Files) Salesforce Marketing Cloud] 接続

## 概要 {#overview}

[[!DNL Salesforce Marketing Cloud]](https://www.salesforce.com/jp/products/marketing-cloud/email-marketing/)は、以前ExactTargetとして知られていたデジタルマーケティングスイートで、訪問者や顧客が体験をパーソナライズするためのジャーニーを構築およびカスタマイズできます。

オーディエンスデータを[!DNL Salesforce Marketing Cloud]に送信するには、まず[Experience Platformの宛先](#connect-destination)に接続し、次に[ ストレージの場所から](#import-data-into-salesforce)へのデータインポート [!DNL Salesforce Marketing Cloud]を設定する必要があります。

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの由来 | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ○ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md)を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの生成元 | ○ | このカテゴリには、[!DNL Segmentation Service]を通じて生成されたオーディエンス以外のすべてのオーディエンスのオリジンが含まれます。 [様々なオーディエンスの起源](/help/segmentation/ui/audience-portal.md#customize)について読みます。 次に例を示します。 <ul><li> カスタムアップロードオーディエンス [がCSV ファイルからExperience Platformに](../../../segmentation/ui/audience-portal.md#import-audience)をインポートしました。</li><li> 類似オーディエンス， </li><li> 連合オーディエンス， </li><li> [!DNL Adobe Journey Optimizer]などの他のExperience Platform アプリで生成されたオーディエンス </li><li> その他。 </li></ul> |

{style="table-layout:auto"}



オーディエンスのデータタイプ別にサポートされるオーディエンス：

| オーディエンスのデータタイプ | サポートあり | 説明 | ユースケース |
|--------------------|-----------|-------------|-----------|
| [人物オーディエンス ](/help/segmentation/types/people-audiences.md) | ○ | 顧客プロファイルにもとづいて、マーケティング施策の特定のグループをターゲットにすることができます。 | 買い物客やカートの放棄が多い |
| [ アカウントオーディエンス ](/help/segmentation/types/account-audiences.md) | × | アカウントベースドマーケティング戦略のために、特定の組織内の個人をターゲットにします。 | B2B マーケティング |
| [見込みオーディエンス ](/help/segmentation/types/prospect-audiences.md) | × | まだ顧客ではないが、ターゲットオーディエンスと特徴を共有する個人をターゲットにします。 | サードパーティデータによる見込み顧客の開拓 |
| [ データセットの書き出し](/help/catalog/datasets/overview.md) | × | [!DNL Adobe Experience Platform] データ レイクに保存されている構造化データのコレクション。 | レポート，データサイエンスワークフロー |

{style="table-layout:auto"}


## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Profile-based]** | [宛先のアクティベーションワークフロー](../../ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性選択画面で選択した目的のスキーマフィールド（例：メールアドレス、電話番号、姓）と共に、セグメントのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Batch]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## IP アドレスの許可リスト {#allow-list}

SFTP ストレージを使用してメールマーケティングの宛先を設定する場合、Adobeでは、特定のIP範囲を契約許可リストに追加することをお勧めします。

Adobe IPをSFTP宛先に追加する必要がある場合は、[SFTP宛先のIP アドレスの許可リストに加える](../cloud-storage/ip-address-allow-list.md)を参照してください。

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage Destinations]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

この宛先は、次の接続タイプをサポートしています。

* **[!UICONTROL SFTP with Password]**
* **[!UICONTROL SFTP with SSH Key]**

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL SFTP with Password]**&#x200B;接続の場合は、次を指定する必要があります。
   * **[!UICONTROL Domain]**: SFTP アカウントのIP アドレスまたはドメイン名。
   * **[!UICONTROL Port]**: SFTP ストレージの場所で使用されるポート。
   * **[!UICONTROL Username]**: SFTP ストレージの場所にログインするユーザー名。
   * **[!UICONTROL Password]**: SFTP ストレージの場所にログインするためのパスワード。
* **[!UICONTROL SFTP with SSH Key]**&#x200B;接続の場合は、次を指定する必要があります。
   * **[!UICONTROL Domain]**: SFTP アカウントのIP アドレスまたはドメイン名。
   * **[!UICONTROL Port]**: SFTP ストレージの場所で使用されるポート。
   * **[!UICONTROL Username]**: SFTP ストレージの場所にログインするユーザー名。
   * **[!UICONTROL SSH Key]**: SFTP ストレージの場所へのログインに使用される秘密SSH キー。 秘密鍵は、Base64 でエンコードされた文字列の形式にする必要があり、パスワードで保護しないでください。

* オプションで、RSA形式の公開鍵を添付して、**[!UICONTROL Key]** セクションで書き出したファイルにPGP/GPGによる暗号化を追加できます。 公開鍵は、[!DNL Base64] でエンコードされた文字列として記述する必要があります。
* **[!UICONTROL Name]**：宛先に関連する名前を選択します。
* **[!UICONTROL Description]**：宛先の説明を入力します。
* **[!UICONTROL Folder Path]**: Experience Platformがエクスポート データをCSV ファイルとしてデポジットする保存場所のパスを指定します。
* **[!UICONTROL File Format]**: **CSV**&#x200B;を選択して、CSV ファイルをストレージの場所に書き出します。

<!--

Commenting out Amazon S3 bucket part for now until support is clarified

- **[!UICONTROL Bucket name]**: Your Amazon S3 bucket, where Experience Platform will deposit the data export. Your input must be between 3 and 63 characters long. Must begin and end with a letter or number. Must contain only lowercase letters, numbers, or hyphens ( - ). Must not be formatted as an IP address (for example, 192.100.1.1).

-->

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の提供が完了したら、**[!UICONTROL Next]**&#x200B;を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID*&#x200B;をエクスポートするには、**[!UICONTROL View Identity Graph]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。<br> ![ ワークフローで強調表示されているID名前空間を選択して、オーディエンスを宛先にアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png " ワークフローで強調表示されたID名前空間を選択して、オーディエンスを宛先にアクティブ化します。"){width="100" zoomable="yes"}

この宛先に対するオーディエンスのアクティブ化の手順については、[ バッチプロファイル書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md)を参照してください。

### 宛先属性 {#destination-attributes}

この宛先に対してオーディエンスをアクティブ化する場合、Adobeでは、[結合スキーマ ](../../../profile/home.md#profile-fragments-and-union-schemas)から一意のIDを選択することをお勧めします。 宛先に書き出す一意の ID およびその他の XDM フィールドを選択します。詳しくは、「[ メールマーケティング宛先に対してオーディエンスをアクティブ化する際のベストプラクティス ](overview.md#best-practices)」を参照してください。

## 書き出したデータ {#exported-data}

[!DNL Salesforce Marketing Cloud]の宛先の場合、Experience Platformは、指定したストレージの場所に`.csv` ファイルを作成します。 ファイルについて詳しくは、オーディエンスアクティベーションのチュートリアルの「[ オーディエンスアクティベーションの検証](../../ui/activate-batch-profile-destinations.md#verify)」を参照してください。

## [!DNL Salesforce Marketing Cloud]へのデータ読み込みを設定 {#import-data-into-salesforce}

[!DNL Experience Platform]を[!DNL SFTP] ストレージに接続した後、ストレージの場所から[!DNL Salesforce Marketing Cloud]へのデータ読み込みを設定する必要があります。 この方法については、[の「](https://help.salesforce.com/articleView?id=mc_es_import_subscribers_from_file.htm&type=5) ファイルからMarketing Cloudへのサブスクライバーの読み込み[!DNL Salesforce Help Center]」を参照してください。
