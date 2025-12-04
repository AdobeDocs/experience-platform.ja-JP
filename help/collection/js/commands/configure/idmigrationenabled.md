---
title: idMigrationEnabled
description: Web SDKが AMCV Cookie を読み取れるようにします。
exl-id: 33b9d645-0fbe-4fe4-8847-e6f9e78557b6
source-git-commit: 2e39a7809049c199d4778a0e17eb9e0f3b1d9775
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 0%

---

# `idMigrationEnabled`

`idMigrationEnabled` プロパティを使用すると、web SDKは以前のAdobe Experience Cloud実装によって設定された AMCV Cookie を読み取ることができます。 実装を web SDKにアップグレードする場合、この設定を使用すると、現在のAdobe Experience Cloud ID サービスへの移行をよりスムーズにおこなうことができます。 この設定は、web SDKへのアップグレード時にユニーク訪問者が急増しないように役立ちます。

組織で新しい web SDKの実装を実行する場合、この設定を有効にしても、データ収集や訪問者の特定には影響しません。 すべての実装で有効にしておいても欠点はありません。

`idMigrationEnabled` コマンドを実行するときは、`configure` のブール値を設定します。 Web SDKの設定時にこのプロパティを省略した場合、デフォルトは `true` になります。 訪問者 API で設定された AMCV Cookie の読み取り機能を無効にする場合は、このプロパティを設定します。 ほとんどの組織では、このプロパティを設定する必要はありません。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  idMigrationEnabled: false
});
```

## Web SDK タグ拡張機能を使用して訪問者 ID の移行を有効にする

これらの設定は、web SDK タグ拡張機能で [ID 設定 &#x200B;](/help/tags/extensions/client/web-sdk/configure/identity.md) を使用して指定できます。
