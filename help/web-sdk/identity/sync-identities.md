---
title: Experience Platform Web SDKを使用したAudience ManagerとAdobe Experience Platform間の Id の同期
description: Experience Platform web SDKを使用してAudience ManagerとAdobe Experience Platformの間で ID を同期する方法を説明します
seo-description: Learn how to sync identities with Adobe Audience Manager with Experience Platform Web SDK
keywords: audience manager;aam;id；同期 ID；名前空間；
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 0%

---


# Audience ManagerとExperience Platform間の ID の同期

Adobe Experience Platform Web SDKでは、[sendEvent](./overview.md#syncing-identities) コマンドを使用して顧客 ID とその認証状態を宣言する機能をサポートしています。

[ID サービス名前空間 &#x200B;](../../identity/../identity-service/features/namespaces.md) から名前空間を選択し、ID 記号列の値を使用して、ID の関連先コンテキストを示します。

![&#x200B; 名前空間 UI の表示 &#x200B;](../assets/identity/edge_namespaceUI_identity-symbol.png)

Audience Managerをご利用のお客様は、ID タイプを使用する既存のすべてのデータソースに対して、対応する ID 名前空間が自動的に設定されます。 Audience Manager Data Sourceに対応する ID 名前空間を見つけるには、Adobe Experience Platformにログインし、「ID」セクションに移動します。

ID タイプ : クロスデバイスを使用する新しい [!DNL Audience Manager] Data Sourceは、対応する ID 名前空間を生成します。 Data Source ID タイプの Cookie および Device Advertising ID は、現在サポートされていません。 さらに、Adobe Experience Platformで作成された ID 名前空間は、対応する [!DNL Audience Manager] Data Sourceを生成しますが、syncIdentity メソッドは名前空間 ID 記号のみをサポートすることに注意してください。
