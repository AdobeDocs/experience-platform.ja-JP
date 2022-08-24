---
keywords: Experience Platform;ホーム;人気の高いトピック;ソース;コネクタ;ソースコネクタ;ソース sdk;SDK;SDK
solution: Experience Platform
title: セルフサービスソース（バッチ SDK）の概要
topic-legacy: overview
description: Adobe Experience Platform Self-Serve Sources(Batch SDK) は、Flow Service API を使用して REST API ベースのソースを統合し、データをExperience Platformに送信できる設定 API のセットです。
exl-id: 5d5449ad-a1ba-402b-a281-0b2d8b704f32
source-git-commit: 4d7799b01c34f4b9e4a33c130583eadcfdc3af69
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 7%

---

# セルフサービスソース（バッチ SDK）の概要

Adobe Experience Platform Self-Serve Sources(Batch SDK) は、 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/). セルフサービスソース（バッチ SDK）は、独自のソースを作成し、バッチデータをExperience Platformに送信するための一連の設定 API を提供します。

セルフサービスソース（バッチ SDK）を使用すると、次のことができます。

* を使用した新しいソースのExperience Platformカタログへの統合 [!DNL Flow Service] API
* サポートされる認証タイプに関する情報やリソースデータの取得方法など、ソースの仕様を定義します。
* 新しいソースに関するユーザー向けドキュメントを作成します。

セルフサービスソースのドキュメントでは、Experience Platformとの REST API ベースのソース統合を設定、テスト、リリースする手順を説明し、ソースを増え続けるソースカタログに含めるようにします。

![カタログ](./assets/catalog.png)

## ソースについて

Experience Platformは、外部ソースからデータを取り込みながら、Experience Platformサービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。 アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

ソースの詳細およびExperience Platformで現在サポートされている様々なソースの一覧については、 [ソースの概要](../home.md).

## ソースの作成

セルフサービスソースを通じて、独自の REST API ベースのソースを統合し、データをとExperience Platformに取り込むことができます。 [!DNL Flow Service]. 新しい接続仕様を作成、設定、送信することで、ソースをExperience Platformソースカタログに統合できます。そのためには、 [!DNL Flow Service] API

詳しくは、 [新しい接続仕様の作成](./api/api-overview.md) を参照してください。

## ソースをドキュメント化

ソースを作成したら、 [ドキュメントガイド](./documentation/doc-overview.md) を使用してソースをドキュメント化する方法について [!DNL GitHub] Web インターフェイス、または独自のテキストエディターを使用する。

## プロセスの概要

Experience Platformでソースを設定する手順を以下に示します。

* 詳しくは、 [セルフサービスソース（バッチ SDK）API ガイド](./api/api-overview.md).
   * 詳しくは、 [入門ガイド](./api/getting-started.md).
   * 次のチュートリアルに従います。 [新しい接続仕様の作成](./api/create.md).
   * 次のチュートリアルに従います。 [接続仕様の更新](./api/update-connection-specs.md).
   * 次のチュートリアルに従います。 [フロー仕様への新しい接続仕様 ID の追加](./api/update-flow-specs.md)
   * [新しいソースを送信](./api/submit.md).
* 接続仕様の構造と特性をより深く理解するには、 [セルフサービスソース（バッチ SDK）の設定オプション](./config/config.md).
   * 次のガイドを読む： [認証仕様の設定](./config/authspec.md) を使用すると、ソースに使用できる様々な認証タイプをより深く理解できます。
   * 次のガイドを読む： [ソース仕様の構成](./config/sourcespec.md) 様々なページネーションタイプ、スケジュール形式およびソースに設定できるカスタムスキーマについて詳しくは、を参照してください。
   * 次のガイドを読む： [参照仕様の設定](./config/explorespec.md) を参照して、ソースに含まれるオブジェクトの調査や検査に必要なパラメータを定義する方法を確認してください。
* ソースのドキュメント化を開始するには、 [セルフサービスソースのドキュメント作成の概要](./documentation/doc-overview.md)
   * この [ソース API ドキュメントテンプレート](./documentation/template.md) を参照してください。
   * この [ソース UI ドキュメントテンプレート](./documentation/ui-template.md) を参照してください。
   * 詳しくは、 [GitHub Web インターフェイスの使用](./documentation/github.md) を参照してください。
   * 詳しくは、 [テキストエディターの使用](./documentation/text-editor.md) ローカルマシンを使用してドキュメントを作成する手順については、を参照してください。
