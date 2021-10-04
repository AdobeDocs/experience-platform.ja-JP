---
description: Adobe Experience Platformの宛先サービスは、宛先機能を構築する複数のコンポーネントの設定テンプレートを使用します。 これらのコンポーネントを組み合わせることで、Experience Platformは宛先パートナーとの接続、カスタムメッセージの送信、デジタルエコシステム全体でのプロファイルデータのアクティブ化をおこなえます。
seo-description: The destinations service in Adobe Experience Platform uses configuration templates for several components that build up the destinations functionality. Combined, these components allow Experience Platform to connect to destination partners, send custom messages, and activate profile data across the digital ecosystem.
seo-title: Configuration options in Destination SDK
title: 宛先 SDK の設定オプション
exl-id: 8890c70a-cdb9-4b9d-aa81-affe72b1fdc5
source-git-commit: 9be8636b02a15c8f16499172289413bc8fb5b6f0
workflow-type: tm+mt
source-wordcount: '345'
ht-degree: 2%

---

# 宛先 SDK の設定オプション

## 概要 {#overview}

Adobe Experience Platformの宛先サービスは、宛先機能を構築する複数のコンポーネントの設定テンプレートを使用します。 これらのコンポーネントを組み合わせることで、Experience Platformは宛先パートナーとの接続、カスタムメッセージの送信、デジタルエコシステム全体でのプロファイルデータのアクティブ化をおこなえます。 Adobe Experience Platformで使用されるテンプレートは次のとおりです。

* **宛先の設定**:宛先に関する基本情報が含まれます。この設定には、宛先でサポートされる ID タイプと、Adobe Experience Platformユーザーインターフェイスの宛先カードの様々な UI 属性が含まれます。
* **サーバーとテンプレートの仕様**:サーバー仕様と、Adobeがペイロードを宛先に配信する際に使用するテンプレートに関する情報を結び付けます。
   * **サーバーの仕様**:エンドポイントの詳細を格納するテンプレート。
   * **テンプレート仕様**:このテンプレートでは、XDM スキーマと、プラットフォームがサポートする形式の間でプロファイル属性フィールドを変換する方法を定義できます。サポートされるテンプレート言語、メッセージ形式、およびAdobeがプラットフォームとの統合を設定するために必要な情報について詳しくは、[ メッセージ形式 ](./message-format.md) を参照してください。
* **認証設定**:これらの設定は、Adobe Experience Platformユーザーが宛先に接続する方法を定義します。
* **オーディエンスのメタデータの設定**:このテンプレートを使用すると、宛先でオーディエンスやセグメントをプログラムによって作成、更新、削除する方法を設定できます。

![宛先 SDK のテンプレートと設定](./assets/self-service-configuration.png)

## 関連リンク {#related-links}

以下のページでは、宛先 SDK で使用できる機能と設定オプション、および対応する実行可能な API 操作について詳しく説明します。

| 機能の説明 | API リファレンス |
|--- |--- |
| [宛先の設定](./destination-configuration.md) | [宛先 API エンドポイントの操作](./destination-configuration-api.md) |
| [サーバーとテンプレートの仕様](./server-and-template-configuration.md) | [宛先サーバー API エンドポイントの操作](./destination-server-api.md) |
| [認証の設定](./credentials-configuration.md) | [Credentials endpoint API 操作](./credentials-configuration-api.md) |
| [Audience metadata management](./audience-metadata-management.md) | [オーディエンスメタデータエンドポイント API 操作](./audience-metadata-api.md) |
| [OAuth 2 設定](./oauth2-authentication.md) | [/宛先 API エンドポイント ](./destination-configuration-api.md) の `customerAuthenticationConfigurations` パラメーターを使用してを設定します。 |
| [メッセージのフォーマット](./message-format.md) | - |
| [宛先のテスト](./test-destination.md) | [宛先テスト API 操作](./destination-testing-api.md) |
| [宛先の公開](./configure-destination-instructions.md#publish-destination) | [宛先公開 API の操作](./destination-publish-api.md) |

{style=&quot;table-layout:auto&quot;}
