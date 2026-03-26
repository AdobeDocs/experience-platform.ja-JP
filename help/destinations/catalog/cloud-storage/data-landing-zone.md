---
title: データランディングゾーンの宛先
description: データランディングゾーンに接続してオーディエンスをアクティブ化し、データセットを書き出す方法について説明します。
last-substantial-update: 2023-07-26T00:00:00Z
exl-id: 40b20faa-cce6-41de-81a0-5f15e6c00e64
source-git-commit: 20427c4c8826905a77fac04d055d523b12a6f739
workflow-type: tm+mt
source-wordcount: '2096'
ht-degree: 24%

---

# データランディングゾーンの宛先

>[!IMPORTANT]
>
>このドキュメントページは、[!DNL Data Landing Zone] *宛先*&#x200B;を参照しています。 ソースカタログには[!DNL Data Landing Zone] *ソース*&#x200B;もあります。 詳しくは、[[!DNL Data Landing Zone] source](/help/sources/connectors/cloud-storage/data-landing-zone.md) ドキュメントを参照してください。


## 概要 {#overview}

[!DNL Data Landing Zone]は、[!DNL Adobe Experience Platform]によってプロビジョニングされたクラウドストレージインターフェイスで、安全なクラウドベースのファイルストレージ機能へのアクセス権を付与して、Experience Platformからファイルを書き出すことができます。 サンドボックスごとに1つの[!DNL Data Landing Zone] コンテナにアクセスでき、すべてのコンテナの合計データ量は、Experience Platform製品およびサービス ライセンスで提供される合計データ量に制限されます。 [!DNL Customer Journey Analytics]、[!DNL Journey Orchestration]、[!DNL Intelligent Services]、[!DNL Real-Time Customer Data Platform]などのExperience Platformとそのアプリケーションのすべてのお客様は、サンドボックスごとに1つの[!DNL Data Landing Zone] コンテナでプロビジョニングされます。

Experience Platformでは、[!DNL Data Landing Zone] コンテナにアップロードされたすべてのファイルに対して、厳密な7日間の有効期間（TTL）が適用されます。 すべてのファイルは 7 日後に削除されます。

[!DNL Data Landing Zone]宛先コネクタは、AzureまたはAmazon Web Serviceのクラウドサポートを使用しているお客様が利用できます。 認証メカニズムは、宛先がプロビジョニングされるクラウドに基づいて異なり、宛先とそのユースケースに関する他のすべてが同じです。 2つの異なる認証メカニズムについて詳しくは、「[Azure Blob](#authenticate-dlz-azure)でプロビジョニングされたデータランディングゾーンに対する認証」および「[AWSでプロビジョニングされたデータランディングゾーンに対する認証](#authenticate-dlz-aws)」の節を参照してください。

クラウド サポートに基づいて、データ ランディング ゾーンの宛先の実装がどのように異なるかを示す![図。](/help/destinations/assets/catalog/cloud-storage/data-landing-zone/dlz-workflow-based-on-cloud-implementation.png " クラウドサポートによるデータランディングゾーンの宛先実装"){zoomable="yes"}

## APIまたはUIを介して[!UICONTROL Data Landing Zone] ストレージに接続する {#connect-api-or-ui}

* Experience Platform ユーザーインターフェイスを使用して[!UICONTROL Data Landing Zone] ストレージの場所に接続するには、以下の「[宛先に接続](#connect)」と「[この宛先にオーディエンスをアクティブ化](#activate)」の節を参照してください。
* プログラムで[!UICONTROL Data Landing Zone]のストレージの場所に接続するには、[Flow Service API チュートリアルを使用してファイルベースの宛先にオーディエンスをアクティブ化する](../../api/activate-segments-file-based-destinations.md)を参照してください。

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの由来 | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ○ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md)を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの生成元 | ○ | このカテゴリには、[!DNL Segmentation Service]を通じて生成されたオーディエンス以外のすべてのオーディエンスのオリジンが含まれます。 [様々なオーディエンスの起源](/help/segmentation/ui/audience-portal.md#customize)について読みます。 次に例を示します。 <ul><li> カスタムアップロードオーディエンス [がCSV ファイルからExperience Platformに](../../../segmentation/ui/audience-portal.md#import-audience)をインポートしました。</li><li> 類似オーディエンス， </li><li> 連合オーディエンス， </li><li> [!DNL Adobe Journey Optimizer]などの他のExperience Platform アプリで生成されたオーディエンス </li><li> その他。 </li></ul> |

{style="table-layout:auto"}



オーディエンスのデータタイプ別にサポートされるオーディエンス：

| オーディエンスのデータタイプ | サポートあり | 説明 | ユースケース |
|--------------------|-----------|-------------|-----------|
| [人物オーディエンス ](/help/segmentation/types/people-audiences.md) | ○ | 顧客プロファイルにもとづいて、マーケティング施策の特定のグループをターゲットにすることができます。 | 買い物客やカートの放棄が多い |
| [ アカウントオーディエンス ](/help/segmentation/types/account-audiences.md) | ○ | アカウントベースドマーケティング戦略のために、特定の組織内の個人をターゲットにします。 | B2B マーケティング |
| [見込みオーディエンス ](/help/segmentation/types/prospect-audiences.md) | ○ | まだ顧客ではないが、ターゲットオーディエンスと特徴を共有する個人をターゲットにします。 | サードパーティデータによる見込み顧客の開拓 |
| [ データセットの書き出し](/help/catalog/datasets/overview.md) | ○ | [!DNL Adobe Experience Platform] データ レイクに保存されている構造化データのコレクション。 | レポート，データサイエンスワークフロー |

{style="table-layout:auto"}


## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しタイプと頻度については、次の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Profile-based]** | [宛先のアクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性の選択画面で選択したように、該当するスキーマフィールド（例：PPID）と共に、セグメントのすべてのメンバーを書き出しています。 |
| 書き出し頻度 | **[!UICONTROL Batch]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## データセットの書き出し {#export-datasets}

この宛先では、データセットの書き出しをサポートしています。 データセットの書き出しを設定する方法について詳しくは、チュートリアルを参照してください。

* Experience Platform ユーザーインターフェイス [を使用してデータセットを](/help/destinations/ui/export-datasets.md) エクスポートする方法。
* Flow Service APIを使用してデータセットをプログラムで[ エクスポートする方法](/help/destinations/api/export-datasets.md)。

## 書き出されたデータのファイル形式 {#file-format}

*オーディエンスデータ*&#x200B;を書き出すと、Experience Platformは、指定した保存場所に`.csv`、`parquet`、または`.json`個のファイルを作成します。 ファイルについて詳しくは、オーディエンスアクティベーションのチュートリアルの「[ サポートされている書き出し用ファイル形式](../../ui/activate-batch-profile-destinations.md#supported-file-formats-export)」セクションを参照してください。

*データセット*&#x200B;を書き出すと、Experience Platformは、指定したストレージの場所に`.parquet`または`.json`個のファイルを作成します。 ファイルについて詳しくは、データセットの書き出しチュートリアルの「[成功したデータセットの書き出しを検証する](../../ui/export-datasets.md#verify)」セクションを参照してください。

## Azure Blobでプロビジョニングされたデータランディングゾーンへの認証 {#authenticate-dlz-azure}

>[!AVAILABILITY]
>
>このセクションは、Microsoft Azureで動作するExperience Platformの実装に適用されます。 サポートされているExperience Platform インフラストラクチャについて詳しくは、[Experience Platform マルチクラウドの概要](https://experienceleague.adobe.com/en/docs/experience-platform/landing/multi-cloud)を参照してください。

[!DNL Azure Storage Explorer] またはコマンドラインインターフェイスを通じて、コンテナに対してファイルの読み取りと書き込みを行うことができます。

[!DNL Data Landing Zone] は SAS ベースの認証をサポートし、そのデータは保存時および転送中は標準 [!DNL Azure Blob] ストレージセキュリティメカニズムで保護されます。SASは[共有アクセス署名](https://learn.microsoft.com/en-us/azure/ai-services/translator/document-translation/how-to-guides/create-sas-tokens?tabs=Containers)を表します。

パブリック インターネット接続でデータを保護するには、SAS ベースの認証を使用して[!DNL Data Landing Zone] コンテナに安全にアクセスしてください。 [!DNL Data Landing Zone] コンテナにアクセスするためにネットワークの変更は必要ありません。つまり、ネットワークに対するネットワークの設定やクロス許可リストの設定を行う必要はありません。

### [!DNL Data Landing Zone] コンテナを[!DNL Azure Storage Explorer]に接続します {#connect-container-to-storage-explorer}

[[!DNL Azure Storage Explorer]](https://azure.microsoft.com/en-us/products/storage/storage-explorer/)を使用して、[!DNL Data Landing Zone] コンテナのコンテンツを管理できます。 [!DNL Data Landing Zone]の使用を開始するには、まず資格情報を取得し、[!DNL Azure Storage Explorer]に入力し、[!DNL Data Landing Zone] コンテナを[!DNL Azure Storage Explorer]に接続する必要があります。

[!DNL Azure Storage Explorer] UI 内で、左側のナビゲーションバーの「接続」アイコンを選択します。**リソースを選択**&#x200B;ウィンドウが開き、接続するオプションが表示されます。**[!DNL Blob container]** を選択し、[!DNL Data Landing Zone] ストレージに接続します。

![Azure UIでハイライト表示されているリソースを選択します。](/help/sources/images/tutorials/create/dlz/select-resource.png)

次に、接続方法として「**共有アクセス署名 URL (SAS)**」を選択し、「**次へ**」をクリックします。

![Azure UIでハイライト表示されている接続方式を選択します。](/help/sources/images/tutorials/create/dlz/select-connection-method.png)

接続方法を選択した後、**表示名**&#x200B;およびお使いの [!DNL Data Landing Zone] コンテナに対応する&#x200B;**[!DNL Blob]コンテナ SAS URL** を入力します。

>[!BEGINSHADEBOX]

### [!DNL Data Landing Zone]の資格情報を取得 {#retrieve-dlz-credentials}

Experience Platform APIを使用して、[!DNL Data Landing Zone]資格情報を取得する必要があります。 資格情報を取得するためのAPI呼び出しについて以下に説明します。 ヘッダーに必要な値の取得について詳しくは、[Adobe Experience Platform APIの概要](/help/landing/api-guide.md) ガイドを参照してください。

**API 形式**

```http
GET /data/foundation/connectors/landingzone/credentials?type=dlz_destination
```

| クエリパラメーター | 説明 |
| --- | --- |
| `dlz_destination` | `dlz_destination` タイプを使用すると、APIはランディングゾーンの宛先コンテナを、使用可能な他のタイプのコンテナと区別できます。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストの例では、既存のランディングゾーンの資格情報を取得します。

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

次の応答は、現在の`SASToken`と`SASUri`を含むランディングゾーンの資格情報と、ランディングゾーンコンテナに対応する`storageAccountName`を返します。

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
| `SASToken` | ランディングゾーンの共有アクセス署名トークン。 この文字列には、リクエストの承認に必要なすべての情報が含まれます。 |
| `SASUri` | ランディングゾーンの共有アクセス署名URI。 この文字列は、認証するランディングゾーンへのURIと、それに対応するSAS トークンとの組み合わせです。 |

{style="table-layout:auto"}

### [!DNL Data Landing Zone]資格情報を更新 {#update-dlz-credentials}

必要に応じて資格情報を更新することもできます。 `SASToken` APIの`/credentials` エンドポイントにPOST リクエストを行うことで、[!DNL Connectors]を更新できます。

**API 形式**

```http
POST /data/foundation/connectors/landingzone/credentials?type=dlz_destination&action=refresh
```

| クエリパラメーター | 説明 |
| --- | --- |
| `dlz_destination` | `dlz_destination` タイプを使用すると、APIはランディングゾーンの宛先コンテナを、使用可能な他のタイプのコンテナと区別できます。 |
| `refresh` | `refresh` アクションは、ランディングゾーンの資格情報をリセットし、新しい`SASToken`を自動的に生成します。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストは、ランディングゾーンの資格情報を更新します。

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

次の応答は、`SASToken`と`SASUri`の更新された値を返します。

```json
{
    "containerName": "dlz-destination",
    "SASToken": "sv=2020-04-08&si=dlz-9c4d03b8-a6ff-41be-9dcf-20123e717e99&sr=c&sp=racwdlm&sig=JbRMoDmFHQU4OWOpgrKdbZ1d%2BkvslO35%2FXTqBO%2FgbRA%3D",
    "storageAccountName": "dlblobstore99hh25i3dflek",
    "SASUri": "https://dlblobstore99hh25i3dflek.blob.core.windows.net/dlz-destination?sv=2020-04-08&si=dlz-9c4d03b8-a6ff-41be-9dcf-20123e717e99&sr=c&sp=racwdlm&sig=JbRMoDmFHQU4OWOpgrKdbZ1d%2BkvslO35%2FXTqBO%2FgbRA%3D"
}
```

>[!ENDSHADEBOX]

前述のAPI呼び出しで返される表示名（`containerName`）と[!DNL Data Landing Zone] SAS URLを入力し、**次へ**&#x200B;を選択します。

![Azure UIでハイライト表示されている接続情報を入力します。](/help/sources/images/tutorials/create/dlz/enter-connection-info.png)

**概要**&#x200B;ウィンドウが開き、[!DNL Blob] エンドポイントと権限を含む設定の概要が表示されます。準備ができたら、「**接続**」を選択します。

![Azure UIに表示される設定の概要。](/help/sources/images/tutorials/create/dlz/summary.png)

接続が成功すると、[!DNL Azure Storage Explorer] UI と [!DNL Data Landing Zone] コンテナが更新されます。

![Azure UIでハイライト表示されたDLZ ユーザーコンテナの概要。](/help/sources/images/tutorials/create/dlz/dlz-user-container.png)

[!DNL Data Landing Zone] コンテナが [!DNL Azure Storage Explorer] に接続され、Experience Platform から [!DNL Data Landing Zone] コンテナへのファイルの書き出しを開始できるようになりました。ファイルを書き出すには、以下の節で説明するように、Experience Platform UIの[!DNL Data Landing Zone]宛先への接続を確立する必要があります。

## AWSでプロビジョニングされたデータランディングゾーンへの認証 {#authenticate-dlz-aws}

>[!AVAILABILITY]
>
>この節は、Amazon Web Services（AWS）で動作するExperience Platformの実装に適用されます。 AWS上で動作するExperience Platformは、現在、一部のお客様にご利用いただけます。 サポートされているExperience Platform インフラストラクチャについて詳しくは、[Experience Platform マルチクラウドの概要](https://experienceleague.adobe.com/en/docs/experience-platform/landing/multi-cloud)を参照してください。

AWSでプロビジョニングされた[!DNL Data Landing Zone] インスタンスに資格情報を取得するには、次の操作を実行します。 次に、任意のクライアントを使用して[!DNL Data Landing Zone] インスタンスに接続します。

>[!BEGINSHADEBOX]

### [!DNL Data Landing Zone]の資格情報を取得 {#retrieve-dlz-credentials-aws}

Experience Platform APIを使用して、[!DNL Data Landing Zone]資格情報を取得する必要があります。 資格情報を取得するためのAPI呼び出しについて以下に説明します。 ヘッダーに必要な値の取得について詳しくは、[Adobe Experience Platform APIの概要](/help/landing/api-guide.md) ガイドを参照してください。

**API 形式**

```http
GET /data/foundation/connectors/landingzone/credentials?type=dlz_destination'
```

| クエリパラメーター | 説明 |
| --- | --- |
| `dlz_destination` | `dlz_destination` クエリパラメーターを追加して、[!DNL Data Landing Zone] *宛先* タイプのコンテナ資格情報を取得することを指定します。 データランディングゾーン *source*&#x200B;の資格情報を接続して取得するには、[sources ドキュメント ](/help/sources/connectors/cloud-storage/data-landing-zone.md)を参照してください。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストの例では、既存のランディングゾーンの資格情報を取得します。

```shell
curl --request GET \
  --url 'https://platform.adobe.io/data/foundation/connectors/landingzone/credentials?type=dlz_destination' \
  --header 'Authorization: Bearer ***' \
  --header 'Content-Type: application/json' \
  --header 'x-api-key: your_api_key' \
  --header 'x-gw-ims-org-id: yourorg@AdobeOrg'
```

**応答**

次の応答は、現在の`awsAccessKeyId`、`awsSecretAccessKey`およびその他の情報を含む、ランディングゾーンの資格情報を返します。

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
| `credentials` | このオブジェクトには、Experience Platformがプロビジョニングされたデータランディングゾーンの場所にファイルを書き出すために使用する`awsAccessKeyId`、`awsSecretAccessKey`および`awsSessionToken`が含まれます。 |
| `dlzPath` | このオブジェクトには、書き出されたファイルがデポジットされるAdobeでプロビジョニングされたAWSの場所のパスが含まれます。 |
| `dlzProvider` | これは、Amazon S3 プロビジョニングされたデータランディングゾーンであることを示します。 |
| `expiryTime` | `credentials` オブジェクトの資格情報の有効期限を示します。 資格情報を更新するには、もう一度リクエストを実行します。 |

{style="table-layout:auto"}

>[!ENDSHADEBOX]

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage Destinations]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/connect-destination.html?lang=ja)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

[!DNL Data Landing Zone]前提条件[!DNL Azure Storage Explorer] セクションの説明に従って、[ コンテナを](#prerequisites)に接続していることを確認してください。 [!DNL Data Landing Zone]はAdobeでプロビジョニングされたストレージであるため、Experience Platform UIでさらに手順を実行して宛先に対する認証を行う必要はありません。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL Encryption key]**: オプションで、RSA形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 正しい形式の暗号化キーの例については、以下の画像を参照してください。
  ![UIで正しくフォーマットされたPGP キーの例を示す画像。](../../assets/catalog/cloud-storage/sftp/pgp-key.png)
* **[!UICONTROL Name]**：この宛先の優先名を入力します。
* **[!UICONTROL Description]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL Folder path]**：書き出されたファイルをホストする宛先フォルダーへのパスを入力します。
* **[!UICONTROL File type]**：書き出したファイルにExperience Platformで使用する形式を選択します。 [!UICONTROL CSV] オプションを選択する際に、[ ファイル形式オプションを設定することもできます](../../ui/batch-destinations-file-formatting-options.md)。
* **[!UICONTROL Compression format]**：書き出したファイルにExperience Platformで使用する圧縮タイプを選択します。
* **[!UICONTROL Include manifest file]**：書き出しの場所や書き出しサイズなどの情報を含むマニフェスト JSON ファイルを書き出しに含める場合は、このオプションをオンに切り替えます。 マニフェストの名前は、形式`manifest-<<destinationId>>-<<dataflowRunId>>.json`を使用して指定されています。 [ サンプルマニフェストファイル ](/help/destinations/assets/common/manifest-d0420d72-756c-4159-9e7f-7d3e2f8b501e-0ac8f3c0-29bd-40aa-82c1-f1b7e0657b19.json)を表示します。 マニフェストファイルには、次のフィールドが含まれます。
   * `flowRunId`: エクスポートされたファイルを生成した[ データフロー実行](/help/dataflows/ui/monitor-destinations.md#dataflow-runs-for-batch-destinations)。
   * `scheduledTime`: ファイルがエクスポートされたUTCの時間。
   * `exportResults.sinkPath`：書き出されたファイルが格納されているストレージの場所のパス。
   * `exportResults.name`: エクスポートされたファイルの名前。
   * `size`：書き出されたファイルのサイズ （バイト単位）。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の提供が完了したら、**[!UICONTROL Next]**&#x200B;を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID*&#x200B;をエクスポートするには、**[!UICONTROL View Identity Graph]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。<br> ![ ワークフローで強調表示されているID名前空間を選択して、オーディエンスを宛先にアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png " ワークフローで強調表示されたID名前空間を選択して、オーディエンスを宛先にアクティブ化します。"){width="100" zoomable="yes"}

この宛先に対するオーディエンスのアクティブ化の手順については、[ バッチプロファイル書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md)を参照してください。

### スケジュール設定 {#scheduling}

**[!UICONTROL Scheduling]**&#x200B;手順では、[宛先に対して](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling)書き出しスケジュール [!DNL Data Landing Zone]を設定でき、書き出したファイルの名前を[設定することもできます](/help/destinations/ui/activate-batch-profile-destinations.md#configure-file-names)。

### 属性と ID のマッピング {#map}

**[!UICONTROL Mapping]** ステップでは、プロファイルにエクスポートする属性フィールドとID フィールドを選択できます。 また、書き出したファイル内のヘッダーを選択して、任意のわかりやすい名前に変更することもできます。詳しくは、「バッチの宛先をアクティベート」UI チュートリアルの[マッピング手順](/help/destinations/ui/activate-batch-profile-destinations.md#mapping)を参照してください。

## データの正常な書き出しの検証 {#exported-data}

データが正常に書き出されたかどうかを確認するには、[!DNL Data Landing Zone] ストレージを確認し、書き出されたファイルに想定されるプロファイル母集団が含まれていることを確認してください。
