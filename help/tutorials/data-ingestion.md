---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ取り込みのチュートリアル
topic: tutorial
translation-type: tm+mt
source-git-commit: 5c5f6c4868e195aef76bacc0a1e5df3857647bde
workflow-type: tm+mt
source-wordcount: '480'
ht-degree: 0%

---


# データを [!DNL Experience Platform]

Adobe Experience Platformは、複数のソースからのデータを統合し、マーケティング担当者が顧客の行動をより深く理解できるようにします。 アドビ [!DNL Experience Platform Data Ingestion] は、これらのソースからデータを [!DNL Platform] 取り込む複数の方法と、そのデータがData Lake内でどのように保持され、ダウンストリームで使用されるかを表し [!DNL Platform services]ます。 [!DNL Data Ingestion] ソースコネクタを使用したバッチ取り込み、ストリーミング取り込み、取り込みが含まれます。 詳しくは、 [データ収集の概要を読むか](../ingestion/home.md) 、 [ソースのドキュメントに直接進んでください](../sources/home.md)。

## UIとAPIでソースコネクタを作成する

ソースコネクタを使用すると、複数のソースからデータを取り込むことができます。この場合、を使用して、ラベル付け、構造化、拡張を行うことができ [!DNL Platform services]ます。 ソースコネクタの作成を開始するには、 [ソースの概要を参照してください](../sources/home.md)。

## バッチデータを取り込む

Adobe Experience Platformを使用すると、データをバッチファイル [!DNL Platform] としてに簡単にインポートできます。 取り込むデータの例としては、CRMシステムのフラットファイル（パーケファイルなど）のプロファイルデータ、またはスキーマレジストリの既知の [!DNL Experience Data Model] (XDM)スキーマに適合するデータが挙げられる。 使用を開始するには、Platformチュートリアルの [インジェストデータを参照してください](../ingestion/tutorials/ingest-batch-data.md)。

## CSVファイルのXDMスキーマへのマップ

CSVデータをAdobe Experience Platformに取り込むには、データを [!DNL Experience Data Model] (XDM)スキーマにマップする必要があります。 ユーザーインターフェイスを使用してCSVファイルをXDMスキーマにマップする手順については、「CSVファイルのXDMスキーマへの [!DNL Experience Platform] マップ [](../ingestion/tutorials/map-a-csv-file.md)」のチュートリアルに従ってください。

## ストリーミング接続の作成

ストリーミングデータを開始するに [!DNL Experience Platform]は、まずHTTPエンドポイントを要求する必要があります。 認証済みの動作を強制するように、このエンドポイントを設定するオプションがあります。 これは、ユー [!DNL Platform] ザーインターフェイスまたはAPIを使用して行うことがで [!DNL Experience Platform] きます。 詳しくは、UIを使用してストリーミング接続を [作成する場合、またはAPIを使用してストリーミング接続を](../ingestion/tutorials/create-streaming-connection-ui.md) 作成する場合のチュートリアルに従ってください [](../ingestion/tutorials/create-streaming-connection.md)。

## 認証済みストリーミング接続の作成

認証済みデータ収集機能を使用すると、 [!DNL Real-time Customer Profile][!DNL Identity]やなどのAdobe Experience Platformサービスを使用して、信頼できるソースと信頼できないソースからのレコードを区別できます。 開始するには、認証済みストリーミング接続の [作成に関するチュートリアルに従ってください](../ingestion/tutorials/create-authenticated-streaming-connection.md)。

## ストリームレコードと時系列データ

データセット接続とストリーミング接続を使用して、レコードまたは時系列データをにストリーミングでき [!DNL Platform]ます。 記録データのストリーミングを開始するには、 [ストリーム記録データをPlatformチュートリアルに従い](../ingestion/tutorials/streaming-record-data.md)ます。 時系列データのストリーミングを開始するには、 [ストリーム時系列データに従ってPlatform](../ingestion/tutorials/streaming-time-series-data.md)。

## 1回のHTTPリクエストで複数のメッセージをストリーミング

データをAdobe Experience Platformにストリーミングする場合、多数のHTTP呼び出しを作成すると高コストになる場合があります。 例えば、1 KBのペイロードで200個のHTTPリクエストを作成する代わりに、200 KBのペイロードでそれぞれ1 KBのメッセージを200 KBずつ含む1個のHTTPリクエストを作成する方が、はるかに効率的です。 正しく使用すれば、1つのリクエスト内で複数のメッセージをグループ化することは、送信先のデータを最適化する優れた方法で [!DNL Experience Platform]す。 ストリーミング取り込みを使用して、1つのHTTP要求内に複数のメッセージを送信する方法を学ぶには、「複数のメッセージの [!DNL Experience Platform] 送信」のチュートリアルに従い [](../ingestion/tutorials/streaming-multiple-messages.md)ます。



