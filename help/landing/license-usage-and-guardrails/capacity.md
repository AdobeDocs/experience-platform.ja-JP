---
title: ライセンスの使用状況と処理能力
description: Adobe Experience Platform内でのライセンス使用状況と容量制限について説明します。
exl-id: 38dad2f1-bd0f-4cc3-a3a6-5105ea866ea4
source-git-commit: 326710e48ea9d6eb16f62b9f288311a1d255b287
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 7%

---

# ライセンスの使用状況と容量

Adobe Experience Platformでは、の処理能力によって、組織がガードレールの上限を超えたかどうかを知ることができ、これらの問題を修正する方法に関する情報が提供されます。

Experience Platformのガードレールについて詳しくは、[Real-Time CDP ガードレールの概要 ](../../rtcdp/guardrails/overview.md) を参照してください。

## 処理能力の動作 {#behavior}

>[!CONTEXTUALHELP]
>id="platform_capacity_streamingthroughput"
>title="ストリーミングスループット"
>abstract="ストリーミングスループット値は、実稼動および開発用サンドボックスをまたいで、プロファイルサービスへのストリーミング取り込みに対する 1 秒あたりの結合されたピークインバウンドイベントを測定します。"

>[!CONTEXTUALHELP]
>id="platform_capacity_streamingaudiences"
>title="ストリーミングオーディエンス数"
>abstract="サンドボックスあたりの最大ストリーミングオーディエンス数。この数には、サンドボックスにあるエッジオーディエンス数が含まれます。"

>[!CONTEXTUALHELP]
>id="platform_capacity_edgeaudiences"
>title="Edge オーディエンス"
>abstract="サンドボックスあたりの最大エッジオーディエンス数。"

現在、Capacity は以下のサービスをサポートしています。

- ストリーミングセグメント化
- ストリーミング取り込み

これらのサービス内では、次のガードレールが追跡されます。

- ストリーミングオーディエンスの最大数は 500 です
   - 500 個のストリーミングオーディエンスのうち、エッジオーディエンスの最大数は 150 個です
- ストリーミングセグメント化の最大結合スループットは、1 秒あたり 1500 レコード（rps）です

オーディエンス処理能力は **サンドボックス** レベルにあります。 つまり、組織内のサンドボックスごとに 500 個のストリーミングオーディエンスを持つことができ、そのうち 150 個はエッジオーディエンスにすることができます。

スループット処理能力は **組織** レベルにあり、個々のサンドボックスに分散させることができます。 例えば、ストリーミングセグメント化スループットに 1,500 rps を使用する場合、実稼動サンドボックスを 1500 rps に、開発用サンドボックスを 150 rps に設定できます。

Experience Platformは、サンドボックスのスループットを 15 分のローリング間隔で計算します。 このスループットはリアルタイムで測定され、データは 60 秒ごとに更新されます。

ライセンス取得済み処理能力の 80% および 90% に達した場合、Experience Platformはアラートを発行し、指定された処理能力の上限に達しつつあることを通知します。 設定を変更して、アラートを受信または完全に削除するキャパシティの割合をカスタマイズできます。

ライセンス済み処理能力の 100% を超える使用量が発生すると、処理能力の超過と見なされます。 この時点で、パフォーマンスの遅延が発生し、サービスレベルターゲット（SLT **が保証されない** 可能性があります。

## よくある質問

次のセクションでは、容量の機能に関するよくある質問の概要を説明します。

### 合計がターゲットの最大値より小さい最大結合スループット制限を設定することはできますか？

+++ 回答

いいえ。結合スループットの最大制限 **必須** は、組織のガードレールの合計です。

+++

### 最大容量を超えた場合はどうなりますか？

+++ 回答

これは、どの処理能力を超えているかによって異なります。

現在、許可されているオーディエンスの最大数を超えても、過剰なオーディエンスは影響を受けません。 ただし、新しいオーディエンスを作成する機能は、今後制限される可能性があります。

ストリーミングスループットを超えると、取り込みとセグメント化でパフォーマンスの待ち時間が発生します。

+++

### なぜ最大処理能力に従う必要があるのですか？

+++ 回答

最大容量の範囲内で作業することで、データの一貫性を維持し、データの整合性を維持できます。

ピークイベント時に一貫したパフォーマンスを確保して、システムパフォーマンスに悪影響を与え、ダウンストリームの顧客体験に影響を与える可能性のある技術的な問題を回避し、最終的にデータの衛生状態と全体的なシステムパフォーマンスを向上させることができます。

+++

### ストリーミングセグメント化スループットを管理するためのベストプラクティスは何ですか。

+++ 回答

ストリーミングセグメント化のスループットを最適に管理するには、データセットを評価して、パーソナライゼーションに必要なデータの優先順位が付けられるようにしてください。


リアルタイム処理が必要ない場合は、ストリーミング取得ではなく、バッチ取得を使用する必要があります。

+++
