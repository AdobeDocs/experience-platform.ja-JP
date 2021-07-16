---
title: Web 拡張機能の条件のタイプ
description: Webプロパティでタグ拡張の条件タイプライブラリモジュールを定義する方法について説明します。
source-git-commit: 39d9468e5d512c75c9d540fa5d2bcba4967e2881
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 86%

---

# Web 拡張機能の条件のタイプ

>[!NOTE]
>
>Adobe Experience Platform Launch は、Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

条件タイプライブラリモジュールの目的は、何が true か false かを評価することです。何を評価するかは、自由に設定できます。

>[!NOTE]
>
>このドキュメントでは、Web 拡張機能の条件のタイプについて説明します。エッジ拡張機能を開発する場合は、代わりに[エッジ拡張機能の条件のタイプ](../edge/condition-types.md)に関するガイドを参照してください。
>
>このドキュメントは、ライブラリモジュールと、それらがタグ拡張に統合される仕組みについても精通していることを前提としています。 説明が必要な場合は、このガイドに戻る前に、[ライブラリモジュールの形式](./format.md)の概要を参照してください。

例えば、ユーザーが `example.com` ホスト上にいるかどうかを評価したい場合、モジュールは次のようになります。

```js
module.exports = function(settings) {
  return document.location.hostname === 'example.com';
};
```

次に、Adobe Experience Platform ユーザーがホスト名を設定できるようにする場合を考えてみましょう。 ユーザーがホスト名を入力し、そのホスト名を設定オブジェクトに保存できるようにします。オブジェクトは次のようになります。

```js
{
  "hostname": "example.com"
}
```

ユーザー定義のホスト名を操作するには、モジュールを次のように変更する必要があります。

```js
module.exports = function(settings) {
  return document.location.hostname === settings.hostname;
};
```

## コンテキストイベントデータ

2 つ目の引数は、ルールを起動したイベントに関するコンテキスト情報を含むモジュールに渡されます。 これは状況によってはメリットがあり、次のように指定することで実現できます。

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
