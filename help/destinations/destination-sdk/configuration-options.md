---
description: Adobe Experience Platformの宛先サービスは、宛先機能を構築する複数のコンポーネントの設定テンプレートを使用します。 これらのコンポーネントを組み合わせることで、Experience Platformは宛先パートナーとの接続、カスタムメッセージの送信、デジタルエコシステムをまたいだプロファイルデータのアクティブ化をおこなうことができます。
title: Destination SDKの設定オプション
exl-id: 8890c70a-cdb9-4b9d-aa81-affe72b1fdc5
source-git-commit: ad0d38cbd249642d582a807c5679065827f57717
workflow-type: tm+mt
source-wordcount: '345'
ht-degree: 9%

---

# Destination SDKの設定オプション

## 概要 {#overview}

Adobe Experience Platformの宛先サービスは、宛先機能を構築する複数のコンポーネントの設定テンプレートを使用します。 これらのコンポーネントを組み合わせることで、Experience Platformは宛先パートナーとの接続、カスタムメッセージの送信、デジタルエコシステムをまたいだプロファイルデータのアクティブ化をおこなうことができます。 Adobe Experience Platformで使用されるテンプレートは次のとおりです。

* **宛先の設定**:宛先に関する基本情報が含まれます。 この設定には、宛先がサポートできる ID タイプや、Adobe Experience Platformユーザーインターフェイスの宛先カードに対する様々な UI 属性が含まれます。
* **サーバーとテンプレートの仕様**:サーバーの仕様と、Adobeがペイロードを宛先に配信する際に使用するテンプレートに関する情報をまとめます。
   * **サーバーの仕様**:エンドポイントの詳細を保存するテンプレート。
   * **テンプレート仕様**:このテンプレートでは、XDM スキーマと、プラットフォームがサポートする形式の間でプロファイル属性フィールドを変換する方法を定義できます。 サポートされるAdobe言語、メッセージ形式およびプラットフォームとの統合を設定するために必要な情報について詳しくは、 [メッセージの形式](./message-format.md).
* **認証設定**:これらの設定は、Adobe Experience Platformユーザーが宛先に接続する方法を定義します。
* **オーディエンスメタデータの設定**:このテンプレートを使用すると、宛先でのオーディエンス/セグメントのプログラムによる作成、更新、削除の方法を設定できます。

![Destination SDKテンプレートと設定](./assets/self-service-configuration.png)

## 関連リンク {#related-links}

以下のページでは、Destination SDKで使用できる機能と設定オプション、および実行できる対応する API 操作について詳しく説明します。

| 機能の説明 | API リファレンス |
|--- |--- |
| [宛先の設定](./destination-configuration.md) | [宛先 API エンドポイントの操作](./destination-configuration-api.md) |
| [サーバーとテンプレートの仕様](./server-and-template-configuration.md) | [宛先サーバー API エンドポイントの操作](./destination-server-api.md) |
| [認証設定](./authentication-configuration.md) | [資格情報エンドポイント API の操作](./credentials-configuration-api.md) |
| [オーディエンスメタデータの管理](./audience-metadata-management.md) | [オーディエンスメタデータエンドポイント API の操作](./audience-metadata-api.md) |
| [OAuth 2 設定](./oauth2-authentication.md) | を使用した設定 `customerAuthenticationConfigurations` パラメーター [/destinations API エンドポイント](./destination-configuration-api.md). |
| [メッセージの形式](./message-format.md) | - |
| [宛先のテスト](./test-destination.md) | [宛先テスト API の操作](./destination-testing-api.md) |
| [宛先の公開](./configure-destination-instructions.md#publish-destination) | [宛先公開 API の操作](./destination-publish-api.md) |

{style=&quot;table-layout:auto&quot;}
