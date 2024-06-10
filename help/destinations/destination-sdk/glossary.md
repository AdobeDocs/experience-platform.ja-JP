---
solution: Experience Platform
title: Adobe Experience Platform Destination SDK用語集
description: Experience PlatformDestination SDKを使用して宛先をオーサリングする際に使用される、重要な用語を理解します。
source-git-commit: 3dae91fbe46dc3e23c6ac0cfb10c25ac8473876a
workflow-type: tm+mt
source-wordcount: '716'
ht-degree: 3%

---


# Adobe Experience Platform Destination SDK用語集

Destination SDKで使用される用語の定義については、この用語集を参照してください。 その他のAdobe Experience Platform用語については、を参照してください。 [Experience Platform用語](/help/landing/glossary.md).

## A

**集計ポリシー**：リアルタイムストリーミングの宛先にデータを書き出す方法を設定する場合、宛先プラットフォームに送信する前に、プロファイルデータを集計する方法を定義できます。 これは、特定の条件に基づいてデータレコードをグループ化し、API 呼び出しの頻度を減らし、全体的な効率を向上させることによって、データ配信を最適化するのに役立ちます。 様々な宛先要件を満たすように様々なポリシーを設定でき、データが最も効果的な方法でパッケージ化され配信されるようにします。 [詳細情報](/help/destinations/destination-sdk/functionality/destination-configuration/aggregation-policy.md)。

**オーディエンスメタデータの設定**：オーディエンスメタデータ設定とは、Adobe Experience Platform内で定義される構造化された設定とパラメーターを指し、これを使用すれば、指定した宛先でオーディエンスセグメントをプログラムで作成、更新、削除できます。 この設定は、宛先プラットフォームのマーケティング API の仕様に合わせて、オーディエンスメタデータテンプレートを使用します。 詳しくは、 [オーディエンスメタデータの設定](/help/destinations/destination-sdk/functionality/audience-metadata-management.md) および [使用可能なマクロ](/help/destinations/destination-sdk/functionality/audience-metadata-management.md#macros).

## D

**宛先設定エンドポイント**:Adobe Experience Platformの宛先設定エンドポイント（特に） `/authoring/destinations` API エンドポイントは、宛先の設定を作成、取得、更新、削除するために使用されます。 これらの設定は、Adobe Experience Platformから様々な外部システムや宛先（マーケティングプラットフォーム、クラウドストレージサービス、その他のデータ処理エンドポイントなど）にデータを配信する方法を定義します。 詳細を読む： [使用可能な設定オプション](/help/destinations/destination-sdk/functionality/configuration-options.md#destination-configuration) を表示します [リファレンスドキュメント](/help/destinations/destination-sdk/authoring-api/destination-configuration/create-destination-configuration.md).

**宛先インスタンス**:Adobe Experience Platformの宛先設定の具体的な設定。Experience PlatformUI を使用して作成および管理されます。 これには、宛先に接続してデータを送信するために必要なすべてのパラメーターと資格情報が含まれます。 宛先への接続を確立すると、次の場合に宛先インスタンス ID を取得できます [宛先との接続の参照](/help/destinations/ui/destination-details-page.md).

![宛先インスタンス ID の取得方法の UI 画像](/help/destinations/destination-sdk/assets/testing-api/get-destination-instance-id.png)

## P

**[!DNL Pebble]template**:A [!DNL Pebble] テンプレートは、Adobe Experience Platformから書き出されたデータを、書き出し先のプラットフォームで必要な形式に変換するために使用されます。 次を使用します [!DNL Pebble] テンプレート言語（などの関数を使用した動的データ変換が可能） `filter`, `for`, `if`、および `set`. Adobe Experience Platformには、などの追加のカスタム関数が含まれています `addedSegments` および `removedSegments`. これらのテンプレートは、宛先の仕様を満たすために、タイムスタンプやオーディエンスメンバーシップなどのデータ要素の形式を設定するのに役立ちます。 詳細情報 [こちら](/help/destinations/destination-sdk/functionality/destination-server/message-format.md) および [こちら](/help/destinations/destination-sdk/functionality/destination-server/templating-specs.md).

**プライベートの宛先**：個々のAdobe Experience Platform ユーザーが作成したカスタム統合。 これらは、特定のビジネスニーズに合わせてカスタマイズされ、お客様の組織内でのみアクセスでき、データ書き出しの設定を柔軟に行えます。 プライベートの宛先は、Real-Time CDP Ultimate のお客様のみが利用できます。 [詳細情報](/help/destinations/destination-sdk/overview.md#productized-custom-integrations)。

**公開先**:Adobe Experience Platform カタログで公開されている統合。 これらの宛先は標準化され、ブランド化されており、事前設定済みのパラメーターを提供することで顧客セットアップを簡略化します。 Adobe Experience Platformを使用するすべてのユーザーがアクセスできます。 [詳細情報](/help/destinations/destination-sdk/overview.md#productized-custom-integrations)。

## S

**セルフサービスドキュメントテンプレート**：セルフサービスドキュメントテンプレートは、宛先をドキュメント化するために使用できる構造化形式を提供します。 概要、ユースケース、前提条件、サポートされている ID、オーディエンス、書き出しタイプおよび頻度に関する節と、宛先への接続、オーディエンスのアクティブ化、属性のマッピング手順が含まれています。 このテンプレートを使用すると、包括的で一貫性のあるドキュメントが確実に提供され、顧客は宛先の使用をすぐに開始でき、提供されたユースケースを理解できるようになります。 詳細を読む： [宛先のドキュメント化方法](/help/destinations/destination-sdk/docs-framework/documentation-instructions.md), [最新のセルフサービスドキュメントテンプレートのダウンロード](/help/destinations/destination-sdk/assets/docs-framework/yourdestination-template.zip)、および [レンダリング方法を表示](/help/destinations/destination-sdk/docs-framework/self-service-template.md).

## T

**テンプレート仕様とテンプレート戦略**：テンプレート仕様は、Adobe Experience Platformから宛先に送信される HTTP リクエストの書式設定に使用される設定です。 XDM スキーマのプロファイル属性フィールドを、宛先プラットフォームでサポートされる形式に変換します。 類似のテンプレート言語の使用 [!DNL Jinja]これらの仕様を使用すると、特定のルールと入力データに基づいて動的データ変換を行うことができます。 [詳細情報](/help/destinations/destination-sdk/functionality/destination-server/templating-specs.md)

**API のテスト**：テスト API を使用すると、公開リクエストを送信する前に宛先設定を検証できます。 サンプルプロファイルを生成してデータフローをテストし、設定が宛先の要件に一致していることを確認するためのツールを提供します。 この API は、ストリーミング宛先とファイルベース（バッチ）宛先の両方をサポートし、データをシミュレートして、セットアッププロセスで発生する可能性のある問題をトラブルシューティングする方法を提供します。 詳しくは、のテスト API を参照してください [ストリーミング](/help/destinations/destination-sdk/testing-api/streaming-destinations/streaming-destination-testing-overview.md) および [ファイルベースの宛先](/help/destinations/destination-sdk/testing-api/batch-destinations/file-based-destination-testing-overview.md).

**変換テンプレート**：変換テンプレートは、Adobe XDM スキーマから宛先の想定される形式にデータ形式をカスタマイズします。 [詳細情報](/help/destinations/destination-sdk/functionality/destination-server/message-format.md)