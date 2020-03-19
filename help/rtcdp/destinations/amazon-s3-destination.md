---
title: Amazon S3の宛先
seo-title: Amazon S3の宛先
description: Amazon Web Services(AWS)S3ストレージへのライブアウトバウンド接続を作成し、タブ区切りのデータファイルまたはCSVデータファイルをAdobe Experience PlatformからS3バケットに定期的にエクスポートします。
seo-description: Amazon Web Services(AWS)S3ストレージへのライブアウトバウンド接続を作成し、タブ区切りのデータファイルまたはCSVデータファイルをAdobe Experience PlatformからS3バケットに定期的にエクスポートします。
translation-type: tm+mt
source-git-commit: acd3865be4994a478048d317060c55b7e0d9914c

---


# Amazon S3の宛先

## 概要

Amazon Web Services(AWS)S3ストレージへのライブアウトバウンド接続を作成し、タブ区切りのデータファイルまたはCSVデータファイルをAdobe Experience PlatformからS3バケットに定期的にエクスポートします。

## 宛先の接続 {#connect-destination}

Amazon S3を含むク [ラウドストレ ](/help/rtcdp/destinations/cloud-storage-destinations-workflow.md)ージの宛先に接続する方法については、「クラウドストレージの宛先のワークフロー」を参照してください。

Amazon S3の宛先については、宛先の作成ワークフローで次の情報を入力します。

* **Amazon S3アクセスキーとAmazon S3秘密キー**:Amazon S3で、アクセスキー — 秘密アクセスキーのペアを生成し、Amazon S3アカウントにAdobe Real-time CDPアクセスを許可します。
* **Amazon S3パス**:これは、Amazon S3バケット内でAdobe Real-time CDPがエクスポートファイルを配信する場所です。


>[!IMPORTANT]
>
>Adobe Real-time CDPには、エクスポートフ `write` ァイルが配信されるバケットオブジェクトに対する権限が必要です。
