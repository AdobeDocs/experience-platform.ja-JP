---
title: エッジ拡張機能のデータ要素のタイプ
description: エッジプロパティでタグ拡張のデータ要素タイプライブラリモジュールを定義する方法について説明します。
source-git-commit: 39d9468e5d512c75c9d540fa5d2bcba4967e2881
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 46%

---

# エッジ拡張機能のデータ要素タイプ

>[!NOTE]
>
>Adobe Experience Platform Launch は、Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

データ要素タイプライブラリモジュールでは、データの一部が取得されます。モジュール作成者は、このデータの取得方法を決定します。 例えば、データ要素タイプを使用して、Adobe Experience PlatformユーザーがXDMレイヤーまたはカスタムデータレイヤーからデータを取得できるようにすることができます。

>[!IMPORTANT]
>
>このドキュメントでは、エッジ拡張機能のデータ要素のタイプについて説明します。web 拡張機能を開発する場合は、[web 拡張機能のデータ要素タイプ](../web/data-element-types.md)に関するガイドを参照してください。
>
>このドキュメントは、ライブラリモジュールと、それらがタグ拡張に統合される仕組みについても精通していることを前提としています。 説明が必要な場合は、このガイドに戻る前に、[ライブラリモジュールの形式](./format.md)の概要を参照してください。

ユーザーがカスタムデータレイヤーからデータを取得できるようにする場合、モジュールは次の例のようになります。

```js
module.exports = (context) => {
  const productName = context.arc.event.data.productName;
  return productName;
};
```

Adobe Experience Platformユーザーが、返されるデータレイヤーのデータを設定できるようにする場合は、ユーザーがキー名を入力し、その名前を`settings`オブジェクトに保存できるようにします。 オブジェクトは次のようになります。

```js
{
  keyName: "campaignId"
}
```

ユーザー定義のローカルストレージ項目名を操作するには、モジュールを次のように変更する必要があります。

```js
module.exports = (context) => {
  const data = context.arc.event.data;
  return data[keyName];
};
```

## ライブラリモジュールコンテキスト

すべてのデータ要素モジュールは、モジュールの呼び出し時に提供される `context` 変数にアクセスできます。詳しくは、[こちら](./context.md)を参照してください。
