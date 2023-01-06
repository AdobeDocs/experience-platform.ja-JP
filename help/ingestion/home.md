---
keywords: Experience Platform;ホーム;人気のトピック;データ取り込み;データの場所;データの場所;データ管理;データ管理;系列;系列;バッチ;バッチ;取り込んだデータ
solution: Experience Platform
title: データ取り込みの概要
description: このドキュメントでは、データを Platform に取り込む 3 つの主な方法を紹介し、詳細については、それぞれの概要ドキュメントへのリンクを示します。
exl-id: c189dd4a-5c59-4189-a18c-a3e45a9ff01d
source-git-commit: e802932dea38ebbca8de012a4d285eab691231be
workflow-type: tm+mt
source-wordcount: '461'
ht-degree: 100%

---

# データ取り込みの概要

Adobe Experience Platform では、複数のソースからのデータを統合し、マーケターが顧客の行動をより深く理解できるようにします。Adobe Experience Platform のデータ取り込みは、[!DNL Platform] がこれらのソースからデータを取得する複数の方法と、そのデータが Data Lake 内でどのように保持され、ダウンストリームの [!DNL Platform] サービスで使用されるかを表します。

このドキュメントでは、データを [!DNL Platform] に取り込む 3 つの主な方法を紹介し、詳細については、それぞれの概要ドキュメントへのリンクを示します。

## バッチ取得

バッチ取り込みでは、データをバッチファイルとして [!DNL Experience Platform] に取り込むことができます。バッチとは、単一の単位として取得される 1 つ以上のファイルで構成されるデータの単位です。取り込まれたバッチには、正常に取り込まれたレコードの数と、失敗したレコードの数、および関連するエラーメッセージを示すメタデータが提供されます。

フラットな CSV ファイル（XDM スキーマにマッピングされる）や Parket データフレームなど、手動でアップロードしたデータファイルは、この方法を使用して取り込む必要があります。

詳しくは、[バッチ取り込みの概要](./batch-ingestion/overview.md)を参照してください。

## ストリーミング取得

ストリーミング取得では、クライアントサイドおよびサーバーサイドのデバイスから、リアルタイムで [!DNL Experience Platform] にデータを送信できます。[!DNL Platform] では、受信したエクスペリエンスデータをストリーミングするためのデータインレットの使用をサポートしています。このデータインレットは、データレイク内のストリーミング対応データセットで保持されます。データインレットは、収集したデータを自動的に認証するように設定でき、信頼できるソースからのデータであることを確認できます。

詳しくは、[ストリーミング取得の概要](./streaming-ingestion/overview.md)を参照してください。

## ソース

[!DNL Experience Platform] では、様々なデータプロバイダーへのソース接続を設定できます。これらの接続を使用すると、外部データソースの認証、取り込みの実行時間の設定、取り込みスループットの管理をおこなうことができます。

ソース接続は、他の Adobe アプリケーション（Adobe Analytics や Adobe Audience Manager など）、サードパーティのクラウドストレージソース（[!DNL Azure Blob]、[!DNL Amazon]、S3、FTP サーバー、SFTP サーバーなど）、サードパーティの CRM システム（[!DNL Microsoft Dynamics] や [!DNL Salesforce] など）からデータを収集するように設定できます。

詳しくは、[ソースの概要](../sources/home.md)を参照してください。

## 次の手順とその他のリソース

このドキュメントでは、[!DNL Experience Platform] での [!DNL Data Ingestion] の様々な側面について簡単に説明しました。各取り込み方法の概要ドキュメントを引き続き参照して、それぞれの機能、ユースケース、ベストプラクティスをよく理解してください。また、次の取り込みの概要ビデオを見ることで、理解を補うこともできます。取り込んだレコードのメタデータを [!DNL Experience Platform] で追跡する方法について詳しくは、[Catalog Service の概要](../catalog/home.md) を参照してください。

>[!WARNING]
>
>次のビデオで使用されている「統合プロファイル」という用語は、今は使われていません。[!DNL "Profile"]または[!DNL "Real-Time Customer Profile"]という用語は、[!DNL Experience Platform] のドキュメントで使用されている正しい用語です。最新の機能については、ドキュメントを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/27106?quality=12&learn=on)
