---
title: Adobe Experience Platform Web SDK 拡張機能 概要
description: Adobe Experience Platform Launch向けAdobe Experience PlatformWeb SDK Extensionについて
translation-type: tm+mt
source-git-commit: 18e511337eaa8b6eb7785b1ee5f1ce2366ddd7c7
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 19%

---


# Adobe Experience PlatformWeb SDK拡張機能の概要

Adobe Experience PlatformWeb SDK拡張は、Adobe Experience Platformエッジネットワークを介して、WebプロパティからAdobe Experience Cloudにデータを送信します。 この拡張機能を使用すると、データをプラットフォームにストリーミングしたり、IDを同期したり、顧客の同意シグナルを処理したり、コンテキストデータを自動的に収集したりできます。

このドキュメントでは、Adobe Experience Platform Launchユーザーインターフェイスで拡張機能を設定する方法を説明します。

## 拡張機能の設定

Platform Web SDK Extensionが既にプロパティ用にインストールされている場合は、Platform launchUIでプロパティを開き、「**[!UICONTROL Extensions]**」タブを選択します。 「Platform Web SDK」で、「**[!UICONTROL Configure]**」を選択します。

![](../images/extension/overview/configure.png)

拡張機能をまだインストールしていない場合は、「**[!UICONTROL カタログ]**」タブを選択します。 使用可能な拡張機能のリストから、Platform Web SDK Extensionを探し、「**[!UICONTROL インストール]**」を選択します。

![](../images/extension/overview/install.png)

どちらの場合も、プラットフォームWeb SDKの設定ページに到達します。 以下の節では、拡張機能の設定オプションについて説明します。

![](../images/extension/overview/config-screen.png)

## 一般的な設定オプション

ページ上部の設定オプションは、データのルーティング先と、サーバーで使用する設定をAdobe Experience Platformに指定します。

### [!UICONTROL 名前]

Adobe Experience PlatformWeb SDK拡張機能は、ページ上の複数のインスタンスをサポートします。 これは、1 つの Platform Launch 設定で複数の組織にデータを送信するために使用します。

拡張子の名前はデフォルトで「[!DNL alloy]」に設定されます。 ただし、インスタンス名は任意の有効な JavaScript オブジェクト名に変更できます。

### **[!UICONTROL IMS 組織 ID]**

[!UICONTROL IMS組織ID]は、Adobeでのデータ送信先の組織です。 ほとんどの場合は、自動入力されるデフォルト値を使用する必要があります。ページに複数のインスタンスがある場合は、データの送信先となる2つ目の組織の値をこのフィールドに入力します。

### **[!UICONTROL エッジドメイン]**

[!UICONTROL Edge Domain]は、Adobe Experience Platform拡張がデータを送受信するドメインです。 拡張機能では、実稼動用トラフィックにファーストパーティの CNAME を使用する必要があります。デフォルトのサードパーティドメインは開発環境で使用できますが、実稼動環境には適していません。ファーストパーティ CNAME の設定方法については、[ここ](https://docs.adobe.com/content/help/ja-JP/core-services/interface/ec-cookies/cookies-first-party.html)で説明します。

## [!UICONTROL エッジ設定]

要求がAdobe Experience Platformエッジネットワークに送信されると、エッジ設定IDを使用してサーバ側設定が参照されます。 これにより、Webサイトでコードを変更することなく、設定を更新できます。

詳しくは、[エッジ設定](../fundamentals/edge-configuration.md)のガイドを参照してください。

## [!UICONTROL プライバシー]

「[!UICONTROL プライバシー]」セクションでは、Webサイトからの顧客の同意シグナルをSDKがどのように処理するかを設定できます。 特に、他の明示的な同意の優先順位が与えられていない場合に、顧客を想定するデフォルトの同意レベルを選択できます。 次の表に、各オプションの内容を示します。

| [!UICONTROL 既定の同意レベル] | 説明 |
| --- | --- |
| [!UICONTROL In] | オプトイン。 デフォルトで顧客の同意が得られ、オプトアウトシグナルのみを受け取る場合は、このオプションを使用します。 |
| [!UICONTROL 保留中] | 「保留中」の同意を持つ顧客は、オプトイン信号が送信されるまで、オプトアウトされていると見なされます。 業務に対する明示的な顧客の同意が必要な場合に、このオプションを使用します。 |
| [!UICONTROL データ要素によって提供される] | デフォルトの同意レベルは、ユーザーが定義する別のデータ要素によって決定されます。 このオプションを使用する場合は、提供されたドロップダウンメニューを使用してデータ要素を指定する必要があります。 |