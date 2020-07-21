---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platformストリーミング取り込みの概要
topic: overview
translation-type: tm+mt
source-git-commit: 73a492ba887ddfe651e0a29aac376d82a7a1dcc4
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 3%

---


# ストリーミング取り込みの概要

Adobe Experience Platformのストリーミング取り込みでは、ユーザーはクライアントおよびサーバー側のデバイスからデータをリアルタイムに送信する方法 [!DNL Experience Platform] を使用できます。

## ストリーミング取り込みで何ができますか。

Adobe Experience Platformを使用すると、個々の顧客に対してを生成することで、調整され、一貫性のある、関連性のあるエクスペリエンス [!DNL Real-time Customer Profile] を実現できます。 ストリーミング取り込みは、可能な限り待ち時間を短くして [!DNL Profile][!DNL Data Lake] データをに配信できるようにすることで、これらのプロファイルを構築する上で重要な役割を果たします。

次のビデオは、ストリーミング取り込みに関する理解を深めるために設計されており、上述の概念について概説しています。

>[!VIDEO](https://video.tv.adobe.com/v/28425?quality=12&learn=on)

### ストリームプロファイルレコードおよび [!DNL ExperienceEvents]

ストリーミング取り込みにより、プロファイルの記録を数秒でストリーミングし、リアルタイム [!DNL ExperienceEvents] のパーソナライゼーション [!DNL Platform] を促進できます。 ストリーミング取り込みAPIに送信されたすべてのデータは、で自動的に保持され [!DNL Data Lake]ます。

詳しくは、『ストリーミング接続の [作成](../tutorials/create-streaming-connection.md) 』ガイドを参照してください。

### データセットへのストリーム

データがクリーンであることが確実にわかったら、 [!DNL Real-time Customer Profile] およびのデータセットを有効にでき [!DNL Identity Service]ます。

およびのデータセットの有効化について詳し [!DNL Profile] くは、『データセットの [!DNL Identity Service]設定 [](../../profile/tutorials/dataset-configuration.md)』ガイドを参照してください。

## What is the expected latency for streaming ingestion on [!DNL Platform]?

| 宛先 | 予想される遅延 |
| --------- | ---------------- |
| リアルタイム顧客プロファイル | &lt; 1分 |
| データレイク | &lt; 60 分 |

## Adobe Experience Platform 拡張機能

Adobe Experience Platform拡張機能を使用して、新しいストリーミング接続を作成できます。 この [!DNL Experience Platform] 拡張機能は、(XDM)でフォーマットされたビーコンを送信し、リアルタイムで取り込むためのアクションを提供し [!DNL Experience Data Model] ま [!DNL Experience Platform]す。 詳しくは、 [Experience Platform拡張機能のドキュメントを参照してください](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/adobe-experience-platform-extension.html) 。