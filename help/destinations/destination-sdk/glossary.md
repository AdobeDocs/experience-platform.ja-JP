---
solution: Experience Platform
title: Adobe Experience Platform Destination SDK用語
description: Experience Platform Destination SDKを使用して宛先をオーサリングする際の重要な用語について説明します。
exl-id: d65f390a-a980-49b8-9570-840f03534553
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '671'
ht-degree: 3%

---

# [!DNL Adobe Experience Platform] Destination SDK用語集

Destination SDKで使用される用語の定義については、この用語集を参照してください。 その他の[!DNL Adobe Experience Platform]用語については、[Experience Platform用語集](/help/landing/glossary.md)を参照してください。

## A {#a}

**集計ポリシー**: リアルタイム ストリーミング宛先にデータをエクスポートする方法を設定する際に、宛先プラットフォームに送信する前にプロファイル データを集計する方法を定義できます。 これにより、特定の基準にもとづいてデータレコードをグループ化し、API呼び出しの頻度を減らし、全体的な効率を向上させることで、データ配信を最適化することができます。 様々な宛先要件に対応するように様々なポリシーを設定して、データを最も効果的な方法でパッケージ化して配信できます。 [詳細情報](/help/destinations/destination-sdk/functionality/destination-configuration/aggregation-policy.md)。

**オーディエンスメタデータ設定**: オーディエンスメタデータ設定とは、[!DNL Adobe Experience Platform]内で定義された構造化セットアップとパラメーターを指し、指定された宛先でのオーディエンスセグメントのプログラムによる作成、更新、削除を可能にします。 この設定では、オーディエンスのメタデータテンプレートを使用して、宛先プラットフォームのマーケティング APIの仕様に合わせて調整します。 [ オーディエンスメタデータ設定](/help/destinations/destination-sdk/functionality/audience-metadata-management.md)と[使用可能なマクロ ](/help/destinations/destination-sdk/functionality/audience-metadata-management.md#macros)について詳しく説明します。

## D {#d}

**宛先設定エンドポイント**: [!DNL Adobe Experience Platform]の宛先設定エンドポイント、特に`/authoring/destinations` API エンドポイントは、宛先の設定を作成、取得、更新、削除します。 これらの設定は、[!DNL Adobe Experience Platform]からのデータを、マーケティングプラットフォーム、クラウドストレージサービス、その他のデータ処理エンドポイントなど、さまざまな外部システムや宛先に配信する方法を定義します。 [使用可能な設定オプション ](/help/destinations/destination-sdk/functionality/configuration-options.md#destination-configuration)について詳しく説明し、[参照ドキュメント ](/help/destinations/destination-sdk/authoring-api/destination-configuration/create-destination-configuration.md)を参照してください。

**宛先インスタンス**: [!DNL Adobe Experience Platform]の宛先設定の具体的な設定で、Experience Platform UIを使用して作成および管理されます。 宛先にデータを接続して送信するために必要なすべてのパラメーターと資格情報が含まれます。 宛先への接続を確立した後、[宛先との接続を参照すると、宛先インスタンス IDを取得できます](/help/destinations/ui/destination-details-page.md)。

![宛先インスタンス ID の取得方法の UI 画像](/help/destinations/destination-sdk/assets/testing-api/get-destination-instance-id.png)

## P {#p}

**[!DNL Pebble]テンプレート**: [!DNL Pebble] テンプレートは、[!DNL Adobe Experience Platform]から書き出されたデータを、宛先プラットフォームで必要な形式に変換します。 [!DNL Pebble] テンプレート言語を使用しています。この言語を使用すると、`filter`、`for`、`if`、`set`などの関数を通じて動的なデータ変換が可能になります。 [!DNL Adobe Experience Platform]には、`addedSegments`や`removedSegments`などのカスタム関数が追加されています。 これらのテンプレートは、タイムスタンプやオーディエンスメンバーシップなどのデータ要素を、宛先の仕様に合わせてフォーマットするのに役立ちます。 詳細については、[こちら](/help/destinations/destination-sdk/functionality/destination-server/message-format.md)および[こちら](/help/destinations/destination-sdk/functionality/destination-server/templating-specs.md)を参照してください。

**プライベート宛先**：個々の[!DNL Adobe Experience Platform]顧客が作成したカスタム統合。 これらは特定のビジネスニーズに合わせてカスタマイズされ、顧客の組織内でのみアクセスでき、データ書き出し設定の柔軟性を提供します。 プライベートの宛先は、[!DNL Real-Time CDP]人のUltimate ユーザーのみが利用できます。 [詳細情報](/help/destinations/destination-sdk/overview.md#productized-custom-integrations)。

**公開先**: [!DNL Adobe Experience Platform] カタログの一般公開されている統合。 これらの宛先は標準化され、ブランド化され、事前設定済みのパラメーターを提供することで、顧客の設定を簡素化します。 [!DNL Adobe Experience Platform]を使用しているすべてのお客様がアクセスできます。 [詳細情報](/help/destinations/destination-sdk/overview.md#productized-custom-integrations)。

## S {#s}

**セルフサービス ドキュメント テンプレート**: セルフサービス ドキュメント テンプレートは、宛先のドキュメント作成に使用できる構造化された形式を提供します。 概要、ユースケース、前提条件、サポートされているID、オーディエンス、書き出しの種類、頻度に関する節と、宛先への接続、オーディエンスのアクティブ化、属性のマッピングの手順を含みます。 このテンプレートを使用して、包括的で一貫性のあるドキュメントを作成することで、顧客はすばやく宛先の使用を開始し、提供されるユースケースを理解できます。 [宛先を文書化する方法](/help/destinations/destination-sdk/docs-framework/documentation-instructions.md)、[最新のセルフサービス文書テンプレートをダウンロード ](/help/destinations/destination-sdk/assets/docs-framework/yourdestination-template.zip)、[そのレンダリング方法を表示](/help/destinations/destination-sdk/docs-framework/self-service-template.md)の詳細をご確認ください。

## T {#t}

**テンプレート仕様とテンプレート戦略**: テンプレート仕様は、[!DNL Adobe Experience Platform]から宛先に送信されたHTTP リクエストの書式設定に使用される設定です。 XDM スキーマのプロファイル属性フィールドを、宛先プラットフォームでサポートされる形式に変換します。 これらの仕様では、[!DNL Jinja]と同様のテンプレート言語を使用して、特定のルールと入力データに基づく動的なデータ変換が可能です。 [詳細情報](/help/destinations/destination-sdk/functionality/destination-server/templating-specs.md)

**テスト API**: テスト APIを使用すると、公開リクエストを送信する前に宛先設定を検証できます。 サンプルプロファイルを生成し、データフローをテストするツールを提供し、設定が宛先の要件に一致することを確認します。 このAPIは、ストリーミング宛先とファイルベース（バッチ）宛先の両方をサポートしており、データをシミュレートし、セットアッププロセスの潜在的な問題をトラブルシューティングする方法を提供します。 [ ストリーミング ](/help/destinations/destination-sdk/testing-api/streaming-destinations/streaming-destination-testing-overview.md)および[ ファイルベースの宛先](/help/destinations/destination-sdk/testing-api/batch-destinations/file-based-destination-testing-overview.md)のテスト APIについて詳しくは、こちらを参照してください。

**変換テンプレート**：変換テンプレートは、Adobe XDM スキーマから宛先の想定されるフォーマットに合わせてデータフォーマットをカスタマイズします。 [詳細情報](/help/destinations/destination-sdk/functionality/destination-server/message-format.md)
