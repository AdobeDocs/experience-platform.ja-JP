---
keywords: Experience Platform；ホーム；人気の高いトピック；データ取り込み；取り込んだデータ；ストリーミング；概要；ストリーミング取り込み；遅延；ストリーミング遅延；
solution: Experience Platform
title: ストリーミング取り込みの概要
topic-legacy: overview
description: Adobe Experience Platformのストリーミング取り込みを使用すると、ユーザーはクライアントおよびサーバーサイドのデバイスから、リアルタイムでExperience Platformにデータを送信できます。
exl-id: 851f15fd-7ac5-4a9f-934d-6b907057da87
source-git-commit: 88939d674c0002590939004e0235d3da8b072118
workflow-type: tm+mt
source-wordcount: '348'
ht-degree: 18%

---

# ストリーミングインジェストの概要

Adobe Experience Platformのストリーミング取り込みを使用すると、ユーザーはクライアントおよびサーバーサイドのデバイスからにデータを送信できます。 [!DNL Experience Platform] リアルタイムで。

## ストリームインジェストの機能

Adobe Experience Platformでは、 [!DNL Real-time Customer Profile] 個々の顧客ごとに ストリーミング取り込みは、配信を可能にすることで、これらのプロファイルの作成に重要な役割を果たします [!DNL Profile] データを [!DNL Data Lake] 待ち時間はできるだけ短く

次のビデオは、ストリーミング取り込みに関する理解を深めるために設計されており、上記の概念の概要を説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/28425?quality=12&learn=on)

### プロファイルレコードと のストリーミング[!DNL ExperienceEvents]

ストリーミング取り込みを使用すると、ユーザーはプロファイルレコードをストリーミングし、 [!DNL ExperienceEvents] から [!DNL Platform] リアルタイムのパーソナライゼーションの促進に役立つ秒数。 ストリーミング取得 API に送信されるすべてのデータは、 [!DNL Data Lake].

詳しくは、[ストリーミング接続の作成に関するガイド](../tutorials/create-streaming-connection.md)を参照してください。

### データセットへのストリーミング

データが正しいことを確認したら、次のようにデータセットを有効にできます。 [!DNL Real-time Customer Profile] および [!DNL Identity Service].

のデータセットを有効にする方法について詳しくは、 [!DNL Profile] および [!DNL Identity Service]を読んでください。 [データセットガイドの設定](../../profile/tutorials/dataset-configuration.md).

## でのストリーミング取り込みに予想される遅延 [!DNL Platform]?

| 宛先 | 予想遅延時間 |
| --------- | ---------------- |
| リアルタイム顧客プロファイル | &lt; 1 分 |
| データレイク | &lt; 60 分 |

## ストリーミング取り込み時の 1 秒あたりのリクエスト (RPS) ガイダンス

次の表に、ストリーミング取り込みの秒数制限に関するガイダンスを示します。

| RPS 制限 | メモ |
| --- | --- |
| 1 秒あたり 1000 リクエスト | これらは、 `/collection/batch` endpoint. |
| 1 秒あたりの個々のメッセージ数 | メッセージは、 `/collection/batch` endpoint. |

>[!IMPORTANT]
>
>制限が適用されると、 **1 分あたり 60 リクエスト** 同期検証をデバッグ目的で使用する場合。

## Adobe Experience Platform 拡張機能

Adobe Experience Platform 拡張機能を使用して、新しいストリーミング接続を作成できます。この [!DNL Experience Platform] 拡張機能では、でフォーマットされたビーコンを送信するアクションが提供されます。 [!DNL Experience Data Model] (XDM) を使用して [!DNL Experience Platform]. 詳しくは、[Experience Platform 拡張機能](../../tags/extensions/client/sdk/overview.md)に関するドキュメントを参照してください。
