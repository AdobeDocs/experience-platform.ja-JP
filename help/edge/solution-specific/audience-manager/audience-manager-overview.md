---
title: Adobe Audience Managerへのデータの送信
seo-title: Adobe Experience PlatformWeb SDKを使用したAdobe Audience Managerへのデータの送信
description: Experience PlatformWeb SDKを使用してAdobe Audience Managerにデータを送信する方法を学びます
seo-description: Experience PlatformWeb SDKを使用してAdobe Audience Managerにデータを送信する方法を学びます
translation-type: tm+mt
source-git-commit: 6a83ab1c6405f45700f4f8899139010d50010b0c
workflow-type: tm+mt
source-wordcount: '292'
ht-degree: 0%

---


# Experience PlatformエッジネットワークのAudience Manager

Adobe Experience PlatformWeb SDKは、Adobe Audience Managerと統合されており、Audience Manager、CookieとURLの宛先、IDの同期からのデータの送受信をサポートしています。

## Audience Managerの有効化

Audience Managerを有効にするには、次の手順を実行する必要があります。

- エ [ッジ設定でAudience Managerを有効にします](../../fundamentals/edge-configuration.md)。
- CookieとURLの宛先を有効または無効にします。
- 外部パートナー同期のID同期コンテナを指定します（オプション）

## IDの同期

Adobe Experience PlatformWeb SDKは、SyncIdentity [](../../fundamentals/identity.md) コマンドを使用して顧客IDとその認証状態を宣言する機能をサポートしています。

syncIdentityメソッドは、 [IDサービス名前空間](../../../identity/../identity-service/namespaces.md) (ID)を使用して、IDが関連付けられるコンテキストを示します。 Audience Managerのお客様は、IDタイプを使用するすべての既存のデータソースを次のように指定します。 デバイス間では、対応するID名前空間が自動的に設定されます。 Audience Managerデータソースに対応するID名前空間を探すには、Adobe Experience Platformにログインし、「ID」セクションに移動します。

![名前空間UIの表示](../../../assets/edge_configuration_identity.png)

Audience Managerデータソースを名前で検索できます。 syncIdentityメソッドは、名前空間を示すためにID記号を使用します。

IDタイプを使用する新しいAudience Managerデータソース： デバイス間で、対応するID名前空間が生成されます。 データソースIDタイプのCookieとデバイス広告IDは、現在サポートされていません。 また、Adobe Experience Platformで作成したID名前空間は、対応するAudience Managerデータソースを生成しますが、syncIdentityメソッドは名前空間IDシンボルのみをサポートすることに注意してください。
