---
title: モバイルから Web への ID とクロスドメイン ID の共有
description: モバイルから Web プロパティ、およびドメイン間で訪問者 ID を保持する方法を説明します。
keywords: ID；モバイル；ID；共有；ドメイン；クロスドメイン；sdk；プラットフォーム；
source-git-commit: 55e28f749741c653a230b42fabf5a047ba8c7d01
workflow-type: tm+mt
source-wordcount: '893'
ht-degree: 1%

---


# モバイルから Web への ID とクロスドメイン ID の共有

## 概要

Adobe Experience Platform Web SDK は、訪問者 ID 共有機能をサポートしています。この機能により、モバイルアプリとモバイル Web コンテンツの間、およびドメイン間で、顧客がより正確にパーソナライズされたエクスペリエンスを配信できます。

## ユースケース {#use-cases}

### モバイルアプリとモバイル Web サイト間で一貫したパーソナライゼーションを実現

衣料品会社は、興味に基づいて顧客のエクスペリエンスをパーソナライズし、WebViews を読み込むモバイルアプリケーションでパーソナライゼーションを正確に保ちたいと考えています。 モバイル/Web 間 ID 共有機能を使用すると、アプリとモバイル Web コンテンツで同じ訪問者 ID を使用して、最も正確なオファーが顧客に提示されるようになります。その際に、 [!DNL ECID] をモバイル web URL に追加します。

### ドメイン間で一貫したパーソナライゼーションを実現

複数のオンラインストアを持つ小売業者は、顧客の興味に基づいて、ドメインをまたいで買い物客のエクスペリエンスをパーソナライズしたいと考えています。 Web SDK のクロスドメイン ID 共有機能を使用すると、小売業者は、すべてのドメインにわたって、顧客の興味に基づいた正確なオファーを配信できます。

### 訪問者のアクティビティレポートの強化

ある技術小売業者は、訪問者のアクティビティレポートを、訪問者がモバイルアプリケーションからモバイル Web サイトまたは他のドメインに移動するタイミングに関する情報で改善したいと考えています。 Web SDK のクロスドメイン ID 共有機能を使用すると、マーケティングチームは Web プロパティをまたいで訪問者を正確に追跡し、アクティビティレポートを生成できます。

## 前提条件 {#prerequisites}

モバイルから Web への ID とクロスドメイン ID の共有を使用するには、を更新する必要があります。 [!DNL Web SDK] バージョン2.11.0以降。

Edge Network モバイル実装の場合、この機能は [Edge ネットワークの ID](https://aep-sdks.gitbook.io/docs/foundation-extensions/identity-for-edge-network) バージョン 1.1.0(iOSおよび Android) 以降の拡張機能。

## モバイルから Web への ID の共有 {#mobile-to-web}

以下を使用： `getUrlVariables` からの API [Edge ネットワークの ID](https://aep-sdks.gitbook.io/docs/foundation-extensions/identity-for-edge-network/api-reference#geturlvariables) 拡張機能を使用して、識別子をクエリパラメーターとして取得し、を開いたときに URL に関連付けることができます。 [!DNL webViews].

Web SDK が受け入れるための追加設定は不要です `ECID` クエリー文字列の値。

クエリー文字列パラメーターには次が含まれます。

* `MCID`:Experience CloudID (`ECID`)
* `MCORGID`:Experience Cloud `orgID` それは `orgID` 設定済み [!DNL Web SDK].
* `TS`:5 分を超えないタイムスタンプパラメーター。


モバイルから Web への ID の共有では、 `adobe_mc` パラメーター。 次の場合に `adobe_mc` パラメーターが存在し、有効な場合は、 `ECID` からのを呼び出すと、Edge ネットワークに対する最初のリクエストで id マップに自動的に追加されます。 以降のすべての Edge ネットワークインタラクションでは、 `ECID`.

モバイルアプリから WebView に訪問者 ID を渡す方法について詳しくは、 [WebViews の処理](https://experienceleague.adobe.com/docs/platform-learn/implement-mobile-sdk/app-implementation/web-views.html#implementation).

## クロスドメイン ID 共有 {#cross-domain-sharing}

クロスドメイン ID 共有の場合、Web SDK バージョン2.11.0では、 `appendIdentityToUrl` コマンドを使用します。 このコマンドを使用すると、 `adobe_mc` クエリー文字列パラメーター。

このコマンドは、1 つのプロパティを持つオブジェクトを受け入れます。 `url`を返し、 `url`.

このコマンドは、同意の更新を待ちません。 同意が提供されていない場合、URL は変更されずに返されます。

次の場合、 `ECID` が指定されていない場合、 `/acquire` エンドポイントが呼び出され、 `ECID`.

顧客が Web サイトでクロスドメイン ID 共有を実装する方法の例を以下に示します。

このコードは、ページ上のすべてのクリックに対してイベントリスナーを追加します。クリックが対応するドメインへのリンク ( この場合は `adobe.com` または `behance.com`) をクリックすると、ID が URL に追加され、そこにユーザーをリダイレクトします。

```js
document.addEventListener("click", event => {
  const anchor = event.target.closest("a");
  if (!anchor || !anchor.href) {
    return;
  }
  const url = new URL(anchor.href);
  if (!url.hostname.endsWith("adobe.com") && !url.hostname.endsWith("behance.com")) {
    return;
  }
  event.preventDefault();
  alloy("appendIdentityToUrl", { url: anchor.href }).then(result => {
    document.location = result.url;
  });
});
```

## タグ拡張機能の使用 {#tags-extension}

次の [!DNL Web SDK]、 [!DNL Tags] 拡張機能を使用して、URL を介して渡された id を使用できます。

タグ拡張機能でモバイルから Web への ID とクロスドメイン ID の共有を使用するには、タグ拡張機能のバージョン2.12.0以降を使用する必要があります。

現在のページの ID を他のドメインと共有するには、新しいアクションを「 [!DNL Web SDK] [!DNL Tags] 拡張子。 このアクションは、 **[!UICONTROL コア — クリック]** イベントタイプと値比較条件。

以下の手順に従います。 [ここ](../../tags/ui/managing-resources/rules.md) 次の設定を使用してルールを作成するには：

* [!UICONTROL イベント設定]:
   * **[!UICONTROL 拡張機能：Core]**
   * **[!UICONTROL イベントタイプ：クリック]**
   * 選択 **[!UICONTROL ユーザーが/特定の要素をクリックしたとき]**
   * を **[!UICONTROL セレクター]**: `a[href]`. このイベントは、 `href` プロパティ。

      ![上記の設定を使用したイベント設定を示す UI 画像](assets/id-sharing-event-configuration.png)

* [!UICONTROL 条件の設定]
   * **[!UICONTROL 論理タイプ]**: [!UICONTROL 標準]
   * **[!UICONTROL 拡張機能]**：[!UICONTROL コア]
   * **[!UICONTROL 条件タイプ]**: [!UICONTROL 値の比較]
   * **[!UICONTROL 左オペランド]**: [!UICONTROL `%this.hostname%`]. これは、 [!UICONTROL コア — クリック] イベントを送信し、クリックされたリンクのホスト名に解決します。
   * **[!UICONTROL 演算子]**: [!UICONTROL Matches Regex]
   * **[!UICONTROL 右オペランド]**:ID を共有するドメインに一致する正規表現を入力します。 例えば、で終わるホスト名を持つリンクを照合するには、次のようにします。 `adobe.com` または `behance.com`、次の正規表現を使用します。 `behance.com$|adobe.com$`. リンクされたページには、 [!DNL Web SDK] または [!DNL Visitor ID] id を受け入れるためにインストールされました。

      ![上記の設定を使用した条件設定を示す UI 画像](assets/id-sharing-condition-configuration.png)

* [!UICONTROL アクションの設定]
   * **[!UICONTROL 拡張]**: [!UICONTROL Adobe Experience Platform Web SDK]
   * **[!UICONTROL アクションタイプ]**: [!UICONTROL ID でリダイレクト]
   * **[!UICONTROL インスタンス]**:インスタンスを選択します。 ほとんどの場合、1 つのインスタンスのみが設定されます。 複数のインスタンスがある場合、共有する ID を持つインスタンスを選択します。

      ![上記の設定を使用したアクション設定を示す UI 画像](assets/id-sharing-action-configuration.png)

この **[!UICONTROL ID でリダイレクト]** 「 」アクションを実行すると、ブラウザーがリンクに移動するのを停止します。 次に、 `appendIdentityToUrl` メソッド [!DNL Web SDK] インスタンス。

最後に、ユーザーを [!DNL URL] と `adobe_mc` クエリー文字列パラメーターを追加しました。
