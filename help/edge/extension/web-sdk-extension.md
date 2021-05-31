---
title: Adobe Experience Platform Web SDK 拡張機能の概要
description: Adobe Experience Platform Launch向けAdobe Experience Platform Web SDK拡張機能について説明します。
exl-id: 96d32db8-0c9a-49f0-91f3-0244522d66df
source-git-commit: c3d66e50f647c2203fcdd5ad36ad86ed223733e3
workflow-type: tm+mt
source-wordcount: '579'
ht-degree: 13%

---

# Adobe Experience Platform Web SDK 拡張機能の概要

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

[!UICONTROL Edge Domain]は、Adobe Experience Platform拡張機能がデータの送受信をおこなうドメインです。 拡張機能では、実稼動用トラフィックにファーストパーティの CNAME を使用する必要があります。デフォルトのサードパーティドメインは開発環境で使用できますが、実稼動環境には適していません。ファーストパーティ CNAME の設定方法については、[ここ](https://experienceleague.adobe.com/docs/core-services/interface/ec-cookies/cookies-first-party.html)で説明します。

## [!UICONTROL データストリーム]

リクエストがAdobe Experience Platform Edgeネットワークに送信されると、データストリームIDがサーバー側の設定の参照に使用されます。 Webサイト上でコードを変更することなく、設定を更新できます。

詳しくは、[datastreams](../fundamentals/datastreams.md)のガイドを参照してください。


## [!UICONTROL プライバシー]

「[!UICONTROL プライバシー]」セクションでは、SDKがWebサイトからのユーザーの同意シグナルを処理する方法を設定できます。 特に、他の明示的な同意設定が指定されていない場合に、ユーザーが想定するデフォルトの同意レベルを選択できます。 デフォルトの同意レベルは、ユーザーのプロファイルに保存されません。 次の表に、各オプションの内容を示します。

| [!UICONTROL デフォルトの同意レベル] | 説明 |
| --- | --- |
| [!UICONTROL In] | ユーザーが同意設定を提供する前に発生したイベントを収集します。 |
| [!UICONTROL Out] | ユーザーが同意設定を提供する前に発生したイベントを破棄する。 |
| [!UICONTROL 保留中] | ユーザーが同意設定を提供する前に発生したイベントをキューに入れる。 同意の環境設定を指定すると、指定した環境設定に応じてイベントが収集または破棄されます。 |
| [!UICONTROL データ要素で指定] | デフォルトの同意レベルは、定義する個別のデータ要素によって決定されます。 このオプションを使用する場合は、提供されたドロップダウンメニューを使用してデータ要素を指定する必要があります。 |

業務上の明示的なユーザーの同意が必要な場合は、「Out」または「Pending」を使用します。
