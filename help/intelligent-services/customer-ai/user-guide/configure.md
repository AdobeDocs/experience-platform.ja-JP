---
keywords: Experience Platform;user guide;customer ai;popular topics;configure instance;create instance;
solution: Experience Platform
title: 顧客AIインスタンスの設定
topic: Instance creation
translation-type: tm+mt
source-git-commit: ec0de4c8775367be9e6016529471254ad9f8f453

---


# 顧客AIインスタンスの設定

顧客AIは、インテリジェントサービスの一部であり、機械学習を気にすることなく、カスタムの傾向スコアを生成できます。

インテリジェントサービスは、様々な用途に対して設定できる使いやすいAdobe SenseiサービスとしてCustomer AIを提供します。 次の節では、顧客 AI のインスタンスを設定する手順を説明します。

## インスタンスの設定 {#set-up-your-instance}

In the Platform UI, click **[!UICONTROL Services]** in the left navigation. The **[!UICONTROL Services]** browser appears and displays all available services at your disposal. In the container for Customer AI, click **[!UICONTROL Open]**.

![](../images/user-guide/navigate-to-service.png)

*顧客 AI* 画面には、既存のすべての顧客 AI インスタンスが表示されます。「**[!UICONTROL Create instance]**」をクリックします。

![](../images/user-guide/dashboard.png)

「*設定*」から始まり、インスタンス作成ワークフローが表示されます。

以下に、インスタンスに指定する必要がある値に関する重要な情報を示します。

* インスタンスの名前は、顧客AIスコアが表示されるすべての場所で使用されます。 したがって、「雑誌購読をキャンセルする可能性」のように、予測スコアが表すものを名前に記述する必要があります。

* 傾向タイプによって、スコアと指標の極性の意図が決まります。またはを選択でき **[!UICONTROL Churn]** ま **[!UICONTROL Conversion]**&#x200B;す。 傾向タイプがインスタンスに与える影響の詳細については、「 [スコアリングの概要](./discover-insights.md#scoring-summary) 」の「インサイトの検出」ドキュメントを参照してください。

* データソースは、データが存在する場所です。 データセットは、スコアの予測に使用される入力データセットです。 設計上、顧客 AI はコンシューマーエクスペリエンスイベントデータを使用して傾向スコアを計算します。ドロップダウンセレクターからデータセットを選択すると、顧客AIと互換性のあるもののみが表示されます。

* デフォルトでは、適格な母集団が指定されていない限り、すべてのプロファイルに対して傾向スコアが生成されます。イベントに基づいてプロファイルを含めたり除外したりする条件を定義することで、適格な母集団を指定できます。

Provide the required values and then click **[!UICONTROL Next]**.

![](../images/user-guide/setup.png)

### 目標の定義 {#define-a-goal}

*目標の定義*&#x200B;手順が表示され、目標を視覚的に定義できるインタラクティブな環境が提供されます。目標は 1 つ以上のイベントで構成され、各イベントの発生は保持する条件に基づきます。顧客 AI インスタンスの目的は、特定の期間内に目標を達成する可能性を判断することです。

をクリック **[!UICONTROL Enter Field Name]** して、ドロップダウンリストからフィールドを選択します。 2 つ目の入力をクリックし、イベントの条件句を選択し、イベントを完了するターゲット値を指定します。Additional events can be configured by clicking **[!UICONTROL Add event]**. Lastly, complete the goal by applying a prediction time frame in number of days, then click **[!UICONTROL Next]**.

![](../images/user-guide/goal.png)

### スケジュールの設定&#x200B;*（オプション）*{#configure-a-schedule}

*詳細*&#x200B;手順が表示されます。This optional step allows you to configure a schedule to automate prediction runs, define prediction exclusions to filter certain events, or click **[!UICONTROL Finish]** if nothing is needed.

「*スコアリング頻度*」を設定して、スコアリングスケジュールを設定します。予測の自動実行は、週単位または月単位でスケジュールできます。

![](../images/user-guide/schedule.png)

スケジュール設定では、特定の条件を満たすイベントがスコアの生成時に評価されるのを防ぐために、予測の除外を定義できます。この機能を使用して、無関係なデータ入力を除外できます。

To exclude certain events, click **[!UICONTROL Add exclusion]** and define the event in the same fashion as to how the goal is defined. To remove an exclusion, click the ellipses (**[!UICONTROL ...]**) to the top-right of the event container and then click **[!UICONTROL Remove Container]**.

![](../images/user-guide/exclusion.png)

Exclude events as needed and then click **[!UICONTROL Finish]** to create the instance.

![](../images/user-guide/advanced.png)

インスタンスが正常に作成されると、予測実行は直ちにトリガされ、定義したスケジュールに従って後続の実行が実行されます。

>[!NOTE] 入力データのサイズに応じて、予測の実行が完了するまでに最大24時間かかる場合があります。

この節では、顧客 AI のインスタンスを設定し、予測実行が実行されました。ランが成功したときに、インサイトに予測されたスコアがプロファイルに自動的に入力されます。 このチュートリアルの次のセクションに進む前に、最大24時間お待ちください。

## 次の手順 {#next-steps}

このチュートリアルに従うと、顧客AIのインスタンスの設定と傾向スコアの生成に成功します。 セグメントビルダーを使用して、予測スコアを含む顧客セグメントを [作成したり、Customer AIでインサイトを](./create-segment.md) 発見したりできるようになりました [](./discover-insights.md)。

## その他のリソース

次のビデオは、お客様向けAIの設定ワークフローを理解していただくためのものです。 さらに、ベストプラクティスと使用例が示されています。

>[!VIDEO](https://video.tv.adobe.com/v/32665?learn=on&quality=12)

