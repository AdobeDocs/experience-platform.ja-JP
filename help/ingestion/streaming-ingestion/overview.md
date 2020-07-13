---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platformストリーミング取り込みの概要
topic: overview
translation-type: tm+mt
source-git-commit: 3f1c3c77a0755a3e305da0fb8a234be0f0ee1863
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 3%

---


# ストリーミング取り込みの概要

Adobe Experience Platformのストリーミング取り込みにより、ユーザーはクライアントおよびサーバー側デバイスからExperience Platformにデータをリアルタイムで送信できます。

## ストリーミング取り込みで何ができますか。

Adobe Experience Platformを使用すると、個々の顧客に対してリアルタイムの顧客プロファイルを生成することで、調整済みで一貫性のある関連性のあるエクスペリエンスを実現できます。 ストリーミング取り込みは、プロファイルデータを可能な限り待ち時間を短くしてデータレークに配信できるようにすることで、これらのプロファイルを構築する上で重要な役割を果たします。

次のビデオは、ストリーミング取り込みに関する理解を深めるために設計されており、上述の概念について概説しています。

>[!VIDEO](https://video.tv.adobe.com/v/28425?quality=12&learn=on)

### ストリームプロファイルレコードとExperienceEvents

ストリーミング取り込みを使用すると、プロファイルの記録やExperienceEventsを数秒でPlatformにストリーミングして、リアルタイムのパーソナライゼーションを実現できます。 ストリーミング取り込みAPIに送信されたすべてのデータは、データレークに自動的に保持されます。

詳しくは、『ストリーミング接続の [作成](../tutorials/create-streaming-connection.md) 』ガイドを参照してください。

### データセットへのストリーム

データがクリーンであると確信したら、リアルタイム顧客プロファイルおよびIDサービス用のデータセットを有効にできます。

プロファイルおよびIDサービスのデータセットを有効にする方法について詳しくは、『データセットの [設定』ガイドを参照してください](../../profile/tutorials/dataset-configuration.md)。

## Platformでのストリーミング取り込みに予想される遅延は何ですか。

| 宛先 | 予想される遅延 |
| --------- | ---------------- |
| リアルタイム顧客プロファイル | &lt; 1分 |
| データレイク | &lt; 60 分 |

## Adobe Experience Platform 拡張機能

Adobe Experience Platform拡張機能を使用して、新しいストリーミング接続を作成できます。 Experience Platform拡張機能は、Experience Data Model(XDM)でフォーマットされたビーコンを送信して、Experience Platformにリアルタイムで取り込むためのアクションを提供します。 詳しくは、 [Experience Platform拡張機能のドキュメントを参照してください](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/adobe-experience-platform-extension.html) 。