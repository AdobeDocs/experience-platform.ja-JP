---
title: Personalization設定
description: Web SDK タグ拡張機能でパーソナライゼーションを設定します。
source-git-commit: 9f4ce2a3a8af72342683c859caa270662b161b7d
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 1%

---

# Personalization設定

この設定セクションを使用すると、パーソナライズされたコンテンツの読み込み中にページの特定の部分を非表示にする方法を決定できます。 正しく設定すると、これらの設定によって、訪問者に適切なパーソナライズされたコンテンツが表示されます。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Extensions]** に移動し、**[!UICONTROL Configure]** カードで [!UICONTROL Adobe Experience Platform Web SDK] を選択します。
1. **[!UICONTROL Personalization]** セクションまで下にスクロールします。

![&#x200B; タグ UI の web SDK タグ拡張機能のパーソナライゼーション設定を示す画像 &#x200B;](../assets/web-sdk-ext-personalization.png)

次のオプションがあります。

## [!UICONTROL Migrate Target from at.js to the Web SDK]**

このオプションを使用すると、`mbox` 1.x または 2.x ライブラリで使用される従来の `mboxEdgeCluster` および `at.js` Cookie の読み取りと書き込みを Web SDKで許可できます。 この設定は、同じ web サイト上で Web SDKまたは `at.js` を使用してページ間を移動する際に、訪問者のプロファイルをそのままの状態に保つのに役立ちます。 サイトのどこにも `at.js` を実装していない場合は、このチェックボックスを有効にする必要はありません。 このチェックボックスに相当するJavaScript ライブラリは [`targetMigrationEnabled`](/help/collection/js/commands/configure/targetmigrationenabled.md) です。

## [!UICONTROL Prehiding style] {#prehiding-style}

事前非表示のスタイルエディターでは、ページの特定のセクションを非表示にするカスタム CSS ルールを定義できます。 ページが読み込まれると、Web SDKはこのスタイルを使用して、パーソナライズの必要なセクションを非表示にし、パーソナライゼーションを取得した後、パーソナライズされたページセクションの非表示を解除します。 このワークフローにより、パーソナライゼーション取得プロセスの読み込みやちらつきを確認せずに、パーソナライズされたコンテンツを訪問者に表示できます。 このコードエディターと同等のJavaScript ライブラリは [`prehidingStyle`](/help/collection/js/commands/configure/prehidingstyle.md) です。

## [!UICONTROL Prehiding snippet] {#prehiding-snippet}

この節は、直接の設定ではなく、実装コードを取得できる便利な場所です。 この事前非表示スニペットをサイトの `<head>` タグ内に実装して、Web SDK ライブラリの読み込み中に目的のコンテンツを非表示にします。

>[!IMPORTANT]
>
>事前非表示スニペットを使用する場合、Adobeでは、事前非表示スニペットと事前非表示スタイルの間に同じ CSS ルールを使用することをお勧めします。

## [!UICONTROL Enable personalization storage]

Web SDKにパーソナライゼーションイベントを保存するためのチェックボックス。 ブラウザーのローカルストレージ内。 ページの読み込み全体を通して訪問者が閲覧したエクスペリエンスを追跡する際に役立ちます。

## [!UICONTROL Auto click collection for Adobe Journey Optimizer]

Adobe Journey Optimizerから返されたコンテンツに対するクリックを web SDKが自動的に収集するタイミングを指定するドロップダウンメニュー。

* **[!UICONTROL Always]**：提案とのインタラクションをすべて収集します。
* **[!UICONTROL Decorated elements only]**:`data-aep-click-label` または `data-aep-click-token` HTML属性を持つ要素に対するインタラクションのみを収集します。
* **[!UICONTROL Never]**：提案とのインタラクションを収集しません。

このドロップダウンメニューに相当するJavaScript ライブラリは [`autoCollectPropositionInteractions.AJO`](/help/collection/js/commands/configure/autocollectpropositioninteractions.md) です。 デフォルト値は [!UICONTROL Always] です。

## [!UICONTROL Auto click collection for Adobe Target]

Adobe Targetから返されたコンテンツに対するクリックを web SDKが自動的に収集するタイミングを指定するドロップダウンメニュー。

* **[!UICONTROL Always]**：提案とのインタラクションをすべて収集します。
* **[!UICONTROL Decorated elements only]**:`data-aep-click-label` または `data-aep-click-token` HTML属性を持つ要素に対するインタラクションのみを収集します。
* **[!UICONTROL Never]**：提案とのインタラクションを収集しません。

このドロップダウンメニューに相当するJavaScript ライブラリは [`autoCollectPropositionInteractions.TGT`](/help/collection/js/commands/configure/autocollectpropositioninteractions.md) です。 デフォルト値は [!UICONTROL Never] です。
