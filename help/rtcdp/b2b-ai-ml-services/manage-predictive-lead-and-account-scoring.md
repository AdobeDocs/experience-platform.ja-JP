---
title: Real-Time CDP B2B でのリードおよびアカウント予測スコアリングの管理
type: Documentation
description: このドキュメントでは、Experience Platform CDP B2B でのリードおよびアカウント予測スコアリング機能の管理について説明します。
feature: Profiles, B2B
badgeB2B: label="B2B エディション" type="Informative" url="https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html newtab=true"
exl-id: fe7eb94e-5cf1-46bf-80e5-affe5735c998
source-git-commit: db57fa753a3980dca671d476521f9849147880f1
workflow-type: tm+mt
source-wordcount: '1038'
ht-degree: 4%

---

# Adobe Real-time Customer Data Platform, B2B Edition でのリードおよびアカウント予測スコアリングの管理

>[!NOTE]
>
>スコア目標を作成、変更および削除できるのは、B2B AI を管理権限を持つユーザーのみです。

このチュートリアルでは、リードおよびアカウント予測スコアリングサービスのスコア目標を管理する手順について説明します。 スコアの目標は、ユーザープロファイルまたはアカウントプロファイルに対して設定できます

## 新しいスコアを作成

新しいスコアを作成するには、サイドバーの **[!UICONTROL サービス]** を選択し、「**[!UICONTROL スコアを作成]**」を選択します。

![plas-new-score](../assets/../b2b-ai-ml-services/assets/plas-create-score.png)

**[!UICONTROL 基本情報]** 画面が表示され、プロファイルタイプの選択、名前の入力、オプションの説明が求められます。 終了したら、「**[!UICONTROL 次へ]**」を選択します。

![plas-enter-basic-information](../assets/../b2b-ai-ml-services/assets/plas-basic-information.png)

**[!UICONTROL 目標を定義]** 画面が表示されます。 ドロップダウン矢印を選択し、表示されるドロップダウンウィンドウから目標タイプを選択します。

![ 目標を選択 ](../assets/../b2b-ai-ml-services/assets/plas-define-goal.png)

**[!UICONTROL 目標の詳細]** ダイアログが開きます。 ドロップダウン矢印を選択し、表示されるドロップダウンウィンドウから目標フィールド名を選択します。

![plas-select-a-goal-field-name](../assets/../b2b-ai-ml-services/assets/plas-goal-specifics-field-name.png)

**[!UICONTROL 目標の条件]** 選択が表示されます。 ドロップダウン矢印を選択し、表示されるドロップダウンリストから「条件」を選択します。

![plas-goal-specifics-condition](../assets/../b2b-ai-ml-services/assets/plas-goal-specidics-condition.png)

**[!UICONTROL 目標値]** フィールドが表示されます。 次に、「目標の詳細 [!UICONTROL &#x200B; を設定し &#x200B;] す。 [!UICONTROL &#x200B; フィールド値を入力 &#x200B;] パネルを選択し、目標値を入力します。

>[!NOTE]
>
>複数の目標の値を追加できます。

![plas-goal-specifics-field-value](../assets/../b2b-ai-ml-services/assets/plas-goal-specifics-field-value.png)

フィールドを追加するには、「**[!UICONTROL フィールドを追加]**」を選択します。

![plas-goal-specifics-add-event](../assets/../b2b-ai-ml-services/assets/plas-goal-specifics-add-event.png)

予測期間を設定するには、ドロップダウン矢印を選択し、期間を選択します。

![plas-prediction-timeframe](../assets/../b2b-ai-ml-services/assets/plas-prediction-timeframe.png)

選択した結合ポリシーによって、ユーザープロファイルのフィールド値の選択方法が決まります。 ドロップダウン矢印を使用して、目的の結合ポリシーを選択し、「**[!UICONTROL 完了]**」を選択します。

**[!UICONTROL スコアリングの設定が完了]** ダイアログが表示され、新しいスコアが作成されたことを確認します。 **[!UICONTROL OK]** を選択します。

![plas-score-complete](../assets/../b2b-ai-ml-services/assets/plas-score-complete.png)

>[!NOTE]
>
>各スコアリングプロセスが完了するまで最大 24 時間かかることがあります。

「**[!UICONTROL サービス]**」タブに戻ると、スコアのリストで作成された新しいスコアを確認できます。

![plas-score-created](../assets/../b2b-ai-ml-services/assets/plas-score-created.png)

スコアを選択して、前回の実行の詳細に関する詳細と追加情報を表示します。

![plas-score-additional-information](../assets/../b2b-ai-ml-services/assets/plas-score-info.png)

前回の実行詳細の下に表示されるエラーコードについて詳しくは、このドキュメントの [AI パイプラインエラーコードのリード ](#leads-ai-pipeline-error-codes) の節を参照してください。

## スコアを編集

スコアを編集するには、「**[!UICONTROL サービス]**」タブでスコアを選択し、画面の右側にある追加の詳細パネルから「**[!UICONTROL 編集]**」を選択します。

![plas-edit-score](../assets/../b2b-ai-ml-services/assets/plas-edit-score.png)

**[!UICONTROL インスタンスを編集]** ダイアログが表示され、スコアの説明を編集できます。 変更を加え、「**[!UICONTROL 保存]**」を選択します。

![plas-edit-save](../assets/../b2b-ai-ml-services/assets/plas-edit-save.png)

>[!NOTE]
>
>スコアの設定は変更できません。これにより、モデルの再トレーニングと再スコアリングがトリガーになるからです。 これは、スコアを削除して新しいスコアを作成することと同等です。 スコアの設定を編集するには、このスコアをクローンするか、新しいスコアを作成する必要があります。

「**[!UICONTROL サービス]**」タブに戻されます。 スコアを選択して、更新された説明の詳細を画面の右側にある追加の詳細パネルに表示します。

## スコアのクローン

スコアをクローンするには、「**[!UICONTROL サービス]**」タブでスコアを選択し、画面の右側にある追加の詳細パネルから「**[!UICONTROL クローン]**」を選択します。

![plas-clone-score](../assets/../b2b-ai-ml-services/assets/plas-clone-score.png)

**[!UICONTROL 基本情報]** 画面が表示されます。 プロファイルタイプ、名前および説明は、元のスコアから複製されます。 これらの詳細を修正し、「**[!UICONTROL 次へ]**」を選択します。

![plas-clone-basic-info](../assets/../b2b-ai-ml-services/assets/plas-clone-basic-info.png)

**[!UICONTROL 目標を定義]** 画面が表示されます。 新しいスコアを作成する場合と同様に「目標」セクションを完了し、「**[!UICONTROL 完了]**」を選択します。

「**[!UICONTROL サービス]**」タブに戻り、新しく複製されたスコアがリストに表示されます。

>[!NOTE]
>
>この **[!UICONTROL 目標を定義]** セクションは、元のスコアから複製されません。

## スコアの削除

スコアを削除するには、「**[!UICONTROL サービス]**」タブでスコアを選択し、画面の右側にある追加の詳細パネルから「**[!UICONTROL 削除]**」を選択します。

![plas-delete-score](../assets/../b2b-ai-ml-services/assets/plas-delete-score.png)

**[!UICONTROL ドキュメントを削除]** 確認ダイアログが表示されます。 「**[!UICONTROL 削除]**」を選択します。

![plas-delete-score-confirmation](../assets/../b2b-ai-ml-services/assets/plas-delete-score-confirmation.png)

>[!NOTE]
>
>スコア定義を削除すると、個人プロファイルまたはアカウントプロファイルの予測スコアもすべて削除されますが、スコア定義用に作成されたフィールドグループは削除されません。 フィールドグループは、データモデルでは「孤立」したままになります。

「**[!UICONTROL サービス]**」タブに戻ると、リストのスコアは表示されなくなります。

## リード AI パイプラインエラーコード

| エラーコード | エラーメッセージ |
| --- | --- |
| 401 | エラー 401。 リード AI パイプラインが停止しました：アカウントのスコアリングに十分な有効なアカウントがありません。 アカウント数：{}。 |
| 402 | エラー 402。 リード AI パイプラインが停止しました：連絡先のスコア付けに有効な連絡先が不足しています。 取引先責任者数：{}。 |
| 403 | エラー 403。 リード AI パイプラインが停止しました：モデルのトレーニングに十分なアクティビティ量がありません。 イベント数：{}。 |
| 404 | エラー 404。 リード AI パイプラインが停止しました：モデルのトレーニングに十分なコンバージョンがありません。 コンバージョン数：{}。 |
| 405 | エラー 405。 リード AI パイプラインが停止しました：有効なモデルトレーニングに対してアクティビティが不十分です。 アカウントの {}% のみがアクティビティを持っています。 |
| 406 | エラー 406。 リード AI パイプラインが停止しました：有効なモデルトレーニングに対してアクティビティが不十分です。 取引先責任者の {}% のみがアクティビティを持っています。 |
| 407 | エラー 407。 リード AI パイプラインが停止しました：スコアリングデータアクティビティタイプがトレーニングデータと一致しません。 |
| 408 | エラー 408。 リード AI パイプラインが停止しました：アクティビティ機能に対して、不足している割合が高すぎます。 不足している比率：{}。 |
| 409 | エラー 409。 リード AI パイプラインが停止しました：テスト精度が低すぎます。 テスト auc: {}. |
| 410 | エラー 410。 リード AI パイプラインが停止しました：パラメーターを調整した後、テスト回数が少なすぎます。 テスト auc: {}. |
| 411 | エラー 411。 リード AI パイプラインが停止しました：トレーニングデータに、信頼性の高いモデルを作成するための十分なコンバージョンがありません。 コンバージョン：{}。 |
| 412 | エラー 412。 リード AI パイプラインが停止しました：テストデータに AUC-ROC を計算するためのコンバージョンがありません。 |

| 警告/情報コード | メッセージ |
| --- | --- |
| 100 | 情報 100。 リード AI 品質チェック：アカウント数は {} です。 |
| 101 | 情報 101。 リード AI 品質チェック：取引先責任者数は {} です。 |
| 102 | 情報 102。 リード AI 品質チェック：商談数は {} です。 |
| 103 | 情報 103。 リード数 AI 品質チェック：auc のテストの頻度が低い。 パラメーターの調整を開始します。 auc のテスト：{}。 |
| 200 | 警告 200。 リード AI 品質チェック：初歩的な機能の欠落率は {} です。 |
| 201 | 警告 201。 リード AI 品質チェック：アクティビティ機能で欠落している割合は {} です。 |

## 次の手順

このチュートリアルに従うと、スコアを正常に作成および管理できます。 詳しくは、次のドキュメントを参照してください。

* [リードおよびアカウントの予測スコアリング](/help/rtcdp/b2b-ai-ml-services/predictive-lead-and-account-scoring.md)
* [リードおよびアカウントの予測スコアリングジョブの監視](/help/dataflows/ui/b2b/monitor-profile-enrichment.md)
