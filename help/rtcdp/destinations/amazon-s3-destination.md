---
title: Amazon S3 の宛先
seo-title: Amazon S3 の宛先
description: Amazon Web Services（AWS）S3 ストレージへのライブアウトバウンド接続を作成し、タブ区切りのデータファイルまたは CSV データファイルを Adobe Experience Platform から S3 バケットへと定期的に書き出します。
seo-description: Amazon Web Services（AWS）S3 ストレージへのライブアウトバウンド接続を作成し、タブ区切りのデータファイルまたは CSV データファイルを Adobe Experience Platform から S3 バケットへと定期的に書き出します。
translation-type: tm+mt
source-git-commit: b96286f6a06f0583b45343a513ee64f0025d79a7
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 51%

---


# [!DNL Amazon S3] destination

## 概要

Create a live outbound connection to your [!DNL Amazon Web Services] (AWS) S3 storage to periodically export tab-delimited or CSV data files from Adobe Experience Platform into your own S3 buckets.

## 宛先の接続 {#connect-destination}

See [Cloud storage destinations workflow ](/help/rtcdp/destinations/cloud-storage-destinations-workflow.md)for instructions on how to connect to your cloud storage destinations, including [!DNL Amazon S3].

For [!DNL Amazon S3] destinations, enter the following information in the create destination workflow:

* **[!DNL Amazon S3]アクセスキーと[!DNL Amazon S3]秘密鍵&#x200B;**: では、アクセス・キー — シークレット・アクセス・キー・ペアを生成[!DNL Amazon S3]して、Adobeに対してリアルタイムCDPアクセスを[!DNL Amazon S3]アカウントに付与します。



>[!IMPORTANT]
>
>アドビのリアルタイム CDP には、書き出しファイルの配信先となるバケットオブジェクトに対する `write` 権限が必要です。
