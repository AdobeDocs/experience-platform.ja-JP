---
title: タグの一貫性テストリファレンス
description: auditor 機能がAdobe Experience Platform Debuggerでタグの一貫性をテストする方法について説明します。
exl-id: 642b0c49-a7c7-4142-8189-67f00ed50015
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '119'
ht-degree: 40%

---

# タグの一貫性テストリファレンス

このリファレンスでは、Adobe Experience Platform Debuggerの auditor 機能によるタグの一貫性のテスト方法について詳しく説明します。

>[!NOTE]
>
>Experience Platform Debugger の auditor テストについて詳しくは、[auditor 機能の概要 ](./overview.md) を参照してください。

タグの整合性テストでは、スキャンされたすべてのページで不整合が見られます。 これらの値や設定は、正確なデータ収集をおこなうために、サイト上のすべてのページで同じにする必要があります。

| テスト | 重み | 条件 | レコメンデーション |
| --- | --- | --- | --- |
| Adobe Analytics – 一貫したコードバージョン | 5 | 複数のバージョンの Analytics コードが見つかりました。 | Analytics のすべてのインスタンスを最新のバージョンに置き換えてください。<br><br>[追加情報](https://experienceleague.adobe.com/docs/analytics/implementation/home.html?lang=ja) |

{style="table-layout:auto"}
