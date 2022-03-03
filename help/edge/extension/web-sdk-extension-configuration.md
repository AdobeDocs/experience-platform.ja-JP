---
title: Adobe Experience Platform Web SDK 拡張機能の設定
description: データ収集 UI でのAdobe Experience Platform Web SDK タグ拡張の設定方法。
exl-id: 96d32db8-0c9a-49f0-91f3-0244522d66df
source-git-commit: 1f9454148ed4ee95f0d86f03c4bcf8c917d0aeea
workflow-type: tm+mt
source-wordcount: '1047'
ht-degree: 6%

---

# Adobe Experience Platform Web SDK 拡張機能の設定

Adobe Experience Platform Web SDK タグ拡張機能は、Adobe Experience Platform Edge Network を通じて、Web プロパティからAdobe Experience Cloudにデータを送信します。 拡張機能を使用すると、データを Platform にストリーミングし、ID を同期し、顧客の同意シグナルを処理して、コンテキストデータを自動的に収集できます。

このドキュメントでは、データ収集 UI で拡張機能を設定する方法について説明します。

## はじめに

Platform Web SDK 拡張機能が既にプロパティ用にインストールされている場合は、データ収集 UI でプロパティを開き、 **[!UICONTROL 拡張機能]** タブをクリックします。 Platform Web SDK で、を選択します。 **[!UICONTROL 設定]**.

![](../images/extension/overview/configure.png)

拡張機能をまだインストールしていない場合は、「 **[!UICONTROL カタログ]** タブをクリックします。 使用可能な拡張機能のリストから、Platform Web SDK 拡張機能を探し、「 」を選択します。 **[!UICONTROL インストール]**.

![](../images/extension/overview/install.png)

どちらの場合も、Platform Web SDK の設定ページに到達します。 以下の節では、拡張機能の設定オプションについて説明します。

![](../images/extension/overview/config-screen.png)

## 一般的な設定オプション

ページ上部の設定オプションは、データのルーティング先と、サーバー上で使用する設定をAdobe Experience Platformに通知します。

### [!UICONTROL 名前]

Adobe Experience Platform Web SDK 拡張機能は、ページ上の複数のインスタンスをサポートします。 この名前は、タグ設定を使用して複数の組織にデータを送信するために使用されます。

拡張機能の名前のデフォルト値は「 」です。[!DNL alloy]&quot;. ただし、インスタンス名は任意の有効な JavaScript オブジェクト名に変更できます。

### **[!UICONTROL IMS 組織 ID]**

この [!UICONTROL IMS 組織 ID] は、Adobe時にデータの送信先となる組織です。 ほとんどの場合、自動入力されるデフォルト値を使用します。 ページに複数のインスタンスが存在する場合は、データの送信先となる 2 つ目の組織の値をこのフィールドに入力します。

### **[!UICONTROL エッジドメイン]**

この [!UICONTROL Edge ドメイン] は、Adobe Experience Platform拡張機能がデータの送受信をおこなうドメインです。 Adobeでは、この拡張機能にファーストパーティドメイン (CNAME) を使用することをお勧めします。 デフォルトのサードパーティドメインは開発環境で使用できますが、実稼動環境には適していません。ファーストパーティ CNAME の設定方法については、[ここ](https://experienceleague.adobe.com/docs/core-services/interface/ec-cookies/cookies-first-party.html?lang=ja)で説明します。

## [!UICONTROL データストリーム]

リクエストがAdobe Experience Platform Edge ネットワークに送信されると、データストリーム ID がサーバー側設定の参照に使用されます。 Web サイト上でコードを変更しなくても、設定を更新できます。

詳しくは、 [datastreams](../fundamentals/datastreams.md) を参照してください。


## [!UICONTROL プライバシー]

![](../images/extension/overview/privacy.png)

この [!UICONTROL プライバシー] 「 」セクションでは、SDK による Web サイトからのユーザー同意シグナルの処理方法を設定できます。 特に、他の明示的な同意の環境設定が指定されていない場合に、ユーザーが想定するデフォルトの同意レベルを選択できます。 デフォルトの同意レベルは、ユーザーのプロファイルに保存されません。 次の表に、各オプションの詳細を示します。

| [!UICONTROL デフォルトの同意レベル] | 説明 |
| --- | --- |
| [!UICONTROL In] | ユーザーが同意設定を提供する前に発生したイベントを収集します。 |
| [!UICONTROL 出力] | ユーザーが同意設定を提供する前に発生したイベントを破棄します。 |
| [!UICONTROL 保留中] | ユーザーが同意設定を提供する前に発生したイベントをキューに入れます。 同意の環境設定が指定されると、指定された環境設定に応じてイベントが収集または破棄されます。 |
| [!UICONTROL データ要素によって指定] | デフォルトの同意レベルは、定義する別のデータ要素で決定します。 このオプションを使用する場合は、提供されたドロップダウンメニューを使用してデータ要素を指定する必要があります。 |

ビジネス運営に対する明示的なユーザー同意が必要な場合は、「送信」または「保留」を使用します。

## [!UICONTROL ID]

![](../images/extension/overview/identity.png)

### [!UICONTROL VisitorAPI から ECID を移行する]

このオプションは、デフォルトでは有効になっています。この機能が有効な場合、SDK は AMCV Cookie と s_ecid Cookie を読み取り、Visitor.js で使用される AMCV Cookie を設定できます。 一部のページでは引き続き Visitor.js を使用している可能性があるので、この機能はAdobe Experience Platform Web SDK に移行する際に重要です。 これにより、SDK は引き続き同じ ECID を使用し、ユーザーが 2 人の異なるユーザーとして識別されないようにすることができます。

### [!UICONTROL サードパーティ Cookie の使用]

このオプションを使用すると、SDK は、サードパーティ Cookie にユーザー識別子を保存しようとします。 成功した場合、ユーザーは、各ドメインで個別のユーザーとして識別されるのではなく、複数のドメインをまたいで移動する際に、単一のユーザーとして識別されます。 このオプションが有効になっている場合、ブラウザーがサードパーティ Cookie をサポートしていない場合や、ユーザーがサードパーティ Cookie を許可しないように設定している場合、SDK は引き続き、ユーザー ID をサードパーティ Cookie に保存できません。 この場合、SDK は識別子をファーストパーティドメインにのみ保存します。

## [!UICONTROL パーソナライゼーション]

![](../images/extension/overview/personalization.png)

パーソナライズされたコンテンツが読み込まれる際に、サイトで特定のパーツを非表示にする場合は、事前非表示のスタイルエディターで非表示にする要素を指定できます。 次に、提供されたデフォルトの事前非表示スニペットをコピーし、 `<head>`HTMLサイトの要素。

## [!UICONTROL データ収集]

![](../images/extension/overview/data-collection.png)

### [!UICONTROL コールバック関数]

拡張機能で提供されるコールバック関数は、 [`onBeforeEventSend` 関数](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html?lang=en) ライブラリ内に保存されます。 この関数を使用すると、イベントがAdobe Edge Network に送信される前に、イベントをグローバルに変更できます。 この関数の使用方法の詳細については、を参照してください。 [ここ](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/tracking-events.html?lang=en#modifying-events-globally).

### [!UICONTROL クリックデータコレクション]

SDK は、自動的にリンククリック情報を収集できます。 デフォルトでは、この機能は有効ですが、このオプションを使用して無効にできます。 また、 [!UICONTROL リンク修飾子のダウンロード] テキストボックス Adobeには、いくつかのデフォルトのダウンロードリンク修飾子が用意されていますが、これらはいつでも編集できます。

### [!UICONTROL 自動的に収集されたコンテキストデータ]

デフォルトでは、SDK は、デバイス、Web、環境、場所コンテキストに関する特定のコンテキストデータを収集します。 収集された情報Adobeの一覧を表示するには、次の URL を見つけます [ここ](https://experienceleague.adobe.com/docs/experience-platform/edge/data-collection/automatic-information.html?lang=en). このデータを収集しない場合や、特定のカテゴリのデータのみを収集する場合は、これらのオプションを変更できます。

## [!UICONTROL 詳細設定]

![](../images/extension/overview/advanced-settings.png)

### [!UICONTROL エッジの基本パス]

Adobe Edge Network とのやり取りに使用するベースパスを変更する必要がある場合は、このフィールドを使用します。 これは更新する必要はありませんが、ベータ版またはアルファ版に参加する場合は、Adobeからこのフィールドを変更するように求められる場合があります。
