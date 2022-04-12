---
title: タグ整合性テストリファレンス
description: Adobe Experience Platform Debugger で Auditor の機能をテストしてタグの整合性を確認する方法について説明します。
exl-id: 642b0c49-a7c7-4142-8189-67f00ed50015
source-git-commit: df1a67e4b6f3d2eaeaba2b8d3c9b1588ee0b1461
workflow-type: tm+mt
source-wordcount: '126'
ht-degree: 44%

---

# タグの整合性テストの参照

このリファレンスでは、Adobe Experience Platform Debugger の Auditor 機能でタグの整合性をテストする方法に関する詳細を提供します。

>[!NOTE]
>
>Platform Debugger での監査テストについて詳しくは、 [auditor 機能の概要](./overview.md).

タグの整合性テストでは、スキャンされたすべてのページで矛盾を探します。 これらの値や設定は、正確なデータ収集をおこなうために、サイト上のすべてのページで同じにする必要があります。

| テスト | 重み付け | 条件 | 推奨 |
| --- | --- | --- | --- |
| Adobe Analytics — コードバージョンが一貫している | 5 | 複数のバージョンの Analytics コードが見つかりました。 | Analytics のすべてのインスタンスを最新のバージョンに置き換えてください。<br><br>[追加情報](https://experienceleague.adobe.com/docs/analytics/implementation/home.html?lang=ja) |

{style=&quot;table-layout:auto&quot;}
