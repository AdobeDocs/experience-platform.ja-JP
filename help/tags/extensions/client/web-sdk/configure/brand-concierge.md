---
title: Brand Concierge 設定
description: Brand Concierge チャットのセッション永続性とストリームタイムアウトを設定します。
exl-id: d5c0bdf7-563d-4e0e-9b1b-71e2fa783e29
source-git-commit: 9f7464b78da9615bf6966e34eb129150a481fb5f
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 21%

---

# Brand Concierge 設定 {#brand-concierge}

>[!AVAILABILITY]
>
>Web SDK用Brand Conciergeは現在&#x200B;**ベータ版**&#x200B;です。 機能とドキュメントは変更される場合があります。

>[!CONTEXTUALHELP]
>id="platform_tags_websdk_brandconcierge"
>title="Brand Concierge"
>abstract="プロパティで Brand Concierge を使用する際の設定。"

**[!UICONTROL Brand Concierge]** セクションでは、Web SDK タグ拡張機能でBrand Concierge チャットセッションの動作を制御できます。

1. Adobe IDの資格情報を使用して[experience.adobe.com](https://experience.adobe.com)にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Extensions]**&#x200B;に移動し、[!UICONTROL Adobe Experience Platform Web SDK] カードの&#x200B;**[!UICONTROL Configure]**&#x200B;を選択します。
1. **[!UICONTROL Brand Concierge]** セクションまでスクロールします。

次のオプションがあります。

## [!UICONTROL Sticky conversation session]

セッション Cookieを使用して、ページの読み込み全体でBrand Concierge セッションを保持するチェックボックス。 このオプションはデフォルトでは無効です。 この値の設定に関するガイダンスについては、JavaScript ライブラリのドキュメントの[`conversation`](/help/collection/js/commands/configure/conversation.md)を参照してください。

## [!UICONTROL Stream timeout (seconds)]

タイムアウトエラーをトリガーする前に会話ストリームのチャンクを待機する最大時間（秒単位）。 デフォルト値は`10`秒です。

## [!UICONTROL Collect sources]

ユーザーがBrand Conciergeの会話内のリンクからページに移動した場合にソースを収集するチェックボックス。 デフォルトではオフになっています。 有効にすると、ライブラリはクエリ文字列パラメーター`adobe_brand_concierge_source`をチェックし、その値を`xdm.channel.referringSource`に入力します。
