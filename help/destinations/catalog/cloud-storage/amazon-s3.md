---
keywords: Amazon S3;S3 destination;s3;amazon s3
title: Amazon S3 接続
description: Amazon Web Services（AWS）S3 ストレージへのライブアウトバウンド接続を作成し、CSV データファイルを Adobe Experience Platform から S3 バケットへと定期的に書き出します。
exl-id: 6a2a2756-4bbf-4f82-88e4-62d211cbbb38
source-git-commit: 55f72e4f229e18648e0e745a2a60e9add50455b0
workflow-type: tm+mt
source-wordcount: '990'
ht-degree: 87%

---

# [!DNL Amazon S3] 接続 {#s3-connection}

## 宛先の変更ログ {#changelog}

>[!IMPORTANT]
>
>データセットの書き出し機能のベータ版リリースと、ファイルの書き出し機能の改善により、宛先カタログに 2 つの [!DNL Amazon S3] カードが表示される場合があります。
>* 既ににファイルを書き出している場合は、 **[!UICONTROL Amazon S3]** の宛先です。新しいデータフローを作成して新しいデータフローを作成してください **[!UICONTROL Amazon S3 beta]** 宛先。
>* **[!UICONTROL Amazon S3]** の宛先へのデータフローをまだ作成していない場合は、新しい **[!UICONTROL Amazon S3 ベータ版]**&#x200B;カードを使用してファイルを **[!UICONTROL Amazon S3]** に書き出してください。


![2 つの Amazon S3 の宛先カードを並べて表示した画像。](../../assets/catalog/cloud-storage/amazon-s3/two-amazons3-destination-cards.png)

新しい [!DNL Amazon S3] 宛先カードの改善点は次のとおりです。

* [データセット書き出しのサポート](/help/destinations/ui/export-datasets.md)。
* 追加の[ファイル命名オプション](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling)。
* 書き出されたファイルにカスタムファイルヘッダーを設定する機能（[マッピングステップの改善](/help/destinations/ui/activate-batch-profile-destinations.md#mapping)による）
* [書き出された CSV データファイルの形式をカスタマイズする機能](/help/destinations/ui/batch-destinations-file-formatting-options.md)。

## 概要 {#overview}

[!DNL Amazon S3] ストレージへのライブアウトバウンド接続を作成して、Adobe Experience Platform から独自の S3 バケットにデータファイルを定期的に書き出します。

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | [宛先のアクティベーションワークフロー](../../ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性選択画面で選択した目的のスキーマフィールド（例：メールアドレス、電話番号、姓）と共に、セグメントのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

![Amazon S3 プロファイルベースの書き出しタイプ](../../assets/catalog/cloud-storage/amazon-s3/catalog.png)

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_s3_rsa"
>title="RSA 公開鍵"
>abstract="必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。正しい形式のキーの例については、以下のドキュメントリンクを参照してください。"

宛先に対して認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

* **[!DNL Amazon S3]アクセスキー** と **[!DNL Amazon S3]秘密鍵**：[!DNL Amazon S3] で `access key - secret access key` ペアを生成して、[!DNL Amazon S3] アカウントに Platform アクセス権を付与します。詳しくは、[Amazon Web Services に関するドキュメント](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/id_credentials_access-keys.html)を参照してください。
* **[!UICONTROL 暗号化キー]**：必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。正しい形式の暗号化キーの例については、以下の画像を参照してください。

   ![UI での正しい形式の PGP キーの例を示す画像](../../assets/catalog/cloud-storage/sftp/pgp-key.png)

### 宛先の詳細の入力 {#destination-details}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_s3_bucket"
>title="バケット名"
>abstract="3～63 文字の長さにする必要があります。先頭および末尾は文字または数字にする必要があります。小文字、数字、ハイフン（-）のみを含める必要があります。IP アドレス（例：192.100.1.1）の形式にはできません。"

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_s3_folderpath"
>title="フォルダーパス"
>abstract="A～Z、a～z、0～9 の文字のみを含める必要があります。また、次の特殊文字を含めることができます。`/!-_.'()"^[]+$%.*"`セグメントファイルごとにフォルダーを作成するには、`/%SEGMENT_NAME%` または `/%SEGMENT_ID%` または `/%SEGMENT_NAME%/%SEGMENT_ID%` のマクロをテキストフィールドに挿入します。マクロは、フォルダーパスの最後にのみ挿入できます。マクロの例については、ドキュメントを参照してください。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/cloud-storage/overview.html?lang=ja#use-macros" text="マクロを使用して、ストレージの場所にフォルダーを作成する"

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：この宛先を識別するのに役立つ名前を入力します。
* **[!UICONTROL 説明]**：この宛先の説明を入力します。
* **[!UICONTROL バケット名]**：この宛先で使用する [!DNL Amazon S3] バケットの名前を入力します。
* **[!UICONTROL フォルダーパス]**：書き出したファイルをホストする宛先フォルダーへのパスを入力します。
* **[!UICONTROL ファイルタイプ]**:書き出すファイルに使用する形式Experience Platformを選択します。 このオプションは、 **[!UICONTROL Amazon S3 beta]** 宛先。 選択時に、 [!UICONTROL CSV] オプションを選択する場合は、 [ファイル形式設定オプションの設定](../../ui/batch-destinations-file-formatting-options.md).
* **[!UICONTROL 圧縮形式]**:書き出したファイルにExperience Platformが使用する圧縮タイプを選択します。 このオプションは、 **[!UICONTROL Amazon S3 beta]** 宛先。
* **[!UICONTROL マニフェストファイルを含める]**:書き出しの場所や書き出しサイズなどに関する情報を含むマニフェスト JSON ファイルを書き出しに含める場合は、このオプションをオンに切り替えます。 このオプションは、 **[!UICONTROL Amazon S3 beta]** 宛先。

>[!TIP]
>
>宛先に接続ワークフローでは、書き出したセグメントファイルごとに Amazon S3 ストレージにカスタムフォルダーを作成できます。手順については、[マクロを使用して、ストレージの場所にフォルダーを作成する](overview.md#use-macros)を参照してください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

### 必要な [!DNL Amazon S3] 権限 {#required-s3-permission}

[!DNL Amazon S3] ストレージの場所に正常に接続してデータを書き出すには、[!DNL Amazon S3] で [!DNL Platform] の IAM（Identity and Access Management）ユーザーを作成し、次のアクションに対する権限を割り当てます。

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
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]**[に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先にオーディエンスセグメントを有効化する手順については、[プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](../../ui/activate-batch-profile-destinations.md)を参照してください。

## （ベータ版）データセットの書き出し {#export-datasets}

この宛先では、データセットの書き出しをサポートしています。 データセットの書き出しを設定する方法について詳しくは、[データセットの書き出しチュートリアル](/help/destinations/ui/export-datasets.md)を参照してください。

## 書き出したデータ {#exported-data}

[!DNL Amazon S3] の宛先の場合、[!DNL Platform] には、指定したストレージの場所にデータファイルが作成されます。ファイルについて詳しくは、セグメントの有効化チュートリアルの[プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](../../ui/activate-batch-profile-destinations.md)を参照してください。
