---
keywords: Experience Platform;home;popular topics;data ingestion;data location;Data Location;Data management;data management;Lineage;lineage;batch;Batch;ingested data
solution: Experience Platform
title: Adobe Experience Platform のデータ取り込みの概要
topic: overview
description: このドキュメントでは、データをプラットフォームに取り込む 3 つの主な方法を紹介し、詳細については、それぞれの概要ドキュメントへのリンクを示します。
translation-type: tm+mt
source-git-commit: bf99b08a1093a815687cc06372407949e170a0b3
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 55%

---


# データ取り込みの概要

Adobe Experience Platform では、複数のソースからのデータを統合し、マーケターが顧客の行動をより深く理解できるようにします。Adobe Experience Platform Data Ingestion represents the multiple methods by which [!DNL Platform] ingests data from these sources, as well as how that data is persisted within the Data Lake for use by downstream [!DNL Platform] services.

This document introduces the three main ways in which data is ingested into [!DNL Platform], with links to their respective overview documentation for more detailed information.

## バッチ取得

Batch ingestion allows you to ingest data into [!DNL Experience Platform] as batch files. バッチとは、1 つの単位として取り込まれる 1 つ以上のファイルで構成されるデータの単位です。取り込まれたバッチには、正常に取り込まれたレコードの数と、失敗したレコードの数、および関連するエラーメッセージを示すメタデータが提供されます。

フラットな CSV ファイル（XDM スキーマにマッピングされる）や Parket データフレームなど、手動でアップロードしたデータファイルは、この方法を使用して取り込む必要があります。

詳しくは、[バッチ取り込みの概要](./batch-ingestion/overview.md)を参照してください。

## ストリーミング取り込み

Streaming ingestion allows you to send data from client- and server-side devices to [!DNL Experience Platform] in real-time. [!DNL Platform] では、受信したエクスペリエンスデータをストリーミングするためのデータインレットの使用をサポートしています。このデータインレットは、データレイク内のストリーミング対応データセットで保持されます。データインレットは、収集したデータを自動的に認証するように設定でき、信頼できるソースからのデータであることを確認できます。

詳しくは、[ストリーミング取り込みの概要](./streaming-ingestion/overview.md)を参照してください。

## ソース

[!DNL Experience Platform] では、様々なデータプロバイダーへのソース接続を設定できます。これらの接続を使用すると、外部データソースの認証、取り込みの実行時間の設定、取り込みスループットの管理をおこなうことができます。

Source connections can be configured to gather data from other Adobe applications (such as Adobe Analytics and Adobe Audience Manager), third-party cloud storage sources (such as [!DNL Azure Blob], [!DNL Amazon] S3, FTP servers, and SFTP servers), and third-party CRM systems (such as [!DNL Microsoft Dynamics] and [!DNL Salesforce]).

詳しくは、[ソースの概要](../sources/home.md)を参照してください。

## 次の手順 およびその他のリソース

This document provided a brief introduction to the different aspects of [!DNL Data Ingestion] in [!DNL Experience Platform]. 各取り込み方法の概要ドキュメントを引き続き参照して、それぞれの機能、ユースケース、ベストプラクティスをよく理解してください。また、次のインジェストの概要ビデオを見ることで、学習を補うこともできます。 For information on how [!DNL Experience Platform] tracks the metadata for ingested records, see the [Catalog Service overview](../catalog/home.md).

>[!WARNING]
>
> 次のビデオで使用されている「統合プロファイル」という用語は古いです。 ドキュメントで使用され [!DNL "Profile"] る用語または [!DNL "Real-time Customer Profile"] 正しい用語 [!DNL Experience Platform] です。 最新の機能については、ドキュメントを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/27106?quality=12&learn=on)