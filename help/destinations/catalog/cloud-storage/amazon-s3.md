---
title: Amazon S3 接続
description: Amazon Web Services（AWS）S3 ストレージへのライブアウトバウンド接続を作成し、CSV データファイルを Adobe Experience Platform から S3 バケットへと定期的に書き出します。
exl-id: 6a2a2756-4bbf-4f82-88e4-62d211cbbb38
source-git-commit: c126e6179309ccfbedfbfe2609cfcfd1ea45f870
workflow-type: tm+mt
source-wordcount: '1354'
ht-degree: 48%

---

# [!DNL Amazon S3] 接続 {#s3-connection}

## 宛先の変更ログ {#changelog}

+++ 変更ログを表示


| リリース月 | 更新タイプ | 説明 |
|---|---|---|
| 2024年1月 | 機能とドキュメントの更新 | Amazon S3 宛先コネクタで、新しい想定されるロール認証タイプがサポートされるようになりました。 詳しくは、 [認証セクション](#assumed-role-authentication). |
| 2023年7月 | 機能とドキュメントの更新 | 2023 年 7 月のExperience Platformリリースに伴い、 [!DNL Amazon S3] の宛先には、次に示す新しい機能が用意されています。 <br><ul><li>[データセットの書き出しのサポート](/help/destinations/ui/export-datasets.md)</li><li>追加の[ファイル命名オプション](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling)。</li><li>書き出されたファイルにカスタムファイルヘッダーを設定する機能（[マッピングステップの改善](/help/destinations/ui/activate-batch-profile-destinations.md#mapping)による）</li><li>[書き出された CSV データファイルの形式をカスタマイズする機能](/help/destinations/ui/batch-destinations-file-formatting-options.md)。</li></ul> |

{style="table-layout:auto"}

+++

## 次に接続： [!DNL Amazon S3] API または UI を介したストレージ {#connect-api-or-ui}

* 次の URL に接続するには： [!DNL Amazon S3] ストレージの場所 Platform ユーザーインターフェイスを使用して、「 」セクションを読みます。 [宛先に接続](#connect) および [この宛先に対するオーディエンスをアクティブ化](#activate) 下
* 次の URL に接続するには： [!DNL Amazon S3] ストレージの場所をプログラムで設定する場合は、次の方法に関するガイドを読んでください。 [フローサービス API のチュートリアルを使用して、ファイルベースの宛先に対するオーディエンスをアクティブ化します](../../api/activate-segments-file-based-destinations.md).

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの起源 | サポートあり | 説明 |
---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [セグメント化サービス](../../../segmentation/home.md). |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/overview.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | [宛先のアクティベーションワークフロー](../../ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性選択画面で選択した目的のスキーマフィールド（例：メールアドレス、電話番号、姓）と共に、セグメントのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

![Amazon S3 のプロファイルベースの書き出しタイプで、UU で強調表示されています。](../../assets/catalog/cloud-storage/amazon-s3/catalog.png)

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_s3_rsa"
>title="RSA 公開鍵"
>abstract="必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。正しい形式のキーの例については、以下のドキュメントリンクを参照してください。"

宛先を認証するには、必須フィールドに入力し、「 」を選択します。 **[!UICONTROL 宛先に接続]**. Amazon S3 の宛先は、次の 2 つの認証方法をサポートします。

* アクセスキーと秘密鍵の認証
* 仮定された役割認証

#### アクセスキーと秘密鍵の認証

この認証方法は、Experience PlatformがAmazon S3 プロパティにデータを書き出せるように、Amazon S3 のアクセスキーと秘密鍵を入力する場合に使用します。

![アクセスキーおよび秘密鍵認証を選択する際の必須フィールドの画像。](/help/destinations/assets/catalog/cloud-storage/amazon-s3/access-key-secret-key-authentication.png)

* **[!DNL Amazon S3]アクセスキー** と **[!DNL Amazon S3]秘密鍵**：[!DNL Amazon S3] で `access key - secret access key` ペアを生成して、[!DNL Amazon S3] アカウントに Platform アクセス権を付与します。詳しくは、[Amazon Web Services に関するドキュメント](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/id_credentials_access-keys.html)を参照してください。
* **[!UICONTROL 暗号化キー]**：必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。正しい形式の暗号化キーの例については、以下の画像を参照してください。

  ![UI での正しく書式設定された PGP キーの例を示す画像。](../../assets/catalog/cloud-storage/sftp/pgp-key.png)

#### 想定される役割 {#assumed-role-authentication}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_s3_assumed_role"
>title="仮定された役割認証"
>abstract="アカウントキーと秘密鍵をAdobeと共有しない場合は、この認証タイプを使用します。 代わりに、Experience Platformは、役割ベースのアクセス権を使用してAmazon S3 の場所に接続します。 AWSで作成した役割の ARN を、Adobeユーザーに貼り付けます。 このパターンは、次のようになります。 `arn:aws:iam::800873819705:role/destinations-role-customer` "

![想定される役割認証を選択する際の必須フィールドの画像。](/help/destinations/assets/catalog/cloud-storage/amazon-s3/assumed-role-authentication.png)

アカウントキーと秘密鍵をAdobeと共有しない場合は、この認証タイプを使用します。 代わりに、Experience Platformは、役割ベースのアクセス権を使用してAmazon S3 の場所に接続します。

これをおこなうには、AWSコンソールで、とのAdobeを想定するユーザーを作成する必要があります [必要な権限](#required-s3-permission) を使用してAmazon S3 バケットに書き込みます。 の作成 **[!UICONTROL 信頼済みエンティティ]** AWSのAdobeアカウント **[!UICONTROL 670664943635]**. 詳しくは、 [AWSのロール作成に関するドキュメント](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user.html).

* **[!DNL Role]**:AWSで作成した役割の ARN を、Adobeユーザーに貼り付けます。 このパターンは、次のようになります。 `arn:aws:iam::800873819705:role/destinations-role-customer`.
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
* **[!UICONTROL 説明]**：この宛先の説明を入力します。
* **[!UICONTROL バケット名]**：この宛先が使用する [!DNL Amazon S3] バケット名を入力します。
* **[!UICONTROL フォルダーパス]**：書き出したファイルをホストする保存先フォルダーのパス。
* **[!UICONTROL ファイルタイプ]**：書き出したファイルに使用する形式Experience Platformを選択します。 選択時に、 [!UICONTROL CSV] オプションを選択する場合は、 [ファイル形式設定オプションの設定](../../ui/batch-destinations-file-formatting-options.md).
* **[!UICONTROL 圧縮形式]**：書き出したファイルにExperience Platformが使用する圧縮タイプを選択します。
* **[!UICONTROL マニフェストファイルを含める]**：書き出しの場所や書き出しサイズなどに関する情報を含むマニフェスト JSON ファイルを書き出しに含める場合は、このオプションをオンに切り替えます。 マニフェストの名前は、形式を使用して付けられます `manifest-<<destinationId>>-<<dataflowRunId>>.json`. を表示します。 [サンプルマニフェストファイル](/help/destinations/assets/common/manifest-d0420d72-756c-4159-9e7f-7d3e2f8b501e-0ac8f3c0-29bd-40aa-82c1-f1b7e0657b19.json). マニフェストファイルには、次のフィールドが含まれます。
   * `flowRunId`: [データフローの実行](/help/dataflows/ui/monitor-destinations.md#dataflow-runs-for-batch-destinations) 書き出されたファイルを生成した
   * `scheduledTime`：ファイルが書き出されたときの UTC 時刻 (UTC)。
   * `exportResults.sinkPath`：書き出されたファイルが格納されるストレージの場所のパス。
   * `exportResults.name`：書き出されたファイルの名前。
   * `size`：書き出されるファイルのサイズ（バイト単位）。

>[!TIP]
>
>宛先の接続ワークフローでは、書き出したオーディエンスファイルごとにAmazon S3 ストレージにカスタムフォルダーを作成できます。 手順については、[マクロを使用して、ストレージの場所にフォルダーを作成する](overview.md#use-macros)を参照してください。

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

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* 書き出す *id*、 **[!UICONTROL ID グラフを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png "ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。"){width="100" zoomable="yes"}

詳しくは、 [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md) を参照してください。

## データセットを書き出し {#export-datasets}

この宛先では、データセットの書き出しをサポートしています。 データセットエクスポートの設定方法について詳しくは、次のチュートリアルを参照してください。

* 方法 [Platform ユーザーインターフェイスを使用したデータセットの書き出し](/help/destinations/ui/export-datasets.md).
* 方法 [フローサービス API を使用したデータセットの書き出し](/help/destinations/api/export-datasets.md).

## 書き出したデータ {#exported-data}

[!DNL Amazon S3] の宛先の場合、[!DNL Platform] には、指定したストレージの場所にデータファイルが作成されます。ファイルについて詳しくは、 [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md) （ audience activation チュートリアル）を参照してください。
