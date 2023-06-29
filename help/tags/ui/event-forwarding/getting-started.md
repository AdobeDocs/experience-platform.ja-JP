---
title: イベント転送の概要
description: Adobe Experience Platform でのイベント転送の使用を開始するには、このステップバイステップのチュートリアルに従ってください。
feature: Event Forwarding
exl-id: f82bfac9-dc2d-44de-a308-651300f107df
source-git-commit: 12bd4c6c1993afc438b75a3e5163ebe2fe8a8dd0
workflow-type: tm+mt
source-wordcount: '873'
ht-degree: 87%

---

# イベント転送の概要

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語が変更されました。用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

Adobe Experience Platform でイベント転送を使用するには、次の 3 つのオプションの 1 つ以上を使用して、データを Adobe Experience Platform Edge ネットワークに送信する必要があります。

* [Adobe Experience Platform Web SDK](../../extensions/client/web-sdk/overview.md)
* [Adobe Experience Platform モバイル SDK](https://sdkdocs.com)
* [サーバー間 API](https://experienceleague.adobe.com/docs/audience-manager/user-guide/api-and-sdk-code/dcs/dcs-apis/dcs-s2s.html?lang=ja)

>[!NOTE]
>Platform Web SDK および Platform Mobile SDK は、Adobe Experience Platform のタグを使用したデプロイメントは必要ありません。ただし、タグを使用してこれらの SDK をデプロイする方法は、推奨されるアプローチです。

Edge ネットワークにデータを送信した後、Adobeソリューションをオンにすることで、Edge ネットワークでデータを送信できます。アドビ以外のソリューションにデータを送信するには、イベント転送で設定します。

## 前提条件

* Adobe Experience Platform Collection Enterprise( 価格については、Adobeのアカウントチームにお問い合わせください )
* Adobe Experience Platform でのイベント転送
* Edge ネットワークにデータを送信するように設定された Adobe Experience Platform Web SDK または Mobile SDK
* エクスペリエンスデータモデル（XDM）へのデータのマッピング（このマッピングはタグを使用しておこなうことができます）

## XDM スキーマの作成

Adobe Experience Platform で、スキーマを作成します。

1. **[!UICONTROL スキーマ]**／**[!UICONTROL スキーマを作成]**&#x200B;を選択し、「**[!UICONTROL XDM ExperienceEvent]**」オプションを選択して、スキーマを作成します。

1. スキーマに名前を付け、簡単な説明を追加します。

1. 「ExperienceEvent Web 詳細」フィールドグループを追加するには、 **[!UICONTROL 追加]** 次の **[!UICONTROL フィールドグループ]**.

   >[!NOTE]
   >
   >必要に応じて、複数のフィールドグループを追加できます。

1. スキーマを保存し、付けた名前をメモします。

スキーマについて詳しくは、 [Experience Data Model（XDM）システムのヘルプ](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html?lang=ja) 参照してください。

## イベント転送プロパティの作成

内 **[!UICONTROL タグ]** ワークスペース、タイプのプロパティを作成 **[!UICONTROL Edge]**.

1. 「**[!UICONTROL 新しいプロパティ]**」を選択します。

1. プロパティに名前を付けます。

1. 「Edge」プラットフォームタイプを選択します。

1. 「**[!UICONTROL 保存]**」を選択します。

プロパティを作成したら、新しいプロパティの「**[!UICONTROL 環境]**」タブに移動し、
環境 ID をメモします。データストリームで使用するアドビ組織とイベント転送で使用するアドビ組織が異なる場合は、「**[!UICONTROL 環境]**」タブから環境 ID をコピーして、データストリーム作成時に貼り付けることができます。それ以外の場合は、ドロップダウンメニューから環境を選択できます。

## データストリームの作成

Adobe Experience Platform でデータストリームを作成するには、イベント転送プロパティの作成時に生成された環境 ID を使用します。

1. 選択 **[!UICONTROL データストリーム]** をクリックします。

1. 設定に名前を付け、必要に応じて説明を入力します。
この説明は、複数の設定を含むリストで目的の設定を識別するのに役立ちます。

1. 「**[!UICONTROL 保存]**」を選択します。

## イベント転送の有効化

次に、Edge ネットワークを設定して、イベント転送や他のアドビ製品にデータを送信します。

1. 内 **[!UICONTROL データストリーム]** ワークスペースで、作成したプロパティを選択します。

1. 「開発」、「実稼働」、または「ステージング」環境を選択します。

   または、アドビ組織外のイベント転送環境にデータを送信する場合は、「**[!UICONTROL 詳細設定モードに切り替え]**」を選択し、ID を貼り付けます。この ID は、イベント転送プロパティを作成すると提供されます。

1. 必要なツールをオンにして、必要に応じて設定します。

   * Adobe Analytics では、レポートスイート ID が必要です。

   * Adobe Experience Platform でのイベント転送には、プロパティ ID と環境 ID が必要です。これは、イベント転送プロパティのパブリッシュパスです。

設定後、新しいプロパティの環境 ID をメモします。

## Platform Web SDK 拡張機能を設定して、以前作成したデータストリームにデータを送信します。

プロパティを **[!UICONTROL タグ]** workspace に移動して、に移動します。 **[!UICONTROL 拡張機能]** をクリックし、カタログからExperience PlatformWeb SDK 拡張機能を選択して、設定およびインストールします。

詳しくは、 [Web SDK 拡張機能のドキュメント](../../extensions/client/web-sdk/overview.md) 設定オプションの詳細については、を参照してください。

## Platform Web SDK にデータを送信するタグルールを作成する

上記の手順を実行した後、イベント転送とタグを使用し、ページからのリクエストを 1 つだけ必要とするデータ定義やルールなどを作成します。

Platform Web SDK 拡張機能と「イベントを送信」アクションタイプを使用して、ページ読み込みルールを作成します。

1. 「**[!UICONTROL ルール]**」タブを開き、「**[!UICONTROL 新しいルールを作成]**」を選択します。

1. ルール名を設定します。

1. 「**[!UICONTROL イベントを追加]**」を選択します。

1. 拡張機能とその拡張機能で使用可能なイベントタイプを 1 つ選択してイベントを追加し、そのイベントの設定を指定します。例えば、「**[!UICONTROL Core - Window 搭載]**」を選択します。

1. Platform Web SDK 拡張機能を使用してアクションを追加します。「**[!UICONTROL アクションタイプ]**」リストから「**[!UICONTROL イベントを送信]**」を選択し、目的のインスタンス（以前設定した Alloy インスタンス）を選択して、Alloy ヒット内の XDM データブロックに追加するデータ要素を選択します。

1. この例では、残りの設定をデフォルトのままにして「**[!UICONTROL 保存]**」を選択します。

別の例では、ユーザーが特定のボタンの上にカーソルを置くと、データレイヤーが Edge に送信されるルールを作成できます。

## 概要

以下の設定が完了したら、イベント転送ルールを作成して、データをアドビ以外の宛先に転送できるようになります。

* エクスペリエンスデータモデルスキーマ（付けた名前をメモします）
* イベント転送プロパティ（プロパティ ID と環境 ID を追跡します）
* データストリーム（環境 ID をメモしておきます。イベント転送の環境 ID と混同しないでください）
* タグのプロパティ
