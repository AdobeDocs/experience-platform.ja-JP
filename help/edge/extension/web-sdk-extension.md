---
title: Adobe Experience Platform Web SDK 拡張機能 概要
description: Adobe Experience Platform Launch向けAdobe Experience PlatformWeb SDK Extensionについて
translation-type: tm+mt
source-git-commit: 0b9a92f006d1ec151a0bb11c10c607ea9362f729
workflow-type: tm+mt
source-wordcount: '640'
ht-degree: 53%

---


# Adobe Experience PlatformWeb SDK拡張機能の概要

Adobe Experience PlatformWeb SDK Extensionは、Adobe Experience Platformエッジネットワークを介して、WebプロパティからAdobe Experience Cloudにデータを送信します。 Adobe Experience Platform Web SDK 拡張機能を使用すると、プラットフォームへのデータのストリーミング、ID の同期、オプトイン、およびコンテキストデータの自動収集が可能になります。

## 拡張機能の設定

この節では、Adobe Experience Platform Web SDK 拡張機能を設定する際に使用できるオプションについて説明します。

Adobe Experience PlatformWeb SDK拡張機能がまだインストールされていない場合は、プロパティを開き、**[!UICONTROL エクステンション/カタログ]**&#x200B;を選択し、Adobe Experience PlatformWeb SDK拡張機能の上にカーソルを置いて、**[!UICONTROL インストール]**&#x200B;を選択します。

拡張機能を構成するには、「**[!UICONTROL 拡張機能]**」タブを開き、拡張機能の上にマウスポインターを置いて、「**[!UICONTROL 設定]**」を選択します。

![](./assets/ext-aep-config.png)

### インスタンス名

Adobe Experience PlatformWeb SDK拡張機能は、ページ上の複数のインスタンスをサポートします。 これは、1 つの Adobe Experience Platform Launch 設定で複数の組織にデータを送信するために使用します。**[!UICONTROL Name]**&#x200B;は、既定でaloyに設定されます。 ただし、インスタンス名は任意の有効な JavaScript オブジェクト名に変更できます。Adobe Experience Platform拡張では、各インスタンスに異なる&#x200B;**[!UICONTROL Config ID]**&#x200B;と異なる&#x200B;**[!UICONTROL 組織ID]**&#x200B;を設定する必要があります。

## **[!UICONTROL Config ID]**

**[!UICONTROL Config ID]**&#x200B;は、データのルーティング先と、サーバで使用する設定をAdobe Experience Platformに指示するものです。 これは、Adobe Experience Platform 拡張機能が動作するために必要です。設定 ID は、ClientCare に問い合わせて取得できます。


### **[!UICONTROL 組織 ID]**



**[!UICONTROL 組織ID]**&#x200B;は、Adobeでのデータ送信先となる組織です。 ほとんどの場合は、自動入力されるデフォルト値を使用する必要があります。ページに複数のインスタンスが存在する場合は、データの送信先となる 2 つ目の組織の値をここに入力します。

### **[!UICONTROL エッジドメイン]**

**[!UICONTROL Edge Domain]**&#x200B;は、Adobe Experience Platform拡張がデータを送受信するドメインです。 拡張機能では、実稼動用トラフィックにファーストパーティの CNAME を使用する必要があります。デフォルトのサードパーティドメインは開発環境で使用できますが、実稼動環境には適していません。ファーストパーティ CNAME の設定方法については、[ここ](https://docs.adobe.com/content/help/ja-JP/core-services/interface/ec-cookies/cookies-first-party.html)で説明します。

### **[!UICONTROL エラーを有効にする]**

デフォルトでは、拡張機能でエラーが発生した場合、エラーはコンソールに記録されます。実稼働環境でエラーを非表示にする場合は、「**[!UICONTROL エラーを有効にする]**」チェックボックスをオフにできます。 Platform Launch でデバッグが有効になっている場合は、引き続きエラーが表示されます。

### **[!UICONTROL オプトインを有効にする]**

**[!UICONTROL オプトインを有効にする]**&#x200B;が有効な場合、拡張機能はオプトインが受信されるまでヒットを保持できます。 オプトインの環境設定をおこなうためのアクションが拡張機能に表示されます。

### **[!UICONTROL 移行ECIDの有効化]**

プラットフォームWeb SDK拡張では、新しいCookieを使用してECIDを保存します。 この設定により、移行目的で、新しい cookie と古い cookie の互換性が有効になります。ECID を持つ既存の訪問者がいない場合を除き、この機能を有効にすることを強くお勧めします。

### **[!UICONTROL サードパーティCookieを使用する]**

Adobe Experience Platform は、常にファーストパーティドメインに cookie を保存します。このオプションを使用すると、ファーストパーティドメインの cookie に加えて、demdex.net に設定されたサードパーティ cookie を使用できます。これは、複数のドメイン間を移動するユーザーがいる場合に役立ちます。これにより、demdex.net への呼び出しが無効になります。

### **[!UICONTROL コンテキスト]**

拡張機能は、リクエストのコンテキストに関する情報（URL やブラウザーについての詳細など）を自動的に収集します。これは、特定のコンテキストを選択解除することで無効にできます。

- **[!UICONTROL web]** - url、転送者など、Webページの詳細
- **[!UICONTROL device]**  — 画面の向き、画面の高さ、画面の幅など、デバイスに関する詳細。
- **[!UICONTROL 環境]**  — コンピューティング環境（ブラウザ、接続など）に関する情報
- **[!UICONTROL location]**  — ユーザーの場所に関する制限のある情報

## 次の作業

1. [アクションタイプ](action-types.md)を設定します。
2. [データ要素の種類](data-element-types.md)を設定します。
