---
title: Adobe Adobeオーディエンスマネージャーへのデータの送信
seo-title: Adobe Experience Platform Web SDKを使用したAdobeオーディエンスマネージャーへのデータの送信
description: Experience Platform Web SDKを使用してAdobeオーディエンスマネージャーにデータを送信する方法を学びます。
seo-description: Experience Platform Web SDKを使用してAdobeオーディエンスマネージャーにデータを送信する方法を学びます。
translation-type: tm+mt
source-git-commit: dfe9ea2889b3ba2e74f8b10296bfb2d123ad9d57
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 7%

---


# （ベータ版）Experience Platform Edge Networkのオーディエンスマネージャ

>[!IMPORTANT]
>
>Adobe Experience Platform Web SDK は現在ベータ版で、すべてのユーザーが利用できるわけではありません。ドキュメントと機能は変更される場合があります。

Adobe Experience Platform Web SDKは、Adobe Platform Web SDKと統合されており、オーディエンスマネージャー、Cookie、URLの宛先、IDの同期からのデータの送受信をサポートしています。

## オーディエンスマネージャーの有効化

オーディエンスマネージャを有効にするには、次の操作を行う必要があります。

- エ [ッジ設定でオーディエンスマネージャを有効にします](../../fundamentals/edge-configuration.md)。
- CookieとURLの宛先を有効または無効にします。
- 外部パートナー同期のID同期コンテナを指定します（オプション）

## IDの同期

Adobe Experience Platform Web SDKは、SyncIdentity [](../../fundamentals/identity.md) コマンドを使用して顧客IDとその認証状態を宣言する機能をサポートしています。

syncIdentityメソッドは、 [IDサービスの名前空間](../../../identity/../identity-service/namespaces.md) を使用して、IDが関連付けられるコンテキストを示します。 オーディエンスマネージャーのお客様は、IDタイプを使用するすべての既存のデータソースを次のように指定します。 デバイス間では、対応するID名前空間が自動的に設定されます。 オーディエンスマネージャーのデータソースに対応するID名前空間を探すには、Adobe Experience Platformにログインし、「ID」セクションに移動します。

![名前空間UIの表示](../../../assets/edge_configuration_identity.png)

ここでは、「名前」でオーディエンスマネージャーのデータソースを検索できます。 syncIdentityメソッドは、名前空間を示すためにID記号を使用します。

IDタイプを使用する新しいオーディエンスマネージャーデータソース： デバイス間で、対応するID名前空間が生成されます。 データソースIDタイプのCookieとデバイス広告IDは、現在サポートされていません。 また、Adobe Experience Platformで作成したID名前空間は、対応するオーディエンスマネージャーデータソースを生成しますが、syncIdentityメソッドは名前空間IDシンボルのみをサポートすることに注意してください。
