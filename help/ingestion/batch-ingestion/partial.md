---
keywords: Experience Platform；ホーム；人気のトピック；バッチ取り込み；バッチ取り込み；バッチ取り込み；部分取り込み；部分取り込み；エラーの取得；エラーの取得；部分バッチ取り込み；部分バッチ取り込み；取り込み；取り込み；
solution: Experience Platform
title: 部分バッチ取り込みの概要
description: このドキュメントでは、部分バッチ取得を管理するためのチュートリアルを提供します。
exl-id: 25a34da6-5b7c-4747-8ebd-52ba516b9dc3
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '946'
ht-degree: 45%

---

# 部分バッチ取得

部分バッチ取得は、エラーを含むデータを特定のしきい値まで取得する機能です。この機能を使用すると、正しいデータをすべて Adobe Experience Platform に正しく取得し、間違ったデータはその理由の詳細とともにすべて別々にバッチ処理されます。

このドキュメントでは、部分バッチ取得を管理するためのチュートリアルを提供します。

## はじめに

このチュートリアルでは、部分バッチ取得に関わる様々な Adobe Experience Platform サービスに関する十分な知識が必要です。このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [ バッチ取り込み ](./overview.md):CSV や Parquet などのデータファイルからデータを取 [!DNL Experience Platform] 込んで保存する方法。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：[!DNL Experience Platform] が、カスタマーエクスペリエンスデータを整理する際に使用する、標準化されたフレームワーク。

以下の節では、[!DNL Experience Platform] API を正しく呼び出すために知っておく必要がある追加情報を示します。

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Experience Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- Authorization： Bearer `{ACCESS_TOKEN}`
- x-api-key： `{API_KEY}`
- x-gw-ims-org-id： `{ORG_ID}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Experience Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name：`{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Experience Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

## API での部分バッチ取り込みのバッチの有効化 {#enable-api}

>[!NOTE]
>
>この節では、API を使用してバッチを有効にして部分バッチ取り込みを行う方法について説明します。 UI の使用方法については、[UI での部分バッチ取り込みのバッチを有効にする ](#enable-ui) ステップを参照してください。

部分取り込みを有効にして新しいバッチを作成できます。

新しいバッチを作成するには、[ バッチ取得開発者ガイド ](./api-overview.md) の手順に従います。 **[!UICONTROL バッチを作成]** ステップに到達したら、リクエスト本文に次のフィールドを追加します。

```json
{
    "enableErrorDiagnostics": true,
    "partialIngestionPercent": 5
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `enableErrorDiagnostics` | バッチに関 [!DNL Experience Platform] る詳細なエラーメッセージを生成できるフラグ。 |
| `partialIngestionPercent` | バッチ全体が失敗する前に許容できるエラーの割合。 この例では、失敗する前にバッチの最大 5% がエラーになる可能性があります。 |


## UI での部分バッチ取り込みのバッチを有効にする {#enable-ui}

>[!NOTE]
>
>この節では、UI を使用してバッチを有効にして部分バッチ取り込みを行う方法について説明します。 API を使用してバッチ取り込みのバッチを既に有効にしている場合は、次の節に進みます。

[!DNL Experience Platform] UI を使用してバッチで部分取り込みを有効にするには、ソース接続を使用して新しいバッチを作成、既存のデータセットで新しいバッチを作成または「[!UICONTROL CSV を XDM フローにマッピング ]」を使用して新しいバッチを作成します。

### 新しいソース接続の作成 {#new-source}

新しいソース接続を作成するには、[ ソースの概要 ](../../sources/home.md) に一覧表示されている手順に従います。 **[!UICONTROL データフローの詳細]** 手順に到達したら、**[!UICONTROL 部分取り込み]** および **[!UICONTROL エラー診断]** フィールドに注意します。

![](../images/batch-ingestion/partial-ingestion/configure-batch.png)

「**[!UICONTROL 部分取得]**」切り替えを使用すると、部分バッチ取得の使用を有効または無効にできます。

**[!UICONTROL エラー診断]** 切替スイッチは、**[!UICONTROL 部分取り込み]** 切替スイッチがオフの場合にのみ表示されます。 この機能を使用 [!DNL Experience Platform] ると、取り込んだバッチに関する詳細なエラーメッセージを生成できます。 **[!UICONTROL 部分取り込み]** 切り替えスイッチがオンになっている場合、拡張エラー診断が自動的に適用されます。

![](../images/batch-ingestion/partial-ingestion/configure-batch-partial-ingestion-focus.png)

**[!UICONTROL エラーしきい値]**&#x200B;を使用すると、バッチ全体が失敗する前に許容可能なエラーの割合を設定できます。デフォルトでは、この値は 5% に設定されています。

### 既存のデータセットを使用する {#existing-dataset}

既存のデータセットを使用するには、まずデータセットを選択します。 右側のサイドバーに、データセットに関する情報が表示されます。

![](../images/batch-ingestion/partial-ingestion/monitor-dataset.png)

「**[!UICONTROL 部分取得]**」切り替えを使用すると、部分バッチ取得の使用を有効または無効にできます。

**[!UICONTROL エラー診断]** 切替スイッチは、**[!UICONTROL 部分取り込み]** 切替スイッチがオフの場合にのみ表示されます。 この機能を使用 [!DNL Experience Platform] ると、取り込んだバッチに関する詳細なエラーメッセージを生成できます。 **[!UICONTROL 部分取り込み]** 切り替えスイッチがオンになっている場合、拡張エラー診断が自動的に適用されます。

![](../images/batch-ingestion/partial-ingestion/monitor-dataset-partial-ingestion-focus.png)

**[!UICONTROL エラーしきい値]**&#x200B;を使用すると、バッチ全体が失敗する前に許容可能なエラーの割合を設定できます。デフォルトでは、この値は 5% に設定されています。

**データを追加** ボタンを使用してデータをアップロードでき、部分取り込みを使用して取り込めるようになりました。

### 「[!UICONTROL CSV を XDM スキーマにマッピング ]」フローを使用します {#map-flow}

「[!UICONTROL CSV を XDM スキーマにマッピング ]」フローを使用するには、[CSV ファイルのマッピング ](../tutorials/map-csv/overview.md) チュートリアルに記載されている手順に従います。 **[!UICONTROL データを追加]** 手順に到達したら、**[!UICONTROL 部分取り込み]** および **[!UICONTROL エラー診断]** フィールドに注意します。

![](../images/batch-ingestion/partial-ingestion/xdm-csv-workflow.png)

「**[!UICONTROL 部分取得]**」切り替えを使用すると、部分バッチ取得の使用を有効または無効にできます。

**[!UICONTROL エラー診断]** 切替スイッチは、**[!UICONTROL 部分取り込み]** 切替スイッチがオフの場合にのみ表示されます。 この機能を使用 [!DNL Experience Platform] ると、取り込んだバッチに関する詳細なエラーメッセージを生成できます。 **[!UICONTROL 部分取り込み]** 切り替えスイッチがオンになっている場合、拡張エラー診断が自動的に適用されます。

![](../images/batch-ingestion/partial-ingestion/xdm-csv-workflow-partial-ingestion-focus.png)

**[!UICONTROL エラーしきい値]** バッチ全体が失敗する前に、許容できるエラーの割合を設定できます。 デフォルトでは、この値は 5% に設定されています。

## 次の手順 {#next-steps}

このチュートリアルでは、部分バッチ取得を有効にするデータセットの作成または変更方法について説明しました。バッチ取得について詳しくは、『[バッチ取得開発者ガイド](./api-overview.md)』を参照してください。

部分取り込みエラーの監視については、[ バッチ取り込みエラー診断ガイド ](../quality/error-diagnostics.md) を参照してください。
