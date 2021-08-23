---
title: エッジ拡張機能のアクションタイプ
description: エッジプロパティでタグ拡張機能用にアクションタイプのライブラリモジュールを定義する方法について説明します。
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 66%

---

# エッジ拡張機能のアクションタイプ

>[!NOTE]
>
>Adobe Experience Platform Launchは、Adobe Experience Platformのデータ収集テクノロジーのスイートとしてリブランドされました。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

タグルールでは、アクションとは、ルール条件が評価を渡した後に実行される操作です。 アクションタイプは拡張機能によって提供され、その効果は拡張機能の作成者によって完全に定義されます。

例えば、拡張機能で「show support chat」アクションタイプを提供すると、サポートチャットダイアログを表示して、チェックアウトに苦労しているユーザーを支援できます。

このドキュメントでは、Adobe Experience Platformのエッジ拡張機能のアクションタイプを定義する方法について説明します。

>[!IMPORTANT]
>
>web 拡張機能を開発する場合は、[web 拡張機能のアクションタイプ](../web/action-types.md)に関するガイドを参照してください。
>
>このドキュメントは、ライブラリモジュールと、それらがエッジ拡張機能に統合される仕組みについても精通していることを前提としています。 説明が必要な場合は、このガイドに戻る前に、[ライブラリモジュールの形式](./format.md)の概要を参照してください。

通常、アクションタイプは次の要素で構成されます。

1. データ収集UI内に表示されるビューで、ユーザーがアクションの設定を変更できます。
2. 設定を解釈し、アクションを実行するためにタグランタイムライブラリ内で生成されるライブラリモジュール。

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

ユーザーがエンドポイントを設定できるようにし、モジュール内の settings オブジェクトに対してエンドポイントの入力と永続性を許可する場合、オブジェクトは次のようになります。

```json
{
  "endpoint": "http://someendpoint.com"
}
```

ユーザー定義のエンドポイントを操作するには、モジュールを次のように変更する必要があります。

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
