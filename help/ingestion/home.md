---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platformデータ取り込みの概要
topic: overview
translation-type: tm+mt
source-git-commit: 3f1c3c77a0755a3e305da0fb8a234be0f0ee1863
workflow-type: tm+mt
source-wordcount: '420'
ht-degree: 10%

---


# データ取り込みの概要

Adobe Experience Platformは、複数のソースからのデータを統合し、マーケティング担当者が顧客の行動をより深く理解できるようにします。 Adobe Experience Platformデータ取り込みは、これらのソースからデータを [!DNL Platform] 取り込む複数の方法、およびダウンストリームサー [!DNL Platform] ビスで使用するデータレーク内でのデータの保持方法を表します。

このドキュメントでは、データを取り込む主な3つの方法について説明し [!DNL Platform]、詳細については、それぞれの概要ドキュメントへのリンクを示します。

## バッチ取り込み

バッチ取り込みを使用すると、データをバッチファイル [!DNL Experience Platform] として取り込むことができます。 バッチとは、1 つの単位として取り込まれる 1 つ以上のファイルで構成されるデータの単位です。取り込まれたバッチには、正常に取り込まれたレコードの数と、失敗したレコードの数、および関連するエラーメッセージを示すメタデータが提供されます。

フラットなCSVファイル(XDMスキーマにマップ)やParketデータフレームなど、手動でアップロードしたデータファイルは、この方法を使用して取り込む必要があります。

See the [batch ingestion overview](./batch-ingestion/overview.md) for more information.

## ストリーミング取り込み

ストリーミング取り込みを使用すると、クライアントおよびサーバ側のデバイスからデータをリアルタイム [!DNL Experience Platform] に送信できます。 [!DNL Platform] は、受信したエクスペリエンスデータをストリーミングするデータ入力の使用をサポートしています。このデータ入力は、Data Lake内のストリーミング対応データセット内で保持されます。 データ入力は、収集したデータを自動的に認証するように設定でき、信頼できるソースからのデータであることを確認できます。

See the [streaming ingestion overview](./streaming-ingestion/overview.md) for more information.

## ソース

[!DNL Experience Platform] 様々なデータプロバイダーへのソース接続を設定できます。 これらの接続を使用すると、外部データソースの認証、インジェストの実行時間の設定、インジェストスループットの管理を行うことができます。

ソース接続は、他のアドビアプリケーション(アドビのAnalyticsやAdobe Audience Managerなど)、サードパーティのクラウドストレージソース( [!DNL Azure Blob][!DNL Amazon] S3、FTPサーバー、SFTPサーバーなど)、およびサードパーティのCRMシステム（Microsoft DynamicsやSalesforceなど）からデータを収集するように設定できます。

See the [Sources overview](../sources/home.md) for more information.

## 次の手順とその他のリソース

このドキュメントでは、の様々な側面について簡単に説明し [!DNL Data Ingestion] ま [!DNL Experience Platform]した。 各取り込み方法の概要に関するドキュメントを引き続き読み、それぞれの機能、使用例、ベストプラクティスについて理解してください。 また、以下のインジェストの概要ビデオを見て、学習内容を伝えることもできます。 取り込んだレコードのメタデータを [!DNL Experience Platform] 追跡する方法について詳しくは、 [Catalog Serviceの概要を参照してください](../catalog/home.md)。

>[!WARNING]
>
> 次のビデオで使用されている「統合プロファイル」という用語は古いです。 ドキュメントで使用され [!DNL "Profile"] る用語または [!DNL "Real-time Customer Profile"] 正しい用語 [!DNL Experience Platform] です。 最新の機能については、ドキュメントを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/27106?quality=12&learn=on)