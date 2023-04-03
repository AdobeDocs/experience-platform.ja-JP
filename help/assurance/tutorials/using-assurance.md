---
title: Adobe Experience Platform Assurance の使用
description: このガイドでは、Adobe Experience Platform Assurance をインストールして実装した後の使用方法を説明します。
source-git-commit: 24f452bdb923179eefe53fdb28d3a92d43e533cd
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 0%

---


# Adobe Experience Platform Assurance の使用

このチュートリアルでは、Adobe Experience Platform Assurance の使用方法を説明します。 Adobe Experience Platform Assurance 拡張機能のインストールと実装の手順については、 [Assurance 拡張機能の実装](./implement-assurance.md).

## セッションの作成

へのログイン後、 [アシュランス UI](https://experience.adobe.com/assurance)を選択すると、 **[!UICONTROL セッションを作成]** をクリックして、セッションの作成を開始します。

![「セッションを作成」ボタンがハイライト表示され、セッションを作成できる場所が示されます。](./images/using-assurance/create-session.png)

この **[!UICONTROL 新しいセッションを作成]** ダイアログが表示されます。 指示を確認し、次の手順に進んでください： **[!UICONTROL 開始]**.

![Create New Session ダイアログが表示され、Assurance の使用方法に関する手順が示されます。](./images/using-assurance/create-new-session.png)

セッションを識別する名前を入力し、 **[!UICONTROL ベース URL]** （アプリのディープリンク URL）。 これらの詳細を入力した後、 **[!UICONTROL 次へ]**.

>[!INFO]
>
>ベース URL は、URL からアプリを起動するために使用されるルート定義です。 セッション URL は、アシュランスセッションを開始するために生成されます。 値の例を次に示します。 `myapp://default` 内 **[!UICONTROL ベース URL]** 「 」フィールドに、アプリのベースディープリンク定義を入力します。

![新しいセッションを作成するための完全なワークフローが表示されます。](./images/using-assurance/create-session.gif)

## セッションに接続

セッションを作成した後、必ず **[!UICONTROL 新しいセッションを作成]** ダイアログに、リンク、QR コード、PIN が表示されます。

![アシュランスセッションに接続するためのオプションを示すダイアログが表示されます。](./images/using-assurance/create-new-session-pin.png)

このダイアログが表示された場合は、デバイスのカメラアプリを使用して QR コードをスキャンし、アプリを開くか、リンクをコピーしてアプリで開くことができます。 アプリが起動すると、PIN エントリ画面がオーバーレイ表示されます。 前の手順の PIN を入力し、を押します。 **[!UICONTROL 接続]**.

Adobe Experience Platformアイコン ( 赤いAdobe「A」) がアプリに表示されている場合、アプリがアシュランスに接続されていることを確認できます。

![アプリケーションをアシュランスセッションに接続するための完全なワークフローが表示されます。](./images/using-assurance/connect-session.gif)

## セッションのエクスポート

アシュランスセッションを書き出すには、アプリのセッションの詳細ページで、 **[!UICONTROL JSON に書き出し]** セッション内：

![セッションのエクスポート](./images/using-assurance/export-session.png)

書き出しオプションは、検索フィルター結果に従い、イベントビューに表示されたイベントのみを書き出します。 例えば、「track」イベントを検索し、「 **[!UICONTROL JSON に書き出し]**&#x200B;に設定すると、「track」イベントの結果のみが書き出されます。
