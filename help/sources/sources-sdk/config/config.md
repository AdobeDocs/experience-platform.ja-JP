---
keywords: Experience Platform;ホーム;人気の高いトピック;ソース;コネクタ;ソースコネクタ;ソース sdk;SDK;SDK
title: セルフサービスソースの設定オプション（バッチ SDK）
description: このドキュメントでは、セルフサービスソース（Batch SDK）を使用するために準備が必要な設定の概要を説明します。
exl-id: a41b3b80-599a-47ed-a391-419721be5aa2
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 22%

---

# セルフサービスソースの設定オプション（バッチ SDK）

このドキュメントでは、セルフサービスソース（Batch SDK）を使用するために準備が必要な設定の概要を説明します。

## 接続仕様

接続仕様は、ソースのコネクタプロパティを返します。 ベース接続とソース接続の作成に関連する認証仕様、および特定のソースに割り当てられる固定接続仕様 ID が含まれます。 接続仕様はテナントおよび組織に依存しません。 一般的な接続仕様には、特定のソースに関する基本情報と、`authSpec`、`sourceSpec`、`exploreSpec` の 3 つの異なるセクションが含まれています。

| 仕様 | 説明 |
| --- | --- |
| `authSpec` | `authSpec` 配列には、ソースをExperience Platformに接続するために必要な認証パラメーターに関する情報が含まれています。 任意の特定のソースは、複数の異なるタイプの認証をサポートできます。 |
| `sourceSpec` | `sourceSpec` 配列には、UI にソースを表示するために必要な属性に関する情報、ドキュメントリンク、ページネーション、ヘッダー、本文、スケジュールに関するパラメーターなど、ソースに関する一般的な情報が含まれています。 さらに、ベース接続からソース接続を作成するために必要なパラメーターのスキーマを `sourceSpec` に記述します。これは、ソース接続を作成するために必要です。 |
| `exploreSpec` | `exploreSpec` 配列は、ソースに含まれるオブジェクトを調査および検査するために必要なパラメーターを定義します。 また、`exploreSpec` は、オブジェクトの調査および検査時に返される応答の形式も定義します。 |

{style="table-layout:auto"}

## 接続仕様値の入力

接続仕様は、認証仕様、ソース仕様および参照仕様の 3 つの異なる部分に分けることができます。

接続仕様の各部分の値を入力する手順については、次のドキュメントを参照してください。

* [認証仕様を設定](./authspec.md)
* [ソース仕様を設定](./sourcespec.md)
* [参照仕様を設定](./explorespec.md)
