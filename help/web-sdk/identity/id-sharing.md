---
title: モバイルから web、およびクロスドメインでの ID の共有
description: モバイルから Web プロパティ、およびドメイン間で訪問者 ID を保持する方法を説明します。
keywords: ID；モバイル；ID；共有；ドメイン；クロスドメイン；sdk；プラットフォーム；
exl-id: b9bb236f-52cf-4615-96d8-1137d957de8c
source-git-commit: b6e084d2beed58339191b53d0f97b93943154f7c
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 3%

---

# モバイルから web、およびクロスドメインでの ID の共有

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

モバイルから Web への ID とクロスドメインの ID の共有を使用するには、 [!DNL Web SDK] バージョン2.11.0以降。

Edge Network モバイル実装の場合、この機能は [Edge ネットワークの ID](https://developer.adobe.com/client-sdks/documentation/identity-for-edge-network/) バージョン 1.1.0(iOSおよび Android) 以降の拡張機能。

この機能は、 [!DNL VisitorAPI.js] バージョン 1.7.0 以降。

## モバイルから Web への ID の共有 {#mobile-to-web}

以下を使用します。 `getUrlVariables` からの API [Edge ネットワークの ID](https://developer.adobe.com/client-sdks/documentation/identity-for-edge-network/api-reference/#geturlvariables) 拡張機能を使用して、識別子をクエリパラメーターとして取得し、を開いたときに URL に関連付けることができます。 [!DNL webViews].

Web SDK が受け入れるための追加設定は不要です `ECID` クエリー文字列の値。

クエリー文字列パラメーターには次が含まれます。

* `MCID`:Experience CloudID (`ECID`)
* `MCORGID`:EXPERIENCE CLOUD `orgID` それは `orgID` 設定済み [!DNL Web SDK].
* `TS`：タイムスタンプパラメーター（5 分を超えることはできません）。


モバイルから Web への ID の共有では、 `adobe_mc` パラメーター。 次の場合に `adobe_mc` パラメーターが存在し、有効な場合は、 `ECID` からのを呼び出すと、Edge ネットワークに対する最初のリクエストで id マップに自動的に追加されます。 以降のすべての Edge ネットワークインタラクションでは、 `ECID`.

モバイルアプリから WebView に訪問者 ID を渡す方法について詳しくは、 [WebViews の処理](https://experienceleague.adobe.com/docs/platform-learn/implement-mobile-sdk/app-implementation/web-views.html#implementation).

## クロスドメイン ID 共有の実装 {#cross-domain-sharing}

詳しくは、 [`appendIdentityToUrl`](../commands/appendidentitytourl.md) コマンドを使用して、Web SDK タグ拡張機能と Web SDK JavaScript ライブラリの両方を使用する実装手順を確認します。
