---
title: データランディングゾーンの宛先
description: データランディングゾーンに接続して、オーディエンスをアクティブ化し、データセットを書き出す方法について説明します。
last-substantial-update: 2023-07-26T00:00:00Z
exl-id: 40b20faa-cce6-41de-81a0-5f15e6c00e64
source-git-commit: 661ef040398a9e2ef8dd9cebdf7bd27d4268636b
workflow-type: tm+mt
source-wordcount: '1482'
ht-degree: 53%

---

# データランディングゾーンの宛先

>[!IMPORTANT]
>
>このドキュメントページでは、 [!DNL Data Landing Zone] *宛先*. また、 [!DNL Data Landing Zone] *ソース* ソースカタログ内。 詳しくは、 [[!DNL Data Landing Zone] ソース](/help/sources/connectors/cloud-storage/data-landing-zone.md) ドキュメント。


## 概要 {#overview}

[!DNL Data Landing Zone] は Adobe Experience Platform によってプロビジョニングされた [!DNL Azure Blob] ストレージインターフェイスです。安全なクラウドベースのファイルストレージ機能にアクセスして、ファイルを Platform から書き出すことができます。サンドボックスごとに 1 つの [!DNL Data Landing Zone] コンテナに対するアクセス権があります。すべてのコンテナの合計データ量は、Platform 製品およびサービスライセンスで提供される合計データ量に制限されます。Platform とそのアプリケーションサービスのすべての顧客（[!DNL Customer Journey Analytics]、[!DNL Journey Orchestration]、[!DNL Intelligent Services]、および [!DNL Real-Time Customer Data Platform]）は、サンドボックスごとに 1 つの [!DNL Data Landing Zone] のコンテナを使用してプロビジョニングされます。[!DNL Azure Storage Explorer] またはコマンドラインインターフェイスを通じて、コンテナに対してファイルの読み取りと書き込みを行うことができます。

[!DNL Data Landing Zone] は SAS ベースの認証をサポートし、そのデータは保存時および転送中は標準 [!DNL Azure Blob] ストレージセキュリティメカニズムで保護されます。SAS ベースの認証を使用すると、パブリックインターネット接続を介して [!DNL Data Landing Zone] コンテナに安全にアクセスできます。ユーザーが [!DNL Data Landing Zone] コンテナにアクセスする場合、ネットワークの変更は必要ありません。つまり、ネットワークに対して許可リストの設定や地域間設定は必要ありません。

Platform では、[!DNL Data Landing Zone] コンテナへアップロードされるすべてのファイルで厳密に 7 日間の有効期間（TTL）が適用されます。すべてのファイルは 7 日後に削除されます。

## 次に接続： [!UICONTROL データランディングゾーン] API または UI を介したストレージ {#connect-api-or-ui}

* 次の URL に接続するには： [!UICONTROL データランディングゾーン] ストレージの場所 Platform ユーザーインターフェイスを使用して、「 」セクションを読みます。 [宛先に接続](#connect) および [この宛先に対するオーディエンスをアクティブ化](#activate) 下
* 次の URL に接続するには： [!UICONTROL データランディングゾーン] ストレージの場所をプログラムで設定し、読み取る [フローサービス API のチュートリアルを使用して、ファイルベースの宛先に対するオーディエンスをアクティブ化します](../../api/activate-segments-file-based-destinations.md).

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの起源 | サポートあり | 説明 |
---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [セグメント化サービス](../../../segmentation/home.md). |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/overview.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | [宛先のアクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性の選択画面で選択したように、該当するスキーマフィールド（例：PPID）と共に、セグメントのすべてのメンバーを書き出しています。 |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## 前提条件 {#prerequisites}

を使用する前に満たす必要がある前提条件は、次のとおりです。 [!DNL Data Landing Zone] 宛先。

### 接続する [!DNL Data Landing Zone] コンテナ [!DNL Azure Storage Explorer]

[[!DNL Azure Storage Explorer]](https://azure.microsoft.com/en-us/products/storage/storage-explorer/) を使用して [!DNL Data Landing Zone] コンテナのコンテンツを管理することができます。を使用し始めるには [!DNL Data Landing Zone]を使用する場合は、まず資格情報を取得し、に入力する必要があります。 [!DNL Azure Storage Explorer]をクリックし、 [!DNL Data Landing Zone] コンテナ [!DNL Azure Storage Explorer].

[!DNL Azure Storage Explorer] UI 内で、左側のナビゲーションバーの「接続」アイコンを選択します。**リソースを選択**&#x200B;ウィンドウが開き、接続するオプションが表示されます。**[!DNL Blob container]** を選択し、[!DNL Data Landing Zone] ストレージに接続します。

![select-resource](/help/sources/images/tutorials/create/dlz/select-resource.png)

次に、接続方法として「**共有アクセス署名 URL (SAS)**」を選択し、「**次へ**」をクリックします。

![select-connection-method](/help/sources/images/tutorials/create/dlz/select-connection-method.png)

接続方法を選択した後、**表示名**&#x200B;およびお使いの [!DNL Data Landing Zone] コンテナに対応する&#x200B;**[!DNL Blob]コンテナ SAS URL** を入力します。

>[!BEGINSHADEBOX]

### の資格情報を取得します。 [!DNL Data Landing Zone] {#retrieve-dlz-credentials}

Platform API を使用して、 [!DNL Data Landing Zone] 認証情報。 資格情報を取得するための API 呼び出しを以下に示します。 ヘッダーに必要な値の取得について詳しくは、 [Adobe Experience Platform API の概要](/help/landing/api-guide.md) ガイド。

**API 形式**

```http
GET /data/foundation/connectors/landingzone/credentials?type=dlz_destination
```

| クエリパラメーター | 説明 |
| --- | --- |
| `dlz_destination` | The `dlz_destination` タイプを使用すると、API はランディングゾーンの宛先コンテナを、使用可能な他のタイプのコンテナと区別できます。 |

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

次の応答は、現在の `SASToken` および `SASUri`、および `storageAccountName` をランディングゾーンコンテナに対応させます。

```json
{
    "containerName": "dlz-user-container",
    "SASToken": "sv=2022-09-11&si=dlz-ed86a61d-201f-4b50-b10f-a1bf173066fd&sr=c&sp=racwdlm&sig=4yTba8voU3L0wlcLAv9mZLdZ7NlMahbfYYPTMkQ6ZGU%3D",
    "storageAccountName": "dlblobstore99hh25i3df123",
    "SASUri": "https://dlblobstore99hh25i3dflek.blob.core.windows.net/dlz-user-container?sv=2022-09-11&si=dlz-ed86a61d-201f-4b50-b10f-a1bf173066fd&sr=c&sp=racwdlm&sig=4yTba8voU3L0wlcLAv9mZLdZ7NlMahbfYYPTMkQ6ZGU%3D"
}
```

| プロパティ | 説明 |
| --- | --- |
| `containerName` | ランディングゾーンの名前。 |
| `SASToken` | ランディングゾーンの共有アクセス署名トークン。 この文字列には、リクエストを承認するために必要なすべての情報が含まれます。 |
| `SASUri` | ランディングゾーンの共有アクセス署名 URI です。 この文字列は、認証先のランディングゾーンへの URI と、対応する SAS トークンの組み合わせです。 |

{style="table-layout:auto"}

### 更新 [!DNL Data Landing Zone] 資格情報 {#update-dlz-credentials}

必要に応じて、資格情報を更新することもできます。 次の項目を更新： `SASToken` に対してPOSTリクエストを行う `/credentials` エンドポイント [!DNL Connectors] API.

**API 形式**

```http
POST /data/foundation/connectors/landingzone/credentials?type=dlz_destination&action=refresh
```

| クエリパラメーター | 説明 |
| --- | --- |
| `dlz_destination` | The `dlz_destination` タイプを使用すると、API はランディングゾーンの宛先コンテナを、使用可能な他のタイプのコンテナと区別できます。 |
| `refresh` | The `refresh` 「 」アクションを使用すると、ランディングゾーンの資格情報をリセットし、新しい `SASToken`. |

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

次の応答は、 `SASToken` および `SASUri`.

```json
{
    "containerName": "dlz-user-container",
    "SASToken": "sv=2020-04-08&si=dlz-9c4d03b8-a6ff-41be-9dcf-20123e717e99&sr=c&sp=racwdlm&sig=JbRMoDmFHQU4OWOpgrKdbZ1d%2BkvslO35%2FXTqBO%2FgbRA%3D",
    "storageAccountName": "dlblobstore99hh25i3dflek",
    "SASUri": "https://dlblobstore99hh25i3dflek.blob.core.windows.net/dlz-user-container?sv=2020-04-08&si=dlz-9c4d03b8-a6ff-41be-9dcf-20123e717e99&sr=c&sp=racwdlm&sig=JbRMoDmFHQU4OWOpgrKdbZ1d%2BkvslO35%2FXTqBO%2FgbRA%3D"
}
```

>[!ENDSHADEBOX]

表示名 (`containerName`) および [!DNL Data Landing Zone] 上記の API 呼び出しで返される SAS URL で、を選択します。 **次へ**.

![enter-connection-info](/help/sources/images/tutorials/create/dlz/enter-connection-info.png)

**概要**&#x200B;ウィンドウが開き、[!DNL Blob] エンドポイントと権限を含む設定の概要が表示されます。準備ができたら、「**接続**」を選択します。

![概要](/help/sources/images/tutorials/create/dlz/summary.png)

接続が成功すると、[!DNL Azure Storage Explorer] UI と [!DNL Data Landing Zone] コンテナが更新されます。

![dlz-user-container](/help/sources/images/tutorials/create/dlz/dlz-user-container.png)

[!DNL Data Landing Zone] コンテナが [!DNL Azure Storage Explorer] に接続され、Experience Platform から [!DNL Data Landing Zone] コンテナへのファイルの書き出しを開始できるようになりました。ファイルを書き出すには、 [!DNL Data Landing Zone] の宛先を指定します（以下の節で説明）。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/connect-destination.html?lang=ja)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

次のページとの連携を確認します： [!DNL Data Landing Zone] コンテナ [!DNL Azure Storage Explorer] 例えば、 [前提条件](#prerequisites) 」セクションに入力します。 理由： [!DNL Data Landing Zone] は、Adobeがプロビジョニングしたストレージです。Experience PlatformUI で追加の手順を実行して、宛先への認証をおこなう必要はありません。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：この宛先に希望する名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL フォルダーパス]**：書き出したファイルをホストする宛先フォルダーへのパス。
* **[!UICONTROL ファイルタイプ]**：書き出したファイルに使用する形式Experience Platformを選択します。 選択時に、 [!UICONTROL CSV] オプションを選択する場合は、 [ファイル形式設定オプションの設定](../../ui/batch-destinations-file-formatting-options.md).
* **[!UICONTROL 圧縮形式]**：書き出したファイルに Experience Platform で使用する圧縮タイプを選択します。
* **[!UICONTROL マニフェストファイルを含める]**：書き出しの場所や書き出しサイズなどに関する情報を含むマニフェスト JSON ファイルを書き出しに含める場合は、このオプションをオンに切り替えます。 マニフェストの名前は、形式を使用して付けられます `manifest-<<destinationId>>-<<dataflowRunId>>.json`. を表示します。 [サンプルマニフェストファイル](/help/destinations/assets/common/manifest-d0420d72-756c-4159-9e7f-7d3e2f8b501e-0ac8f3c0-29bd-40aa-82c1-f1b7e0657b19.json). マニフェストファイルには、次のフィールドが含まれます。
   * `flowRunId`: [データフローの実行](/help/dataflows/ui/monitor-destinations.md#dataflow-runs-for-batch-destinations) 書き出されたファイルを生成した
   * `scheduledTime`：ファイルが書き出されたときの UTC 時刻 (UTC)。
   * `exportResults.sinkPath`：書き出されたファイルが格納されるストレージの場所のパス。
   * `exportResults.name`：書き出されたファイルの名前。
   * `size`：書き出されるファイルのサイズ（バイト単位）。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。
>* 書き出す *id*、 **[!UICONTROL ID グラフを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png "ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。"){width="100" zoomable="yes"}

詳しくは、 [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md) を参照してください。

### スケジュール設定

**[!UICONTROL スケジュール設定]**&#x200B;手順では、[!DNL Data Landing Zone] 宛先の[書き出しスケジュールを設定](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling)し、[書き出したファイルの名前を設定](/help/destinations/ui/activate-batch-profile-destinations.md#file-names)することもできます。

### 属性と ID のマッピング {#map}

**[!UICONTROL マッピング]**&#x200B;手順では、プロファイルに書き出す属性および ID フィールドを選択できます。 また、書き出したファイル内のヘッダーを選択して、任意のわかりやすい名前に変更することもできます。詳しくは、「バッチの宛先をアクティベート」UI チュートリアルの[マッピング手順](/help/destinations/ui/activate-batch-profile-destinations.md#mapping)を参照してください。

## （ベータ版）データセットの書き出し {#export-datasets}

この宛先では、データセットの書き出しをサポートしています。 データセットエクスポートの設定方法について詳しくは、次のチュートリアルを参照してください。

* 方法 [Platform ユーザーインターフェイスを使用したデータセットの書き出し](/help/destinations/ui/export-datasets.md).
* 方法 [フローサービス API を使用したデータセットの書き出し](/help/destinations/api/export-datasets.md).

## データの正常な書き出しの検証 {#exported-data}

データが正常に書き出されたかどうかを確認するには、[!DNL Data Landing Zone] ストレージを確認し、書き出されたファイルに想定されるプロファイル母集団が含まれていることを確認してください。
