---
description: Adobe Experience Platform の宛先サービスは、宛先機能を構築するいくつかのコンポーネント用に設定エンドポイントを使用します。これらのコンポーネントをどのように組み合わせれば、Experience Platform が、宛先パートナーに接続したり、カスタムメッセージを送信したり、デジタルエコシステム全体にわたってプロファイルデータをアクティブ化したりできるかを説明します。
title: Destination SDK の設定オプション
source-git-commit: 3f31a54c0cf329d374808dacce3fac597a72aa11
workflow-type: ht
source-wordcount: '828'
ht-degree: 100%

---


# Destination SDK の設定オプション

Adobe Experience Platform の宛先サービスは、宛先機能を構築するいくつかのコンポーネント用に設定エンドポイントを使用します。

これらのコンポーネントを組み合わせることで、Experience Platform は、宛先プラットフォームに接続したり、カスタムメッセージを送信したり、カスタムファイルを書き出したり、デジタルエコシステム全体にわたってプロファイルデータをアクティブ化したりできます。

以下の図に、独自の宛先を作成するために Destination SDK を通じて設定できるコンポーネントの概要を示します。これらのコンポーネントについては、後述します。

![Destination SDK コンポーネント、設定エンドポイントおよびそれらでサポートされている操作を示す図。](../assets/functionality/destination-sdk-components-diagram.png)

## サーバー設定 {#server-configuration}

宛先サーバー設定は、サーバー仕様に関する情報と宛先にペイロードを配信するためにアドビが使用するテンプレートを結びつけます。

例えば、ここでは、Experience Platform が接続する必要のあるお客様側の API エンドポイントや、Platform が行う API 呼び出しのヘッダーおよび形式を指定します。

ファイルベースの宛先の場合、この設定には、宛先でサポートされるファイル形式および圧縮形式も含まれます。[宛先サーバーエンドポイント](../authoring-api/destination-server/create-destination-server.md)を介して、以下に記載されている機能を設定できます。

* [サーバー仕様](destination-server/server-specs.md)：データが送信されるストレージの場所や HTTP エンドポイントに関する情報を含む、設定テンプレート。
* [テンプレート仕様](destination-server/templating-specs.md)：このテンプレートでは、エンドポイントに対する HTTP API リクエストの構造化方法（XDM スキーマとプラットフォームがサポートする形式との間のプロファイル属性フィールドの変換方法など）を定義できます。この情報を[メッセージ形式](destination-server/message-format.md)ドキュメントと共に使用します。
* [メッセージ形式](destination-server/message-format.md)：この節では、サポートされるテンプレート言語、メッセージ形式、プラットフォームとの統合を設定するためにアドビが必要とする情報に関する詳細情報を扱います。この情報を[テンプレート仕様](destination-server/templating-specs.md)ドキュメントと共に使用します。
* [ファイル仕様](destination-server/file-formatting.md)：バッチ宛先用のファイル形式および圧縮オプションを含む、設定テンプレート。

## 宛先設定 {#destination-configuration}

この設定エンドポイントには、宛先に関する基本および詳細情報が含まれます。例えば、ここでは、宛先がサポートできる ID タイプ、書き出されたファイル（ファイルベースの宛先用）の目的の形式、Adobe Experience Platform ユーザーインターフェイスの宛先カード用の様々な UI 属性を指定します。

各宛先設定コンポーネントについて詳しくは、以下のドキュメントを参照してください。[宛先エンドポイント](../authoring-api/destination-configuration/create-destination-configuration.md)を介して、以下に記載されている機能を設定できます。

* [顧客認証設定](destination-configuration/customer-authentication.md)：宛先に接続するために Experience Platform が使用する必要がある認証メカニズムを選択します。この設定は、Experience Platform ユーザーインターフェイスの[新しい宛先を設定](../../ui/connect-destination.md)ページを生成します。このページでは、ユーザーが持っている宛先のアカウントに Experience Platform を接続します。
* [OAuth2 認証](destination-configuration/oauth2-authentication.md)：Destination SDK でサポートされているすべての [!DNL OAuth2] 認証フローについて説明し、宛先用に [!DNL OAuth2] 認証を設定する手順を示します。
* [顧客データフィールド](destination-configuration/customer-data-fields.md)：Experience Platform UI で入力フィールドを作成する方法を説明します。これにより、ユーザーは、宛先への接続およびデータの書き出し方法に関連する様々な情報を指定できます。
* [UI 属性](destination-configuration/ui-attributes.md)：Destination SDK で作成された宛先に対する UI 属性（ドキュメントリンク、宛先カードカテゴリ、宛先接続タイプおよび頻度など）の設定方法を説明します。
* [スキーマ設定](destination-configuration/schema-configuration.md)：ユーザーがプロファイル属性と ID をマッピングする、宛先のターゲットスキーマの定義方法を説明します。
* [ID 名前空間設定](destination-configuration/identity-namespace-configuration.md)：宛先でサポートされる ID の設定方法を説明します。 この設定により、Experience Platform ユーザーインターフェイスの[マッピング手順](../../ui/activate-segment-streaming-destinations.md#mapping)でターゲット ID が設定され、ユーザーが XDM スキーマから宛先のスキーマに ID および属性をマッピングします。
* [宛先配信](destination-configuration/destination-delivery.md)：書き出されたデータの正確な移動先や、データが到達する場所で使用される認証ルールの設定方法を説明します。
* [オーディエンスメタデータ設定](destination-configuration/audience-metadata-configuration.md)：オーディエンスメタデータ（オーディエンス名や ID）を Experience Platform と宛先の間でどのように共有すべきかを説明します。
* [集計ポリシー](destination-configuration/aggregation-policy.md)：集計ポリシーを設定して、宛先に対する HTTP リクエストがどのようにグループ化およびバッチ化されるかを説明します。
* [バッチ設定](destination-configuration/batch-configuration.md)：Experience Platform ユーザーインターフェイスで宛先に接続する際にユーザーが使用できる様々なファイル名および書き出しスケジュールを設定します。
* [プロファイル選定履歴](destination-configuration/historical-profile-qualifications.md)：Destination SDK で作成された宛先でサポートされるプロファイル選定履歴について説明します。

## オーディエンスメタデータ設定 {#audience-metadata-configuration}

このコンポーネントを使用すると、宛先でオーディエンスがプログラムでどのように作成、更新または削除されるかを設定できます。ファイルベースの宛先の場合、ファイルが正常に宛先に配信されるといつでも通知されるように設定できます。[オーディエンステンプレートエンドポイント](../metadata-api/create-audience-template.md)を介して、この機能を設定できます。

## 次の手順 {#next-steps}

この記事を読むことで、Destination SDK が提供する機能の一般的な概要と、特定の設定に関する詳細情報を得るための参照ページについて知ることができました。次に、Destination SDK を使用して[ストリーミング](../guides/configure-destination-instructions.md)や[ファイルベースの宛先を設定](../guides/configure-file-based-destination-instructions.md)するためのすべての手順が含まれるガイドを参照してください。
