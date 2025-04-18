---
keywords: Experience Platform;ホーム;人気の高いトピック;ソース;コネクタ;ソースコネクタ;ソース sdk;SDK;SDK
solution: Experience Platform
title: セルフサービスソース（バッチ SDK）の概要
description: Adobe Experience Platform セルフサービスソース（バッチ SDK）は、Flow Service API を使用して REST API ベースのソースを統合し、データをExperience Platformに取り込むことができる、設定 API のセットです。
exl-id: 5d5449ad-a1ba-402b-a281-0b2d8b704f32
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '592'
ht-degree: 7%

---

# セルフサービスソース（バッチ SDK）の概要

Adobe Experience Platform セルフサービスソース（バッチ SDK）は、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して REST API ベースのソースをExperience Platform ソースカタログに統合できるフレームワークです。 セルフサービスソース（バッチ SDK）には、独自のソースを作成してバッチデータをExperience Platformに取り込むための一連の設定 API が用意されています。

セルフサービスソース（バッチ SDK）を使用すると、次のことができます。

* [!DNL Flow Service] API を使用して、Experience Platform カタログに新しいソースを設定して統合します。
* サポートされる認証タイプやリソースデータの取得方法など、ソースの仕様を定義する。
* 新しいソースに関するユーザー向けドキュメントを作成する。

セルフサービスソースのドキュメントでは、Experience Platformと REST API ベースのソース統合を設定、テスト、リリースし、増え続けるソースカタログにソースを含める手順について説明しています。

![カタログ](./assets/catalog.png)

## ソースについて

Experience Platformでは、外部ソースからデータを取り込むときに、Experience Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

ソースおよびExperience Platformで現在サポートされている様々なソースのリストについて詳しくは、[ ソースの概要 ](../home.md) を参照してください。

## ソースを作成

セルフサービスソースを使用すると、独自の REST API ベースのソースを統合し、[!DNL Flow Service] を使用してデータをExperience Platformに取り込むことができます。 [!DNL Flow Service] API を使用して新しい接続仕様を作成、設定および送信することで、ソースをExperience Platform ソースカタログに統合できます。

新しいソースをExperience Platformに統合する方法については、[ 新しい接続仕様の作成 ](./api/api-overview.md) に関するガイドを参照してください。

## ソースのドキュメント化

ソースが作成されたら、[ ドキュメントガイド ](./documentation/doc-overview.md) を参照して、[!DNL GitHub] web インターフェイスまたは独自のテキストエディターを使用してソースをドキュメント化する手順を確認してください。

## プロセスの概要

Experience Platformでソースを設定する手順の概要を次に示します。

* 詳しくは、[ セルフサービスソース（バッチ SDK） API ガイド ](./api/api-overview.md) を参照してください。
   * [ はじめる前に ](./api/getting-started.md) を参照してください。
   * [ 新しい接続仕様の作成 ](./api/create.md) のチュートリアルに従います。
   * [ 接続仕様の更新 ](./api/update-connection-specs.md) のチュートリアルに従います。
   * [ フロー仕様への新しい接続仕様 ID の追加 ](./api/update-flow-specs.md) に関するチュートリアルに従います
   * [ 新しいソースを送信 ](./api/submit.md) します。
* 接続仕様の構造とプロパティをより深く理解するには、[ セルフサービスソースのコンフィギュレーションオプション（バッチ SDK） ](./config/config.md) に関するガイドを参照してください。
   * ソースに使用できる様々な認証タイプについて詳しくは、[ 認証仕様の設定 ](./config/authspec.md) に関するガイドを参照してください。
   * ソースに対して設定できる様々なページネーションタイプ、スケジュール形式およびカスタムスキーマについて詳しくは、[ ソース仕様の設定 ](./config/sourcespec.md) に関するガイドを参照してください。
   * ソースに含まれるオブジェクトの調査および検査に必要なパラメーターを定義する方法について詳しくは、[ 探索仕様の設定 ](./config/explorespec.md) に関するガイドを参照してください。
* ソースのドキュメント化を開始するには、[ セルフサービスソースのドキュメント作成の概要 ](./documentation/doc-overview.md) を参照してください
   * この [sources API ドキュメントテンプレート ](./documentation/template.md) を使用して、API ドキュメントを構築できます。
   * この [ ソース UI ドキュメントテンプレート ](./documentation/ui-template.md) を使用して、UI ドキュメントを構築できます。
   * GitHub を使用してドキュメントを作成する手順については、[GitHub web インターフェイスの使用 ](./documentation/github.md) に関するガイドを参照してください。
   * ローカルマシンを使用してドキュメントを作成する手順については、[ テキストエディターの使用 ](./documentation/text-editor.md) に関するガイドを参照してください。
