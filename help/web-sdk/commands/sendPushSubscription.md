---
title: sendPushSubscription
description: Adobe Experience Platformにプッシュ通知のサブスクリプションを登録します。
source-git-commit: 9c3f19cc2b32ab70869584b620f5a55d5b808751
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 2%

---


# `sendPushSubscription` {#send-push-subscription}

>[!AVAILABILITY]
>
> Web SDKのプッシュ通知は現在 **ベータ版** です。 機能とドキュメントは変更される場合があります。

`sendPushSubscription` コマンドは、プッシュ通知の購読をAdobe Experience Platformに登録します。 このコマンドは、ブラウザーからのプッシュ購読の詳細の取得を処理し、設定済みのデータストリームに送信します。

## 前提条件 {#prerequisites}

`sendPushSubscription` を使用する前に、次のことを確認します。

1. **設定済みのプッシュ通知**:VAPID 公開鍵を使用して [`pushNotifications`](configure/pushnotifications.md) 設定プロパティを設定します
2. **ユーザー権限**：ユーザーは、通知権限を付与されている必要があります（`Notification.permission === "granted"`）
3. **サービスワーカー**：登録されたサービスワーカーがサイトで使用可能である必要があります
4. **プッシュマネージャーのサポート**：ブラウザーはプッシュ通知をサポートし、PushManager API を使用できる必要があります

## Web SDK タグ拡張機能を使用したプッシュ購読の登録 {#register-push-subscription-tag-extension}

プッシュ購読データの送信は、Adobe Experience Platform Data Collection Tags インターフェイスのルール内でアクションとして実行されます。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL ルール]** に移動し、目的のルールを選択します。
1. [!UICONTROL &#x200B; アクション &#x200B;] で、既存のアクションを選択するか、アクションを作成します。
1. [!UICONTROL &#x200B; 拡張機能 &#x200B;] ドロップダウンフィールドを **[!UICONTROL Adobe Experience Platform Web SDK]** に設定し、[!UICONTROL &#x200B; アクションタイプ &#x200B;] を **[!UICONTROL プッシュ購読を送信]** に設定します。
1. 「**[!UICONTROL 変更を保持]**」をクリックして、公開ワークフローを実行します。

## Web SDK JavaScript ライブラリを使用したプッシュ購読の登録 {#register-push-subscription-javascript}

設定済みの Web SDK インスタンスを呼び出す際に、`sendPushSubscription` コマンドを実行します。 [`configure`](configure/overview.md) コマンドを呼び出す前に、プッシュ通知を設定して `sendPushSubscription` コマンドを呼び出してください。

```js
alloy("sendPushSubscription")
  .then(() => {
    console.log("Push subscription recorded successfully");
  })
  .catch((error) => {
    console.error("Failed to send push subscription:", error);
  });
```

## 推奨実行頻度 {#recommended-execution-frequency}

Adobe最適なプッシュ通知機能を使用するには、`sendPushSubscription` コマンドを **1 日に 1 回** 実行することをお勧めします。 これにより、次のことが保証されます。

- 購読の詳細はAdobe Experience Platformで最新の状態を維持します
- プッシュトークンまたは購読ステータスに対する変更が取り込まれます
- ユーザーのプロファイルは、最新のプッシュ通知環境設定で更新されたままになります

次のようなアプローチを使用して、これを実装できます。

```js
// Check if we've sent subscription data today
const lastSent = localStorage.getItem("alloy_push_last_sent");
const today = new Date().toDateString();

if (lastSent !== today) {
  alloy("sendPushSubscription").then(() => {
    localStorage.setItem("alloy_push_last_sent", today);
  });
}
```

## 仕組み {#how-it-works}

`sendPushSubscription` コマンドは、次のアクションを実行します。

1. **前提条件を検証**：プッシュ通知が設定され、ユーザー権限が付与されていることを確認します
2. **ID を待機**：ユーザーの ECID が使用可能になるまで待機します
3. **サブスクリプションを取得**：設定された VAPID キーを使用して、サービスワーカーからアクティブなプッシュサブスクリプションを取得します
4. **変更のチェック**：現在のサブスクリプションの詳細とキャッシュされた値（ECID + サブスクリプションの詳細）を比較します。 購読の詳細が変更されていない場合、コマンドは情報メッセージをログに記録し、ネットワークリクエストを行わずに戻ります
5. **データストリームに送信**：変更が検出された場合、は購読データを設定済みのAdobe Experience Platform データストリームに送信します
6. **キャッシュを更新**：今後の比較のために新しいサブスクリプションの詳細を保存します

## エラー処理 {#error-handling}

一般的なエラー条件とそのメッセージ：

| エラー | 原因 |
| ------- | ---- |
| `"Push notifications module is not configured. No VAPID public key was provided."` | pushNotifications 設定がないか無効 |
| `"Service workers are not supported in this browser."` | ブラウザーはサービスワーカーをサポートしていません |
| `"Push notifications are not supported in this browser."` | ブラウザーはプッシュ通知または Notification API をサポートしていません |
| `"The user has not given permission to send push notifications."` | ユーザーが通知権限を付与していません |
| `"No service worker registration was found."` | 現在の接触チャネルにサービス ワーカーが登録されていません |
| `"No VAPID public key was provided."` | 設定に VAPID 公開鍵がありません |

## データペイロード {#data-payload}

このコマンドは、プッシュ通知データを次の形式で送信します。

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

## 関連ドキュメント

- [プッシュ通知の設定](configure/pushnotifications.md)
- [Web プッシュ API の仕様 &#x200B;](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
- [&#x200B; サービスワーカー API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
