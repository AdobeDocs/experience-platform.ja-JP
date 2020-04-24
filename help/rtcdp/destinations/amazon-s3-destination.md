---
title: Amazon S3 の宛先
seo-title: Amazon S3 の宛先
description: Amazon Web Services（AWS）S3 3ストレージへのライブアウトバウンド接続を作成し、タブ区切りのデータファイルまたは CSV データファイルを Adobe Experience Platform から S3 バケットへと定期的に書き出します。
seo-description: Amazon Web Services（AWS）S3 3ストレージへのライブアウトバウンド接続を作成し、タブ区切りのデータファイルまたは CSV データファイルを Adobe Experience Platform から S3 バケットへと定期的に書き出します。
translation-type: tm+mt
source-git-commit: f3c6c27b7ad07ada0df18aabe0e8503253b38342

---


# Amazon S3 の宛先

## 概要

Amazon Web Services（AWS）S3 3ストレージへのライブアウトバウンド接続を作成し、タブ区切りのデータファイルまたは CSV データファイルを Adobe Experience Platform から S3 バケットへと定期的に書き出します。

## 宛先の接続 {#connect-destination}

クラウドストレージの宛先（Amazon S3 を含む）への接続方法については、「[クラウドストレージの宛先のワークフロー](/help/rtcdp/destinations/cloud-storage-destinations-workflow.md)」を参照してください。

Amazon S3 の宛先については、宛先の作成ワークフローで次の情報を入力します。

* **Amazon S3アクセスキーとAmazon S3秘密キー**:Amazon S3で、アクセスキー — 秘密アクセスキーのペアを生成し、Amazon S3アカウントにAdobe Real-time CDPアクセスを許可します。



>[!IMPORTANT]
>
>Adobe Real-time CDP には、書き出しファイルの配信先となるバケットオブジェクトに対する `write` 権限が必要です。
