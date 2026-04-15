---
title: sendPushSubscription
description: Adobe Experience Platformでプッシュ通知のサブスクリプションを登録します。
exl-id: 7cb13834-46f4-481c-bd9d-600083eb6cfb
source-git-commit: 76ba5719bd922c4ff9bff6fda4a359b18f549c5e
workflow-type: tm+mt
source-wordcount: '398'
ht-degree: 3%

---

# `sendPushSubscription` {#send-push-subscription}

`sendPushSubscription` コマンドは、プッシュ通知のサブスクリプションをAdobe Experience Platformに登録します。 このコマンドは、ブラウザーからプッシュサブスクリプションの詳細を取得し、設定されたデータストリームに送信します。 Web SDK バージョン 2.29.0以降で使用できます。

## 前提条件 {#prerequisites}

`sendPushSubscription`を使用する前に、次の条件を満たしていることを確認してください。

1. **設定されたプッシュ通知**: VAPID公開鍵で[`pushNotifications`](configure/pushnotifications.md)設定プロパティを設定します
2. **ユーザー権限**: ユーザーが通知権限（`Notification.permission === "granted"`）を付与している必要があります
3. **Service worker**：登録されたService Workerがサイトで利用できる必要があります
4. **プッシュマネージャーのサポート**: ブラウザーはプッシュ通知をサポートし、PushManager APIを使用可能にする必要があります

Web SDKの設定済みインスタンスを呼び出す際に、`sendPushSubscription` コマンドを実行します。 [`configure`](configure/overview.md) コマンドを呼び出す前に、プッシュ通知が設定された`sendPushSubscription` コマンドを呼び出していることを確認してください。

```js
alloy("sendPushSubscription")
  .then(() => {
    console.log("Push subscription recorded successfully");
  })
  .catch((error) => {
    console.error("Failed to send push subscription:", error);
  });
```

## 推奨される実行頻度 {#recommended-execution-frequency}

最適なプッシュ通知機能を使用するには、Adobeで`sendPushSubscription` コマンド **を1日1回実行することをお勧めします**。 この頻度により、次のことが可能になります。

* サブスクリプションの詳細はAdobe Experience Platformに残ります
* プッシュトークンやサブスクリプションステータスに対する変更はすべて
* ユーザーのプロファイルは、最新のプッシュ通知の環境設定で常に更新されます

次のようなアプローチを使用して、これを実装できます。

```js
// Check if subscription data was sent today
const lastSent = localStorage.getItem("alloy_push_last_sent");
const today = new Date().toDateString();

if (lastSent !== today) {
  alloy("sendPushSubscription").then(() => {
    localStorage.setItem("alloy_push_last_sent", today);
  });
}
```

## 仕組み {#how-it-works}

`sendPushSubscription` コマンドは次のアクションを実行します。

1. **前提条件を検証**: プッシュ通知が設定され、ユーザー権限が付与されていることを確認します
2. **IDを待機**: ユーザーのECIDが使用可能になるまで待機します
3. **サブスクリプションを取得**：設定されたVAPID キーを使用して、Service Workerからアクティブなプッシュ サブスクリプションを取得します
4. **変更を確認します**：現在のサブスクリプションの詳細とキャッシュされた値（ECID + サブスクリプションの詳細）を比較します。 サブスクリプションの詳細が変更されていない場合、コマンドは情報メッセージをログに記録し、ネットワークリクエストを行わずに返します
5. **データストリームに送信**：変更が検出された場合、設定したAdobe Experience Platform データストリームにサブスクリプションデータを送信します
6. **キャッシュを更新**：今後の比較用に新しいサブスクリプションの詳細を保存します

## エラー処理 {#error-handling}

一般的なエラー条件とそのメッセージ：

| エラー | 原因 |
| ------- | ---- |
| `"Push notifications module is not configured. No VAPID public key was provided."` | pushNotifications設定が見つからないか無効です |
| `"Service workers are not supported in this browser."` | ブラウザーはService Workerをサポートしていません |
| `"Push notifications are not supported in this browser."` | ブラウザーはプッシュ通知または通知APIをサポートしていません |
| `"The user has not given permission to send push notifications."` | ユーザーが通知の権限を付与していません（`Notification.permission === "granted"`） |
| `"No service worker registration was found."` | 現在のオリジンに登録されているサービスワーカーはありません |
| `"No VAPID public key was provided."` | VAPID公開鍵が設定に見つかりません |

## データペイロード {#data-payload}

このコマンドは、次の形式でプッシュ通知データを送信します。

```js
{
  pushNotificationDetails: [
    {
      appID: "example.com", // Current domain
      token: "...", // Serialized subscription details + ECID
      platform: "web", // Always "web" for Web SDK
      denylisted: false, // Always false
      identity: {
        namespace: {
          code: "ECID",
        },
        id: "12345678901234567890", // User's ECID
      },
    },
  ],
}
```

## Web SDK タグ拡張機能を使用したプッシュ通知の登録 {#register-push-subscription-tag-extension}

このフィールドと同等のWeb SDK タグ拡張機能は、タグルール内で[[!UICONTROL Send Push Subscription]](/help/tags/extensions/client/web-sdk/actions/send-push-subscription.md) アクションを使用しています。

>[!MORELIKETHIS]
>
>* [プッシュ通知の設定](configure/pushnotifications.md)
>* [Web プッシュ API仕様](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
>* [Service Worker API](https://developer.mozilla.org/ja-JP/docs/Web/API/Service_Worker_API)
