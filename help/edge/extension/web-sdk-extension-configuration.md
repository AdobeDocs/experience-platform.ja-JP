---
title: Adobe Experience Platform Web SDKの設定
description: Adobe Experience Platform Launch向けAdobe Experience Platform Web SDK拡張機能について説明します。
exl-id: 96d32db8-0c9a-49f0-91f3-0244522d66df
source-git-commit: e5b43a65bb9907f333f1ffedfbeed00f02f8ef39
workflow-type: tm+mt
source-wordcount: '1043'
ht-degree: 8%

---

# 概要

Adobe Experience Platform Web SDK拡張機能は、Adobe Experience Platform Edge Networkを通じて、WebプロパティからAdobe Experience Cloudにデータを送信します。 拡張機能を使用すると、データをPlatformにストリーミングし、IDを同期し、顧客の同意シグナルを処理して、コンテキストデータを自動的に収集できます。

このドキュメントでは、Adobe Experience Platform Launchユーザーインターフェイスで拡張機能を設定する方法について説明します。

## 拡張機能の設定

Platform Web SDK拡張機能が既にプロパティ用にインストールされている場合は、Platform launchUIでプロパティを開き、「**[!UICONTROL 拡張機能]**」タブを選択します。 Platform Web SDKで、「**[!UICONTROL 設定]**」を選択します。

![](../images/extension/overview/configure.png)

拡張機能をまだインストールしていない場合は、「**[!UICONTROL カタログ]**」タブを選択します。 使用可能な拡張機能のリストから、Platform Web SDK拡張機能を探し、「**[!UICONTROL インストール]**」を選択します。

![](../images/extension/overview/install.png)

どちらの場合も、Platform Web SDKの設定ページが表示されます。 以下の節では、拡張機能の設定オプションについて説明します。

![](../images/extension/overview/config-screen.png)

## 一般的な設定オプション

ページ上部の設定オプションは、Adobe Experience Platformに対し、データのルーティング先と、サーバー上で使用する設定を伝えます。

### [!UICONTROL 名前]

Adobe Experience Platform Web SDK拡張機能は、ページ上の複数のインスタンスをサポートします。 この名前は、1つの組織設定で複数の組織にデータをPlatform launchする場合に使用します。

拡張機能の名前は、デフォルトで「[!DNL alloy]」に設定されます。 ただし、インスタンス名は任意の有効な JavaScript オブジェクト名に変更できます。

### **[!UICONTROL IMS 組織 ID]**

[!UICONTROL IMS組織ID]は、Adobe時にデータの送信先となる組織です。 ほとんどの場合、自動入力されるデフォルト値を使用します。 ページに複数のインスタンスが存在する場合は、データの送信先となる2つ目の組織の値をこのフィールドに入力します。

### **[!UICONTROL エッジドメイン]**

[!UICONTROL Edge Domain]は、Adobe Experience Platform拡張機能がデータの送受信をおこなうドメインです。 拡張機能では、実稼動用トラフィックにファーストパーティの CNAME を使用する必要があります。デフォルトのサードパーティドメインは開発環境で使用できますが、実稼動環境には適していません。ファーストパーティ CNAME の設定方法については、[ここ](https://experienceleague.adobe.com/docs/core-services/interface/ec-cookies/cookies-first-party.html?lang=ja)で説明します。

## [!UICONTROL データストリーム]

リクエストがAdobe Experience Platform Edgeネットワークに送信されると、データストリームIDがサーバー側の設定の参照に使用されます。 Webサイト上でコードを変更することなく、設定を更新できます。

詳しくは、[datastreams](../fundamentals/datastreams.md)のガイドを参照してください。


## [!UICONTROL プライバシー]

![](../images/extension/overview/privacy.png)

「[!UICONTROL プライバシー]」セクションでは、SDKがWebサイトからのユーザーの同意シグナルを処理する方法を設定できます。 特に、他の明示的な同意設定が指定されていない場合に、ユーザーが想定するデフォルトの同意レベルを選択できます。 デフォルトの同意レベルは、ユーザーのプロファイルに保存されません。 次の表に、各オプションの内容を示します。

| [!UICONTROL デフォルトの同意レベル] | 説明 |
| --- | --- |
| [!UICONTROL In] | ユーザーが同意設定を提供する前に発生したイベントを収集します。 |
| [!UICONTROL Out] | ユーザーが同意設定を提供する前に発生したイベントを破棄する。 |
| [!UICONTROL 保留中] | ユーザーが同意設定を提供する前に発生したイベントをキューに入れる。 同意の環境設定を指定すると、指定した環境設定に応じてイベントが収集または破棄されます。 |
| [!UICONTROL データ要素で指定] | デフォルトの同意レベルは、定義する個別のデータ要素によって決定されます。 このオプションを使用する場合は、提供されたドロップダウンメニューを使用してデータ要素を指定する必要があります。 |

業務上の明示的なユーザーの同意が必要な場合は、「Out」または「Pending」を使用します。

## [!UICONTROL ID]

![](../images/extension/overview/identity.png)

### [!UICONTROL VisitorAPIからのECIDの移行]

このオプションは、デフォルトでは有効になっています。この機能が有効な場合、SDKはAMCV Cookieとs_ecid Cookieを読み取り、Visitor.jsで使用されるAMCV Cookieを設定できます。 一部のページでは引き続きVisitor.jsを使用している可能性があるので、この機能はAdobe Experience Platform Web SDKに移行する際に重要です。 これにより、SDKは引き続き同じECIDを使用できるので、ユーザーが2人の異なるユーザーとして識別されることはありません。

### [!UICONTROL サードパーティCookieを使用する]

このオプションを選択すると、SDKはユーザーIDをサードパーティCookieに保存しようとします。 成功した場合、ユーザーは、各ドメインで個別のユーザーとして識別されるのではなく、複数のドメインをまたいで移動する単一のユーザーとして識別されます。 このオプションが有効になっている場合、ブラウザーがサードパーティCookieをサポートしていない場合や、ユーザーがサードパーティCookieを許可しないように設定している場合、SDKは引き続きユーザーIDをサードパーティCookieに保存できない可能性があります。 この場合、SDKは識別子をファーストパーティドメインにのみ保存します。

## [!UICONTROL パーソナライズ機能]

![](../images/extension/overview/personalization.png)

パーソナライズされたコンテンツが読み込まれる際にサイトの特定の部分を非表示にする場合は、事前非表示のスタイルエディターで非表示にする要素を指定できます。 次に、提供されたデフォルトの事前非表示スニペットをコピーし、HTMLサイトの`<head>`要素内に貼り付けます。

## [!UICONTROL データ収集]

![](../images/extension/overview/data-collection.png)

### [!UICONTROL コールバック関数]

拡張機能で提供されるコールバック関数は、ライブラリ内では[`onBeforeEventSend`関数](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html?lang=en)とも呼ばれます。 この関数を使用すると、イベントをAdobe Edge Networkに送信する前にグローバルに変更できます。 この関数の使い方の詳細は、[こちら](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/tracking-events.html?lang=en#modifying-events-globally)を参照してください。

### [!UICONTROL クリックデータ収集]

SDKは、リンククリック情報を自動的に収集できます。 デフォルトでは、この機能は有効になっていますが、このオプションを使用して無効にできます。 また、「[!UICONTROL ダウンロードリンク修飾子]」テキストボックスにリストされたダウンロード式の1つが含まれる場合、リンクにダウンロードリンクというラベルが付けられます。 Adobeには、デフォルトのダウンロードリンク修飾子がいくつか用意されていますが、いつでも編集できます。

### [!UICONTROL 自動的に収集されたコンテキストデータ]

デフォルトでは、SDKは、デバイス、Web、環境、場所コンテキストに関する特定のコンテキストデータを収集します。 収集した情報Adobeのリストを表示したい場合は、[ここ](https://experienceleague.adobe.com/docs/experience-platform/edge/data-collection/automatic-information.html?lang=en)にあります。 このデータを収集しない場合や、特定のカテゴリのデータのみを収集する場合は、これらのオプションを変更できます。

## [!UICONTROL 詳細設定]

![](../images/extension/overview/advanced-settings.png)

### [!UICONTROL エッジの基本パス]

Adobe Edge Networkとのやり取りに使用するベースパスを変更する必要がある場合は、このフィールドを使用します。 これは更新する必要はありませんが、ベータ版またはアルファ版に参加している場合は、Adobeからこのフィールドを変更するように求められる場合があります。
