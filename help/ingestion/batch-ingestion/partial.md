---
keywords: Experience Platform；ホーム；人気のトピック；バッチ取り込み；バッチ取り込み；バッチ取り込み；部分取り込み；部分取り込み；エラーの取得；エラーの取得；部分バッチ取り込み；部分バッチ取り込み；取り込み；取り込み；
solution: Experience Platform
title: 部分バッチ取り込みの概要
description: このドキュメントでは、部分バッチ取得を管理するためのチュートリアルを提供します。
exl-id: 25a34da6-5b7c-4747-8ebd-52ba516b9dc3
source-git-commit: bc72f77b1b4a48126be9b49c5c663ff11e9054ea
workflow-type: tm+mt
source-wordcount: '1209'
ht-degree: 30%

---

# 部分バッチ取得

部分バッチ取得は、エラーを含むデータを特定のしきい値まで取得する機能です。この機能を使用すると、正しいデータをすべて Adobe Experience Platform に正しく取得し、間違ったデータはその理由の詳細とともにすべて別々にバッチ処理されます。

このドキュメントでは、部分バッチ取得を管理するためのチュートリアルを提供します。

## はじめに

このチュートリアルでは、部分バッチ取得に関わる様々な Adobe Experience Platform サービスに関する十分な知識が必要です。このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [&#x200B; バッチ取り込み &#x200B;](./overview.md):CSV や Parquet などのデータファイルからデータを取 [!DNL Experience Platform] 込んで保存する方法。
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
>この節では、API を使用してバッチを有効にして部分バッチ取り込みを行う方法について説明します。 UI の使用方法については、[UI での部分バッチ取り込みのバッチを有効にする &#x200B;](#enable-ui) ステップを参照してください。

部分取り込みを有効にして新しいバッチを作成できます。

新しいバッチを作成するには、[&#x200B; バッチ取得開発者ガイド &#x200B;](./api-overview.md) の手順に従います。 **[!UICONTROL Create batch]** ステップに到達したら、リクエスト本文内で次のフィールドを追加します。

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

[!DNL Experience Platform] UI を使用してバッチで部分取り込みを有効にするには、ソース接続で新しいバッチを作成するか、既存のデータセットで新しいバッチを作成するか、「[!UICONTROL Map CSV to XDM flow]」を使用して新しいバッチを作成します。

### 新しいソース接続の作成 {#new-source}

新しいソース接続を作成するには、[&#x200B; ソースの概要 &#x200B;](../../sources/home.md) に一覧表示されている手順に従います。 **[!UICONTROL Dataflow detail]** の手順に到達したら、**[!UICONTROL Partial ingestion]** と **[!UICONTROL Error diagnostics]** のフィールドに注意してください。

![](../images/batch-ingestion/partial-ingestion/configure-batch.png)

**[!UICONTROL Partial ingestion]** 切替スイッチを使用すると、部分バッチ取り込みの使用を有効または無効にできます。

**[!UICONTROL Error diagnostics]** の切り替えは、**[!UICONTROL Partial ingestion]** の切り替えがオフの場合にのみ表示されます。 この機能を使用 [!DNL Experience Platform] ると、取り込んだバッチに関する詳細なエラーメッセージを生成できます。 **[!UICONTROL Partial ingestion]** の切り替えがオンになっている場合、拡張エラー診断が自動的に適用されます。

![](../images/batch-ingestion/partial-ingestion/configure-batch-partial-ingestion-focus.png)

**[!UICONTROL Error threshold]** を使用すると、バッチ全体が失敗する前に許容可能なエラーの割合を設定できます。 デフォルトでは、この値は 5% に設定されています。

### 既存のデータセットを使用する {#existing-dataset}

既存のデータセットを使用するには、まずデータセットを選択します。 右側のサイドバーに、データセットに関する情報が表示されます。

![](../images/batch-ingestion/partial-ingestion/monitor-dataset.png)

**[!UICONTROL Partial ingestion]** 切替スイッチを使用すると、部分バッチ取り込みの使用を有効または無効にできます。

**[!UICONTROL Error diagnostics]** の切り替えは、**[!UICONTROL Partial ingestion]** の切り替えがオフの場合にのみ表示されます。 この機能を使用 [!DNL Experience Platform] ると、取り込んだバッチに関する詳細なエラーメッセージを生成できます。 **[!UICONTROL Partial ingestion]** の切り替えがオンになっている場合、拡張エラー診断が自動的に適用されます。

![](../images/batch-ingestion/partial-ingestion/monitor-dataset-partial-ingestion-focus.png)

**[!UICONTROL Error threshold]** を使用すると、バッチ全体が失敗する前に許容可能なエラーの割合を設定できます。 デフォルトでは、この値は 5% に設定されています。

**データを追加** ボタンを使用してデータをアップロードでき、部分取り込みを使用して取り込めるようになりました。

### 「[!UICONTROL Map CSV to XDM schema]」フローの使用 {#map-flow}

「[!UICONTROL Map CSV to XDM schema]」フローを使用するには、[CSV ファイルのマッピング チュートリアル &#x200B;](../tutorials/map-csv/overview.md) に示されている手順に従います。 **[!UICONTROL Add data]** の手順に到達したら、**[!UICONTROL Partial ingestion]** と **[!UICONTROL Error diagnostics]** のフィールドに注意してください。

![](../images/batch-ingestion/partial-ingestion/xdm-csv-workflow.png)

**[!UICONTROL Partial ingestion]** 切替スイッチを使用すると、部分バッチ取り込みの使用を有効または無効にできます。

**[!UICONTROL Error diagnostics]** の切り替えは、**[!UICONTROL Partial ingestion]** の切り替えがオフの場合にのみ表示されます。 この機能を使用 [!DNL Experience Platform] ると、取り込んだバッチに関する詳細なエラーメッセージを生成できます。 **[!UICONTROL Partial ingestion]** の切り替えがオンになっている場合、拡張エラー診断が自動的に適用されます。

![](../images/batch-ingestion/partial-ingestion/xdm-csv-workflow-partial-ingestion-focus.png)

**[!UICONTROL Error threshold]** では、バッチ全体が失敗する前に、許容可能なエラーの割合を設定できます。 デフォルトでは、この値は 5% に設定されています。

## 既存のデータフローの部分取り込みとエラー診断を有効にする

部分的な取り込みまたはエラー診断を有効にせずにExperience Platformでデータフローを作成した場合でも、フローを再作成せずにこれらの機能を有効にできます。 部分取り込みと堅牢なエラー診断を有効にすることで、データ取り込みワークフローの信頼性とトラブルシューティングのしやすさを大幅に向上させることができます。 [!DNL Flow Service] API を使用して既存のデータフローの部分取得とエラー診断を有効にする方法については、以下の節を参照してください。

デフォルトでは、データフローで、部分取り込みまたはエラー診断が有効になっていない場合があります。 これらの機能は、データ取り込み中の問題を特定し分離するのに役立ちます。 [!DNL Flow Service] API を使用すると、現在のデータフロー設定を取得し、PATCH リクエストを使用して必要な変更を適用することができます。

既存のデータフローの部分取り込みとエラー診断を有効にするには、次の手順に従います。

### フローの詳細を取得

データフロー設定を取得するには、`/flows/{FLOW_ID}` エンドポイントに対してGET リクエストを実行し、データフローの ID を指定します。 データフローの詳細の取得について詳しくは、[API を使用したデータフローの更新  [!DNL Flow Service]  ガイドを参照し &#x200B;](../../sources/tutorials/api/update-dataflows.md) ください。

応答で返された `etag` フィールドの値を必ず保存してください。 これは、更新リクエストでバージョンの一貫性を確保するために必要です。

### フロー設定を更新

次に、`/flows/` エンドポイントに対してPATCH リクエストを実行し、部分取り込みとエラー診断を有効にするデータフローの ID を指定します。

>[!IMPORTANT]
>
>- If-Match キーを使用して、以前に保存した `etag` 値をリクエストヘッダーに含めます。
>- 特定のニーズに合わせて `partialIngestionPercent` 値を変更できます。

**API 形式**

```http
PATCH /flows/{FLOW_ID}
```

**リクエスト**

```shell
curl -X PATCH \
    'https://platform.adobe.io/data/foundation/flowservice/flows/2edc08ac-4df5-4fe6-936f-81a19ce92f5c' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
    -H 'If-Match: "1a0037e4-0000-0200-0000-602e06f60000"' \
    -d '[
        {
            "op": "add",
            "path": "/options",
            "value": {
                "partialIngestionPercent": "10"
            }
        },
        {
            "op": "add",
            "path": "/options/errorDiagnosticsEnabled",
            "value": true
        }
    ]'
```

**応答**

応答が成功すると、データフローの `id` と更新された `etag` が返されます。

```json
{
    "id": "2edc08ac-4df5-4fe6-936f-81a19ce92f5c",
    "etag": "\"2c000802-0000-0200-0000-613976440000\""
}
```

### 更新の検証

PATCHが完了したら、GET リクエストを行ってデータフローを取得し、変更内容が正常に完了したことを確認します。

**API 形式**

```http
GET /flows/{FLOW_ID}
```

**リクエスト**

次のリクエストでは、フロー ID に関する更新済み情報を取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/flows/2edc08ac-4df5-4fe6-936f-81a19ce92f5c' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、データフローの詳細が返され、`options` セクションで部分取り込みとエラー診断が有効になったことが確認されます。

```json
"options": {
    "partialIngestionPercent": 10,
    "errorDiagnosticsEnabled": true
}
```

## 次の手順 {#next-steps}

このチュートリアルでは、部分バッチ取得を有効にするデータセットの作成または変更方法について説明しました。バッチ取得について詳しくは、『[バッチ取得開発者ガイド](./api-overview.md)』を参照してください。

部分取り込みエラーの監視については、[&#x200B; バッチ取り込みエラー診断ガイド &#x200B;](../quality/error-diagnostics.md) を参照してください。
