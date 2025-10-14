---
title: The Trade Desk Real-Time Conversions API 拡張機能の概要
description: Adobe Experience Platformでのイベント転送用の Trade Desk Real-Time Conversions API 拡張機能について説明します。
exl-id: 1ff32e2b-9ff8-4395-ae44-cba75a2da515
source-git-commit: eb650da02ac69c5afbebfe6ba371cc19617f79d0
workflow-type: tm+mt
source-wordcount: '930'
ht-degree: 3%

---

# [!DNL The Trade Desk Real-Time Conversions API] 拡張機能の概要

[[!DNL The Trade Desk Real-Time Conversions API]](https://partner.thetradedesk.com/v3/portal/data/doc/DataConversionEventsApi) 拡張機能を使用すると、[&#x200B; イベント転送 &#x200B;](../../../ui/event-forwarding/overview.md) ルールの API 機能を利用して、Adobe Experience Platform Edge Networkから [!DNL The Trade Desk] にデータを送信できます。

[!DNL The Trade Desk Real-Time Conversions API] 拡張機能を使用すると、[&#x200B; イベント転送 &#x200B;](../../../ui/event-forwarding/overview.md) ルールで API を活用して、Adobe Experience Platform Edge Networkから [!DNL The Trade Desk] にデータを送信できます。

このドキュメントでは、拡張機能をインストールし、イベント転送 [&#x200B; ルール &#x200B;](../../../ui/managing-resources/rules.md) でその機能を使用する方法について説明します。

>[!NOTE]
>
>この拡張機能およびドキュメントページは、チームが管理 [!DNL The Trade Desk] ています。 お問い合わせや更新のリクエストについては、直接ご連絡ください。

## 前提条件 {#prerequisites}

[[!DNL The Trade Desk Real-Time Conversions API]](https://partner.thetradedesk.com/v3/portal/data/doc/DataConversionEventsApi) を設定するには、関連する広告主 ID、UPixel ID およびトラッカー ID が [!DNL The Trade Desk] アカウント内から必要です。

>[!INFO]
>
>マーチャントの場合は、マーチャント ID も取得する必要があります。

## [!DNL The Trade Desk] Real-Time Conversions API のインストールと設定 {#install}

拡張機能をインストールするには、[&#x200B; イベント転送プロパティを作成 &#x200B;](../../../ui/event-forwarding/overview.md#properties) するか、代わりに編集する既存のプロパティを選択します。

左側のナビゲーションの「**[!UICONTROL 拡張機能]**」をクリックします。「**[!UICONTROL カタログ]**」タブで **[!UICONTROL The Trade Desk]** Real-time Conversions API カードを選択し、「**[!UICONTROL インストール]**」を選択します。

![&#x200B; インストールを強調表示した [!DNL The Trade Desk] 拡張機能カードを示す拡張機能カタログ &#x200B;](../../../images/extensions/server/tradedesk/install-extension.png)

次の画面で、[!UICONTROL &#x200B; 広告主 ID] と、オプションで [!UICONTROL &#x200B; マーチャント ID] を入力します。 ID をこれらの入力に直接貼り付けることも、代わりにデータ要素を使用することもできます。 これらは、Real-Time Conversions API に対してイベント呼び出しを行う際に使用されるデフォルト値 [!DNL The Trade Desk] して機能します。 完了したら、「**[!UICONTROL 保存]**」をクリックします。

データ要素を作成し、タグプロパティの拡張機能で使用できるようにする方法については、[&#x200B; データ要素の作成 &#x200B;](https://experienceleague.adobe.com/ja/docs/platform-learn/data-collection/tags/create-data-elements) チュートリアルに従ってください。

![ 「[!DNL The Trade Desk] 広告主 ID フィールドと「マーチャント ID] フィールドがハイライト表示された [[!UICONTROL &#x200B; 広告拡張機能設定ページ &#x200B;]](../../../images/extensions/server/tradedesk/configure-extension.png)

拡張機能がインストールされ、イベント転送ルールでその機能を使用できるようになりました。

## イベント転送ルールの設定 {#rule}

拡張機能をインストールして設定したら、イベントを [!DNL The Trade Desk] に送信する方法とタイミングを決定するイベント転送ルールの作成を開始できます。

受け入れ可能なすべての [&#x200B; リクエストプロパティ &#x200B;](https://partner.thetradedesk.com/v3/portal/data/doc/DataConversionEventsApi#properties) を [!DNL The Trade Desk] および Real-Time Conversions API を介して送信するには、いくつかのルールの設定 [!DNL The Trade Desk] 検討する必要があります。

>[!NOTE]
>
>イベントは、リアルタイムで、または可能な限りリアルタイムに近いタイミングで送信する必要があります。

イベント転送プロパティに新しいイベント転送 [&#x200B; ルール &#x200B;](../../../ui/managing-resources/rules.md) を作成します。 **[!UICONTROL アクション]** で、新しいアクションを追加し、拡張機能を **[!UICONTROL The Trade Desk]** に設定します。 次に、**[!UICONTROL アクションタイプ]** で「**[!UICONTROL リアルタイムコンバージョン]**」を選択します。

![&#x200B; イベント転送ルールのアクション設定を追加するために必要なフィールドがハイライト表示されたイベント転送プロパティルール ビュー。](../../../images/extensions/server/tradedesk/tradedesk-event-action.png)

選択後、[!DNL The Trade Desk] に送信されるイベントデータをさらに設定するための追加のコントロールが表示されます。 「**[!UICONTROL 変更を保持]**」を選択して、ルールを保存します。

設定オプションは、以下に示すように、3 つの主なセクションに分かれています。

**[!UICONTROL 基本的なリクエストのプロパティ]**

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
| 広告 ID タイプ | 広告 ID プロパティで指定された広告 ID のタイプ（TDID、IDFA、AAID、DAID、NAID、IDL、EUID、UID2）。 |
| インプレッション | イベントが関連付けられるインプレッションの一意の ID として機能する 36 文字の文字列（ダッシュを含む）。 |
| 注文 ID | イベントに関連付けられたオーダー識別子。 |
| td1-td10 | 追加のコンバージョンメタデータを提供するために使用できる、10 個の連続する番号が付いたカスタム動的プロパティ。 |

{style="table-layout:auto"}

![&#x200B; フィールドへのデータ入力の例を示す [!DNL Basic Request Properties] の節。](../../../images/extensions/server/tradedesk/configure-extension-basic-request-properties.png)

Real-Time Conversions API によって受け入れられる [&#x200B; リクエストプロパティ &#x200B;](https://partner.thetradedesk.com/v3/portal/data/doc/DataConversionEventsApi#properties) について詳しくは、[!DNL The Trade Desk] 開発者向けドキュメント [!DNL The Trade Desk] 参照してください。

**[!UICONTROL オブジェクトリクエストパラメーター]**

詳細情報を含む JSON オブジェクト。 キー値の入力の縮小セットを使用するか、生の JSON を提供するかを選択できます。 さらに、右側のディスク（![&#x200B; ディスクアイコン &#x200B;](/help/images/icons/database.png)）を選択すると、データ要素から動的データを取得できます。


![&#x200B; 使用可能なフィールドを示す [!DNL Object Request Parameters] のセクション。](../../../images/extensions/server/tradedesk/configure-object-request-params.png)

[&#x200B; オブジェクトリクエストパラメーター &#x200B;](https://partner.thetradedesk.com/v3/portal/data/doc/DataConversionEventsApi#properties-items) とそのプロパティについて詳しくは、[!UICONTROL &#x200B; リアルタイムコンバージョンイベント &#x200B;] ドキュメントを参照してください。

**[!UICONTROL 設定の上書き]**

>[!NOTE]
>
>[!UICONTROL &#x200B; 設定の上書き &#x200B;] フィールドを使用すると、ルールごとに異なる [!DNL Advertiser ID] や [!DNL Merchant ID] を設定できます。

| 入力 | 説明 |
| --- | --- |
| 広告主 ID | このイベントが関連付けられている広告主の一意の ID。 別の広告主 ID を指定して、拡張機能の設定で指定した ID を上書きできます。 |
| マーチャント ID | オンボーディング手順を通じて各マーチャントが [!DNL The Trade Desk] によって付与される一意の ID。 拡張機能の設定で指定した ID を上書きするために、別のマーチャント ID を指定できます。 |

![&#x200B; 使用可能なフィールドを示す [!DNL Configuration Overrides] のセクション。](../../../images/extensions/server/tradedesk/configure-overrides.png)

ルールの設定が完了したら、「**[!UICONTROL ライブラリに保存]**」を選択します。 最後に、新しいイベント転送 [&#x200B; ビルド &#x200B;](../../../ui/publishing/builds.md) を公開して、ライブラリに加えられた変更を有効にします。

## 次の手順

このガイドでは、[!DNL The Trade Desk] Real-Time Conversions API 拡張機能を使用してサーバーサイドイベントデータを [!DNL The Trade Desk] に送信する方法について説明しました。 ここから、キャンペーンごとに適用可能な特定のコンバージョンイベントを送信する個別のルールを作成して、統合を拡張することをお勧めします。 [!DNL Adobe Experience Platform] のイベント転送機能について詳しくは、[&#x200B; イベント転送の概要 &#x200B;](../../../ui/event-forwarding/overview.md) を参照してください。

統合を効果的に実装する方法について詳しくは、[Real [!DNL The Trade Desk] Time Conversions API のベストプラクティス &#x200B;](https://www.facebook.com/business/help/308855623839366?id=818859032317965) に関する [!DNL The Trade Desk] ドキュメントを参照してください。

イベント デバッガーとExperience Platform転送モニタリング ツールを使用して実装をデバッグする方法について詳しくは、[Adobe Experience Platform Debuggerの概要 &#x200B;](../../../../debugger/home.md) および [&#x200B; イベント転送でのアクティビティの監視 &#x200B;](../../../ui/event-forwarding/monitoring.md) を参照してください。
