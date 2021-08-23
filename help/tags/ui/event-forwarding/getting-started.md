---
title: イベント転送の概要
description: このステップバイステップのチュートリアルに従って、Adobe Experience Platform でイベント転送の使用を開始してください。
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 94%

---

# イベント転送の概要

>[!NOTE]
>
>Adobe Experience Platform Launchは、Adobe Experience Platformのデータ収集テクノロジーのスイートとしてリブランドされました。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

Adobe Experience Platform でイベント転送を使用するには、次の 3 つのオプションの 1 つ以上を使用して、データを Adobe Experience Platform Edge ネットワークに送信する必要があります。

* [Adobe Experience Platform Web SDK](../../extensions/web/sdk/overview.md)
* [Adobe Experience Platform モバイル SDK](https://sdkdocs.com)
* [サーバー間 API](https://experienceleague.adobe.com/docs/audience-manager/user-guide/api-and-sdk-code/dcs/dcs-apis/dcs-s2s.html?lang=ja)

>[!NOTE]
>Platform web SDK および Platform Mobile SDK は、Adobe Experience Platform のタグを介したデプロイメントは必要ありません。ただし、タグを使用してこれらの SDK をデプロイすることが推奨されるアプローチです。

Edge ネットワークにデータを送信した後、Adobeソリューションをオンにすることで、Edge ネットワークでデータを送信できます。アドビ以外のソリューションにデータを送信するには、イベント転送で設定します。

## 前提条件

* Adobe Experience Platform Collection Enterprise（価格については担当のアカウントマネージャーにお問い合わせください）
* Adobe Experience Platform でのイベント転送
* Edge ネットワークにデータを送信するように設定された Adobe Experience Platform Web SDK または Mobile SDK
* Experience Data Model（XDM）へのデータのマッピング（このマッピングはタグを使用して実行できます）

## XDM スキーマの作成

Adobe Experience Platform で、スキーマを作成します。

1. **[!UICONTROL スキーマ]**／**[!UICONTROL スキーマを作成]**&#x200B;を選択し、「**[!UICONTROL XDM ExperienceEvent]**」オプションを選択して、スキーマを作成します。

1. スキーマに名前を付け、簡単な説明を追加します。

1. 「**[!UICONTROL フィールドグループ]**」の横にある「**[!UICONTROL 追加]**」を選択して、「ExperienceEvent Web の詳細」フィールドグループを追加できます。

   >[!NOTE]
   >
   >必要に応じて、複数のフィールドグループを追加できます。

1. スキーマを保存し、付けた名前をメモします。

スキーマについて詳しくは、[Experience Data Model（XDM）システムのヘルプ](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html?lang=ja)を参照してください。

## イベント転送プロパティの作成

データ収集 UI で、「Edge」タイプのプロパティを作成します。

1. 「**[!UICONTROL 新しいプロパティ]**」を選択します。

1. プロパティに名前を付けます。

1. 「Edge」プラットフォームタイプを選択します。

1. 「**[!UICONTROL 保存]**」を選択します。

プロパティを作成したら、新しいプロパティの「**[!UICONTROL 環境]**」タブに移動し、
環境 ID をメモします。データストリームで使用されるAdobe組織がAdobe転送で使用されるイベント組織と異なる場合は、**[!UICONTROL 「環境]**」タブから環境IDをコピーし、データストリームの作成時に貼り付けることができます。 それ以外の場合は、ドロップダウンメニューから環境を選択できます。

## データストリームの作成

Adobe Experience Platform でデータストリームを作成するには、イベント転送プロパティの作成時に生成された環境 ID を使用します。

1. データ収集 UI の左側のレールにあるリンクを使用して、データストリームインターフェイスを開きます。

1. **[!UICONTROL データストリーム]**&#x200B;を選択します。

1. 設定に名前を付け、必要に応じて説明を入力します。
この説明は、複数の設定を含むリストで目的の設定を識別するのに役立ちます。

1. 「**[!UICONTROL 保存]**」を選択します。



## イベント転送の有効化

次に、Edge ネットワークを設定して、イベント転送や他のアドビ製品にデータを送信します。

1. データストリーム UI で、作成したプロパティを選択します。

1. 「開発」、「実稼働」、または「ステージング」環境を選択します。

   または、アドビ組織外のイベント転送環境にデータを送信する場合は、「**[!UICONTROL 詳細設定モードに切り替え]**」を選択して、ID を貼り付けます。この ID は、イベント転送プロパティの作成時に提供されます。

1. 必要なツールをオンにして、必要に応じて設定します。

   * Adobe Analytics では、レポートスイート ID が必要です。

   * Adobe Experience Platform でのイベント転送には、プロパティ ID と環境 ID が必要です。 これは、イベント転送プロパティの公開パスです。

設定後、新しいプロパティの環境 ID をメモします。

## タグ Web SDK 拡張機能を設定して、前に作成したデータストリームにデータを送信します。

データ収集 UI でプロパティを作成し、Adobe Experience Platform Web SDK 拡張機能を使用して設定します。

1. プロパティに名前を付けます。

   Alloy の複数のインスタンスを使用することができます。例えば、ペイウォール前とペイウォール後のトラッキングプロパティが異なる場合があります。

1. 組織 ID を選択します。

1. Edge ドメインを選択します。

設定オプションについて詳しくは、[Web SDK 拡張機能のドキュメント](../../extensions/web/sdk/overview.md)を参照してください。

## Platform web SDK にデータを送信するタグルールを作成する

上記を完了した後、データ定義やルールなどを作成します。その中ではイベント転送とタグを使用しますが、ページから必要なリクエストは 1 つだけです。

Platform Web SDK 拡張機能と「イベントを送信」アクションタイプを使用して、ページ読み込みルールを作成します。

1. 「**[!UICONTROL ルール]**」タブを開き、「**[!UICONTROL 新しいルールを作成]**」を選択します。

1. ルール名を設定します。

1. 「**[!UICONTROL イベントを追加]**」を選択します。

1. 拡張機能とその拡張機能で使用可能なイベントタイプを 1 つ選択してイベントを追加し、そのイベントの設定を指定します。例えば、「**[!UICONTROL Core - Window 搭載]**」を選択します。

1. Platform Web SDK 拡張機能を使用してアクションを追加します。「**[!UICONTROL アクションタイプ]**」リストから「**[!UICONTROL イベントを送信]**」を選択し、目的のインスタンス（以前設定した Alloy インスタンス）を選択して、Alloy ヒット内の XDM データブロックに追加するデータ要素を選択します。

1. この例では、残りの設定をデフォルトのままにして「**[!UICONTROL 保存]**」を選択します。

別の例では、ユーザーが特定のボタンの上にカーソルを置くと、データレイヤーが Edge に送信されるルールを作成できます。

## 概要

以下の設定を完了すると、アドビ以外の宛先にデータを転送するイベント転送ルールを作成できるようになります。

* エクスペリエンスデータモデルスキーマ（付けた名前をメモします）
* イベント転送プロパティ（プロパティ ID と環境 ID を追跡します。）
* データストリーム（イベント転送からの環境 ID と混同しないように、環境 ID に注意します。）
* タグのプロパティ
