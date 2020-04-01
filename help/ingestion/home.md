---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platform Data Engestの概要
topic: overview
translation-type: tm+mt
source-git-commit: 4817162fe2b7cbf4ae4c1ed325db2af31da5b5d3

---


# データ取り込みの概要

Adobe Experience Platformは、複数のソースからのデータを統合し、マーケターが顧客の行動をより深く理解できるようにします。 Adobe Experience Platform Data Ingestionは、Platformがこれらのソースからデータを取り込む複数の方法と、そのデータがData Lake内でどのように保持され、ダウンストリームのPlatformサービスで使用されるかを表します。

このドキュメントでは、データをプラットフォームに取り込む3つの主な方法を紹介し、詳細については、それぞれの概要ドキュメントへのリンクを示します。

## バッチの取り込み

バッチインジェストを使用すると、データをバッチファイルとしてエクスペリエンスプラットフォームに取り込むことができます。 バッチとは、単一の単位として取り込まれる1つ以上のファイルで構成されるデータの単位です。 取り込まれたバッチは、正常に取り込まれたレコードの数、失敗したレコードおよび関連するエラーメッセージを示すメタデータを提供します。

フラットなCSVファイル(XDMスキーマにマッピング)やParketのデータフレームなど、手動でアップロードしたデータファイルは、この方法を使用して取り込む必要があります。

See the [batch ingestion overview](./batch-ingestion/overview.md) for more information.

## ストリーミングの取り込み

ストリーミング取り込みを使用すると、クライアント側およびサーバ側のデバイスからExperience Platformにデータをリアルタイムで送信できます。 プラットフォームは、受信したエクスペリエンスデータをストリーミングするためのデータインレットの使用をサポートしています。このデータインレットは、Data Lake内のストリーミング対応データセットで保持されます。 データ入力は、収集したデータを自動的に認証するように設定でき、信頼できるソースからのデータであることを確認できます。

See the [streaming ingestion overview](./streaming-ingestion/overview.md) for more information.

## ソース

エクスペリエンスプラットフォームを使用すると、様々なデータプロバイダーへのソース接続を設定できます。 これらの接続を使用すると、外部データソースの認証、取り込みの実行時間の設定、取り込みスループットの管理を行うことができます。

ソース接続は、他のAdobeオーディエンス（Adobe AnalyticsやAdobe Application Managerなど）、サードパーティのクラウドストレージソース（Azure Blob、Amazon S3、FTPサーバー、SFTPサーバーなど）、およびサードパーティのCRMシステム（Microsoft DynamicsやSalesforceなど）からデータを収集するように設定できます。

See the [Sources overview](../source-connectors/home.md) for more information.

## 次の手順

このドキュメントでは、Experience Platformでのデータ取り込みの様々な側面について簡単に説明しました。 各取り込み方法の概要ドキュメントを読み続けて、それぞれの機能、使用事例、ベストプラクティスについて理解してください。 取り込んだレコードのメタデータをExperience Platformが追跡する方法について詳しくは、 [Catalog Serviceの概要を参照してください](../catalog/home.md)。