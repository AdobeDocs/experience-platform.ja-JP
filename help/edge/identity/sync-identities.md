---
title: Platform Web SDKを使用したAudience ManagerとAdobe Experience PlatformのIDの同期
description: Platform Web SDKを使用して、Audience ManagerとAdobe Experience PlatformのIDを同期する方法について説明します
seo-description: Adobe Audience ManagerとExperience PlatformWeb SDKのIDを同期する方法を説明します
keywords: audience manager;aam;identities；同期id；名前空間；
source-git-commit: 3a1d08a4ea87ee3db7a2a8b048d5721fa679c372
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---


# Audience ManagerとExperience PlatformのIDの同期

Adobe Experience Platform Web SDKは、[sendEvent](./overview.md#syncing-identities)コマンドを使用して顧客IDとその認証状態を宣言する機能をサポートしています。

[IDサービスの名前空間](../../identity/../identity-service/namespaces.md)から名前空間を選択し、IDシンボル列の値を使用して、IDが関連するコンテキストを示します。

![名前空間UIの表示](../images/identity/edge_namespaceUI_identity-symbol.png)

Audience Managerのお客様は、IDタイプを使用するすべての既存のデータソースを次のように指定します。クロスデバイスには、対応するID名前空間が自動的に設定されます。 対応するAudience ManagerデータソースのID名前空間を見つけるには、Adobe Experience Platformにログインし、「 ID 」セクションに移動します。

IDタイプを使用する新しい[!DNL Audience Manager]データソース：クロスデバイス対応のID名前空間が生成されます。 データソースIDタイプCookieとデバイス広告IDは、現在サポートされていません。 また、 Adobe Experience PlatformでID名前空間を作成すると、対応する[!DNL Audience Manager]データソースが生成されますが、 syncIdentityメソッドは名前空間IDシンボルのみをサポートすることに注意してください。
