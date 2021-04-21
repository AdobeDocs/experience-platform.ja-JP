---
keywords: Experience Platform；ホーム；人気のあるトピック；カタログサービス；カタログ；カタログサービス；データの場所；データの場所；データ管理;データ管理；系列；系列；カタログ；データセットを有効にする
solution: Experience Platform
title: カタログサービスの概要
topic-legacy: overview
description: カタログサービスは、Adobe Experience Platform 内のデータの場所と系列のレコードのシステムです。Experience Platform に取得されるすべてのデータはファイルとディレクトリとしてデータレイクに保存されますが、カタログには、参照や監視のために、これらのファイルとディレクトリのメタデータと説明が保持されます。
exl-id: ef0c173b-607b-41b8-8676-c54ae9472e23
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '805'
ht-degree: 41%

---

# [!DNL Catalog Service] の概要

[!DNL Catalog Service] は、Adobe Experience Platform内のデータの位置と系統に関する記録システムです。[!DNL Experience Platform]に取り込まれるすべてのデータは[!DNL Data Lake]にファイルやディレクトリとして保存されますが、[!DNL Catalog]は、参照や監視のために、これらのファイルやディレクトリのメタデータと説明を保持します。

簡単に言うと、[!DNL Catalog]はメタデータストア（「カタログ」）の役割を果たし、[!DNL Experience Platform]内でデータに関する情報を見つけることができます。 [!DNL Catalog]を使って、次の質問に答えることができます。

* データの場所
* データが処理のどの段階であるか。
* データに対してどのようなシステムまたはプロセスが作用しているか。
* 正常に処理されたデータの量
* 処理中に発生したエラー

[!DNL Catalog] には、基本的なCRUD操作を使用して [!DNL Platform] メタデータをプログラムで管理できるRESTful APIが用意されています。詳しくは、『[カタログ開発者ガイド](api/getting-started.md)』を参照してください。

## [!DNL Catalog] および [!DNL Experience Platform] サービス

[!DNL Catalog Service]が追跡するリソースは、複数の[!DNL Experience Platform]サービスで使用されます。 [!DNL Catalog's]の機能を最大限に活用するために、これらのサービスと[!DNL Catalog]とのやり取り方を理解することをお勧めします。

### [!DNL Experience Data Model] (XDM)システム

[!DNL Experience Data Model] (XDM)システムは、顧客体験データを [!DNL Platform] 編成する標準化されたフレームワークです。[!DNL Experience Platform] は、XDM スキーマを活用して、一貫した再利用可能な方法でデータの構造を記述します。

データが[!DNL Platform]に取り込まれると、そのデータの構造はXDMスキーマにマッピングされ、[!DNL Data Lake]にデータセットの一部として保存されます。 各データセットのメタデータは[!DNL Catalog Service]によって追跡されます。この中には、データセットが準拠するXDMスキーマへの参照が含まれています。

XDM システムの一般的な情報については、「[XDM システムの概要](../xdm/home.md)」を参照してください。

### [!DNL Data Ingestion]

[!DNL Experience Platform] 複数のソースからデータを取り込み、レコードをデータセットとして内に保持 [!DNL Data Lake]します。[!DNL Catalog] 取り込み元や取り込み方法に関係なく、これらのデータセットのメタデータを追跡します。

バッチ取り込み方法を使用する場合、[!DNL Catalog]はバッチファイルの追加のメタデータも追跡します。 バッチとは、単一の単位として取得される 1 つ以上のファイルで構成されるデータの単位です。[!DNL Catalog] これらのバッチファイルのメタデータと、取り込み後に保持されるデータセットを追跡します。バッチメタデータには、正常に取得されたレコードの数、失敗したレコードおよび関連するエラーメッセージに関する情報が含まれます。

詳しくは、「[データ取得の概要](../ingestion/home.md)」を参照してください。

## [!DNL Catalog] オブジェクト

前の節で説明したように、[!DNL Catalog]は、他の[!DNL Platform]サービスで使用されるいくつかの種類のリソースと操作のメタデータを追跡します。 [!DNL Catalog] は、このメタデータをカプセル化する「オブジェクト」の独自のストアを維持します。[!DNL Catalog] オブジェクトは [!DNL Platform] データをクエリー可能に表したもので、データ自体にアクセスする必要なく、データの検索、監視、ラベル付けを行うことができます。

次の表に、[!DNL Catalog]がサポートする各種オブジェクトの概要を示します。

| オブジェクト | API エンドポイント | 定義 |
|---|---|---|
| アカウント | `/accounts` | ソース接続を作成する場合は、認証資格情報を指定する必要があります。アカウントは、特定の種類の接続の作成に使用された認証資格情報のコレクションを表します。各接続には、[!DNL Catalog]で保持され、[!DNL Azure Key Vault]で保護される一意のパラメーターのセットがあります。 |
| バッチ | `/batches` | バッチとは、単一の単位として取得される 1 つ以上のファイルで構成されるデータの単位です。[!DNL Catalog]のバッチオブジェクトは、バッチの取り込み指標（ディスク上で処理されたレコード数やサイズなど）の概要を示し、バッチ処理の影響を受けたデータセット、表示、その他のリソースへのリンクを含む場合があります。 |
| 接続 | `/connections` | 接続はソースコネクタの単一インスタンスです。接続は組織に固有で、コネクタの種類に適した認証資格情報を使用して構成されます。 |
| コネクタ | `/connectors` | コネクタは、他のAdobeアプリケーション(Adobe AnalyticsやAdobe Audience Managerなど)、サードパーティのクラウドストレージソース（[!DNL Azure Blob]、[!DNL Amazon S3]、FTPサーバー、SFTPサーバーなど）、およびサードパーティのCRMシステム（[!DNL Microsoft Dynamics]、[!DNL Salesforce]など）からデータを収集するソース接続の方法を定義します。 |
| データセット | `/dataSets` | データセットは、スキーマ（列）とフィールド（行）を含むデータ（通常はテーブル）の収集に使用されるストレージと管理の構成体です。詳しくは、[データセットの概要](./datasets/overview.md)を参照してください。 |
| データセットファイル | `/datasetFiles` | データセットファイルは、[!DNL Platform]に保存されたデータのブロックを表します。 リテラルファイルのレコードについては、ファイルのサイズ、ファイルに含まれるレコードの数、およびファイルを取得したバッチへの参照を見つけることができます。 |

## 次の手順

このドキュメントは[!DNL Catalog Service]の紹介と、[!DNL Experience Platform]の範囲内での機能の仕組みを提供しています。 [!DNL Catalog] APIの異なるエンドポイントとの対話手順については、[[!DNL Catalog] 開発者ガイド](api/getting-started.md)を参照してください。 API 応答で返されるデータを制限するベストプラクティスに従うために、[カタログデータのフィルタリング](api/filter-data.md)に関するガイドも参照することをお勧めします。
