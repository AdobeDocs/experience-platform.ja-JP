---
title: appendIdentityToUrl
description: アプリ間、web 間およびドメイン間で、パーソナライズされたエクスペリエンスをより正確に提供します。
exl-id: 09dd03bd-66d8-4d53-bda8-84fc4caadea6
source-git-commit: 7c262e5819f8e3488c5ddd5a0221d1c52c28c029
workflow-type: tm+mt
source-wordcount: '412'
ht-degree: 3%

---

# `appendIdentityToUrl`

`appendIdentityToUrl` コマンドを使用すると、ユーザー識別子をクエリ文字列として URL に追加できます。 このアクションにより、ドメイン間で訪問者の ID を持ち歩き、ドメインまたはチャネルの両方を含むデータセットに対して、重複した訪問者数を防ぐことができます。 Web SDK バージョン 2.11.0 以降で使用できます。

生成され、URL に追加されたクエリ文字列は `adobe_mc` です。 Web SDKで ECID が見つからない場合は、`/acquire` エンドポイントを呼び出して ECID を生成します。

>[!NOTE]
>
>同意が指定されていない場合、このメソッドからの URL は変更されずに返されます。 このコマンドは直ちに実行され、同意の更新を待つことはありません。

## Web SDK拡張機能を使用して URL に ID を追加 {#extension}

URL への ID の追加は、Adobe Experience Platform Data Collection タグインターフェイスのルール内のアクションとして実行されます。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL ルール]** に移動し、目的のルールを選択します。
1. [!UICONTROL &#x200B; アクション &#x200B;] で、既存のアクションを選択するか、アクションを作成します。
1. 「[!UICONTROL &#x200B; 拡張機能 &#x200B;]」ドロップダウンフィールドを **[!UICONTROL Adobe Experience Platform Web SDK]** に設定し、「[!UICONTROL &#x200B; アクションタイプ &#x200B;] を **[!UICONTROL ID を使用してリダイレクト]** に設定します。
1. 「**[!UICONTROL 変更を保持]**」をクリックして、公開ワークフローを実行します。

このコマンドは、通常、クリックをリッスンして目的のドメインを確認する特定のルールと共に使用されます。

+++ルールイベントの条件

`href` プロパティを持つアンカータグがクリックされたときのトリガー。

* **[!UICONTROL 拡張機能]**：コア
* **[!UICONTROL イベントタイプ]**：クリックします
* **[!UICONTROL ユーザーがクリックしたとき]**：特定の要素
* **[!UICONTROL CSS セレクターに一致する要素]**:`a[href]`

![ ルールイベント ](../assets/id-sharing-event-configuration.png)

+++

+++ルール条件

目的のドメインのみでトリガーします。

* **[!UICONTROL 論理タイプ]**：標準
* **[!UICONTROL 拡張機能]**：コア
* **[!UICONTROL 条件タイプ]**：値の比較
* **[!UICONTROL 左オペランド]**: `%this.hostname%`
* **[!UICONTROL 演算子]**：正規表現に一致します
* **[!UICONTROL 右オペランド]**：目的のドメインに一致する正規表現。 例：`adobe.com$|behance.com$`

![ ルールの条件 ](../assets/id-sharing-condition-configuration.png)

+++

+++ルールアクション

URL に ID を追加します。

* **[!UICONTROL 拡張機能]**：Adobe Experience Platform Web SDK
* **[!UICONTROL アクションタイプ]**:ID を使用したリダイレクト

![ ルールアクション ](../assets/id-sharing-action-configuration.png)

+++

## Web SDK JavaScript ライブラリを使用して URL に ID を追加

URL をパラメーターとして使用して `appendIdentityToUrl` コマンドを実行します。 メソッドは、識別子がクエリ文字列として追加された URL を返します。

```js
alloy("appendIdentityToUrl",
  {
    url: document.location.href
  }
);
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

このコマンドを使用して [ 応答を処理 ](command-responses.md) する場合、応答オブジェクトには、ID 情報がクエリ文字列パラメーターとして追加された **`url`** という新しい URL が含まれます。
