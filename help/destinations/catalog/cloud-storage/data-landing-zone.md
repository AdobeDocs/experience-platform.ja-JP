---
title: データランディングゾーンの宛先
description: データランディングゾーンに接続してオーディエンスをアクティブ化し、データセットを書き出す方法を説明します。
last-substantial-update: 2023-07-26T00:00:00Z
exl-id: 40b20faa-cce6-41de-81a0-5f15e6c00e64
source-git-commit: 4eef1804d6974fd54f5e74e0efe62257190f408b
workflow-type: tm+mt
source-wordcount: '2019'
ht-degree: 32%

---

# データランディングゾーンの宛先

>[!IMPORTANT]
>
>このドキュメントページでは、[!DNL Data Landing Zone]*宛先* について説明しています。 また、ソースカタログにも [!DNL Data Landing Zone]*source* があります。 詳しくは、[[!DNL Data Landing Zone] source](/help/sources/connectors/cloud-storage/data-landing-zone.md) ドキュメントを参照してください。


## 概要 {#overview}

[!DNL Data Landing Zone] は、Adobe Experience Platformによってプロビジョニングされたクラウドストレージインターフェイスです。安全なクラウドベースのファイルストレージ機能にアクセスして、ファイルをExperience Platformから書き出すことができます。 サンドボックスごとに 1 つの [!DNL Data Landing Zone] コンテナに対するアクセス権があります。すべてのコンテナの合計データ量は、Experience Platform製品およびサービスライセンスで提供される合計データ量に制限されます。 Experience Platformとそのアプリケーション（[!DNL Customer Journey Analytics]、[!DNL Journey Orchestration]、[!DNL Intelligent Services]、[!DNL Real-Time Customer Data Platform] など）のすべてのユーザーは、サンドボックスごとに 1 つの [!DNL Data Landing Zone] コンテナを使用してプロビジョニングされます。

Experience Platformでは、[!DNL Data Landing Zone] コンテナにアップロードされるすべてのファイルで厳密に 7 日間の有効期間（TTL）が適用されます。 すべてのファイルは 7 日後に削除されます。

[!DNL Data Landing Zone] 宛先コネクタは、Azure またはAmazon Web サービスクラウドサポートを使用するお客様が利用できます。 認証メカニズムは、宛先がプロビジョニングされるクラウドによって異なります。宛先に関するその他の要素とユースケースはすべて同じです。 2 つの異なる認証メカニズムの詳細については、[Azure Blob にプロビジョニングされたデータランディングゾーンに対する認証 ](#authenticate-dlz-azure) および [AWSがプロビジョニングされたデータランディングゾーンに対する認証 ](#authenticate-dlz-aws) の節を参照してください。

![ データランディングゾーン宛先の実装がクラウドのサポートに基づいてどのように異なるかを示す図。](/help/destinations/assets/catalog/cloud-storage/data-landing-zone/dlz-workflow-based-on-cloud-implementation.png " クラウドサポートによるデータランディングゾーンの宛先実装 "){zoomable="yes"}

## API または UI を介して [!UICONTROL  データランディングゾーン ] ストレージに接続 {#connect-api-or-ui}

* Experience Platform ユーザーインターフェイスを使用して [!UICONTROL  データランディングゾーン ] ストレージの場所に接続するには、以下の [ 宛先への接続 ](#connect) および [ この宛先に対するオーディエンスのアクティブ化 ](#activate) の節を参照してください。
* [!UICONTROL  データランディングゾーン ] ストレージの場所にプログラムで接続するには、[Flow Service API チュートリアルを使用したファイルベースの宛先に対するオーディエンスのアクティブ化 ](../../api/activate-segments-file-based-destinations.md) を参照してください。

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | [宛先のアクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性の選択画面で選択したように、該当するスキーマフィールド（例：PPID）と共に、セグメントのすべてのメンバーを書き出しています。 |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## データセットの書き出し {#export-datasets}

この宛先では、データセットの書き出しをサポートしています。 データセットの書き出しを設定する方法について詳しくは、次のチュートリアルを参照してください。

* [Experience Platform ユーザーインターフェイスを使用したデータセットの書き出し ](/help/destinations/ui/export-datasets.md) 方法。
* [Flow Service API を使用してプログラムでデータセットを書き出す ](/help/destinations/api/export-datasets.md) 方法。

## 書き出されたデータのファイル形式 {#file-format}

*オーディエンスデータ* を書き出すと、Experience Platformは、指定されたストレージの場所に `.csv`、`parquet` または `.json` ファイルを作成します。 ファイルについて詳しくは、Audience Activation チュートリアルの [ 書き出しでサポートされるファイル形式 ](../../ui/activate-batch-profile-destinations.md#supported-file-formats-export) の節を参照してください。

*データセット* を書き出すと、Experience Platformは、指定されたストレージの場所に `.parquet` または `.json` ファイルを保存します。 ファイルについて詳しくは、データセットの書き出しチュートリアルの [ データセットの書き出しが成功したことを確認する ](../../ui/export-datasets.md#verify) の節を参照してください。

## Azure Blob にプロビジョニングされたデータランディングゾーンに対する認証 {#authenticate-dlz-azure}

>[!AVAILABILITY]
>
>この節の内容は、Microsoft Azure 上で動作するExperience Platformの実装に適用されます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](https://experienceleague.adobe.com/en/docs/experience-platform/landing/multi-cloud) を参照してください。

[!DNL Azure Storage Explorer] またはコマンドラインインターフェイスを通じて、コンテナに対してファイルの読み取りと書き込みを行うことができます。

[!DNL Data Landing Zone] は SAS ベースの認証をサポートし、そのデータは保存時および転送中は標準 [!DNL Azure Blob] ストレージセキュリティメカニズムで保護されます。SAS は [ 共有アクセス署名 ](https://learn.microsoft.com/en-us/azure/ai-services/translator/document-translation/how-to-guides/create-sas-tokens?tabs=Containers) を表します。

パブリックインターネット接続を介してデータを保護するには、SAS ベースの認証を使用して [!DNL Data Landing Zone] コンテナに安全にアクセスします。 ユーザーが [!DNL Data Landing Zone] コンテナにアクセスする場合、ネットワークの変更は必要ありません。つまり、ネットワークに対して許可リストの設定や地域間設定は必要ありません。

### [!DNL Data Landing Zone] コンテナの [!DNL Azure Storage Explorer] への接続

[[!DNL Azure Storage Explorer]](https://azure.microsoft.com/en-us/products/storage/storage-explorer/) を使用して、[!DNL Data Landing Zone] コンテナのコンテンツを管理できます。 [!DNL Data Landing Zone] の使用を開始するには、まず資格情報を取得し、それらを [!DNL Azure Storage Explorer] に入力して、[!DNL Data Landing Zone] コンテナを [!DNL Azure Storage Explorer] に接続する必要があります。

[!DNL Azure Storage Explorer] UI 内で、左側のナビゲーションバーの「接続」アイコンを選択します。**リソースを選択**&#x200B;ウィンドウが開き、接続するオプションが表示されます。**[!DNL Blob container]** を選択し、[!DNL Data Landing Zone] ストレージに接続します。

![Azure UI でハイライト表示されているリソースを選択 ](/help/sources/images/tutorials/create/dlz/select-resource.png)

次に、接続方法として「**共有アクセス署名 URL (SAS)**」を選択し、「**次へ**」をクリックします。

![Azure UI でハイライト表示されている接続方法を選択します。](/help/sources/images/tutorials/create/dlz/select-connection-method.png)

接続方法を選択した後、**表示名**&#x200B;およびお使いの [!DNL Data Landing Zone] コンテナに対応する&#x200B;**[!DNL Blob]コンテナ SAS URL** を入力します。

>[!BEGINSHADEBOX]

### [!DNL Data Landing Zone] ーザーの資格情報の取得 {#retrieve-dlz-credentials}

[!DNL Data Landing Zone] 資格情報を取得するには、Experience Platform API を使用する必要があります。 資格情報を取得するための API 呼び出しは、以下に説明されています。 ヘッダーに必要な値の取得について詳しくは、[Adobe Experience Platform API の概要 ](/help/landing/api-guide.md) ガイドを参照してください。

**API 形式**

```http
GET /data/foundation/connectors/landingzone/credentials?type=dlz_destination
```

| クエリパラメーター | 説明 |
| --- | --- |
| `dlz_destination` | `dlz_destination` タイプを使用すると、API は、ランディングゾーンの宛先コンテナを、使用可能な他のタイプのコンテナと区別できます。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエスト例では、既存のランディングゾーンの資格情報を取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/connectors/landingzone/credentials?type=dlz_destination' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
```

**応答**

次の応答は、現在の `SASToken` と `SASUri`、ランディングゾーンコンテナに対応する `storageAccountName` など、ランディングゾーンの資格情報を返します。

```json
{
    "containerName": "dlz-destination",
    "SASToken": "sv=2022-09-11&si=dlz-ed86a61d-201f-4b50-b10f-a1bf173066fd&sr=c&sp=racwdlm&sig=4yTba8voU3L0wlcLAv9mZLdZ7NlMahbfYYPTMkQ6ZGU%3D",
    "storageAccountName": "dlblobstore99hh25i3df123",
    "SASUri": "https://dlblobstore99hh25i3dflek.blob.core.windows.net/dlz-destination?sv=2022-09-11&si=dlz-ed86a61d-201f-4b50-b10f-a1bf173066fd&sr=c&sp=racwdlm&sig=4yTba8voU3L0wlcLAv9mZLdZ7NlMahbfYYPTMkQ6ZGU%3D"
}
```

| プロパティ | 説明 |
| --- | --- |
| `containerName` | ランディングゾーンの名前。 |
| `SASToken` | ランディングゾーンの共有アクセス署名トークン。 この文字列には、リクエストの認証に必要なすべての情報が含まれます。 |
| `SASUri` | ランディングゾーンの共有アクセス署名 URI。 この文字列は、認証対象のランディングゾーンの URI とそれに対応する SAS トークンの組み合わせです。 |

{style="table-layout:auto"}

### 資格情報 [!DNL Data Landing Zone] 更新 {#update-dlz-credentials}

必要に応じて、資格情報を更新することもできます。 `SASToken` API の `/credentials` エンドポイントに対して POST リクエストを実行することで、[!DNL Connectors] を更新できます。

**API 形式**

```http
POST /data/foundation/connectors/landingzone/credentials?type=dlz_destination&action=refresh
```

| クエリパラメーター | 説明 |
| --- | --- |
| `dlz_destination` | `dlz_destination` タイプを使用すると、API は、ランディングゾーンの宛先コンテナを、使用可能な他のタイプのコンテナと区別できます。 |
| `refresh` | `refresh` のアクションを使用すると、ランディングゾーンの資格情報をリセットし、新しい `SASToken` を自動的に生成できます。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストは、ランディングゾーン資格情報を更新します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/connectors/landingzone/credentials?type=dlz_destination&action=refresh' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
```

**応答**

次の応答は、`SASToken` および `SASUri` の更新された値を返します。

```json
{
    "containerName": "dlz-destination",
    "SASToken": "sv=2020-04-08&si=dlz-9c4d03b8-a6ff-41be-9dcf-20123e717e99&sr=c&sp=racwdlm&sig=JbRMoDmFHQU4OWOpgrKdbZ1d%2BkvslO35%2FXTqBO%2FgbRA%3D",
    "storageAccountName": "dlblobstore99hh25i3dflek",
    "SASUri": "https://dlblobstore99hh25i3dflek.blob.core.windows.net/dlz-destination?sv=2020-04-08&si=dlz-9c4d03b8-a6ff-41be-9dcf-20123e717e99&sr=c&sp=racwdlm&sig=JbRMoDmFHQU4OWOpgrKdbZ1d%2BkvslO35%2FXTqBO%2FgbRA%3D"
}
```

>[!ENDSHADEBOX]

上記の API 呼び出しで返され `containerName` 表示名（[!DNL Data Landing Zone]）と SAS URL を入力し、「**次へ**」を選択します。

![Azure UI でハイライト表示されている接続情報を入力 ](/help/sources/images/tutorials/create/dlz/enter-connection-info.png)

**概要**&#x200B;ウィンドウが開き、[!DNL Blob] エンドポイントと権限を含む設定の概要が表示されます。準備ができたら、「**接続**」を選択します。

![Azure UI に表示される設定の概要。](/help/sources/images/tutorials/create/dlz/summary.png)

接続が成功すると、[!DNL Azure Storage Explorer] UI と [!DNL Data Landing Zone] コンテナが更新されます。

![Azure UI でハイライト表示されている DLZ ユーザーコンテナの概要。](/help/sources/images/tutorials/create/dlz/dlz-user-container.png)

[!DNL Data Landing Zone] コンテナが [!DNL Azure Storage Explorer] に接続され、Experience Platform から [!DNL Data Landing Zone] コンテナへのファイルの書き出しを開始できるようになりました。ファイルを書き出すには、以下の節で説明されているように、Experience Platform UI で [!DNL Data Landing Zone] の宛先への接続を確立する必要があります。

## AWSがプロビジョニングしたデータランディングゾーンに対する認証 {#authenticate-dlz-aws}

>[!AVAILABILITY]
>
>この節の内容は、Amazon Web Services（AWS）上で動作するExperience Platformの実装に適用されます。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](https://experienceleague.adobe.com/en/docs/experience-platform/landing/multi-cloud) を参照してください。

以下の操作を実行して、AWSでプロビジョニングされた [!DNL Data Landing Zone] インスタンスに対する資格情報を取得します。 次に、任意のクライアントを使用して [!DNL Data Landing Zone] インスタンスに接続します。

>[!BEGINSHADEBOX]

### [!DNL Data Landing Zone] ーザーの資格情報の取得 {#retrieve-dlz-credentials-aws}

[!DNL Data Landing Zone] 資格情報を取得するには、Experience Platform API を使用する必要があります。 資格情報を取得するための API 呼び出しは、以下に説明されています。 ヘッダーに必要な値の取得について詳しくは、[Adobe Experience Platform API の概要 ](/help/landing/api-guide.md) ガイドを参照してください。

**API 形式**

```http
GET /data/foundation/connectors/landingzone/credentials?type=dlz_destination'
```

| クエリパラメーター | 説明 |
| --- | --- |
| `dlz_destination` | `dlz_destination` クエリパラメーターを追加して、[!DNL Data Landing Zone] *宛先* タイプのコンテナ資格情報を取得することを指定します。 データランディングゾーン *source* の資格情報を接続して取得するには、[sources のドキュメント ](/help/sources/connectors/cloud-storage/data-landing-zone.md) を参照してください。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエスト例では、既存のランディングゾーンの資格情報を取得します。

```shell
curl --request GET \
  --url 'https://platform.adobe.io/data/foundation/connectors/landingzone/credentials?type=dlz_destination' \
  --header 'Authorization: Bearer ***' \
  --header 'Content-Type: application/json' \
  --header 'x-api-key: your_api_key' \
  --header 'x-gw-ims-org-id: yourorg@AdobeOrg'
```

**応答**

次の応答では、現在の `awsAccessKeyId`、`awsSecretAccessKey`、その他の情報を含む、ランディングゾーンの資格情報が返されます。

```json
{
    "credentials": {
        "awsAccessKeyId": "ABCDW3MEC6HE2T73ZVKP",
        "awsSecretAccessKey": "A1B2Zdxj6y4xfR0QZGtf/phj/hNMAbOGtzM/JNeE",
        "awsSessionToken": "***"
    },
    "dlzPath": {
        "bucketName": "your-bucket-name",
        "dlzFolder": "dlz-destination"
    },
    "dlzProvider": "Amazon S3",
    "expiryTime": 1734494017
}
```

| プロパティ | 説明 |
| --- | --- |
| `credentials` | このオブジェクトには、プロビジョニングされたデータランディングゾーンの場所にファイルを書き出すためにExperience Platformが使用する `awsAccessKeyId`、`awsSecretAccessKey` および `awsSessionToken` が含まれます。 |
| `dlzPath` | このオブジェクトには、書き出されたファイルが格納される、AdobeでプロビジョニングされたAWSの場所のパスが含まれます。 |
| `dlzProvider` | これがAmazon S3 でプロビジョニングされたデータランディングゾーンであることを示します。 |
| `expiryTime` | `credentials` オブジェクトの資格情報の有効期限が切れるタイミングを示します。 資格情報を更新するには、もう一度リクエストを実行します。 |

{style="table-layout:auto"}

>[!ENDSHADEBOX]

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/connect-destination.html?lang=ja)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

「[!DNL Data Landing Zone] 前提条件 [!DNL Azure Storage Explorer]」セクションの説明に従って、[ コンテナが ](#prerequisites) に接続されていることを確認します。 [!DNL Data Landing Zone] は、Adobeでプロビジョニングされたストレージであるため、Experience Platform UI で宛先への認証のためにそれ以上手順を実行する必要はありません。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 暗号化キー]**：必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。正しい形式の暗号化キーの例については、以下の画像を参照してください。
  ![UI での正しい形式の PGP キーの例を示す画像。](../../assets/catalog/cloud-storage/sftp/pgp-key.png)
* **[!UICONTROL 名前]**：この宛先に希望する名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL フォルダーパス]**：書き出したファイルをホストする宛先フォルダーへのパス。
* **[!UICONTROL ファイルの種類]**：書き出したファイルにExperience Platformで使用する形式を選択します。 [!UICONTROL CSV] オプションを選択する場合、[ ファイル形式オプションを設定 ](../../ui/batch-destinations-file-formatting-options.md) することもできます。
* **[!UICONTROL 圧縮形式]**：書き出したファイルにExperience Platformで使用する圧縮タイプを選択します。
* **[!UICONTROL マニフェストファイルを含める]**：書き出しに、書き出しの場所や書き出しのサイズなどに関する情報を含んだマニフェスト JSON ファイルを含めたい場合は、このオプションをオンに切り替えます。 マニフェストには、形式 `manifest-<<destinationId>>-<<dataflowRunId>>.json` を使用して名前を付けます。 [ サンプル マニフェスト ファイル ](/help/destinations/assets/common/manifest-d0420d72-756c-4159-9e7f-7d3e2f8b501e-0ac8f3c0-29bd-40aa-82c1-f1b7e0657b19.json) を表示します。 マニフェストファイルには、次のフィールドが含まれています。
   * `flowRunId`：書き出されたファイルを生成した [ データフロー実行 ](/help/dataflows/ui/monitor-destinations.md#dataflow-runs-for-batch-destinations)。
   * `scheduledTime`: ファイルが書き出された時間（UTC 単位）。
   * `exportResults.sinkPath`：書き出されたファイルが格納されるストレージの場所のパス。
   * `exportResults.name`：書き出すファイルの名前。
   * `size`：書き出されたファイルのサイズ（バイト単位）。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先に対してオーディエンスをアクティブ化する手順については、[ プロファイル書き出しのバッチ宛先に対するオーディエンスデータのアクティブ化 ](../../ui/activate-batch-profile-destinations.md) を参照してください。

### スケジュール設定

**[!UICONTROL スケジュール設定]**&#x200B;手順では、[!DNL Data Landing Zone] 宛先の[書き出しスケジュールを設定](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling)し、[書き出したファイルの名前を設定](/help/destinations/ui/activate-batch-profile-destinations.md#file-names)することもできます。

### 属性と ID のマッピング {#map}

**[!UICONTROL マッピング]**&#x200B;手順では、プロファイルに書き出す属性および ID フィールドを選択できます。 また、書き出したファイル内のヘッダーを選択して、任意のわかりやすい名前に変更することもできます。詳しくは、「バッチの宛先をアクティベート」UI チュートリアルの[マッピング手順](/help/destinations/ui/activate-batch-profile-destinations.md#mapping)を参照してください。

## データの正常な書き出しの検証 {#exported-data}

データが正常に書き出されたかどうかを確認するには、[!DNL Data Landing Zone] ストレージを確認し、書き出されたファイルに想定されるプロファイル母集団が含まれていることを確認してください。
