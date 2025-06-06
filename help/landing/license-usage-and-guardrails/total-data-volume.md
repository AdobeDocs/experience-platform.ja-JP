---
title: 合計データ量指標
description: 新しい合計データ量指標と、以前の平均プロファイル充実性指標に代わる方法について説明します。
exl-id: 4b21d25c-b82b-4d1a-83ce-b510f02fd160
source-git-commit: 62f5ecf82df46284365e64d633c8242ac45567bc
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 1%

---

# 合計データ量指標

合計データボリュームは、プロファイルストアの使用権限に対する使用状況を追跡するためのよりシンプルで説明しやすい方法として、以前の平均プロファイル充実性指標に代わるものです。

**合計データ量= アドレス可能なオーディエンス ×平均プロファイル充実度**

この指標は、**プロファイルストア** に保存され、リアルタイム顧客プロファイルエンゲージメントワークフローで使用できるデータの総量を反映します。 **データレイク** に保存されたデータは含まれません **。** この変更により、プロファイルベースのパーソナライゼーションとエンゲージメントに関連するデータに関して、より焦点を絞った透過的なビューが提供されます。

## よくある質問 {#faq}

次の節では、この更新に関するよくある質問を一覧表示しています。

### この変更が行われた理由

この変更は、ライセンス使用権限指標に準拠し続けるプロセスを簡素化するために行われました。 平均的なプロファイルの充実度は、理解し管理するのが難しいと感じるお客様から、よく耳にします。 この変更により、リアルタイム顧客プロファイルのライセンス使用状況をより深く理解し、管理できるようになることを願っています。

### 指標のこの変更により、プロファイルストアの使用権限は変更されますか。

いいえ。プロファイルストアの使用権限は以前と同じままです。これは、合計データ量は、アドレス可能なオーディエンスに「平均プロファイル充実度」を掛けた値に相当するからです。

### この変更は、購入した使用権限パッケージに影響しますか？

いいえ。以前に購入した使用権限パッケージの利点は引き続き得られます。

### プロファイルストアの使用権限と比較して、合計データ量に異なる値が表示されるのはなぜですか。

合計データボリュームは、エンゲージメントワークフローでプロファイルが使用できるデータの **合計** 量を表します。 測定がより決定論的で説明可能になるように更新されました。 ほとんどのユーザーは **合計データ量と合計プロファイルストレージの間に大きな変化が見られる** ありません）。 ご不明な点がございましたら、カスタマーサービス担当者までご連絡ください。

### 合計データ量の計算方法 これには、データレイクストレージは含まれますか？

合計データ量は、次の式を使用して計算されます：

**合計データ量= アドレス可能なオーディエンス ×平均プロファイル充実度**

この指標は、リアルタイム顧客プロファイルがエンゲージメントワークフローで使用できる、プロファイルストアに保存された合計データ量を反映します。 データレイクに保存されたデータは **含まれません**。

以前は、一部のレガシーオファーでは、プロファイルストアとデータレイクの両方の使用状況を組み合わせた「合計ストレージ」指標を使用していました。 ただし、合計データ量はプロファイルストアのみに制限され、プロファイルベースのエンゲージメントに関連するデータのより焦点を絞ったビューを提供します。

### 平均プロファイル充実度の管理を継続するには、変更を再作成する必要がありますか？

いいえ。データセットのプロファイルのフィルタリングや無効化、エクスペリエンスイベントの有効期限と偽名プロファイルデータの有効期限の設定などのアクションは、引き続き合計データ量の管理に役立ちます。

### サンドボックスに取り込んだファイルのサイズが、合計データボリュームと異なるのはなぜですか？

プロファイルは、システムの設計対象となるパーソナライゼーションワークフローおよびエンゲージメントワークフローを実行するための迅速な処理のために、データを最適化します。 クライアント側のファイルサイズは、エンコーディング、圧縮、形式のタイプなどの要因により、合計データ量とは異なる場合があります。

### この変更は、プロファイルとデータレイクの両方で共有制限がある SKU に適用されますか。

プロファイルとデータレイクの間で平均プロファイル充実度を共有している SKU を使用しているお客様には、引き続き「合計使用量ストレージ」指標が表示され、平均プロファイル充実度指標は表示 **されません**。
