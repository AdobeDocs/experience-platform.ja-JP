---
title: Amazon S3 接続
description: Amazon Web Services（AWS）S3 ストレージへのライブアウトバウンド接続を作成し、CSV データファイルを Adobe Experience Platform から S3 バケットへと定期的に書き出します。
exl-id: 6a2a2756-4bbf-4f82-88e4-62d211cbbb38
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1503'
ht-degree: 48%

---

# [!DNL Amazon S3] 接続 {#s3-connection}

## 宛先の変更ログ {#changelog}

+++ 変更ログを表示


| リリース月 | 更新タイプ | 説明 |
|---|---|---|
| 2024年1月 | 機能とドキュメントの更新 | Amazon S3 宛先コネクタで、新しく想定されるロール認証タイプがサポートされるようになりました。 詳しくは、[ 認証の節 ](#assumed-role-authentication) を参照してください。 |
| 2023年7月 | 機能とドキュメントの更新 | 2023 年 7 月のExperience Platform リリースでは、以下に示すように、[!DNL Amazon S3] の宛先が新しい機能を提供します。<br><ul><li>[ データセット書き出しのサポート ](/help/destinations/ui/export-datasets.md)</li><li>追加の[ファイル命名オプション](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling)。</li><li>書き出されたファイルにカスタムファイルヘッダーを設定する機能（[マッピングステップの改善](/help/destinations/ui/activate-batch-profile-destinations.md#mapping)による）</li><li>[書き出された CSV データファイルの形式をカスタマイズする機能](/help/destinations/ui/batch-destinations-file-formatting-options.md)。</li></ul> |

{style="table-layout:auto"}

+++

## API または UI を使用した [!DNL Amazon S3] ストレージへの接続 {#connect-api-or-ui}

* Experience Platform ユーザーインターフェイスを使用して [!DNL Amazon S3] ストレージの場所に接続するには、以下の [ 宛先への接続 ](#connect) および [ この宛先に対するオーディエンスのアクティブ化 ](#activate) の節を参照してください。
* [!DNL Amazon S3] ストレージの場所にプログラムで接続する方法については、[Flow Service API チュートリアルを使用してオーディエンスをファイルベースの宛先に対してアクティブ化する ](../../api/activate-segments-file-based-destinations.md) 方法に関するガイドを参照してください。

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

![UU.](../../assets/catalog/cloud-storage/amazon-s3/catalog.png) でハイライト表示されたAmazon S3 プロファイルベースの書き出しタイプ

## データセットを書き出し {#export-datasets}

この宛先では、データセットの書き出しをサポートしています。 データセットの書き出しを設定する方法について詳しくは、次のチュートリアルを参照してください。

* [Experience Platform ユーザーインターフェイスを使用したデータセットの書き出し ](/help/destinations/ui/export-datasets.md) 方法。
* [Flow Service API を使用してプログラムでデータセットを書き出す ](/help/destinations/api/export-datasets.md) 方法。

## 書き出されたデータのファイル形式 {#file-format}

*オーディエンスデータ* を書き出すと、Experience Platformは、指定されたストレージの場所に `.csv`、`parquet` または `.json` ファイルを作成します。 ファイルについて詳しくは、Audience Activation チュートリアルの [ 書き出しでサポートされるファイル形式 ](../../ui/activate-batch-profile-destinations.md#supported-file-formats-export) の節を参照してください。

*データセット* を書き出すと、Experience Platformは、指定されたストレージの場所に `.parquet` または `.json` ファイルを保存します。 ファイルについて詳しくは、データセットの書き出しチュートリアルの [ データセットの書き出しが成功したことを確認する ](../../ui/export-datasets.md#verify) の節を参照してください。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_s3_rsa"
>title="RSA 公開鍵"
>abstract="必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。正しい形式のキーの例については、以下のドキュメントリンクを参照してください。"

宛先に対して認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。 Amazon S3 の宛先では、次の 2 つの認証方法をサポートしています。

* アクセスキーと秘密鍵の認証
* 想定される役割認証

#### アクセスキーと秘密鍵の認証

Experience PlatformがAmazon S3 プロパティにデータを書き出せるようにAmazon S3 アクセスキーと秘密鍵を入力する場合は、この認証方法を使用します。

![ アクセスキーと秘密鍵の認証を選択する際の必須フィールドの画像 ](/help/destinations/assets/catalog/cloud-storage/amazon-s3/access-key-secret-key-authentication.png)

* **[!DNL Amazon S3]アクセスキー** および秘密鍵 **: [!DNL Amazon S3] で `access key - secret access key` ペア**&#x200B;[!DNL Amazon S3] 生成して、[!DNL Amazon S3] アカウントにExperience Platform アクセス権を付与します。 詳しくは、[Amazon Web Services に関するドキュメント](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/id_credentials_access-keys.html)を参照してください。
* **[!UICONTROL 暗号化キー]**：必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。正しい形式の暗号化キーの例については、以下の画像を参照してください。

  ![UI での正しい形式の PGP キーの例を示す画像。](../../assets/catalog/cloud-storage/sftp/pgp-key.png)

#### 想定される役割 {#assumed-role-authentication}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_s3_assumed_role"
>title="想定される役割認証"
>abstract="アカウントキーと秘密鍵をアドビと共有したくない場合は、この認証タイプを使用します。代わりに、Experience Platform は、役割ベースのアクセス権を使用して Amazon S3 の場所に接続します。アドビユーザー用に AWS で作成した役割の ARN をペーストします。このパターンは、`arn:aws:iam::800873819705:role/destinations-role-customer` のようになります "

![ 想定される役割認証を選択する際の必須フィールドの画像 ](/help/destinations/assets/catalog/cloud-storage/amazon-s3/assumed-role-authentication.png)

アカウントキーと秘密鍵をアドビと共有したくない場合は、この認証タイプを使用します。代わりに、Experience Platformは、役割ベースのアクセスを使用してAmazon S3 の場所に接続します。

これを行うには、AWS コンソールで、Amazon S3 バケットに書き込む [ 必要な権限を持つ ](#minimum-permissions-iam-user)Adobeのユーザーを想定する必要があります。 Adobe アカウント **[!UICONTROL 670664943635]** を使用して、AWSに **[!UICONTROL 信頼済みエンティティ]** を作成します。 詳しくは、[ 役割の作成に関するAWS ドキュメント ](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user.html) を参照してください。

* **[!DNL Role]**:Adobe ユーザー用にAWSで作成したロールの ARN を貼り付けます。 パターンは `arn:aws:iam::800873819705:role/destinations-role-customer` に似ています。
* **[!UICONTROL 暗号化キー]**：必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。正しい形式の暗号化キーの例については、以下の画像を参照してください。

### 宛先の詳細を入力 {#destination-details}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_s3_bucket"
>title="バケット名"
>abstract="3～63 文字の長さにする必要があります。先頭および末尾は文字または数字にする必要があります。小文字、数字、ハイフン（-）のみを含める必要があります。IP アドレス（例：192.100.1.1）の形式にはできません。"

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_s3_folderpath"
>title="フォルダーパス"
>abstract="A～Z、a～z、0～9 の文字のみを含める必要があります。また、次の特殊文字を含めることができます。`/!-_.'()"^[]+$%.*"`オーディエンスファイルごとにフォルダーを作成するには、`/%SEGMENT_NAME%` または `/%SEGMENT_ID%` または `/%SEGMENT_NAME%/%SEGMENT_ID%` のマクロをテキストフィールドに挿入します。マクロは、フォルダーパスの最後にのみ挿入できます。マクロの例については、ドキュメントを参照してください。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/cloud-storage/overview.html?lang=ja#use-macros" text="マクロを使用して、ストレージの場所にフォルダーを作成する"

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：この宛先を識別するのに役立つ名前を入力します。
* **[!UICONTROL 説明]**：宛先の説明を入力します。
* **[!UICONTROL バケット名]**：この宛先が使用する [!DNL Amazon S3] バケット名を入力します。
* **[!UICONTROL フォルダーパス]**：書き出したファイルをホストする保存先フォルダーのパス。
* **[!UICONTROL ファイルの種類]**：書き出したファイルにExperience Platformで使用する形式を選択します。 [!UICONTROL CSV] オプションを選択する場合、[ ファイル形式オプションを設定 ](../../ui/batch-destinations-file-formatting-options.md) することもできます。
* **[!UICONTROL 圧縮形式]**：書き出したファイルにExperience Platformで使用する圧縮タイプを選択します。
* **[!UICONTROL マニフェストファイルを含める]**：書き出しに、書き出しの場所や書き出しのサイズなどに関する情報を含んだマニフェスト JSON ファイルを含めたい場合は、このオプションをオンに切り替えます。 マニフェストには、形式 `manifest-<<destinationId>>-<<dataflowRunId>>.json` を使用して名前を付けます。 [ サンプル マニフェスト ファイル ](/help/destinations/assets/common/manifest-d0420d72-756c-4159-9e7f-7d3e2f8b501e-0ac8f3c0-29bd-40aa-82c1-f1b7e0657b19.json) を表示します。 マニフェストファイルには、次のフィールドが含まれています。
   * `flowRunId`：書き出されたファイルを生成した [ データフロー実行 ](/help/dataflows/ui/monitor-destinations.md#dataflow-runs-for-batch-destinations)。
   * `scheduledTime`: ファイルが書き出された時間（UTC 単位）。
   * `exportResults.sinkPath`：書き出されたファイルが格納されるストレージの場所のパス。
   * `exportResults.name`：書き出すファイルの名前。
   * `size`：書き出されたファイルのサイズ（バイト単位）。

>[!TIP]
>
>宛先に接続ワークフローでは、書き出したオーディエンスファイルごとにAmazon S3 ストレージにカスタムフォルダーを作成できます。 手順については、[マクロを使用して、ストレージの場所にフォルダーを作成する](overview.md#use-macros)を参照してください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

### 必要な [!DNL Amazon S3] 権限 {#required-s3-permission}

[!DNL Amazon S3] ストレージの場所に正常に接続してデータを書き出すには、[!DNL Amazon S3] で [!DNL Experience Platform] の IAM（Identity and Access Management）ユーザーを作成し、次のアクションに対する権限を割り当てます。

* `s3:DeleteObject`
* `s3:GetBucketLocation`
* `s3:GetObject`
* `s3:ListBucket`
* `s3:PutObject`
* `s3:ListMultipartUploadParts`

#### IAM が役割認証を想定する場合に必要な最小権限 {#minimum-permissions-iam-user}

IAM 役割を顧客として設定する場合は、役割に関連付けられた権限ポリシーに、バケット内のターゲットフォルダーへの必要なアクションと、バケットのルートへの `s3:ListBucket` のアクションが含まれていることを確認します。 この認証タイプの最小権限ポリシーの例を以下に示します。

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:DeleteObject",
                "s3:GetBucketLocation",
                "s3:ListMultipartUploadParts"
            ],
            "Resource": "arn:aws:s3:::bucket/folder/*"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": "arn:aws:s3:::bucket"
        }
    ]
}  
```

<!--

Commenting out this note, as write permissions are assigned through the s3:PutObject permission.

>[!IMPORTANT]
>
>Experience Platform needs `write` permissions on the bucket object where the export files will be delivered.

-->

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先に対してオーディエンスをアクティブ化する手順については、[ プロファイル書き出しのバッチ宛先に対するオーディエンスデータのアクティブ化 ](../../ui/activate-batch-profile-destinations.md) を参照してください。

## データの正常な書き出しの検証 {#exported-data}

データが正常に書き出されたかどうかを確認するには、[!DNL Amazon S3] ストレージを確認し、書き出されたファイルに想定されるプロファイル母集団が含まれていることを確認してください。

## IP アドレスの許可リスト {#ip-address-allow-list}

許可リストにAdobe許可リストに加えるの IP を登録する必要がある場合は、[IP アドレス ](ip-address-allow-list.md) の記事を参照してください。