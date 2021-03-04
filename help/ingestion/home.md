---
keywords: Experience Platform；ホーム；人気のあるトピック；データ取り込み；データの場所；データの場所；データ管理;データ管理；系列；バッチ；取り込んだデータ
solution: Experience Platform
title: データ取り込みの概要
topic: 概要
description: このドキュメントでは、データをプラットフォームに取り込む 3 つの主な方法を紹介し、詳細については、それぞれの概要ドキュメントへのリンクを示します。
translation-type: tm+mt
source-git-commit: 126b3d1cf6d47da73c6ab045825424cf6f99e5ac
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 51%

---


# データ取り込みの概要

Adobe Experience Platform では、複数のソースからのデータを統合し、マーケターが顧客の行動をより深く理解できるようにします。Adobe Experience Platformデータインジェストは、[!DNL Platform]がこれらのソースからデータを取り込む複数の方法と、そのデータがData Lake内でどのように保持され、ダウンストリーム[!DNL Platform]サービスで使用されるかを表します。

このドキュメントでは、データが[!DNL Platform]に取り込まれる3つの主な方法を紹介し、詳細については、それぞれの概要ドキュメントへのリンクを示します。

## バッチ取得

バッチ取り込みを使用すると、データをバッチファイルとして[!DNL Experience Platform]に取り込むことができます。 バッチとは、1 つの単位として取り込まれる 1 つ以上のファイルで構成されるデータの単位です。取り込まれたバッチには、正常に取り込まれたレコードの数と、失敗したレコードの数、および関連するエラーメッセージを示すメタデータが提供されます。

フラットな CSV ファイル（XDM スキーマにマッピングされる）や Parket データフレームなど、手動でアップロードしたデータファイルは、この方法を使用して取り込む必要があります。

詳しくは、[バッチ取り込みの概要](./batch-ingestion/overview.md)を参照してください。

## ストリーミング取り込み

ストリーミング取り込みを使用すると、クライアント側およびサーバ側のデバイスから[!DNL Experience Platform]にデータをリアルタイムで送信できます。 [!DNL Platform] では、受信したエクスペリエンスデータをストリーミングするためのデータインレットの使用をサポートしています。このデータインレットは、データレイク内のストリーミング対応データセットで保持されます。データインレットは、収集したデータを自動的に認証するように設定でき、信頼できるソースからのデータであることを確認できます。

詳しくは、[ストリーミング取り込みの概要](./streaming-ingestion/overview.md)を参照してください。

## ソース

[!DNL Experience Platform] では、様々なデータプロバイダーへのソース接続を設定できます。これらの接続を使用すると、外部データソースの認証、取り込みの実行時間の設定、取り込みスループットの管理をおこなうことができます。

ソース接続は、他のAdobeアプリケーション(Adobe Analytics、Adobe Audience Managerなど)、サードパーティのクラウドストレージソース（[!DNL Azure Blob]、[!DNL Amazon] S3、FTPサーバー、SFTPサーバーなど）、およびサードパーティのCRMシステム（[!DNL Microsoft Dynamics]、[!DNL Salesforce]など）からデータを収集するように設定できます。

詳しくは、[ソースの概要](../sources/home.md)を参照してください。

## 次の手順 およびその他のリソース

このドキュメントは、[!DNL Experience Platform]の[!DNL Data Ingestion]の様々な側面について簡単に説明しました。 各取り込み方法の概要ドキュメントを引き続き参照して、それぞれの機能、ユースケース、ベストプラクティスをよく理解してください。また、次のインジェストの概要ビデオを見ることで、学習を補うこともできます。 [!DNL Experience Platform]が取り込むレコードのメタデータを追跡する方法について詳しくは、[カタログサービスの概要](../catalog/home.md)を参照してください。

>[!WARNING]
>
>次のビデオで使用されている「統合プロファイル」という用語は古いです。 [!DNL "Profile"]または[!DNL "Real-time Customer Profile"]という用語は、[!DNL Experience Platform]のドキュメントで使用されている正しい用語です。 最新の機能については、ドキュメントを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/27106?quality=12&learn=on)