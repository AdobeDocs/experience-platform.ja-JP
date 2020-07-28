---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: カタログサービスの概要
topic: overview
translation-type: tm+mt
source-git-commit: bfbf2074a9dcadd809de043d62f7d2ddaa7c7b31
workflow-type: tm+mt
source-wordcount: '732'
ht-degree: 39%

---


# [!DNL Catalog Service] 概要

[!DNL Catalog Service] は、Adobe Experience Platform内のデータの場所と系列のレコードシステムです。 While all data that is ingested into [!DNL Experience Platform] is stored in the [!DNL Data Lake] as files and directories, [!DNL Catalog] holds the metadata and description of those files and directories for lookup and monitoring purposes.

Simply put, [!DNL Catalog] acts as a metadata store or &quot;[!UICONTROL catalog]&quot; where you can find information about your data within [!DNL Experience Platform]. You can use [!DNL Catalog] to answer the following questions:

* データの場所
* データが処理のどの段階であるか。
* データに対してどのようなシステムまたはプロセスが作用しているか。
* 正常に処理されたデータの量
* 処理中に発生したエラー

[!DNL Catalog] には、基本的なCRUD操作を使用してメタデータをプログラムで管理できるRESTful APIが用意されています。 [!DNL Platform] 詳しくは、『[カタログ開発者ガイド](api/getting-started.md)』を参照してください。

## [!DNL Catalog] および [!DNL Experience Platform] サービス

The resources that [!DNL Catalog Service] tracks are used by multiple [!DNL Experience Platform] services. In order to make the most of [!DNL Catalog's] capabilities, it is recommended that you become familiar with these services and how they interact with [!DNL Catalog].

### [!DNL Experience Data Model] (XDM)システム

[!DNL Experience Data Model] (XDM)システムは、顧客体験データを [!DNL Platform] 編成する標準化されたフレームワークです。 [!DNL Experience Platform] は、XDM スキーマを活用して、一貫した再利用可能な方法でデータの構造を記述します。

When data is ingested into [!DNL Platform], the structure of that data is mapped to an XDM schema and stored within the [!DNL Data Lake] as part of a **dataset**. The metadata for each dataset is tracked by [!DNL Catalog Service], which includes a reference to the XDM schema that the dataset conforms to.

XDM システムの一般的な情報については、「[XDM システムの概要](../xdm/home.md)」を参照してください。

### [!DNL Data Ingestion]

[!DNL Experience Platform] 複数のソースからデータを取り込み、レコードをデータセットとして内に保持 [!DNL Data Lake]します。 [!DNL Catalog] 取り込み元や取り込み方法に関係なく、これらのデータセットのメタデータを追跡します。

When using the batch ingestion method, [!DNL Catalog] also tracks additional metadata for **batch** files. バッチとは、単一の単位として取得される 1 つ以上のファイルで構成されるデータの単位です。[!DNL Catalog] これらのバッチファイルのメタデータと、取り込み後に保持されるデータセットを追跡します。 バッチメタデータには、正常に取得されたレコードの数、失敗したレコードおよび関連するエラーメッセージに関する情報が含まれます。

詳しくは、「[データ取得の概要](../ingestion/home.md)」を参照してください。

## [!DNL Catalog] オブジェクト

As outlined in the previous section, [!DNL Catalog] tracks metadata for several kinds of resources and operations that are used by other [!DNL Platform] services. [!DNL Catalog] は、このメタデータをカプセル化する「オブジェクト」の独自のストアを維持します。 [!DNL Catalog] オブジェクトは [!DNL Platform] データをクエリー可能に表したもので、データ自体にアクセスする必要なく、データの検索、監視、ラベル付けを行うことができます。

The following table outlines the different object types supported by [!DNL Catalog]:

| オブジェクト | API エンドポイント | 定義 |
|---|---|---|
| アカウント | `/accounts` | ソース接続を作成する場合は、認証資格情報を指定する必要があります。アカウントは、特定の種類の接続の作成に使用された認証資格情報のコレクションを表します。Each connection has a set of unique parameters that are persisted by [!DNL Catalog] and secured in an [!DNL Azure Key Vault]. |
| バッチ | `/batches` | バッチとは、単一の単位として取得される 1 つ以上のファイルで構成されるデータの単位です。A batch object in [!DNL Catalog] outlines the batch&#39;s ingestion metrics (such as the number of records processed or size on disk) and may also include links to datasets, views, and other resources that were affected by the batch operation. |
| 接続 | `/connections` | 接続はソースコネクタの単一インスタンスです。接続は組織に固有で、コネクタの種類に適した認証資格情報を使用して構成されます。 |
| コネクタ | `/connectors` | Connectors define how source connections are to gather data from other Adobe applications (such as Adobe Analytics and Adobe Audience Manager), third-party cloud storage sources (such as [!DNL Azure Blob], [!DNL Amazon S3], FTP servers, and SFTP servers), and third-party CRM systems (such as [!DNL Microsoft Dynamics] and [!DNL Salesforce]). |
| データセット | `/dataSets` | データセットは、スキーマ（列）とフィールド（行）を含むデータ（通常はテーブル）の収集に使用されるストレージと管理の構成体です。See the [datasets overview](./datasets/overview.md) for more information. |
| データセットファイル | `/datasetFiles` | Dataset files represent blocks of data that has been saved on [!DNL Platform]. リテラルファイルのレコードについては、ファイルのサイズ、ファイルに含まれるレコードの数、およびファイルを取得したバッチへの参照を見つけることができます。 |

## 次の手順

This document provided an introduction to [!DNL Catalog Service] and how it functions within the greater scope of [!DNL Experience Platform]. See the [Catalog developer guide](api/getting-started.md) for steps on interacting with the different endpoints of that [!DNL Catalog] API. API 応答で返されるデータを制限するベストプラクティスに従うために、[カタログデータのフィルタリング](api/filter-data.md)に関するガイドも参照することをお勧めします。