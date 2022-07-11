---
description: ファイルベースの宛先テスト API は、Destination SDKを通じて構築されたファイルベースの宛先の設定を検証するために使用できるエンドポイントの集まりです。
title: ファイルベースの宛先テスト API
source-git-commit: d2d362f4b61e04fc2fa4d9cd9db70ed94a850642
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 0%

---


# ファイルベースの宛先テスト API

## 概要 {#overview}

ファイルベースの宛先テスト API は、Destination SDKを通じて構築されたファイルベースの宛先の設定を検証するために使用できる一連のエンドポイントです。

これらのツールを使用して、以前に設定を検証することをお勧めします。 [送信](submit-destination.md) レビュー先のAdobe

最も良いテスト結果を得るには、次のフロー図に基づいてこの API を使用することをお勧めします。

![推奨される宛先テストフローを示す図](assets/file-based-testing-flow.png)

各エンドポイントで実行できる操作の概要については、以下の節を参照してください。

## サンプル生成エンドポイント {#sample-generation-endpoint}

このエンドポイントは、既存のソーススキーマに基づいてサンプルプロファイルを生成するのに役立ちます。

サンプルプロファイルは、プロファイルの JSON 構造を理解するのに役立ちます。 さらに、独自のプロファイルデータでカスタマイズして、さらに宛先のテストをおこなうためのバックボーンも提供されます。

詳しくは、 [専用ドキュメント](file-based-sample-profile-generation-api.md) を参照してください。

## 宛先設定テストエンドポイント {#destination-configuration-testing-endpoint}

このエンドポイントを使用すると、ファイルベースの宛先が正しく設定されているかどうかをテストし、設定された宛先へのデータフローの整合性を検証できます。

テストエンドポイントには、を追加するかどうかに関わらず、リクエストをおこなうことができます。 [サンプルプロファイル](file-based-sample-profile-generation-api.md) を呼び出しに追加します。 リクエストでプロファイルを送信しない場合、API はサンプルプロファイルを自動的に生成し、リクエストに追加します。

詳しくは、 [専用ドキュメント](file-based-destination-testing-api.md) を参照してください。

## アクティブ化結果のエンドポイント {#activation-results}

このエンドポイントを使用すると、ファイルベースの宛先テスト結果の完全な詳細を表示できます。

この API エンドポイントは、 [フローサービス API](../api/update-destination-dataflows.md) データフローを監視する。

詳しくは、 [専用ドキュメント](file-based-destination-results-api.md) を参照してください。

## 顧客フィールドレンダリングエンドポイント {#customer-fields-rendering-endpoint}

このエンドポイントを使用すると、テンプレート化されている方法を視覚化できます [顧客データフィールド](file-based-destination-configuration.md#customer-data-fields) の宛先設定で定義されたは次のようになります。

エンドポイントは、顧客データフィールドのランダムな値を生成し、応答で返します。 これにより、バケット名やフォルダーパスなど、顧客データフィールドのセマンティック構造を検証できます。

詳しくは、 [専用ドキュメント](file-based-render-template-api.md) を参照してください。