---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ取り込みのチュートリアル
topic: tutorial
translation-type: tm+mt
source-git-commit: 0eecd802fc8d0ace3a445f3f188a7f095b97d0c8
workflow-type: tm+mt
source-wordcount: '516'
ht-degree: 0%

---


# データをエクスペリエンスプラットフォームに取り込む

Adobe Experience Platformは、複数のソースからのデータを統合して、マーケティング担当者が顧客の行動をより深く理解できるようにします。 Adobe Experience Platform Data Ingestは、Platformがこれらのソースからデータを取り込む複数の方法と、そのデータがData Lake内でどのように保持され、ダウンストリームプラットフォームサービスで使用されるかを表します。 データ取り込みには、ソースコネクタを使用したバッチ取り込み、ストリーミング取り込み、取り込みが含まれます。 詳しくは、 [データ収集の概要を読むか](../ingestion/home.md) 、 [ソースのドキュメントに直接進んでください](../sources/home.md)。

## UIとAPIでソースコネクタを作成する

ソースコネクタを使用すると、複数のソースからデータを取り込むことができます。ソースコネクタでは、Platform Servicesを使用して、データのラベル付け、構造化、拡張を行うことができます。 ソースコネクタの作成を開始するには、 [ソースの概要を参照してください](../sources/home.md)。

## バッチデータを取り込む

Adobe Experience Platformを使用すると、データをバッチファイルとしてプラットフォームに簡単に読み込むことができます。 取り込むデータの例としては、CRMシステムのフラットファイル（パーケファイルなど）のプロファイルデータ、またはスキーマレジストリの既知のエクスペリエンスデータモデル(XDM)スキーマに適合するデータが挙げられます。 使用を開始するには、プラットフォームの [インジェストデータのチュートリアルにアクセスし](../ingestion/tutorials/ingest-batch-data.md)ます。

## CSVファイルのXDMスキーマへのマップ

CSVデータをAdobe Experience Platformに取り込むには、そのデータをExperience Data Model(XDM)スキーマにマッピングする必要があります。 Experience Platformユーザーインターフェイスを使用してCSVファイルをXDMスキーマにマップする手順については、「CSVファイルのXDMスキーマへの [マップ」チュートリアルに従ってください](../ingestion/tutorials/map-a-csv-file.md)。

## ストリーミング接続の作成

Experience Platformに開始ストリーミングデータを送信するには、まずHTTPエンドポイントをリクエストする必要があります。 認証済みの動作を強制するように、このエンドポイントを設定するオプションがあります。 これは、プラットフォームのユーザーインターフェイスまたはエクスペリエンスプラットフォームAPIを使用して行うことができます。 詳しくは、UIを使用してストリーミング接続を [作成する場合、またはAPIを使用してストリーミング接続を](../ingestion/tutorials/create-streaming-connection-ui.md) 作成する場合のチュートリアルに従ってください [](../ingestion/tutorials/create-streaming-connection.md)。

## 認証済みストリーミング接続の作成

認証済みデータ収集機能を使用すると、リアルタイム顧客プロファイルやIDなどのAdobe Experience Platformサービスで、信頼できるソースからのレコードと信頼できないソースからのレコードを区別できます。 開始するには、認証済みストリーミング接続の [作成に関するチュートリアルに従ってください](../ingestion/tutorials/create-authenticated-streaming-connection.md)。

## ストリームレコードと時系列データ

データセット接続とストリーミング接続を使用すると、レコードや時系列のデータをプラットフォームにストリーミング配信できます。 記録データのストリーミングを開始するには、ストリー [ム記録データに従って、プラットフォームのチュートリアルに進み](../ingestion/tutorials/streaming-record-data.md)ます。 時系列データのストリーミングを開始するには、ストリー [ム時系列データに従ってプラットフォーム](../ingestion/tutorials/streaming-time-series-data.md)に入ります。

## 1回のHTTPリクエストで複数のメッセージをストリーミング

データをAdobe Experience Platformにストリーミングする場合、多数のHTTP呼び出しを行うと費用がかかる場合があります。 例えば、1 KBのペイロードで200個のHTTPリクエストを作成する代わりに、200 KBのペイロードでそれぞれ1 KBのメッセージを200 KBずつ含む1個のHTTPリクエストを作成する方が、はるかに効率的です。 正しく使用すれば、1つのリクエスト内で複数のメッセージをグループ化することは、エクスペリエンスプラットフォームに送信されるデータを最適化する優れた方法です。 ストリーミング取り込みを使用して、1つのHTTPリクエスト内で複数のメッセージをエクスペリエンスプラットフォームに送信する方法を学ぶには、「複数のメッセージの [送信」チュートリアルに従い](../ingestion/tutorials/streaming-multiple-messages.md)ます。



