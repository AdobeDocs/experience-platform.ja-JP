---
keywords: Experience Platform；ホーム；人気のあるトピック；データ取り込み；取り込んだデータ；ストリーミング；概要；ストリーミング取り込み；待ち時間；ストリーミング待ち時間；
solution: Experience Platform
title: ストリーミング取り込みの概要
topic-legacy: overview
description: Adobe Experience Platformのストリーミング取り込み機能を使用すると、クライアントおよびサーバ側のデバイスからExperience Platformにデータをリアルタイムで送信できます。
exl-id: 851f15fd-7ac5-4a9f-934d-6b907057da87
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 22%

---

# ストリーミングインジェストの概要

Adobe Experience Platformのストリーミング取り込み機能は、ユーザがクライアントおよびサーバ側デバイスから[!DNL Experience Platform]にデータをリアルタイムで送信する方法を提供します。

## ストリームインジェストの機能

Adobe Experience Platformでは、個々の顧客に対して[!DNL Real-time Customer Profile]を生成することで、協調性、一貫性、関連性のあるエクスペリエンスを実現できます。 ストリーミング取り込みは、[!DNL Profile]データを[!DNL Data Lake]にできるだけ待ち時間を短くして配信できるようにすることで、これらのプロファイルの構築に重要な役割を果たします。

次のビデオは、ストリーミング取り込みに関する理解を深めるために設計されており、上述の概念について概説しています。

>[!VIDEO](https://video.tv.adobe.com/v/28425?quality=12&learn=on)

### プロファイルレコードと のストリーミング[!DNL ExperienceEvents]

ストリーミング取り込みにより、プロファイルレコードと[!DNL ExperienceEvents]を数秒で[!DNL Platform]にストリーミングして、リアルタイムのパーソナライゼーションを促進できます。 ストリーミング取り込みAPIに送信されるすべてのデータは、自動的に[!DNL Data Lake]に保持されます。

詳しくは、[ストリーミング接続の作成に関するガイド](../tutorials/create-streaming-connection.md)を参照してください。

### データセットへのストリーミング

データがクリーンであると確信したら、[!DNL Real-time Customer Profile]と[!DNL Identity Service]のデータセットを有効にできます。

[!DNL Profile]と[!DNL Identity Service]のデータセットを有効にする方法について詳しくは、[『データセットガイド](../../profile/tutorials/dataset-configuration.md)を設定する』を参照してください。

## [!DNL Platform]でのストリーミング取り込みに予想される遅延は何ですか？

| 宛先 | 予想遅延時間 |
| --------- | ---------------- |
| リアルタイム顧客プロファイル | &lt; 1 分 |
| データレイク | &lt; 60 分 |

## Adobe Experience Platform 拡張機能

Adobe Experience Platform 拡張機能を使用して、新しいストリーミング接続を作成できます。[!DNL Experience Platform]拡張機能は、[!DNL Experience Data Model] (XDM)でフォーマットされたビーコンを[!DNL Experience Platform]にリアルタイムで取り込むために送信するアクションを提供します。 詳しくは、[Experience Platform 拡張機能](https://experienceleague.adobe.com/docs/launch/using/extensions-ref/adobe-extension/adobe-experience-platform-extension.html)に関するドキュメントを参照してください。
