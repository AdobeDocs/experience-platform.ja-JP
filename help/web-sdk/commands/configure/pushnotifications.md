---
title: pushNotification
description: Web SDKのプッシュ通知を設定して、ブラウザーベースのプッシュメッセージを有効にします。
source-git-commit: 9c3f19cc2b32ab70869584b620f5a55d5b808751
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 2%

---


# `pushNotifications` {#push-notifications}

>[!AVAILABILITY]
>
> Web SDKのプッシュ通知は現在 **ベータ版** です。 機能とドキュメントは変更される場合があります。

`pushNotifications` プロパティを使用すると、web アプリケーションのプッシュ通知を設定できます。 この機能を使用すると、web サイトが現在ブラウザーに読み込まれていない場合やブラウザーが実行されていない場合でも、web アプリはサーバーからプッシュされたメッセージを受信できます。

## 前提条件 {#prerequisites}

プッシュ通知を設定する前に、次のことを確認します。

1. **ユーザー権限**：ユーザーは、通知の権限を明示的に付与する必要があります
2. **サービスワーカー**：プッシュ通知が機能するには、登録済みのサービスワーカーが必要です
3. **VAPID キー**：安全な通信のための VAPID （任意申請サーバー ID）キーを生成します

## VAPID キーの生成 {#generate-vapid-keys}

VAPID キーを生成するには、`web-push` の NPM パッケージをインストールして実行します。

```bash
npm install web-push -g
web-push generate-vapid-keys
```

これにより、公開鍵と秘密鍵のペアが生成されます。 Web SDK設定で公開鍵を使用し、その秘密鍵をAdobe Journey Optimizer プッシュ通知チャネル内に保存します。

## Web SDK タグ拡張機能を使用してプッシュ通知を設定します {#configure-push-notifications-tag-extension}

プッシュ通知を有効にして設定するには、次の手順に従います。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL 拡張機能]** に移動し、**[!UICONTROL Adobe Experience Platform Web SDK]** カードの [!UICONTROL &#x200B; 設定 &#x200B;] をクリックします。
1. 「カスタムビルドコンポーネント **セクションの** プッシュ通知を有効にする」を選択します。
1. 下にスクロールして、「プッシュ通知 [!UICONTROL &#x200B; セクションを見つけ &#x200B;] す。
1. VAPID 公開鍵を「**[!UICONTROL VAPID 公開鍵]**」フィールドに入力します。
1. 「**[!UICONTROL 保存]**」をクリックして、変更を公開します。

>[!NOTE]
>
> プッシュ通知は、タグ拡張機能の設定で明示的に有効にする必要があります。 この機能はデフォルトで無効になっています。

## Web SDK JavaScript ライブラリを使用してプッシュ通知を設定します {#configure-push-notifications-javascript}

`pushNotifications` コマンドの実行時に `configure` オブジェクトを設定します。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  pushNotifications: {
    vapidPublicKey:
      "BEl62iUYgUivElbkzaBgNL3r3vOAhvJyFXjS6FjjRRojYD4NElJkLBJKZvS3xAAh4_gE3WnMaZNu_KGP4jAQlJz",
  },
});
```

## プロパティ {#properties}

| プロパティ | タイプ | 必須 | 説明 |
| ------ | ------ | -------- | ----- |
| `vapidPublicKey` | 文字列 | ○ | プッシュ購読に使用される VAPID 公開鍵。 Base64 でエンコードされた文字列である必要があります。 |

## 重要な考慮事項 {#important-considerations}

- **セキュリティ**：プッシュ購読は、購読中に使用される特定の VAPID 公開鍵に関連付けられます。 VAPID キーを変更すると、既存の購読が自動的に購読解除され、新しいキーで再作成されます。
- **キャッシュ**:Web SDKは、現在の ECID と購読の詳細をキャッシュされた値と比較することで、購読の更新を自動的に管理します。 購読データは、変更が検出された場合にのみ送信されます。
- **サービスワーカー要件**：プッシュ通知には、登録済みのサービスワーカーが必要です。 プッシュイベントを処理するようにサービスワーカーが正しく設定されていることを確認します。

## 次の手順 {#next-steps}

プッシュ通知を設定したら、[`sendPushSubscription`](../sendPushSubscription.md) コマンドを使用してプッシュ購読をAdobe Experience Platformに登録します。
