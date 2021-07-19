---
keywords: Confirmit Digital Feedback;confirmit拡張機能；confirmit
title: Confirmit Digital Feedback 拡張機能
description: Confirmit Digital Feedback拡張機能は、Adobe Experience Platformの「Voice of the Customer」の宛先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
exl-id: b45efd1f-318c-42c4-8a88-02f25fc31319
source-git-commit: 12c3f440319046491054b3ef3ec404798bb61f06
workflow-type: tm+mt
source-wordcount: '525'
ht-degree: 28%

---

# [!DNL Confirmit Digital Feedback] 拡張機能

## 概要 {#overview}

[!DNL Confirmit Digital Feedback] ソリューションは、webサイトのトラフィックをリアルタイムのインサイトに変換するのに役立ちます。[!DNL Confirmit]を使用すると、控えめで高度にターゲットを絞った調査を要件に応じて表示し、訪問者に次のようなフィードバックを提供するよう促すことができます。

* Web サイトのフィードバック
* 取引満足度
* 終了インテントの理由
* 買い物かご放棄情報
* 全体的な顧客満足度
* その他多数

[!DNL Confirmit] デジタルフィードバックは、Adobe Experience PlatformのVoice of the Customer拡張機能です。拡張機能について詳しくは、[Adobe Exchange](https://exchange.adobe.com/experiencecloud.details.103247.confirmit-digital-feedback-for-adobe-launch.html) の拡張機能のページを参照してください。

この宛先は、Adobe Experience Platform Launch拡張機能です。 PlatformでのPlatform launch拡張機能の仕組みについて詳しくは、「[Adobe Experience Platform Launch拡張機能の概要](../launch-extensions/overview.md)」を参照してください。

![Confirmit Digital Feedback 拡張機能](../../assets/catalog/voice/confirmit-digital-feedback/catalog.png)

## 前提条件 {#prerequisites}

この拡張機能は、Platformを購入したすべての顧客の[!DNL Destinations]カタログで利用できます。

この拡張機能を使用するには、Adobe Experience Platform Launchにアクセスする必要があります。  Platform Launch は、Adobe Experience Cloud に付属の付加価値機能として提供されています。組織の管理者に問い合わせてPlatform launchへのアクセス権を取得し、拡張機能のインストールに必要な&#x200B;**[!UICONTROL manage_properties]**&#x200B;権限を付与するよう管理者に依頼します。

## 拡張機能のインストール {#install-extension}

[!DNL Confirmit] Digital Feedback拡張機能をインストールするには：

[Platformインターフェイス](http://platform.adobe.com/)で、**[!UICONTROL 宛先]**/**[!UICONTROL カタログ]**&#x200B;に移動します。

カタログから拡張機能を選択するか、検索バーを使用します。

宛先をクリックしてハイライトし、右側のレールで「**[!UICONTROL 設定]**」を選択します。 **[!UICONTROL Configure]**&#x200B;コントロールがグレー表示になっている場合は、**[!UICONTROL manage_properties]**&#x200B;権限がありません。 [前提条件](#prerequisites)を確認してください。

**[!UICONTROL 使用可能なPlatform launchプロパティを選択]**&#x200B;ウィンドウで、拡張機能をインストールするPlatform launchプロパティを選択します。 また、「 」で新しいプロパティを作成するオプションもあります。Platform launch プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについては、Platform launchドキュメントの[プロパティページの節](../../../tags/ui/administration/companies-and-properties.md#properties-page)を参照してください。

ワークフローに従って、Platform launchが表示され、インストールが完了します。

拡張機能の設定オプションとインストールのサポートについて詳しくは、[Adobe Exchange の Confirmit Digital Feedback ページ](https://exchange.adobe.com/experiencecloud.details.103247.confirmit-digital-feedback-for-adobe-launch.html)を参照してください。

拡張機能は、[Adobe Experience Platform Launchインターフェイス](https://launch.adobe.com/)に直接インストールすることもできます。 platform launchのドキュメントの[新しい拡張子](../../../tags/ui/managing-resources/extensions/overview.md#add-a-new-extension)の追加を参照してください。

## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールしたら、その拡張機能のルールを直接Platform launchで設定できます。

platform launchでは、特定の状況でのみ拡張機能の宛先にイベントデータを送信するように、インストールした拡張機能のルールを設定できます。 拡張機能のルールの設定について詳しくは、[ルールに関するドキュメント](../../../tags/ui/managing-resources/rules.md)を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

拡張機能インターフェイスで、拡張機能の設定、アップグレードおよび削除をPlatform launchできます。

>[!TIP]
>
>拡張機能が既にいずれかのプロパティにインストールされている場合、Platform UIには、拡張機能に対して&#x200B;**[!UICONTROL Install]**&#x200B;が表示されます。 「[拡張機能のインストール](#install-extension)」の説明に従ってインストールワークフローを開始し、Platform launchに移動して拡張機能を設定または削除します。

拡張機能をアップグレードするには、拡張機能のドキュメントの「[拡張機能のアップグレード](../../../tags/ui/managing-resources/extensions/extension-upgrade.md)」を参照してください。
