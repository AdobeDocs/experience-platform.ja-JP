---
title: 検証エディタービュー
description: このガイドでは、Adobe Experience Platform Assurance の検証エディタービューに関する詳細情報を説明します。
source-git-commit: 5778d4db27d0f57281821dc8e042a31b69745514
workflow-type: tm+mt
source-wordcount: '527'
ht-degree: 4%

---


# 検証エディタービュー

検証エディターを使用すると、Adobe Experience Platform Assurance セッションでイベントを検証する JavaScript 関数をすばやく簡単に管理できます。 各関数は、アシュランスセッションでイベントを受け取ります。 関数を記述して、クライアントの設定、イベント条件、テストおよび使用例を検証できます。

## 検証エディターの概要

後 [アシュランスの設定](../tutorials/implement-assurance.md)、 **[!UICONTROL ホーム]** 表示、選択 **[!UICONTROL 検証エディター]**.

![Validation-Editor-Screen-Shot](https://user-images.githubusercontent.com/6597105/198680074-f548a646-6f2f-4a65-82fd-0f1687d869bf.png)

## 検証関数を作成する

この機能を使用すると、Adobe Experience Platform Assurance セッションの検証機能を作成、編集または削除できます。

1. 選択 **[!UICONTROL 新しい検証の作成]**.
2. を入力します。 **名前** 検証を識別するには、 **カテゴリ** および **説明**.
3. エディターでコードを編集し、アシュランスセッションのイベントを検証します。

関数のテストが完了したら、「 」を選択します。 **[!UICONTROL 公開]** 検証を保存します。

### イベント定義

| キー | タイプ | 説明 |
| :--- | :--- | :--- |
| `uuid` | 文字列 | イベントの汎用的に一意の識別子。 |
| `timestamp` | 数値 | イベントが Assurance に送信された際のクライアントからのタイムスタンプ。 |
| `eventNumber` | 数値 | イベントが送信された際に注文に使用されます。 このキーは、イベントのタイムスタンプが同じ場合に役立ちます。 |
| `vendor` | 文字列 | 逆ドメイン名フォーマットのベンダー識別文字列（例：com.adobe.assurance）。 |
| `type` | 文字列 | イベントのタイプを示すために使用されます。 |
| `payload` | オブジェクト | イベントのデータを定義し、一意の共通プロパティを含みます。 一般的なプロパティには次のものがあります。 `ACPExtensionEventSource` および `ACPExtensionEventType`. |
| `annotations` | 配列 | 注釈オブジェクトの配列。 |

### 注釈定義

| キー | タイプ | 説明 |
| :--- | :--- | :--- |
| `uuid` | 文字列 | 注釈の汎用的な一意の識別子。 |
| `type` | 文字列 | 注釈のタイプを示すために使用され、通常はプラグインの名前です（例：analytics）。 |
| `payload` | オブジェクト | イベントを補完する必要があるデータを定義します。 Adobe Analyticsの場合、後処理されたヒットデータはここに格納されます。 |

### 検証結果

検証関数は、次の値を含むオブジェクトを返す必要があります。

| キー | タイプ | 説明 |
| :--- | :--- | :--- |
| `message` | 文字列 | 概要結果に表示する検証メッセージ。 |
| `events` | 配列 | 一致としてレポートされる、または一致しないとレポートされるイベント UID の配列。 |
| `links` | 配列 | の配列 `ValidationResultLink` ドキュメントおよびその他のリソースを参照するオブジェクト `{( type: 'doc'|'product', url: String )}` |
| `result` | 文字列 | これは検証結果で、列挙された文字列の 1 つと見なされます。&quot;matched&quot;、&quot;not matched&quot;、&quot;unknown&quot; |

## 検証結果の表示

関数の結果は、コードエディターの下の結果セクションに表示されます。 検証結果が `unknown` または `not matched` そして `events` 配列に 1 つ以上の値が含まれています `uuids`を指定した場合、イベントはタイムライン内で次の色でハイライト表示されます。

* 緑 — 一致
* オレンジ — 不明
* 赤 — 一致なし

![Timeling-Validation-Highlights-Screen-Shot](https://user-images.githubusercontent.com/6597105/198681412-93d10a5a-3212-4e85-850a-aeaf5caf0521.png)

## トラブルシューティング

次の項目を追加できます。 `console.log()` 関数内で開発者コンソールに項目を印刷します。 または、結果オブジェクトの message プロパティを使用して、結果パネルにメッセージをデバッグできます。

JavaScript コードエディターでエラーが発生した場合は、エラーステータスと理由が表示されます。

検証の詳細については、 [Adobe Experience Platform Assurance の検証](https://github.com/adobe/griffon-validation-plugins) GitHub。 次に、Adobeが所有する検証の例を示します。 詳しくは、 [wiki](https://github.com/adobe/griffon-validation-plugins/wiki) を参照してください。
