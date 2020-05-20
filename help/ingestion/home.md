---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platform Data Ingestの概要
topic: overview
translation-type: tm+mt
source-git-commit: 2f0f155beacbc6a4ba2892ae211a9c0305e969ac
workflow-type: tm+mt
source-wordcount: '391'
ht-degree: 0%

---


# データ取り込みの概要

Adobe Experience Platformは、複数のソースからのデータを統合して、マーケティング担当者が顧客の行動をより深く理解できるようにします。 Adobe Experience Platform Data Ingestは、Platformがこれらのソースからデータを取り込む複数の方法と、そのデータがData Lake内でどのように保持され、ダウンストリームプラットフォームサービスで使用されるかを表します。

このドキュメントでは、データをプラットフォームに取り込む3つの主な方法について説明し、詳細については、それぞれの概要ドキュメントへのリンクを示します。

## バッチ取り込み

バッチインジェストを使用すると、データをバッチファイルとしてエクスペリエンスプラットフォームに取り込むことができます。 バッチとは、1つの単位として取り込まれる1つ以上のファイルで構成されるデータの単位です。 取り込まれたバッチには、正常に取り込まれたレコードの数と、失敗したレコードおよび関連するエラーメッセージを示すメタデータが提供されます。

フラットなCSVファイル(XDMスキーマにマップ)やParketデータフレームなど、手動でアップロードしたデータファイルは、この方法を使用して取り込む必要があります。

See the [batch ingestion overview](./batch-ingestion/overview.md) for more information.

## ストリーミング取り込み

ストリーミング取り込みを使用すると、クライアントおよびサーバー側のデバイスからExperience Platformにデータをリアルタイムで送信できます。 プラットフォームは、受信したエクスペリエンスデータをストリーミングするためのデータインレットの使用をサポートしています。このデータインレットは、Data Lake内のストリーミング対応データセット内で保持されます。 データ入力は、収集したデータを自動的に認証するように設定でき、信頼できるソースからのデータであることを確認できます。

See the [streaming ingestion overview](./streaming-ingestion/overview.md) for more information.

## ソース

エクスペリエンスプラットフォームを使用すると、様々なデータプロバイダーへのソース接続を設定できます。 これらの接続を使用すると、外部データソースの認証、インジェストの実行時間の設定、インジェストスループットの管理を行うことができます。

ソース接続は、他のアドビオーディエンス（Adobe AnalyticsやAdobe Application Managerなど）、サードパーティのクラウドストレージソース（Azure Blob、Amazon S3、FTPサーバー、SFTPサーバーなど）、およびサードパーティのCRMシステム（Microsoft DynamicsやSalesforceなど）からデータを収集するように設定できます。

See the [Sources overview](../sources/home.md) for more information.

## 次の手順

このドキュメントでは、Experience Platformでのデータ取り込みの様々な側面について簡単に説明しました。 各取り込み方法の概要に関するドキュメントを引き続き読み、それぞれの機能、使用例、ベストプラクティスについて理解してください。 Experience Platformが取り込むレコードのメタデータを追跡する方法について詳しくは、 [Catalog Serviceの概要を参照してください](../catalog/home.md)。