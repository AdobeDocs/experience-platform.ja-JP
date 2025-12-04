---
title: モバイルから web、およびクロスドメインでの ID の共有
description: モバイルから web プロパティ、およびドメインをまたいで訪問者 ID を永続化する方法を説明します
keywords: ID；モバイル；ID；共有；ドメイン；クロスドメイン；SDK;Platform;
exl-id: b9bb236f-52cf-4615-96d8-1137d957de8c
source-git-commit: 217282135bcd750740f4d3f8c6e17a0b8f9578bd
workflow-type: tm+mt
source-wordcount: '460'
ht-degree: 3%

---

# モバイルから web、およびクロスドメインでの ID の共有

## 概要

Adobe Experience Platform Web SDKは、訪問者 ID 共有機能をサポートし、モバイルアプリとモバイル web コンテンツの間、およびドメイン間で、より正確にパーソナライズされたエクスペリエンスを提供できるようにします。

## ユースケース {#use-cases}

### モバイルアプリとモバイル web サイト間で一貫性のあるパーソナライゼーションを提供します

ある衣料品会社は、興味に基づいて顧客のエクスペリエンスをパーソナライズし、Web ビューも読み込むモバイルアプリケーションでパーソナライゼーションを正確に保ちたいと考えています。 モバイルから web への ID 共有機能を使用すると、アプリとモバイル web コンテンツで同じ訪問者識別子を使用して、[!DNL ECID] をモバイル web URL に渡すことで、最も正確なオファーを顧客に確実に提示できます。

### ドメイン間で一貫性のあるパーソナライゼーションを提供

複数のオンラインストアを持つretailerが、顧客の関心に基づいて、ドメイン全体で買い物客のエクスペリエンスをパーソナライズしたいと考えています。 Web SDKのクロスドメイン ID 共有機能を使用すると、retailerは、顧客の関心に基づいた正確なオファーをすべてのドメインにわたって提供できます。

### 訪問者アクティビティレポートの強化

あるテクノロジーretailerは、訪問者がモバイルアプリケーションからモバイル web サイト、または他のドメインに移動したタイミングに関する情報を使用して、訪問者アクティビティレポートを改善したいと考えています。 Web SDKのクロスドメイン ID 共有機能を使用すると、マーケティングチームは、web プロパティをまたいで訪問者を正確に追跡し、アクティビティレポートを生成できます。

## 前提条件 {#prerequisites}

モバイルから web およびクロスドメインでの ID 共有を使用するには、[!DNL Web SDK] バージョン 2.11.0 以降を使用する必要があります。

Edge Network モバイル実装の場合、この機能は、バージョン 1.1.0 以降の [Edge Networkの ID](https://developer.adobe.com/client-sdks/documentation/identity-for-edge-network/) 拡張機能（iOSおよびAndroid）でサポートされます。

この機能は、[!DNL VisitorAPI.js] バージョン 1.7.0 以降とも互換性があります。

## モバイルから web への ID の共有 {#mobile-to-web}

`getUrlVariables`Edge Networkの ID[&#x200B; 拡張機能から &#x200B;](https://developer.adobe.com/client-sdks/documentation/identity-for-edge-network/api-reference/#geturlvariables) API を使用して、識別子をクエリパラメーターとして取得し、[!DNL webViews] を開く際に URL に添付します。

Web SDKがクエリ文字列内の `ECID` 値を受け入れるために、追加の設定は必要ありません。

クエリ文字列パラメーターには、次のものが含まれます。

* `MCID`:Experience Cloud ID （`ECID`）
* `MCORGID`:`orgID` で設定された `orgID` に一致する必要があるExperience Cloud [!DNL Web SDK]。
* `TS`: 5 分未満のタイムスタンプパラメーター。


モバイルから web への ID 共有では、`adobe_mc` パラメーターを使用します。 `adobe_mc` パラメーターが存在し、有効な場合、Edge Networkに対して行われた最初のリクエストで、クエリ文字列からの `ECID` が ID マップに自動的に追加されます。 それ以降のEdge Network インタラクションでは、すべてその `ECID` が使用されます。

モバイルアプリから WebView に訪問者 ID を渡す方法について詳しくは、[WebView の処理 &#x200B;](https://experienceleague.adobe.com/docs/platform-learn/implement-mobile-sdk/app-implementation/web-views.html?lang=ja#implementation) に関するドキュメントを参照してください。

## クロスドメイン ID 共有の実装 {#cross-domain-sharing}

Web SDKの設定方法に応じて、次のリンクを参照してください。

* **JavaScript ライブラリ**: [`appendIdentityToUrl`](../../js/commands/appendidentitytourl.md) コマンド
* **タグ拡張機能**:[ID でリダイレクト &#x200B;](/help/tags/extensions/client/web-sdk/actions/redirect-with-identity.md) アクション
