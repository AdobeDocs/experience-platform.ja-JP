---
title: チュートリアルAdobe LaunchでのWebサイトタグの実装
seo-title: Adobe LaunchでのWebサイトタグの実装
description: Adobe Launchを使用してAdobe Experience PlatformにWebサイトタグを実装する
seo-description: Adobe Launchを使用してAdobe Experience PlatformにWebサイトタグを実装する
translation-type: tm+mt
source-git-commit: 3083f6fb25a331eb6dd1d9a63b65aa206481dcb3

---


# チュートリアル：Adobe LaunchでのWebサイトタグの実装

このチュートリアルでは、Adobe Launchを使用してAdobe Experience Platformにデータを送信するWebサイトタグを実装する方法について説明します。

## 前提条件

* 必要なスキーマとデータセットは、Platformで作成します。
* 必要な設定がExperience edgeにデプロイされ、一致する設定IDとEdgeドメインを持っている。
* 会社のCMSは、各ページにJavaScriptオブジェクトを配信し、プラットフォームに送信する必要のあるデータを提供するように設定されています。

## 手順

このチュートリアルでは、次の手順を実行します。

1. Adobe Experience Platform Web SDK Extensionをインストールします。
1. 「送信するデータを起動」を指定するルールを作成します。
1. 拡張機能とルールをライブラリにバンドルします。

## Adobe Experience Platform Web SDK Extensionのインストール

まず、Adobe Experience Platform Web SDK Extensionをインストールします。

1. In Launch, open the **[!UICONTROL Extensions]** tab.

   ![image](assets/launch-overview.png)

1. 拡張機能カタログの起動設定画面が開き、Adobe Experience Platform Web SDK拡張機能を選択します。

   ![image](assets/launch-extension-install.png)

   詳しくは、Launchドキュメントの [Extensions](https://docs.adobe.com/content/help/en/launch/using/reference/manage-resources/extensions/overview.html) （拡張機能）を参照してください。

1. 拡張機能を設定します。

   現在必要な設定は次のとおりです。

   * **** 設定ID:アドビの担当者から取得した設定IDを指定します。
   * **** エッジドメイン：アドビの担当者から取得したエッジドメインを指定します。

1. 「保存」 **[!UICONTROL をクリックし]** 、次の手順に進みます。

## 「送信するデータを起動」を指示するルールを作成します

次に、Adobe Experience Platformに送信するデータと送信するデータを起動に知らせるルールを作成します。

1. 「ルール **** 」タブで、起動ライブラリが読み込まれたときにWebサイトの新しい各ページでトリガーされるイベントを設定します。

   ![image](assets/launch-make-a-rule.png)

1. アクションの追加.

   アクションを設定するには、「起動」にデータレイヤーの場所を指定します。 データレイヤーは、ページ上に存在するJavaScriptオブジェクトで、Webページをレンダリングするのと同じCMSから配信されます。 データオブジェクトへのJavaScriptパスを指定します。

   ![image](assets/launch-add-aep-action.png)

   送信するデータオブジェクトは、設定IDに接続されたデータセットが使用するスキーマに対する検証を渡す有効なXDMである必要があります。

1. Click **[!UICONTROL Keep Changes]**.

詳しくは、起動ドキュメントの [ルール](https://docs.adobe.com/content/help/en/launch/using/reference/manage-resources/rules.html) （英語のみ）を参照してください。

## ライブラリへの拡張機能とルールのバンドル

次に、拡張 [機能と新しいルールを](https://docs.adobe.com/content/help/en/launch/using/reference/publish/overview.html) 1つのライブラリにバンドルし、それらの変更を開発環境でテストします。

![image](assets/launch-add-changes-to-library.png)

テストが完了したら、ワークフローを通じてライブラリをプロモーションし、実稼働サイトにデプロイできるようにします。 個々のユーザーからAdobe Experience Platformにデータが流れるようになりました。

![image](assets/launch-promote-library.png)

詳しくは、Launchドキュメントの [Libraries](https://docs.adobe.com/content/help/en/launch/using/reference/publish/libraries.html) （ライブラリ）を参照してください。