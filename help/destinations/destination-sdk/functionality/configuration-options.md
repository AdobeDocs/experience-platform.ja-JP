---
description: Adobe Experience Platformの宛先サービスは、宛先機能を構築する複数のコンポーネントの設定エンドポイントを使用します。 これらのコンポーネントを組み合わせることで、Experience Platformが宛先パートナーとの接続、カスタムメッセージの送信、デジタルエコシステムをまたいだプロファイルデータのアクティブ化をおこなう方法を説明します。
title: Destination SDKの設定オプション
source-git-commit: 65a658208b48a50184e55a6d64cdf7ad6de0f04f
workflow-type: tm+mt
source-wordcount: '829'
ht-degree: 0%

---


# Destination SDKの設定オプション

Adobe Experience Platformの宛先サービスは、宛先機能を構築する複数のコンポーネントの設定エンドポイントを使用します。

これらのコンポーネントを組み合わせることで、Experience Platformは宛先プラットフォームへの接続、カスタムメッセージの送信、カスタムファイルの書き出し、デジタルエコシステムをまたいだプロファイルデータのアクティブ化をおこなえます。

次の図は、独自の宛先を構築するためにDestination SDKを通じて設定できるコンポーネントの概要を示しています。 これらのコンポーネントについては、後述します。

![Destination SDKコンポーネント、設定エンドポイント、およびそれらによってサポートされる操作を示す図です。](../assets/functionality/destination-sdk-components-diagram.png)

## サーバーの設定 {#server-configuration}

宛先サーバーの設定は、サーバー仕様と、Adobeがペイロードを宛先に配信する際に使用するテンプレートに関する情報を結び付けます。

例えば、Experience Platformが接続する必要がある側の API エンドポイントと、Platform がおこなう API 呼び出しのヘッダーおよび形式を指定する場所です。

ファイルベースの宛先の場合、この設定には、宛先でサポートされているファイル形式と圧縮形式も含まれます。 以下で説明する機能は、 [destination-servers エンドポイント](../authoring-api/destination-server/create-destination-server.md).

* [サーバーの仕様](destination-server/server-specs.md):データの送信先のストレージの場所または HTTP エンドポイントに関する情報を含む設定テンプレート。
* [テンプレート仕様](destination-server/templating-specs.md):このテンプレートでは、XDM スキーマ間のプロファイル属性フィールドの変換方法や、プラットフォームがサポートする形式など、エンドポイントに対する HTTP API リクエストの構造を定義できます。 この情報を [メッセージ形式](destination-server/message-format.md) ドキュメント。
* [メッセージの形式](destination-server/message-format.md):この節では、サポートされるAdobe言語、メッセージ形式、およびプラットフォームとの統合を設定するために Template で必要な情報について詳しく説明します。 この情報を [テンプレート仕様](destination-server/templating-specs.md) ドキュメント。
* [ファイル仕様](destination-server/file-formatting.md):バッチ保存先のファイルフォーマットと圧縮のオプションを含む設定テンプレート。

## 宛先の設定 {#destination-configuration}

この設定エンドポイントには、宛先に関する基本情報と詳細情報が含まれています。 例えば、ここで、宛先がサポートできる ID タイプ、書き出すファイルの目的の形式（ファイルベースの宛先）、Adobe Experience Platformユーザーインターフェイスの宛先カードの様々な UI 属性を指定します。

各宛先設定コンポーネントの詳細については、以下のドキュメントを参照してください。 以下で説明する機能は、 [宛先エンドポイント](../authoring-api/destination-configuration/create-destination-configuration.md).

* [顧客認証設定](destination-configuration/customer-authentication.md):宛先への接続にExperience Platformが使用する認証メカニズムを選択します。 この設定により、 [新しい宛先の設定](../../ui/connect-destination.md) ページを開きます。このページでは、Experience Platformが、宛先のアカウントにExperience Platformを接続できます。
* [OAuth2 認証](destination-configuration/oauth2-authentication.md):すべての [!DNL OAuth2] 認証フローがDestination SDKでサポートされ、設定手順を取得します。 [!DNL OAuth2] 宛先の認証…
* [顧客データフィールド](destination-configuration/customer-data-fields.md):Experience PlatformUI で入力フィールドを作成し、宛先へのデータの接続および書き出し方法に関連する様々な情報をユーザーが指定できるようにする方法を説明します。
* [UI 属性](destination-configuration/ui-attributes.md):Destination SDKで作成した宛先に対して、ドキュメントリンク、宛先カードカテゴリ、宛先接続のタイプと頻度などの UI 属性を設定する方法について説明します。
* [スキーマ設定](destination-configuration/schema-configuration.md):ユーザーがプロファイルの属性と ID をマッピングできる宛先のターゲットスキーマを定義する方法について説明します。
* [ID 名前空間の設定](destination-configuration/identity-namespace-configuration.md):宛先でサポートされる ID を設定する方法について説明します。 この設定により、 [マッピング手順](../../ui/activate-segment-streaming-destinations.md#mapping) を使用します。
* [宛先の配信](destination-configuration/destination-delivery.md):書き出されたデータの送信先と、データの送信先で使用される認証ルールを設定する方法について説明します。
* [オーディエンスメタデータの設定](destination-configuration/audience-metadata-configuration.md):セグメント名や ID などのセグメントメタデータを、セグメントと宛先の間で共有するExperience Platformについて説明します。
* [集計ポリシー](destination-configuration/aggregation-policy.md):宛先への HTTP リクエストをグループ化およびバッチ化する方法を決定する、集計ポリシーを設定する方法について説明します。
* [バッチ設定](destination-configuration/batch-configuration.md):ユーザーが宛先に接続する際に、ユーザーユーザーインターフェイスでユーザーが使用できる様々なファイルの命名および書き出しスケジュール設定をExperience Platformします。
* [過去のプロファイルの選定](destination-configuration/historical-profile-qualifications.md):Destination SDKで作成された宛先でサポートされる履歴プロファイルの絞り込みについて説明します。

## オーディエンスメタデータの設定 {#audience-metadata-configuration}

このコンポーネントを使用すると、宛先でのオーディエンス/セグメントのプログラムによる作成、更新、削除の方法を設定できます。 ファイルベースの宛先の場合は、ファイルが宛先に正常に配信された場合にいつでも通知を設定できます。 この機能は、 [audience-templates エンドポイント](../metadata-api/create-audience-template.md).

## 次の手順 {#next-steps}

この記事では、Destination SDKが提供する機能の一般的な概要と、特定の設定に関する詳細情報を読むページを示します。 次に、 [ストリーミングの設定](../guides/configure-destination-instructions.md) または [ファイルベースの宛先](../guides/configure-file-based-destination-instructions.md) Destination SDK
