---
title: Trade Desk Real-Time Conversions API拡張機能の概要
description: Adobe Experience Platformでのイベント転送用のTrade Desk Real-Time Conversions API拡張機能について説明します。
exl-id: 1ff32e2b-9ff8-4395-ae44-cba75a2da515
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '888'
ht-degree: 3%

---

# [!DNL The Trade Desk Real-Time Conversions API]拡張機能の概要

[[!DNL The Trade Desk Real-Time Conversions API]](https://partner.thetradedesk.com/v3/portal/data/doc/DataConversionEventsApi)拡張機能を使用して、[!DNL The Trade Desk] イベント転送[&#x200B; ルールのAPI機能を利用して、Adobe Experience Platform Edge Networkから](../../../ui/event-forwarding/overview.md)にデータを送信できます。

[!DNL The Trade Desk Real-Time Conversions API]拡張機能を使用すると、[&#x200B; イベント転送](../../../ui/event-forwarding/overview.md) ルールのAPI機能を活用して、Adobe Experience Platform Edge Networkから[!DNL The Trade Desk]にデータを送信できます。

このドキュメントでは、拡張機能をインストールし、イベント転送[&#x200B; ルール &#x200B;](../../../ui/managing-resources/rules.md)でその機能を使用する方法について説明します。

>[!NOTE]
>
>この拡張機能とドキュメントのページは、[!DNL The Trade Desk] チームによって管理されています。 お問い合わせやアップデートのご依頼は、直接お問い合わせください。

## 前提条件 {#prerequisites}

[!DNL The Trade Desk][[!DNL The Trade Desk Real-Time Conversions API]を設定するには、関連する広告主ID、UPixel ID、およびトラッカーIDが](https://partner.thetradedesk.com/v3/portal/data/doc/DataConversionEventsApi) アカウント内から必要です。

>[!INFO]
>
>加盟店の場合は、加盟店IDも取得する必要があります。

## [!DNL The Trade Desk] Real-Time Conversions APIのインストールと設定 {#install}

拡張機能をインストールするには、[&#x200B; イベント転送プロパティ &#x200B;](../../../ui/event-forwarding/overview.md#properties)を作成するか、代わりに編集する既存のプロパティを選択します。

左側のナビゲーションの「**[!UICONTROL Extensions]**」を選択します。 「**[!UICONTROL Catalog]**」タブで、**[!UICONTROL The Trade Desk]** Real-Time Conversions API カードを選択し、**[!UICONTROL Install]**&#x200B;を選択します。

![&#x200B; インストールを強調表示する[!DNL The Trade Desk]拡張カードを示す拡張カタログ。](../../../images/extensions/server/tradedesk/install-extension.png)

次の画面で、[!UICONTROL Advertiser ID]を入力し、オプションで[!UICONTROL Merchant ID]を入力します。 IDを直接入力に貼り付けることも、代わりにデータ要素を使用することもできます。 これらは、[!DNL The Trade Desk] Real-Time Conversions APIへのイベント呼び出しを行う際に使用されるデフォルト値として機能します。 終了したら「**[!UICONTROL Save]**」を選択します。

データ要素を作成し、タグプロパティの拡張機能で利用できるようにする方法については、[&#x200B; データ要素を作成](https://experienceleague.adobe.com/ja/docs/platform-learn/data-collection/tags/create-data-elements) チュートリアルに従ってください。

![[!DNL The Trade Desk]および[!UICONTROL Advertiser ID] フィールドがハイライト表示された[!UICONTROL Merchant ID]拡張機能設定ページ。](../../../images/extensions/server/tradedesk/configure-extension.png)

拡張機能がインストールされ、イベント転送ルールでその機能を使用できるようになりました。

## イベント転送ルールの設定 {#rule}

拡張機能をインストールして設定したら、イベントの送信方法と送信日時を決定するイベント転送ルールの作成を開始できます。[!DNL The Trade Desk]

受け入れられたすべての[&#x200B; リクエストプロパティ &#x200B;](https://partner.thetradedesk.com/v3/portal/data/doc/DataConversionEventsApi#properties)を[!DNL The Trade Desk]および[!DNL The Trade Desk] Real-Time Conversions API経由で送信するには、いくつかのルールを設定することを検討してください。

>[!NOTE]
>
>イベントは、リアルタイム、またはできるだけリアルタイムに近い方法で送信する必要があります。

イベント転送プロパティに新しいイベント転送[&#x200B; ルール &#x200B;](../../../ui/managing-resources/rules.md)を作成します。 **[!UICONTROL Actions]**&#x200B;で、新しいアクションを追加し、拡張機能を&#x200B;**[!UICONTROL The Trade Desk]**&#x200B;に設定します。 次に、**[!UICONTROL Real Time Conversion]**&#x200B;の&#x200B;**[!UICONTROL Action Type]**&#x200B;を選択します。

![&#x200B; イベント転送プロパティのルール ビュー。イベント転送ルールのアクション設定を追加するために必要なフィールドがハイライト表示されています。](../../../images/extensions/server/tradedesk/tradedesk-event-action.png)

選択後、追加のコントロールが表示され、[!DNL The Trade Desk]に送信されるイベントデータをさらに設定します。 ルールを保存するには、**[!UICONTROL Keep Changes]**&#x200B;を選択します。

設定オプションは、次の3つの主なセクションに分かれています。

**[!UICONTROL Basic Request Properties]**

| 入力 | 説明 |
| --- | --- |
| トラッカーID | イベントトラッカーのプラットフォーム ID。 |
| UPixel ID | イベントのユニバーサルピクセル ID。 |
| リファラー URL | イベントが発生した場所からのweb サイト URL （存在する場合）。 |
| イベント名 | パートナープラットフォームによって定義されたイベントのタイプ。 |
| 値 | 収益トラッキング値（19.98など）。 |
| 通貨 | ISO形式の通貨コード。 |
| クライアント IP | クライアント IPv4またはIPv6 IP アドレス。 |
| 広告 ID | イベントの一意の広告ID。 |
| 広告ID タイプ | AD ID プロパティで指定された広告IDのタイプ（TDID、IDFA、AAID、DAID、NAID、IDL、EUID、またはUID2）。 |
| インプレッション | イベントが属するインプレッションの一意のIDとして機能する36文字の文字列（ダッシュを含む）。 |
| 注文ID | イベントに関連付けられた注文ID。 |
| td1-td10 | 追加のコンバージョンメタデータを提供するために使用できる、10個の順序付きカスタム動的プロパティ。 |

{style="table-layout:auto"}

![&#x200B; フィールドへのデータ入力の例を示す[!DNL Basic Request Properties] セクション。](../../../images/extensions/server/tradedesk/configure-extension-basic-request-properties.png)

[!DNL The Trade Desk] Real-Time Conversions APIが受け入れた[&#x200B; リクエストプロパティ &#x200B;](https://partner.thetradedesk.com/v3/portal/data/doc/DataConversionEventsApi#properties)について詳しくは、[!DNL The Trade Desk]開発者ドキュメントを参照してください。

**[!UICONTROL Object Request Parameters]**

詳細な情報を含むJSON オブジェクト。 縮小されたキー値入力セットを使用するか、生のJSONを提供するかを選択できます。 さらに、右側のディスク （![&#x200B; ディスクアイコン &#x200B;](/help/images/icons/database.png)）を選択すると、データ要素から動的データを取得できます。


![使用可能なフィールドを表示する[!DNL Object Request Parameters] セクション。](../../../images/extensions/server/tradedesk/configure-object-request-params.png)

[とそのプロパティについて詳しくは、](https://partner.thetradedesk.com/v3/portal/data/doc/DataConversionEventsApi#properties-items) リアルタイム コンバージョンイベント [!UICONTROL Object Request Parameters]のドキュメントを参照してください。

**[!UICONTROL Configuration Overrides]**

>[!NOTE]
>
>[!UICONTROL Configuration Overrides] フィールドを使用すると、各ルールに異なる[!DNL Advertiser ID]または[!DNL Merchant ID]を設定できます。

| 入力 | 説明 |
| --- | --- |
| 広告主 ID | このイベントが関連付けられている広告主の一意の識別子。 拡張機能の設定で指定したIDを上書きするには、別の広告主IDを指定できます。 |
| 加盟店ID | オンボーディング手順を通じて[!DNL The Trade Desk]が各販売者に与える一意のID。 拡張機能の設定で指定したIDを上書きするには、別の加盟店IDを指定できます。 |

![使用可能なフィールドを表示する[!DNL Configuration Overrides] セクション。](../../../images/extensions/server/tradedesk/configure-overrides.png)

ルールに問題がなければ、**[!UICONTROL Save to Library]**&#x200B;を選択します。 最後に、新しいイベント転送[&#x200B; ビルド &#x200B;](../../../ui/publishing/builds.md)を公開して、ライブラリに加えられた変更を有効にします。

## 次の手順

このガイドでは、[!DNL The Trade Desk] Real-Time Conversions API拡張機能を使用してサーバーサイドのイベントデータを[!DNL The Trade Desk]に送信する方法について説明しました。 ここから、キャンペーンごとに特定のコンバージョンイベントを該当する形で送信する個別のルールを作成して、統合を拡張することをお勧めします。 [!DNL Adobe Experience Platform]のイベント転送機能について詳しくは、[&#x200B; イベント転送の概要](../../../ui/event-forwarding/overview.md)を参照してください。

統合を効果的に実装する方法の詳細については、[!DNL The Trade Desk]Real-Time Conversions API[の [!DNL The Trade Desk]  ベストプラクティスに関する](https://partner.thetradedesk.com/v3/portal/data/doc/DataConversionEventsApi)のドキュメントを参照してください。

Experience Platform Debugger and Event Forwarding Monitoring Toolを使用して実装をデバッグする方法について詳しくは、[Adobe Experience Platform Debuggerの概要](../../../../debugger/home.md)および[&#x200B; イベント転送におけるアクティビティの監視](../../../ui/event-forwarding/monitoring.md)を参照してください。
