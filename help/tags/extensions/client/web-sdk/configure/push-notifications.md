---
title: プッシュ通知設定
description: Web SDK タグ拡張機能のプッシュ通知の設定。
exl-id: 96ab7ea8-7180-46bb-9c15-eecba2009c52
source-git-commit: d38cfb7d2ace7c1bb45dcb584a2cdf10063da06a
workflow-type: tm+mt
source-wordcount: '178'
ht-degree: 11%

---

# プッシュ通知設定 {#push-notifications}

>[!CONTEXTUALHELP]
>id="platform_tags_websdk_pushnotifications"
>title="プッシュ通知"
>abstract="プッシュ通知認証用の VAPID 公開鍵を設定します。"

この設定セクションでは、プッシュ通知認証用にVAPID公開鍵を設定できます。

>[!NOTE]
>
>この機能は、最初に[ カスタムビルドコンポーネント ](custom-build-components.md)を使用して有効にする必要があります。デフォルトでは無効になっています。

1. Adobe IDの資格情報を使用して[experience.adobe.com](https://experience.adobe.com)にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Extensions]**&#x200B;に移動し、**[!UICONTROL Configure]** カードの[!UICONTROL Adobe Experience Platform Web SDK]をクリックします。
1. **[!UICONTROL Custom build components]**&#x200B;を展開し、**[!UICONTROL Push notifications]**&#x200B;を有効にします。
1. [!UICONTROL SDK instances]で、下にスクロールして[!UICONTROL Push Notifications] セクションを見つけます。
1. **[!UICONTROL VAPID Public Key]** フィールドにVAPID公開鍵を入力します。

![Web SDK タグ拡張機能を使用したプッシュ通知の設定を示す画像](../assets/push-notifications.png)

次のフィールドを使用できます。

## [!UICONTROL VAPID public key]

プッシュサブスクリプションに使用されるVAPID公開鍵。 これはBase64でエンコードされた文字列です。

## [!UICONTROL Application ID]

VAPID公開鍵に関連付けられたアプリケーション ID。

## [!UICONTROL Tracking dataset ID]

プッシュ通知のトラッキングと分析のデータセット ID。

## JavaScript ライブラリを使用したプッシュ通知

このセクションは、JavaScript ライブラリを設定する際の[`pushNotifications`](/help/collection/js/commands/configure/pushnotifications.md)に相当するタグです。 リンクされたページには、前提条件とVAPID公開鍵の生成に関する情報も表示されます。
