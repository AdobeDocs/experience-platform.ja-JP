---
title: appendIdentityToUrl
description: アプリ間、web 間およびドメイン間で、パーソナライズされたエクスペリエンスをより正確に提供します。
exl-id: 09dd03bd-66d8-4d53-bda8-84fc4caadea6
source-git-commit: 153c5bae42c027c25a38a8b63070249d1b1a8f01
workflow-type: tm+mt
source-wordcount: '412'
ht-degree: 3%

---

# `appendIdentityToUrl`

この `appendIdentityToUrl` コマンドを使用すると、ユーザー識別子をクエリ文字列として URL に追加できます。 このアクションにより、ドメイン間で訪問者の ID を持ち歩き、ドメインまたはチャネルの両方を含むデータセットに対して、重複した訪問者数を防ぐことができます。 Web SDK バージョン 2.11.0 以降で使用できます。

生成され、URL に追加されるクエリ文字列はです `adobe_mc`. Web SDK で ECID が見つからない場合は、を呼び出します `/acquire` エンドポイントを生成します。

>[!NOTE]
>
>同意が指定されていない場合、このメソッドからの URL は変更されずに返されます。 このコマンドは直ちに実行され、同意の更新を待つことはありません。

## Web SDK 拡張機能を使用した URL への ID の追加 {#extension}

URL への ID の追加は、Adobe Experience Platform Data Collection タグインターフェイスのルール内のアクションとして実行されます。

1. へのログイン [experience.adobe.com](https://experience.adobe.com) Adobe IDの資格情報を使用します。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL ルール]**&#x200B;を選択してから、目的のルールを選択します。
1. 次の下 [!UICONTROL アクション]、既存のアクションを選択するか、アクションを作成します。
1. を [!UICONTROL 拡張機能] ドロップダウンフィールドの移動先 **[!UICONTROL Adobe Experience Platform Web SDK]**、を設定します。 [!UICONTROL アクションタイプ] 対象： **[!UICONTROL ID でリダイレクト]**.
1. クリック **[!UICONTROL 変更を保持]**&#x200B;次に、公開ワークフローを実行します。

このコマンドは、通常、クリックをリッスンして目的のドメインを確認する特定のルールと共に使用されます。

+++ルールイベントの条件

を使用したアンカータグ時のトリガー `href` プロパティがクリックされました。

* **[!UICONTROL 拡張機能]**：コア
* **[!UICONTROL イベントタイプ]**：クリック
* **[!UICONTROL ユーザーがをクリックした場合]**：特定の要素
* **[!UICONTROL CSS セレクターに一致する要素]**: `a[href]`

![ルールイベント](../assets/id-sharing-event-configuration.png)

+++

+++ルール条件

目的のドメインのみでトリガーします。

* **[!UICONTROL 論理タイプ]**：標準
* **[!UICONTROL 拡張機能]**：コア
* **[!UICONTROL 条件タイプ]**：値の比較
* **[!UICONTROL 左オペランド]**: `%this.hostname%`
* **[!UICONTROL 演算子]**：正規表現に一致
* **[!UICONTROL 右オペランド]**：目的のドメインに一致する正規表現。 例：`adobe.com$|behance.com$`

![ルール条件](../assets/id-sharing-condition-configuration.png)

+++

+++ルールアクション

URL に ID を追加します。

* **[!UICONTROL 拡張機能]**：Adobe Experience Platform Web SDK
* **[!UICONTROL アクションタイプ]**:ID を使用したリダイレクト

![ルールアクション](../assets/id-sharing-action-configuration.png)

+++

## Web SDK JavaScript ライブラリを使用して URL に ID を追加

を実行 `appendIdentityToUrl` URL をパラメーターとして使用するコマンド。 メソッドは、識別子がクエリ文字列として追加された URL を返します。

```js
alloy("appendIdentityToUrl",document.location);
```

ページ上で受け取ったすべてのクリックに関するイベントリスナーを追加し、URL が目的のドメインに一致するかどうかを確認できます。 追加される場合は、URL に ID を追加し、ユーザーをリダイレクトします。

```js
document.addEventListener("click", event => {
  // Check if the click was a link
  const anchor = event.target.closest("a");
  if (!anchor || !anchor.href) return;

  // Check if the link points to the desired domain
  const url = new URL(anchor.href);
  if (!url.hostname.endsWith(".adobe.com") && !url.hostname.endsWith(".behance.com")) return;

  // Append the identity to the URL, then direct the user to the URL
  event.preventDefault();
  alloy("appendIdentityToUrl", {url: anchor.href}).then(result => {document.location = result.url;});
});
```

## 応答オブジェクト

以下を行う場合 [応答を処理](command-responses.md) このコマンドを使用すると、応答オブジェクトには次のものが含まれます **`url`**:ID 情報を含む新しい URL をクエリ文字列パラメーターとして追加します。
