---
title: Adobe Experience Platform Assurance の使用
description: このガイドでは、インストールおよび実装が完了した後のAdobe Experience Platform Assurance の使用方法について説明します。
exl-id: 872c83d1-82e8-40d8-9b66-3e51a91a955f
source-git-commit: 05a7b73da610a30119b4719ae6b6d85f93cdc2ae
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 0%

---

# Adobe Experience Platform Assurance の使用

このチュートリアルでは、Adobe Experience Platform Assurance の使用方法について説明します。 Adobe Experience Platform Assurance 拡張機能のインストールおよび実装方法について詳しくは、[Assurance 拡張機能の実装 ](./implement-assurance.md) に関するチュートリアルを参照してください。

## セッションを作成

[Assurance UI](https://experience.adobe.com/assurance) にログインした後、**[!UICONTROL セッションを作成]** を選択してセッションの作成を開始できます。

![ 「セッションを作成」ボタンがハイライト表示され、セッションを作成できる場所が示されている様子。](./images/using-assurance/create-session.png)

**[!UICONTROL 新規セッションを作成]** ダイアログが表示されます。 指示を確認し、「**[!UICONTROL 開始]**」を選択して続行してください。

![ 新規セッションの作成ダイアログが表示され、Assurance の使用方法が示されます。](./images/using-assurance/create-new-session.png)

セッションを識別する名前を入力し、**[!UICONTROL ベース URL]** （アプリのディープリンク URL）を指定できるようになりました。 これらの詳細を入力したら、「**[!UICONTROL 次へ]**」を選択します。

>[!INFO]
>
>ベース URL は、URL からアプリを起動するために使用されるルート定義です。 セッション URL は、アシュランスセッションを開始するために生成されます。 値の例は次のようになります。`myapp://default` **[!UICONTROL ベース URL]** フィールドに、アプリのベースのディープリンク定義を入力します。

![ 新しいセッションの作成に関する完全なワークフローが表示されます。](./images/using-assurance/create-session.gif)

## セッションへの接続

セッションを作成したら、**[!UICONTROL 新しいセッションを作成]** ダイアログに、リンク、QR コード、PIN が表示されます。

![Assurance セッションへの接続オプションを示すダイアログが表示されます。](./images/using-assurance/create-new-session-pin.png)

このダイアログが表示された場合は、デバイスのカメラアプリを使用して QR コードをスキャンし、アプリを開くか、リンクをコピーしてアプリで開くことができます。 アプリが起動すると、PIN 入力画面がオーバーレイ表示されます。 前の手順で入力した PIN を入力し、**[!UICONTROL 接続]** キーを押します。

Adobe Experience Platformアイコン（赤Adobe「A」）がアプリに表示されると、アプリが Assurance に接続されていることを確認できます。

![ アプリケーションをアシュランスセッションに接続する完全なワークフローが表示されます。](./images/using-assurance/connect-session.gif)

## セッションのエクスポート

Assurance セッションを書き出すには、アプリのセッションの詳細ページで、セッションの「**[!UICONTROL JSON に書き出し]**」を選択します。

![ セッションのエクスポート ](./images/using-assurance/export-session.png)

書き出しオプションは、検索フィルターの結果に従い、イベント表示に表示されるイベントのみを書き出します。 例えば、「トラック」イベントを検索してから「JSON に書き出し **[!UICONTROL を選択すると、「トラック」イベントの結果のみ]** 書き出されます。
