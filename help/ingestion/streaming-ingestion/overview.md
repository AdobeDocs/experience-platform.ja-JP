---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platform Streaming Ingestの概要
topic: overview
translation-type: tm+mt
source-git-commit: a570a7a3d905c4618d80f56f01747cced1d124e8
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 3%

---


# ストリーミング取り込みの概要

Adobe Experience Platformのストリーミング取り込み機能を使用すると、ユーザーはクライアントおよびサーバー側のデバイスからExperience Platformにデータをリアルタイムで送信できます。

## ストリーミング取り込みで何ができますか。

Adobe Experience Platformを使用すると、個々の顧客に対してリアルタイムの顧客プロファイルを生成することで、連携し、一貫性のある、関連するエクスペリエンスを実現できます。 ストリーミング取り込みは、プロファイルデータを可能な限り待ち時間を短くしてデータレークに配信できるようにすることで、これらのプロファイルを構築する上で重要な役割を果たします。

### ストリームプロファイルレコードとExperienceEvents

ストリーミング取り込みにより、プロファイルの記録とExperienceEventsを数秒でプラットフォームにストリーミング配信し、リアルタイムのパーソナライゼーションを実現できます。 ストリーミング取り込みAPIに送信されたすべてのデータは、データレークに自動的に保持されます。

詳しくは、『ストリーミング接続の [作成](../tutorials/create-streaming-connection.md) 』ガイドを参照してください。

### データセットへのストリーム

データがクリーンであると確信したら、リアルタイム顧客プロファイルおよびIDサービス用のデータセットを有効にできます。

プロファイルおよびIDサービスのデータセットを有効にする方法について詳しくは、『データセットの [設定』ガイドを参照してください](../../profile/tutorials/dataset-configuration.md)。

## プラットフォームでのストリーミング取り込みに予想される遅延は何ですか。

| 宛先 | 予想される遅延 |
| --------- | ---------------- |
| リアルタイム顧客プロファイル | &lt; 1分 |
| データレイク | &lt; 60 分 |

## Adobe Experience Platform 拡張機能

Adobe Experience Platform Extensionを使用して、新しいストリーミング接続を作成できます。 Experience Platform Extensionには、Experience Data Model(XDM)でフォーマットされたビーコンを送信するアクションが用意されており、Experience Platformにリアルタイムで取り込むことができます。 詳しくは、 [エクスペリエンスプラットフォーム拡張機能のドキュメントを参照してください](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/adobe-experience-platform-extension.html) 。