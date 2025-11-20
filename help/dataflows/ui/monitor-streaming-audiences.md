---
title: ストリーミングオーディエンスの監視
description: 監視ダッシュボードを使用して、ストリーミングセグメント化を使用して評価されるオーディエンスを監視する方法について説明します
hide: true
hidefromtoc: true
source-git-commit: 6fe0a36a8f2ac2cb954935ee8fe64432442b6e84
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 9%

---


# ストリーミングオーディエンスの監視

導入部ぼかし

## 基本を学ぶ

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ データフロー ](../home.md)：データフローは、Experience Platform間で情報を転送するデータジョブを表します。 様々なサービスにわたって設定され、ソースコネクタからターゲットデータセット、ID サービス、リアルタイム顧客プロファイル、宛先へのデータの移動を容易にします。
* [ セグメント化サービス ](../../segmentation/home.md):
* [ 処理能力 ](../../landing/license-usage-and-guardrails/capacity.md):Experience Platformの処理能力を使用すると、組織がガードレールの上限を超えたかどうかを把握でき、これらの問題を修正する方法に関する情報が得られます。

## ストリーミングオーディエンスの指標の監視 {#streaming-audience-metrics}

>[!CONTEXTUALHELP]
>id="platform_monitoring_streaming_audience_evaluation_rate"
>title="評価率"
>abstract="この指標は、1 秒あたりに評価されるレコードの数を表します。"
>text="Learn more in documentation"

>[!CONTEXTUALHELP]
>id="platform_monitoring_streaming_audience_p95_latency"
>title="P95 取り込み待ち時間"
>abstract="この指標は、オーディエンスの評価に成功するまでの、Adobe Experience Platformに到着するイベントの 95 パーセンタイルの待ち時間を測定します。"
>text="Learn more in documentation"

次の表は、ストリーミングオーディエンスに使用される指標に関する詳細情報を示しています。

| 指標 | 説明 | ディメンション |
| ------ | ----------- | ---------- |
| 評価率 | 1 秒あたりに処理されたオーディエンス評価の数。 | サンドボックス、データセット |
| P95 取り込み待ち時間 | オーディエンスに成功したイベント到着の 95 パーセンタイルの待ち時間。 | サンドボックス、データセット |
| 受信したレコード | 選択した時間枠にストリーミングセグメント化のストリーミング取り込みから受信したレコードの合計数です。 | データセット |
| 評価されたレコード | 選択した時間枠でストリーミングセグメント化を使用して評価された **正常に** レコードの合計数です。 | データセット |
| 失敗したレコード | 選択した時間枠内のエラーが原因で、ストリーミングセグメント化で評価 **失敗** したレコードの合計数です。 | データセット、フロー実行 |
| スキップされたレコード | 選択した時間枠内のエラーが原因で、ストリーミングセグメント化で評価が **スキップ** されたレコードの合計数です。 | データセット、フロー実行 |
| 認定されたプロファイル | 選択した時間枠にオーディエンスに選定されたプロファイルの数。 | サンドボックス、オーディエンス |
| 不適格なプロファイル | 選択した時間枠にオーディエンスから不適格となったプロファイルの数。 | サンドボックス、オーディエンス |

{style="table-layout:auto"}

## ストリーミングオーディエンスの監視ダッシュボードの使用 {#monitoring-dashboard}

ストリーミングオーディエンスのモニタリングダッシュボードにアクセスするには、Experience Platform UI に移動し、左側のナビゲーションから「**[!UICONTROL Monitoring]**」を選択してから「**[!UICONTROL Streaming end-to-end]**」を選択します。

画像

ダッシュボードの上部には、**[!UICONTROL Audiences]** の指標カードがあります。 これにより、オーディエンスの **評価率** に関する情報が表示されます。