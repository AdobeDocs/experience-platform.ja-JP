---
title: イベント転送の概要
description: Adobe Experience Platform でのイベント転送の使用を開始するには、このステップバイステップのチュートリアルに従ってください。
feature: Event Forwarding
exl-id: f82bfac9-dc2d-44de-a308-651300f107df
source-git-commit: 44e2b8241a8c348d155df3061d398c4fa43adcea
workflow-type: tm+mt
source-wordcount: '809'
ht-degree: 70%

---

# イベント転送の概要

>[!NOTE]
>
>イベント転送は、Adobe Real-time Customer Data Platform Connections、Prime または Ultimate の一部として提供される有料機能です。

Adobe Experience Platform でイベント転送を使用するには、次の 3 つのオプションの 1 つ以上を使用して、データを Adobe Experience Platform Edge ネットワークに送信する必要があります。

* [Adobe Experience Platform Web SDK](../../extensions/client/web-sdk/overview.md)
* [Adobe Experience Platform モバイル SDK](https://sdkdocs.com)
* [Edge Network API](https://developer.adobe.com/data-collection-apis/docs/)

>[!NOTE]
>Experience Platform Web SDKおよびExperience Platform Mobile SDKは、Adobe Experience Platformのタグを使用したデプロイメントは必要ありません。 ただし、タグを使用してこれらの SDK をデプロイする方法は、推奨されるアプローチです。

Edge ネットワークにデータを送信した後、Adobe ソリューションをオンに切り替えることで、Edge ネットワークでデータを送信できます。アドビ以外のソリューションにデータを送信するには、イベント転送で設定します。

## 前提条件

* Adobe Real-Time CDP Connections、Prime、Ultimateのいずれか（価格については、Adobe アカウントチームにお問い合わせください）
* Adobe Experience Platform でのイベント転送
* Edge Networkにデータを送信するように設定されたAdobe Experience Platform Web SDK、Mobile SDKまたはEdge Network API
* エクスペリエンスデータモデル（XDM）へのデータのマッピング（このマッピングはタグを使用しておこなうことができます）

## XDM スキーマの作成

Adobe Experience Platform で、スキーマを作成します。

1. **[!UICONTROL Schemas]** > **[!UICONTROL Create Schema]** を選択し「**[!UICONTROL XDM ExperienceEvent]**&#x200B;オプション」を選択してスキーマを作成します。

1. スキーマに名前を付け、簡単な説明を追加します。

1. **[!UICONTROL Add]** の横にある「**[!UICONTROL Field Groups]**」を選択して、「ExperienceEvent web details」フィールドグループを追加できます。

   >[!NOTE]
   >
   >必要に応じて、複数のフィールドグループを追加できます。

1. スキーマを保存し、付けた名前をメモします。

スキーマについて詳しくは、 [Experience Data Model（XDM）システムのヘルプ](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html?lang=ja) 参照してください。

## イベント転送プロパティの作成

**[!UICONTROL Tags]** ワークスペースで、タイプ **[!UICONTROL Edge]** のプロパティを作成します。

1. **[!UICONTROL New Property]** を選択します。

1. プロパティに名前を付けます。

1. 「Edge」プラットフォームタイプを選択します。

1. **[!UICONTROL Save]** を選択します。

プロパティを作成したら、新しいプロパティの「**[!UICONTROL Environments]**」タブに移動し、
環境 ID をメモします。データストリームで使用するAdobe組織とイベント転送で使用するAdobe組織が異なる場合は、「**[!UICONTROL Environments]**」タブから環境 ID をコピーして、データストリーム作成時に貼り付けることができます。 それ以外の場合は、ドロップダウンメニューから環境を選択できます。

## データストリームの作成

Adobe Experience Platform でデータストリームを作成するには、イベント転送プロパティの作成時に生成された環境 ID を使用します。

1. 左側のナビゲーションの「**[!UICONTROL Datastreams]**」を選択します。

1. 設定に名前を付け、必要に応じて説明を入力します。
この説明は、複数の設定を含むリストで目的の設定を識別するのに役立ちます。

1. **[!UICONTROL Save]** を選択します。

## イベント転送の有効化 {#enable-event-forwarding}

次に、Edge ネットワークを設定して、イベント転送や他のアドビ製品にデータを送信します。

1. **[!UICONTROL Datastreams]** ワークスペースで、作成したプロパティを選択します。

1. 「開発」、「実稼働」、または「ステージング」環境を選択します。

   または、Adobe組織外のイベント転送環境にデータを送信する場合は、「**[!UICONTROL Switch to Advanced Mode]**」を選択して ID を貼り付けます。 この ID は、イベント転送プロパティを作成すると提供されます。

1. 必要なツールをオンに切り替えて、必要に応じて設定します。

   * Adobe Analytics では、レポートスイート ID が必要です。

   * Adobe Experience Platform でのイベント転送には、プロパティ ID と環境 ID が必要です。これは、イベント転送プロパティのパブリッシュパスです。

設定後、新しいプロパティの環境 ID をメモします。

## Experience Platform Web SDK拡張機能を設定して、前に作成したデータストリームにデータを送信します。

**[!UICONTROL Tags]** Workspace でプロパティを作成し、**[!UICONTROL Extensions]** に移動して、カタログからExperience Platform Web SDK拡張機能を選択して、設定とインストールを行います。

設定オプションについて詳しくは、[Web SDK拡張機能のドキュメント &#x200B;](../../extensions/client/web-sdk/overview.md) を参照してください。

## Experience Platform Web SDKにデータを送信するタグルールを作成する

上記の手順を実行した後、イベント転送とタグを使用し、ページからのリクエストを 1 つだけ必要とするデータ定義やルールなどを作成します。

Experience Platform Web SDK拡張機能と「イベントを送信」アクションタイプを使用して、ページ読み込みルールを作成します。

1. 「**[!UICONTROL Rules]**」タブを開き、「**[!UICONTROL Create New Rule]**」を選択します。

1. ルール名を設定します。

1. **[!UICONTROL Events Add]** を選択します。

1. 拡張機能とその拡張機能で使用可能なイベントタイプを 1 つ選択してイベントを追加し、そのイベントの設定を指定します。例えば、「 **[!UICONTROL Core - Window Loaded]**」を選択します。

1. Experience Platform Web SDK拡張機能を使用してアクションを追加します。 「**[!UICONTROL Action Type]**」リストから「**[!UICONTROL Send Event]**」を選択し、目的のインスタンス（以前に設定した Alloy インスタンス）を選択して、Alloy ヒット内の XDM データブロックに追加するデータ要素を選択します。

1. この例では、残りの設定をデフォルトのままにし、「**[!UICONTROL Save]**」を選択します。

別の例では、ユーザーが特定のボタンの上にカーソルを置くと、データレイヤーが Edge に送信されるルールを作成できます。

## 概要

以下の設定が完了したら、イベント転送ルールを作成して、データをアドビ以外の宛先に転送できるようになります。

* エクスペリエンスデータモデルスキーマ（付けた名前をメモします）
* イベント転送プロパティ（プロパティ ID と環境 ID を追跡します）
* データストリーム（環境 ID をメモしておきます。イベント転送の環境 ID と混同しないでください）
* タグのプロパティ
