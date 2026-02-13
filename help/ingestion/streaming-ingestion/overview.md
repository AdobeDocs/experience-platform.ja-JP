---
keywords: Experience Platform；ホーム；人気のトピック；データ取得；取得したデータ；ストリーミング；概要；ストリーミング取得；待ち時間；ストリーミング待ち時間；
solution: Experience Platform
title: ストリーミング取り込みの概要
description: Adobe Experience Platformでのストリーミング取得について説明します。
exl-id: 851f15fd-7ac5-4a9f-934d-6b907057da87
source-git-commit: 568208c9b2cb774bbbeed74ae2d456c87e99bca9
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 15%

---

# ストリーミング取り込みの概要

Adobe Experience Platformのストリーミング取得は、クライアントおよびサーバーサイドデバイスからリアルタイムでExperience Platformにデータを送信する手段を提供します。

## ストリーミング取り込みの機能

Adobe Experience Platformを使用すると、個々の顧客に対してリアルタイム顧客プロファイルを生成することで、調整された一貫性のある関連性の高いエクスペリエンスを提供できます。 ストリーミング取り込みは、可能な限り少ない待ち時間でプロファイルデータをデータレイクに配信できるようにすることで、これらのプロファイルを作成するうえで重要な役割を果たします。

次のビデオは、ストリーミング取得に関する理解を深めるために用意されており、上記の概念の概要を説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/28425?quality=12&learn=on)

### プロファイルレコードと [!DNL ExperienceEvents] のストリーミング

ストリーミング取得を使用すると、プロファイルレコードや [!DNL ExperienceEvents] ータを数秒でExperience Platformにストリーミングして、リアルタイムのパーソナライゼーションを促進できます。 ストリーミング取得 API に送信されるすべてのデータは、データレイクに自動的に保持されます。

詳しくは、[ストリーミング接続の作成に関するガイド](../tutorials/create-streaming-connection.md)を参照してください。

### データセットへのストリーミング

データがクリーンであると確信したら、リアルタイム顧客プロファイルおよび [!DNL Identity Service] のデータセットを有効にすることができます。

プロファイルおよび [!DNL Identity Service] ークフローに対してデータセットを有効にする方法について詳しくは、[ データセットの設定ガイド ](/help/profile/tutorials/dataset-configuration.md) を参照してください。

## Experience Platformでのストリーミング取得に想定される待ち時間はどれくらいですか？

>[!IMPORTANT]
>
>ストリーミング取り込み用のガードレールは、組織全体に対応するライセンス使用権限の合計にバインドされます。 さらに、開発用サンドボックスでのデータ使用は、プロファイル全体の 10% に制限されています。 ライセンス使用権限について詳しくは、[ データ管理のベストプラクティスガイド ](/help/landing/license-usage-and-guardrails/data-management-best-practices.md) を参照してください。 ストリーミングスループットに制限を設定する方法については、[ 処理能力の概要 ](../../landing/license-usage-and-guardrails/capacity.md) を参照してください。

| 宛先 | 予想遅延時間 |
| --------- | ---------------- |
| リアルタイム顧客プロファイル | <ul><li>B2C データ取り込みの 95 パーセンタイルで 15 分未満。</li><li>B2B データ取り込みの 95 パーセンタイルで 30 分未満。</li></ul> |
| データレイク | &lt; 60 分 |

## ストリーミング取り込みに関するリクエスト/秒（RPS）ガイダンス

次の表に、ストリーミング取り込みのリクエスト/秒の制限に関するガイダンスを示します。

| RPS 制限 | メモ |
| --- | --- |
| 1 秒あたり 1,000 リクエスト | エンドポイントを使用する場合、これらは複数のメッセージ `/collection/batch` 含むことができます。 |
| 1 秒あたり 10000 件の個別メッセージ | `/collection/` エンドポイントを使用すると、メッセージをグループ化して、実際のリクエストの数を減らすことができます。 |

>[!IMPORTANT]
>
>デバッグ目的で同期検証を使用する場合 **適用される制限は 1 分あたり** 60 リクエストになります。

## Adobe Experience Platform 拡張機能

Adobe Experience Platform 拡張機能を使用して、新しいストリーミング接続を作成できます。[!DNL Experience Platform] 拡張機能には、[!DNL Experience Data Model] にリアルタイムに取り込まれるように [!DNL Experience Platform] （XDM）形式のビーコンを送信するアクションが用意されています。 詳しくは、[Experience Platform 拡張機能](/help/tags/extensions/client/web-sdk/overview.md)に関するドキュメントを参照してください。
