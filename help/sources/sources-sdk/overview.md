---
keywords: Experience Platform；ホーム；人気の高いトピック；ソース；コネクタ；ソースコネクタ；ソース sdk;SDK;SDK
solution: Experience Platform
title: ソース SDK の概要（ベータ版）
topic-legacy: overview
description: Adobe Experience Platform Sources SDK は、Flow Service API を使用して REST API ベースのソースを統合し、データをExperience Platformに導くための一連の設定 API です。
hide: true
hidefromtoc: true
exl-id: 5d5449ad-a1ba-402b-a281-0b2d8b704f32
source-git-commit: ce902e461c748e30e0307558da894a4dbdd212a4
workflow-type: tm+mt
source-wordcount: '544'
ht-degree: 8%

---

# ソース SDK の概要（ベータ版）

>[!IMPORTANT]
>
>ソース SDK は現在ベータ版です。お客様の組織はまだアクセスできない可能性があります。 このドキュメントで説明する機能は、変更される場合があります。

Adobe Experience Platform Sources SDK は、REST API ベースのソースを統合するための一連の設定 API で、 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) データをExperience Platform

ソース SDK を使用すると、次のことができます。

* 作成、読み取り、更新、削除の機能を備えた、Platform カタログへの新しいソースの設定には、 [!DNL Flow Service] API
* サポートされる認証タイプに関する情報やリソースデータの取得方法など、ソースの仕様を定義します。
* 新しいソースに関するユーザー向けドキュメントを作成します。

ソース SDK のドキュメントでは、Adobe Experience Platform Sources SDK を使用して、Platform との REST API ベースのソース統合を設定、テスト、リリースし、ソースを増え続けるソースカタログの一部にする手順を説明しています。

![カタログ](./assets/catalog.png)

## ソースについて

 Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

ソースの詳細および Platform で現在サポートされている様々なソースのリストについては、 [ソースの概要](../home.md).

## ソースの作成

ソース SDK を使用すると、独自の REST API ベースのソースを統合し、データをと Platform に取り込むことができます。 [!DNL Flow Service]. ソース SDK を使用すると、 [!DNL Flow Service] API

詳しくは、 [新しい接続仕様の作成](./api/api-overview.md) 新しいソースを Platform に統合する方法について詳しくは、を参照してください。

## ソースをドキュメント化

ソースを作成したら、 [ドキュメントガイド](./documentation/doc-overview.md) を使用してソースをドキュメント化する方法について [!DNL GitHub] Web インターフェイス、または独自のテキストエディターを使用する。

## 高レベルのプロセス

Experience Platformでソースを設定する手順を以下に示します。

* 詳しくは、 [ソース SDK API ガイド](./api/api-overview.md);
   * 詳しくは、 [入門ガイド](./api/getting-started.md);
   * 次のチュートリアルに従います。 [新しい接続仕様の作成](./api/create.md);
   * 次のチュートリアルに従います。 [接続仕様の更新](./api/update-connection-specs.md);
   * 次のチュートリアルに従います。 [フロー仕様への新しい接続仕様 ID の追加](./api/update-flow-specs.md)
   * [新しいソースを送信](./api/submit.md).
* 接続仕様の構造とプロパティをより深く理解するには、 [ソース SDK の設定オプション](./config/config.md);
   * 詳しくは、 [認証仕様の設定](./config/authspec.md);
   * 詳しくは、 [ソース仕様の構成](./config/sourcespec.md);
   * 詳しくは、 [参照仕様の設定](./config/explorespec.md);
* ソースのドキュメント化を開始するには、 [ソース SDK 用ドキュメント作成の概要](./documentation/doc-overview.md)
   * この [ソース API ドキュメントテンプレート](./documentation/template.md) API ドキュメントを構築する
   * この [ソース UI ドキュメントテンプレート](./documentation/ui-template.md) UI ドキュメントを構築するには、以下を実行します。
   * 詳しくは、 [GitHub Web インターフェイスの使用](./documentation/github.md) を参照してください。
   * 詳しくは、 [テキストエディターの使用](./documentation/text-editor.md) ローカルマシンを使用してドキュメントを作成する手順については、を参照してください。
