---
keywords: Experience Platform；ホーム；人気の高いトピック；PSQL;psql;クエリサービス；クエリサービス；メタデータ；コマンド；メタデータコマンド；
solution: Experience Platform
title: クエリサービスのMetadata PostgreSQLコマンド
topic-legacy: metadata
description: 現在、Adobe Experience Platformクエリサービス内のメタデータのクエリに対してサポートされているPostgreSQLコマンドのリストです。
exl-id: bfcbad55-3086-44c9-9938-6ba0504e747b
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '338'
ht-degree: 77%

---

# クエリサービスのMetadata PostgreSQLコマンド

データセットのメタデータについては、現在、次のPostgreSQLコマンドがクエリ用にサポートされています。

>[!NOTE]
>
> 以下に示すコマンドでは、大文字と小文字が区別されます。

| コマンド | 説明 |
|------- | ------------|
| `\conninfo` | 現在のデータベース接続に関する情報を出力します。 |
| `\d` | 表示可能なすべてのテーブル、ビュー、マテリアライズドビュー、シーケンスおよび外部テーブルのリストが表示されます。 |
| `\dE` | 外部テーブルのリストを表示します。 |
| `\df or \df+` | 関数のリストを表示します。 |
| `\di` | インデックスのリストを表示します。 |
| `\dm` | マテリアライズドビューのリストを表示します。 |
| `\dn` | スキーマ（名前空間）のリストを表示します。 |
| `\ds` | シーケンスのリストを表示します。 |
| `\dS` | PostgreSQL で定義されたテーブルのリストを表示します。 |
| `\dt` | テーブルのリストを表示します。 |
| `\dT` | データタイプのリストを表示します。 |
| `\dv` | ビューのリストを表示します。 |
| `\encoding` | 現在のクライアント文字セットのエンコーディングを一覧表示します。 |
| `\errverbose` | 最大限詳細に、最近のサーバーエラーメッセージを繰り返します。 |
| `\l or \list` | サーバー内のリストのデータベースを表示します。 |
| `\set` | 現在のすべての psql 変数の名前と値を表示します。 |
| `\showtables` | 次の情報を表示します。<br>name：テーブルの参照元の名前。<br>datasetId：保存されるデータセットの ID。<br>dataset：保存されるデータセットの名前。<br>description：データセットの説明。<br>resolved：現在のセッションでデータセットが解決されたかどうかのステータスを示すブール値。 |
| `\timing` | 表示のオンとオフを切り替えます。表示はミリ秒単位です。1 秒より長い間隔は、「分:秒」の形式で表示され、必要に応じて時間と日付のフィールドが追加されます。 |

`\d` で開始するすべてのコマンドを組み合わせることができます。例えば、すべてのテーブル、シーケンスおよびリストの `\dtsn` スキーマを表示できます。`\d` を使用すると、すべての表示可能な表、表示、マテリアライズドビュー、シーケンスが表示されます。

上記のコマンドの詳細は、[postgresql.org](https://www.postgresql.org/docs/10/app-psql.html) にあるドキュメントを参照してください。ただし、PostgreSQLのドキュメントに示すすべてのオプションが[!DNL Experience Platform]でサポートされているわけではありません。
