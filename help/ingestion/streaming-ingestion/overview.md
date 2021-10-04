---
keywords: Experience Platform；ホーム；人気のあるトピック；データ取り込み；取り込んだデータ；ストリーミング；概要；ストリーミング取り込み；遅延；ストリーミング遅延；
solution: Experience Platform
title: ストリーミング取り込みの概要
topic-legacy: overview
description: Adobe Experience Platformのストリーミング取り込みを使用すると、ユーザーはクライアントおよびサーバーサイドのデバイスからExperience Platformにリアルタイムでデータを送信できます。
exl-id: 851f15fd-7ac5-4a9f-934d-6b907057da87
source-git-commit: 12c3f440319046491054b3ef3ec404798bb61f06
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 23%

---

# ストリーミングインジェストの概要

Adobe Experience Platformのストリーミング取り込みを使用すると、ユーザーはクライアントおよびサーバー側のデバイスから [!DNL Experience Platform] にリアルタイムでデータを送信できます。

## ストリームインジェストの機能

Adobe Experience Platformでは、個々の顧客ごとに [!DNL Real-time Customer Profile] を生成することで、調整され、一貫性のある、関連性の高いエクスペリエンスを実現できます。 ストリーミング取り込みは、これらのプロファイルの構築に重要な役割を果たします。これにより、 [!DNL Profile] データを可能な限り短い待ち時間で [!DNL Data Lake] に配信できます。

次のビデオは、ストリーミング取り込みに関する理解を深めるためのもので、上記の概念の概要を説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/28425?quality=12&learn=on)

### プロファイルレコードと のストリーミング[!DNL ExperienceEvents]

ストリーミング取り込みを使用すると、プロファイルレコードと [!DNL ExperienceEvents] を秒単位で [!DNL Platform] にストリーミングして、リアルタイムのパーソナライゼーションを実現できます。 ストリーミング取得 API に送信されたすべてのデータは、[!DNL Data Lake] に自動的に保持されます。

詳しくは、[ストリーミング接続の作成に関するガイド](../tutorials/create-streaming-connection.md)を参照してください。

### データセットへのストリーミング

データが正しいと確信したら、[!DNL Real-time Customer Profile] と [!DNL Identity Service] のデータセットを有効にできます。

[!DNL Profile] と [!DNL Identity Service] のデータセットを有効にする方法について詳しくは、[ データセットの設定に関するガイド ](../../profile/tutorials/dataset-configuration.md) を参照してください。

## [!DNL Platform] でのストリーミング取り込みで予想される遅延はどのくらいですか？

| 宛先 | 予想遅延時間 |
| --------- | ---------------- |
| リアルタイム顧客プロファイル | &lt; 1 分 |
| データレイク | &lt; 60 分 |

## Adobe Experience Platform 拡張機能

Adobe Experience Platform 拡張機能を使用して、新しいストリーミング接続を作成できます。[!DNL Experience Platform] 拡張機能は、[!DNL Experience Data Model](XDM) でフォーマットされたビーコンを送信し、[!DNL Experience Platform] にリアルタイムに取り込むためのアクションを提供します。 詳しくは、[Experience Platform 拡張機能](../../tags/extensions/web/sdk/overview.md)に関するドキュメントを参照してください。
