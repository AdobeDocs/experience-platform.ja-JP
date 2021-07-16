---
title: Edge 拡張機能の条件のタイプ
description: Adobe Experience Platformのエッジ拡張機能に条件タイプライブラリモジュールを定義する方法について説明します。
source-git-commit: 39d9468e5d512c75c9d540fa5d2bcba4967e2881
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 58%

---

# エッジ拡張機能の条件のタイプ

>[!NOTE]
>
> Adobe Experience Platform Launch は、Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

条件タイプライブラリモジュールは、何かがtrueかfalseかを評価し、ブール値を返します。

>[!IMPORTANT]
>
>このドキュメントでは、エッジ拡張機能の条件のタイプについて説明します。web 拡張機能を開発する場合は、[web 拡張機能の条件のタイプ](../web/condition-types.md)に関するガイドを参照してください。
>
>このドキュメントは、ライブラリモジュールと、それらがタグ拡張に統合される仕組みについても精通していることを前提としています。 説明が必要な場合は、このガイドに戻る前に、[ライブラリモジュールの形式](./format.md)の概要を参照してください。

例えば、ユーザーが`example.com`ホスト上にいるかどうかを評価する場合、モジュールは次のようになります。

```js
module.exports = (context) => {
  const URL = context.arc.event.xdm.web.webpageDetails.URL;
  return URL.endsWith("adobelaunch.com");
};
```

ユーザーがホスト名を設定できるようにして、ホスト名の入力を許可し、設定オブジェクトに保存する場合、オブジェクトは次の例のようになります。

```js
{
  "hostname": "example.com"
}
```

ユーザー定義のホスト名を操作するには、モジュールを次のように変更する必要があります。

```js
module.exports = (context) => {
  const URL = context.arc.event.xdm.web.webpageDetails.URL;
  return URL.endsWith(settings.hostname);
};
```

## 条件の結果

条件モジュールから返される結果は、次のいずれかになります。

1. ブール値（`true` または `false`）。
1. 解決後にブール値を返す[プロミス](https://developer.mozilla.org/ja-JP/docs/Web/JavaScript/Reference/Global_Objects/Promise)。

## ライブラリモジュールコンテキスト

すべての条件モジュールは、モジュールの呼び出し時に提供される `context` 変数にアクセスできます。詳しくは、[こちら](./context.md)を参照してください。
