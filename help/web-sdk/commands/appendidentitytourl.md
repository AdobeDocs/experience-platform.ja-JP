---
title: appendIdentityToUrl
description: アプリ、Web、複数のドメイン間で、より正確にパーソナライズされたエクスペリエンスを提供します。
source-git-commit: f75dcfc945be2f45c1638bdd4d670288aef6e1e6
workflow-type: tm+mt
source-wordcount: '412'
ht-degree: 0%

---

# `appendIdentityToUrl`

The `appendIdentityToUrl` コマンドを使用すると、ユーザー識別子をクエリ文字列として URL に追加できます。 このアクションを使用すると、ドメイン間で訪問者の ID を保持し、ドメインやチャネルの両方を含むデータセットで訪問者が重複してカウントされるのを防ぐことができます。 Web SDK バージョン2.11.0以降で利用できます。

生成され、URL に追加されるクエリー文字列は `adobe_mc`. Web SDK が ECID を見つけられない場合は、 `/acquire` エンドポイントを使用して生成します。

>[!NOTE]
>
>同意が提供されていない場合、このメソッドの URL は変更されずに返されます。 このコマンドは、すぐに実行され、同意の更新を待つ必要はありません。

## Web SDK 拡張機能を使用して URL に ID を追加する

ID の URL への追加は、 Adobe Experience Platform Data Collection Tags インターフェイスのルール内でアクションとして実行されます。

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL ルール]**&#x200B;を選択し、目的のルールを選択します。
1. の下 [!UICONTROL アクション]、既存のアクションを選択するか、アクションを作成します。
1. を設定します。 [!UICONTROL 拡張] ドロップダウンフィールド **[!UICONTROL Adobe Experience Platform Web SDK]**&#x200B;をクリックし、 [!UICONTROL アクションタイプ] から **[!UICONTROL ID でリダイレクト]**.
1. クリック **[!UICONTROL 変更を保持]**&#x200B;を開き、パブリッシュワークフローを実行します。

このコマンドは、通常、クリックをリッスンして目的のドメインを確認する特定のルールで使用します。

+++ルールイベント条件

トリガー: `href` プロパティがクリックされたとき。

* **[!UICONTROL 拡張]**：コア
* **[!UICONTROL イベントタイプ]**：クリック
* **[!UICONTROL ユーザーが]**：特定の要素
* **[!UICONTROL CSS セレクターに一致する要素]**: `a[href]`

![ルールイベント](../assets/id-sharing-event-configuration.png)

+++

+++ルール条件

トリガーは、目的のドメインでのみ使用できます。

* **[!UICONTROL 論理タイプ]**：標準
* **[!UICONTROL 拡張]**：コア
* **[!UICONTROL 条件タイプ]**：値の比較
* **[!UICONTROL 左オペランド]**: `%this.hostname%`
* **[!UICONTROL 演算子]**: Matches Regex
* **[!UICONTROL 右オペランド]**：目的のドメインに一致する正規表現。 例：`adobe.com$|behance.com$`

![ルール条件](../assets/id-sharing-condition-configuration.png)

+++

+++ルールアクション

ID を URL に追加します。

* **[!UICONTROL 拡張]**: Adobe Experience Platform Web SDK
* **[!UICONTROL アクションタイプ]**:ID でリダイレクト

![ルールアクション](../assets/id-sharing-action-configuration.png)

+++

## Web SDK JavaScript ライブラリを使用して URL に ID を追加する

を実行します。 `appendIdentityToUrl` コマンドの先頭に URL を指定します。 このメソッドは、識別子が追加された URL をクエリ文字列として返します。

```js
alloy("appendIdentityToUrl",document.location);
```

ページで受信したすべてのクリックに対してイベントリスナーを追加し、その URL が目的のドメインと一致するかどうかを確認できます。 存在する場合は、ID を URL に追加し、ユーザーをリダイレクトします。

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

もしあなたが [応答を処理する](command-responses.md) このコマンドを使用すると、応答オブジェクトには次の値が含まれます。 **`url`**:id 情報がクエリー文字列パラメーターとして追加された新しい URL。
