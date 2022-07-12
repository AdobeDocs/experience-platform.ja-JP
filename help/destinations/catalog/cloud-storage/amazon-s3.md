---
keywords: Amazon S3;S3 destination;s3;amazon s3
title: Amazon S3 接続
description: Amazon Web Services(AWS)S3 ストレージへのライブアウトバウンド接続を作成し、CSV データファイルをAdobe Experience Platformから独自の S3 バケットに定期的に書き出します。
exl-id: 6a2a2756-4bbf-4f82-88e4-62d211cbbb38
source-git-commit: fd2019feb25b540612a278cbea5bf5efafe284dc
workflow-type: tm+mt
source-wordcount: '753'
ht-degree: 7%

---

# [!DNL Amazon S3] 接続 {#s3-connection}

## 概要 {#overview}

へのライブアウトバウンド接続を作成します [!DNL Amazon Web Services] (AWS)S3 ストレージを使用し、Adobe Experience Platformから独自の S3 バケットに CSV データファイルを定期的に書き出します。

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | セグメントのすべてのメンバーを、目的のスキーマフィールド ( 例：（電子メールアドレス、電話番号、姓）。「プロファイル属性を選択」画面で選択します。 [宛先のアクティベーションワークフロー](../../ui/activate-batch-profile-destinations.md#select-attributes). |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳細を表示 [バッチファイルベースの宛先](/help/destinations/destination-types.md#file-based). |

{style=&quot;table-layout:auto&quot;}

![Amazon S3 プロファイルベースの書き出しタイプ](../../assets/catalog/cloud-storage/amazon-s3/catalog.png)

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先設定ワークフローで、以下の 2 つのセクションにリストするフィールドに入力します。

### 宛先に対する認証 {#authenticate}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_s3_rsa"
>title="RSA 公開鍵"
>abstract="必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 公開鍵は、 [!DNL Base64-encoded] 文字列。 以下のドキュメントリンクで、正しく書式設定されたキーの例を確認してください。"

宛先を認証するには、必須フィールドに入力し、「 」を選択します。 **[!UICONTROL 宛先に接続]**.

* **[!DNL Amazon S3]アクセスキー** および **[!DNL Amazon S3]秘密鍵**:In [!DNL Amazon S3]、 `access key - secret access key` ペアを使用して、 [!DNL Amazon S3] アカウント 詳しくは、 [Amazon Web Servicesドキュメント](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html).
* **[!UICONTROL 暗号化キー]**:必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 公開鍵は、 [!DNL Base64-encoded] 文字列。
   * 例: `----BEGIN PGP PUBLIC KEY BLOCK---- {Base64-encoded string} ----END PGP PUBLIC KEY BLOCK----`. 簡潔にするために中央部を短くした、正しくフォーマットされた PGP キーの例を以下に示します。

      ![PGP キー](../../assets/catalog/cloud-storage/sftp/pgp-key.png)

### 宛先の詳細を入力 {#destination-details}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_s3_bucket"
>title="バケット名"
>abstract="3 ～ 63 文字の長さにする必要があります。 先頭および末尾は文字または数字にする必要があります。 小文字、数字、ハイフン (-) のみを含める必要があります。 IP アドレス（例：192.100.1.1）の形式にはできません。"

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_s3_folderpath"
>title="フォルダーパス"
>abstract="A ～ Z、a ～ z、0 ～ 9 の文字のみを含める必要があります。また、次の特殊文字を含めることができます。 `/!-_.'()"^[]+$%.*"`. セグメントファイルごとにフォルダーを作成するには、マクロを挿入します。 `/%SEGMENT_NAME%` または `/%SEGMENT_ID%` または `/%SEGMENT_NAME%/%SEGMENT_ID%` をテキストフィールドに挿入します。 マクロは、フォルダーパスの最後にのみ挿入できます。 マクロの例をドキュメントに表示します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/cloud-storage/overview.html#use-macros" text="マクロを使用して、ストレージの場所にフォルダーを作成する"

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。 UI でフィールドの横にアスタリスクが表示される場合は、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**:この宛先を識別するのに役立つ名前を入力します。
* **[!UICONTROL 説明]**:この宛先の説明を入力します。
* **[!UICONTROL バケット名]**:名前を入力 [!DNL Amazon S3] この宛先で使用するバケット。
* **[!UICONTROL フォルダーパス]**:書き出したファイルをホストする保存先フォルダーのパスを入力します。

>[!TIP]
>
>「宛先の接続」ワークフローでは、書き出したセグメントファイルごとにAmazon S3 ストレージにカスタムフォルダーを作成できます。 読み取り [マクロを使用して、ストレージの場所にフォルダーを作成する](overview.md#use-macros) 」を参照してください。

### アラートの有効化 {#enable-alerts}

アラートを有効にして、宛先へのデータフローのステータスに関する通知を受け取ることができます。 リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートの詳細については、 [UI を使用した宛先アラートの購読](../../ui/alerts.md).

宛先接続の詳細の指定が完了したら、 **[!UICONTROL 次へ]**.

### 必須 [!DNL Amazon S3] 権限 {#required-s3-permission}

データを正常に接続してに書き出すには、以下を実行します。 [!DNL Amazon S3] ストレージの場所、次の IAM (Identity and Access Management) ユーザーを作成する [!DNL Platform] in [!DNL Amazon S3] 次のアクションに対する権限を割り当てます。

* `s3:DeleteObject`
* `s3:GetBucketLocation`
* `s3:GetObject`
* `s3:ListBucket`
* `s3:PutObject`
* `s3:ListMultipartUploadParts`

<!--

Commenting out this note, as write permissions are assigned through the s3:PutObject permission.

>[!IMPORTANT]
>
>Platform needs `write` permissions on the bucket object where the export files will be delivered.

-->

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

詳しくは、 [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md) を参照してください。

## 書き出したデータ {#exported-data}

の場合 [!DNL Amazon S3] 宛先、 [!DNL Platform] を作成 `.csv` ファイルを指定したストレージの場所に保存します。 ファイルの詳細については、 [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md) （セグメントのアクティベーションに関するチュートリアル）。
