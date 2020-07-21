---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: カタログサービスの概要
topic: overview
translation-type: tm+mt
source-git-commit: bfbf2074a9dcadd809de043d62f7d2ddaa7c7b31
workflow-type: tm+mt
source-wordcount: '732'
ht-degree: 6%

---


# [!DNL Catalog Service]概要

[!DNL Catalog Service] は、Adobe Experience Platform内のデータの場所と系列のレコードシステムです。 に取り込まれるすべてのデータ [!DNL Experience Platform] は、ファイルとディレクトリ [!DNL Data Lake] としてに保存されますが、では、参照と監視のために、これらのファイルとディレクトリのメタデータと説明が [!DNL Catalog] 保持されます。

簡単に言うと、メタデータストア [!DNL Catalog] または「[!UICONTROL カタログ]」の役割を果たします。ここで、内のデータに関する情報を見つけることができ [!DNL Experience Platform]ます。 を使用して、次 [!DNL Catalog] の質問に答えることができます。

* データの場所
* このデータは処理のどの段階ですか。
* データに対してどのようなシステムまたはプロセスが作用したか。
* 正常に処理されたデータ量。
* 処理中に発生したエラー

[!DNL Catalog] には、基本的なCRUD操作を使用してメタデータをプログラムで管理できるRESTful APIが用意されています。 [!DNL Platform] 詳しくは、 [カタログ開発ガイド](api/getting-started.md) を参照してください。

## [!DNL Catalog] および [!DNL Experience Platform] サービス

追跡するリソースは、複数のサー [!DNL Catalog Service][!DNL Experience Platform] ビスで使用されます。 機能を最大限に活用するために、これらのサービスとのやり取りについて理解することをお勧めし [!DNL Catalog's][!DNL Catalog]ます。

### [!DNL Experience Data Model] (XDM)システム

[!DNL Experience Data Model] (XDM)システムは、顧客体験データを [!DNL Platform] 編成する標準化されたフレームワークです。 [!DNL Experience Platform] XDMスキーマを利用して、一貫した再利用可能な方法でデータの構造を記述します。

データがに取り込まれ [!DNL Platform]ると、そのデータの構造がXDMスキーマにマッピングされ、 [!DNL Data Lake] データセットの一部とし **てに格納されます**。 各データセットのメタデータは、データセットが適合するXDMスキーマへの参照を含む、 [!DNL Catalog Service]によって追跡されます。

XDMシステムの一般情報について詳しくは、 [XDMシステムの概要を参照してください](../xdm/home.md)。

### [!DNL Data Ingestion]

[!DNL Experience Platform] 複数のソースからデータを取り込み、レコードをデータセットとして内に保持 [!DNL Data Lake]します。 [!DNL Catalog] 取り込み元や取り込み方法に関係なく、これらのデータセットのメタデータを追跡します。

バッチ取り込み方法を使用する場合は、バッ [!DNL Catalog] チ **** ファイルの追加のメタデータも追跡します。 バッチとは、1 つの単位として取り込まれる 1 つ以上のファイルで構成されるデータの単位です。[!DNL Catalog] これらのバッチファイルのメタデータと、取り込み後に保持されるデータセットを追跡します。 バッチメタデータには、正常に取り込まれたレコードの数、失敗したレコードおよび関連するエラーメッセージに関する情報が含まれます。

See the [data ingestion overview](../ingestion/home.md) for more information.

## [!DNL Catalog] オブジェクト

前の節で説明したように、他のサー [!DNL Catalog][!DNL Platform] ビスで使用される複数の種類のリソースおよび操作のメタデータを追跡します。 [!DNL Catalog] は、このメタデータをカプセル化する「オブジェクト」の独自のストアを維持します。 [!DNL Catalog] オブジェクトは [!DNL Platform] データをクエリー可能に表したもので、データ自体にアクセスする必要なく、データの検索、監視、ラベル付けを行うことができます。

次の表に、でサポートされる各種オブジェクトの概要を示し [!DNL Catalog]ます。

| オブジェクト | APIエンドポイント | 定義 |
|---|---|---|
| アカウント | `/accounts` | ソース接続を作成する場合は、認証資格情報を指定する必要があります。 アカウントは、特定の種類の接続の作成に使用された認証資格情報のコレクションを表します。 各接続には、によって保持され、に保護される一意のパラメーターのセット [!DNL Catalog] があり [!DNL Azure Key Vault]ます。 |
| バッチ | `/batches` | バッチとは、1 つの単位として取り込まれる 1 つ以上のファイルで構成されるデータの単位です。バッチオブジェクトには、バッチの取り込み指標（ディスク上の処理済みレコード数やサイズなど）の概要が [!DNL Catalog] 記載され、バッチ処理の影響を受けたデータセット、表示、その他のリソースへのリンクが含まれる場合があります。 |
| 接続 | `/connections` | 接続とは、組織に固有で、コネクタタイプに対して適切な認証資格情報を使用して設定された、ソースコネクタの単一のインスタンスです。 |
| コネクタ | `/connectors` | コネクターは、他のアドビアプリケーション(アドビのAnalyticsやAdobe Audience Managerなど)、サードパーティのクラウドストレージソース( [!DNL Azure Blob]、 [!DNL Amazon S3]、FTPサーバー、SFTPサーバーなど)、およびサードパーティのCRMシステム( [!DNL Microsoft Dynamics] となど [!DNL Salesforce])からデータを収集するソース接続の方法を定義します。 |
| データセット | `/dataSets` | データセットは、スキーマ（列）とフィールド（行）を含むデータ（通常はテーブル）の収集に使用されるストレージと管理の構成体です。 See the [datasets overview](./datasets/overview.md) for more information. |
| データセットファイル | `/datasetFiles` | データセットファイルは、保存されたデータのブロックを表し [!DNL Platform]ます。 リテラルファイルの記録として、ファイルのサイズ、ファイルに含まれるレコード数、ファイルを取り込んだバッチへの参照を確認できます。 |

## 次の手順

このドキュメントでは、のより広い範囲での機能 [!DNL Catalog Service] と機能についての紹介を行い [!DNL Experience Platform]ました。 その [APIの異なるエンドポイントとの対話手順については、](api/getting-started.md)[!DNL Catalog] カタログ開発者ガイドを参照してください。 API応答で返されるデータを制限するベストプラクティスに従うため、カタログデータの [フィルタリングに関するガイドも参照することをお勧めします](api/filter-data.md) 。