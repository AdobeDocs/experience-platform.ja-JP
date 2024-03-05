---
title: Platform Web SDK を使用したAudience ManagerとAdobe Experience Platformの ID の同期
description: Platform Web SDK を使用して、Audience ManagerとAdobe Experience Platformの間で ID を同期する方法について説明します
seo-description: Learn how to sync identities with Adobe Audience Manager with Experience Platform Web SDK
keywords: audience manager;aam;identities；同期 id；名前空間；
source-git-commit: 5b37b51308dc2097c05b0e763293467eb12a2f21
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 0%

---


# Audience ManagerとExperience Platformの ID の同期

Adobe Experience Platform Web SDK は、 [sendEvent](./overview.md#syncing-identities) コマンドを使用します。

次から名前空間を選択します。 [ID サービスの名前空間](../../identity/../identity-service/features/namespaces.md) 「アイデンティティ・シンボル」列の値を使用して、アイデンティティが関連するコンテキストを示すには、次の手順に従います。

![名前空間 UI の表示](../assets/identity/edge_namespaceUI_identity-symbol.png)

Audience Managerのお客様は、ID タイプ：クロスデバイスを使用する既存のすべてのデータソースに、対応する ID 名前空間を自動的に持ちます。 Audience Managerデータソースに対応する ID 名前空間を見つけるには、Adobe Experience Platformにログインし、「 ID 」セクションに移動します。

任意の新規 [!DNL Audience Manager] ID タイプを使用するデータソース：クロスデバイスは、対応する ID 名前空間を生成します。 データソース ID タイプ Cookie とデバイス広告 ID は、現在サポートされていません。 さらに、Adobe Experience Platformで作成された ID 名前空間は、対応する [!DNL Audience Manager] データソースですが、syncIdentity メソッドは名前空間 ID シンボルのみをサポートします。
