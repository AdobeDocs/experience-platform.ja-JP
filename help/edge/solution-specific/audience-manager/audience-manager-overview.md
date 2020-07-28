---
title: Adobe Audience Managerへのデータの送信
seo-title: Adobe Experience PlatformWeb SDKを使用したAdobe Audience Managerへのデータの送信
description: Experience PlatformWeb SDKを使用してAdobe Audience Managerにデータを送信する方法を学びます
seo-description: Experience PlatformWeb SDKを使用してAdobe Audience Managerにデータを送信する方法を学びます
translation-type: tm+mt
source-git-commit: 7b07a974e29334cde2dee7027b9780a296db7b20
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 0%

---


# [!DNL Audience Manager] の [!DNL Experience Platform Edge Network]

このAdobe Experience Platform [!DNL Web SDK] はAdobe Audience Managerと統合されており、CookieとURLの宛先 [!DNL Audience Manager]、IDの同期からのデータの送受信をサポートしています。

## 有効にする [!DNL Audience Manager]

有効にす [!DNL Audience Manager] るには、次の操作を行う必要があります。

- [!DNL Audience Manager] エッジ設定 [でを有効にします](../../fundamentals/edge-configuration.md)。
- CookieとURLの宛先を有効または無効にします。
- 外部パートナー同期のID同期コンテナを指定します（オプション）

## IDの同期

Adobe Experience Platform [!DNL Web SDK] では、SyncIdentity [](../../fundamentals/identity.md) コマンドを使用して顧客IDとその認証状態を宣言する機能をサポートしています。

syncIdentityメソッドは、 [IDサービス名前空間](../../../identity/../identity-service/namespaces.md) (ID)を使用して、IDが関連付けられるコンテキストを示します。 お客様は、 [!DNL Audience Manager] IDタイプを使用するすべての既存のデータソースを次のように指定します。 デバイス間では、自動的に対応する [!DNL Identity Namespace]。 対応するのが見つか [!DNL Identity Namespace] るに [!DNL Audience Manager Data Source]は、Adobe Experience Platformにログインし、その [!DNL Identities] セクションに移動します。

![名前空間UIの表示](../../../assets/edge_configuration_identity.png)

ここでは、「名前」で [!DNL Audience Manager] データソースを検索できます。 syncIdentityメソッドは、名前空間を示すためにID記号を使用します。

IDタイプを使用する新しい [!DNL Audience Manager] データソース： デバイス間で、対応するID名前空間が生成されます。 データソースIDタイプのCookieとデバイス広告IDは、現在サポートされていません。 また、Adobe Experience Platformで作成したID名前空間は、対応する [!DNL Audience Manager] データソースを生成しますが、syncIdentityメソッドは名前空間IDシンボルのみをサポートすることに注意してください。
