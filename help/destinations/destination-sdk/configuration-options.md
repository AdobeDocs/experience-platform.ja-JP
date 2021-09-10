---
description: Adobe Experience Platformの宛先サービスは、宛先機能を構築する複数のコンポーネントの設定テンプレートを使用します。 これらのコンポーネントを組み合わせることで、Experience Platformは宛先パートナーとの接続、カスタムメッセージの送信、デジタルエコシステム全体でのプロファイルデータのアクティブ化をおこなえます。
seo-description: The destinations service in Adobe Experience Platform uses configuration templates for several components that build up the destinations functionality. Combined, these components allow Experience Platform to connect to destination partners, send custom messages, and activate profile data across the digital ecosystem.
seo-title: Configuration options in Destination SDK
title: 宛先SDKの設定オプション
exl-id: 8890c70a-cdb9-4b9d-aa81-affe72b1fdc5
source-git-commit: 9be8636b02a15c8f16499172289413bc8fb5b6f0
workflow-type: tm+mt
source-wordcount: '345'
ht-degree: 2%

---

# 宛先SDKの設定オプション

## 概要 {#overview}

Adobe Experience Platformの宛先サービスは、宛先機能を構築する複数のコンポーネントの設定テンプレートを使用します。 これらのコンポーネントを組み合わせることで、Experience Platformは宛先パートナーとの接続、カスタムメッセージの送信、デジタルエコシステム全体でのプロファイルデータのアクティブ化をおこなえます。 Adobe Experience Platformで使用されるテンプレートは次のとおりです。

* **宛先の設定**:宛先に関する基本情報が含まれます。この設定には、宛先でサポートできるIDタイプ、Adobe Experience Platformユーザーインターフェイスの宛先カードの様々なUI属性が含まれます。
* **サーバーとテンプレートの仕様**:ペイロードを宛先に配信するためにAdobeが使用するサーバー仕様とテンプレートに関する情報をまとめます。
   * **サーバーの仕様**:エンドポイントの詳細を保存するテンプレート。
   * **テンプレート仕様**:このテンプレートでは、XDMスキーマと、プラットフォームがサポートする形式の間でプロファイル属性フィールドを変換する方法を定義できます。サポートされるAdobe言語、メッセージ形式、およびプラットフォームとの統合を設定するためにで必要な情報について詳しくは、[メッセージ形式](./message-format.md)を参照してください。
* **認証設定**:これらの設定は、Adobe Experience Platformユーザーが宛先に接続する方法を定義します。
* **オーディエンスのメタデータの設定**:このテンプレートを使用すると、宛先でオーディエンスやセグメントをプログラムによって作成、更新、削除する方法を設定できます。

![宛先SDKのテンプレートと設定](./assets/self-service-configuration.png)

## 関連リンク {#related-links}

以下のページでは、宛先SDKで使用できる機能と設定オプション、および実行可能な対応するAPI操作について詳しく説明します。

| 機能の説明 | API リファレンス |
|--- |--- |
| [宛先の設定](./destination-configuration.md) | [宛先APIエンドポイントの操作](./destination-configuration-api.md) |
| [サーバーとテンプレートの仕様](./server-and-template-configuration.md) | [宛先サーバーAPIエンドポイントの操作](./destination-server-api.md) |
| [認証の設定](./credentials-configuration.md) | [Credentials endpoint API操作](./credentials-configuration-api.md) |
| [Audience Metadata Management](./audience-metadata-management.md) | [オーディエンスメタデータエンドポイントAPIの操作](./audience-metadata-api.md) |
| [OAuth 2設定](./oauth2-authentication.md) | [/宛先APIエンドポイント](./destination-configuration-api.md)の`customerAuthenticationConfigurations`パラメーターを使用してを設定します。 |
| [メッセージのフォーマット](./message-format.md) | - |
| [宛先のテスト](./test-destination.md) | [宛先テストAPI操作](./destination-testing-api.md) |
| [宛先の公開](./configure-destination-instructions.md#publish-destination) | [宛先公開APIの操作](./destination-publish-api.md) |

{style=&quot;table-layout:auto&quot;}
