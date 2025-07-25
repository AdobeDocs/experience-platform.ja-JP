---
title: 送信時間最適化モデルの詳細
description: Adobe Journey Optimizerでの送信時間の最適化に使用する AI モデルについて説明します。
hide: true
hidefromtoc: true
exl-id: 95e1fc8f-1817-40d7-aa55-93daa50f43c0
source-git-commit: d1c7d1038b45bae4c014e3488f55a427c9cb02ed
workflow-type: tm+mt
source-wordcount: '1220'
ht-degree: 7%

---

# 送信時間最適化モデルの詳細

## モデルの概要 {#model-overview}

* **モデル名とバージョン**：送信時間の最適化
* **モデルリリース日**:2024 年 9 月
* **モデルの目的**:Adobe Journey Optimizerの送信時間最適化モデルは、メールおよびプッシュメッセージの最適な送信時間を選択して、コンシューマーの過去のオープンおよびクリック行動に基づいて、コンシューマーエンゲージメントを最大限に高めます。
* **対象ユーザー**：このモデルの主なユーザーは、マーケティング担当者、製品管理者、および顧客エンゲージメントチームで、Adobe Journey Optimizerを活用してデータ駆動型のマーケティング戦略を推進します。
* **ユースケース**：送信時間の最適化は、緊急を要しないマーケティング通信（毎週の広告、新製品のプロモーション情報、1 か月の販売に関する情報など）に最適です。 送信時間の最適化は、Journey Optimizerに組み込まれているメールおよびプッシュのアクションタイプでのみ使用できます。現在、カスタムアクションを介して送信されたメッセージや、その他のアクションタイプでは使用できません。
* **誤用の可能性**：送信時間の最適化は、緊急で時間に依存する操作メッセージ（注文確認、パスワードリセット通知、フライトゲート変更通知など）には使用しないでください。

## モデルの詳細 {#model-details}

* **モデルタイプ**：送信時間最適化モデルは、組織のAdobe Journey Optimizer消費者の行動データを取り込み、ユーザーレベルのオープンイベントとクリックイベントを調べて、消費者がメッセージにエンゲージする可能性が最も高いタイミングを予測します。 週の各時間の消費者レベルのオープンおよびクリック動作は重み付けされ、[!DNL Bayesian] 推定量を使用して類似および全体的な消費者行動と組み合わされます。 その後、週の各時間に対する [!DNL Bayesian] の予測をランク付けし、各指標（メールの開封数、メールのクリック数、プッシュの開封数）の「ヒートマップ」を顧客ごとに作成して、各消費者に連絡する時間が最も多く、望ましいエンゲージメント結果につながる可能性が最も低い週の時間を予測します。
* **入力**：送信時間の最適化は、指定されている場合、[!UICONTROL &#x200B; 環境設定の詳細 &#x200B;] フィールドグループ内の `timeZone` フィールドの消費者のタイムゾーンデータを使用して、消費者のタイムゾーンを判断します。 消費者のタイムゾーンが `timeZone` フィールドで使用できない場合、Send-Time Optimization は、[ 住所データタイプ ](../../xdm/data-types/postal-address.md) を使用して、消費者のプロファイルに保存されている最初の住所と最も一般的なタイムゾーンの一致に基づいて、消費者のタイムゾーンの推定を試みます。 送信時間の最適化は、次の 3 種類の行動データに基づいて各消費者に対する予測を行います。
   * コンシューマー全体のオープンおよびクリック動作。
   * 同じタイムゾーンでの類似コンシューマーのオープンおよびクリック動作。
   * その個々の消費者のオープンおよびクリック動作。
* **出力**：これらの予測は、重み付けされ、[!DNL Bayesian] アプローチを使用して組み合わされます。次のヒートマップの例に示すように、各顧客の各指標（メール開封数、メールのクリック数、プッシュの開封数）の「ヒートマップ」が得られ、その顧客に連絡した時間が目的のエンゲージメント結果（開封/クリック数）になる可能性が最も高く最も低いことを示します。

![ 送信時間の最適化のヒートマップ。](../images/models/send-time-optimization.png)

* **入力および出力例**：プロファイル充実度に対するモデルの影響を最小限に抑えるために、モデルスコアは `_experience.intelligentServices.journeyAI.sendTimeOptimization` に保存された 3 つのプロファイル属性に保存および圧縮されます。また、人間が判読できるように設計されていません。

## モデルトレーニング {#model-training}

* **トレーニングデータと前処理**：各組織のトレーニングデータセットは、Adobe Experience Platform内の独自のデータからのみ得られます。
   * 組織で送信時間の最適化機能が有効になると、送信時間の最適化を使用するかどうかに関係なく、過去 16 週間にわたるすべての組織のジャーニーおよびアクションにわたって、メールとプッシュ、送信、開く、クリックのイベントに関するモデルのトレーニングが行われます。 これにより、送信時間の最適化は、コンシューマーが生成したすべてのデータを活用できます。
   * モデルは、最初にトレーニングが行われ、毎週スコアリングされます。16 週間後、モデルは毎月、再トレーニングが行われ、再スコアリングされます。モデルスコアリングには、前回のスコアリング実行以降に作成されたすべての顧客プロファイル（既存と新規の両方）が含まれます。
   * 送信時間の最適化によって送信されるメッセージは、次のいずれかを受け取ります。
      * 様々な送信時間をテストし、消費者の反応を監視するために選択された「探索」メッセージ送信時間。
      * クリック率/開封率を最大化するために選択された、「最適化」されたメッセージ送信時間。 送信イベントの 5％は「探索」送信時間を受信し、送信イベントの 95％は「最適化」されます。
   * 探索送信時間は、設定された最大待機時間で使用可能な送信時間からランダムに選択されます。例えば、水曜日の午前 9 時に、送信時間の最適化がオンで、最大待機時間が 3 時間のメッセージを選択した場合、メッセージの探索の送信時間は午前 9 時、午前 10 時、午前 11 時、午後 12 時に均等に分割されます。

## モデル評価 {#model-evaluation}

* **モデル評価**：送信時間の最適化により、組織が最適化したすべてのメッセージで、メールのクリック率とプッシュの開封率が約 2～10% の範囲で高くなる場合があります。
   * 例えば、送信時間の最適化を行わずにメールを送信した組織のクリック率が平均で 5.0% の場合、送信時間の最適化を行った同じセットのメールでは、平均で 5.5% のクリック率（5.0% * （1+10%） = 5.5%）が生じる場合があります。
   * 小さなサンプルサイズ内では変動が生じるので、単一メッセージの送信では送信時間の最適化によるメリットが確認できない場合があります。
   * 組織は、次のような場合に、送信時間の最適化を使用することでより大きなメリットが得られる可能性が高くなります。
      * 既存のジャーニーは、固定されており、適切に最適化されていない送信時間を使用します。
      * 消費者の行動（クリック数と開封数）のばらつきは、消費者の場所と好みに対応します。
      * 組織では、より多くのメールおよびプッシュメッセージで送信時間の最適化を使用します。
      * 組織は、6～12 時間の推奨範囲内で最大待機時間を選択します。

## モデルの導入 {#model-deployment}

* **モデルの更新**：モデルは最初にトレーニングされ、毎週採点されます。 16 週間後、モデルは毎月、再トレーニングが行われ、再スコアリングされます。モデルスコアリングには、すべての消費者プロファイル（前回のスコアリング実行以降に作成された既存と新規の両方）が含まれます。

## 公平性と偏見 {#fairness-and-bias}

* **モデルの公平性**：消費者のタイムゾーンを誤って推論すると、特定の消費者に最適なものよりも早く、または特定の消費者に最適なものよりも遅くメッセージが送信される場合があります。 ただし、送信時間の最適化を利用した通信では、すべてのコンシューマーが最終的にメッセージを受け取り、メッセージを操作する機会を得ることができます。 さらに、このモデルでは、消費者の人口統計データや人口統計データ用のプロキシは利用されません。消費者の行動および消費者のタイムゾーンデータのみが利用されます。 したがって、公平性に関する懸念は限られ、軽減されます。
* **データバイアス**：消費者のタイムゾーンを誤って推論すると、特定の消費者に最適なものよりも早く、または特定の消費者に最適なものよりも後でメッセージが送信される場合があります。 ただし、送信時間の最適化を利用した通信では、すべてのコンシューマーが最終的にメッセージを受け取り、メッセージを操作する機会を得ることができます。 さらに、このモデルでは、消費者の人口統計データや人口統計データ用のプロキシは利用されません。消費者の行動および消費者のタイムゾーンデータのみが利用されます。 したがって、バイアスの懸念は制限され、軽減されます。

## 堅牢性 {#robustness}

* **モデルの堅牢性**：十分なインタラクションイベントデータを持たないプロファイル（多くの場合、新しいプロファイルの場合）は、即時送信時間、選択したウィンドウ内での探査送信時間、またはすべての顧客で最もパフォーマンスの高い送信時間に基づく送信時間を受け取ります。

## 倫理的考慮事項 {#ethical-considerations}

* **モデルに関連する倫理的考慮事項**：消費者のタイムゾーンを誤って推論すると、特定の消費者に最適なものよりも早く送信される場合や、特定の消費者に最適なものよりも遅く送信される場合があります。 ただし、送信時間の最適化を利用した通信では、すべてのコンシューマーが最終的にメッセージを受け取り、メッセージを操作する機会を得ることができます。 さらに、このモデルでは、消費者の人口統計データや人口統計データ用のプロキシは利用されません。消費者の行動および消費者のタイムゾーンデータのみが利用されます。 したがって、倫理的な懸念は限られ、軽減されます。