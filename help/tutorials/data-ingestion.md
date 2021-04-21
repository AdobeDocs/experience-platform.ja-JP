---
keywords: Experience Platform；ホーム；人気の高いトピック
solution: Experience Platform
title: データ取得のチュートリアル
topic-legacy: tutorial
type: Tutorial
description: データ取得には、バッチ取得、ストリーミング取得、ソースコネクタを使用した取得が含まれます。
exl-id: 51627acf-e90b-4911-aa54-4a59f3b6a8f9
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '497'
ht-degree: 40%

---

# データを[!DNL Experience Platform]に取り込む

Adobe Experience Platform は、複数のソースからのデータを統合し、マーケターが顧客の行動をより深く理解できるようにします。Adobe[!DNL Experience Platform Data Ingestion]は、[!DNL Platform]がこれらのソースからデータを取り込む複数のメソッドを表します。また、そのデータがData Lake内でどのように保持され、ダウンストリーム[!DNL Platform services]で使用されるかを表します。 [!DNL Data Ingestion] ソースコネクタを使用したバッチ取り込み、ストリーミング取り込み、取り込みが含まれます。詳しくは、「[データ取得の概要](../ingestion/home.md)」を読むか、[ソースのドキュメント](../sources/home.md)に直接進んでください。

## UIとAPIでのソース接続の作成

ソースコネクタを使用すると、複数のソースからデータを取り込むことができ、[!DNL Platform services]を使用してラベル付け、構造化、拡張を行うことができます。 ソースコネクタの作成を開始するには、[ソースの概要](../sources/home.md)を参照してください。

## バッチデータの取得

Adobe Experience Platformでは、データをバッチファイルとして[!DNL Platform]に簡単にインポートできます。 取り込むデータの例としては、CRMシステムのフラットファイル（Parketファイルなど）のプロファイルデータ、またはスキーマレジストリの既知の[!DNL Experience Data Model](XDM)スキーマに適合するデータが挙げられる。 開始するには、『[Platform へのデータ取得のチュートリアル](../ingestion/tutorials/ingest-batch-data.md)』に進みます。

## CSV ファイルの XDM スキーマへのマッピング

CSVデータをAdobe Experience Platformに取り込むには、データを[!DNL Experience Data Model] (XDM)スキーマにマップする必要があります。 [!DNL Experience Platform]ユーザーインターフェイスを使用してCSVファイルをXDMスキーマにマップする手順については、[CSVファイルをXDMスキーマチュートリアルにマップ](../ingestion/tutorials/map-a-csv-file.md)の手順に従ってください。

## ストリーミング接続の作成

[!DNL Experience Platform]に開始ストリーミングデータを送信するには、まずHTTPエンドポイントを要求する必要があります。 認証済みの動作を強制するように、このエンドポイントを設定するオプションがあります。 これは、[!DNL Platform]ユーザーインターフェイスまたは[!DNL Experience Platform] APIを使用して行うことができます。 詳しくは、[UI を使用したストリーミング接続の作成](../ingestion/tutorials/create-streaming-connection-ui.md)または [API を使用したストリーミング接続の作成](../ingestion/tutorials/create-streaming-connection.md)に関するチュートリアルに従ってください。

## 認証済みストリーミング接続の作成

認証済みのデータ収集機能を使用すると、[!DNL Real-time Customer Profile]や[!DNL Identity]などのAdobe Experience Platformサービスで、信頼できるソースと信頼できないソースからのレコードを区別できます。 開始するには、[認証済みのストリーミング接続を作成](../ingestion/tutorials/create-authenticated-streaming-connection.md)するためのチュートリアルに従ってください。

## ストリームレコードと時系列データ

データセット接続とストリーミング接続を使用すると、レコードや時系列のデータを[!DNL Platform]にストリーミングできます。 レコードデータのストリーミングを開始するには、『[Platform へのレコードデータのストリーミングのチュートリアル](../ingestion/tutorials/streaming-record-data.md)』に進みます。時系列データのストリーミングを開始するには、「[Platform への時系列データのストリーミング](../ingestion/tutorials/streaming-time-series-data.md)」に従います。

## 1 回の HTTP リクエストでの複数メッセージのストリーミング

データを Adobe Experience Platform にストリーミングする場合、多数の HTTP 呼び出しを作成するコストが高くなる可能性があります。例えば、1 KB のペイロードで 200 個の HTTP リクエストを作成するよりも、それぞれが 1 KB の 200 個のメッセージが含まれた 1 個の HTTP リクエスト（200 KB の単一ペイロード）を作成する方がはるかに効率的です。正しく使用すれば、1つのリクエスト内で複数のメッセージをグループ化するのは、[!DNL Experience Platform]に送信されるデータを最適化する優れた方法です。 ストリーミング取り込みを使用して、1つのHTTP要求内に複数のメッセージを[!DNL Experience Platform]に送信する方法を学ぶには、[複数のメッセージの送信チュートリアル](../ingestion/tutorials/streaming-multiple-messages.md)に従ってください。
