---
title: 外部オーディエンスの作成とアクティブ化
type: Tutorial
description: Experience Platform API を使用してAdobe Experience Platformで外部オーディエンスを作成する方法を説明します。
source-git-commit: 0a37ef2f5fc08eb515c7c5056936fd904ea6d360
workflow-type: tm+mt
source-wordcount: '892'
ht-degree: 7%

---


# API を使用した外部オーディエンスの作成とアクティブ化

このチュートリアルでは、Adobe Experience Platform API を使用して外部オーディエンスを作成するために必要な手順について説明します。

## はじめに

このチュートリアルでは、外部オーディエンスの作成に関連する様々なExperience Platform サービスについて、実際に理解している必要があります。 このチュートリアルを開始する前に、次のサービスのドキュメントを参照してください。

- [&#x200B; ソース &#x200B;](../../sources/home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
- [[!DNL Adobe Experience Platform Segmentation Service]](../home.md)：外部データからオーディエンスを作成できます。
- [&#x200B; 宛先 &#x200B;](../../destinations/home.md)：宛先は、一般に使用されるアプリケーションとの事前定義済みの統合で、これを使用すると、Experience Platformのデータをシームレスにアクティブ化してクロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告などを実現できます。

### 必須ヘッダー

また、このチュートリアルでは、API を正しく呼び出すために、[&#x200B; 認証に関するチュートリアル &#x200B;](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja) を完了している必要 [!DNL Experience Platform] あります。 次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- Authorization： Bearer `{ACCESS_TOKEN}`
- x-api-key： `{API_KEY}`
- x-gw-ims-org-id： `{ORG_ID}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Experience Platform] API へのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name：`{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Experience Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

すべての POST、PUT、および PATCH リクエストには、次の追加ヘッダーが必要です。

- Content-Type: application/json

## 外部オーディエンスの準備 {#prepare}

Experience Platform内に外部オーディエンスを作成する前に、オーディエンスデータを含んだファイルを準備する必要があります。

この例では、CSV ファイルを使用する必要があります。 ECID、メール ID、CRM ID などの ID 値を持つ列が CSV ファイルに **少なくとも 1 つ** 含まれていることを確認してください。 さらに、セグメント化とアクティベーションに必要なすべてのエンリッチメント属性が含まれていることを確認します。

また、ファイルがExperience Platform スキーマの要件に準拠していることを確認する必要があります。 スキーマの作成について詳しくは、[API を使用したスキーマの作成に関するチュートリアル &#x200B;](/help/xdm/tutorials/create-schema-api.md) または [UI を使用したスキーマの作成に関するチュートリアル &#x200B;](/help/xdm/tutorials/create-schema-ui.md) を参照してください。

必要なすべての情報が CSV ファイルに含まれ、スキーマに準拠していることを確認したら、CSV ファイルをクラウドストレージプロバイダーにアップロードして、ソースを使用してデータをExperience Platformに取り込めるようにする必要があります。 クラウドストレージソースの使用について詳しくは、[API を使用したクラウドストレージオプションの調査に関するチュートリアル &#x200B;](/help/sources/tutorials/api/explore/cloud-storage.md) または [&#x200B; ソースの概要 &#x200B;](/help/sources/home.md#cloud-storage) を参照してください。

## 外部オーディエンスの作成 {#create}

CSV ファイルを準備したら、外部オーディエンスの作成プロセスを開始できます。

`/external-audience/` エンドポイントに POST リクエストを実行することで、外部オーディエンスを作成できます。

このリクエストをおこなう際は、次の情報を指定する必要があります。

- オーディエンスの名前
- オーディエンスの説明
- CSV とスキーマの間の対応するフィールド
- ソースの仕様情報
   - これには、取得用の CSV ファイルのファイルパスが含まれます
      - ファイルパス **にスペースを含めることはできません**。 例えば、パスが `activation/sample-source/Example CSV File.csv` の場合、パスを `activation/sample-source/ExampleCSVFile.csv` に設定します。

このエンドポイントの使用方法について詳しくは、[&#x200B; 外部オーディエンスエンドポイントガイド &#x200B;](/help/segmentation/api/external-audiences.md#create-audience) を参照してください。

+++リクエスト

```shell
curl -X POST https://platform.adobe.io/data/core/ais/external-audience/ \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '{
        "name": "Sample external audience",
        "description": "A sample version of an external audience",
        "fields": [
            {
                "name": "ppid",
                "type": "string",
                "identityNs": "email"
            },
            {
                "name": "list_id",
                "type": "string",
                "labels": ["core/C2", "custom/deep"]
            },
            {
                "name": "delete",
                "type": "number"
            },
            {
                "name": "process_consent",
                "type": "string"
            }
        ],
        "sourceSpec": {
            "params": {
                "path": "activation/sample-source/example.csv",
                "type": "file",
                "sourceType": "Cloud Storage",
                "baseConnectionId": "1d1d4bc5-b527-46a3-9863-530246a61b2b"
            }
        },
        "ttlInDays": "40",
        "labels": ["core/C1"],
        "audienceType": "people",
        "originName": "CUSTOM_UPLOAD"
    }'
```

+++

このリクエストを行った後は、オーディエンス ID を取得できるよう、応答から受け取った `operationId` をメモしておいてください。

## オーディエンス ID を取得 {#retrieve-audience-id}

外部オーディエンスを作成したので、オーディエンス ID を取得して、オーディエンスをExperience Platformに取り込むことができます。

オーディエンス ID を取得するには、`/external-audiences/operations` エンドポイントに対してGET リクエストを実行し、以前に外部オーディエンス作成応答から受け取った操作の ID を指定します。

このエンドポイントの使用方法について詳しくは、[&#x200B; 外部オーディエンスエンドポイントガイド &#x200B;](/help/segmentation/api/external-audiences.md#retrieve-status) を参照してください。

+++ リクエスト

```shell
curl -X GET https://platform.adobe.io/data/core/ais/external-audience/operations/{OPERATION_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

このリクエストを行ったら、オーディエンスの取り込みジョブをトリガーにできるように、応答から受け取った `audienceId` をメモしておいてください。

## オーディエンスの取り込みを開始 {#start-ingestion}

`audienceId` が用意されたので、外部オーディエンスのExperience Platformへの取り込みをトリガーに設定できるようになりました。

オーディエンス ID を指定しながら次のエンドポイントに対して POST リクエストを実行することで、オーディエンスの取り込みを開始できます。 さらに、どのファイルが処理されるかを決定するための開始時間を指定する必要があります。

このエンドポイントの使用方法について詳しくは、[&#x200B; 外部オーディエンスエンドポイントガイド &#x200B;](/help/segmentation/api/external-audiences.md#start-audience-ingestion) を参照してください。

+++ リクエスト

```shell
curl -X POST https://platform.adobe.io/data/core/ais/external-audience/{AUDIENCE_ID}/runs \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '{
    "dataFilterStartTime": 764245635
 }' 
```

+++

このリクエストを行った後は、取り込みステータスを監視できるように、応答から受け取った `runId` を必ずメモしてください。

## 取り込みステータスの監視 {#monitor-ingestion}

オーディエンスの取り込みをトリガーした後は、取り込みの進行状況を監視して、取り込みの成功を確認し、ダウンストリームのアクティベーションに向けたオーディエンスの可用性を検証できます。

オーディエンス ID と実行 ID の両方を提供しながら、次のエンドポイントに対してGET リクエストを実行することで、オーディエンス取り込みのステータスを取得できます。

このエンドポイントの使用方法について詳しくは、[&#x200B; 外部オーディエンスエンドポイントガイド &#x200B;](/help/segmentation/api/external-audiences.md#retrieve-ingestion-status) を参照してください。

+++ リクエスト

```shell
curl -X GET https://platform.adobe.io/data/core/ais/external-audience/{AUDIENCE_ID}/runs/{RUN_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

## 次の手順 {#next-steps}

>[!IMPORTANT]
>
>外部で生成されたオーディエンスを使用するには、毎日のセグメント化ジョブが完了するまで待つ **必要があります**。

外部オーディエンスが正常に取り込まれたことを確認したら、Audience Portal で表示し、宛先などのダウンストリームサービスで使用できます。

オーディエンスポータルについて詳しくは、[&#x200B; オーディエンスポータル UI ガイド &#x200B;](/help/segmentation/ui/audience-portal.md) を参照してください。 宛先について詳しくは、[&#x200B; 宛先の概要 &#x200B;](/help/destinations/home.md) を参照してください。

