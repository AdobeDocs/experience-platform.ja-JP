---
title: エッジ拡張機能のアクションタイプ
description: エッジプロパティでタグ拡張のアクションタイプライブラリモジュールを定義する方法について説明します。
source-git-commit: 39d9468e5d512c75c9d540fa5d2bcba4967e2881
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 49%

---

# エッジ拡張機能のアクションタイプ

>[!NOTE]
>
>Adobe Experience Platform Launch は、Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

アクションタイプライブラリモジュールは、事前に定義されたアクションを実行するように設計されています。 このアクションの効果は、作成者によって完全に定義されます。 モジュールは、ビーコンとして作成したり、イベントから一部のデータを変換したりできます。

>[!IMPORTANT]
>
>このドキュメントでは、エッジ拡張機能のアクションタイプについて説明します。web 拡張機能を開発する場合は、[web 拡張機能のアクションタイプ](../web/action-types.md)に関するガイドを参照してください。
>
>このドキュメントは、ライブラリモジュールと、それらがタグ拡張に統合される仕組みについても精通していることを前提としています。 説明が必要な場合は、このガイドに戻る前に、[ライブラリモジュールの形式](./format.md)の概要を参照してください。

例えば、一部のデータをサードパーティのエンドポイントに転送するモジュールは、次のようになります。

```js
module.exports = (context) {
  const { arc, utils } = context;
  const { fetch } = utils;
  const { event: { xdm } } = arc;
  return fetch('http://someendpoint.com', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(xdm)
  })
};
```

ユーザーがエンドポイントを設定できるようにし、モジュール内のsettingsオブジェクトに対してエンドポイントの入力と永続性を許可する場合、オブジェクトは次のようになります。

```json
{
  "endpoint": "http://someendpoint.com"
}
```

ユーザー定義のエンドポイントを操作するには、モジュールを次の例に変更する必要があります。

```js
module.exports = (context) {
  const { arc, utils } = context;
  const { fetch } = utils;
  const { event: { xdm } } = arc;
  const  { endpoint } = settings;
  return fetch(endpoint, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(xdm)
  })
};
```

## アクションの結果

アクションモジュールからは、どのような結果でも返すことができます。アクションで非同期タスクを実行する必要がある場合は、解決後に必要な結果を返す[プロミス](https://developer.mozilla.org/ja-JP/docs/Web/JavaScript/Reference/Global_Objects/Promise)を返すことができます。

アクションの結果は `ruleStash` キーに保存されます。このキーは、`context` パラメーター（`context.arc.ruleStash`）を介してすべてのモジュールで使用できるようになります。`ruleStash` について詳しくは、[こちら](./context.md#rulestash)を参照してください。
