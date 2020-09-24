---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ取得のチュートリアル
topic: tutorial
type: Tutorial
description: データ取得には、バッチ取得、ストリーミング取得、ソースコネクタを使用した取得が含まれます。
translation-type: tm+mt
source-git-commit: 97dfd3a9a66fe2ae82cec8954066bdf3b6346830
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 42%

---


# Ingest data into [!DNL Experience Platform]

Adobe Experience Platform は、複数のソースからのデータを統合し、マーケターが顧客の行動をより深く理解できるようにします。Adobe [!DNL Experience Platform Data Ingestion] represents the multiple methods by which [!DNL Platform] ingests data from these sources, as well as how that data is persisted within the Data Lake for use by downstream [!DNL Platform services]. [!DNL Data Ingestion] ソースコネクタを使用したバッチ取り込み、ストリーミング取り込み、取り込みが含まれます。 詳しくは、「[データ取得の概要](../ingestion/home.md)」を読むか、[ソースのドキュメント](../sources/home.md)に直接進んでください。

## UI と API でのソースコネクタの作成

Source connectors allow you to ingest data from multiple sources, where it can then be labeled, structured, and enhanced using [!DNL Platform services]. ソースコネクタの作成を開始するには、 [ソースの概要を参照してください](../sources/home.md)。

## バッチデータの取得

Adobe Experience Platform allows you to easily import data into [!DNL Platform] as batch files. Examples of data to be ingested may include profile data from a flat file in a CRM system (such as a parquet file) or data that conforms to a known [!DNL Experience Data Model] (XDM) schema in the Schema Registry. 開始するには、『[Platform へのデータ取得のチュートリアル](../ingestion/tutorials/ingest-batch-data.md)』に進みます。

## CSV ファイルの XDM スキーマへのマッピング

In order to ingest CSV data into Adobe Experience Platform, the data must be mapped to an [!DNL Experience Data Model] (XDM) schema. For steps showing how to map a CSV file to an XDM schema using the [!DNL Experience Platform] user interface, follow the [map a CSV file to an XDM schema tutorial](../ingestion/tutorials/map-a-csv-file.md).

## ストリーミング接続の作成

ストリーミングデータを開始するに [!DNL Experience Platform]は、まずHTTPエンドポイントを要求する必要があります。 認証済みの動作を強制するように、このエンドポイントを設定するオプションがあります。 This can be done using the [!DNL Platform] user interface or [!DNL Experience Platform] APIs. 詳しくは、[UI を使用したストリーミング接続の作成](../ingestion/tutorials/create-streaming-connection-ui.md)または [API を使用したストリーミング接続の作成](../ingestion/tutorials/create-streaming-connection.md)に関するチュートリアルに従ってください。

## 認証済みストリーミング接続の作成

Authenticated Data Collection allows Adobe Experience Platform services, such as [!DNL Real-time Customer Profile] and [!DNL Identity], to differentiate between records coming from trusted sources and untrusted sources. 開始するには、[認証済みのストリーミング接続を作成](../ingestion/tutorials/create-authenticated-streaming-connection.md)するためのチュートリアルに従ってください。

## ストリームレコードと時系列データ

With a dataset and steaming connections in place, you can stream record or time series data into [!DNL Platform]. レコードデータのストリーミングを開始するには、『[Platform へのレコードデータのストリーミングのチュートリアル](../ingestion/tutorials/streaming-record-data.md)』に進みます。時系列データのストリーミングを開始するには、「[Platform への時系列データのストリーミング](../ingestion/tutorials/streaming-time-series-data.md)」に従います。

## 1 回の HTTP リクエストでの複数メッセージのストリーミング

データを Adobe Experience Platform にストリーミングする場合、多数の HTTP 呼び出しを作成するコストが高くなる可能性があります。例えば、1 KB のペイロードで 200 個の HTTP リクエストを作成するよりも、それぞれが 1 KB の 200 個のメッセージが含まれた 1 個の HTTP リクエスト（200 KB の単一ペイロード）を作成する方がはるかに効率的です。When used correctly, grouping multiple messages within a single request is an excellent way to optimize data being sent to [!DNL Experience Platform]. To learn how to send multiple messages to [!DNL Experience Platform] within a single HTTP request using streaming ingestion, follow the [sending multiple messages tutorial](../ingestion/tutorials/streaming-multiple-messages.md).



