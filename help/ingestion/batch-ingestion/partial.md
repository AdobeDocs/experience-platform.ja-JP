---
keywords: Experience Platform;home;popular topics;batch ingestion;Batch ingestion;partial ingestion;Partial ingestion;Retrieve error;retrieve error;Partial batch ingestion;partial batch ingestion;partial;ingestion;Ingestion;
solution: Experience Platform
title: 部分的なバッチ取り込みの概要
topic: overview
description: このドキュメントでは、部分バッチ取得を管理するためのチュートリアルを提供します。
translation-type: tm+mt
source-git-commit: f86f7483e7e78edf106ddd34dc825389dadae26a
workflow-type: tm+mt
source-wordcount: '915'
ht-degree: 36%

---


# 部分バッチ取得

部分バッチ取得は、エラーを含むデータを特定のしきい値まで取得する機能です。この機能を使用すると、正しいデータをすべて Adobe Experience Platform に正しく取得し、間違ったデータはその理由の詳細とともにすべて別々にバッチ処理されます。

このドキュメントでは、部分バッチ取得を管理するためのチュートリアルを提供します。

## はじめに

このチュートリアルでは、部分バッチ取得に関わる様々な Adobe Experience Platform サービスに関する十分な知識が必要です。このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [バッチインジェスト](./overview.md):CSVやParketなどのデータファイルからデータを [!DNL Platform] 取り込んで保存する方法。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md):顧客体験データを [!DNL Platform] 整理する際に使用される標準化されたフレームワーク。

The following sections provide additional information that you will need to know in order to successfully make calls to [!DNL Platform] APIs.

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切に書式設定されたリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

In order to make calls to [!DNL Platform] APIs, you must first complete the [authentication tutorial](../../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

- Authorization: Bearer `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

All resources in [!DNL Experience Platform] are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in:

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>For more information on sandboxes in [!DNL Platform], see the [sandbox overview documentation](../../sandboxes/home.md).

## Enable a batch for partial batch ingestion in the API {#enable-api}

>[!NOTE]
>
>この節では、APIを使用した部分的なバッチ取り込みに対してバッチを有効にする方法を説明します。 For instructions on using the UI, please read the [enable a batch for partial batch ingestion in the UI](#enable-ui) step.

部分的な取り込みが有効な新しいバッチを作成できます。

新しいバッチを作成するには、『 [バッチインジェスト開発者ガイド](./api-overview.md)』の手順に従います。 Once you reach the **[!UICONTROL Create batch]** step, add the following field within the request body:

```json
{
    "enableErrorDiagnostics": true,
    "partialIngestionPercentage": 5
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `enableErrorDiagnostics` | バッチに関する詳細なエラーメッセージ [!DNL Platform] を生成するためのフラグ。 |
| `partialIngestionPercentage` | バッチ全体が失敗する前の許容エラー率。したがって、この例では、バッチが失敗する前に、最大5%のエラーが発生する可能性があります。 |


## Enable a batch for partial batch ingestion in the UI {#enable-ui}

>[!NOTE]
>
>この節では、UIを使用した部分的なバッチ取り込みに対してバッチを有効にする方法について説明します。 APIを使用してバッチの部分的な取り込みを既に有効にしている場合は、次のセクションに進むことができます。

バッチを [!DNL Platform] UIから部分的に取り込むために有効にするには、ソース接続を介して新しいバッチを作成するか、既存のデータセットに新しいバッチを作成するか、「[!UICONTROL CSVをXDMフローにマップ]」を介して新しいバッチを作成します。

### 新しいソース接続の作成 {#new-source}

新しいソース接続を作成するには、 [ソースの概要に記載されている手順に従います](../../sources/home.md)。 Once you reach the **[!UICONTROL Dataflow detail]** step, take note of the **[!UICONTROL Partial ingestion]** and **[!UICONTROL Error diagnostics]** fields.

![](../images/batch-ingestion/partial-ingestion/configure-batch.png)

「**[!UICONTROL 部分取得]**」切り替えを使用すると、部分バッチ取得の使用を有効または無効にできます。

The **[!UICONTROL Error diagnostics]** toggle only appears when the **[!UICONTROL Partial ingestion]** toggle is off. This feature allows [!DNL Platform] to generate detailed error messages about your ingested batches. If the **[!UICONTROL Partial ingestion]** toggle is turned on, enhanced error diagnostics are automatically enforced.

![](../images/batch-ingestion/partial-ingestion/configure-batch-partial-ingestion-focus.png)

**[!UICONTROL エラーしきい値]**&#x200B;を使用すると、バッチ全体が失敗する前に許容可能なエラーの割合を設定できます。デフォルトでは、この値は 5% に設定されています。

### 既存のデータセットを使用する {#existing-dataset}

既存のデータセットを使用するには、開始がデータセットを選択します。 右側のサイドバーに、データセットに関する情報が表示されます。

![](../images/batch-ingestion/partial-ingestion/monitor-dataset.png)

「**[!UICONTROL 部分取得]**」切り替えを使用すると、部分バッチ取得の使用を有効または無効にできます。

The **[!UICONTROL Error diagnostics]** toggle only appears when the **[!UICONTROL Partial ingestion]** toggle is off. This feature allows [!DNL Platform] to generate detailed error messages about your ingested batches. If the **[!UICONTROL Partial ingestion]** toggle is turned on, enhanced error diagnostics are automatically enforced.

![](../images/batch-ingestion/partial-ingestion/monitor-dataset-partial-ingestion-focus.png)

**[!UICONTROL エラーしきい値]**&#x200B;を使用すると、バッチ全体が失敗する前に許容可能なエラーの割合を設定できます。デフォルトでは、この値は 5% に設定されています。

これで、 **追加「** data」ボタンを使用してデータをアップロードでき、部分的な取り込みを使用して取り込むことができます。

### 「[!UICONTROL Map CSV to XDMスキーマ]」フローの使用 {#map-flow}

「[!UICONTROL CSVをXDMスキーマに]マップ [」のフローを使用するには、「CSVファイルを](../tutorials/map-a-csv-file.md)マップ」のチュートリアルに示されている手順に従います。 Once you reach the **[!UICONTROL Add data]** step, take note of the **[!UICONTROL Partial ingestion]** and **[!UICONTROL Error diagnostics]** fields.

![](../images/batch-ingestion/partial-ingestion/xdm-csv-workflow.png)

「**[!UICONTROL 部分取得]**」切り替えを使用すると、部分バッチ取得の使用を有効または無効にできます。

The **[!UICONTROL Error diagnostics]** toggle only appears when the **[!UICONTROL Partial ingestion]** toggle is off. This feature allows [!DNL Platform] to generate detailed error messages about your ingested batches. If the **[!UICONTROL Partial ingestion]** toggle is turned on, enhanced error diagnostics are automatically enforced.

![](../images/batch-ingestion/partial-ingestion/xdm-csv-workflow-partial-ingestion-focus.png)

**[!UICONTROL エラーしきい値]** ：バッチ全体が失敗する前に、許容できるエラーの割合を設定できます。 デフォルトでは、この値は 5% に設定されています。

## 次の手順 {#next-steps}

このチュートリアルでは、部分バッチ取得を有効にするデータセットの作成または変更方法について説明しました。バッチ取得について詳しくは、『[バッチ取得開発者ガイド](./api-overview.md)』を参照してください。

部分的なインジェストエラーの監視の詳細については、『 [バッチインジェストエラー診断ガイド](../quality/error-diagnostics.md)』を参照してください。
