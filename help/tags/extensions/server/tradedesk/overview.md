---
title: The Trade Desk Real-Time Conversions API 拡張機能の概要
description: Adobe Experience Platformでのイベント転送用の Trade Desk Real-Time Conversions API 拡張機能について説明します。
exl-id: 1ff32e2b-9ff8-4395-ae44-cba75a2da515
source-git-commit: 8cf838b6f6794b52f80cb899945c066014e211c2
workflow-type: tm+mt
source-wordcount: '888'
ht-degree: 3%

---

# [!DNL The Trade Desk Real-Time Conversions API] 拡張機能の概要

[[!DNL The Trade Desk Real-Time Conversions API]](https://partner.thetradedesk.com/v3/portal/data/doc/DataConversionEventsApi) 拡張機能を使用すると、[!DNL The Trade Desk] イベント転送 [ ルールの API 機能を利用して、Adobe Experience Platform Edge Networkから ](../../../ui/event-forwarding/overview.md) にデータを送信できます。

[!DNL The Trade Desk Real-Time Conversions API] 拡張機能を使用すると、[ イベント転送 ](../../../ui/event-forwarding/overview.md) ルールで API の機能を活用して、Adobe Experience Platform Edge Networkから [!DNL The Trade Desk] にデータを送信できます。

このドキュメントでは、拡張機能をインストールし、イベント転送 [ ルール ](../../../ui/managing-resources/rules.md) でその機能を使用する方法について説明します。

>[!NOTE]
>
>この拡張機能およびドキュメントページは、チームが管理 [!DNL The Trade Desk] ています。 お問い合わせや更新のリクエストについては、直接ご連絡ください。

## 前提条件 {#prerequisites}

[!DNL The Trade Desk][[!DNL The Trade Desk Real-Time Conversions API] を設定するには、関連する広告主 ID、UPixel ID およびトラッカー ID が ](https://partner.thetradedesk.com/v3/portal/data/doc/DataConversionEventsApi) アカウント内から必要です。

>[!INFO]
>
>マーチャントの場合は、マーチャント ID も取得する必要があります。

## [!DNL The Trade Desk] Real-Time Conversions API のインストールと設定 {#install}

拡張機能をインストールするには、[ イベント転送プロパティを作成 ](../../../ui/event-forwarding/overview.md#properties) するか、代わりに編集する既存のプロパティを選択します。

左側のナビゲーションの「**[!UICONTROL Extensions]**」を選択します。 「**[!UICONTROL Catalog]**」タブで、**[!UICONTROL The Trade Desk]** Real-Time Conversions API カードを選択し、「**[!UICONTROL Install]**」を選択します。

![ インストールを強調表示した [!DNL The Trade Desk] 拡張機能カードを示す拡張機能カタログ ](../../../images/extensions/server/tradedesk/install-extension.png)

次の画面で、[!UICONTROL Advertiser ID] を入力し、オプションで [!UICONTROL Merchant ID] を入力します。 ID をこれらの入力に直接貼り付けることも、代わりにデータ要素を使用することもできます。 これらは、Real-Time Conversions API に対してイベント呼び出しを行う際に使用されるデフォルト値 [!DNL The Trade Desk] して機能します。 終了したら「**[!UICONTROL Save]**」を選択します。

データ要素を作成し、タグプロパティの拡張機能で使用できるようにする方法については、[ データ要素の作成 ](https://experienceleague.adobe.com/en/docs/platform-learn/data-collection/tags/create-data-elements) チュートリアルに従ってください。

![[!DNL The Trade Desk] と [!UICONTROL Advertiser ID] のフィールドがハイライト表示された [!UICONTROL Merchant ID] 拡張機能の設定ページ ](../../../images/extensions/server/tradedesk/configure-extension.png)

拡張機能がインストールされ、イベント転送ルールでその機能を使用できるようになりました。

## イベント転送ルールの設定 {#rule}

拡張機能をインストールして設定したら、イベントを [!DNL The Trade Desk] に送信する方法とタイミングを決定するイベント転送ルールの作成を開始できます。

受け入れ可能なすべての [ リクエストプロパティ ](https://partner.thetradedesk.com/v3/portal/data/doc/DataConversionEventsApi#properties) を [!DNL The Trade Desk] および Real-Time Conversions API を介して送信するには、いくつかのルールの設定 [!DNL The Trade Desk] 検討する必要があります。

>[!NOTE]
>
>イベントは、リアルタイムで、または可能な限りリアルタイムに近いタイミングで送信する必要があります。

イベント転送プロパティに新しいイベント転送 [ ルール ](../../../ui/managing-resources/rules.md) を作成します。 **[!UICONTROL Actions]** の下で、新しいアクションを追加し、拡張機能を **[!UICONTROL The Trade Desk]** に設定します。 次に、**[!UICONTROL Real Time Conversion]** の **[!UICONTROL Action Type]** を選択します。

![ イベント転送ルールのアクション設定を追加するために必要なフィールドがハイライト表示されたイベント転送プロパティルール ビュー。](../../../images/extensions/server/tradedesk/tradedesk-event-action.png)

選択後、[!DNL The Trade Desk] に送信されるイベントデータをさらに設定するための追加のコントロールが表示されます。 「**[!UICONTROL Keep Changes]**」を選択して、ルールを保存します。

設定オプションは、以下に示すように、3 つの主なセクションに分かれています。

**[!UICONTROL Basic Request Properties]**

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

![ フィールドへのデータ入力の例を示す [!DNL Basic Request Properties] の節。](../../../images/extensions/server/tradedesk/configure-extension-basic-request-properties.png)

Real-Time Conversions API によって受け入れられる [!DNL The Trade Desk] リクエストプロパティ [ について詳しくは、](https://partner.thetradedesk.com/v3/portal/data/doc/DataConversionEventsApi#properties) 開発者向けドキュメント [!DNL The Trade Desk] 参照してください。

**[!UICONTROL Object Request Parameters]**

詳細情報を含む JSON オブジェクト。 キー値の入力の縮小セットを使用するか、生の JSON を提供するかを選択できます。 さらに、右側のディスク（![ ディスクアイコン ](/help/images/icons/database.png)）を選択すると、データ要素から動的データを取得できます。


![ 使用可能なフィールドを示す [!DNL Object Request Parameters] のセクション。](../../../images/extensions/server/tradedesk/configure-object-request-params.png)

コンバージョンとそのプロパティについて詳しくは、[ リアルタイムコンバージョン ](https://partner.thetradedesk.com/v3/portal/data/doc/DataConversionEventsApi#properties-items) ベント [!UICONTROL Object Request Parameters] ドキュメントを参照してください。

**[!UICONTROL Configuration Overrides]**

>[!NOTE]
>
>[!UICONTROL Configuration Overrides] のフィールドを使用すると、ルールごとに異なる [!DNL Advertiser ID] や [!DNL Merchant ID] を設定できます。

| 入力 | 説明 |
| --- | --- |
| 広告主 ID | このイベントが関連付けられている広告主の一意の ID。 別の広告主 ID を指定して、拡張機能の設定で指定した ID を上書きできます。 |
| マーチャント ID | オンボーディング手順を通じて各マーチャントが [!DNL The Trade Desk] によって付与される一意の ID。 拡張機能の設定で指定した ID を上書きするために、別のマーチャント ID を指定できます。 |

![ 使用可能なフィールドを示す [!DNL Configuration Overrides] のセクション。](../../../images/extensions/server/tradedesk/configure-overrides.png)

ルールの設定が完了したら、「**[!UICONTROL Save to Library]**」を選択します。 最後に、新しいイベント転送 [ ビルド ](../../../ui/publishing/builds.md) を公開して、ライブラリに加えられた変更を有効にします。

## 次の手順

このガイドでは、[!DNL The Trade Desk] Real-Time Conversions API 拡張機能を使用してサーバーサイドイベントデータを [!DNL The Trade Desk] に送信する方法について説明しました。 ここから、キャンペーンごとに適用可能な特定のコンバージョンイベントを送信する個別のルールを作成して、統合を拡張することをお勧めします。 [!DNL Adobe Experience Platform] のイベント転送機能について詳しくは、[ イベント転送の概要 ](../../../ui/event-forwarding/overview.md) を参照してください。

統合を効果的に実装する方法について詳しくは、[!DNL The Trade Desk]Real[Time Conversions API のベストプラクティス  [!DNL The Trade Desk]  に関する ](https://partner.thetradedesk.com/v3/portal/data/doc/DataConversionEventsApi) ドキュメントを参照してください。

Experience Platform Debugger とイベント転送の監視ツールを使用した実装のデバッグ方法について詳しくは、[Adobe Experience Platform Debuggerの概要 ](../../../../debugger/home.md) および [ イベント転送でのアクティビティの監視 ](../../../ui/event-forwarding/monitoring.md) を参照してください。
