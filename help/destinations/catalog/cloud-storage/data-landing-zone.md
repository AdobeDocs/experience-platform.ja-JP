---
title: データランディングゾーンの宛先
description: データランディングゾーンに接続してオーディエンスをアクティブ化し、データセットを書き出す方法を説明します。
last-substantial-update: 2023-07-26T00:00:00Z
exl-id: 40b20faa-cce6-41de-81a0-5f15e6c00e64
source-git-commit: c35b43654d31f0f112258e577a1bb95e72f0a971
workflow-type: tm+mt
source-wordcount: '1585'
ht-degree: 44%

---

# データランディングゾーンの宛先

>[!IMPORTANT]
>
>このドキュメントページでは、 [!DNL Data Landing Zone] *宛先*. また、 [!DNL Data Landing Zone] *ソース* ソースカタログ内 詳しくは、 [[!DNL Data Landing Zone] ソース](/help/sources/connectors/cloud-storage/data-landing-zone.md) ドキュメント。


## 概要 {#overview}

[!DNL Data Landing Zone] は Adobe Experience Platform によってプロビジョニングされた [!DNL Azure Blob] ストレージインターフェイスです。安全なクラウドベースのファイルストレージ機能にアクセスして、ファイルを Platform から書き出すことができます。サンドボックスごとに 1 つの [!DNL Data Landing Zone] コンテナに対するアクセス権があります。すべてのコンテナの合計データ量は、Platform 製品およびサービスライセンスで提供される合計データ量に制限されます。Platform およびそのアプリケーションのすべての顧客（例：） [!DNL Customer Journey Analytics], [!DNL Journey Orchestration], [!DNL Intelligent Services]、および [!DNL Real-Time Customer Data Platform] 1 つをプロビジョニングする [!DNL Data Landing Zone] サンドボックスごとのコンテナ。 [!DNL Azure Storage Explorer] またはコマンドラインインターフェイスを通じて、コンテナに対してファイルの読み取りと書き込みを行うことができます。

[!DNL Data Landing Zone] は SAS ベースの認証をサポートし、そのデータは保存時および転送中は標準 [!DNL Azure Blob] ストレージセキュリティメカニズムで保護されます。SAS ベースの認証を使用すると、パブリックインターネット接続を介して [!DNL Data Landing Zone] コンテナに安全にアクセスできます。ユーザーが [!DNL Data Landing Zone] コンテナにアクセスする場合、ネットワークの変更は必要ありません。つまり、ネットワークに対して許可リストの設定や地域間設定は必要ありません。

Platform では、[!DNL Data Landing Zone] コンテナへアップロードされるすべてのファイルで厳密に 7 日間の有効期間（TTL）が適用されます。すべてのファイルは 7 日後に削除されます。

## に接続 [!UICONTROL Data Landing Zone] api または UI を介したストレージ {#connect-api-or-ui}

* に接続するには [!UICONTROL Data Landing Zone] ストレージの場所 Platform ユーザーインターフェイスを使用して、セクションを参照してください [宛先への接続](#connect) および [この宛先に対してオーディエンスをアクティブ化](#activate) 下。
* に接続するには [!UICONTROL Data Landing Zone] ストレージの場所をプログラムで読み取る [Flow Service API チュートリアルを使用して、ファイルベースの宛先に対するオーディエンスをアクティブ化します](../../api/activate-segments-file-based-destinations.md).

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platformを通じて生成されたオーディエンス [セグメント化サービス](../../../segmentation/home.md). |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | [宛先のアクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性の選択画面で選択したように、該当するスキーマフィールド（例：PPID）と共に、セグメントのすべてのメンバーを書き出しています。 |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## データセットを書き出し {#export-datasets}

この宛先では、データセットの書き出しをサポートしています。 データセットの書き出しを設定する方法について詳しくは、次のチュートリアルを参照してください。

* 方法 [platform ユーザーインターフェイスを使用したデータセットの書き出し](/help/destinations/ui/export-datasets.md).
* 方法 [flow Service API を使用したプログラムによるデータセットの書き出し](/help/destinations/api/export-datasets.md).

## 書き出されたデータのファイル形式 {#file-format}

書き出し時 *オーディエンスデータ*、Platform は `.csv`, `parquet`、または `.json` ファイルは、指定したストレージの場所にあります。 ファイルの詳細については、を参照してください [書き出しでサポートされるファイル形式](../../ui/activate-batch-profile-destinations.md#supported-file-formats-export) Audience Activation チュートリアルの「」の節。

書き出し時 *データセット*、Platform は `.parquet` または `.json` ファイルは、指定したストレージの場所にあります。 ファイルの詳細については、を参照してください [データセットの正常な書き出しの確認](../../ui/export-datasets.md#verify) データセットの書き出しに関するチュートリアルの「」節。

## 前提条件 {#prerequisites}

を使用するには、次の前提条件を満たしておく必要があります。 [!DNL Data Landing Zone] の宛先。

### を接続 [!DNL Data Landing Zone] コンテナ先 [!DNL Azure Storage Explorer]

次を使用できます [[!DNL Azure Storage Explorer]](https://azure.microsoft.com/en-us/products/storage/storage-explorer/) のコンテンツを管理するには [!DNL Data Landing Zone] コンテナ。 を使用するには [!DNL Data Landing Zone]。最初に資格情報を取得して、に入力する必要があります [!DNL Azure Storage Explorer]を作成して、 [!DNL Data Landing Zone] コンテナ先 [!DNL Azure Storage Explorer].

[!DNL Azure Storage Explorer] UI 内で、左側のナビゲーションバーの「接続」アイコンを選択します。**リソースを選択**&#x200B;ウィンドウが開き、接続するオプションが表示されます。**[!DNL Blob container]** を選択し、[!DNL Data Landing Zone] ストレージに接続します。

![Azure UI でハイライト表示されているリソースを選択します。](/help/sources/images/tutorials/create/dlz/select-resource.png)

次に、接続方法として「**共有アクセス署名 URL (SAS)**」を選択し、「**次へ**」をクリックします。

![Azure UI でハイライト表示されている接続方法を選択します。](/help/sources/images/tutorials/create/dlz/select-connection-method.png)

接続方法を選択した後、**表示名**&#x200B;およびお使いの [!DNL Data Landing Zone] コンテナに対応する&#x200B;**[!DNL Blob]コンテナ SAS URL** を入力します。

>[!BEGINSHADEBOX]

### の資格情報を取得する [!DNL Data Landing Zone] {#retrieve-dlz-credentials}

を取得するには、Platform API を使用する必要があります [!DNL Data Landing Zone] 資格情報。 資格情報を取得するための API 呼び出しは、以下に説明されています。 ヘッダーに必要な値の取得について詳しくは、 [Adobe Experience Platform API の概要](/help/landing/api-guide.md) ガイド。

**API 形式**

```http
GET /data/foundation/connectors/landingzone/credentials?type=dlz_destination
```

| クエリパラメーター | 説明 |
| --- | --- |
| `dlz_destination` | この `dlz_destination` タイプを使用すると、API は、ランディングゾーンの宛先コンテナを、使用可能な他のタイプのコンテナと区別できます。 |

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

次の応答では、現在の情報を含むランディングゾーンの資格情報が返されます `SASToken` および `SASUri`、および `storageAccountName` これはランディングゾーンコンテナに対応します。

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
| `SASToken` | ランディングゾーンの共有アクセス署名トークン。 この文字列には、リクエストの認証に必要なすべての情報が含まれます。 |
| `SASUri` | ランディングゾーンの共有アクセス署名 URI。 この文字列は、認証対象のランディングゾーンの URI とそれに対応する SAS トークンの組み合わせです。 |

{style="table-layout:auto"}

### 更新 [!DNL Data Landing Zone] 資格情報 {#update-dlz-credentials}

必要に応じて、資格情報を更新することもできます。 を更新できます `SASToken` に対してPOSTリクエストを行う `/credentials` エンドポイント [!DNL Connectors] API です。

**API 形式**

```http
POST /data/foundation/connectors/landingzone/credentials?type=dlz_destination&action=refresh
```

| クエリパラメーター | 説明 |
| --- | --- |
| `dlz_destination` | この `dlz_destination` タイプを使用すると、API は、ランディングゾーンの宛先コンテナを、使用可能な他のタイプのコンテナと区別できます。 |
| `refresh` | この `refresh` アクションを使用すると、ランディングゾーンの資格情報をリセットし、新しいを自動的に生成できます `SASToken`. |

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

次の応答は、の更新された値を返します `SASToken` および `SASUri`.

```json
{
    "containerName": "dlz-user-container",
    "SASToken": "sv=2020-04-08&si=dlz-9c4d03b8-a6ff-41be-9dcf-20123e717e99&sr=c&sp=racwdlm&sig=JbRMoDmFHQU4OWOpgrKdbZ1d%2BkvslO35%2FXTqBO%2FgbRA%3D",
    "storageAccountName": "dlblobstore99hh25i3dflek",
    "SASUri": "https://dlblobstore99hh25i3dflek.blob.core.windows.net/dlz-user-container?sv=2020-04-08&si=dlz-9c4d03b8-a6ff-41be-9dcf-20123e717e99&sr=c&sp=racwdlm&sig=JbRMoDmFHQU4OWOpgrKdbZ1d%2BkvslO35%2FXTqBO%2FgbRA%3D"
}
```

>[!ENDSHADEBOX]

ディスプレイ名を入力（`containerName`）および [!DNL Data Landing Zone] 上記の API 呼び出しで返された SAS URL を選択して、 **次**.

![Azure UI でハイライト表示されている接続情報を入力します。](/help/sources/images/tutorials/create/dlz/enter-connection-info.png)

**概要**&#x200B;ウィンドウが開き、[!DNL Blob] エンドポイントと権限を含む設定の概要が表示されます。準備ができたら、「**接続**」を選択します。

![Azure UI に表示される設定の概要です。](/help/sources/images/tutorials/create/dlz/summary.png)

接続が成功すると、[!DNL Azure Storage Explorer] UI と [!DNL Data Landing Zone] コンテナが更新されます。

![Azure UI で強調表示されている DLZ ユーザーコンテナの概要。](/help/sources/images/tutorials/create/dlz/dlz-user-container.png)

[!DNL Data Landing Zone] コンテナが [!DNL Azure Storage Explorer] に接続され、Experience Platform から [!DNL Data Landing Zone] コンテナへのファイルの書き出しを開始できるようになりました。ファイルをエクスポートするには、への接続を確立する必要があります [!DNL Data Landing Zone] Experience PlatformUI での宛先（以下の節で説明します）。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/connect-destination.html?lang=ja)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

を接続したことを確認します [!DNL Data Landing Zone] コンテナ先 [!DNL Azure Storage Explorer] で説明されているように [前提条件](#prerequisites) セクション。 なぜなら [!DNL Data Landing Zone] は、Adobeがプロビジョニングしたストレージです。宛先への認証のために、Experience PlatformUI でそれ以上手順を実行する必要はありません。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：この宛先に希望する名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL フォルダーパス]**：書き出したファイルをホストする宛先フォルダーへのパス。
* **[!UICONTROL ファイルタイプ]**：書き出したファイルでExperience Platformが使用するフォーマットを選択します。 を選択した場合 [!UICONTROL CSV] オプションを使用すると、次のこともできます [ファイル形式オプションの設定](../../ui/batch-destinations-file-formatting-options.md).
* **[!UICONTROL 圧縮フォーマット]**：書き出したファイルにExperience Platformで使用する圧縮タイプを選択します。
* **[!UICONTROL マニフェストファイルを含める]**：書き出しに、書き出しの場所、書き出しサイズなどに関する情報を含んだマニフェスト JSON ファイルを含める場合は、このオプションをオンに切り替えます。 マニフェストには、形式を使用して名前を付けます `manifest-<<destinationId>>-<<dataflowRunId>>.json`. を表示 [サンプルマニフェストファイル](/help/destinations/assets/common/manifest-d0420d72-756c-4159-9e7f-7d3e2f8b501e-0ac8f3c0-29bd-40aa-82c1-f1b7e0657b19.json). マニフェストファイルには、次のフィールドが含まれています。
   * `flowRunId`：です [データフローの実行](/help/dataflows/ui/monitor-destinations.md#dataflow-runs-for-batch-destinations) 書き出されたファイルを生成した。
   * `scheduledTime`：ファイルが書き出された時間（UTC 単位）。
   * `exportResults.sinkPath`：書き出したファイルを格納するストレージの場所のパス。
   * `exportResults.name`：書き出すファイルの名前。
   * `size`：書き出したファイルのサイズ（バイト単位）。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* エクスポートする *id*、が必要です **[!UICONTROL ID グラフの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![宛先に対してオーディエンスをアクティブ化するために、ワークフローで強調表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png "宛先に対してオーディエンスをアクティブ化するために、ワークフローで強調表示されている ID 名前空間を選択します。"){width="100" zoomable="yes"}

参照： [プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](../../ui/activate-batch-profile-destinations.md) この宛先に対してオーディエンスをアクティブ化する手順については、を参照してください。

### スケジュール設定

**[!UICONTROL スケジュール設定]**&#x200B;手順では、[!DNL Data Landing Zone] 宛先の[書き出しスケジュールを設定](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling)し、[書き出したファイルの名前を設定](/help/destinations/ui/activate-batch-profile-destinations.md#file-names)することもできます。

### 属性と ID のマッピング {#map}

**[!UICONTROL マッピング]**&#x200B;手順では、プロファイルに書き出す属性および ID フィールドを選択できます。 また、書き出したファイル内のヘッダーを選択して、任意のわかりやすい名前に変更することもできます。詳しくは、「バッチの宛先をアクティベート」UI チュートリアルの[マッピング手順](/help/destinations/ui/activate-batch-profile-destinations.md#mapping)を参照してください。

## データの正常な書き出しの検証 {#exported-data}

データが正常に書き出されたかどうかを確認するには、[!DNL Data Landing Zone] ストレージを確認し、書き出されたファイルに想定されるプロファイル母集団が含まれていることを確認してください。
