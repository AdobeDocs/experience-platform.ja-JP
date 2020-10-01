---
title: Adobe Experience PlatformウェブSDKリリースノート
seo-title: Adobe Experience PlatformウェブSDKリリースノート
description: Adobe Experience PlatformWeb SDKのリリースノート
seo-description: Adobe Experience PlatformWeb SDKのリリースノート
keywords: Adobe Experience Platform Web SDK;Platform Web SDK;Web SDK;release notes;
translation-type: tm+mt
source-git-commit: 738dfe782ee7d6bef06d14910e0c26540b0ec734
workflow-type: tm+mt
source-wordcount: '108'
ht-degree: 3%

---


# リリースノート

## バージョン 2.2.0

* バグ修正：オプトインオブジェクトが、Allyがを呼び出すのをブロックし `idMigrationEnabled` ていました `true`。
* バグ修正：ちらつきの問題を防ぐために、パーソナライズオファーが返される必要がある要求をAllyに認識させます。

## バージョン 2.1.0

* コマンドを削除し、 `syncIdentity` コマンドにこれらのIDを渡すことをサポート `sendEvent` します。
* IAB 2.0同意基準をサポートします。
* コマンドに追加のIDを渡す機能をサポートし `setConsent` ます。
* コマンドでのの上書き `datasetId` をサポートし `sendEvent` ます。
* サポートアロイモニタ([詳細情報](https://github.com/adobe/alloy/wiki/Monitoring-Hooks))
* 実装 `environment: browser` の詳細コンテキストデータを渡します。
