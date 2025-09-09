---
solution: Experience Platform
title: Flow Service API を使用して、ファイルベースの宛先に対するオーディエンスをアクティブ化します
description: Flow Service API を使用して、認定プロファイルを含むファイルをクラウドストレージ宛先に書き出す方法を説明します。
type: Tutorial
exl-id: 62028c7a-3ea9-4004-adb7-5e27bbe904fc
source-git-commit: eb7d1b9c167839db39cbb28bf497edac706c0b6c
workflow-type: tm+mt
source-wordcount: '4911'
ht-degree: 9%

---

# Flow Service API を使用して、ファイルベースの宛先に対するオーディエンスをアクティブ化します

拡張されたファイル書き出し機能を使用すると、Experience Platformからファイルを書き出す際に拡張されたカスタマイズ機能にアクセスできます。

* 追加の[ファイル命名オプション](/help/destinations/ui/activate-batch-profile-destinations.md#file-names)。
* 書き出されたファイルにカスタムファイルヘッダーを設定する機能（[マッピングステップの改善](/help/destinations/ui/activate-batch-profile-destinations.md#mapping)による）
* 書き出されたファイルの [ ファイルタイプ ](/help/destinations/ui/connect-destination.md#file-formatting-and-compression-options) を選択する機能。
* [書き出された CSV データファイルの形式をカスタマイズする機能](/help/destinations/ui/batch-destinations-file-formatting-options.md)。

この機能は、以下に示す 6 つのクラウドストレージカードでサポートされています。

* [[!DNL Azure Data Lake Storage Gen2]](../../destinations/catalog/cloud-storage/adls-gen2.md)
* [[!DNL Data Landing Zone]](../../destinations/catalog/cloud-storage/data-landing-zone.md)
* [[!DNL Google Cloud Storage]](../../destinations/catalog/cloud-storage/google-cloud-storage.md)
* [[!DNL Amazon S3]](../../destinations/catalog/cloud-storage/amazon-s3.md#changelog)
* [[!DNL Azure Blob]](../../destinations/catalog/cloud-storage/azure-blob.md#changelog)
* [[!DNL SFTP]](../../destinations/catalog/cloud-storage/sftp.md#changelog)

この記事では、[Flow Service API](https://developer.adobe.com/experience-platform-apis/references/destinations/) を使用して、認定プロファイルをAdobe Experience Platformから上記にリンクされたクラウドストレージの場所の 1 つに書き出すために必要なワークフローについて説明します。

>[!TIP]
>
>また、Experience Platform ユーザーインターフェイスを使用して、プロファイルをクラウドストレージの宛先に書き出すこともできます。 詳しくは、[ ファイルベース宛先のアクティブ化のチュートリアル ](/help/destinations/ui/activate-batch-profile-destinations.md) を参照してください。

<!--

## API users migration {#api-migration}

If you were already using the Flow Service API to export profiles to the Amazon S3, Azure Blob, or SFTP cloud storage destinations, read the [API migration guide](/help/destinations/api/api-migration-guide-cloud-storage-destinations.md) for necessary migration steps as Adobe transitions users from the legacy destinations to the new destinations. 

-->

## はじめに {#get-started}

![ ユーザーがオンになっている現在の手順をハイライト表示するオーディエンスをアクティブ化する手順 ](/help/destinations/assets/api/file-based-segment-export/segment-export-overview.png)

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md)：[!DNL Experience Platform] がカスタマーエクスペリエンスのデータの整理に使用する、標準化されたフレームワーク。
* [[!DNL Segmentation Service]](../../segmentation/api/overview.md): [!DNL Adobe Experience Platform Segmentation Service] を使用すると、オーディエンスを作成し、[!DNL Adobe Experience Platform] データから [!DNL Real-Time Customer Profile] でオーディエンスを生成できます。
* [[!DNL Sandboxes]](../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Experience Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、Experience Platformでファイルベースの宛先に対してデータをアクティブ化するために必要な追加情報を示します。

### 必要な権限 {#permissions}

プロファイルを書き出すには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

*ID* を書き出すには、**[!UICONTROL ID グラフの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

### API 呼び出し例の読み取り {#reading-sample-api-calls}

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーおよびオプションヘッダーの値の収集 {#gather-values-headers}

[!DNL Experience Platform] API を呼び出すには、まず [Experience Platform認証チュートリアル ](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja) を完了する必要があります。 次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* Authorization： Bearer `{ACCESS_TOKEN}`
* x-api-key： `{API_KEY}`
* x-gw-ims-org-id： `{ORG_ID}`

[!DNL Experience Platform] のリソースは、特定の仮想サンドボックスに分離できます。[!DNL Experience Platform] API へのリクエストでは、操作を実行するサンドボックスの名前と ID を指定できます。次に、オプションのパラメーターを示します。

* x-sandbox-name：`{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Experience Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（`POST`、`PUT`、`PATCH`）を含むすべてのリクエストには、次のような追加のメディアタイプヘッダーが必要です。

* Content-Type: `application/json`

### API リファレンスドキュメント {#api-reference-documentation}

このチュートリアルに含まれるすべての API 操作について、付属リファレンスドキュメントが用意されています。詳しくは、Adobe Developer web サイト [Flow Service - Destinations API ドキュメント ](https://developer.adobe.com/experience-platform-apis/references/destinations/) を参照してください。 このチュートリアルと API リファレンスのドキュメントを並行して使用することをお勧めします。

### 用語集 {#glossary}

この API チュートリアルで発生する用語については、API リファレンスドキュメントの [ 用語集の節 ](https://developer.adobe.com/experience-platform-apis/references/destinations/#tag/Glossary) を参照してください。

## オーディエンスを書き出す宛先を選択 {#select-destination}

![ ユーザーがオンになっている現在の手順をハイライト表示するオーディエンスをアクティブ化する手順 ](/help/destinations/assets/api/file-based-segment-export/step1.png)

プロファイルの書き出しワークフローを開始する前に、オーディエンスの書き出し先とする宛先の接続仕様およびフロー仕様 ID を特定します。 以下の表を参照してください。

| 宛先 | 接続仕様 | フロー仕様 |
---------|----------|---------|
| Amazon S3 | `4fce964d-3f37-408f-9778-e597338a21ee` | `1a0514a6-33d4-4c7f-aff8-594799c47549` |
| Azure Blob ストレージ | `6d6b59bf-fb58-4107-9064-4d246c0e5bb2` | `752d422f-b16f-4f0d-b1c6-26e448e3b388` |
| Azure Data Lake Gen 2 （ADLS Gen2） | `be2c3209-53bc-47e7-ab25-145db8b873e1` | `17be2013-2549-41ce-96e7-a70363bec293` |
| データランディングゾーン（DLZ） | `10440537-2a7b-4583-ac39-ed38d4b848e8` | `cd2fc47e-e838-4f38-a581-8fff2f99b63a` |
| Google Cloud Storage | `c5d93acb-ea8b-4b14-8f53-02138444ae99` | `585c15c4-6cbf-4126-8f87-e26bff78b657` |
| SFTP | `36965a81-b1c6-401b-99f8-22508f1e6a26` | `fd36aaa4-bf2b-43fb-9387-43785eeeb799` |

{style="table-layout:auto"}

これらの ID は、このチュートリアルの次の手順で様々なフローサービスエンティティを構築するために必要です。 また、接続仕様自体の一部を参照して、特定のエンティティを設定し、Flow Service API から接続仕様を取得できるようにする必要もあります。 テーブル内のすべての宛先の接続仕様を取得する例を以下に示します。

>[!BEGINTABS]

>[!TAB Amazon S3]

**リクエスト**

+++[!DNL connection spec] の [!DNL Amazon S3] の取得

```shell
curl --location --request GET 'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs/4fce964d-3f37-408f-9778-e597338a21ee' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}'
```

+++

**応答**

+++[!DNL Amazon S3] – 接続仕様

```json
{
    "items": [
        {
            "id": "4fce964d-3f37-408f-9778-e597338a21ee",
            "name": "Amazon S3",
            "providerId": "14e34fac-d307-11e9-bb65-2a2ae2dbcce4",
            "version": "1.0",
//...
```

+++

>[!TAB Azure Blob Storage]

**リクエスト**

+++[!DNL connection spec] の [!DNL Azure Blob Storage] の取得

```shell
curl --location --request GET 'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs/6d6b59bf-fb58-4107-9064-4d246c0e5bb2' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}'
```

+++

**応答**

+++[!DNL Azure Blob Storage] - [!DNL Connection spec]

```json
{
    "items": [
        {
            "id": "6d6b59bf-fb58-4107-9064-4d246c0e5bb2",
            "name": "Azure Blob Storage",
            "providerId": "14e34fac-d307-11e9-bb65-2a2ae2dbcce4",
            "version": "1.0",
//...
```

+++

>[!TAB Azure Data Lake Gen 2 （ADLS Gen2） ]

**リクエスト**

+++[!DNL connection spec] の [!DNL Azure Data Lake Gen 2(ADLS Gen2] の取得）

```shell
curl --location --request GET 'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs/be2c3209-53bc-47e7-ab25-145db8b873e1' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}'
```

+++

**応答**

+++[!DNL Azure Data Lake Gen 2(ADLS Gen2)] - [!DNL Connection spec]

```json
{
    "items": [
        {
            "id": "be2c3209-53bc-47e7-ab25-145db8b873e1",
            "name": "Azure Data Lake Gen2",
            "providerId": "14e34fac-d307-11e9-bb65-2a2ae2dbcce4",
            "version": "1.0",
//...
```

+++

>[!TAB  データランディングゾーン（DLZ） ]

**リクエスト**

+++[!DNL connection spec] の [!DNL Data Landing Zone(DLZ)] の取得

```shell
curl --location --request GET 'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs/10440537-2a7b-4583-ac39-ed38d4b848e8' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}'
```

+++

**応答**

+++[!DNL Data Landing Zone(DLZ)] - [!DNL Connection spec]

```json
{
    "items": [
        {
            "id": "10440537-2a7b-4583-ac39-ed38d4b848e8",
            "name": "Data Landing Zone",
            "providerId": "14e34fac-d307-11e9-bb65-2a2ae2dbcce4",
            "version": "1.0",
//...
```

+++

>[!TAB Google Cloud Storage]

**リクエスト**

+++[!DNL connection spec] の [!DNL Google Cloud Storage] の取得

```shell
curl --location --request GET 'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs/c5d93acb-ea8b-4b14-8f53-02138444ae99' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}'
```

+++

**応答**

+++[!DNL Google Cloud Storage] - [!DNL Connection spec]

```json
{
    "items": [
        {
            "id": "c5d93acb-ea8b-4b14-8f53-02138444ae99",
            "name": "Google Cloud Storage",
            "providerId": "14e34fac-d307-11e9-bb65-2a2ae2dbcce4",
            "version": "1.0",
//...
```

+++

>[!TAB SFTP]

**リクエスト**

+++SFTP の [!DNL connection spec] の取得

```shell
curl --location --request GET 'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs/36965a81-b1c6-401b-99f8-22508f1e6a26' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}'
```

+++

**応答**

+++SFTP - [!DNL Connection spec]

```json
{
    "items": [
        {
            "id": "36965a81-b1c6-401b-99f8-22508f1e6a26",
            "name": "SFTP",
            "providerId": "14e34fac-d307-11e9-bb65-2a2ae2dbcce4",
            "version": "1.0",
//...
```

+++

>[!ENDTABS]

クラウドストレージの宛先へのオーディエンス書き出しデータフローを設定するには、次の手順に従います。 一部の手順では、リクエストと応答はクラウドストレージの様々な宛先間で異なります。 その場合、ページ上のタブを使用して、オーディエンスの接続および書き出し先となる宛先に固有のリクエストと応答を取得します。 設定している宛先に適した `connection spec` と `flow spec` を使用してください。

## Source接続の作成 {#create-source-connection}

![ ユーザーがオンになっている現在の手順をハイライト表示するオーディエンスをアクティブ化する手順 ](/help/destinations/assets/api/file-based-segment-export/step2.png)

オーディエンスを書き出す宛先を決定したら、ソース接続を作成する必要があります。 [ ソース接続 ](https://developer.adobe.com/experience-platform-apis/references/destinations/#tag/Glossary) は、内部 [Experience Platform プロファイルストア ](/help/profile/home.md#profile-data-store) への接続を表します。

>[!BEGINSHADEBOX]

**リクエスト**

+++ソース接続を作成 – リクエスト

リクエストの例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。このコメントによって追加情報が提供されます。 リクエストを選択したターミナルにコピー&amp;ペーストする際に、インラインコメントを削除します。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--data-raw '{
   "name":"Connect to Profile store",
   "description":"Optional",
   "connectionSpec":{
      "id":"8a9c3494-9708-43d7-ae3f-cda01e5030e1", // this connection spec ID is always the same for Source Connections
      "version":"1.0"
   }
}'
```

+++

**応答**

+++ソース接続の作成 – 応答

```json
{
    "id": "900df191-b983-45cd-90d5-4c7a0326d650",
    "etag": "\"0500ebe1-0000-0200-0000-63e28d060000\""
}
```

+++

>[!ENDSHADEBOX]

リクエストが成功した場合は、新しく作成したソース接続の ID （`id`）と `etag` が返されます。 後でデータフローを作成する際に必要になるので、ソース接続 ID をメモしておきます。

## ベース接続の作成 {#create-base-connection}

![ ユーザーがオンになっている現在の手順をハイライト表示するオーディエンスをアクティブ化する手順 ](/help/destinations/assets/api/file-based-segment-export/step3.png)

[ ベース接続 ](https://developer.adobe.com/experience-platform-apis/references/destinations/#tag/Glossary) は、資格情報を宛先に安全に保存します。 宛先のタイプによって、その宛先に対して認証するために必要な資格情報は異なる場合があります。 これらの認証パラメーターを見つけるには、`connection spec` オーディエンスの書き出し先の宛先の選択 [ の節で説明されているように、最初に目的の宛先の ](#select-destination) を取得してから、応答の `authSpec` を確認します。 サポートされているすべての宛先の `authSpec` プロパティについては、以下のタブを参照してください。

>[!BEGINTABS]

>[!TAB Amazon S3]

+++[!DNL Amazon S3] を表示している [!DNL Connection spec] - [!DNL auth spec]

次の [!DNL connection spec] の例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。この例では、[!DNL connection spec] 内の認証パラメーターの場所に関する追加情報を提供しています。

```json {line-numbers="true" start-line="1" highlight="8"}
{
    "items": [
        {
        "id": "4fce964d-3f37-408f-9778-e597338a21ee",
        "name": "amazon-s3",
        "providerId": "14e34fac-d307-11e9-bb65-2a2ae2dbcce4",
        "version": "1.0",
        "authSpec": [
            {
            "name": "Access Key",
            "type": "KeyBased",
            "spec": {
                "$schema": "http://json-schema.org/draft-07/schema#",
                "description": "Defines auth params required for connecting to amazon-s3",
                "type": "object",
                "properties": {
                "s3AccessKey": {
                    "description": "Access key id",
                    "type": "string",
                    "pattern": "^[A-Z2-7]{20}$"
                },
                "s3SecretKey": {
                    "description": "Secret access key for the user account",
                    "type": "string",
                    "format": "password",
                    "pattern": "^[A-Za-z0-9\\/\\+]{40}$"
                }
                },
                "required": [
                "s3SecretKey",
                "s3AccessKey"
                ]
            }
            },
            {
            "name": "Assumed Role",
            "type": "S3RoleBased",
            "spec": {
                "$schema": "http://json-schema.org/draft-07/schema#",
                "description": "Defines role based auth params required for connecting to amazon-s3",
                "type": "object",
                "properties": {
                "s3Role": {
                    "title": "Role",
                    "description": "S3 role",
                    "type": "string",
                    "format": "password"
                }
                },
                "required": [
                "s3Role"
                ]
            }
            }
        ],
//...
```

+++

>[!TAB Azure Blob Storage]

+++[!DNL Azure Blob Storage] を表示している [!DNL Connection spec] - [!DNL auth spec]

次の [!DNL connection spec] の例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。この例では、[!DNL connection spec] 内の認証パラメーターの場所に関する追加情報を提供しています。

```json {line-numbers="true" start-line="1" highlight="8"}
{
    "items": [
        {
            "id": "6d6b59bf-fb58-4107-9064-4d246c0e5bb2",
            "name": "Azure Blob Storage",
            "providerId": "14e34fac-d307-11e9-bb65-2a2ae2dbcce4",
            "version": "1.0",
            "authSpec": [ // describes the authentication parameters
                {
                    "name": "ConnectionString",
                    "type": "ConnectionString",
                    "spec": {
                        "$schema": "http://json-schema.org/draft-07/schema#",
                        "description": "Connection String for Azure Blob based destinations",
                        "type": "object",
                        "properties": {
                            "connectionString": {
                                "description": "connection string for login",
                                "type": "string",
                                "format": "password"
                            }
                        },
                        "required": [
                            "connectionString"
                        ]
                    }
                }
            ],
//...
```

+++


>[!TAB Azure Data Lake Gen 2 （ADLS Gen2） ]

+++[!DNL Azure Data Lake Gen 2(ADLS Gen2)] を表示している [!DNL Connection spec] - [!DNL auth spec]

次の [!DNL connection spec] の例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。この例では、[!DNL connection spec] 内の認証パラメーターの場所に関する追加情報を提供しています。

```json {line-numbers="true" start-line="1" highlight="8"}
{
    "items": [
        {
            "id": "be2c3209-53bc-47e7-ab25-145db8b873e1",
            "name": "Azure Data Lake Gen2",
            "providerId": "14e34fac-d307-11e9-bb65-2a2ae2dbcce4",
            "version": "1.0",
            "authSpec": [ // describes the authentication parameters
                {
                    "name": "Azure Service Principal Auth",
                    "type": "AzureServicePrincipal",
                    "spec": {
                        "$schema": "http://json-schema.org/draft-07/schema#",
                        "description": "defines auth params required for connecting to adlsgen2 using service principal",
                        "type": "object",
                        "properties": {
                            "url": {
                                "description": "Endpoint for Azure Data Lake Storage Gen2.",
                                "type": "string"
                            },
                            "servicePrincipalId": {
                                "description": "Service Principal Id to connect to ADLSGen2.",
                                "type": "string"
                            },
                            "servicePrincipalKey": {
                                "description": "Service Principal Key to connect to ADLSGen2.",
                                "type": "string",
                                "format": "password"
                            },
                            "tenant": {
                                "description": "Tenant information(domain name or tenant ID).",
                                "type": "string"
                            }
                        },
                        "required": [
                            "servicePrincipalKey",
                            "url",
                            "tenant",
                            "servicePrincipalId"
                        ]
                    }
                }
            ],
//...
```

+++


>[!TAB  データランディングゾーン（DLZ） ]

+++[!DNL Data Landing Zone(DLZ)] を表示している [!DNL Connection spec] - [!DNL auth spec]

>[!NOTE]
>
>データランディングゾーンの宛先には [!DNL auth spec] は必要ありません。

```json
{
    "items": [
        {
            "id": "10440537-2a7b-4583-ac39-ed38d4b848e8",
            "name": "Data Landing Zone",
            "providerId": "14e34fac-d307-11e9-bb65-2a2ae2dbcce4",
            "version": "1.0",
            "authSpec": [],
//...
```

+++

>[!TAB Google Cloud Storage]

+++[!DNL Google Cloud Storage] を表示している [!DNL Connection spec] - [!DNL auth spec]

次の [!DNL connection spec] の例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。この例では、[!DNL connection spec] 内の認証パラメーターの場所に関する追加情報を提供しています。

```json {line-numbers="true" start-line="1" highlight="8"}
{
    "items": [
        {
            "id": "c5d93acb-ea8b-4b14-8f53-02138444ae99",
            "name": "Google Cloud Storage",
            "providerId": "14e34fac-d307-11e9-bb65-2a2ae2dbcce4",
            "version": "1.0",
            "authSpec": [ // describes the authentication parameters
                {
                    "name": "Google Cloud Storage authentication credentials",
                    "type": "GoogleCloudStorageAuth",
                    "spec": {
                        "$schema": "http://json-schema.org/draft-07/schema#",
                        "description": "defines auth params required for connecting to google cloud storage connector.",
                        "type": "object",
                        "properties": {
                            "accessKeyId": {
                                "description": "Access Key Id for the user account",
                                "type": "string"
                            },
                            "secretAccessKey": {
                                "description": "Secret Access Key for the user account",
                                "type": "string",
                                "format": "password"
                            }
                        },
                        "required": [
                            "accessKeyId",
                            "secretAccessKey"
                        ]
                    }
                }
            ],
//...
```

+++

>[!TAB SFTP]

+++SFTP - [!DNL Connection spec] を表示する [!DNL auth spec]

>[!NOTE]
>
>SFTP 宛先には、パスワードと SSH キー認証の両方をサポートしているので、[!DNL auth spec] に 2 つの異なる項目が含まれています。

次の [!DNL connection spec] の例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。この例では、[!DNL connection spec] 内の認証パラメーターの場所に関する追加情報を提供しています。

```json {line-numbers="true" start-line="1" highlight="8"}
{
    "items": [
        {
            "id": "36965a81-b1c6-401b-99f8-22508f1e6a26",
            "name": "SFTP",
            "providerId": "14e34fac-d307-11e9-bb65-2a2ae2dbcce4",
            "version": "1.0",
            "authSpec": [ // describes the authentication parameters
                {
                    "name": "SFTP with Password",
                    "type": "SFTP",
                    "spec": {
                        "$schema": "http://json-schema.org/draft-07/schema#",
                        "description": "defines auth params required for connecting to sftp locations with a password",
                        "type": "object",
                        "properties": {
                            "domain": {
                                "description": "Domain of server",
                                "type": "string"
                            },
                            "username": {
                                "description": "Username",
                                "type": "string"
                            },
                            "password": {
                                "description": "Password",
                                "type": "string",
                                "format": "password"
                            }
                        },
                        "required": [
                            "password",
                            "domain",
                            "username"
                        ]
                    }
                },
                {
                    "name": "SFTP with SSH Key",
                    "type": "SFTP",
                    "spec": {
                        "$schema": "http://json-schema.org/draft-07/schema#",
                        "description": "defines auth params required for connecting to sftp locations using SSH Key",
                        "type": "object",
                        "properties": {
                            "domain": {
                                "description": "Domain of server",
                                "type": "string"
                            },
                            "username": {
                                "description": "Username",
                                "type": "string"
                            },
                            "sshKey": {
                                "description": "Base64 string of the private SSH key",
                                "type": "string",
                                "format": "password",
                                "contentEncoding": "base64",
                                "uiAttributes": {
                                    "tooltip": {
                                        "id": "platform_destinations_connect_sftp_ssh",
                                        "fallbackUrl": "http://www.adobe.com/go/destinations-sftp-connection-parameters-en "
                                    }
                                }
                            }
                        },
                        "required": [
                            "sshKey",
                            "domain",
                            "username"
                        ]
                    }
                }
            ],
//...
```

+++

>[!ENDTABS]

認証仕様で指定されたプロパティ（つまり、応答からの `authSpec`）を使用して、以下の例に示すように、各宛先タイプに固有の必要な資格情報を含むベース接続を作成できます。

>[!BEGINTABS]

>[!TAB Amazon S3]

**リクエスト**

+++[!DNL Amazon S3] - アクセスキーと秘密鍵の認証を使用したベース接続リクエスト

>[!TIP]
>
>必要な認証資格情報の取得方法について詳しくは、Amazon S3 の宛先ドキュメントページの [ 宛先への認証 ](/help/destinations/catalog/cloud-storage/amazon-s3.md#authenticate) の節を参照してください。

リクエストの例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。このコメントによって追加情報が提供されます。 リクエストを選択したターミナルにコピー&amp;ペーストする際に、リクエスト内のインラインコメントを削除します。

```shell {line-numbers="true" start-line="1" highlight="18"}
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/connections' \
--header 'accept: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "name": "Amazon S3 Base Connection",
  "auth": {
    "specName": "Access Key",
    "params": {
      "s3SecretKey": "<Add secret key>",
      "s3AccessKey": "<Add access key>"
    }
  },
  "connectionSpec": {
    "id": "4fce964d-3f37-408f-9778-e597338a21ee", // Amazon S3 connection spec
    "version": "1.0"
  }
}'
```

+++

+++[!DNL Amazon S3] – 想定される役割認証を使用したベース接続リクエスト

>[!TIP]
>
>必要な認証資格情報の取得方法について詳しくは、Amazon S3 の宛先ドキュメントページの [ 宛先への認証 ](/help/destinations/catalog/cloud-storage/amazon-s3.md#authenticate) の節を参照してください。

リクエストの例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。このコメントによって追加情報が提供されます。 リクエストを選択したターミナルにコピー&amp;ペーストする際に、リクエスト内のインラインコメントを削除します。

```shell {line-numbers="true" start-line="1" highlight="17"}
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/connections' \
--header 'accept: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "name": "Amazon S3 Base Connection",
  "auth": {
    "specName": "Assumed Role",
    "params": {
      "s3Role": "<Add s3 role>"
    }
  },
  "connectionSpec": {
    "id": "4fce964d-3f37-408f-9778-e597338a21ee", // Amazon S3 connection spec
    "version": "1.0"
  }
}'
```

+++

**応答**

+++[!DNL Amazon S3] ベース接続応答

```json
{
    "id": "12401496-2573-4ca7-8137-fef1aeb9dd4c",
    "etag": "\"0000d781-0000-0200-0000-63e29f420000\""
}
```

+++

>[!TAB Azure Blob Storage]

**リクエスト**

+++[!DNL Azure Blob Storage] - ベース接続リクエスト

>[!TIP]
>
>必要な認証資格情報の取得方法について詳しくは、Azure Blob Storage 宛先ドキュメントページの [ 宛先への認証 ](/help/destinations/catalog/cloud-storage/azure-blob.md#authenticate) の節を参照してください。

リクエストの例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。このコメントによって追加情報が提供されます。 リクエストを選択したターミナルにコピー&amp;ペーストする際に、リクエスト内のインラインコメントを削除します。

```shell {line-numbers="true" start-line="1" highlight="17"}
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/connections' \
--header 'accept: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "name": "Azure Blob Storage Base Connection",
  "auth": {
    "specName": "ConnectionString",
    "params": {
      "connectionString": "<Add Azure Blob connection string>"
    }
  },
  "connectionSpec": {
    "id": "6d6b59bf-fb58-4107-9064-4d246c0e5bb2", // Azure Blob Storage connection spec
    "version": "1.0"
  }
}'
```

+++

**応答**

+++[!DNL Azure Blob Storage] - ベース接続応答

```json
{
    "id": "12401496-2573-4ca7-8137-fef1aeb9dd4c",
    "etag": "\"0000d781-0000-0200-0000-63e29f420000\""
}
```

+++

>[!TAB Azure Data Lake Gen 2 （ADLS Gen2） ]

**リクエスト**

+++[!DNL Azure Data Lake Gen 2(ADLS Gen2)] - ベース接続リクエスト

>[!TIP]
>
>必要な認証資格情報の取得方法については、Azure Data Lake Gen 2 （ADLS Gen2）宛先ドキュメントページの [ 宛先への認証 ](/help/destinations/catalog/cloud-storage/adls-gen2.md#authenticate) の節を参照してください。

リクエストの例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。このコメントによって追加情報が提供されます。 リクエストを選択したターミナルにコピー&amp;ペーストする際に、リクエスト内のインラインコメントを削除します。

```shell {line-numbers="true" start-line="1" highlight="20"}
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/connections' \
--header 'accept: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "name": "Azure Data Lake Gen 2(ADLS Gen2) Base Connection",
  "auth": {
    "specName": "Azure Service Principal Auth",
    "params": {
      "servicePrincipalKey": "<Add servicePrincipalKey>",
      "url": "<Add url>",
      "tenant": "<Add tenant>",
      "servicePrincipalId": "<Add servicePrincipalId>"
    }
  },
  "connectionSpec": {
    "id": "be2c3209-53bc-47e7-ab25-145db8b873e1", // Azure Data Lake Gen 2(ADLS Gen2) connection spec
    "version": "1.0"
  }
}'
```

+++

**応答**

+++[!DNL Azure Data Lake Gen 2(ADLS Gen2)] - ベース接続応答

```json
{
    "id": "12401496-2573-4ca7-8137-fef1aeb9dd4c",
    "etag": "\"0000d781-0000-0200-0000-63e29f420000\""
}
```

+++

>[!TAB  データランディングゾーン（DLZ） ]

**リクエスト**

+++[!DNL Data Landing Zone(DLZ)] - ベース接続リクエスト

>[!TIP]
>
>データランディングゾーンの宛先には、認証資格情報は必要ありません。 詳しくは、データランディングゾーンの宛先に関するドキュメントページの [ 宛先への認証 ](/help/destinations/catalog/cloud-storage/data-landing-zone.md#authenticate) の節を参照してください。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/connections' \
--header 'accept: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "name": "Data Landing Zone(DLZ) Base Connection"
}'
```

+++

**応答**

+++[!DNL Data Landing Zone] - ベース接続応答

```json
{
    "id": "12401496-2573-4ca7-8137-fef1aeb9dd4c",
    "etag": "\"0000d781-0000-0200-0000-63e29f420000\""
}
```

+++

>[!TAB Google Cloud Storage]

**リクエスト**

+++[!DNL Google Cloud Storage] - ベース接続リクエスト

>[!TIP]
>
>必要な認証資格情報の取得方法について詳しくは、Google Cloud Storage の宛先ドキュメントページの [ 宛先への認証 ](/help/destinations/catalog/cloud-storage/google-cloud-storage.md#authenticate) の節を参照してください。

リクエストの例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。このコメントによって追加情報が提供されます。 リクエストを選択したターミナルにコピー&amp;ペーストする際に、リクエスト内のインラインコメントを削除します。

```shell {line-numbers="true" start-line="1" highlight="18"}
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/connections' \
--header 'accept: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "name": "Google Cloud Storage Base Connection",
  "auth": {
    "specName": "Google Cloud Storage authentication credentials",
    "params": {
      "accessKeyId": "<Add accessKeyId>",
      "secretAccessKey": "<Add secret Access Key>"
    }
  },
  "connectionSpec": {
    "id": "c5d93acb-ea8b-4b14-8f53-02138444ae99", // Google Cloud Storage connection spec
    "version": "1.0"
  }
}'
```

+++

**応答**

+++[!DNL Google Cloud Storage] - ベース接続応答

```json
{
    "id": "12401496-2573-4ca7-8137-fef1aeb9dd4c",
    "etag": "\"0000d781-0000-0200-0000-63e29f420000\""
}
```

+++

>[!TAB SFTP]

**リクエスト**

+++パスワードを使用した SFTP - ベース接続リクエスト

>[!TIP]
>
>必要な認証資格情報の取得方法について詳しくは、SFTP 宛先ドキュメントページの [ 宛先への認証 ](/help/destinations/catalog/cloud-storage/sftp.md#authentication-information) の節を参照してください。

リクエストの例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。このコメントによって追加情報が提供されます。 リクエストを選択したターミナルにコピー&amp;ペーストする際に、リクエスト内のインラインコメントを削除します。

```shell {line-numbers="true" start-line="1" highlight="19"}
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/connections' \
--header 'accept: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "name": "SFTP with password Base Connection",
  "auth": {
    "specName": "SFTP with Password",
    "params": {
      "domain": "<Add domain>",
      "username": "<Add username>",
      "password": "<Add password>",
      "port": "<Add port>"      
    }
  },
  "connectionSpec": {
    "id": "36965a81-b1c6-401b-99f8-22508f1e6a26", // SFTP connection spec
    "version": "1.0"
  }
}'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `specName` | `SFTP with Password`.を使用します。 |
| `domain` | SFTP ストレージの場所の IP アドレスまたはドメイン名。 |
| `username` | SFTP ストレージの場所にログインするためのユーザー名。 |
| `password` | SFTP ストレージの場所にログインするためのパスワード。 |
| `port` | SFTP ストレージの場所で使用されるポート。 |

{style="table-layout:auto"}

+++

+++SSH キーを使用した SFTP - ベース接続リクエスト

>[!TIP]
>
>必要な認証資格情報の取得方法について詳しくは、SFTP 宛先ドキュメントページの [ 宛先への認証 ](/help/destinations/catalog/cloud-storage/sftp.md#authentication-information) の節を参照してください。

リクエストの例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。このコメントによって追加情報が提供されます。 リクエストを選択したターミナルにコピー&amp;ペーストする際に、リクエスト内のインラインコメントを削除します。

```shell {line-numbers="true" start-line="1" highlight="19"}
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/connections' \
--header 'accept: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "name": "SFTP with SSH key Base Connection",
  "auth": {
    "specName": "SFTP with SSH Key",
    "params": {
      "domain": "<Add domain>",
      "username": "<Add username>",
      "sshKey": "<Add SSH key>",
      "port": "<Add port>"
    }
  },
  "connectionSpec": {
    "id": "36965a81-b1c6-401b-99f8-22508f1e6a26", // SFTP connection spec
    "version": "1.0"
  }
}'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `specName` | `SFTP with Password`.を使用します。 |
| `domain` | SFTP ストレージの場所の IP アドレスまたはドメイン名。 |
| `username` | SFTP ストレージの場所にログインするためのユーザー名。 |
| `sshKey` | SFTP ストレージの場所へのログインに使用する SSH 秘密鍵。 秘密鍵は、Base64 でエンコードされた文字列の形式にする必要があり、パスワードで保護しないでください。 |
| `port` | SFTP ストレージの場所で使用されるポート。 |

{style="table-layout:auto"}

+++

**応答**

+++SFTP - ベース接続応答

```json
{
    "id": "12401496-2573-4ca7-8137-fef1aeb9dd4c",
    "etag": "\"0000d781-0000-0200-0000-63e29f420000\""
}
```

+++

>[!ENDTABS]

### 書き出したファイルへの暗号化の追加

オプションで、書き出したファイルに暗号化を追加できます。 これを行うには、`encryptionSpecs` から項目を追加する必要があります。 必須パラメーターがハイライト表示された以下のリクエストの例を参照してください。


>[!BEGINSHADEBOX]

+++ クラウドストレージの宛先の暗号化仕様の表示

```json {line-numbers="true" start-line="1" highlight="26-27"}
           "encryptionSpecs": [
                {
                    "name": "File PGP/GPG Encryption",
                    "type": "FileAsymmetric",
                    "spec": {
                        "$schema": "http://json-schema.org/draft-07/schema#",
                        "description": "Defines parameters required for capturing user's inputs for encryption",
                        "type": "object",
                        "properties": {
                            "publicKey": {
                                "description": "Base64 encoded RSA public key",
                                "type": "string",
                                "contentEncoding": "base64"
                            },
                            "encryptionAlgo": {
                                "description": "Algorithm for encryption.",
                                "type": "string",
                                "default": "PGPGPG",
                                "enum": [
                                    "PGPGPG",
                                    "NONE"
                                ]
                            }
                        },
                        "required": [
                            "encryptionAlgo",
                            "publicKey"
                        ]
                    }
                }
            ]
```

+++ 

**リクエスト**

+++ベース接続への暗号化の追加 – リクエスト

リクエストの例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。このコメントによって追加情報が提供されます。 リクエストを選択したターミナルにコピー&amp;ペーストする際に、インラインコメントを削除します。

```shell {line-numbers="true" start-line="1" highlight="19"}
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/connections' \
--header 'accept: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "name": "SFTP with SSH key Base Connection",
  "auth": {
    "specName": "SFTP with SSH Key",
    "params": {
      "domain": "<Add domain>",
      "username": "<Add username>",
      "sshKey": "<Add SSH key>"
      }
    },
  "encryptionSpecs":{
     "specName": "Encryption spec",
     "params": {
         "encryptionAlgo":"PGPGPG",
         "publicKey":"<Add public key>"
      }            
    },
  "connectionSpec": {
    "id": "36965a81-b1c6-401b-99f8-22508f1e6a26", // SFTP connection spec
    "version": "1.0"
  }
}'
```

+++

**応答**

+++ベース接続への暗号化の追加 – 応答

```json
{
    "id": "900df191-b983-45cd-90d5-4c7a0326d650",
    "etag": "\"0500ebe1-0000-0200-0000-63e28d060000\""
}
```

+++

>[!ENDSHADEBOX]

応答からの接続 ID をメモしておきます。 この ID は、次の手順でターゲット接続を作成する際に必要になります。

## ターゲット接続の作成 {#create-target-connection}

![ ユーザーがオンになっている現在の手順をハイライト表示するオーディエンスをアクティブ化する手順 ](/help/destinations/assets/api/file-based-segment-export/step4.png)

次に、ターゲット接続を作成する必要があります。 [ ターゲット接続 ](https://developer.adobe.com/experience-platform-apis/references/destinations/#tag/Glossary) 書き出されたオーディエンスの書き出しパラメーターを保存します。 書き出しパラメータには、書き出し場所、ファイル形式、圧縮、およびその他の詳細が含まれます。 例えば、CSV ファイルの場合は、複数の書き出しオプションを選択できます。 [ ファイル形式設定ページ ](/help/destinations/ui/batch-destinations-file-formatting-options.md) で、サポートされるすべての CSV 書き出しオプションに関する詳細な情報を取得します。

各宛先タイプでサポートされているプロパティを理解するには、宛先の `targetSpec` で提供されている `connection spec` のプロパティを参照してください。 サポートされているすべての宛先の `targetSpec` プロパティについては、以下のタブを参照してください。

>[!BEGINTABS]

>[!TAB Amazon S3]

+++[!DNL Amazon S3] - ターゲット接続パラメーターを示す [!DNL Connection spec]

次の [!DNL connection spec] の例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。これは、接続仕様で [!DNL target spec] のパラメーターが見つかる場所に関する追加情報を提供します。 また、以下の例では、オーディエンス書き出しの宛先に適用されない *適用されない* ターゲットパラメーターも確認できます。

```json {line-numbers="true" start-line="1" highlight="10,56"}
{
    "items": [
        {
            "id": "4fce964d-3f37-408f-9778-e597338a21ee",
            "name": "Amazon S3",
            "providerId": "14e34fac-d307-11e9-bb65-2a2ae2dbcce4",
            "version": "1.0",
            "authSpec": [...],
            "encryptionSpecs": [...],
            "targetSpec": { //describes the target connection parameters
                "name": "User based target",
                "type": "UserNamespace",
                "spec": {
                    "$schema": "http://json-schema.org/draft-07/schema#",
                    "type": "object",
                    "properties": {
                        "bucketName": {
                            "title": "Bucket name",
                            "description": "Bucket name",
                            "type": "string",
                            "pattern": "(?=^.{3,63}$)(?!^(\\d+\\.)+\\d+$)(^(([a-z0-9]|[a-z0-9][a-z0-9\\-]*[a-z0-9])\\.)*([a-z0-9]|[a-z0-9][a-z0-9\\-]*[a-z0-9])$)",
                            "uiAttributes": {
                                "tooltip": {
                                    "id": "platform_destinations_connect_s3_bucket",
                                    "fallbackUrl": "http://www.adobe.com/go/destinations-amazon-s3-connection-parameters-en"
                                }
                            }
                        },
                        "path": {
                            "title": "Folder path",
                            "description": "Output path for copying files",
                            "type": "string",
                            "pattern": "^[0-9a-zA-Z\/\\!\\-_\\.\\*\\''\\(\\)]*((\\%SEGMENT_(NAME|ID)\\%)?\/?)+$",
                            "uiAttributes": {
                                "tooltip": {
                                    "id": "platform_destinations_connect_s3_folderpath",
                                    "fallbackUrl": "http://www.adobe.com/go/destinations-amazon-s3-connection-parameters-en"
                                }
                            }
                        },
                        "fileType": {
                            "conditional": {
                                "field": "flowSpec.attributes._workflow",
                                "operator": "NOT_CONTAINS",
                                "value": "DATASETS"
                            },
                            "title": "File Type",
                            "description": "Select file format",
                            "type": "string",
                            "enum": [
                                "CSV",
                                "JSON",
                                "PARQUET"
                            ]
                        },
                        "datasetFileType": { // does not apply to audience export destinations
                            "conditional": {
                                "field": "flowSpec.attributes._workflow",
                                "operator": "CONTAINS",
                                "value": "DATASETS"
                            },
                            "title": "File Type",
                            "description": "Select file format",
                            "type": "string",
                            "enum": [
                                "JSON",
                                "PARQUET"
                            ]
                        },
                        "csvOptions": {
                            "conditional": {
                                "field": "fileType",
                                "operator": "EQUALS",
                                "value": "CSV"
                            },
                            "title": "CSV Options",
                            "description": "Select your CSV options",
                            "type": "object",
                            "properties": {
                                "quote": {
                                    "title": "Quote Character",
                                    "description": "Select your Quote character",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "Double Quotes (\")",
                                            "const": "\""
                                        },
                                        {
                                            "title":"Null Character (\\)",
                                            "const": ""
                                        }
                                    ],
                                    "default": ""
                                },
                                "escape": {
                                    "title": "Escape Character",
                                    "description": "Select your Escape character",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "Back Slash (\\)",
                                            "const": "\\"
                                        },
                                        {
                                            "title": "Single Quote (')",
                                            "const": "'"
                                        }
                                    ],
                                    "default": "\\"
                                },
                                "delimiter": {
                                    "title": "Delimiter",
                                    "description": "Select your Delimiter",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "Comma (,)",
                                            "const": ","
                                        },
                                        {
                                            "title": "Tab (\\t)",
                                            "const": "\t"
                                        },
                                        {
                                            "title": "Pipe (|)",
                                            "const": "|"
                                        },
                                        {
                                            "title": "Semicolon (;)",
                                            "const": ";"
                                        },
                                        {
                                            "title": "Colon (:)",
                                            "const": ":"
                                        }
                                    ],
                                    "default": ","
                                },
                                "nullValue": {
                                    "title": "Null Value",
                                    "description": "Select the output value of 'null' fields",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "null",
                                            "const": "null"
                                        },
                                        {
                                            "title": "\"\"",
                                            "const": "\"\""
                                        },
                                        {
                                            "title": "Empty String",
                                            "const": ""
                                        }
                                    ],
                                    "default": "null"
                                },
                                "emptyValue": {
                                    "title": "Empty Value",
                                    "description": "Select the output value of blank fields",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "null",
                                            "const": "null"
                                        },
                                        {
                                            "title": "\"\"",
                                            "const": "\"\""
                                        },
                                        {
                                            "title": "Empty String",
                                            "const": ""
                                        }
                                    ],
                                    "default": ""
                                }
                            }
                        },
                        "compression": {
                            "title": "Compression format",
                            "description": "Select the desired file compression format.",
                            "type": "string",
                            "enum": [
                                "NONE",
                                "GZIP"
                            ]
                        }
                    },
                    "required": [
                        "bucketName",
                        "path",
                        "datasetFileType",
                        "compression",
                        "fileType"
                    ]
                }
//...
```

+++

>[!TAB Azure Blob Storage]

+++[!DNL Azure Blob Storage] - ターゲット接続パラメーターを示す [!DNL Connection spec]

次の [!DNL connection spec] の例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。これは、接続仕様で [!DNL target spec] のパラメーターが見つかる場所に関する追加情報を提供します。 また、以下の例では、オーディエンス書き出しの宛先に適用されない *適用されない* ターゲットパラメーターも確認できます。

```json {line-numbers="true" start-line="1" highlight="10,44"}
{
    "items": [
        {
            "id": "6d6b59bf-fb58-4107-9064-4d246c0e5bb2",
            "name": "Azure Blob Storage",
            "providerId": "14e34fac-d307-11e9-bb65-2a2ae2dbcce4",
            "version": "1.0",
            "authSpec": [...],
            "encryptionSpecs": [...],
            "targetSpec": { // describes the target connection parameters
                "name": "User based target",
                "type": "UserNamespace",
                "spec": {
                    "$schema": "http://json-schema.org/draft-07/schema#",
                    "type": "object",
                    "properties": {
                        "path": {
                            "title": "Folder path",
                            "description": "Output path (relative) indicating where to upload the data",
                            "type": "string",
                            "pattern": "^[0-9a-zA-Z\/\\!\\-_\\.\\*\\'\\(\\)]+$"
                        },
                        "container": {
                            "title": "Container",
                            "description": "Container within the storage where to upload the data",
                            "type": "string",
                            "pattern": "^[a-z0-9](?!.*--)[a-z0-9-]{1,61}[a-z0-9]$"
                        },
                        "fileType": {
                            "conditional": {
                                "field": "flowSpec.attributes._workflow",
                                "operator": "NOT_CONTAINS",
                                "value": "DATASETS"
                            },
                            "title": "File Type",
                            "description": "Select file format",
                            "type": "string",
                            "enum": [
                                "CSV",
                                "JSON",
                                "PARQUET"
                            ]
                        },
                        "datasetFileType": { // does not apply to audience export destinations
                            "conditional": {
                                "field": "flowSpec.attributes._workflow",
                                "operator": "CONTAINS",
                                "value": "DATASETS"
                            },
                            "title": "File Type",
                            "description": "Select file format",
                            "type": "string",
                            "enum": [
                                "JSON",
                                "PARQUET"
                            ]
                        },
                        "csvOptions": {
                            "conditional": {
                                "field": "fileType",
                                "operator": "EQUALS",
                                "value": "CSV"
                            },
                            "title": "CSV Options",
                            "description": "Select your CSV options",
                            "type": "object",
                            "properties": {
                                "quote": {
                                    "title": "Quote Character",
                                    "description": "Select your Quote character",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "Double Quotes (\")",
                                            "const": "\""
                                        },
                                        {
                                            "title":"Null Character (\\)",
                                            "const": ""
                                        }
                                    ],
                                    "default": ""
                                },
                                "escape": {
                                    "title": "Escape Character",
                                    "description": "Select your Escape character",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "Back Slash (\\)",
                                            "const": "\\"
                                        },
                                        {
                                            "title": "Single Quote (')",
                                            "const": "'"
                                        }
                                    ],
                                    "default": "\\"
                                },
                                "delimiter": {
                                    "title": "Delimiter",
                                    "description": "Select your Delimiter",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "Comma (,)",
                                            "const": ","
                                        },
                                        {
                                            "title": "Tab (\\t)",
                                            "const": "\t"
                                        },
                                        {
                                            "title": "Pipe (|)",
                                            "const": "|"
                                        },
                                        {
                                            "title": "Semicolon (;)",
                                            "const": ";"
                                        },
                                        {
                                            "title": "Colon (:)",
                                            "const": ":"
                                        }
                                    ],
                                    "default": ","
                                },
                                "nullValue": {
                                    "title": "Null Value",
                                    "description": "Select the output value of 'null' fields",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "null",
                                            "const": "null"
                                        },
                                        {
                                            "title": "\"\"",
                                            "const": "\"\""
                                        },
                                        {
                                            "title": "Empty String",
                                            "const": ""
                                        }
                                    ],
                                    "default": "null"
                                },
                                "emptyValue": {
                                    "title": "Empty Value",
                                    "description": "Select the output value of blank fields",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "null",
                                            "const": "null"
                                        },
                                        {
                                            "title": "\"\"",
                                            "const": "\"\""
                                        },
                                        {
                                            "title": "Empty String",
                                            "const": ""
                                        }
                                    ],
                                    "default": ""
                                }
                            }
                        },
                        "compression": {
                            "title": "Compression format",
                            "description": "Select the desired file compression format.",
                            "type": "string",
                            "enum": [
                                "NONE",
                                "GZIP"
                            ]
                        }
                    },
                    "required": [
                        "container",
                        "path",
                        "datasetFileType",
                        "compression",
                        "fileType"
                    ]
                }
//...
```

+++


>[!TAB Azure Data Lake Gen 2 （ADLS Gen2） ]

+++[!DNL Azure Data Lake Gen 2(ADLS Gen2)] - ターゲット接続パラメーターを示す [!DNL Connection spec]

次の [!DNL connection spec] の例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。これは、接続仕様で [!DNL target spec] のパラメーターが見つかる場所に関する追加情報を提供します。 また、以下の例では、オーディエンス書き出しの宛先に適用されない *適用されない* ターゲットパラメーターも確認できます。

```json {line-numbers="true" start-line="1" highlight="10,22,37"}
{
    "items": [
        {
            "id": "be2c3209-53bc-47e7-ab25-145db8b873e1",
            "name": "Azure Data Lake Gen2",
            "providerId": "14e34fac-d307-11e9-bb65-2a2ae2dbcce4",
            "version": "1.0",
            "authSpec": [...],
            "encryptionSpecs": [...],
            "targetSpec": { // describes the target connection parameters
                "name": "User based target",
                "type": "UserNamespace",
                "spec": {
                    "$schema": "http://json-schema.org/draft-07/schema#",
                    "type": "object",
                    "properties": {
                        "path": {
                            "title": "Folder path",
                            "description": "Enter the path to your Azure Data Lake Storage folder",
                            "type": "string"
                        },
                        "fileType": {
                            "conditional": {
                                "field": "flowSpec.attributes._workflow",
                                "operator": "NOT_CONTAINS",
                                "value": "DATASETS"
                            },
                            "title": "File Type",
                            "description": "Select file format",
                            "type": "string",
                            "enum": [
                                "CSV",
                                "JSON",
                                "PARQUET"
                            ]
                        },
                        "datasetFileType": { // does not apply to audience export destinations
                            "conditional": {
                                "field": "flowSpec.attributes._workflow",
                                "operator": "CONTAINS",
                                "value": "DATASETS"
                            },
                            "title": "File Type",
                            "description": "Select file format",
                            "type": "string",
                            "enum": [
                                "JSON",
                                "PARQUET"
                            ]
                        },
                        "csvOptions": {
                            "conditional": {
                                "field": "fileType",
                                "operator": "EQUALS",
                                "value": "CSV"
                            },
                            "title": "CSV Options",
                            "description": "Select your CSV options",
                            "type": "object",
                            "properties": {
                                "quote": {
                                    "title": "Quote Character",
                                    "description": "Select your Quote character",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "Double Quotes (\")",
                                            "const": "\""
                                        },
                                        {
                                            "title":"Null Character (\\)",
                                            "const": ""
                                        }
                                    ],
                                    "default": ""
                                },
                                "escape": {
                                    "title": "Escape Character",
                                    "description": "Select your Escape character",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "Back Slash (\\)",
                                            "const": "\\"
                                        },
                                        {
                                            "title": "Single Quote (')",
                                            "const": "'"
                                        }
                                    ],
                                    "default": "\\"
                                },
                                "delimiter": {
                                    "title": "Delimiter",
                                    "description": "Select your Delimiter",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "Comma (,)",
                                            "const": ","
                                        },
                                        {
                                            "title": "Tab (\\t)",
                                            "const": "\t"
                                        },
                                        {
                                            "title": "Pipe (|)",
                                            "const": "|"
                                        },
                                        {
                                            "title": "Semicolon (;)",
                                            "const": ";"
                                        },
                                        {
                                            "title": "Colon (:)",
                                            "const": ":"
                                        }
                                    ],
                                    "default": ","
                                },
                                "nullValue": {
                                    "title": "Null Value",
                                    "description": "Select the output value of 'null' fields",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "null",
                                            "const": "null"
                                        },
                                        {
                                            "title": "\"\"",
                                            "const": "\"\""
                                        },
                                        {
                                            "title": "Empty String",
                                            "const": ""
                                        }
                                    ],
                                    "default": "null"
                                },
                                "emptyValue": {
                                    "title": "Empty Value",
                                    "description": "Select the output value of blank fields",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "null",
                                            "const": "null"
                                        },
                                        {
                                            "title": "\"\"",
                                            "const": "\"\""
                                        },
                                        {
                                            "title": "Empty String",
                                            "const": ""
                                        }
                                    ],
                                    "default": ""
                                }
                            }
                        },
                        "compression": {
                            "title": "Compression format",
                            "description": "Select the desired file compression format.",
                            "type": "string",
                            "enum": [
                                "NONE",
                                "GZIP"
                            ]
                        }
                    },
                    "required": [
                        "path",
                        "datasetFileType",
                        "compression",
                        "fileType"
                    ]
                }
//...
```

+++

>[!TAB  データランディングゾーン（DLZ） ]

+++[!DNL Data Landing Zone(DLZ)] - ターゲット接続パラメーターを示す [!DNL Connection spec]

次の [!DNL connection spec] の例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。これは、接続仕様で [!DNL target spec] のパラメーターが見つかる場所に関する追加情報を提供します。 また、以下の例では、オーディエンス書き出しの宛先に適用されない *適用されない* ターゲットパラメーターも確認できます。

```json {line-numbers="true" start-line="1" highlight="9,36"}
"items": [
    {
        "id": "10440537-2a7b-4583-ac39-ed38d4b848e8",
        "name": "Data Landing Zone",
        "providerId": "14e34fac-d307-11e9-bb65-2a2ae2dbcce4",
        "version": "1.0",
        "authSpec": [],
        "encryptionSpecs": [],
            "targetSpec": { // describes the target connection parameters
                "name": "User based target",
                "type": "UserNamespace",
                "spec": {
                    "$schema": "http://json-schema.org/draft-07/schema#",
                    "type": "object",
                    "properties": {
                        "path": {
                            "title": "Folder path",
                            "description": "Enter the path to your Azure Data Lake Storage folder",
                            "type": "string"
                        },
                        "fileType": {
                            "conditional": {
                                "field": "flowSpec.attributes._workflow",
                                "operator": "NOT_CONTAINS",
                                "value": "DATASETS"
                            },
                            "title": "File Type",
                            "description": "Select file format",
                            "type": "string",
                            "enum": [
                                "CSV",
                                "JSON",
                                "PARQUET"
                            ]
                        },
                        "datasetFileType": { // does not apply to audience export destinations
                            "conditional": {
                                "field": "flowSpec.attributes._workflow",
                                "operator": "CONTAINS",
                                "value": "DATASETS"
                            },
                            "title": "File Type",
                            "description": "Select file format",
                            "type": "string",
                            "enum": [
                                "JSON",
                                "PARQUET"
                            ]
                        },
                        "csvOptions": {
                            "conditional": {
                                "field": "fileType",
                                "operator": "EQUALS",
                                "value": "CSV"
                            },
                            "title": "CSV Options",
                            "description": "Select your CSV options",
                            "type": "object",
                            "properties": {
                                "quote": {
                                    "title": "Quote Character",
                                    "description": "Select your Quote character",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "Double Quotes (\")",
                                            "const": "\""
                                        },
                                        {
                                            "title":"Null Character (\\)",
                                            "const": ""
                                        }
                                    ],
                                    "default": ""
                                },
                                "escape": {
                                    "title": "Escape Character",
                                    "description": "Select your Escape character",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "Back Slash (\\)",
                                            "const": "\\"
                                        },
                                        {
                                            "title": "Single Quote (')",
                                            "const": "'"
                                        }
                                    ],
                                    "default": "\\"
                                },
                                "delimiter": {
                                    "title": "Delimiter",
                                    "description": "Select your Delimiter",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "Comma (,)",
                                            "const": ","
                                        },
                                        {
                                            "title": "Tab (\\t)",
                                            "const": "\t"
                                        },
                                        {
                                            "title": "Pipe (|)",
                                            "const": "|"
                                        },
                                        {
                                            "title": "Semicolon (;)",
                                            "const": ";"
                                        },
                                        {
                                            "title": "Colon (:)",
                                            "const": ":"
                                        }
                                    ],
                                    "default": ","
                                },
                                "nullValue": {
                                    "title": "Null Value",
                                    "description": "Select the output value of 'null' fields",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "null",
                                            "const": "null"
                                        },
                                        {
                                            "title": "\"\"",
                                            "const": "\"\""
                                        },
                                        {
                                            "title": "Empty String",
                                            "const": ""
                                        }
                                    ],
                                    "default": "null"
                                },
                                "emptyValue": {
                                    "title": "Empty Value",
                                    "description": "Select the output value of blank fields",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "null",
                                            "const": "null"
                                        },
                                        {
                                            "title": "\"\"",
                                            "const": "\"\""
                                        },
                                        {
                                            "title": "Empty String",
                                            "const": ""
                                        }
                                    ],
                                    "default": ""
                                }
                            }
                        },
                        "compression": {
                            "title": "Compression format",
                            "description": "Select the desired file compression format.",
                            "type": "string",
                            "enum": [
                                "NONE",
                                "GZIP"
                            ]
                        }
                    },
                    "required": [
                        "path",
                        "datasetFileType",
                        "compression",
                        "fileType"
                    ]
                }
//...
```

+++

>[!TAB Google Cloud Storage]

+++[!DNL Google Cloud Storage] - ターゲット接続パラメーターを示す [!DNL Connection spec]

次の [!DNL connection spec] の例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。これは、接続仕様で [!DNL target spec] のパラメーターが見つかる場所に関する追加情報を提供します。 また、以下の例では、オーディエンス書き出しの宛先に適用されない *適用されない* ターゲットパラメーターも確認できます。

```json {line-numbers="true" start-line="1" highlight="10,44"}
{
    "items": [
        {
            "id": "c5d93acb-ea8b-4b14-8f53-02138444ae99",
            "name": "Google Cloud Storage",
            "providerId": "14e34fac-d307-11e9-bb65-2a2ae2dbcce4",
            "version": "1.0",
            "authSpec": [...],
            "encryptionSpecs": [...],
            "targetSpec": { // describes the target connection parameters
                "name": "User based target",
                "type": "UserNamespace",
                "spec": {
                    "$schema": "http://json-schema.org/draft-07/schema#",
                    "type": "object",
                    "properties": {
                        "bucketName": {
                            "title": "Bucket name",
                            "description": "Bucket name",
                            "type": "string",
                            "pattern": "(?!^goog.*$)(?!^.*g(o|0)(o|0)gle.*$)(((?=^.{3,63}$)(^([a-z0-9]|[a-z0-9][a-z0-9\\-_]*)[a-z0-9]$))|((?=^.{3,222}$)(?!^(\\d+\\.)+\\d+$)(^(([a-z0-9]{1,63}|[a-z0-9][a-z0-9\\-_]{1,61}[a-z0-9])\\.)*([a-z0-9]{1,63}|[a-z0-9][a-z0-9\\-_]{1,61}[a-z0-9])$)))"
                        },
                        "path": {
                            "title": "Folder path",
                            "description": "Output path for copying files",
                            "type": "string",
                            "pattern": "^[0-9a-zA-Z\/\\!\\-_\\.\\*\\''\\(\\)]*((\\%SEGMENT_(NAME|ID)\\%)?\/?)+$"
                        },
                        "fileType": {
                            "conditional": {
                                "field": "flowSpec.attributes._workflow",
                                "operator": "NOT_CONTAINS",
                                "value": "DATASETS"
                            },
                            "title": "File Type",
                            "description": "Select file format",
                            "type": "string",
                            "enum": [
                                "CSV",
                                "JSON",
                                "PARQUET"
                            ]
                        },
                        "datasetFileType": { // does not apply to audience export destinations
                            "conditional": {
                                "field": "flowSpec.attributes._workflow",
                                "operator": "CONTAINS",
                                "value": "DATASETS"
                            },
                            "title": "File Type",
                            "description": "Select file format",
                            "type": "string",
                            "enum": [
                                "JSON",
                                "PARQUET"
                            ]
                        },
                        "csvOptions": {
                            "conditional": {
                                "field": "fileType",
                                "operator": "EQUALS",
                                "value": "CSV"
                            },
                            "title": "CSV Options",
                            "description": "Select your CSV options",
                            "type": "object",
                            "properties": {
                                "quote": {
                                    "title": "Quote Character",
                                    "description": "Select your Quote character",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "Double Quotes (\")",
                                            "const": "\""
                                        },
                                        {
                                            "title":"Null Character (\\)",
                                            "const": ""
                                        }
                                    ],
                                    "default": ""
                                },
                                "escape": {
                                    "title": "Escape Character",
                                    "description": "Select your Escape character",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "Back Slash (\\)",
                                            "const": "\\"
                                        },
                                        {
                                            "title": "Single Quote (')",
                                            "const": "'"
                                        }
                                    ],
                                    "default": "\\"
                                },
                                "delimiter": {
                                    "title": "Delimiter",
                                    "description": "Select your Delimiter",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "Comma (,)",
                                            "const": ","
                                        },
                                        {
                                            "title": "Tab (\\t)",
                                            "const": "\t"
                                        },
                                        {
                                            "title": "Pipe (|)",
                                            "const": "|"
                                        },
                                        {
                                            "title": "Semicolon (;)",
                                            "const": ";"
                                        },
                                        {
                                            "title": "Colon (:)",
                                            "const": ":"
                                        }
                                    ],
                                    "default": ","
                                },
                                "nullValue": {
                                    "title": "Null Value",
                                    "description": "Select the output value of 'null' fields",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "null",
                                            "const": "null"
                                        },
                                        {
                                            "title": "\"\"",
                                            "const": "\"\""
                                        },
                                        {
                                            "title": "Empty String",
                                            "const": ""
                                        }
                                    ],
                                    "default": "null"
                                },
                                "emptyValue": {
                                    "title": "Empty Value",
                                    "description": "Select the output value of blank fields",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "null",
                                            "const": "null"
                                        },
                                        {
                                            "title": "\"\"",
                                            "const": "\"\""
                                        },
                                        {
                                            "title": "Empty String",
                                            "const": ""
                                        }
                                    ],
                                    "default": ""
                                }
                            }
                        },
                        "compression": {
                            "title": "Compression format",
                            "description": "Select the desired file compression format.",
                            "type": "string",
                            "enum": [
                                "NONE",
                                "GZIP"
                            ]
                        }
                    },
                    "required": [
                        "bucketName",
                        "path",
                        "datasetFileType",
                        "compression",
                        "fileType"
                    ]
                }
//...
```

+++

>[!TAB SFTP]

+++SFTP - ターゲット接続パラメーターを表示する [!DNL Connection spec]

次の [!DNL connection spec] の例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。これは、接続仕様で [!DNL target spec] のパラメーターが見つかる場所に関する追加情報を提供します。 また、以下の例では、オーディエンス書き出しの宛先に適用されない *適用されない* ターゲットパラメーターも確認できます。

```json {line-numbers="true" start-line="1" highlight="10,37"}
{
    "items": [
        {
            "id": "36965a81-b1c6-401b-99f8-22508f1e6a26",
            "name": "SFTP",
            "providerId": "14e34fac-d307-11e9-bb65-2a2ae2dbcce4",
            "version": "1.0",
            "authSpec": [...],
            "encryptionSpecs": [...],
            "targetSpec": { // describes the target connection parameters
                "name": "User based target",
                "type": "UserNamespace",
                "spec": {
                    "$schema": "http://json-schema.org/draft-07/schema#",
                    "type": "object",
                    "properties": {
                        "remotePath": {
                            "title": "Folder path",
                            "description": "Enter your folder path",
                            "type": "string"
                        },
                        "fileType": {
                            "conditional": {
                                "field": "flowSpec.attributes._workflow",
                                "operator": "NOT_CONTAINS",
                                "value": "DATASETS"
                            },
                            "title": "File Type",
                            "description": "Select file format",
                            "type": "string",
                            "enum": [
                                "CSV",
                                "JSON",
                                "PARQUET"
                            ]
                        },
                        "datasetFileType": { // does not apply to audience export destinations
                            "conditional": {
                                "field": "flowSpec.attributes._workflow",
                                "operator": "CONTAINS",
                                "value": "DATASETS"
                            },
                            "title": "File Type",
                            "description": "Select file format",
                            "type": "string",
                            "enum": [
                                "JSON",
                                "PARQUET"
                            ]
                        },
                        "csvOptions": {
                            "conditional": {
                                "field": "fileType",
                                "operator": "EQUALS",
                                "value": "CSV"
                            },
                            "title": "CSV Options",
                            "description": "Select your CSV options",
                            "type": "object",
                            "properties": {
                                "quote": {
                                    "title": "Quote Character",
                                    "description": "Select your Quote character",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "Double Quotes (\")",
                                            "const": "\""
                                        },
                                        {
                                            "title":"Null Character (\\)",
                                            "const": ""
                                        }
                                    ],
                                    "default": ""
                                },
                                "escape": {
                                    "title": "Escape Character",
                                    "description": "Select your Escape character",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "Back Slash (\\)",
                                            "const": "\\"
                                        },
                                        {
                                            "title": "Single Quote (')",
                                            "const": "'"
                                        }
                                    ],
                                    "default": "\\"
                                },
                                "delimiter": {
                                    "title": "Delimiter",
                                    "description": "Select your Delimiter",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "Comma (,)",
                                            "const": ","
                                        },
                                        {
                                            "title": "Tab (\\t)",
                                            "const": "\t"
                                        },
                                        {
                                            "title": "Pipe (|)",
                                            "const": "|"
                                        },
                                        {
                                            "title": "Semicolon (;)",
                                            "const": ";"
                                        },
                                        {
                                            "title": "Colon (:)",
                                            "const": ":"
                                        }
                                    ],
                                    "default": ","
                                },
                                "nullValue": {
                                    "title": "Null Value",
                                    "description": "Select the output value of 'null' fields",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "null",
                                            "const": "null"
                                        },
                                        {
                                            "title": "\"\"",
                                            "const": "\"\""
                                        },
                                        {
                                            "title": "Empty String",
                                            "const": ""
                                        }
                                    ],
                                    "default": "null"
                                },
                                "emptyValue": {
                                    "title": "Empty Value",
                                    "description": "Select the output value of blank fields",
                                    "type": "string",
                                    "oneOf": [
                                        {
                                            "title": "null",
                                            "const": "null"
                                        },
                                        {
                                            "title": "\"\"",
                                            "const": "\"\""
                                        },
                                        {
                                            "title": "Empty String",
                                            "const": ""
                                        }
                                    ],
                                    "default": ""
                                }
                            }
                        },
                        "compression": {
                            "title": "Compression format",
                            "description": "Select the desired file compression format.",
                            "type": "string",
                            "enum": [
                                "GZIP",
                                "NONE"
                            ]
                        }
                    },
                    "required": [
                        "remotePath",
                        "datasetFileType",
                        "compression",
                        "fileType"
                    ]
                },
//...
```

+++

>[!ENDTABS]

上記の仕様を使用すると、以下のタブに示すように、目的のクラウドストレージの宛先に固有のターゲット接続リクエストを作成できます。

>[!BEGINTABS]

>[!TAB Amazon S3]

**リクエスト**

+++[!DNL Amazon S3] - ターゲット接続リクエスト

>[!TIP]
>
>必要なターゲットパラメーターの取得方法について詳しくは、[ しい宛先のドキュメントページの ](/help/destinations/catalog/cloud-storage/amazon-s3.md#destination-details) 宛先の詳細の入力 [!DNL Amazon S3] の節を参照してください。

リクエストの例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。このコメントによって追加情報が提供されます。 リクエストを選択したターミナルにコピー&amp;ペーストする際に、リクエスト内のインラインコメントを削除します。

```shell {line-numbers="true" start-line="1" highlight="19"}
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--data-raw '{
    "name": "Amazon S3 Target Connection",
    "baseConnectionId": "<FROM_STEP_CREATE_BASE_CONNECTION>",
    "params": {
        "mode": "Server-to-server",
        "bucketName": "your-bucket-name",
        "path": "folder/subfolder",
        "compression": "NONE",
        "fileType": "JSON",
        "includeFileManifest": true // Include this parameter if you want to enable manifest file generation for your destination
    },
    "connectionSpec": {
        "id": "4fce964d-3f37-408f-9778-e597338a21ee", // Amazon S3 connection spec id
        "version": "1.0"
    }
}'
```

+++

+++[!DNL Amazon S3] - CSV オプションを使用したターゲット接続リクエスト

>[!TIP]
>
>ファイル書き出しに使用できる CSV オプションについて詳しくは、[ ファイル形式設定ページ ](/help/destinations/ui/batch-destinations-file-formatting-options.md) を参照してください。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--data-raw '{
   "name":"Amazon S3 Target Connection",
   "baseConnectionId":"<FROM_STEP_CREATE_BASE_CONNECTION>",
   "params":{
      "mode":"Server-to-server",
      "bucketName":"your-bucket-name",
      "path":"folder/subfolder",
      "compression":"GZIP",
      "fileType":"CSV",
      "includeFileManifest": true, //Include this parameter if you want to enable manifest file generation for your destination
      "csvOptions":{
         "nullValue":"null",
         "emptyValue":"",
         "escape":"\\",
         "quote":"",
         "delimiter":","
      }
   },
   "connectionSpec":{
      "id":"4fce964d-3f37-408f-9778-e597338a21ee",
      "version":"1.0"
   }
}'
```

+++

**応答**

+++ターゲット接続 – 応答

```json
{
    "id": "12401496-2573-4ca7-8137-fef1aeb9dd4c",
    "etag": "\"0000d781-0000-0200-0000-63e29f420000\""
}
```

+++

>[!TAB Azure Blob Storage]

**リクエスト**

+++[!DNL Azure Blob Storage] - ターゲット接続リクエスト

>[!TIP]
>
>必要なターゲットパラメーターの取得方法について詳しくは、[ しい宛先のドキュメントページの ](/help/destinations/catalog/cloud-storage/azure-blob.md#destination-details) 宛先の詳細の入力 [!DNL Azure Blob Storage] の節を参照してください。

リクエストの例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。このコメントによって追加情報が提供されます。 リクエストを選択したターミナルにコピー&amp;ペーストする際に、リクエスト内のインラインコメントを削除します。

```shell {line-numbers="true" start-line="1" highlight="19"}
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--data-raw '{
    "name": "Azure Blob Storage Target Connection",
    "baseConnectionId": "<FROM_STEP_CREATE_BASE_CONNECTION>",
    "params": {
        "mode": "Server-to-server",
        "container": "your-container-name",
        "path": "folder/subfolder",
        "compression": "NONE",
        "fileType": "JSON",
        "includeFileManifest": true // Include this parameter if you want to enable manifest file generation for your destination
    },
    "connectionSpec": {
        "id": "6d6b59bf-fb58-4107-9064-4d246c0e5bb2", // Azure Blob Storage connection spec id
        "version": "1.0"
    }
}'
```

+++

+++[!DNL Azure Blob Storage] - CSV オプションを使用したターゲット接続リクエスト

>[!TIP]
>
>ファイル書き出しに使用できる CSV オプションについて詳しくは、[ ファイル形式設定ページ ](/help/destinations/ui/batch-destinations-file-formatting-options.md) を参照してください。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--data-raw '{
   "name":"Azure Blob Storage Target Connection",
   "baseConnectionId":"<FROM_STEP_CREATE_BASE_CONNECTION>",
   "params":{
      "mode":"Server-to-server",
      "bucketName":"your-bucket-name",
      "path":"folder/subfolder",
      "compression":"GZIP",
      "fileType":"CSV",
      "includeFileManifest": true, //Include this parameter if you want to enable manifest file generation for your destination
      "csvOptions":{
         "nullValue":"null",
         "emptyValue":"",
         "escape":"\\",
         "quote":"",
         "delimiter":","
      }
   },
   "connectionSpec":{
      "id":"6d6b59bf-fb58-4107-9064-4d246c0e5bb2",
      "version":"1.0"
   }
}'
```

+++

**応答**

+++ターゲット接続 – 応答

```json
{
    "id": "12401496-2573-4ca7-8137-fef1aeb9dd4c",
    "etag": "\"0000d781-0000-0200-0000-63e29f420000\""
}
```

+++

>[!TAB Azure Data Lake Gen 2 （ADLS Gen2） ]

**リクエスト**

+++[!DNL Azure Data Lake Gen 2(ADLS Gen2)] - ターゲット接続リクエスト

>[!TIP]
>
>必要なターゲットパラメーターの取得方法について詳しくは、Azure [ 宛先ドキュメントページの ](/help/destinations/catalog/cloud-storage/adls-gen2.md#destination-details) 宛先の詳細の入力 [!DNL Data Lake Gen 2(ADLS Gen2)] の節を参照してください。

リクエストの例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。このコメントによって追加情報が提供されます。 リクエストを選択したターミナルにコピー&amp;ペーストする際に、リクエスト内のインラインコメントを削除します。

```shell {line-numbers="true" start-line="1" highlight="18"}
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--data-raw '{
    "name": "Azure Data Lake Gen 2(ADLS Gen2) Target Connection",
    "baseConnectionId": "<FROM_STEP_CREATE_BASE_CONNECTION>",
    "params": {
        "mode": "Server-to-server",
        "path": "folder/subfolder",
        "compression": "NONE",
        "fileType": "JSON",
        "includeFileManifest": true // Include this parameter if you want to enable manifest file generation for your destination
    },
    "connectionSpec": {
        "id": "be2c3209-53bc-47e7-ab25-145db8b873e1", // Azure Data Lake Gen 2(ADLS Gen2) connection spec id
        "version": "1.0"
    }
}'
```

+++

+++[!DNL Azure Data Lake Gen 2(ADLS Gen2)] - CSV オプションを使用したターゲット接続リクエスト

>[!TIP]
>
>ファイル書き出しに使用できる CSV オプションについて詳しくは、[ ファイル形式設定ページ ](/help/destinations/ui/batch-destinations-file-formatting-options.md) を参照してください。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--data-raw '{
   "name":"Azure Data Lake Gen 2(ADLS Gen2)",
   "baseConnectionId":"<FROM_STEP_CREATE_BASE_CONNECTION>",
   "params":{
      "mode":"Server-to-server",
      "bucketName":"your-bucket-name",
      "path":"folder/subfolder",
      "compression":"GZIP",
      "fileType":"CSV",
      "includeFileManifest": true, //Include this parameter if you want to enable manifest file generation for your destination
      "csvOptions":{
         "nullValue":"null",
         "emptyValue":"",
         "escape":"\\",
         "quote":"",
         "delimiter":","
      }
   },
   "connectionSpec":{
      "id":"be2c3209-53bc-47e7-ab25-145db8b873e1",
      "version":"1.0"
   }
}'
```

+++

**応答**

+++ターゲット接続 – 応答

```json
{
    "id": "12401496-2573-4ca7-8137-fef1aeb9dd4c",
    "etag": "\"0000d781-0000-0200-0000-63e29f420000\""
}
```

+++

>[!TAB  データランディングゾーン（DLZ） ]

**リクエスト**

+++[!DNL Data Landing Zone] - ターゲット接続リクエスト

>[!TIP]
>
>必要なターゲットパラメーターの取得方法について詳しくは、[ しい宛先のドキュメントページの ](/help/destinations/catalog/cloud-storage/data-landing-zone.md#destination-details) 宛先の詳細の入力 [!DNL Data Landing Zone] の節を参照してください。

リクエストの例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。このコメントによって追加情報が提供されます。 リクエストを選択したターミナルにコピー&amp;ペーストする際に、リクエスト内のインラインコメントを削除します。

```shell {line-numbers="true" start-line="1" highlight="18"}
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--data-raw '{
    "name": "Data Landing Zone Target Connection",
    "baseConnectionId": "<FROM_STEP_CREATE_BASE_CONNECTION>",
    "params": {
        "mode": "Server-to-server",
        "path": "folder/subfolder",
        "compression": "NONE",
        "fileType": "JSON",
        "includeFileManifest": true // Include this parameter if you want to enable manifest file generation for your destination
    },
    "connectionSpec": {
        "id": "10440537-2a7b-4583-ac39-ed38d4b848e8", // Data Landing Zone connection spec id
        "version": "1.0"
    }
}'
```

+++

+++[!DNL Data Landing Zone] - CSV オプションを使用したターゲット接続リクエスト

>[!TIP]
>
>ファイル書き出しに使用できる CSV オプションについて詳しくは、[ ファイル形式設定ページ ](/help/destinations/ui/batch-destinations-file-formatting-options.md) を参照してください。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--data-raw '{
   "name":"Data Landing Zone Target Connection",
   "baseConnectionId":"<FROM_STEP_CREATE_BASE_CONNECTION>",
   "params":{
      "mode":"Server-to-server",
      "bucketName":"your-bucket-name",
      "path":"folder/subfolder",
      "compression":"GZIP",
      "fileType":"CSV",
      "includeFileManifest": true, //Include this parameter if you want to enable manifest file generation for your destination
      "csvOptions":{
         "nullValue":"null",
         "emptyValue":"",
         "escape":"\\",
         "quote":"",
         "delimiter":","
      }
   },
   "connectionSpec":{
      "id":"10440537-2a7b-4583-ac39-ed38d4b848e8",
      "version":"1.0"
   }
}'
```

+++

**応答**

+++ターゲット接続 – 応答

```json
{
    "id": "12401496-2573-4ca7-8137-fef1aeb9dd4c",
    "etag": "\"0000d781-0000-0200-0000-63e29f420000\""
}
```

+++

>[!TAB Google Cloud Storage]

**リクエスト**

+++[!DNL Google Cloud Storage] - ターゲット接続リクエスト

>[!TIP]
>
>必要なターゲットパラメーターの取得方法について詳しくは、[ しい宛先のドキュメントページの ](/help/destinations/catalog/cloud-storage/google-cloud-storage.md#destination-details) 宛先の詳細の入力 [!DNL Google Cloud Storage] の節を参照してください。

リクエストの例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。このコメントによって追加情報が提供されます。 リクエストを選択したターミナルにコピー&amp;ペーストする際に、リクエスト内のインラインコメントを削除します。

```shell {line-numbers="true" start-line="1" highlight="19"}
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--data-raw '{
    "name": "Google Cloud Storage Target Connection",
    "baseConnectionId": "<FROM_STEP_CREATE_BASE_CONNECTION>",
    "params": {
        "mode": "Server-to-server",
        "bucketName": "your-bucket-name",
        "path": "folder/subfolder",
        "compression": "NONE",
        "fileType": "JSON",
        "includeFileManifest": true // Include this parameter if you want to enable manifest file generation for your destination
    },
    "connectionSpec": {
        "id": "c5d93acb-ea8b-4b14-8f53-02138444ae99", // Google Cloud Storage connection spec id
        "version": "1.0"
    }
}'
```

+++

+++[!DNL Google Cloud Storage] - CSV オプションを使用したターゲット接続リクエスト

>[!TIP]
>
>ファイル書き出しに使用できる CSV オプションについて詳しくは、[ ファイル形式設定ページ ](/help/destinations/ui/batch-destinations-file-formatting-options.md) を参照してください。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--data-raw '{
   "name":"Google Cloud Storage Target Connection",
   "baseConnectionId":"<FROM_STEP_CREATE_BASE_CONNECTION>",
   "params":{
      "mode":"Server-to-server",
      "bucketName":"your-bucket-name",
      "path":"folder/subfolder",
      "compression":"GZIP",
      "fileType":"CSV",
      "includeFileManifest": true, //Include this parameter if you want to enable manifest file generation for your destination
      "csvOptions":{
         "nullValue":"null",
         "emptyValue":"",
         "escape":"\\",
         "quote":"",
         "delimiter":","
      }
   },
   "connectionSpec":{
      "id":"c5d93acb-ea8b-4b14-8f53-02138444ae99",
      "version":"1.0"
   }
}'
```

+++

**応答**

+++ターゲット接続 – 応答

```json
{
    "id": "12401496-2573-4ca7-8137-fef1aeb9dd4c",
    "etag": "\"0000d781-0000-0200-0000-63e29f420000\""
}
```

+++

>[!TAB SFTP]

**リクエスト**

+++SFTP - ターゲット接続リクエスト

>[!TIP]
>
>必要なターゲットパラメーターの取得方法について詳しくは、SFTP 宛先ドキュメントページの [ 宛先の詳細の入力 ](/help/destinations/catalog/cloud-storage/google-cloud-storage.md#destination-details) の節を参照してください。

リクエストの例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。このコメントによって追加情報が提供されます。 リクエストを選択したターミナルにコピー&amp;ペーストする際に、リクエスト内のインラインコメントを削除します。

```shell {line-numbers="true" start-line="1" highlight="18"}
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--data-raw '{
    "name": "SFTP Target Connection",
    "baseConnectionId": "<FROM_STEP_CREATE_BASE_CONNECTION>",
    "params": {
        "mode": "Server-to-server",
        "remotePath": "folder/subfolder",
        "compression": "NONE",
        "fileType": "JSON",
        "includeFileManifest": true // Include this parameter if you want to enable manifest file generation for your destination
    },
    "connectionSpec": {
        "id": "36965a81-b1c6-401b-99f8-22508f1e6a26", // SFTP connection spec id
        "version": "1.0"
    }
}'
```

+++

+++SFTP - CSV オプションを使用したターゲット接続リクエスト

>[!TIP]
>
>ファイル書き出しに使用できる CSV オプションについて詳しくは、[ ファイル形式設定ページ ](/help/destinations/ui/batch-destinations-file-formatting-options.md) を参照してください。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--data-raw '{
   "name":"SFTP Target Connection",
   "baseConnectionId":"<FROM_STEP_CREATE_BASE_CONNECTION>",
   "params":{
      "mode":"Server-to-server",
      "bucketName":"your-bucket-name",
      "path":"folder/subfolder",
      "compression":"GZIP",
      "fileType":"CSV",
      "includeFileManifest": true, //Include this parameter if you want to enable manifest file generation for your destination
      "csvOptions":{
         "nullValue":"null",
         "emptyValue":"",
         "escape":"\\",
         "quote":"",
         "delimiter":","
      }
   },
   "connectionSpec":{
      "id":"36965a81-b1c6-401b-99f8-22508f1e6a26",
      "version":"1.0"
   }
}'
```

+++

**応答**

+++ターゲット接続 – 応答

```json
{
    "id": "12401496-2573-4ca7-8137-fef1aeb9dd4c",
    "etag": "\"0000d781-0000-0200-0000-63e29f420000\""
}
```

+++

>[!ENDTABS]

応答からの `target connection ID` に注意してください。 この ID は、次の手順で、オーディエンスを書き出すデータフローを作成する際に必要になります。

リクエストが成功した場合は、新しくターゲットにしたソース接続の ID （`id`）と `etag` が返されます。 後でデータフローを作成する際に必要になるので、ターゲット接続 ID をメモしておきます。

## データフローの作成 {#create-dataflow}

![ ユーザーがオンになっている現在の手順をハイライト表示するオーディエンスをアクティブ化する手順 ](/help/destinations/assets/api/file-based-segment-export/step5.png)

宛先設定の次の手順は、データフローを作成することです。 [ データフロー ](https://developer.adobe.com/experience-platform-apis/references/destinations/#tag/Glossary) は、以前に作成したエンティティを結び付け、オーディエンスの書き出しスケジュールを設定するためのオプションも提供します。 データフローを作成するには、目的のクラウドストレージ宛先に応じて以下のペイロードを使用し、前の手順で取得したフローエンティティ ID を置き換えます。 この手順では、属性または ID マッピングに関連する情報をデータフローに追加しません。 それは次のステップに続きます。

>[!BEGINTABS]

>[!TAB Amazon S3]

**リクエスト**

+++宛先へのオーディエンス書き出しデータフロー [!DNL Amazon S3] 作成 – リクエスト

リクエストの例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。このコメントによって追加情報が提供されます。 リクエストを選択したターミナルにコピー&amp;ペーストする際に、リクエスト内のインラインコメントを削除します。

```shell {line-numbers="true" start-line="1" highlight="12,22-25"}
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/flows' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--data-raw '{
    "name": "Activate audiences to an Amazon S3 cloud storage destination",
    "description": "This operation creates a dataflow to export audiences to an Amazon S3 cloud storage destination",
    "flowSpec": {
        "id": "1a0514a6-33d4-4c7f-aff8-594799c47549", // Amazon S3 flow spec ID
        "version": "1.0"
    },
    "sourceConnectionIds": [
        "<FROM_STEP_CREATE_SOURCE_CONNECTION>"
    ],
    "targetConnectionIds": [
        "<FROM_STEP_CREATE_TARGET_CONNECTION>"
    ],
    "transformations": []
}'
```

+++

**応答**

+++データフローの作成 – 応答

```json
{
    "id": "eb54b3b3-3949-4f12-89c8-64eafaba858f",
    "etag": "\"0000d781-0000-0200-0000-63e29f420000\""
}
```

+++

>[!TAB Azure Blob Storage]

**リクエスト**

+++宛先へのオーディエンス書き出しデータフロー [!DNL Azure Blob Storage] 作成 – リクエスト

リクエストの例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。このコメントによって追加情報が提供されます。 リクエストを選択したターミナルにコピー&amp;ペーストする際に、リクエスト内のインラインコメントを削除します。

```shell {line-numbers="true" start-line="1" highlight="12,22-25"}
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/flows' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--data-raw '{
    "name": "Activate audiences to an Azure Blob Storage cloud storage destination",
    "description": "This operation creates a dataflow to export audiences to an Azure Blob Storage cloud storage destination",
    "flowSpec": {
        "id": "752d422f-b16f-4f0d-b1c6-26e448e3b388", // Azure Blob Storage flow spec ID
        "version": "1.0"
    },
    "sourceConnectionIds": [
        "<FROM_STEP_CREATE_SOURCE_CONNECTION>"
    ],
    "targetConnectionIds": [
        "<FROM_STEP_CREATE_TARGET_CONNECTION>"
    ],
    "transformations": [
        {
        "name": "GeneralTransform",
        "params": {
            "mandatoryFields": [],
            "primaryFields": [],
            "profileMapping": {},
            "segmentSelectors": {
            "selectors": []
            }
        }
        }
    ]
}'
```

+++

**応答**

+++データフローの作成 – 応答

```json
{
    "id": "eb54b3b3-3949-4f12-89c8-64eafaba858f",
    "etag": "\"0000d781-0000-0200-0000-63e29f420000\""
}
```

+++

>[!TAB Azure Data Lake Gen 2 （ADLS Gen2） ]

**リクエスト**

+++宛先へのオーディエンス書き出しデータフロー [!DNL Azure Data Lake Gen 2(ADLS Gen2)] 作成 – リクエスト

リクエストの例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。このコメントによって追加情報が提供されます。 リクエストを選択したターミナルにコピー&amp;ペーストする際に、リクエスト内のインラインコメントを削除します。

```shell {line-numbers="true" start-line="1" highlight="12,22-25"}
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/flows' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--data-raw '{
    "name": "Activate audiences to an Azure Data Lake Gen 2(ADLS Gen2) cloud storage destination",
    "description": "This operation creates a dataflow to export audiences to an Azure Data Lake Gen 2(ADLS Gen2) cloud storage destination",
    "flowSpec": {
        "id": "17be2013-2549-41ce-96e7-a70363bec293", // Azure Data Lake Gen 2(ADLS Gen2) flow spec ID
        "version": "1.0"
    },
    "sourceConnectionIds": [
        "<FROM_STEP_CREATE_SOURCE_CONNECTION>"
    ],
    "targetConnectionIds": [
        "<FROM_STEP_CREATE_TARGET_CONNECTION>"
    ],
    "transformations": []
}'
```

+++

**応答**

+++データフローの作成 – 応答

```json
{
    "id": "eb54b3b3-3949-4f12-89c8-64eafaba858f",
    "etag": "\"0000d781-0000-0200-0000-63e29f420000\""
}
```

+++

>[!TAB  データランディングゾーン（DLZ） ]

**リクエスト**

+++宛先へのオーディエンス書き出しデータフロー [!DNL Data Landing Zone] 作成 – リクエスト

リクエストの例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。このコメントによって追加情報が提供されます。 リクエストを選択したターミナルにコピー&amp;ペーストする際に、リクエスト内のインラインコメントを削除します。

```shell {line-numbers="true" start-line="1" highlight="12,22-25"}
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/flows' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--data-raw '{
    "name": "Activate audiences to a Data Landing Zone cloud storage destination",
    "description": "This operation creates a dataflow to export audiences to a Data Landing Zone cloud storage destination",
    "flowSpec": {
        "id": "cd2fc47e-e838-4f38-a581-8fff2f99b63a", // Data Landing Zone flow spec ID
        "version": "1.0"
    },
    "sourceConnectionIds": [
        "<FROM_STEP_CREATE_SOURCE_CONNECTION>"
    ],
    "targetConnectionIds": [
        "<FROM_STEP_CREATE_TARGET_CONNECTION>"
    ],
    "transformations": []
}'
```

+++

**応答**

+++データフローの作成 – 応答

```json
{
    "id": "eb54b3b3-3949-4f12-89c8-64eafaba858f",
    "etag": "\"0000d781-0000-0200-0000-63e29f420000\""
}
```

+++

>[!TAB Google Cloud Storage]

**リクエスト**

+++宛先へのオーディエンス書き出しデータフロー [!DNL Google Cloud Storage] 作成 – リクエスト

リクエストの例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。このコメントによって追加情報が提供されます。 リクエストを選択したターミナルにコピー&amp;ペーストする際に、リクエスト内のインラインコメントを削除します。

```shell {line-numbers="true" start-line="1" highlight="12,22-25"}
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/flows' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--data-raw '{
    "name": "Activate audiences to a Google Cloud Storage cloud storage destination",
    "description": "This operation creates a dataflow to export audiences to a Google Cloud Storage destination",
    "flowSpec": {
        "id": "585c15c4-6cbf-4126-8f87-e26bff78b657", // Google Cloud Storage flow spec ID
        "version": "1.0"
    },
    "sourceConnectionIds": [
        "<FROM_STEP_CREATE_SOURCE_CONNECTION>"
    ],
    "targetConnectionIds": [
        "<FROM_STEP_CREATE_TARGET_CONNECTION>"
    ],
    "transformations": []
}'
```

+++

**応答**

+++データフローの作成 – 応答

```json
{
    "id": "eb54b3b3-3949-4f12-89c8-64eafaba858f",
    "etag": "\"0000d781-0000-0200-0000-63e29f420000\""
}
```

+++

>[!TAB SFTP]

**リクエスト**

+++SFTP 宛先へのオーディエンス書き出しデータフローの作成 – リクエスト

リクエストの例では、ハイライト表示された行にインラインコメントが付いていることに注意してください。このコメントによって追加情報が提供されます。 リクエストを選択したターミナルにコピー&amp;ペーストする際に、リクエスト内のインラインコメントを削除します。

```shell {line-numbers="true" start-line="1" highlight="12,22-25"}
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/flows' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--data-raw '{
    "name": "Activate audiences to an SFTP cloud storage destination",
    "description": "This operation creates a dataflow to export audiences to an SFTP cloud storage destination",
    "flowSpec": {
        "id": "fd36aaa4-bf2b-43fb-9387-43785eeeb799", // SFTP flow spec ID
        "version": "1.0"
    },
    "sourceConnectionIds": [
        "<FROM_STEP_CREATE_SOURCE_CONNECTION>"
    ],
    "targetConnectionIds": [
        "<FROM_STEP_CREATE_TARGET_CONNECTION>"
    ],
    "transformations": []
}'
```

+++

**応答**

+++データフローの作成 – 応答

```json
{
    "id": "eb54b3b3-3949-4f12-89c8-64eafaba858f",
    "etag": "\"0000d781-0000-0200-0000-63e29f420000\""
}
```

+++

>[!ENDTABS]

応答のデータフロー ID をメモします。 この ID は、後の手順で必要になります。

### 書き出しにオーディエンスを追加

この手順では、宛先に書き出すオーディエンスを選択することもできます。 この手順と、オーディエンスをデータフローに追加するリクエスト形式に関する詳細な情報については、API リファレンスドキュメントの [ 宛先データフローの更新 ](https://developer.adobe.com/experience-platform-apis/references/destinations/#tag/Dataflows/operation/patchFlowById) の節の例を参照してください。


## 属性および ID マッピングの設定 {#attribute-and-identity-mapping}

![ ユーザーがオンになっている現在の手順をハイライト表示するオーディエンスをアクティブ化する手順 ](/help/destinations/assets/api/file-based-segment-export/step6.png)

データフローを作成したら、書き出す属性と ID のマッピングを設定する必要があります。 これは、次の 3 つの手順で構成されます。

1. 入力スキーマの作成
2. 出力スキーマの作成
3. マッピングセットの設定による作成済みスキーマの接続

例えば、UI に表示される次のマッピングを取得するには、上記の 3 つの手順を実行し、次の見出しで詳しく説明する必要があります。

![ マッピングステップの例 ](/help/destinations/assets/api/file-based-segment-export/mapping-example.png)

### 入力スキーマの作成

入力スキーマを作成するには、まず [ 和集合スキーマ ](/help/profile/ui/union-schema.md) と、宛先に書き出すことができる ID を取得する必要があります。 これは、ソースマッピングとして選択できる属性および ID のスキーマです。

![ ソースフィールドを選択ビューでの属性および ID オプションの記録中 ](/help/destinations/assets/api/file-based-segment-export/select-source-field.gif)

属性と ID を取得するためのリクエストと応答の例を以下に示します。

>[!BEGINSHADEBOX]

**属性の取得リクエスト**

+++和集合スキーマから使用可能な属性を取得 – リクエスト

```shell
curl --location --request GET 'https://platform.adobe.io/data/core/ups/config/entityTypes/_xdm.context.profile?property=fullSchema==true&property=includeRelationshipDescriptors==true' \ 
--header 'x-gw-ims-org-id: {ORG_ID}' \ 
--header 'x-api-key: {API_KEY}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \ 
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
```

+++

**応答**

+++結合スキーマからの使用可能な属性の取得 – 応答

以下の応答は、簡潔にするために短縮されました。

```json
       "person": {
            "title": "Person",
            "description": "An individual actor, contact, or owner.",
            "meta:referencedFrom": [
                "https://ns.adobe.com/xdm/context/person"
            ],
            "type": "object",
            "meta:xdmType": "object",
            "properties": {
                "birthDate": {
                    "meta:xdmType": "date",
                    "type": "string",
                    "format": "date",
                    "title": "Birth date(YYYY-MM-DD)",
                    "description": "The full date a person was born."
                },
                "birthDayAndMonth": {
                    "type": "string",
                    "meta:xdmType": "string",
                    "title": "Birth date (MM-DD)",
                    "description": "The day and month a person was born, in the format MM-DD. This field should be used when the day and month of a person's birth is known, but not the year."
                },
                "birthYear": {
                    "meta:xdmType": "short",
                    "type": "integer",
                    "minimum": 1,
                    "maximum": 32767,
                    "title": "Birth year",
                    "description": "The year a person was born including the century, for example, 1983.  This field should be used when only the person's age is known, not the full birth date."
                },
                "gender": {
                    "type": "string",
                    "meta:xdmType": "string",
                    "title": "Gender",
                    "description": "Gender identity of the person.\n",
                    "enum": [
                        "male",
                        "female",
                        "not_specified",
                        "non_specific"
                    ],
                    "meta:enum": {
                        "male": "Male",
                        "female": "Female",
                        "not_specified": "Not Specified",
                        "non_specific": "Non-specific"
                    },
                    "default": "not_specified"
                },
                "maritalStatus": {
                    "type": "string",
                    "meta:xdmType": "string",
                    "title": "Marital Status",
                    "description": "Describes a person's relationship with a significant other.",
                    "enum": [
                        "married",
                        "single",
                        "divorced",
                        "widowed",
                        "not_specified"
                    ],
                    "meta:enum": {
                        "divorced": "Divorced",
                        "not_specified": "Not Specified",
                        "married": "Married",
                        "single": "Single",
                        "widowed": "Widowed"
                    },
                    "default": "not_specified"
                },
                "name": {
                    "title": "Full name",
                    "description": "The person's full name.",
                    "meta:referencedFrom": [
                        "https://ns.adobe.com/xdm/context/person-name"
                    ],
                    "type": "object",
                    "meta:xdmType": "object",
                    "properties": {
                        "courtesyTitle": {
                            "type": "string",
                            "meta:xdmType": "string",
                            "title": "Courtesy title",
                            "description": "Normally an abbreviation of a persons title, honorific, or salutation. The `courtesyTitle` is used in front of full or last name in opening texts. For example, Mr. Miss. or Dr."
                        },
                        "firstName": {
                            "type": "string",
                            "meta:xdmType": "string",
                            "title": "First name",
                            "description": "The first audience of the name in the writing order most commonly accepted in the language of the name. In many cultures this is the preferred personal or given name. The `firstName` and `lastName` properties have been introduced to maintain compatibility with existing systems that model names in a simplified, non-semantic, and non-internationalizable way. Using `xdm:fullName` is always preferable."
                        },
                        "fullName": {
                            "type": "string",
                            "meta:xdmType": "string",
                            "title": "Full name",
                            "description": "The full name of the person, in writing order most commonly accepted in the language of the name."
                        },
                        "lastName": {
                            "type": "string",
                            "meta:xdmType": "string",
                            "title": "Last name",
                            "description": "The last audience of the name in the writing order most commonly accepted in the language of the name. In many cultures this is the inherited family name, surname, patronymic, or matronymic name. The `firstName` and `lastName` properties have been introduced to maintain compatibility with existing systems that model names in a simplified, non-semantic, and non-internationalizable way. Using `xdm:fullName` is always preferable."
                        },
                        "middleName": {
                            "type": "string",
                            "meta:xdmType": "string",
                            "title": "Middle name",
                            "description": "Middle, alternative, or additional names supplied between the first name and last name."
                        },
                        "suffix": {
                            "type": "string",
                            "meta:xdmType": "string",
                            "title": "Suffix",
                            "description": "A group of letters provided after a person's name to provide additional information. The `suffix` is used at the end of someones name. For example Jr., Sr., M.D., PhD, I, II, III, etc."
                        }
                    }
                },
                "nationality": {
                    "type": "string",
                    "meta:xdmType": "string",
                    "title": "Nationality",
                    "description": "The legal relationship between a person and their state represented using the ISO 3166-1 Alpha-2 code."
                },
                "taxId": {
                    "type": "string",
                    "meta:xdmType": "string",
                    "title": "Tax ID",
                    "description": "The Tax / Fiscal ID of the person, e.g. the TIN in the US or the CIF/NIF in Spain.",
                    "meta:status": "deprecated"
                },
                "type": {
                    "type": "string",
                    "meta:xdmType": "string",
                    "title": "Type",
                    "description": "The type of individual in different business contexts like B2C."
                }
            }
        }
```

+++

>[!ENDSHADEBOX]



>[!BEGINSHADEBOX]

**ID の取得リクエスト**

+++マッピングステップで使用できる使用可能な ID の取得

```shell
curl --location --request GET 'https://platform.adobe.io/data/core/idnamespace/identities' \ 
--header 'x-gw-ims-org-id: {ORG_ID}' \ 
--header 'x-api-key: {API_KEY}' \ 
--header 'x-sandbox-name: {SANDBOX_NAME}' \ 
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
```

+++

**応答**

+++ 入力スキーマで使用する使用可能な ID の表示

応答は、入力スキーマを作成する際に使用できる ID を返します。 この応答は、Experience Platformで設定した [ 標準 ](/help/identity-service/features/namespaces.md#standard)ID 名前空間と [ カスタム ](/help/identity-service/features/namespaces.md#manage-namespaces) ID 名前空間の両方を返すことに注意してください。

```json
[
    {
        "updateTime": 1551688425455,
        "code": "CORE",
        "status": "ACTIVE",
        "description": "Adobe Audience Manger UUID",
        "id": 0,
        "createTime": 1551688425455,
        "idType": "COOKIE",
        "namespaceType": "Standard",
        "name": "CORE",
        "custom": false
    },
    {
        "updateTime": 1551688425455,
        "code": "ECID",
        "status": "ACTIVE",
        "description": "Adobe Experience Cloud ID",
        "id": 4,
        "createTime": 1551688425455,
        "idType": "COOKIE",
        "namespaceType": "Standard",
        "name": "ECID",
        "custom": false
    },
    {
        "updateTime": 1551688425455,
        "code": "AdCloud",
        "status": "ACTIVE",
        "description": "Adobe AdCloud - ID Syncing Partner",
        "id": 411,
        "createTime": 1551688425455,
        "idType": "COOKIE",
        "namespaceType": "Standard",
        "name": "AdCloud",
        "custom": false
    },
    {
        "updateTime": 1551688425455,
        "code": "Email_LC_SHA256",
        "status": "ACTIVE",
        "description": "Email addresses need to be hashed using SHA256 and lowercased. Please also note that leading and trailing spaces need to be trimmed before an email address is normalized. You won't be able to change this setting later",
        "id": 11,
        "createTime": 1551688425455,
        "idType": "Email",
        "namespaceType": "Standard",
        "name": "Emails (SHA256, lowercased)",
        "custom": false,
        "hashFunction": "SHA256",
        "transform": "lowercase"
    },
    {
        "updateTime": 1597996026101,
        "code": "Phone_E.164",
        "status": "ACTIVE",
        "description": "Namespace for raw phone numbers in E.164 format. + sign is required",
        "id": 17,
        "createTime": 1597996026101,
        "idType": "Phone",
        "namespaceType": "Standard",
        "name": "Phone (E.164)",
        "custom": false
    },
    {
        "updateTime": 1551688425455,
        "code": "GAID",
        "status": "ACTIVE",
        "description": "This datasource is associated to a Google Ad ID",
        "id": 20914,
        "createTime": 1551688425455,
        "idType": "DEVICE",
        "namespaceType": "Standard",
        "name": "Google Ad ID (GAID)",
        "custom": false
    },
    {
        "updateTime": 1476993749000,
        "code": "IDFA",
        "status": "ACTIVE",
        "description": "Apple ID for Advertisers. See: https://support.apple.com/en-us/HT202074 for more info.",
        "id": 20915,
        "createTime": 1476993749000,
        "idType": "DEVICE",
        "namespaceType": "Standard",
        "name": "Apple IDFA (ID for Advertisers)",
        "custom": false
    },
    {
        "updateTime": 1551688425455,
        "code": "AAID",
        "status": "ACTIVE",
        "description": "Adobe Analytics (Legacy ID)",
        "id": 10,
        "createTime": 1551688425455,
        "idType": "COOKIE",
        "namespaceType": "Standard",
        "name": "Adobe Analytics (Legacy ID)",
        "custom": false
    },
    {
        "updateTime": 1551688425455,
        "code": "Email",
        "status": "ACTIVE",
        "description": "Email",
        "id": 6,
        "createTime": 1551688425455,
        "idType": "Email",
        "namespaceType": "Standard",
        "name": "Email",
        "custom": false
    },
    {
        "updateTime": 1551688425455,
        "code": "WAID",
        "status": "ACTIVE",
        "description": "Windows AID",
        "id": 8,
        "createTime": 1551688425455,
        "idType": "DEVICE",
        "namespaceType": "Standard",
        "name": "Windows AID",
        "custom": false
    },
    {
        "updateTime": 1551688425455,
        "code": "TNTID",
        "status": "ACTIVE",
        "description": "Adobe Target (TNTID)",
        "id": 9,
        "createTime": 1551688425455,
        "idType": "COOKIE",
        "namespaceType": "Standard",
        "name": "TNTID",
        "custom": false
    },
    {
        "updateTime": 1556676464714,
        "code": "Google",
        "status": "ACTIVE",
        "id": 771,
        "createTime": 1556676464714,
        "idType": "COOKIE",
        "namespaceType": "Integration",
        "name": "Google",
        "custom": false
    },
    {
        "updateTime": 1604597776019,
        "code": "Phone_SHA256_E.164",
        "status": "ACTIVE",
        "description": "Phone numbers need to be hashed using SHA256 without any dashes. Hash should be completed by customers on raw phone numbers in E.164 format. Please note that some destinations may have different phone number formatting requirements. Refer to documentation or consult your Adobe representative",
        "id": 18,
        "createTime": 1604597776019,
        "idType": "Phone",
        "namespaceType": "Standard",
        "name": "Phone (SHA256_E.164)",
        "custom": false,
        "hashFunction": "SHA256"
    },
    {
        "updateTime": 1604597776019,
        "code": "Phone_SHA256",
        "status": "ACTIVE",
        "description": "Remove symbols, letters, and any leading zeroes before hashing. Prefix the country code before hashing. Please note that some destinations may have different phone number formatting requirements. Refer to documentation or consult your Adobe representative",
        "id": 15,
        "createTime": 1597995542594,
        "idType": "Phone",
        "namespaceType": "Standard",
        "name": "Phone (SHA256)",
        "custom": false,
        "hashFunction": "SHA256"
    },
    {
        "updateTime": 1551688425455,
        "code": "Phone",
        "status": "ACTIVE",
        "description": "Phone",
        "id": 7,
        "createTime": 1551688425455,
        "idType": "PHONE_NUMBER",
        "namespaceType": "Standard",
        "name": "Phone",
        "custom": false
    }
]
```

+++

>[!ENDSHADEBOX]

次に、上記の応答をコピーし、それを使用して入力スキーマを作成する必要があります。 上記の応答から JSON 応答全体をコピーして、以下に示す `jsonSchema` オブジェクトに配置できます。

>[!BEGINSHADEBOX]

**入力スキーマの作成リクエスト**

+++入力スキーマの作成 – リクエスト

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/conversion/schemas/' \ 
--header 'x-gw-ims-org-id: {ORG_ID}' \ 
--header 'x-api-key: {API_KEY}' \ 
--header 'x-sandbox-name: {SANDBOX_NAME}' \ 
--header 'Authorization: Bearer {ACCESS_TOKEN}' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{
    "name":"Sample Schema for Flow Mapping",
    "jsonSchema":{...insert response from union schema response here}
    '
```

+++

**応答**

+++入力スキーマの作成 – 応答

```json
{
   "id":"8db56468c2ab475dbf17c2621f92c0f8",
   "version":0,
   "jsonSchema":{
      "title":"XDM Individual Profile",
      "description":"An XDM Individual Profile forms a singular representation of the attributes and interests of both identified and partially-identified individuals. Less-identified profiles may contain only anonymous behavioral signals, such as browser cookies, while highly-identified profiles may contain detailed personal information such as name, date of birth, location, and email address. As a profile grows, it becomes a robust repository of personal information, identification information, contact details, and communication preferences for an individual.",
      "type":"object",
      "properties":{ // this section returns the contents that you have added to the jsonSchema object in the request
      }
   }
}
```

+++

>[!ENDSHADEBOX]

応答の ID は、作成した入力スキーマの一意の ID を表します。 後の手順で再利用できるように、応答から ID をコピーします。

### 出力スキーマの作成

次に、書き出しの出力スキーマを設定する必要があります。 まず、既存のパートナースキーマを見つけて検査する必要があります。

>[!BEGINSHADEBOX]

**リクエスト**

+++出力スキーマのパートナースキーマを取得するリクエスト

以下の例では、Amazon S3 に `connection spec ID` を使用しています。 この値を、宛先に固有の接続仕様 ID に置き換えてください。

```shell
curl --location --request GET 'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs/4fce964d-3f37-408f-9778-e597338a21ee' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
```

+++

**スキーマの例を使用した応答**

上記の呼び出しを実行する際に取得した応答を調べます。 応答を詳しく調べて、オブジェクト `targetSpec.attributes.partnerSchema.jsonSchema` を見つける必要があります

+++ 出力スキーマのパートナースキーマを取得するための応答

```json
{
   "title":"defaultschema",
   "type":"object",
   "properties":{
      "attributes":{
         "type":"object",
         "meta:xdmType":"map",
         "additionalProperties":{
            "type":"object",
            "properties":{
               "value":{
                  "type":"string",
                  "title":"Value"
               }
            },
            "meta:xdmType":"object"
         }
      },
      "identityMap":{
         "type":"object",
         "meta:xdmField":"xdm:identityMap",
         "meta:xdmType":"map",
         "additionalProperties":{
            "type":"array",
            "items":{
               "type":"object",
               "properties":{
                  "id":{
                     "type":"string",
                     "title":"Identifier",
                     "description":"Identity of the consumer in the related namespace.",
                     "meta:xdmType":"string",
                     "meta:xdmField":"xdm:id"
                  },
                  "primary":{
                     "type":"boolean",
                     "title":"Primary",
                     "default":false,
                     "description":"Indicates this identity is the preferred identity. Is used as a hint to help systems better organize how identities are queried.",
                     "meta:xdmType":"boolean",
                     "meta:xdmField":"xdm:primary"
                  },
                  "authenticatedState":{
                     "enum":[
                        "ambiguous",
                        "authenticated",
                        "loggedOut"
                     ],
                     "type":"string",
                     "default":"ambiguous",
                     "meta:enum":{
                        "ambiguous":"Ambiguous",
                        "loggedOut":"User was identified by a login action at some point of time previously, but is not currently logged in.",
                        "authenticated":"User identified by a login or similar action that was valid at the time of the event observation."
                     },
                     "description":"The state this identity is authenticated as for this observed ExperienceEvent.",
                     "meta:xdmType":"string",
                     "meta:xdmField":"xdm:authenticatedState"
                  }
               },
               "meta:xdmType":"object",
               "meta:referencedFrom":"https://ns.adobe.com/xdm/context/identityitem"
            },
            "meta:xdmType":"array"
         }
      },
      "segmentMembership":{
         "title":"Segment membership map",
         "type":"object",
         "meta:xdmField":"xdm:segmentMembership",
         "meta:xdmType":"map",
         "additionalProperties":{
            "type":"object",
            "title":"Segment membership per namespace",
            "meta:xdmType":"map",
            "additionalProperties":{
               "type":"object",
               "properties":{
                  "status":{
                     "enum":[
                        "realized",
                        "exited"
                     ],
                     "type":"string",
                     "title":"Status",
                     "default":"realized",
                     "meta:enum":{
                        "exited":"Entity is exiting the segment.",
                        "realized":"Entity is entering the segment."
                     },
                     "description":"Is the audience participation realized as part of the current request.",
                     "meta:xdmType":"string",
                     "meta:xdmField":"xdm:status"
                  },
                  "payload":{
                     "type":"object",
                     "title":"Payload",
                     "required":[
                        "payloadType"
                     ],
                     "properties":{
                        "payloadType":{
                           "enum":[
                              "boolean",
                              "number",
                              "propensity",
                              "string"
                           ],
                           "type":"string",
                           "title":"Payload Type",
                           "meta:enum":{
                              "number":"Number",
                              "string":"String",
                              "boolean":"Boolean",
                              "propensity":"Propensity"
                           },
                           "description":"The type of payload.",
                           "meta:xdmType":"string",
                           "meta:xdmField":"xdm:payloadType"
                        },
                        "payloadNumberValue":{
                           "type":"number",
                           "title":"Value",
                           "description":"The number.",
                           "meta:xdmType":"number",
                           "meta:xdmField":"xdm:payloadNumberValue"
                        },
                        "payloadStringValue":{
                           "type":"string",
                           "title":"Value",
                           "description":"The string value.",
                           "meta:xdmType":"string",
                           "meta:xdmField":"xdm:payloadStringValue"
                        },
                        "payloadBooleanValue":{
                           "type":"boolean",
                           "title":"Value",
                           "description":"The boolean value.",
                           "meta:xdmType":"boolean",
                           "meta:xdmField":"xdm:payloadBooleanValue"
                        },
                        "payloadPropensityValue":{
                           "type":"number",
                           "title":"Value",
                           "maximum":1,
                           "description":"The propensity.",
                           "meta:xdmType":"number",
                           "meta:xdmField":"xdm:payloadPropensityValue",
                           "exclusiveMinimum":0
                        }
                     },
                     "description":"Values that are directly related with the audience realization. This payload exists with the same 'validUntil' as the audience realization. Note that the intention is that exactly one payload value be included, as indicated by the payload type. This was originally modeled using 'oneOf', but due to limitations in our tooling that was removed. This more semantically meaningful representation will be re-introduced in the future.",
                     "meta:xdmType":"object",
                     "meta:xdmField":"xdm:payload"
                  },
                  "version":{
                     "type":"string",
                     "title":"Version",
                     "description":"The version of the audience definition used in this audience assertion. Version can be omitted in audience lists when all memberships versions are the same.",
                     "meta:xdmType":"string",
                     "meta:xdmField":"xdm:version"
                  },
                  "segmentID":{
                     "type":"object",
                     "title":"Segment ID",
                     "properties":{
                        "_id":{
                           "type":"string",
                           "title":"Identifier",
                           "format":"uri-reference",
                           "description":"Identity of the audience in the related namespace.",
                           "meta:xdmType":"string",
                           "meta:xdmField":"@id"
                        },
                        "xid":{
                           "type":"string",
                           "title":"Experience identifier",
                           "description":"When present, this value represents a cross-namespace identifier that is unique across all namespace-scoped identifiers in all namespaces.",
                           "meta:xdmType":"string",
                           "meta:xdmField":"xdm:xid"
                        },
                        "namespace":{
                           "type":"object",
                           "title":"Namespace",
                           "required":[
                              "code"
                           ],
                           "properties":{
                              "code":{
                                 "type":"string",
                                 "title":"Code",
                                 "description":"The code is a human readable identifier for the namespace and can be used to request the technical namespace id which is used for identity graph processing.",
                                 "meta:xdmType":"string",
                                 "meta:xdmField":"xdm:code"
                              }
                           },
                           "description":"The namespace associated with the `xid` attribute.",
                           "meta:xdmType":"object",
                           "meta:xdmField":"xdm:namespace",
                           "meta:referencedFrom":"https://ns.adobe.com/xdm/context/namespace"
                        }
                     },
                     "description":"The identity of the audience or snapshot definition in with the domain of the specific system that processes that type of segment. Deprecated.",
                     "meta:status":"deprecated",
                     "meta:xdmType":"object",
                     "meta:xdmField":"xdm:segmentID",
                     "meta:referencedFrom":"https://ns.adobe.com/xdm/context/segmentidentity"
                  },
                  "validUntil":{
                     "type":"string",
                     "title":"Valid until",
                     "format":"date-time",
                     "description":"The timestamp for when the audienceassertion should no longer be assumed to be valid and should either be ignored or revalidated.",
                     "meta:xdmType":"date-time",
                     "meta:xdmField":"xdm:validUntil"
                  },
                  "profileStitchID":{
                     "type":"object",
                     "properties":{
                        "_id":{
                           "type":"string",
                           "title":"Identifier",
                           "format":"uri-reference",
                           "description":"Identity of the profile stitch in the related namespace.",
                           "meta:xdmType":"string",
                           "meta:xdmField":"@id"
                        },
                        "xid":{
                           "type":"string",
                           "title":"Experience identifier",
                           "description":"When present, this value represents a cross-namespace identifier that is unique across all namespace-scoped identifiers in all namespaces.",
                           "meta:xdmType":"string",
                           "meta:xdmField":"xdm:xid"
                        },
                        "namespace":{
                           "type":"object",
                           "title":"Namespace",
                           "required":[
                              "code"
                           ],
                           "properties":{
                              "code":{
                                 "type":"string",
                                 "title":"Code",
                                 "description":"The code is a human readable identifier for the namespace and can be used to request the technical namespace id which is used for identity graph processing.",
                                 "meta:xdmType":"string",
                                 "meta:xdmField":"xdm:code"
                              }
                           },
                           "description":"The namespace associated with the `xid` attribute.",
                           "meta:xdmType":"object",
                           "meta:xdmField":"xdm:namespace",
                           "meta:referencedFrom":"https://ns.adobe.com/xdm/context/namespace"
                        }
                     },
                     "meta:xdmType":"object",
                     "meta:xdmField":"xdm:profileStitchID",
                     "meta:referencedFrom":"https://ns.adobe.com/xdm/context/profileStitchIdentity"
                  },
                  "lastQualificationTime":{
                     "type":"string",
                     "title":"Last qualification time",
                     "format":"date-time",
                     "description":"The timestamp when the assertion of audience membership was made.",
                     "meta:xdmType":"date-time",
                     "meta:xdmField":"xdm:lastQualificationTime"
                  }
               },
               "meta:xdmType":"object",
               "meta:referencedFrom":"https://ns.adobe.com/xdm/context/segmentmembership"
            }
         }
      }
   }
}
```

+++

>[!ENDSHADEBOX]

次に、出力スキーマを作成します。 上記で取得した JSON 応答をコピーし、以下の `jsonSchema` オブジェクトに貼り付けます。

>[!BEGINSHADEBOX]

**リクエスト**

+++出力スキーマの作成 – リクエスト

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/conversion/schemas' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Authorization: Bearer TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "Sample Schema for Flow Mapping",
"jsonSchema":{...insert JSON from the response above here}
}
'
```

+++

**応答**

+++出力スキーマの作成 – 応答

```json
{
    "id": "2f4fd51934c1409fb1d8207dd9f43dc9",
    "version": 0,
    "jsonSchema": {
        "title": "defaultschema",
        "type": "object",
        "properties": {
            "identityMap": {
                "type": "object",
                "meta:xdmField": "xdm:identityMap",
                "meta:xdmType": "map",
                "additionalProperties": {
                    "meta:xdmType": "array",
                    "items": {
                        "meta:referencedFrom": "https://ns.adobe.com/xdm/context/identityitem",
                        "properties": {
                            "primary": {
                                "meta:xdmField": "xdm:primary",
                                "meta:xdmType": "boolean",
                                "description": "Indicates this identity is the preferred identity. Is used as a hint to help systems better organize how identities are queried.",
                                "default": false,
                                "type": "boolean",
                                "title": "Primary"
                            },
                            "id": {
                                "meta:xdmField": "xdm:id",
                                "meta:xdmType": "string",
                                "description": "Identity of the consumer in the related namespace.",
                                "type": "string",
                                "title": "Identifier"
                            },
                            "authenticatedState": {
                                "meta:xdmField": "xdm:authenticatedState",
                                "meta:xdmType": "string",
                                "meta:enum": {
                                    "loggedOut": "User was identified by a login action at some point of time previously, but is not currently logged in.",
                                    "authenticated": "User identified by a login or similar action that was valid at the time of the event observation.",
                                    "ambiguous": "Ambiguous"
                                },
                                "enum": [
                                    "ambiguous",
                                    "authenticated",
                                    "loggedOut"
                                ],
                                "default": "ambiguous",
                                "type": "string",
                                "description": "The state this identity is authenticated as for this observed ExperienceEvent."
                            }
                        },
                        "meta:xdmType": "object",
                        "type": "object"
                    },
                    "type": "array"
                }
            },
            "segmentMembership": {
                "title": "Segment membership map",
                "type": "object",
                "meta:xdmField": "xdm:segmentMembership",
                "meta:xdmType": "map",
                "additionalProperties": {
                    "additionalProperties": {
                        "meta:referencedFrom": "https://ns.adobe.com/xdm/context/segmentmembership",
                        "properties": {
                            "version": {
                                "meta:xdmField": "xdm:version",
                                "meta:xdmType": "string",
                                "description": "The version of the audience definition used in this audience assertion. Version can be omitted in audience lists when all memberships versions are the same.",
                                "type": "string",
                                "title": "Version"
                            },
                            "validUntil": {
                                "meta:xdmField": "xdm:validUntil",
                                "meta:xdmType": "date-time",
                                "description": "The timestamp for when the audienceassertion should no longer be assumed to be valid and should either be ignored or revalidated.",
                                "format": "date-time",
                                "type": "string",
                                "title": "Valid until"
                            },
                            "status": {
                                "meta:xdmField": "xdm:status",
                                "meta:xdmType": "string",
                                "meta:enum": {
                                    "exited": "Entity is exiting the segment.",
                                    "realized": "Entity is entering the segment."
                                },
                                "enum": [
                                    "realized",
                                    "exited"
                                ],
                                "default": "realized",
                                "description": "Is the audience participation realized as part of the current request.",
                                "type": "string",
                                "title": "Status"
                            },
                            "segmentID": {
                                "meta:xdmField": "xdm:segmentID",
                                "meta:referencedFrom": "https://ns.adobe.com/xdm/context/segmentidentity",
                                "properties": {
                                    "xid": {
                                        "meta:xdmField": "xdm:xid",
                                        "meta:xdmType": "string",
                                        "description": "When present, this value represents a cross-namespace identifier that is unique across all namespace-scoped identifiers in all namespaces.",
                                        "type": "string",
                                        "title": "Experience identifier"
                                    },
                                    "namespace": {
                                        "meta:xdmField": "xdm:namespace",
                                        "required": [
                                            "code"
                                        ],
                                        "meta:referencedFrom": "https://ns.adobe.com/xdm/context/namespace",
                                        "properties": {
                                            "code": {
                                                "meta:xdmField": "xdm:code",
                                                "meta:xdmType": "string",
                                                "description": "The code is a human readable identifier for the namespace and can be used to request the technical namespace id which is used for identity graph processing.",
                                                "type": "string",
                                                "title": "Code"
                                            }
                                        },
                                        "meta:xdmType": "object",
                                        "type": "object",
                                        "description": "The namespace associated with the `xid` attribute.",
                                        "title": "Namespace"
                                    },
                                    "_id": {
                                        "meta:xdmField": "@id",
                                        "meta:xdmType": "string",
                                        "description": "Identity of the audience in the related namespace.",
                                        "format": "uri-reference",
                                        "type": "string",
                                        "title": "Identifier"
                                    }
                                },
                                "meta:xdmType": "object",
                                "type": "object",
                                "description": "The identity of the audience or snapshot definition in with the domain of the specific system that processes that type of segment. Deprecated.",
                                "meta:status": "deprecated",
                                "title": "Segment ID"
                            },
                            "profileStitchID": {
                                "meta:xdmField": "xdm:profileStitchID",
                                "meta:referencedFrom": "https://ns.adobe.com/xdm/context/profileStitchIdentity",
                                "properties": {
                                    "xid": {
                                        "meta:xdmField": "xdm:xid",
                                        "meta:xdmType": "string",
                                        "description": "When present, this value represents a cross-namespace identifier that is unique across all namespace-scoped identifiers in all namespaces.",
                                        "type": "string",
                                        "title": "Experience identifier"
                                    },
                                    "namespace": {
                                        "meta:xdmField": "xdm:namespace",
                                        "required": [
                                            "code"
                                        ],
                                        "meta:referencedFrom": "https://ns.adobe.com/xdm/context/namespace",
                                        "properties": {
                                            "code": {
                                                "meta:xdmField": "xdm:code",
                                                "meta:xdmType": "string",
                                                "description": "The code is a human readable identifier for the namespace and can be used to request the technical namespace id which is used for identity graph processing.",
                                                "type": "string",
                                                "title": "Code"
                                            }
                                        },
                                        "meta:xdmType": "object",
                                        "type": "object",
                                        "description": "The namespace associated with the `xid` attribute.",
                                        "title": "Namespace"
                                    },
                                    "_id": {
                                        "meta:xdmField": "@id",
                                        "meta:xdmType": "string",
                                        "description": "Identity of the profile stitch in the related namespace.",
                                        "format": "uri-reference",
                                        "type": "string",
                                        "title": "Identifier"
                                    }
                                },
                                "meta:xdmType": "object",
                                "type": "object"
                            },
                            "payload": {
                                "meta:xdmField": "xdm:payload",
                                "meta:xdmType": "object",
                                "required": [
                                    "payloadType"
                                ],
                                "properties": {
                                    "payloadType": {
                                        "meta:xdmField": "xdm:payloadType",
                                        "meta:xdmType": "string",
                                        "description": "The type of payload.",
                                        "meta:enum": {
                                            "string": "String",
                                            "propensity": "Propensity",
                                            "number": "Number",
                                            "boolean": "Boolean"
                                        },
                                        "enum": [
                                            "boolean",
                                            "number",
                                            "propensity",
                                            "string"
                                        ],
                                        "type": "string",
                                        "title": "Payload Type"
                                    },
                                    "payloadStringValue": {
                                        "meta:xdmField": "xdm:payloadStringValue",
                                        "meta:xdmType": "string",
                                        "description": "The string value.",
                                        "type": "string",
                                        "title": "Value"
                                    },
                                    "payloadPropensityValue": {
                                        "meta:xdmField": "xdm:payloadPropensityValue",
                                        "meta:xdmType": "number",
                                        "maximum": 1,
                                        "exclusiveMinimum": 0,
                                        "description": "The propensity.",
                                        "type": "number",
                                        "title": "Value"
                                    },
                                    "payloadNumberValue": {
                                        "meta:xdmField": "xdm:payloadNumberValue",
                                        "meta:xdmType": "number",
                                        "description": "The number.",
                                        "type": "number",
                                        "title": "Value"
                                    },
                                    "payloadBooleanValue": {
                                        "meta:xdmField": "xdm:payloadBooleanValue",
                                        "meta:xdmType": "boolean",
                                        "description": "The boolean value.",
                                        "type": "boolean",
                                        "title": "Value"
                                    }
                                },
                                "type": "object",
                                "description": "Values that are directly related with the audience realization. This payload exists with the same 'validUntil' as the audience realization. Note that the intention is that exactly one payload value be included, as indicated by the payload type. This was originally modeled using 'oneOf', but due to limitations in our tooling that was removed. This more semantically meaningful representation will be re-introduced in the future.",
                                "title": "Payload"
                            },
                            "lastQualificationTime": {
                                "meta:xdmField": "xdm:lastQualificationTime",
                                "meta:xdmType": "date-time",
                                "description": "The timestamp when the assertion of audience membership was made.",
                                "format": "date-time",
                                "type": "string",
                                "title": "Last qualification time"
                            }
                        },
                        "meta:xdmType": "object",
                        "type": "object"
                    },
                    "meta:xdmType": "map",
                    "type": "object",
                    "title": "Segment membership per namespace"
                }
            },
            "attributes": {
                "type": "object",
                "meta:xdmType": "map",
                "additionalProperties": {
                    "properties": {
                        "value": {
                            "type": "string",
                            "title": "Value"
                        }
                    },
                    "meta:xdmType": "object",
                    "type": "object"
                }
            },
            "firstName": {
                "title": "firstName",
                "type": "string"
            },
            "Email": {
                "title": "Email",
                "type": "array",
                "items": {
                    "type": "object",
                    "properties": {
                        "id": {
                            "title": "id",
                            "type": "string",
                            "meta:xdmType": "string"
                        }
                    },
                    "meta:xdmType": "object"
                },
                "meta:xdmType": "array"
            }
        }
    }
}
```

応答の ID は、作成した入力スキーマの一意の ID を表します。 後の手順で再利用できるように、応答から ID をコピーします。

>[!ENDSHADEBOX]

### マッピングセットを作成 {#create-mapping-set}

次に、[data prep API](https://developer.adobe.com/experience-platform-apis/references/data-prep/#tag/Mapping-sets/operation/createMappingSet) を使用して、入力スキーマ ID、出力スキーマ ID および目的のフィールドマッピングを使用してマッピングセットを作成します。

>[!BEGINSHADEBOX]

**リクエスト**

+++マッピングセットを作成 – リクエスト

>[!IMPORTANT]
>
>* 以下に示すマッピングオブジェクトでは、`destination` パラメーターはドット `"."` を受け付けません。 例えば、設定例でハイライト表示されているように、personalEmail_address または segmentMembership_status を使用する必要があります。
>* ソース属性が ID 属性で、ドットが含まれている場合は、特に異なるケースがあります。 この場合、以下に示すように、属性は `//` でエスケープする必要があります。
>* また、以下の設定例に `Email` と `Phone_E.164` が含まれている場合でも、データフローごとに 1 つの ID 属性のみを書き出すことができます。

```shell {line-numbers="true" start-line="1" highlight="16-38"}
curl --location --request POST 'https://platform.adobe.io/data/foundation/conversion/mappingSets' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Authorization: Bearer TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "inputSchema": {
        "id": "81d7fc5376e54eb58d5186fd30d5a8c9"
    },  
    "outputSchema": {
        "id": "2f4fd51934c1409fb1d8207dd9f43dc9"
    },
    "mappings": [
        {
            "destination": "firstName",
            "source": "person.name.firstName",
            "sourceType": "ATTRIBUTE"
        }, 
        {
            "destination": "Email",
            "source": "identityMap.Email",
            "sourceType": "ATTRIBUTE"
        },
        {
            "destination": "Phone_E_164",
            "source": "identityMap.Phone_E//.164",
            "sourceType": "ATTRIBUTE"
        },
        {
            "destination": "personalEmail_address",
            "source": "personalEmail.address",
            "sourceType": "ATTRIBUTE"
        },        
        {
            "destination": "segmentMembership_status",
            "source": "segmentMembership.ups.seg_id.status",
            "sourceType": "ATTRIBUTE"
        }        
    ],
    "xdmVersion": "1.0"
}'
```

+++

**応答**

+++ マッピングセットの作成 – 応答

```json
{
    "id": "5f0031f8ccd4444dac9c5c391389e9e8",
    "version": 0,
    "createdDate": 1678999005197,
    "modifiedDate": 1678999005197,
    "createdBy": "example@AdobeID",
    "modifiedBy": "example@AdobeID"
}
```

+++

>[!ENDSHADEBOX]

マッピングセットの ID をメモします。次の手順で、既存のデータフローをマッピングセット ID で更新する際に必要になります。

次に、更新するデータフローの ID を取得します。

>[!BEGINSHADEBOX]

データフローの ID の取得について詳しくは、[ 宛先データフローの詳細の取得 ](https://developer.adobe.com/experience-platform-apis/references/destinations/#tag/Dataflows/operation/getFlowById) を参照してください。

>[!ENDSHADEBOX]

最後に、作成したマッピングセット情報を使用してデータフローを `PATCH` 成する必要があります。

>[!BEGINSHADEBOX]

**リクエスト**

+++ マッピングセット情報を使用したデータフローの更新 – リクエスト

```shell
curl --location --request PATCH 'https://platform.adobe.io/data/foundation/flowservice/flows/df8245a8-dd8f-42da-a04a-2d3a92654d9e' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'If-Match: ETAG_HERE' \
--data-raw '[
   {
      "op":"ADD",
      "path":"/transformations/0/params/profileMapping",
      "value":{
         "mappingId":"304ca6a2fff244949c956cad1cd9eae6",
         "mappingVersion":0
      }
   }
]'
```

+++

**応答**

+++ マッピングセット情報を使用したデータフローの更新 – 応答

Flow Service API からの応答は、更新されたデータフローの ID を返します。

```json
{
    "id": "1c246ae4-ba0d-43cd-a0ec-f16fe0a56b55",
    "etag": "\"1500c67f-0000-0200-0000-64137ee00000\""
}
```

+++

>[!ENDSHADEBOX]

## 他のデータフロー更新を行う {#other-dataflow-updates}

![ ユーザーがオンになっている現在の手順をハイライト表示するオーディエンスをアクティブ化する手順 ](/help/destinations/assets/api/file-based-segment-export/step7.png)

データフローを更新するには、`PATCH` 操作を使用します。 例えば、データフローにマーケティングアクションを追加したり、データフローを更新して必須キーまたは重複排除キーとしてフィールドを選択したり、既存の宛先にファイルマニフェストの生成を追加したりできます。

### マーケティングアクションの追加 {#add-marketing-action}

[ マーケティングアクション ](/help/data-governance/api/marketing-actions.md) を追加するには、以下のリクエストと応答の例を参照してください。

>[!IMPORTANT]
>
>`If-Match` リクエストを行う場合、`PATCH` ヘッダーは必須です。 このヘッダーの値は、更新するデータフローの一意のバージョンです。 etag の値は、データフロー、ターゲット接続などのフローエンティティが正常に更新されるたびに更新されます。
>
> etag 値の最新バージョンを取得するには、`https://platform.adobe.io/data/foundation/flowservice/flows/{ID}` エンドポイントに対してGET リクエストを実行します。`{ID}` は、更新するデータフロー ID です。
>
> `If-Match` リクエストを行う場合は、以下の例のように、`PATCH` ヘッダーの値を必ず二重引用符で囲みます。

>[!BEGINSHADEBOX]

**リクエスト**

>[!TIP]
>
>マーケティングアクションをデータフローに追加する前に、既存のコアマーケティングアクションとカスタムマーケティングアクションを検索できます。 表示 [ 既存のマーケティングアクションのリストを取得する方法 ](/help/data-governance/api/marketing-actions.md#list)。

+++宛先データフローへのマーケティングアクションの追加 – リクエスト

```shell
curl --location --request PATCH 'https://platform.adobe.io/data/foundation/flowservice/flows/{DATAFLOW_ID}' \
--header 'accept: application/json' \
--header 'Content-Type: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'If-Match: "{ETAG_HERE}"' \
--data-raw '[
   {
      "op":"add",
      "path":"/policy",
      "value":{
         "enforcementRefs":[
            
         ]
      }
   },
   {
      "op":"add",
      "path":"/policy/enforcementRefs/-",
      "value":"/dulepolicy/marketingActions/custom/6b935bc8-bb9e-451b-a327-0ffddfb91e66/constraints"
   }
]'
```

+++


**応答**

+++マーケティングアクションの追加 – 応答

応答が成功すると、応答コード `200` と、更新されたデータフローの ID および更新された eTag が返されます。

```json
{
    "id": "eb54b3b3-3949-4f12-89c8-64eafaba858f",
    "etag": "\"0000d781-0000-0200-0000-63e29f420000\""
}
```

+++

>[!ENDSHADEBOX]

### 必須キーを追加 {#add-mandatory-key}

[ 必須キー ](/help/destinations/ui/activate-batch-profile-destinations.md#mandatory-attributes) を追加するには、以下のリクエストと応答の例を参照してください。

>[!IMPORTANT]
>
>`If-Match` リクエストを行う場合、`PATCH` ヘッダーは必須です。 このヘッダーの値は、更新するデータフローの一意のバージョンです。 etag の値は、データフロー、ターゲット接続などのフローエンティティが正常に更新されるたびに更新されます。
>
> etag 値の最新バージョンを取得するには、`https://platform.adobe.io/data/foundation/flowservice/flows/{ID}` エンドポイントに対してGET リクエストを実行します。`{ID}` は、更新するデータフロー ID です。
>
> `If-Match` リクエストを行う場合は、以下の例のように、`PATCH` ヘッダーの値を必ず二重引用符で囲みます。

>[!BEGINSHADEBOX]

**リクエスト**

+++ID を必須フィールドとして追加 – リクエスト

```shell
curl --location --request PATCH 'https://platform.adobe.io/data/foundation/flowservice/flows/{DATAFLOW_ID}' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'If-Match: "{ETAG_HERE}"' \
--data-raw '
[
  {
    "op": "add",
    "path": "/transformations/0/params/mandatoryFields",
    "value": [
      "GAID"
    ]
  }
]'
```

+++

+++XDM 属性を必須フィールドとして追加 – リクエスト

```shell
curl --location --request PATCH 'https://platform.adobe.io/data/foundation/flowservice/flows/{DATAFLOW_ID}' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'If-Match: "{ETAG_HERE}"' \
--data-raw '
[
  {
    "op": "add",
    "path": "/transformations/0/params/mandatoryFields",
    "value": [
      "GAID"
    ]
  }
]'
```

+++

**応答**

+++必須フィールドの追加 – 応答

```json
{
    "id": "eb54b3b3-3949-4f12-89c8-64eafaba858f",
    "etag": "\"0000d781-0000-0200-0000-63e29f420000\""
}
```

+++

>[!ENDSHADEBOX]

### 重複排除キーを追加 {#add-deduplication-key}

[ 重複排除キー ](/help/destinations/ui/activate-batch-profile-destinations.md#deduplication-keys) を追加するには、以下のリクエストと応答の例を参照してください

>[!IMPORTANT]
>
>`If-Match` リクエストを行う場合、`PATCH` ヘッダーは必須です。 このヘッダーの値は、更新するデータフローの一意のバージョンです。 etag の値は、データフロー、ターゲット接続などのフローエンティティが正常に更新されるたびに更新されます。
>
> etag 値の最新バージョンを取得するには、`https://platform.adobe.io/data/foundation/flowservice/flows/{ID}` エンドポイントに対してGET リクエストを実行します。`{ID}` は、更新するデータフロー ID です。
>
> `If-Match` リクエストを行う場合は、以下の例のように、`PATCH` ヘッダーの値を必ず二重引用符で囲みます。

>[!BEGINSHADEBOX]

**リクエスト**

+++ID を重複排除キーとして追加 – リクエスト

```shell
curl --location --request PATCH 'https://platform.adobe.io/data/foundation/flowservice/flows/{DATAFLOW_ID}' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'If-Match: "{ETAG_HERE}"' \
--data-raw '
[
  {
    "op": "add",
    "path": "/transformations/0/params/primaryFields",
    "value": [
      {
        "identityNamespace": "GAID",
        "fieldType": "IDENTITY"
      }
    ]
  }
]'
```

+++

+++重複排除キーとして XDM 属性を追加 – リクエスト

```shell
curl --location --request PATCH 'https://platform.adobe.io/data/foundation/flowservice/flows/{DATAFLOW_ID}' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'If-Match: "{ETAG_HERE}"' \
--data-raw '
[
  {
    "op": "add",
    "path": "/transformations/0/params/primaryFields",
    "value": [
      {
        "identityNamespace": "GAID",
        "fieldType": "IDENTITY"
      }
    ]
  }
]'
```

+++

**応答**

+++必須フィールドの追加 – 応答

```json
{
    "id": "eb54b3b3-3949-4f12-89c8-64eafaba858f",
    "etag": "\"0000d781-0000-0200-0000-63e29f420000\""
}
```

+++

>[!ENDSHADEBOX]

### 既存の宛先へのファイルマニフェスト生成の追加 {#add-file-manifest}

既存の宛先にファイルマニフェストの生成を追加するには、`PATCH` 操作を使用してターゲット接続パラメーターを更新する必要があります。 これにより、宛先のマニフェストファイルの生成が可能になり、書き出されたファイルに関するメタデータが提供されます。

>[!IMPORTANT]
>
>`If-Match` リクエストを行う場合、`PATCH` ヘッダーは必須です。 このヘッダーの値は、更新するターゲット接続の一意のバージョンです。 etag の値は、データフロー、ターゲット接続などのフローエンティティが正常に更新されるたびに更新されます。
>
> etag 値の最新バージョンを取得するには、`https://platform.adobe.io/data/foundation/flowservice/targetConnections/{ID}` エンドポイントに対してGET リクエストを実行します。`{ID}` は、更新するターゲット接続 ID です。
>
> `If-Match` リクエストを行う場合は、以下の例のように、`PATCH` ヘッダーの値を必ず二重引用符で囲みます。

>[!BEGINSHADEBOX]

**リクエスト**

+++既存のターゲット接続へのファイルマニフェストの追加 – リクエスト

```shell
curl --location --request PATCH 'https://platform.adobe.io/data/foundation/flowservice/targetConnections/{TARGET_CONNECTION_ID}' \
--header 'accept: application/json' \
--header 'Content-Type: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'If-Match: "{ETAG_HERE}"' \
--data-raw '[
  {
    "op": "add",
    "path": "/params/includeFileManifest",
    "value": true
  }
]'
```

>[!ENDSHADEBOX]

## データフローの検証（データフローの実行を取得） {#get-dataflow-runs}

![ ユーザーがオンになっている現在の手順をハイライト表示するオーディエンスをアクティブ化する手順 ](/help/destinations/assets/api/file-based-segment-export/step8.png)

データフローの実行を確認するには、Dataflow Runs API を使用します。

>[!BEGINSHADEBOX]

**リクエスト**

+++データフロー実行の取得 – リクエスト

```shell
curl --location --request GET 'https://platform.adobe.io/data/foundation/flowservice/runs?property=flowId==eb54b3b3-3949-4f12-89c8-64eafaba858f' \
--header 'accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--data-raw ''
```

+++

**応答**

+++データフロー実行の取得 – 応答

```json
{
    "items": [
        {
            "id": "4b7728dd-83c9-4c38-95a4-24ddab545404",
            "createdAt": 1675807718296,
            "updatedAt": 1675807731834,
            "createdBy": "aep_activation_batch@AdobeID",
            "updatedBy": "acp_foundation_connectors@AdobeID",
            "createdClient": "aep_activation_batch",
            "updatedClient": "acp_foundation_connectors",
            "sandboxId": "7dfdcd30-0a09-11ea-8ea6-7bf93ce86c28",
            "sandboxName": "sand-1",
            "imsOrgId": "5555467B5D8013E50A494220@AdobeOrg",
            "flowId": "aae5ec63-b0ac-4808-9a44-abf2ea67bd5a",
            "flowSpec": {
                "id": "615d3489-36d2-4671-9467-4ae1129facd3",
                "version": "1.0"
            },
            "providerRefId": "ba56f98e0c49b572adb249980c39b1c7",
            "etag": "\"08005e9e-0000-0200-0000-63e2cbf30000\"",
            "metrics": {
                "durationSummary": {
                    "startedAtUTC": 1675807719411,
                    "completedAtUTC": 1675807719416
                },
                "sizeSummary": {
                    "inputBytes": 0
                },
                "recordSummary": {
                    "inputRecordCount": 0,
                    "skippedRecordCount": 0,
                    "sourceSummaries": [
                        {
                            "id": "ea2b1205-4692-49de-b448-ebf75b1d188a",
                            "inputRecordCount": 0,
                            "skippedRecordCount": 0,
                            "entitySummaries": [
                                {
//...
```

+++

>[!ENDSHADEBOX]

[ データフロー実行 API によって返される様々なパラメーター ](https://developer.adobe.com/experience-platform-apis/references/destinations/#tag/Dataflow-runs/operation/getFlowRuns) に関する情報については、API リファレンスドキュメントを参照してください。

## API エラー処理 {#api-error-handling}

このチュートリアルの API エンドポイントは、Experience Platform API の一般的なエラーメッセージの原則に従っています。 エラー応答の解釈について詳しくは、Experience Platform トラブルシューティングガイドの [API ステータスコード ](/help/landing/troubleshooting.md#api-status-codes) および [ リクエストヘッダーエラー ](/help/landing/troubleshooting.md#request-header-errors) を参照してください。

## 次の手順 {#next-steps}

このチュートリアルでは、Experience Platformを目的のクラウドストレージ宛先の 1 つに正常に接続し、オーディエンスを書き出すための各宛先へのデータフローを設定しました。 次のページでは、Flow Service API を使用した既存のデータフローの編集方法などの詳細を確認します。

* [宛先の概要](../home.md)
* [宛先カタログの概要](../catalog/overview.md)
* [Flow Service API を使用した宛先データフローの更新](../api/update-destination-dataflows.md)
