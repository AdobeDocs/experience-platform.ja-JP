---
title: IDとAudience ManagerおよびAdobe Experience Platformの同期
seo-title: IDとAudience ManagerおよびAdobe Experience PlatformとAdobe Experience PlatformWeb SDKの同期
description: IDをExperience PlatformWeb SDKとAdobe Audience Managerと同期する方法を学びます
seo-description: IDをExperience PlatformWeb SDKとAdobe Audience Managerと同期する方法を学びます
keywords: audience manager;aam;identities;sync identities;namespace;
translation-type: tm+mt
source-git-commit: 290792cd507248c41690c493cc18daaab869db50
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---


# IDの同期

Adobe Experience PlatformWeb SDKは、sendEvent [](./overview.md#syncing-identities) コマンドを使用して顧客IDとその認証状態を宣言する機能をサポートしています。

「 [ID名前空間」列の値を使用して、IDの関連するコンテキストを示す名前空間を「](../../identity/../identity-service/namespaces.md) IDサービスのシンボル」から選択します。

![名前空間UIの表示](../../assets/edge_namespaceUI_identity-symbol.png)

Audience Managerのお客様は、IDタイプを使用するすべての既存のデータソースを次のように指定します。クロスデバイスには、対応するID名前空間が自動的に設定されます。 Audience Managerデータソースに対応するID名前空間を探すには、Adobe Experience Platformにログインし、「ID」セクションに移動します。

IDタイプを使用する新しい [!DNL Audience Manager] データソース：デバイス間で、対応するID名前空間が生成されます。 データソースIDタイプのCookieとデバイス広告IDは、現在サポートされていません。 また、Adobe Experience Platformで作成されたID名前空間は、対応する [!DNL Audience Manager] データソースを生成しますが、syncIdentityメソッドは名前空間IDシンボルのみをサポートしていることに注意してください。
