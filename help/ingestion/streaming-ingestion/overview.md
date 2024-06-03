---
keywords: Experience Platform；ホーム；人気のトピック；データ取得；取得したデータ；ストリーミング；概要；ストリーミング取得；待ち時間；ストリーミング待ち時間；
solution: Experience Platform
title: ストリーミング取り込みの概要
description: Adobe Experience Platformのストリーミング取得は、クライアントおよびサーバーサイドデバイスから、リアルタイムでExperience Platformにデータを送信する手段を提供します。
exl-id: 851f15fd-7ac5-4a9f-934d-6b907057da87
source-git-commit: d6424e2a9afc046f4bff329797954fd43939a819
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 14%

---

# ストリーミングインジェストの概要

Adobe Experience Platformのストリーミング取得は、クライアントおよびサーバーサイドデバイスからにデータを送信する手段をユーザーに提供します [!DNL Experience Platform] リアルタイム。

## ストリームインジェストの機能

Adobe Experience Platformでは、以下を生成することで、調整され、一貫性のある、適切なエクスペリエンスを提供できます [!DNL Real-Time Customer Profile] 個々の顧客ごとに。 ストリーミング取り込みは、を配信できるようにすることで、これらのプロファイルを作成するうえで重要な役割を果たします [!DNL Profile] データをに [!DNL Data Lake] できるだけ少ない待ち時間で。

次のビデオは、ストリーミング取得に関する理解を深めるために用意されており、上記の概念の概要を説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/28425?quality=12&learn=on)

### プロファイルレコードのストリーミングと [!DNL ExperienceEvents]

ストリーミング取り込みを使用すると、ユーザーはプロファイルレコードおよびをストリーミングできます [!DNL ExperienceEvents] 対象： [!DNL Platform] 数秒でリアルタイムのパーソナライゼーションを促進します。 ストリーミング取得 API に送信されるすべてのデータは、に自動的に保持されます。 [!DNL Data Lake].

詳しくは、[ストリーミング接続の作成に関するガイド](../tutorials/create-streaming-connection.md)を参照してください。

### データセットへのストリーミング

データがクリーンであると確信したら、次のデータセットを有効にできます [!DNL Real-Time Customer Profile] および [!DNL Identity Service].

のデータセットの有効化について詳しくは、 [!DNL Profile] および [!DNL Identity Service]を読んでください。 [データセットガイドの設定](../../profile/tutorials/dataset-configuration.md).

## Experience Platformでのストリーミング取得に想定される待ち時間はどれくらいですか？

>[!IMPORTANT]
>
>ストリーミング取り込みのガードレールは、サンドボックスレベルではなく、組織レベルで計算されます。 つまり、サンドボックスごとのデータ使用状況は、組織全体に対応するライセンス使用権限の合計にバインドされます。 さらに、開発用サンドボックスでのデータ使用は、プロファイル全体の 10% に制限されています。 ライセンス使用権限について詳しくは、を参照してください [データ管理のベストプラクティスガイド](../../landing/license-usage-and-guardrails/data-management-best-practices.md).

| 宛先 | 予想遅延時間 |
| --------- | ---------------- |
| リアルタイム顧客プロファイル | 第 95 百分位で 15 分未満 |
| データレイク | &lt; 60 分 |

## ストリーミング取り込みに関するリクエスト/秒（RPS）ガイダンス

次の表に、ストリーミング取り込みのリクエスト/秒の制限に関するガイダンスを示します。

| RPS 制限 | メモ |
| --- | --- |
| 1 秒あたり 1,000 リクエスト | を使用する場合、これらは複数のメッセージを含むことができます `/collection/batch` エンドポイント。 |
| 1 秒あたり 10000 件の個別メッセージ | を使用すると、メッセージをグループ化して、実際のリクエストの数を減らすことができます `/collection/batch` エンドポイント。 |

>[!IMPORTANT]
>
>適用される制限はになります **1 分あたり 60 リクエスト** デバッグの目的で同期検証を使用する場合。

## Adobe Experience Platform 拡張機能

Adobe Experience Platform 拡張機能を使用して、新しいストリーミング接続を作成できます。この [!DNL Experience Platform] 拡張機能には、でフォーマットされたビーコンを送信するアクションが用意されています。 [!DNL Experience Data Model] （XDM）による次へのリアルタイム取り込み [!DNL Experience Platform]. 詳しくは、[Experience Platform 拡張機能](../../tags/extensions/client/web-sdk/overview.md)に関するドキュメントを参照してください。
