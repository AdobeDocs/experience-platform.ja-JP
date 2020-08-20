---
title: E.Adobe Audience Managerにデータを送る
seo-title: Adobe Experience PlatformWeb SDKを使用したAdobe Audience Managerへのデータの送信
description: Experience PlatformWeb SDKを使用してデータをAdobe Audience Managerに送信する方法を学びます
seo-description: Experience PlatformWeb SDKを使用してデータをAdobe Audience Managerに送信する方法を学びます
keywords: audience manager;aam;identities;sync identities;namespace;
translation-type: tm+mt
source-git-commit: 8c256b010d5540ea0872fa7e660f71f2903bfb04
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---


# [!DNL Audience Manager] の [!DNL Experience Platform Edge Network]

Adobe Experience Platform [!DNL Web SDK] はAdobe Audience Managerと統合されており、CookieとURLの宛先 [!DNL Audience Manager]、IDの同期からのデータの送受信をサポートしています。

## 有効にする [!DNL Audience Manager]

有効にす [!DNL Audience Manager] るには、次の操作を行う必要があります。

- [!DNL Audience Manager] エッジ設定 [でを有効にします](../../fundamentals/edge-configuration.md)。
- CookieとURLの宛先を有効または無効にします。
- 外部パートナー同期のID同期コンテナを指定します（オプション）

## IDの同期

Adobe Experience PlatformWeb SDKは、sendEvent [](../../fundamentals/identity.md#syncing-identities) コマンドを使用して顧客IDとその認証状態を宣言する機能をサポートしています。

「 [ID名前空間」列の値を使用して、IDの関連するコンテキストを示す名前空間を「](../../../identity/../identity-service/namespaces.md) IDサービスのシンボル」から選択します。

![名前空間UIの表示](../../../assets/edge_namespaceUI_identity-symbol.png)

Audience Managerのお客様は、IDタイプを使用するすべての既存のデータソースを次のように指定します。クロスデバイスには、対応するID名前空間が自動的に設定されます。 Audience Managerデータソースに対応するID名前空間を探すには、Adobe Experience Platformにログインし、「ID」セクションに移動します。

IDタイプを使用する新しい [!DNL Audience Manager] データソース：デバイス間で、対応するID名前空間が生成されます。 データソースIDタイプのCookieとデバイス広告IDは、現在サポートされていません。 また、Adobe Experience Platformで作成されたID名前空間は、対応する [!DNL Audience Manager] データソースを生成しますが、syncIdentityメソッドは名前空間IDシンボルのみをサポートしていることに注意してください。
