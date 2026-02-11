---
title: Brand Concierge設定
description: Brand Concierge チャットのセッション永続性とストリームタイムアウトを設定します。
source-git-commit: 0a45b688243b17766143b950994f0837dc0d0b48
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 11%

---

# Brand Concierge設定

>[!AVAILABILITY]
>
>Web SDK用のBrand Conciergeは現在 **ベータ版** です。 機能とドキュメントは変更される場合があります。

**[!UICONTROL Brand Concierge]** セクションでは、Web SDK タグ拡張機能でのBrand Concierge チャットセッションの動作を制御できます。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Extensions]** に移動し、**[!UICONTROL Configure]** カードで [!UICONTROL Adobe Experience Platform Web SDK] を選択します。
1. **[!UICONTROL Brand Concierge]** セクションまで下にスクロールします。

次のオプションがあります。

## [!UICONTROL Sticky conversation session]

セッション cookie を使用してページ読み込み全体でBrand Concierge セッションを保持するチェックボックス。 このオプションは、デフォルトでは無効になっています。 この値を設定する際のガイダンスについては、JavaScript ライブラリのドキュメントの [`conversation`](/help/collection/js/commands/configure/conversation.md) を参照してください。

## [!UICONTROL Stream timeout (seconds)]

タイムアウトエラーをトリガーするまでに会話ストリームのチャンクを待機する最大時間（秒単位）。 デフォルト値は `10` 秒です。
