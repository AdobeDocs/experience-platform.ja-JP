---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ取り込みのチュートリアル
topic: tutorial
translation-type: tm+mt
source-git-commit: e4da80338dbfbad70dfb3cf7df9fe589e949e788

---


# データをエクスペリエンスプラットフォームに取り込む

Adobe Experience Platformは、複数のソースからのデータを統合し、マーケターが顧客の行動をより深く理解できるようにします。 Adobe Experience Platform Data Ingestionは、Platformがこれらのソースからデータを取り込む複数の方法と、そのデータがData Lake内でどのように保持され、ダウンストリームのPlatformサービスで使用されるかを表します。 データ取り込みには、ソースコネクタを使用したバッチ取り込み、ストリーミング取り込み、取り込みが含まれます。 詳しくは、データ収集の概要を読 [むか](../ingestion/home.md) 、ソースのドキュメントに直接進ん [でください](../sources/home.md)。

## UIとAPIでのソースコネクタの作成

ソースコネクタを使用すると、複数のソースからデータを取り込み、プラットフォームサービスを使用してラベル付け、構造化、拡張を行うことができます。 ソースコネクタの作成を開始するには、ソースの概要を参 [照してください](../sources/home.md)。

## バッチデータの取り込み

Adobe Experience Platformを使用すると、データをバッチファイルとしてプラットフォームに簡単に読み込むことができます。 取り込むデータの例としては、CRMシステムのフラットファイル（パーケーファイルなど）のプロファイルデータ、またはスキーマレジストリの既知のエクスペリエンスデータモデル(XDM)スキーマに準拠するデータが含まれます。 開始するには、プラットフォームへの [統合データのチュートリアルに進みま](../ingestion/tutorials/ingest-batch-data.md)す。

## CSVファイルのXDMへのマッピングスキーマ

CSVデータをAdobe Experience Platformに取り込むには、データをエクスペリエンスデータモデル(XDM)スキーマにマップする必要があります。 Experience Platformユーザーインターフェイスを使用してCSVファイルをXDMスキーマにマップする手順については、CSVファイルのXDMスキーマへのマッ [プのチュートリアルに従ってください](../ingestion/tutorials/map-a-csv-file.md)。

## ストリーミング接続の作成

Experience Platformに開始ストリーミングデータを送信するには、まずストリーミングHTTP接続を作成する必要があります。 ストリーミング接続を作成する場合、ストリーミングデータのソース、および信頼できる（認証済み）ソースと信頼できない（認証済み）ソースのどちらからデータを送信するかなど、主要な詳細を指定する必要があります。 これは、プラットフォームユーザーインターフェイスまたはエクスペリエンスプラットフォームAPIを使用して行うことができます。 詳しくは、UIを使用したストリーミング接続の作成 [またはAPIを使用したストリーミング接続の作成に関する](../ingestion/tutorials/create-streaming-connection-ui.md) 、チ [ュートリアルに従ってください](../ingestion/tutorials/create-streaming-connection.md)。

## 認証済みストリーミング接続の作成

認証済みのデータ収集機能を使用すると、リアルタイムの顧客プロファイルやIDなどのAdobe Experience Platformサービスで、信頼できるソースと信頼できないソースからのレコードを区別できます。 開始するには、認証済みのストリーミング接続を作成す [るためのチュートリアルに従ってくださ](../ingestion/tutorials/create-authenticated-streaming-connection.md)い。

## ストリームレコードと時系列データ

データセット接続とストリーミング接続を使用して、レコードや時系列のデータをプラットフォームにストリーミング配信できます。 記録データのストリーミングを開始するには、ストリーム記録デ [ータの後に、プラットフォームのチュートリアルに進みま](../ingestion/tutorials/streaming-record-data.md)す。 時系列データのストリーミングを開始するには、ストリーム時 [系列データに従ってプラットフォームに入りま](../ingestion/tutorials/streaming-time-series-data.md)す。

## 1回のHTTPリクエストで複数のメッセージをストリーミング

データをAdobe Experience Platformにストリーミングする場合、多数のHTTP呼び出しを作成すると高コストになる可能性があります。 例えば、1 KBのペイロードで200個のHTTPリクエストを作成する代わりに、200 KBの各メッセージで200個のペイロードを持つ1個のHTTPリクエストを作成する方が、より効率的です。 正しく使用すると、1つのリクエスト内で複数のメッセージをグループ化することが、エクスペリエンスプラットフォームに送信されるデータを最適化する優れた方法です。 ストリーミング取り込みを使用して、単一のHTTPリクエスト内で複数のメッセージをエクスペリエンスプラットフォームに送信する方法を学ぶには、複数のメッセージの送 [信のチュートリアルに従ってくださ](../ingestion/tutorials/streaming-multiple-messages.md)い。



