---
title: Adobe Experience Platform Debugger を使用した埋め込みコードのテスト
description: Experience Platform Debugger を使用して、web サイト上でAdobe Experience Platformの様々な埋め込みコードをローカルでテストする方法について説明します。
exl-id: ae6183b9-0d25-49d0-b0e9-f8b5ba58ab33
source-git-commit: 44e2b8241a8c348d155df3061d398c4fa43adcea
workflow-type: tm+mt
source-wordcount: '445'
ht-degree: 50%

---

# Adobe Experience Platform Debugger を使用した埋め込みコードのテスト

Adobe Experience Platform のタグライブラリビルドに変更を加える場合は、ビルドを本番環境にデプロイする前に、これらの変更をテストする必要があります。Web サイトに専用のステージングまたは開発環境がない場合は、Adobe Experience Platform Debugger を使用して、サイト内の様々な埋め込みコードをローカルでテストできます。

## 前提条件

このチュートリアルでは、タグの環境と埋め込みコードの使用に関する十分な知識が必要です。 「[環境の概要](./environments.md) 」で詳細情報を参照してください。

また、このチュートリアルでは、Experience Platform Debugger ブラウザー拡張機能がインストールされている必要があります。 Experience Platform Debugger は、Chrome ブラウザーで使用できます。 チュートリアルを開始する前に、次のリンクを使用して拡張機能をインストールします。

* [Chrome用Experience Platform デバッガー ](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)

## Web サイトでExperience Platform Debugger を開きます。

任意のブラウザーを使用して web サイトに移動し、Experience Platform Debugger 拡張機能を開きます。 Experience Platform Debugger が現在接続しているサイトが、ウィンドウの下部に表示されます。 タグが現在サイトで実行中の場合は、「[!UICONTROL Summary]」タブに表示されます。

![](./images/embed-code-testing/summary.png)

>[!NOTE]
>
>Experience Platform Debugger が最初に接続しない場合は、再試行する前に、web サイトを表示しているブラウザータブを再読み込みする必要がある可能性があります。

## 埋め込みコードの置換

Experience Platform Debugger がサイトに接続したら、左側のナビゲーションで「**[!UICONTROL Launch]**」を選択します。 ここには、環境や関連する拡張機能など、現在サイトで実行されているライブラリビルドに関する情報が表示されます。 ここから **[!UICONTROL Configuration]** を選択すると、埋め込みコードを管理するコントロールが表示されます。

![](./images/embed-code-testing/launch-tab.png)

[!UICONTROL Page Embed Codes] の下に、サイトで現在使用している埋め込みコードが表示されます。 埋め込みコードの右側で **[!UICONTROL Actions]** を選択してから、**[!UICONTROL Replace]** を選択します。

![](./images/embed-code-testing/replace.png)

現在の埋め込みコードを置き換える埋め込みコードを指定するよう求めるポップオーバーが表示されます。 Experience Platform Debugger を使用して埋め込みコードを置き換えても、サイトにデプロイされた埋め込みコードは変更されません。 代わりに、ローカルで実行されている埋め込みコードのみが置き換えられるので、実装のテストとデバッグを実行できます。

テストする埋め込みコードを表示されるテキストボックスにペーストし、**[!UICONTROL Apply]** を選択します。

![](./images/embed-code-testing/paste-code.png)

「**[!UICONTROL Configuration]**」タブが再表示され、ライブ埋め込みコードが指定したコードに置き換えられたことが示されます。 Web ブラウザーを使用して、テスト中の埋め込みコードが期待どおりに動作しているかどうかを確認できるようになりました。

![](./images/embed-code-testing/code-replaced.png)

## 次の手順

このチュートリアルでは、Experience Platform Debugger を使用してテスト目的で埋め込みコードをローカルに切り替える方法について説明しました。 様々な機能について詳しくは、[Experience Platform Debugger のドキュメント ](../../../debugger/home.md) を参照してください。
