---
keywords: Experience Platform；ホーム；人気のトピック；データ取得；取得したデータ；ストリーミング；概要；ストリーミング取得；待ち時間；ストリーミング待ち時間；
solution: Experience Platform
title: ストリーミング取り込みの概要
description: Adobe Experience Platformのストリーミング取得は、クライアントおよびサーバーサイドデバイスからリアルタイムでExperience Platformにデータを送信する手段を提供します。
exl-id: 851f15fd-7ac5-4a9f-934d-6b907057da87
source-git-commit: f5ae9170b312d9f24c863a14b8cc2310fcaf1cb2
workflow-type: tm+mt
source-wordcount: '419'
ht-degree: 15%

---

# ストリーミングインジェストの概要

Adobe Experience Platformのストリーミング取得は、クライアントおよびサーバーサイドデバイスから [!DNL Experience Platform] にリアルタイムでデータを送信する手段をユーザーに提供します。

## ストリームインジェストの機能

Adobe Experience Platformを使用すると、個々の顧客に対して [!DNL Real-Time Customer Profile] を生成することで、調整された一貫性のある関連性の高いエクスペリエンスを提供できます。 ストリーミング取得は、可能な限り少ない待ち時間で [!DNL Profile] しいデータを [!DNL Data Lake] に配信できるようにすることで、これらのプロファイルを作成する上で重要な役割を果たします。

次のビデオは、ストリーミング取得に関する理解を深めるために用意されており、上記の概念の概要を説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/31657?quality=12&learn=on&captions=jpn)

### プロファイルレコードと [!DNL ExperienceEvents] のストリーミング

ストリーミング取り込みを使用すると、ユーザーはプロファイルレコードをストリーミングし、数秒で [!DNL ExperienceEvents] ータを [!DNL Experience Platform] き出して、リアルタイムのパーソナライゼーションを促進できます。 ストリーミング取得 API に送信されるすべてのデータは、[!DNL Data Lake] に自動的に保持されます。

詳しくは、[ストリーミング接続の作成に関するガイド](../tutorials/create-streaming-connection.md)を参照してください。

### データセットへのストリーミング

データがクリーンであると確信したら、データセットで [!DNL Real-Time Customer Profile] と [!DNL Identity Service] を有効にできます。

[!DNL Profile] および [!DNL Identity Service] のデータセットの有効化に関する詳細については、[ データセットガイドの設定 ](../../profile/tutorials/dataset-configuration.md) を参照してください。

## Experience Platformでのストリーミング取得に想定される待ち時間はどれくらいですか？

>[!IMPORTANT]
>
>ストリーミング取り込み用のガードレールは、組織全体に対応するライセンス使用権限の合計にバインドされます。 さらに、開発用サンドボックスでのデータ使用は、プロファイル全体の 10% に制限されています。 ライセンス使用権限について詳しくは、[ データ管理のベストプラクティスガイド ](../../landing/license-usage-and-guardrails/data-management-best-practices.md) を参照してください。 ストリーミングスループットに制限を設定する方法については、[ 処理能力の概要 ](../../landing/license-usage-and-guardrails/capacity.md) を参照してください。

| 宛先 | 予想遅延時間 |
| --------- | ---------------- |
| リアルタイム顧客プロファイル | 第 95 百分位で 15 分未満 |
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

Adobe Experience Platform 拡張機能を使用して、新しいストリーミング接続を作成できます。[!DNL Experience Platform] 拡張機能には、[!DNL Experience Data Model] にリアルタイムに取り込まれるように [!DNL Experience Platform] （XDM）形式のビーコンを送信するアクションが用意されています。 詳しくは、[Experience Platform 拡張機能](../../tags/extensions/client/web-sdk/overview.md)に関するドキュメントを参照してください。
