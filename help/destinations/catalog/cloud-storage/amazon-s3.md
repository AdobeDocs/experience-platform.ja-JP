---
keywords: Amazon S3;S3 destination;s3;amazon s3
title: Amazon S3 接続
description: Amazon Web Services（AWS）S3 ストレージへのライブアウトバウンド接続を作成し、タブ区切りのデータファイルまたは CSV データファイルを Adobe Experience Platform から S3 バケットへと定期的に書き出します。
exl-id: 6a2a2756-4bbf-4f82-88e4-62d211cbbb38
source-git-commit: 3aac1e7c7fe838201368379da8504efc8e316e1c
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 9%

---

# [!DNL Amazon S3] 接続 {#s3-connection}

## 概要 {#overview}

[!DNL Amazon Web Services](AWS)S3 ストレージへのライブアウトバウンド接続を作成して、タブ区切りのデータファイルまたは CSV データファイルをAdobe Experience Platformから S3 バケットに定期的に書き出します。

## 書き出しタイプ {#export-type}

**プロファイルベース**  — セグメントのすべてのメンバーを、目的のスキーマフィールド ( 例：電子メールアドレス、電話番号、姓 )。宛先のアクティベーションワークフローの属性を選択画面 [から選択します](../../ui/activate-segment-streaming-destinations.md#mapping)。

![Amazon S3 プロファイルベースの書き出しタイプ](../../assets/catalog/cloud-storage/amazon-s3/catalog.png)

## 宛先に接続 {#connect}

この宛先に接続するには、[ 宛先の設定に関するチュートリアル ](../../ui/connect-destination.md) で説明されている手順に従います。

### 接続パラメーター {#parameters}

[ この宛先を設定 ](../../ui/connect-destination.md) する際は、次の情報を指定する必要があります。

* **[!DNL Amazon S3]アクセ** スキー **[!DNL Amazon S3]と秘密鍵**:で、 [!DNL Amazon S3]アカウントへの `access key - secret access key` アクセスを Platform に許可するペアを生成 [!DNL Amazon S3] します。詳しくは、[Amazon Web Servicesのドキュメント ](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) を参照してください。
* **[!UICONTROL 名前]**:この宛先を識別するのに役立つ名前を入力します。
* **[!UICONTROL 説明]**:この宛先の説明を入力します。
* **[!UICONTROL バケット名]**:この宛先で使用す [!DNL Amazon S3] るバケットの名前を入力します。
* **[!UICONTROL フォルダーパス]**:書き出したファイルをホストする宛先フォルダーのパスを入力します。

必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 公開鍵は、[!DNL Base64] エンコードされた文字列として書き込む必要があります。

>[!TIP]
>
>「宛先の接続」ワークフローでは、書き出したセグメントファイルごとにAmazon S3 ストレージにカスタムフォルダーを作成できます。 [ マクロを使用して、ストレージの場所 ](overview.md#use-macros) にフォルダーを作成する手順をお読みください。

### 必要な [!DNL Amazon S3] 権限 {#required-s3-permission}

データを正常に接続して [!DNL Amazon S3] ストレージの場所に書き出すには、[!DNL Amazon S3] の [!DNL Platform] の ID およびアクセス管理 (IAM) ユーザーを作成し、次の操作に対するアクセス許可を割り当てます。

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

## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、[ プロファイルの一括書き出し先へのオーディエンスデータのアクティブ化 ](../../ui/activate-batch-profile-destinations.md) を参照してください。

## エクスポートされたデータ {#exported-data}

[!DNL Amazon S3] の宛先の場合、[!DNL Platform] は指定したストレージの場所にタブ区切りの `.csv` ファイルを作成します。 ファイルについて詳しくは、セグメントアクティベーションのチュートリアルの「[ プロファイルのバッチ書き出し先に対するオーディエンスデータのアクティブ化 ](../../ui/activate-batch-profile-destinations.md)」を参照してください。
