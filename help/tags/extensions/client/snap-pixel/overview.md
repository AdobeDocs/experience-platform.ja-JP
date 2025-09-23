---
title: Snap Pixel Extension の概要
description: Snap Pixel タグ拡張機能を使用して、Adobe Experience Platformでの価値のあるユーザーインタラクションをキャプチャする方法を説明します。
last-substantial-update: 2025-09-17T00:00:00Z
source-git-commit: d846bd5dee5ce0ee8836dc25e20d9fd070714114
workflow-type: tm+mt
source-wordcount: '634'
ht-degree: 1%

---

# [!DNL Snap Pixel] 拡張機能の概要

[[!DNL Snap Pixel]](https://businesshelp.snapchat.com/s/article/snap-pixel-about) は、web サイト上での貴重なユーザーインタラクションをキャプチャできるようにするJavaScript ベースの分析ツールです。 購入、サインアップ、その他のコンバージョンなどの重要な訪問者アクションは、[ 広告マネージャー ](http://ads.snapchat.com/) に自動的に送信され、広告、キャンペーン、コンバージョンパスなどのパフォーマンスを測定および最適化できます。

[!DNL Snap Pixel] タグ拡張機能を使用すると、[!DNL Snap Pixel] の機能をクライアントサイドのタグライブラリに直接統合できます。 このドキュメントでは、拡張機能をインストールし、タグ管理ルール内でその機能を実装する方法の概要を説明します。

[!DNL Snap Pixel] タグ拡張機能を使用すると、既存のクライアント側タグライブラリに [!DNL Snap Pixel] の機能を効率的に統合できます。 このドキュメントでは、拡張機能をインストールし、タグ管理 [ ルール ](../../../ui/managing-resources/rules.md) 内でその機能を設定する方法の概要を説明します。

## 前提条件 {#prerequisites}

拡張機能を使用するには、[!DNL Snap] へのアクセス権を持つ有効な [!DNL Ads Manager] アカウントが必要です。 [ 新しいを作成  [!DNL Snap Pixel]](https://forbusiness.snapchat.com/advertising/snap-pixel#about) し、そのピクセル ID をコピーして、アカウントの拡張機能を設定する必要があります。 既存の [!DNL Snap Pixel] がある場合は、その ID を使用するだけです。

[!DNL Snap Pixel] と共に [!DNL Snap Conversions API] を使用して、クライアントサイドとサーバーサイドの両方から同じイベントを送信することをお勧めします。 このアプローチは、[!DNL Snap Pixel] ーザーだけではキャプチャできないイベントを回復するのに役立ちます。 サーバーサイド実装に統合する手順については、[[!DNL Snap]  イベント転送用の Conversions API 拡張機能 ](../../server/snap/overview.md) を参照してください。 サーバーサイド拡張機能を使用するには、組織が [ イベント転送 ](../../../ui/event-forwarding/overview.md) にアクセスできる必要があることに注意してください。

## 拡張機能のインストール {#install}

[!DNL Snap Pixel] 拡張機能をインストールするには、データ収集 UI またはExperience Platform UI に移動し、左側のナビゲーションから **[!UICONTROL タグ]** を選択します。 ここから、拡張機能を追加するプロパティを選択するか、代わりに新しいプロパティを作成します。

目的のプロパティを選択または作成したら、左側のナビゲーションで **[!UICONTROL 拡張機能]** を選択し、「**[!UICONTROL カタログ]**」タブを選択します。 [!UICONTROL Snap Pixel] カードを検索し、「**[!UICONTROL インストール]**」を選択します。

![ データ収集 UI で [!UICONTROL Snap Pixel] 拡張機能用に選択されている [!UICONTROL &#x200B; インストール &#x200B;] ボタン ](./images/install.png)

表示される設定ビューで、拡張機能をアカウントにリンクするには、以前にコピーしたピクセル ID を指定する必要があります。 ID を入力に直接貼り付けることも、代わりに既存のデータ要素を選択することもできます。

完了したら「**[!UICONTROL 保存]**」を選択します。

![ 拡張機能の設定ビューでデータ要素として提供された [!DNL Pixel] ID。](./images/configure.png)

拡張機能がインストールされ、タグルールで様々なアクションを使用できるようになりました。

## タグルールの設定 {#rule}

[!DNL Snap Pixel] れぞれ特定のコンテキストと受け入れられるパラメーターを持つ、事前定義済みの一連の標準イベントをサポートしています。 [!DNL Snap Pixel] 拡張機能で使用できるルールアクションは、これらのイベントタイプに合わせられるので、タイプに基づいて [!DNL Snap] に送信されるイベントを簡単に分類および設定できます。

デモンストレーションを目的として、この節では、購入イベントを [!DNL Snap] に送信するルールの作成方法を示します。

まず、新しいタグルールを作成し、必要に応じて条件を定義します。 ルールのアクションを設定する場合は、拡張機能として [!DNL Snap Pixel] を選択し、アクションタイプとして **[!UICONTROL 購入イベントを送信]** を選択します。

[!UICONTROL &#x200B; 購入イベントを送信 &#x200B;] アクションの設定が完了したら、「**[!UICONTROL 変更を保持]**」を選択して、ルール設定に追加します。

![ データ収集 UI でルール用に選択された [!UICONTROL &#x200B; 購入イベントを送信 &#x200B;] アクションタイプ ](./images/action-type.png)

ルールの全体的な設定に満足したら、「**[!UICONTROL ライブラリに保存]**」を選択します。

更新を適用するには、新しいタグ [ ビルド ](../../../ui/publishing/builds.md) を公開して、ライブラリに対する変更を有効にします。

## データを受信 [!DNL Snap] ていることを確認します。 {#confirm}

更新したビルドが web サイトにデプロイされたら、ブラウザーでコンバージョンイベントをトリガーし、[[!DNL Snap Events Manager]](https://businesshelp.snapchat.com/s/article/events-manager) に表示されることを確認することで、データが期待どおりに送信されていることを確認できます。

## 次の手順 {#next-steps}

このガイドでは、[!DNL Snap] タグ拡張機能を使用して [!DNL Snap Pixel] にデータを送信する方法について説明します。 サーバーサイドイベントも [!DNL Snap] に送信する予定の場合は、[[!DNL Snap Conversions API event forwarding extension]](../../server/snap/overview.md) のインストールと設定に進んでください。
