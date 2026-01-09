---
keywords: Experience Platform；クエリサービス；クエリサービス；クエリ
title: Adobe Experience Platform クエリサービスの概要
description: Adobe Experience Platform クエリサービスを最大限に活用するために必要な手順の分類
exl-id: 36ab9354-23f9-4cb8-bcd4-00fe076386ab
source-git-commit: fa22a0ca0c79d5d62fd39de3a808f84a11a80c4d
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 2%

---

# Adobe Experience Platform [!DNL Query Service] の概要 {#getting-started}

Adobe Experience Platform クエリサービスを使用して、取り込んだデータセットに対して SQL クエリを実行し、複数のソースからデータを結合し、分析、機械学習ワークフローまたはリアルタイム顧客プロファイル用の派生データセットを生成します。 データを取り込んだ後、UI からクエリサービスにアクセスしてインタラクティブな分析とコラボレーションを行うか、API を使用してクエリを自動的かつプログラム的に実行します。

## 前提条件 {#prerequisites}

データのクエリを開始する前に、次のことを確認します。

- **必要な権限**：ユーザーアカウントがExperience Platformのクエリサービスにアクセスできます。 UI でサービスを使用できない場合は、[&#x200B; 権限ドキュメント &#x200B;](../../access-control/home.md#permissions) を確認し、システム管理者に問い合わせてください。
- **データ取り込み**：データがExperience Platformに取り込まれています。

データを取り込む必要がある場合は、[&#x200B; データ取り込みチュートリアルビデオ &#x200B;](https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html) で、データセットの作成、スキーママッピング、取り込み、検証の概要を確認してください。 詳しい情報やその他の学習リソースへのリンクについては、[&#x200B; 取り込みの概要ドキュメント &#x200B;](../../ingestion/home.md) を参照してください。

## クイックスタートのパス

データをExperience Platformに取り込んだら、Experience Platformの [!DNL Query Editor] または Query Service API を使用してクエリサービスの使用を開始できます。

### [!DNL Query Editor]

分析、データ調査および共同クエリ開発に [!DNL Query Editor] を使用します。 UI 機能の概要については、[&#x200B; クエリサービス UI ドキュメント &#x200B;](../ui/overview.md) を参照してください。 UI でクエリを記述および実行する方法については、[[!DNL Query Editor user guide]](../ui/user-guide.md) を参照してください。

### クエリサービス API

Query Service API を使用して、ワークフローの自動化、クエリテンプレートの管理、プログラムによる統合を行います。 Query Service API の使用に関する詳細な手順については、『 [&#x200B; クエリサービス開発者ガイド &#x200B;](../api/getting-started.md) 』を参照してください。

## 次の手順

このドキュメントでは、Experience Platformの [!DNL Query Service] 機能を使用するために必要な前提条件を説明しました。 クエリサービスの機能をすぐに使い始めるには、次のドキュメントを読むことをお勧めします。

- [クエリ実行の一般的なガイダンス](../best-practices/writing-queries.md)
- [クエリサービスの SQL 構文](../sql/syntax.md)
- [SQL を使用した派生データセットの作成](../data-distiller/derived-datasets/create-derived-datasets-with-sql.md)

また、Experience Platformでのクエリサービスのデータ処理におけるメリットについて詳しくは、[&#x200B; 放棄された参照のユースケースのプレゼンテーション &#x200B;](../use-cases/abandoned-browse.md#video-example) をご覧ください。

[!DNL Query Service] の理解を深めるために、次のリソースが役立ちます。

- [[!DNL Query Service] UI の概要](../ui/overview.md)
- [[!DNL Query Service] 資格情報](../ui/credentials.md)
- [クライアント接続の概要](../clients/overview.md)
