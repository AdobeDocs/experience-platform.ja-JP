---
title: Edge 拡張機能の条件のタイプ
description: Adobe Experience Platform のエッジ拡張機能の条件タイプライブラリモジュールを定義する方法について説明します。
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: ht
source-wordcount: '408'
ht-degree: 100%

---

# Edge 拡張機能の条件のタイプ

>[!NOTE]
>
> Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

タグのルールでは、条件はイベントの発生後に評価されます。 ルールの処理を続行するには、すべての条件が true を返す必要があります。 条件タイプは拡張機能によって提供され、対象が true と false のどちらであるかを評価します。

例えば、拡張機能には「viewport contains」条件タイプを指定できます。この条件タイプでは、 のユーザーが CSS セレクターを指定できます。 この条件がクライアントの Web サイトで評価されると、拡張機能は、CSS セレクターに一致する要素を見つけ出し、その要素のいずれかがユーザーのビューポート内に含まれているかどうかを返すことができます。

このドキュメントでは、Adobe Experience Platform のエッジ拡張機能の条件タイプを定義する方法について説明します。

>[!IMPORTANT]
>
>web 拡張機能を開発する場合は、 [web 拡張機能の条件のタイプ](../web/condition-types.md) に関するガイドを参照してください。
>
>このドキュメントでは、読者がライブラリモジュールと、ライブラリモジュールがエッジ拡張機能に統合される仕組みについて理解していることを前提としています。説明が必要な場合は、このガイドに戻る前に、 [ライブラリモジュールの形式](./format.md) の概要を参照してください。

通常、条件タイプは次の要素で構成されます。

1. データ収集 UI に表示されるビュー。条件の設定を変更できます。
2. タグのランタイムライブラリ内で生成されるライブラリモジュール。設定を解釈し、条件を評価します。

例えば、ユーザーが `example.com` ホスト上にいるかどうかを評価したい場合、モジュールは次のようになります。

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

すべての条件モジュールは、モジュールの呼び出し時に提供される `context` 変数にアクセスできます。詳しくは、 [こちら](./context.md) を参照してください。
