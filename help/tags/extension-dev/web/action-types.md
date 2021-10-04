---
title: Web 拡張機能のアクションの種類
description: Web プロパティでタグ拡張機能のアクションタイプライブラリモジュールを定義する方法について説明します。
exl-id: d4539132-a72c-40b0-84b6-50cbe3785d2d
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '408'
ht-degree: 100%

---

# Web 拡張機能のアクションの種類

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

データ収集タグのコンテキストでは、アクションとは、ルールイベントが発生し、すべての条件が評価を渡した後に実行される操作です。

例えば、拡張機能で「show support chat」アクションタイプを提供すると、サポートチャットダイアログを表示して、チェックアウトに苦労しているユーザーを支援できます。

このドキュメントでは、Adobe Experience Platform の Web 拡張機能のアクションタイプを定義する方法について説明します。

>[!IMPORTANT]
>
>このドキュメントでは、Web 拡張機能のアクションの種類について説明します。エッジ拡張機能を開発する場合は、代わりに [エッジ拡張機能のアクションタイプ](../edge/action-types.md) に関するガイドを参照してください。
>
>このドキュメントでは、読者がライブラリモジュールと、ライブラリモジュールが web 拡張機能に統合される仕組みについて理解していることを前提としています。説明が必要な場合は、このガイドに戻る前に、 [ライブラリモジュールの形式](./format.md) の概要を参照してください。

通常、アクションタイプは次の要素で構成されます。

1. データ収集 UI に表示される [ビュー](./views.md)。アクションの設定を変更できます。
2. タグのランタイムライブラリ内で生成されるライブラリモジュール。設定を解釈し、アクションを実行します。

```js
module.exports = function(settings) {
  alert('Thanks for visiting our site!');
};
```

例えば、Adobe Experience Platform ユーザーがメッセージを設定できるように、ユーザーに対し、メッセージを入力して settings オブジェクトに保存することを許可できます。 オブジェクトは次のようになります。

```json
{
  "message": "Thank you for being one of our VIP members!"
}
```

ユーザー定義のメッセージを操作するには、モジュールを次のように変更する必要があります。

```js
module.exports = function(settings) {
  alert(settings.message);
}
```

## コンテキストイベントデータ

次に、2 つ目の引数を、ルールを実行するイベントに関するコンテキスト情報を含むモジュールに渡す必要があります。 これは状況によってはメリットがあり、次のように指定することで実現できます。

```js
module.exports = function(settings, event) {
  // event contains information regarding the event that fired the rule
};
```

`event` オブジェクトには次のプロパティが含まれている必要があります。

| プロパティ | 説明 |
| --- | --- |
| `$type` | 拡張機能名とイベント名を表す文字列（ピリオドを使用して結合）。例：`youtube.play` |
| `$rule` | 現在実行中のルールに関する情報を含むオブジェクト。 オブジェクトには、次のサブプロパティが含まれている必要があります。<ul><li>`id`：現在実行中のルールの ID。</li><li>`name`：現在実行中のルールの名前。</li></ul> |

ルールをトリガーしたイベントタイプを提供する拡張機能では、必要に応じて、その他の役に立つ情報をこの `event` オブジェクトに追加できます。
