---
title: プッシュ通知設定
description: Web SDK タグ拡張機能のプッシュ通知を設定します。
exl-id: 96ab7ea8-7180-46bb-9c15-eecba2009c52
source-git-commit: 6c05d8abde0e4d6b07fe37d6e3eacd5d3dd67ec2
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 6%

---

# プッシュ通知設定 {#push-notifications}

>[!CONTEXTUALHELP]
>id="platform_tags_websdk_pushnotifications"
>title="プッシュ通知"
>abstract="プッシュ通知認証用の VAPID 公開鍵を設定します。"

>[!AVAILABILITY]
>
>Web SDKのプッシュ通知は現在 **ベータ版** です。 機能とドキュメントは変更される場合があります。

この設定セクションでは、プッシュ通知認証用の VAPID 公開鍵を設定できます。

>[!NOTE]
>
>この機能は、まず [ カスタムビルドコンポーネント ](custom-build-components.md) を使用して有効にする必要があります。デフォルトでは無効になっています。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Extensions]** に移動して、**[!UICONTROL Configure]** カードの [!UICONTROL Adobe Experience Platform Web SDK] をクリックします。
1. 「**[!UICONTROL Custom build components]**」を展開し、「**[!UICONTROL Push notifications]**」を有効にします。
1. [!UICONTROL SDK instances] の下で、下にスクロールして [!UICONTROL Push Notifications] セクションを探します。
1. VAPID 公開鍵を「**[!UICONTROL VAPID Public Key]**」フィールドに入力します。

![Web SDK タグ拡張機能を使用したプッシュ通知の設定を示す画像 ](../assets/push-notifications.png)

次のフィールドを使用できます。

## [!UICONTROL VAPID public key]

プッシュ購読に使用される VAPID 公開鍵。 Base64 でエンコードされた文字列です。

## [!UICONTROL Application ID]

VAPID 公開鍵に関連付けられたアプリケーション ID。

## [!UICONTROL Tracking dataset ID]

プッシュ通知トラッキングおよび分析用のデータセット ID。

## JavaScript ライブラリを使用したプッシュ通知

このセクションは、JavaScript ライブラリを設定する際のタグの [`pushNotifications`](/help/collection/js/commands/configure/pushnotifications.md) に相当します。 また、このリンク先ページには、前提条件と VAPID 公開鍵の生成に関する情報も含まれています。
