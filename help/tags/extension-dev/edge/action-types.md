---
title: エッジ拡張機能のアクションタイプ
description: エッジプロパティでタグ拡張機能用にアクションタイプのライブラリモジュールを定義する方法について説明します。
exl-id: c0b058aa-f0fe-4fd8-a873-018482c3e4db
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 100%

---

# エッジ拡張機能のアクションタイプ

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

タグのルールでは、アクションとは、ルールの条件が評価を渡した後に実行される操作です。アクションタイプは拡張機能によって提供され、その効果は拡張機能の作成者が定義します。

例えば、拡張機能で「show support chat」アクションタイプを提供すると、サポートチャットダイアログを表示して、チェックアウトに苦労しているユーザーを支援できます。

このドキュメントでは、Adobe Experience Platform のエッジ拡張機能のアクションタイプを定義する方法について説明します。

>[!IMPORTANT]
>
>web 拡張機能を開発する場合は、 [web 拡張機能のアクションタイプ](../web/action-types.md) に関するガイドを参照してください。
>
>このドキュメントでは、読者がライブラリモジュールと、ライブラリモジュールがエッジ拡張機能に統合される仕組みについて理解していることを前提としています。説明が必要な場合は、このガイドに戻る前に、 [ライブラリモジュールの形式](./format.md) の概要を参照してください。

通常、アクションタイプは次の要素で構成されます。

1. データ収集 UI に表示されるビュー。アクションの設定を変更できます。
2. タグのランタイムライブラリ内で生成されるライブラリモジュール。設定を解釈し、アクションを実行します。

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

ユーザー定義のエンドポイントを操作するには、モジュールを次の例のように変更する必要があります。

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

アクションの結果は `ruleStash` キーに保存されます。このキーは、`context` パラメーター（`context.arc.ruleStash`）を介してすべてのモジュールで使用できるようになります。`ruleStash` について詳しくは、 [こちら](./context.md#rulestash) を参照してください。
