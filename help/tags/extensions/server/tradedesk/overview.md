---
title: The Trade Desk Real-Time Conversions API 拡張機能の概要
description: Adobe Experience Platformでのイベント転送用の Trade Desk Real-Time Conversions API 拡張機能について説明します。
hide: true
hidefromtoc: true
source-git-commit: 8000bbf36e6763b8fca17c2ae0d5c2fe53bc6964
workflow-type: tm+mt
source-wordcount: '897'
ht-degree: 4%

---

# [!DNL The Trade Desk Real-Time Conversions API] 拡張機能の概要

[[!DNL The Trade Desk Real-Time Conversions API]](https://partner.thetradedesk.com/v3/portal/data/doc/DataConversionEventsApi) イベントをに送信できます [!DNL The Trade Desk] リターゲティングとアトリビューションを活用するために使用します。

を使用できます [!DNL The Trade Desk Real-Time Conversions API] Adobe Experience Platform Edge Networkからにデータを送信するための拡張機能 [!DNL The Trade Desk] の API の機能を利用する [イベント転送](../../../ui/event-forwarding/overview.md) ルール。

使用 [!DNL The Trade Desk Real-Time Conversions API] 拡張機能を使用すると、で API の機能を利用できます [イベント転送](../../../ui/event-forwarding/overview.md) にデータを送信するためのルール [!DNL The Trade Desk] Adobe Experience Platform Edge Networkから変更します。

このドキュメントでは、拡張機能をインストールし、イベント転送でその機能を使用する方法を説明します [ルール](../../../ui/managing-resources/rules.md).

>[!NOTE]
>
>この拡張機能およびドキュメントページは、で管理されています。 [!DNL The Trade Desk] チーム。 お問い合わせや更新のリクエストについては、直接ご連絡ください。

## 前提条件 {#prerequisites}

関連する広告主 ID、ピクセル ID およびトラッカー ID が内から必要です [!DNL The Trade Desk] を設定するためのアカウント [[!DNL The Trade Desk Real-Time Conversions API]](https://partner.thetradedesk.com/v3/portal/data/doc/DataConversionEventsApi).

>[!INFO]
>
>マーチャントの場合は、マーチャント ID も取得する必要があります。

## のインストールと設定 [!DNL The Trade Desk] Real-Time Conversions API {#install}

拡張機能をインストールするには、 [イベント転送プロパティの作成](../../../ui/event-forwarding/overview.md#properties) または、代わりに既存のプロパティを選択して編集します。

左側のナビゲーションの「**[!UICONTROL 拡張機能]**」をクリックします。が含まれる **[!UICONTROL カタログ]** タブで、 **[!UICONTROL Trade Desk]** Real-Time Conversions API カードを選択し、 **[!UICONTROL インストール]**.

![を示した拡張機能カタログ [!DNL The Trade Desk] 拡張機能カードのハイライト表示インストール。](../../../images/extensions/server/tradedesk/install-extension.png)

次の画面で、 [!UICONTROL 広告主 ID]、およびオプションで [!UICONTROL マーチャント ID]. ID をこれらの入力に直接貼り付けることも、代わりにデータ要素を使用することもできます。 これらは、に対してイベントを呼び出す際に使用されるデフォルト値として機能します。 [!DNL The Trade Desk] Real-Time Conversions API 完了したら、「**[!UICONTROL 保存]**」をクリックします。

データ要素を作成し、タグプロパティの拡張機能で使用できるようにする方法については、次を参照してください [データ要素の作成](https://experienceleague.adobe.com/en/docs/platform-learn/data-collection/tags/create-data-elements) チュートリアル。

![この [!DNL The Trade Desk] 拡張機能の設定ページ [!UICONTROL 広告主 ID] および [!UICONTROL マーチャント ID] ハイライト表示されたフィールド](../../../images/extensions/server/tradedesk/configure-extension.png)

拡張機能がインストールされ、イベント転送ルールでその機能を使用できるようになりました。

## イベント転送ルールの設定 {#rule}

拡張機能をインストールして設定したら、イベントの送信先と送信先を決定するイベント転送ルールの作成を開始できます [!DNL The Trade Desk].

許可されたすべてを送信するために、いくつかのルールの設定を検討する必要があります [リクエストプロパティ](https://partner.thetradedesk.com/v3/portal/data/doc/DataConversionEventsApi#properties) 経由 [!DNL The Trade Desk] および [!DNL The Trade Desk] Real-Time Conversions API

>[!NOTE]
>
>イベントは、リアルタイムで、または可能な限りリアルタイムに近いタイミングで送信する必要があります。

新しいイベント転送の作成 [ルール](../../../ui/managing-resources/rules.md) をイベント転送プロパティに追加します。 次の下 **[!UICONTROL アクション]**&#x200B;新しいアクションを追加して、拡張機能をに設定します **[!UICONTROL Trade Desk]**. 次に、を選択します **[!UICONTROL リアルタイムコンバージョン]** の場合 **[!UICONTROL アクションタイプ]**.

![イベント転送ルールのアクション設定を追加するために必要なフィールドがハイライト表示されたイベント転送プロパティルール ビュー。](../../../images/extensions/server/tradedesk/tradedesk-event-action.png)

選択後、追加的なコントロールが表示され、送信先のイベントデータをさらに設定できます [!DNL The Trade Desk]. を選択 **[!UICONTROL 変更を保持]** をクリックしてルールを保存します。

設定オプションは、以下に示すように、3 つの主なセクションに分かれています。

**[!UICONTROL 基本的なリクエストプロパティ]**

| 入力 | 説明 |
| --- | --- |
| トラッカー ID | イベントトラッカーのプラットフォーム ID。 |
| ピクセル ID | イベントの汎用ピクセル ID。 |
| リファラー URL | イベントが発生した web サイトの URL （存在する場合）。 |
| イベント名 | パートナープラットフォームによって定義されたイベントのタイプ。 |
| 値 | 10 進数の文字列での売上高トラッキング値（例：「19.98」）。 |
| 通貨 | ISO 形式の通貨コード。 |
| クライアント IP | クライアントの IPv4 または IPv6 IP アドレス。 |
| 広告 ID | イベントの一意の広告 ID。 |
| 広告 ID タイプ | 広告 ID プロパティで指定された広告 ID のタイプ：TDID、IDFA、AAID、DAID、NAID、IDL、EUID、UID2。 |
| インプレッション | イベントが関連付けられるインプレッションの一意の ID として機能する 36 文字の文字列（ダッシュを含む）。 |
| 注文 ID | イベントに関連付けられたオーダー識別子。 |
| td1-td10 | 追加のコンバージョンメタデータを提供するために使用できる、10 個の連続する番号が付いたカスタム動的プロパティ。 |

{style="table-layout:auto"}

![この [!DNL Basic Request Properties] フィールドへのデータ入力の例を示すセクション。](../../../images/extensions/server/tradedesk/configure-extension-basic-request-properties.png)

を参照してください。 [!DNL The Trade Desk] の詳細については、開発者向けドキュメントを参照してください。 [リクエストプロパティ](https://partner.thetradedesk.com/v3/portal/data/doc/DataConversionEventsApi#properties) 承認者 [!DNL The Trade Desk] Real-Time Conversions API

**[!UICONTROL オブジェクトリクエストパラメーター]**

項目、プライバシー、データ処理などの JSON 形式のリクエストパラメーターについて詳しくは、次の節を参照してください。

![この [!DNL Object Request Parameters] 使用可能なフィールドを示すセクション。](../../../images/extensions/server/tradedesk/configure-object-request-params.png)

を参照してください。 [リアルタイムコンバージョンイベント](https://partner.thetradedesk.com/v3/portal/data/doc/DataConversionEventsApi#properties-items) の詳細については、ドキュメントを参照してください。 [!UICONTROL オブジェクトリクエストパラメーター] とそのプロパティ。

**[!UICONTROL 設定の上書き]**

>メモ
>
>この [!UICONTROL 設定の上書き] フィールドには、別の値を設定できます [!DNL Advertiser ID] および/または [!DNL Merchant ID] すべてのルールで。

| 入力 | 説明 |
| --- | --- |
| 広告主 ID | 拡張機能設定で指定された広告主 ID を上書きする広告主 ID。 |
| マーチャント ID | 拡張機能の設定で指定されたマーチャント ID を上書きするマーチャント ID。 |

![この [!DNL Configuration Overrides] 使用可能なフィールドを示すセクション。](../../../images/extensions/server/tradedesk/configure-overrides.png)

ルールに満足したら、を選択します。 **[!UICONTROL ライブラリに保存]**. 最後に、新しいイベント転送を公開します [ビルド](../../../ui/publishing/builds.md) をクリックして、ライブラリに加えられた変更を有効にします。

## 次の手順

このガイドでは、サーバーサイドイベントデータをに送信する方法について説明しました。 [!DNL The Trade Desk] の使用 [!DNL The Trade Desk] Real-Time Conversions API 拡張機能。 ここから、キャンペーンごとに適用可能な特定のコンバージョンイベントを送信する個別のルールを作成して、統合を拡張することをお勧めします。 のイベント転送機能について詳しくは、こちらを参照してください [!DNL Adobe Experience Platform]、を読み取ります [イベント転送の概要](../../../ui/event-forwarding/overview.md).

を参照してください。 [!DNL The Trade Desk] のドキュメント [のベストプラクティス [!DNL The Trade Desk] Real-Time Conversions API](https://www.facebook.com/business/help/308855623839366?id=818859032317965) 統合を効果的に実装する方法に関するガイダンスが必要です。

Experience Platformデバッガーとイベント転送の監視ツールを使用した実装のデバッグ方法について詳しくは、 [Adobe Experience Platform Debuggerの概要](../../../../debugger/home.md) および [イベント転送でのアクティビティの監視](../../../ui/event-forwarding/monitoring.md).
