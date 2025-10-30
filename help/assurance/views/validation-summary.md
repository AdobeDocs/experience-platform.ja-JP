---
title: 検証エディタービュー
description: このガイドでは、Adobe Experience Platform Assuranceの検証エディタービューについて詳しく説明します。
exl-id: 09be531c-8dc3-48b8-814f-b7a06adf1da3
source-git-commit: be2ad7a02d4bdf5a26a0847c8ee7a9a93746c2ad
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 5%

---

# 検証エディタービュー

検証エディターを使用すると、JavaScript機能を素早く簡単に管理して、Adobe Experience Platform Assurance セッション内のイベントを検証できます。 各関数は、Assurance セッションでイベントを受け取ります。 クライアント設定、イベント条件、テストおよびユースケースを検証する関数を作成できます。

## 検証エディターの概要

[Assuranceの設定 ](../tutorials/implement-assurance.md) の後、**[!UICONTROL Home]** ビューで「**[!UICONTROL Validation Editor]**」を選択します。

![Validation-Editor-Screen-Shot](https://user-images.githubusercontent.com/6597105/198680074-f548a646-6f2f-4a65-82fd-0f1687d869bf.png)

## 検証関数の記述

この機能を使用すると、Adobe Experience Platform Assurance セッションの検証関数を作成、編集または削除できます。

1. **[!UICONTROL Create a New Validation]** を選択します。
2. 検証を識別する **名前** を入力し、**カテゴリ** と **説明** を入力します。
3. エディターでコードを編集して、Assurance セッションのイベントを検証します。

関数テストが完了したら、「**[!UICONTROL Publish]**」を選択して検証を保存します。

### イベント定義

| キー | タイプ | 説明 |
| :--- | :--- | :--- |
| `uuid` | 文字列 | イベントのユニバーサル固有識別子（UUID）。 |
| `timestamp` | 数値 | イベントがAssuranceに送信されたときのクライアントからのタイムスタンプ。 |
| `eventNumber` | 数値 | イベントが送信された際の注文に使用します。 このキーは、イベントのタイムスタンプが同じ場合に役立ちます。 |
| `vendor` | 文字列 | 逆ドメイン名形式のベンダー識別文字列（例：com.adobe.assurance）。 |
| `type` | 文字列 | イベントのタイプを示すために使用されます。 |
| `payload` | オブジェクト | イベントのデータを定義し、一意で共通のプロパティを含みます。 一部の共通プロパティには、`ACPExtensionEventSource` および `ACPExtensionEventType` があります。 |
| `annotations` | 配列 | 注釈オブジェクトの配列。 |

### 注釈の定義

| キー | タイプ | 説明 |
| :--- | :--- | :--- |
| `uuid` | 文字列 | 注釈のユニバーサル固有識別子（UUID）。 |
| `type` | 文字列 | 注釈のタイプを示すために使用され、通常はプラグインの名前です（analytics など）。 |
| `payload` | オブジェクト | イベントを補完する必要があるデータを定義します。 Adobe Analyticsの場合、後処理されたヒットデータが含まれる場所になります。 |

### 検証結果

検証関数は、次を含むオブジェクトを返すと想定されています。

| キー | タイプ | 説明 |
| :--- | :--- | :--- |
| `message` | 文字列 | 概要結果に表示する検証メッセージ。 |
| `events` | 配列 | 一致または一致しないとレポートされるイベント uuid の配列。 |
| `links` | 配列 | ドキュメントおよびその他のリソースを参照する `ValidationResultLink` オブジェクトの配列 `{( type: 'doc'`&amp;vert;`'product', url: String )}` |
| `result` | 文字列 | これは検証結果で、「matched」、「not matched」、「unknown」などの列挙文字列のいずれかであることが想定されます |

## 検証結果の表示

関数の結果は、コードエディターの下の結果セクションに表示されます。 検証結果が `unknown` または `not matched` で、`events` 配列に 1 つ以上の `uuids` が含まれている場合、イベントはタイムラインで次の色でハイライト表示されます。

* 緑 – 一致
* オレンジ – 不明
* 赤 – 一致しません

![Timeling-Validation-Highlights-Screen-shot](https://user-images.githubusercontent.com/6597105/198681412-93d10a5a-3212-4e85-850a-aeaf5caf0521.png)

## トラブルシューティング

関数に `console.log()` を追加して、開発者コンソールに項目を印刷できます。 または、結果オブジェクトの message プロパティを使用して、結果パネルに対するメッセージをデバッグすることもできます。

JavaScript コードエディターでエラーが発生した場合は、エラーステータスと理由が表示されます。

検証について詳しくは、[Adobe Experience Platform Assuranceの検証 ](https://github.com/adobe/griffon-validation-plugins) GitHub を参照してください。 Adobeが所有する検証の例をご覧いただけます。 検証の詳細については、[wiki](https://github.com/adobe/griffon-validation-plugins/wiki) を参照してください。
