---
keywords: Experience Platform；ホーム；人気のトピック；データ取り込み；取り込んだデータ；ストリーミング；概要；ストリーミング取り込み；待ち時間；ストリーミング待ち時間；
solution: Experience Platform
title: ストリーミング取り込みの概要
description: Adobe Experience Platformのストリーミング取り込みについて説明します。
exl-id: 851f15fd-7ac5-4a9f-934d-6b907057da87
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 15%

---

# ストリーミング取り込みの概要

Adobe Adobe Experience Platformのストリーミング取り込みでは、クライアントデバイスとサーバーサイドデバイスからExperience Platformにデータをリアルタイムで送信できます。

## ストリーミング取り込みの機能

Adobe Experience Platformなら、顧客一人ひとりに合わせてリアルタイムの顧客プロファイルを作成し、一貫性のある、適切なエクスペリエンスを提供できます。 ストリーミング取り込みは、プロファイルデータを可能な限り遅延なくデータレイクに配信できるようにすることで、これらのプロファイルを構築する上で重要な役割を果たします。

次のビデオは、ストリーミング取り込みに関する理解を支援するために設計されており、上記の概念の概要を示しています。

>[!VIDEO](https://video.tv.adobe.com/v/28425?quality=12&learn=on)

### ストリームプロファイルレコードと[!DNL ExperienceEvents]

ストリーミング取り込みにより、ユーザーはプロファイルレコードと[!DNL ExperienceEvents]を数秒でExperience Platformにストリーミングして、リアルタイムのパーソナライゼーションを促進できます。 ストリーミング取り込みAPIに送信されたすべてのデータは、データレイクに自動的に保持されます。

詳しくは、[ストリーミング接続の作成に関するガイド](../tutorials/create-streaming-connection.md)を参照してください。

### データセットへのストリーミング

データがクリーンであることを確認したら、リアルタイム顧客プロファイルと[!DNL Identity Service]のデータセットを有効にできます。

プロファイルと[!DNL Identity Service]のデータセットを有効にする方法について詳しくは、[&#x200B; データセットの設定ガイド &#x200B;](/help/profile/tutorials/dataset-configuration.md)を参照してください。

## Experience Platformでのストリーミング取り込みの予想待ち時間を教えてください。

>[!IMPORTANT]
>
>ストリーミング取り込みのガードレールは、組織全体に対応するライセンス使用権限の合計にバインドされます。 さらに、開発用サンドボックスにおけるデータの使用は、プロファイル全体の10%に制限されています。 ライセンス使用権限について詳しくは、[&#x200B; データ管理のベストプラクティスガイド &#x200B;](/help/landing/license-usage-and-guardrails/data-management-best-practices.md)を参照してください。 ストリーミングスループットに制限を設定する方法については、[&#x200B; キャパシティの概要](../../landing/license-usage-and-guardrails/capacity.md)を参照してください。

| 宛先 | 予想遅延時間 |
| --------- | ---------------- |
| リアルタイム顧客プロファイル | <ul><li>B2C データ収集の95 パーセンタイル時に15分未満。</li><li>B2B データ取り込みの95 パーセンタイル時に30分未満。</li></ul> |
| データレイク | &lt; 60 分 |

## ストリーミング取り込みに関するRPS （Request per seconds）ガイダンス

次の表は、ストリーミング取り込みの1秒当たりのリクエスト制限に関するガイダンスを示しています。

| RPS制限 | メモ |
| --- | --- |
| 毎秒1000件のリクエスト | これらは、`/collection/batch` エンドポイントを使用する場合に複数のメッセージを含めることができます。 |
| 1秒あたりに個別のメッセージを10000出 | `/collection/` エンドポイントを使用する場合、メッセージを実際のリクエストの数を減らしてグループ化できます。 |

>[!IMPORTANT]
>
>強制された制限は、デバッグ目的で同期検証を使用する場合、1分あたり&#x200B;**60 リクエストになります。**

## Adobe Experience Platform 拡張機能

Adobe Experience Platform 拡張機能を使用して、新しいストリーミング接続を作成できます。[!DNL Experience Platform]拡張機能は、[!DNL Experience Data Model]へのリアルタイム取り込み用に[!DNL Experience Platform] （XDM）でフォーマットされたビーコンを送信するアクションを提供します。 詳しくは、[Experience Platform 拡張機能](/help/tags/extensions/client/web-sdk/overview.md)に関するドキュメントを参照してください。
