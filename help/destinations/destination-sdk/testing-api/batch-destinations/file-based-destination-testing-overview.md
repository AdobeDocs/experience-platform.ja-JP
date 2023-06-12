---
description: ファイルベースの宛先テスト API を使用して、Destination SDK で作成されたファイルベースの宛先の設定を検証する方法を説明します。
title: ファイルベースの宛先のテスト API
exl-id: 2733fd00-af08-4763-a30e-a53ee56c0a19
source-git-commit: adf75720f3e13c066b5c244d6749dd0939865a6f
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 100%

---


# ファイルベースの宛先のテスト API の概要

ファイルベースの宛先テスト API は、Destination SDK で作成されたファイルベースの宛先の設定を検証するために使用できる、エンドポイントのセットです。

宛先をレビュー用にアドビに[送信](../../guides/submit-destination.md)する前に、これらのツールを使用して設定を検証することをお勧めします。

最良のテスト結果を得るために、以下のフロー図に基づいてこの API を使用することをお勧めします。

![推奨される宛先テストフローを示す図](../../assets/testing-api/batch-destinations/file-based-testing-flow.png)

各エンドポイントでできることの簡潔な概要については、以下の節を参照してください。

## サンプルプロファイルの生成 {#generate-sample-profiles}

`/sample-profiles` API エンドポイントを使用して、既存のソーススキーマに基づいて、サンプルプロファイルを生成します。

サンプルプロファイルは、プロファイルの JSON 構造を理解するのに役立ちます。また、これをデフォルトとして、独自のプロファイルデータでカスタマイズすることで、より詳細に宛先をテストできます。

サンプルプロファイルの生成方法については、[専用のドキュメント](file-based-sample-profile-generation-api.md)を参照してください。

## 宛先設定のテスト {#test-destination-configuration}

`/testing/destinationInstance` API エンドポイントを使用して、ファイルベースの宛先が正しく設定されているかどうかをテストしたり、設定された宛先に対するデータフローの整合性を検証したりします。

呼び出しに[サンプルプロファイル](file-based-sample-profile-generation-api.md)を追加してもしなくても、エンドポイントをテストするためのリクエストを行うことができます。リクエスト時に任意のプロファイルを送信しない場合、API は、サンプルプロファイルを自動的に生成して、リクエストに追加します。

サンプルプロファイルを使用した宛先設定のテスト方法について詳しくは、[専用のドキュメント](file-based-destination-testing-api.md)を参照してください。

## 詳細なアクティベーション結果の表示 {#view-detailed-activation-results}

`/testing/destinationInstance` API エンドポイントを使用して、ファイルベースの宛先テスト結果の完全な詳細を表示できます。

この API エンドポイントは、データフローを監視するための[フローサービス API](../../../api/update-destination-dataflows.md) を使用する際に取得するのと同じ結果を返します。

詳細なアクティベーション結果を表示する方法について詳しくは、[専用のドキュメント](file-based-destination-results-api.md)を参照してください。

## 顧客データフィールドのレンダリング {#render-customer-data-fields}

`/authoring/testing/template/render` API エンドポイントを使用して、宛先設定で定義された、テンプレート化された[顧客データフィールド](../../functionality/destination-configuration/customer-data-fields.md)の外観を視覚化します。

この API エンドポイントは、顧客データフィールド用にランダムな値を生成し、応答でそれらを返します。これは、顧客データフィールド（バケット名やフォルダーパスなど）のセマンティック構造を検証するのに役立ちます。

顧客データフィールドの値を生成および視覚化する方法について詳しくは、[専用のドキュメント](file-based-render-template-api.md)を参照してください。
