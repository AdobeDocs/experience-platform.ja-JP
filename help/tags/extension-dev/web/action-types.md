---
title: Web 拡張機能のアクションの種類
description: Webプロパティでタグ拡張のアクションタイプライブラリモジュールを定義する方法について説明します。
source-git-commit: 39d9468e5d512c75c9d540fa5d2bcba4967e2881
workflow-type: tm+mt
source-wordcount: '338'
ht-degree: 65%

---

# Web 拡張機能のアクションの種類

>[!NOTE]
>
>Adobe Experience Platform Launch は、Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

アクションタイプライブラリモジュールは、事前に定義されたアクションを実行することを目的としています。 このアクションで何をおこなうかは、ユーザーが決定します。ビーコンの送信、オファーの表示、訪問者に対する感謝、Cookie の保存、サポートチャットの開始などをおこなえます。

>[!IMPORTANT]
>
>このドキュメントでは、Web 拡張機能のアクションの種類について説明します。エッジ拡張機能を開発する場合は、代わりに[エッジ拡張機能のアクションタイプ](../edge/action-types.md)に関するガイドを参照してください。
>
>このドキュメントは、ライブラリモジュールと、それらがタグ拡張に統合される仕組みについても精通していることを前提としています。 説明が必要な場合は、このガイドに戻る前に、[ライブラリモジュールの形式](./format.md)の概要を参照してください。

```js
module.exports = function(settings) {
  alert('Thanks for visiting our site!');
};
```

例えば、Adobe Experience Platformユーザーがメッセージを設定できるように、ユーザーがメッセージを入力して設定オブジェクトに保存できるようにすることができます。 次のようなオブジェクト。

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

次に、2つ目の引数を、ルールを実行するイベントに関するコンテキスト情報を含むモジュールに渡す必要があります。 これは状況によってはメリットがあり、次のように指定することで実現できます。

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
