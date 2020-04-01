---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platform Streaming Ingestの概要
topic: overview
translation-type: tm+mt
source-git-commit: a570a7a3d905c4618d80f56f01747cced1d124e8

---


# ストリーミング取り込みの概要

Adobe Experience Platformのストリーミングインジェストでは、クライアントおよびサーバー側のデバイスからExperience Platformにリアルタイムでデータを送信する方法が提供されます。

## ストリーミングの取り込みで何ができる？

Adobe Experience Platformを使用すると、各顧客に対してリアルタイム顧客プロファイルを生成することで、調整され、一貫性のある、関連するエクスペリエンスを実現できます。 ストリーミング取り込みは、プロファイルデータを可能な限り短い待ち時間でデータレークに配信できるようにすることで、これらのプロファイルの構築に重要な役割を果たします。

### ストリームプロファイルレコードとExperienceEvents

ストリーミングの取り込みにより、プロファイルレコードとExperienceEventを数秒でプラットフォームにストリーミングし、リアルタイムのパーソナライゼーションを実現できます。 ストリーミング取り込みAPIに送信されるすべてのデータは、データレークに自動的に保持されます。

詳しくは、『ストリーミ [ング接続の作成ガイド](../tutorials/create-streaming-connection.md) 』を参照してください。

### データセットへのストリーミング

データがクリーンであると確信したら、リアルタイムの顧客プロファイルとIDサービスのデータセットを有効にできます。

プロファイルおよびIDサービスのデータセットを有効にする方法について詳しくは、『データセットの設定』 [ガイドを参照してくださ](../../profile/tutorials/dataset-configuration.md)い。

## プラットフォームでのストリーミング取り込みに予想される遅延は何ですか。

| 宛先 | 予想される遅延 |
| --------- | ---------------- |
| リアルタイム顧客プロファイル | &lt; 1分 |
| データレイク | &lt; 60 分 |

## Adobe Experience Platform 拡張機能

Adobe Experience Platform Extensionを使用して、新しいストリーミング接続を作成できます。 Experience Platform Extensionは、Experience Data Model(XDM)でフォーマットされたビーコンを送信し、Experience Platformにリアルタイムで取り込むためのアクションを提供します。 詳しくは、 [Experience Platform Extensionのドキュメントを参照してください](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/adobe-experience-platform-extension.html) 。