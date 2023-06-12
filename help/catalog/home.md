---
keywords: Experience Platform;ホーム;人気のトピック;catalog service;カタログ;Catalog service;データの場所;データの場所;データ管理;データ管理;系列;系列;カタログ;データセットの有効化
solution: Experience Platform
title: Catalog Service の概要
description: カタログサービスは、Adobe Experience Platform 内のデータの場所と系列のレコードのシステムです。Experience Platform に取得されるすべてのデータはファイルとディレクトリとしてデータレイクに保存されますが、カタログには、参照や監視のために、これらのファイルとディレクトリのメタデータと説明が保持されます。
exl-id: ef0c173b-607b-41b8-8676-c54ae9472e23
source-git-commit: 0ebe9eadb1bce6252b43a50af009ce1b0f6e5d6e
workflow-type: ht
source-wordcount: '694'
ht-degree: 100%

---

# [!DNL Catalog Service] の概要

[!DNL Catalog Service] は、Adobe Experience Platform 内のデータの場所と系列の記録システムです。[!DNL Experience Platform] に取り込まれるすべてのデータは [!DNL Data Lake] にファイルおよびディレクトリとして保存されますが、[!DNL Catalog] には、参照や監視のために、これらのファイルおよびディレクトリのメタデータと説明が保持されます。

簡単に言えば、[!DNL Catalog] はメタデータストア（「カタログ」）として機能し、[!DNL Experience Platform] 内のデータに関する情報を検索できます。[!DNL Catalog] を使用して、次の質問に答えることができます。

* データの場所
* データが処理のどの段階であるか。
* データに対してどのようなシステムまたはプロセスが作用しているか。
* 正常に処理されたデータの量
* 処理中に発生したエラー

[!DNL Catalog] には、基本的な CRUD 操作を使用して [!DNL Platform] メタデータをプログラムで管理できる RESTful API が用意されています。詳しくは、『[カタログ開発者ガイド](api/getting-started.md)』を参照してください。

## [!DNL Catalog] および [!DNL Experience Platform] サービス

[!DNL Catalog Service] で追跡するリソースは、複数の [!DNL Experience Platform] サービスで使用されます。[!DNL Catalog's] の機能を最大限に活用するために、これらのサービスと、それらの [!DNL Catalog] とのやり取りについて理解することをお勧めします。

### [!DNL Experience Data Model]（XDM）システム

[!DNL Experience Data Model]（XDM）システムは、[!DNL Platform] が顧客体験データを整理する際に使用する標準化されたフレームワークです。[!DNL Experience Platform] は、XDM スキーマを活用して、一貫した再利用可能な方法でデータの構造を記述します。

データが [!DNL Platform] に取り込まれると、そのデータの構造が XDM スキーマにマッピングされ、データセットの一部として [!DNL Data Lake] 内に保存されます。各データセットのメタデータは、データセットが準拠する XDM スキーマへの参照を含む [!DNL Catalog Service] によって追跡されます。

XDM システムの一般的な情報については、「[XDM システムの概要](../xdm/home.md)」を参照してください。

### [!DNL Data Ingestion]

[!DNL Experience Platform] は、複数のソースからデータを取得し、[!DNL Data Lake] 内にデータセットとしてレコードを保持します。[!DNL Catalog] は、取得元や取得方法に関係なく、これらのデータセットのメタデータを追跡します。

バッチ取得法を使用する場合、[!DNL Catalog] はバッチファイルの追加のメタデータも追跡します。バッチとは、単一の単位として取得される 1 つ以上のファイルで構成されるデータの単位です。[!DNL Catalog] は、これらのバッチファイルのメタデータと、取得後に保持されるデータセットを追跡します。バッチメタデータには、正常に取得されたレコードの数、失敗したレコードおよび関連するエラーメッセージに関する情報が含まれます。

詳しくは、「[データ取得の概要](../ingestion/home.md)」を参照してください。

## [!DNL Catalog] オブジェクト

前の節で説明したように、[!DNL Catalog] は、他の [!DNL Platform] サービスで使用される複数の種類のリソースおよび操作のメタデータを追跡します。[!DNL Catalog] は、このメタデータをカプセル化する「オブジェクト」の独自のストアを維持します。[!DNL Catalog]オブジェクトは、データ自体にアクセスする必要なく、データの検索、監視、ラベル付けをおこなえる、[!DNL Platform] データのクエリ可能な表現です。

次の表に、[!DNL Catalog] でサポートされる各種オブジェクトの概要を示します。

| オブジェクト | API エンドポイント | 定義 |
|---|---|---|
| バッチ | `/batches` | バッチとは、単一の単位として取得される 1 つ以上のファイルで構成されるデータの単位です。[!DNL Catalog] 内のバッチオブジェクトは、バッチの取得指標（処理されたレコード数やディスク上のサイズなど）の概要を示し、バッチ操作の影響を受けたデータセット、ビュー、その他のリソースへのリンクも含みます。 |
| データセット | `/dataSets` | データセットは、スキーマ（列）とフィールド（行）を含むデータ（通常はテーブル）の収集に使用されるストレージと管理の構成体です。詳しくは、[データセットの概要](./datasets/overview.md)を参照してください。 |
| データセットファイル | `/datasetFiles` | データセットファイルは、[!DNL Platform] に保存されたデータのブロックを表します。リテラルファイルのレコードについては、ファイルのサイズ、ファイルに含まれるレコードの数、およびファイルを取得したバッチへの参照を見つけることができます。 |

## 次の手順

このドキュメントでは、[!DNL Catalog Service] の概要と、より広い範囲の [!DNL Experience Platform] においてどのように機能するかを説明しました。その [!DNL Catalog] API の様々なエンドポイントの操作手順については、[[!DNL Catalog] デベロッパーガイド](api/getting-started.md)を参照してください。API 応答で返されるデータを制限するベストプラクティスに従うために、[カタログデータのフィルタリング](api/filter-data.md)に関するガイドも参照することをお勧めします。
