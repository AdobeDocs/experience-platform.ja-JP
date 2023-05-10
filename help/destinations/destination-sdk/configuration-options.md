---
description: Adobe Experience Platformの宛先サービスは、宛先機能を構築する複数のコンポーネントの設定エンドポイントを使用します。 これらのコンポーネントを組み合わせることで、Experience Platformは宛先パートナーとの接続、カスタムメッセージの送信、デジタルエコシステムをまたいだプロファイルデータのアクティブ化をおこなうことができます。
title: Destination SDKの設定オプション
exl-id: 8890c70a-cdb9-4b9d-aa81-affe72b1fdc5
source-git-commit: 9b4c7da5aa02ae27608c2841b1d825445ac3015e
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 7%

---

# Destination SDKの設定オプション

## 概要 {#overview}

Adobe Experience Platformの宛先サービスは、宛先機能を構築する複数のコンポーネントの設定エンドポイントを使用します。 これらのコンポーネントを組み合わせることで、Experience Platformは宛先プラットフォームへの接続、カスタムメッセージの送信、カスタムファイルの書き出し、デジタルエコシステムをまたいだプロファイルデータのアクティブ化をおこなえます。 次の設定がAdobe Experience Platform Destination SDKに使用されます。

* **宛先の設定**:宛先に関する基本情報が含まれます。 この設定には、宛先がサポートできる ID タイプ、書き出すファイルの目的の形式（ファイルベースの宛先）、Adobe Experience Platformユーザーインターフェイスの宛先カードの様々な UI 属性が含まれます。
* **サーバ、ファイル、およびテンプレートの仕様**:サーバーの仕様と、Adobeがペイロードを宛先に配信する際に使用するテンプレートに関する情報をまとめます。 ファイルベースの宛先の場合、この設定には、宛先でサポートされているファイル形式と圧縮形式も含まれます。
   * **サーバーの仕様**:データの送信先のストレージの場所または HTTP エンドポイントに関する情報を含む設定テンプレート。
   * **ファイル仕様**:バッチ保存先のファイルフォーマットと圧縮のオプションを含む設定テンプレート。
   * **テンプレート仕様**:このテンプレートでは、XDM スキーマと、プラットフォームがサポートする形式の間でプロファイル属性フィールドを変換する方法を定義できます。 サポートされるAdobe言語、メッセージ形式およびプラットフォームとの統合を設定するために必要な情報について詳しくは、 [メッセージの形式](./message-format.md).
* **認証設定**:これらの設定は、Adobe Experience Platformユーザーが宛先に接続する方法を定義します。
* **オーディエンスメタデータの設定**:この設定エンドポイントを使用すると、宛先でオーディエンスやセグメントをプログラムによって作成、更新、削除する方法を設定できます。 バッチ保存先の場合は、ファイルが宛先に正常に配信された場合にいつでも通知を設定できます。

![Destination SDK設定エンドポイントと、それらの併用方法を示す図。](./assets/self-service-configuration.png)

## 関連リンク {#related-links}

以下のページでは、Destination SDKで使用できる機能と設定オプション、および実行できる対応する API 操作について詳しく説明します。

| 機能の説明（ストリーミングの宛先） | 機能の説明（バッチ保存先） | API リファレンス |
|--- |--- |--- |
| [宛先設定オプション](./destination-configuration.md) | [ファイルベースの宛先設定オプション](/help/destinations/destination-sdk/file-based-destination-configuration.md) | [宛先 API エンドポイントの操作](./destination-configuration-api.md) |
| [サーバーとテンプレートの仕様](./server-and-template-configuration.md) | [サーバおよびファイル構成の仕様](/help/destinations/destination-sdk/server-and-file-configuration.md) | [宛先サーバー API エンドポイントの操作](./destination-server-api.md) |
| [認証設定](./authentication-configuration.md) | ストリーミングの宛先の場合と同じです。 | 宛先の認証情報は、`/destinations` エンドポイントの `customerAuthenticationConfigurations` パラメーターを介して設定できます。詳細情報： [ストリーミング](/help/destinations/destination-sdk/destination-configuration.md#customer-authentication-configurations) および [バッチ](/help/destinations/destination-sdk/file-based-destination-configuration.md#customer-authentication-configurations) 宛先。 |
| [オーディエンスメタデータの管理](./audience-metadata-management.md) | ストリーミングと同じです。 詳しくは、 [ファイルベースの例](/help/destinations/destination-sdk/audience-metadata-management.md#example-file-based) を参照してください。 | [オーディエンスメタデータエンドポイント API の操作](./audience-metadata-api.md) |
| [OAuth 2 設定](./oauth2-authentication.md) | ストリーミングの宛先と同じ | を使用した設定 `customerAuthenticationConfigurations` パラメーター [/destinations API エンドポイント](./destination-configuration-api.md). |
| [メッセージの形式](./message-format.md) | - | - |
| [ストリーミング宛先のテストツール](./test-destination.md) | [ファイルベースの宛先のテストツール](/help/destinations/destination-sdk/file-based-destination-testing-overview.md) | [宛先テスト API の操作](./destination-testing-api.md) |
| [宛先の公開](./configure-destination-instructions.md#publish-destination) | ストリーミングの宛先と同じ | [宛先公開 API の操作](./destination-publish-api.md) |

{style="table-layout:auto"}

## 次の手順 {#next-steps}

この記事では、Destination SDKが提供する機能の一般的な概要と、特定の設定に関する詳細情報を読むページを示します。 次に、 [ストリーミングの設定](/help/destinations/destination-sdk/configure-destination-instructions.md) または [ファイルベースの宛先](/help/destinations/destination-sdk/configure-file-based-destination-instructions.md) Destination SDK
