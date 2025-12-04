---
title: targetMigrationEnabled
description: Web SDKがAdobe Target Cookie の読み取りおよび書き込みを行えるようにします。
exl-id: 4b9203c6-31b7-45af-a6a6-a206d7edac3f
source-git-commit: dade74bf4a5cfd7b60eaf216583deb67b93a61db
workflow-type: tm+mt
source-wordcount: '124'
ht-degree: 0%

---

# `targetMigrationEnabled`

`targetMigrationEnabled` プロパティは、Adobe Target 1.x および 2.x ライブラリが使用する [`mbox` および `mboxEdgeCluster` Cookie の読み取りと書き込みを &#x200B;](https://experienceleague.adobe.com/en/docs/core-services/interface/data-collection/cookies/web-sdk)Web SDKで許可するブール値です。 このオプションを使用すると、以前のAdobe Target実装を使用したページと、web SDKを使用したページとの間で訪問者プロファイルを保持できます。

`targetMigrationEnabled` コマンドを実行するときは、`configure` のブール値を設定します。 Web SDKの設定時にこのプロパティを省略した場合、デフォルトは `false` になります。 一部のページでAdobe Target 1.x または 2.x ライブラリが使用されている場合は、この値を `true` に設定します。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  targetMigrationEnabled: true
});
```

## Web SDK タグ拡張機能を使用した Target 移行の有効化

この設定は、[Personalization configuration settings](/help/tags/extensions/client/web-sdk/configure/personalization.md#migrate-target-from-atjs-to-the-web-sdk) を使用して、web SDK タグ拡張機能で設定できます。
