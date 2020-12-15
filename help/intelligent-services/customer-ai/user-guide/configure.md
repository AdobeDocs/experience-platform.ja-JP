---
keywords: Experience Platform;user guide;customer ai;popular topics;configure instance;create instance;
solution: Experience Platform, Intelligent Services, Real-time Customer Data Platform
title: 顧客AIインスタンスの設定
topic: Instance creation
description: インテリジェントサービスは、様々な用途に設定できる、使いやすい Adobe Sensei サービスとして顧客 AI を提供します。次の節では、顧客 AI のインスタンスを設定する手順を説明します。
translation-type: tm+mt
source-git-commit: de16ebddd8734f082f908f5b6016a1d3eadff04c
workflow-type: tm+mt
source-wordcount: '1284'
ht-degree: 35%

---


# 顧客AIインスタンスの設定

顧客 AI は、インテリジェントサービスの一部として、機械学習を気にすることなく、カスタムの傾向スコアを生成できます。

インテリジェントサービスは、様々な用途に設定できる、使いやすい Adobe Sensei サービスとして顧客 AI を提供します。次の節では、顧客 AI のインスタンスを設定する手順を説明します。

## インスタンスの設定 {#set-up-your-instance}

In the Platform UI, select **[!UICONTROL Services]** in the left navigation. 「**[!UICONTROL サービス]**」ブラウザーが表示され、使用可能なすべてのサービスが表示されます。In the container for Customer AI, select **[!UICONTROL Open]**.

![](../images/user-guide/navigate-to-service.png)

「 **Customer AI** 」 UIが表示され、すべてのサービスインスタンスが表示されます。

- 「スコア対象プロファイル **[!UICONTROL の合計]** 」指標は、インスタンスを **[!UICONTROL 作成]** コンテナの右下にあります。 この指標は、すべてのSandbox環境および削除されたサービスインスタンスを含む、現在のカレンダー年度のCustomer AIによってスコアされたプロファイルの合計数を追跡します。

![](../images/user-guide/total-profiles.png)

サービスインスタンスは、UIの右側のコントロールを使用して、編集、複製および削除できます。 これらのコントロールを表示するには、既存の **[!UICONTROL サービスインスタンスからインスタンスを選択し]**&#x200B;ます。 コントロールには、次の項目が含まれます。

- **[!UICONTROL 編集]**:「 **[!UICONTROL 編集]** 」を選択すると、既存のサービスインスタンスを変更できます。 インスタンスの名前、説明およびスコアリング頻度を編集できます。
- **[!UICONTROL クローン]**:「 **[!UICONTROL Clone]** 」を選択すると、現在選択されているサービスインスタンスのセットアップがコピーされます。 その後、ワークフローを変更してマイナーツイークを作成し、新しいインスタンスとして名前を変更できます。
- **[!UICONTROL 削除]**:過去の実行を含むサービスインスタンスを削除できます。
- **[!UICONTROL データソース]**:このインスタンスで使用されるデータセットへのリンク。
- **[!UICONTROL 前回の実行の詳細]**:これは、実行に失敗した場合にのみ表示されます。 エラーコードなど、実行に失敗した理由に関する情報がここに表示されます。
- **[!UICONTROL スコアの定義]**:このインスタンスに対して設定した目標の概要を簡単に示します。

![](../images/user-guide/service-instance-panel.png)

新しいインスタンスを作成するには、「インスタンスを **[!UICONTROL 作成]**」を選択します。

![](../images/user-guide/dashboard.png)

「**[!UICONTROL 設定]**」から始まり、インスタンス作成ワークフローが表示されます。

インスタンスに指定する必要がある値に関する重要な情報を次に示します。

- インスタンスの名前は、顧客AIスコアが表示されるすべての場所で使用されます。 したがって、「雑誌購読をキャンセルする可能性」など、予測スコアが何を表すかを、名前で示す必要があります。

- 傾向タイプによって、スコアと指標の極性の意図が決まります。「**[!UICONTROL チャーン]**」または「**[!UICONTROL コンバージョン]**」を選択できます。傾向タイプがインスタンスに与える影響の詳細については、「インサイトの検出」ドキュメントの[スコアリングの概要](./discover-insights.md#scoring-summary)のを参照してください。

- データソースは、データが存在する場所です。データセットは、スコアの予測に使用される入力データセットです。設計上、顧客 AI はコンシューマーエクスペリエンスイベントデータを使用して傾向スコアを計算します。ドロップダウンセレクターからデータセットを選択すると、顧客 AI と互換性のあるもののみが表示されます。

- デフォルトでは、適格な母集団が指定されていない限り、すべてのプロファイルに対して傾向スコアが生成されます。イベントに基づいてプロファイルを含めたり除外したりする条件を定義することで、適格な母集団を指定できます。

Provide the required values and then select **[!UICONTROL Next]**.

![](../images/user-guide/setup.png)

### 目標の定義 {#define-a-goal}

The **[!UICONTROL Define goal]** step appears and it provides an interactive environment for you to visually define a prediction goal. 目標は 1 つ以上のイベントで構成され、各イベントの発生は保持する条件に基づきます。顧客 AI インスタンスの目的は、特定の期間内に目標を達成する可能性を判断することです。

目標を作成するには、「 **[!UICONTROL Enter Field Name]** 」を選択し、ドロップダウンリストからフィールドを選択します。 2つ目の入力を選択し、イベントの条件の句を選択して、イベントを完了するターゲット値を指定します。 Additional events can be configured by selecting **[!UICONTROL Add event]**. Lastly, complete the goal by applying a prediction time frame in number of days, then select **[!UICONTROL Next]**.

![](../images/user-guide/goal.png)

#### 発生し、発生しない

目標を定義する際、「発生 **[!UICONTROL する]** 」または「発生しない **[!UICONTROL 」を選択でき]**&#x200B;ます。 「発生 **[!UICONTROL する]** 」を選択すると、顧客のイベントデータをInsights UIに含めるために、定義したイベント条件を満たす必要があります。

例えば、顧客が購入するかどうかを予測するアプリを設定する場合は、「発生 **[!UICONTROL する」を選択し]** 、「すべて **[!UICONTROL 」を選択して「]** コマース **.購入.id」を「演算子」に****** 指定します。

![発生する](../images/user-guide/occur.png)

ただし、あるイベントが特定の期間に発生しないかどうかを予測したい場合があります。 このオプションで目標を設定するには、[ **[!UICONTROL 発生しない]** ]ドロップダウンを選択します。

例えば、関与が少なくなり、次の月にアカウントのログインページを訪問しない顧客を予測したい場合、 「発生し **[!UICONTROL ない」を選択し]** 、「次のす **[!UICONTROL べて」に続けて「発生しない」を選択し]** 、次に「web.webWebInteraction.URL」と入力します。 **等しい」は、「次の値を含む********** アカウントログイン」の演算子です。

![発生しない](../images/user-guide/not-occur.png)

#### すべての

場合によっては、イベントの組み合わせが発生するかどうかを予測したい場合や、あらかじめ定義されたセットから任意のイベントの発生を予測したい場合があります。 顧客がイベントの組み合わせを持つかどうかを予測するには、「目標の **[!UICONTROL 定義]** 」ページの第2レベルのドロップダウンから **** 、「すべて」オプションを選択します。

例えば、顧客が特定の製品を購入したかどうかを予測できます。 この予測目標は、次の2つの条件で定義されます。が `commerce.order.purchaseID` 存在し **、** が特定の値と等しい `productListItems.SKU`**** 。

![すべての例](../images/user-guide/all-of.png)

特定のセットから顧客が何らかのイベントを持つかどうかを予測するには、「 **[!UICONTROL いずれか]** 」オプションを使用します。

例えば、あるURLや特定の名前を持つWebページに顧客が訪問するかを予測できます。 この予測目標は、次の2つの条件で定義されます。 `web.webPageDetails.URL` **特定の値を持つ開始** 、および特定の値を持つ `web.webPageDetails.name` 開始 **** 。

![任意の例](../images/user-guide/any-of.png)

### スケジュールの設定&#x200B;*（オプション）*{#configure-a-schedule}

The **[!UICONTROL Advanced]** step appears. This optional step allows you to configure a schedule to automate prediction runs, define prediction exclusions to filter certain events, or select **[!UICONTROL Finish]** if nothing is needed.

「**[!UICONTROL スコアリング頻度]**」を設定して、スコアリングスケジュールを設定します。予測の自動実行は、週単位または月単位でスケジュールできます。

![](../images/user-guide/schedule.png)

スケジュール設定では、特定の条件を満たすイベントがスコアの生成時に評価されるのを防ぐために、予測の除外を定義できます。この機能を使用して、無関係なデータ入力を除外できます。

To exclude certain events, select **[!UICONTROL Add exclusion]** and define the event in the same fashion as to how the goal is defined. To remove an exclusion, select the ellipses (**[!UICONTROL ...]**) to the top-right of the event container and then select **[!UICONTROL Remove Container]**.

![](../images/user-guide/exclusion.png)

Exclude events as needed and then select **[!UICONTROL Finish]** to create the instance.

![](../images/user-guide/advanced.png)

インスタンスが正常に作成されると、予測実行が直ちにトリガーされ、定義したスケジュールに従って後続の予測実行が実行されます。

>[!NOTE]
>
>入力データのサイズによっては、予測の実行が完了するまでに最大 24 時間かかる場合があります。

この節では、顧客 AI のインスタンスを設定し、予測実行が実行されました。実行が正常に完了すると、スコアリングされた洞察が、予測されたスコアと共にプロファイルを自動入力します。このチュートリアルの次の節に進む前に、最大 24 時間お待ちください。

## 次の手順 {#next-steps}

このチュートリアルに従うと、顧客AIのインスタンスの設定と傾向スコアの生成に成功します。 セグメントビルダーを使用して、予測スコアを含む顧客セグメントを [作成したり、Customer AIでインサイトを](./create-segment.md) 発見したりできるようになりました [](./discover-insights.md)。

## その他のリソース

次のビデオは、お客様向けAIの設定ワークフローを理解していただくためのものです。 さらに、ベストプラクティスと使用例が示されています。

>[!VIDEO](https://video.tv.adobe.com/v/32665?learn=on&quality=12)

