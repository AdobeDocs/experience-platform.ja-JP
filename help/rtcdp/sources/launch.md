---
keywords: Webタグの起動；Webタグの起動；Webタグ；Webタグ；Webタグ；起動；起動
title: チュートリアル：Adobe Launch を使用した Web サイトタグの実装
seo-title: Adobe Launch を使用した Web サイトタグの実装
description: Adobe Launch を使用した Adobe Experience Platform での Web サイトタグの実装
seo-description: Adobe Launch を使用した Adobe Experience Platform での Web サイトタグの実装
translation-type: tm+mt
source-git-commit: 00010d38a5d05800aeac9af8505093fee3593b45
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 78%

---


# チュートリアル：Adobe Launch を使用した Web サイトタグの実装

このチュートリアルでは、Adobe Launch を使用して Adobe Experience Platform にデータを送信する Web サイトタグを実装する方法について説明します。

## 前提条件

* 必要なスキーマとデータセットは[!DNL Platform]に作成されます。
* 必要な設定が Experience Edge にデプロイされ、一致する設定 ID と Edge ドメインを持っている。
* 会社の CMS は、各ページに JavaScript オブジェクトを配信し、Platform 送信する必要のあるデータを提供するように設定されている。。

## 手順

このチュートリアルでは、次の手順を実行します。

1. Adobe Experience Platform[!DNL Web SDK]拡張をインストールします。
1. 送信するデータを[!DNL Launch]に伝えるルールを作成します。
1. 拡張機能とルールをライブラリにバンドルします。

## Adobe Experience Platform[!DNL Web SDK]拡張機能をインストールします。

まず、Adobe Experience Platform[!DNL Web SDK]拡張をインストールします。

1. [!DNL Launch]で、「**[!UICONTROL 拡張子]**」タブを開きます。

   ![画像](assets/launch-overview.png)

1. 「Launch 拡張機能カタログ」から「Adobe Experience Platform Web SDK 拡張機能」を選択します。
設定画面が開きます。

   ![画像](assets/launch-extension-install.png)

   詳しくは、 ドキュメントの「[拡張機能](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/manage-resources/extensions/overview.html)」を参照してください。[!DNL Launch]

1. 拡張機能を設定します。

   現在必要な設定は以下のみです。

   * **設定 ID**：Adobe 担当者から取得した設定 ID を指定します。
   * **Edge ドメイン**：Adobe 担当者から取得した Edge ドメインを指定します。

1. 「**[!UICONTROL 保存]**」を選択し、次の手順に進みます。

## 送信するデータを[!DNL Launch]に伝えるルールを作成する

次に、[!DNL Launch]に、Adobe Experience Platformに送信するデータと送信するタイミングを知らせるルールを作成します。

1. 「**[!UICONTROL ルール]**」タブで、 ライブラリが読み込まれたときに Web サイトの新しい各ページでトリガーされるイベントを設定します。[!DNL Launch]

   ![画像](assets/launch-make-a-rule.png)

1. アクションを追加します。

   アクションを設定するには、[!DNL Launch]にデータレイヤーの場所を指定します。 データレイヤーは、ページ上に存在する JavaScript オブジェクトで、Web ページをレンダリングするのと同じ CMS から配信されます。データオブジェクトへの JavaScript パスを指定します。

   ![画像](assets/launch-add-aep-action.png)

   送信するデータオブジェクトは、設定 ID に接続されたデータセットが使用するスキーマに対して検証された有効な XDM である必要があります。

1. 「**[!UICONTROL 変更を保持]**」を選択します。

詳しくは、 ドキュメントの「[ルール](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/manage-resources/rules.html)」を参照してください。[!DNL Launch]

## ライブラリでの拡張機能とルールのバンドル

次に、ライブラリで新しいルールを[拡張機能とバンドル](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/publish/overview.html)し、それらの変更を開発環境でテストします。

![画像](assets/launch-add-changes-to-library.png)

テストが完了したら、ワークフローを通じてライブラリをプロモーションし、実稼働サイトにデプロイできるようにします。これで、個々のユーザーから Adobe Experience Platform にデータが流れるようになりました。

![画像](assets/launch-promote-library.png)

詳しくは、 ドキュメントの「[ライブラリ](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/publish/libraries.html)」を参照してください。[!DNL Launch]