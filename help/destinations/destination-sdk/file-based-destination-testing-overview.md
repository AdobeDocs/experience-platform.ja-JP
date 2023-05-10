---
description: ファイルベースの宛先テスト API は、Destination SDKを通じて構築されたファイルベースの宛先の設定を検証するために使用できるエンドポイントの集まりです。
title: ファイルベースの宛先のテスト API
exl-id: 2733fd00-af08-4763-a30e-a53ee56c0a19
source-git-commit: 44e056407f5089c927752f00cc6bf173d7640b83
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 3%

---

# ファイルベースの宛先のテスト API

## 概要 {#overview}

ファイルベースの宛先テスト API は、Destination SDKを通じて構築されたファイルベースの宛先の設定を検証するために使用できる一連のエンドポイントです。

これらのツールを使用して、以前に設定を検証することをお勧めします。 [送信](submit-destination.md) レビュー先のAdobe

最も良いテスト結果を得るには、次のフロー図に基づいてこの API を使用することをお勧めします。

![推奨される宛先テストフローを示す図](assets/file-based-testing-flow.png)

各エンドポイントで実行できる操作の概要については、以下の節を参照してください。

## サンプルプロファイルを生成 {#generate-sample-profiles}

以下を使用： `/sample-profiles` 既存のソーススキーマに基づいてサンプルプロファイルを生成する API エンドポイント。

サンプルプロファイルは、プロファイルの JSON 構造を理解するのに役立ちます。 さらに、独自のプロファイルデータを使用してをカスタマイズし、宛先テストを実施するためのデフォルトが提供されます。

詳しくは、 [専用ドキュメント](file-based-sample-profile-generation-api.md) を参照してください。

## 宛先設定のテスト {#test-destination-configuration}

以下を使用： `/testing/destinationInstance` ファイルベースの宛先が正しく設定されているかどうかをテストし、設定した宛先へのデータフローの整合性を検証するための API エンドポイント。

テストエンドポイントには、を追加するかどうかに関わらず、リクエストをおこなうことができます。 [サンプルプロファイル](file-based-sample-profile-generation-api.md) を呼び出しに追加します。 リクエストでプロファイルを送信しない場合、API はサンプルプロファイルを自動的に生成し、リクエストに追加します。

詳しくは、 [専用ドキュメント](file-based-destination-testing-api.md) を参照してください。

## 詳細なアクティベーション結果の表示 {#view-detailed-activation-results}

以下を使用： `/testing/destinationInstance` ファイルベースの宛先テスト結果の完全な詳細を表示する API エンドポイント。

この API エンドポイントは、 [フローサービス API](../api/update-destination-dataflows.md) データフローを監視する。

詳しくは、 [専用ドキュメント](file-based-destination-results-api.md) を参照してください。

## 顧客データフィールドをレンダリング {#render-customer-data-fields}

以下を使用： `/authoring/testing/template/render` テンプレート化の仕組みを視覚化する API エンドポイント [顧客データフィールド](file-based-destination-configuration.md#customer-data-fields) の宛先設定で定義されたは次のようになります。

API エンドポイントは、顧客データフィールドにランダムな値を生成し、応答で返します。 これにより、バケット名やフォルダーパスなどの顧客データフィールドのセマンティック構造を検証できます。

詳しくは、 [専用ドキュメント](file-based-render-template-api.md) 顧客データフィールドの値を生成および視覚化する方法を学ぶには、以下を参照してください。
