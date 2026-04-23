---
title: pushNotifications
description: Web SDKのプッシュ通知を設定して、ブラウザーベースのプッシュメッセージを有効にします。
exl-id: a5cf4817-a4c2-4cf1-8f3a-7e92b807de8f
source-git-commit: d38cfb7d2ace7c1bb45dcb584a2cdf10063da06a
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 3%

---

# `pushNotifications` {#push-notifications}

`pushNotifications` プロパティを使用すると、web アプリケーションのプッシュ通知を設定できます。 この機能を使用すると、web サイトがブラウザーに読み込まれていない場合でも、web アプリがサーバーからプッシュされたメッセージを受信できます。

## 前提条件 {#prerequisites}

プッシュ通知を設定する前に、次のことを確認してください。

1. **ユーザー権限**: ユーザーは通知の権限を明示的に付与する必要があります
2. **Service worker**：プッシュ通知を機能させるには、登録されたService Workerが必要です
3. **VAPID キー**：安全な通信のためのVAPID （任意アプリケーションサーバー識別）キーを生成します
4. **アプリケーション ID**: Adobe Journey Optimizer内のVAPID キーを保存するときに使用されるアプリ ID -> チャネル -> プッシュ設定 – > プッシュ資格情報
5. **トラッキングデータセット ID**: 「AJO Push Tracking Experience Event Dataset」という名前のシステムデータセットのID。 Adobe Journey Optimizerから取得 – > Datasets

## VAPID キーの生成 {#generate-vapid-keys}

VAPID キーを生成するには、`web-push` NPM パッケージをインストールして実行します。

```bash
npm install web-push -g
web-push generate-vapid-keys
```

このアクションは、公開鍵と秘密鍵のペアを生成します。 Web SDK設定で公開鍵を使用し、Adobe Journey Optimizer プッシュ通知チャネル内に秘密鍵を保存します。

## Service Workerをインストールします

サービスワーカーコードは、web サイトと同じドメインから提供される必要があります。 AdobeのCDNからサービスワーカーコードをダウンロードし、独自のサーバーからJavaScript ファイルをホストします。 Web SDK サービスワーカーコードは、次のURL構造を使用して使用できます。

- **縮小**: `https://cdn1.adoberesources.net/alloy/[VERSION]/alloyServiceWorker.min.js`
- **フル**: `https://cdn1.adoberesources.net/alloy/[VERSION]/alloyServiceWorker.js`

Service Workerのインストール方法の例を次に示します。

```html
<script>
  navigator.serviceWorker.register("/alloyServiceWorker.js", { scope: "/" });
</script>
```

## 実装

`pushNotifications` コマンドの実行時に`configure` オブジェクトを設定します。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  pushNotifications: {
    vapidPublicKey: "BEl62iUYgU[...]KGP4jAQlJz",
    applicationId: "my-app-id",
    trackingDatasetId: "4dc19305cdd27e03dd9a6bbe",
  },
});
```

## プロパティ {#properties}

| プロパティ | タイプ | 必須 | 説明 |
|---|---|---|---|
| **`vapidPublicKey`** | 文字列 | ○ | プッシュサブスクリプションに使用されるVAPID公開鍵。 Base64でエンコードされた文字列である必要があります。 |
| **`applicationId`** | 文字列 | ○ | VAPID公開鍵に関連付けられたアプリケーション ID。 |
| **`trackingDatasetId`** | 文字列 | ○ | プッシュ通知トラッキングに使用されるシステムデータセット ID。 |

## 重要な検討事項 {#important-considerations}

- **セキュリティ**: プッシュサブスクリプションは、サブスクリプション中に使用される特定のVAPID公開鍵に関連付けられます。 VAPID キーを変更すると、既存のサブスクリプションは自動的に登録解除され、新しいキーで再作成されます。
- **キャッシュ**:Web SDKは、現在のECIDとサブスクリプションの詳細をキャッシュされた値と比較することで、サブスクリプションの更新を自動的に管理します。 購読データは、変更が検出されたときにのみ送信されます。
- **Service worker要件**: プッシュ通知には登録されたService Workerが必要です。 Service Workerがプッシュイベントを処理するように適切に設定されていることを確認します。

## Web SDK タグ拡張機能を使用したプッシュ通知の設定 {#configure-push-notifications-tag-extension}

このプロパティと同等のWeb SDK タグ拡張機能は、拡張機能を設定する際の[[!UICONTROL Push notifications]](/help/tags/extensions/client/web-sdk/configure/push-notifications.md) セクションです。

## 次の手順 {#next-steps}

プッシュ通知を設定したら、[sendPushSubscription](../sendpushsubscription.md) コマンドを使用して、Adobe Experience Platformにプッシュサブスクリプションを登録します。
