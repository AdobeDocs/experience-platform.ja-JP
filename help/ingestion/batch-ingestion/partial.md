---
keywords: Experience Platform；ホーム；人気の高いトピック；バッチ取得；バッチ取得；バッチ取得；部分取得；部分取得；エラーの取得；部分バッチ取得；部分バッチ取得；部分；取得；取得；
solution: Experience Platform
title: 部分バッチ取り込みの概要
topic-legacy: overview
description: このドキュメントでは、部分バッチ取得を管理するためのチュートリアルを提供します。
exl-id: 25a34da6-5b7c-4747-8ebd-52ba516b9dc3
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '945'
ht-degree: 44%

---

# 部分バッチ取得

部分バッチ取得は、エラーを含むデータを特定のしきい値まで取得する機能です。この機能を使用すると、正しいデータをすべて Adobe Experience Platform に正しく取得し、間違ったデータはその理由の詳細とともにすべて別々にバッチ処理されます。

このドキュメントでは、部分バッチ取得を管理するためのチュートリアルを提供します。

## はじめに

このチュートリアルでは、部分バッチ取得に関わる様々な Adobe Experience Platform サービスに関する十分な知識が必要です。このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [バッチ取得](./overview.md):メソッド [!DNL Platform] CSV や Parquet などのデータファイルからデータを取得して保存します。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：[!DNL Platform] がカスタマーエクスペリエンスのデータの整理に使用する、標準化されたフレームワーク。

以下の節では、への呼び出しを正しくおこなうために知っておく必要がある追加情報を示します。 [!DNL Platform] API

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- Authorization： Bearer `{ACCESS_TOKEN}`
- x-api-key： `{API_KEY}`
- x-gw-ims-org-id： `{ORG_ID}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name：`{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

## API での部分バッチ取り込み用のバッチの有効化 {#enable-api}

>[!NOTE]
>
>この節では、API を使用した部分バッチ取得のバッチの有効化について説明します。 UI の使用手順については、 [UI での部分バッチ取り込み用のバッチの有効化](#enable-ui) 手順

部分取得を有効にした新しいバッチを作成できます。

新しいバッチを作成するには、 [バッチ取得開発者ガイド](./api-overview.md). 次に **[!UICONTROL バッチを作成]** ステップで、リクエスト本文内に次のフィールドを追加します。

```json
{
    "enableErrorDiagnostics": true,
    "partialIngestionPercent": 5
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `enableErrorDiagnostics` | 許可するフラグ [!DNL Platform] ：バッチに関する詳細なエラーメッセージを生成します。 |
| `partialIngestionPercent` | バッチ全体が失敗する前の許容エラー率。したがって、この例では、失敗する前に、バッチの最大 5%がエラーになる場合があります。 |


## UI での部分バッチ取り込み用のバッチの有効化 {#enable-ui}

>[!NOTE]
>
>この節では、UI を使用した部分バッチ取り込みのバッチの有効化について説明します。 API を使用して部分バッチ取得のバッチを既に有効にしている場合は、次の節に進んでください。

バッチで [!DNL Platform] UI では、ソース接続を使用して新しいバッチを作成したり、既存のデータセットで新しいバッチを作成したり、「[!UICONTROL CSV を XDM フローにマッピング]&quot;.

### 新しいソース接続を作成 {#new-source}

新しいソース接続を作成するには、 [ソースの概要](../../sources/home.md). 次に **[!UICONTROL データフローの詳細]** ステップ、メモ **[!UICONTROL 部分取り込み]** および **[!UICONTROL エラー診断]** フィールド。

![](../images/batch-ingestion/partial-ingestion/configure-batch.png)

「**[!UICONTROL 部分取得]**」切り替えを使用すると、部分バッチ取得の使用を有効または無効にできます。

この **[!UICONTROL エラー診断]** トグルは、 **[!UICONTROL 部分取り込み]** 切り替えはオフです。 この機能を使用すると、 [!DNL Platform] 取り込んだバッチに関する詳細なエラーメッセージを生成する場合。 この **[!UICONTROL 部分取り込み]** 切り替えをオンにすると、拡張エラー診断が自動的に適用されます。

![](../images/batch-ingestion/partial-ingestion/configure-batch-partial-ingestion-focus.png)

**[!UICONTROL エラーしきい値]**&#x200B;を使用すると、バッチ全体が失敗する前に許容可能なエラーの割合を設定できます。デフォルトでは、この値は 5% に設定されています。

### 既存のデータセットを使用する {#existing-dataset}

既存のデータセットを使用するには、まずデータセットを選択します。 右側のサイドバーに、データセットに関する情報が表示されます。

![](../images/batch-ingestion/partial-ingestion/monitor-dataset.png)

「**[!UICONTROL 部分取得]**」切り替えを使用すると、部分バッチ取得の使用を有効または無効にできます。

この **[!UICONTROL エラー診断]** トグルは、 **[!UICONTROL 部分取り込み]** 切り替えはオフです。 この機能を使用すると、 [!DNL Platform] 取り込んだバッチに関する詳細なエラーメッセージを生成する場合。 この **[!UICONTROL 部分取り込み]** 切り替えをオンにすると、拡張エラー診断が自動的に適用されます。

![](../images/batch-ingestion/partial-ingestion/monitor-dataset-partial-ingestion-focus.png)

**[!UICONTROL エラーしきい値]**&#x200B;を使用すると、バッチ全体が失敗する前に許容可能なエラーの割合を設定できます。デフォルトでは、この値は 5% に設定されています。

これで、 **データを追加** ボタンに貼り付け、部分取り込みを使用して取り込まれます。

### 「[!UICONTROL CSV を XDM スキーマにマッピング]`フロー {#map-flow}

「[!UICONTROL CSV を XDM スキーマにマッピング]&quot;フロー、 [CSV ファイルのマッピングのチュートリアル](../tutorials/map-a-csv-file.md). 次に **[!UICONTROL データを追加]** ステップ、メモ **[!UICONTROL 部分取り込み]** および **[!UICONTROL エラー診断]** フィールド。

![](../images/batch-ingestion/partial-ingestion/xdm-csv-workflow.png)

「**[!UICONTROL 部分取得]**」切り替えを使用すると、部分バッチ取得の使用を有効または無効にできます。

この **[!UICONTROL エラー診断]** トグルは、 **[!UICONTROL 部分取り込み]** 切り替えはオフです。 この機能を使用すると、 [!DNL Platform] 取り込んだバッチに関する詳細なエラーメッセージを生成する場合。 この **[!UICONTROL 部分取り込み]** 切り替えをオンにすると、拡張エラー診断が自動的に適用されます。

![](../images/batch-ingestion/partial-ingestion/xdm-csv-workflow-partial-ingestion-focus.png)

**[!UICONTROL エラーしきい値]** では、バッチ全体が失敗する前に許容可能なエラーの割合を設定できます。 デフォルトでは、この値は 5% に設定されています。

## 次の手順 {#next-steps}

このチュートリアルでは、部分バッチ取得を有効にするデータセットの作成または変更方法について説明しました。バッチ取得について詳しくは、『[バッチ取得開発者ガイド](./api-overview.md)』を参照してください。

部分取り込みエラーの監視について詳しくは、 [バッチ取得エラー診断ガイド](../quality/error-diagnostics.md).
