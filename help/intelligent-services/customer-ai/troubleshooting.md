---
keywords: Experience Platform；はじめに；顧客 ai；人気のトピック；顧客 ai 入力；顧客 ai 出力；顧客 ai トラブルシューティング；顧客 ai エラー
solution: Intelligent Services, Real-time Customer Data Platform
feature: Customer AI
title: 顧客 AI エラーのトラブルシューティング
topic-legacy: Getting started
description: 顧客 AI でよく発生するエラーへの回答を見つけます。
type: Documentation
exl-id: 37ff4e85-da92-41ca-afd4-b7f3555ebd43
source-git-commit: 16120a10f8a6e3fd7d2143e9f52a822c59a4c935
workflow-type: tm+mt
source-wordcount: '529'
ht-degree: 0%

---

# 顧客 AI エラーのトラブルシューティング

モデルのトレーニング、スコアリングおよび設定が失敗すると、顧客 AI にエラーが表示されます。 内 **[!UICONTROL サービスインスタンス]** セクション、 **[!UICONTROL 前回の実行ステータス]** は、次のいずれかのメッセージを表示します。 **[!UICONTROL 成功]**, **[!UICONTROL トレーニングの問題]**、および **[!UICONTROL 失敗]**.

![前回の実行ステータス](./images/errors/last-run-status.png)

次の場合 **[!UICONTROL 失敗]** または **[!UICONTROL トレーニングの問題]** が表示されたら、実行ステータスを選択してサイドパネルを開くことができます。 サイドパネルには、 **[!UICONTROL 前回の実行ステータス]** および **[!UICONTROL 前回の実行の詳細]**. **[!UICONTROL 前回の実行の詳細]** 実行が失敗した理由に関する情報が含まれています。 エラーの詳細を顧客 AI から提供できない場合は、提供されているエラーコードをサポートに連絡してください。

<img src="./images/errors/last-run-details.png" width="300" /><br />

## Chrome の匿名で顧客 AI にアクセスできません

Google Chrome の匿名モードセキュリティ設定が更新されたので、Google Chrome の匿名モードでの読み込みエラーが発生します。 この問題は、experience.adobe.com を信頼されたドメインにするために、Chrome で積極的に作業を進めています。

<img src="./images/errors/error.PNG" width="500" /><br />

### 推奨される修正

この問題に対処するには、常に cookie を使用できるサイトとして experience.adobe.com を追加する必要があります。 最初に、 **chrome://settings/cookies**. 次に、 **カスタマイズされた動作** セクションで **追加** ボタンをクリックします。 表示されるポップオーバーで、コピーして貼り付けます。 `[*.]experience.adobe.com` 次に、 **サードパーティ Cookie を含める** （このサイトのチェックボックス）。 完了したら、「 」を選択します。 **追加** を参照し、匿名で顧客 AI をリロードします。

![推奨修正](./images/errors/cookies2.gif)

## モデルの品質が低い

エラー「[!UICONTROL モデルの品質が低い。 変更した設定で新しいアプリを作成することをお勧めします]&quot;. トラブルシューティングに役立つ推奨手順を次に示します。

<img src="./images/errors/model-quality.png" width="300" /><br />

### 推奨される修正

「モデルの品質が低い」は、モデルの精度が許容可能な範囲内にないことを意味します。 顧客 AI は、トレーニング後、信頼できるモデルを構築できず、AUC（ROC 曲線の下の面積） &lt; 0.65 になりました。 エラーを修正するには、いずれかの設定パラメーターを変更し、トレーニングを再実行することをお勧めします。

まず、データの正確性を確認します。 データに、予測結果に必要なフィールドが含まれていることが重要です。

- データセットに最新の日付が含まれているかどうかを確認します。 顧客 AI は、モデルがトリガーされる際に、常にデータが最新であると想定します。
- 定義した予測および実施要件ウィンドウ内の欠落したデータを確認します。 データは、ギャップをなくして完全にする必要があります。 また、データセットが [顧客 AI の履歴データ要件](./input-output.md#data-requirements).
- スキーマフィールドプロパティ内のコマース、アプリケーション、Web および検索で、欠落しているデータを確認します。

データに問題がないように見える場合は、実施要件母集団条件を変更して、モデルを特定のプロファイルに制限してみてください ( 例： `_experience.analytics.customDimensions.eVars.eVar142` が過去 56 日間に存在している )。 これにより、トレーニングウィンドウで使用するデータの母集団とサイズが制限されます。

実施要件母集団の制限が機能しなかった場合や制限が不可能な場合は、予測ウィンドウを変更します。

- 予測期間を 7 日に変更して、エラーが引き続き発生するかどうかを確認します。 エラーが発生しなくなった場合は、定義した予測ウィンドウに十分なデータがない可能性があることを示します。
