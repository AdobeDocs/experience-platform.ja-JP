---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: プライバシージョブ API エンドポイント
description: Privacy API を使用してExperience CloudアプリケーションのPrivacy Serviceジョブを管理する方法について説明します。
role: Developer
exl-id: 74a45f29-ae08-496c-aa54-b71779eaeeae
source-git-commit: 26a50f21c1ebebf485eaf62712bd02de3406cceb
workflow-type: tm+mt
source-wordcount: '1810'
ht-degree: 45%

---

# プライバシージョブエンドポイント

このドキュメントでは、API 呼び出しを使用したプライバシージョブの操作方法について説明します。 特に、[!DNL Privacy Service] API の `/job` エンドポイントの使用について説明します。 このガイドを読む前に、[ はじめる前に ](./getting-started.md) を参照して、必要なヘッダーやサンプル API 呼び出しの読み取り方法など、API の呼び出しを正しく実行するために必要な重要な情報を確認してください。

>[!NOTE]
>
>顧客からの同意またはオプトアウトリクエストを管理しようとする場合は、[ 同意エンドポイントガイド ](./consent.md) を参照してください。

## すべてのジョッブをリスト {#list}

`/jobs` エンドポイントに対してGETリクエストを行うことで、組織内で使用可能なすべてのプライバシージョブのリストを表示できます。

**API 形式**

このリクエスト形式は、`/jobs` エンドポイントで `regulation` クエリパラメーターを使用するので、以下に示すように、疑問符（`?`）で始まります。 リソースをリストする場合、Privacy ServiceAPI は最大 1,000 個のジョブを返し、応答にページ番号を付けます。 その他のクエリパラメーター（`page`、`size`、日付のフィルター）を使用して、応答をフィルタリングします。 アンパサンド（`&`）を使用して、複数のパラメーターを区切ることができます。

>[!TIP]
>
>追加のクエリパラメーターを使用して、特定のクエリの結果をさらにフィルタリングします。 例えば、特定の期間に送信されたプライバシージョブの数や、そのステータスが `status`、`fromDate`、`toDate` のクエリパラメーターを使用しているかなどを確認できます。

```http
GET /jobs?regulation={REGULATION}
GET /jobs?regulation={REGULATION}&page={PAGE}
GET /jobs?regulation={REGULATION}&size={SIZE}
GET /jobs?regulation={REGULATION}&page={PAGE}&size={SIZE}
GET /jobs?regulation={REGULATION}&fromDate={FROMDATE}&toDate={TODATE}&status={STATUS}
```

| パラメーター | 説明 |
| --- | --- |
| `{REGULATION}` | クエリする規制の種類。使用できる値は次のとおりです。 <ul><li>`apa_aus`</li><li>`ccpa`</li><li>`cpa_usa`</li><li>`cpra_usa`</li><li>`ctdpa_usa`</li><li>`dpdpa`</li><li>`fdbr_usa`</li><li>`gdpr`</li><li>`hipaa_usa`</li><li>`icdpa_usa`</li><li>`lgpd_bra`</li><li>`mcdpa_usa`</li><li>`mhmda_usa`</li><li>`ndpa_usa`</li><li>`nhpa_usa`</li><li>`njdpa_usa`</li><li>`nzpa_nzl`</li><li>`ocpa_usa`</li><li>`pdpa_tha`</li><li>`ql25`</li><li>`tdpsa_usa`</li><li>`ucpa_usa`</li><li>`vcdpa_usa`</li></ul><br> 上記の値が表すプライバシー規制について詳しくは、[ サポートされる規制 ](../regulations/overview.md) の概要を参照してください。 |
| `{PAGE}` | 0 を基準とする番号を使用した、表示するデータのページ。デフォルトは `0` です。 |
| `{SIZE}` | 各ページに表示する結果の数。デフォルトは `100` で、最大は `1000` です。最大値を超えると、API は 400 コードエラーを返します。 |
| `{status}` | デフォルトの動作では、すべてのステータスが含まれます。 ステータスタイプを指定すると、リクエストはそのステータスタイプに一致するプライバシージョブのみを返します。 指定できる値は次のとおりです。 <ul><li>`processing`</li><li>`complete`</li><li>`error`</li></ul> |
| `{toDate}` | このパラメーターを指定すると、指定した日付より前に処理された結果に制限されます。 リクエストの日から、システムは 45 日間振り返ることができます。 ただし、範囲は 30 日を超えることはできません。<br>YYYY-MM-DD の形式を使用できます。 指定した日付は、グリニッジ標準時（GMT）で表された終了日と解釈されます。<br> このパラメーター（および対応する `fromDate`）を指定しない場合、デフォルトの動作では、過去 7 日間にそのデータを返すジョブが返されます。 `toDate` を使用する場合、`fromDate` クエリパラメーターも使用する必要があります。 両方を使用しない場合、呼び出しは 400 エラーを返します。 |
| `{fromDate}` | このパラメーターを使用すると、指定した日付以降に処理された結果に制限されます。 リクエストの日から、システムは 45 日間振り返ることができます。 ただし、範囲は 30 日を超えることはできません。<br>YYYY-MM-DD の形式を使用できます。 指定した日付は、リクエストの原産日としてグリニッジ標準時（GMT）で表されたものと解釈されます。<br> このパラメーター（および対応する `toDate`）を指定しない場合、デフォルトの動作では、過去 7 日間にそのデータを返すジョブが返されます。 `fromDate` を使用する場合、`toDate` クエリパラメーターも使用する必要があります。 両方を使用しない場合、呼び出しは 400 エラーを返します。 |
| `{filterDate}` | このパラメーターを使用すると、結果を指定した日付に処理された結果に制限できます。 YYYY-MM-DD の形式を使用できます。 システムは過去 45 日間を振り返ることができます。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストは、ページ サイズが 50 の 3 番目のページから、組織内のすべてのジョブのページ分割されたリストを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/privacy/jobs?regulation=gdpr&page=2&size=50 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}'
```

**応答** 

正常な応答は、ジョブのリストを返し、各ジョブに `jobId` などの詳細などが含まれます。この例では、結果の 3 ページ目から始まる 50 個のリストのジョブが応答に含まれます。

### 後続のページへのアクセス

ページ分割された応答の次の結果セットを取得するには、`page` クエリパラメーターを 1 増やして、同じエンドポイントに対して別の API 呼び出しをおこないます。

## プライバシージョブの作成 {#create-job}

>[!IMPORTANT]
>
>Privacy Service は、データ主体と消費者の権利リクエストのみを目的としています。それ以外にデータのクリーンアップやメンテナンスに Privacy Service を使用することは、サポートされておらず、許可もされていません。アドビには、それらをタイムリーに履行する法的義務があります。したがって、これが実稼動専用の環境であり、有効なプライバシーリクエストの不要なバックログが作成されるので、Privacy Service の読み込みテストは許可されていません。
>
>サービスの不正使用を防ぐために、1 日あたりのアップロードに対するハードリミットが設定されるようになりました。システムの不正使用が判明したユーザーは、サービスへのアクセスが無効になります。その後、それらのユーザーのアクションに対処するための会議がユーザー本人を交えて開催され、Privacy Service の適切な使用について議論が行われます。

新しいジョブリクエストを作成する前に、まず、データにアクセス、削除、またはオプトアウトするデータ主体の識別情報を収集する必要があります。必要なデータを取得したら、`/jobs` エンドポイントへのPOSTリクエストのペイロードで指定する必要があります。

>[!NOTE]
>
>互換性のあるAdobe Experience Cloud アプリケーションは、データ主体の識別に異なる値を使用します。 お使いのアプリケーションで必要な ID の詳細については [&#128279;](../experience-cloud-apps.md)Privacy ServiceおよびExperience Cloudアプリケーションに関するガイドを参照してください。 [!DNL Privacy Service] に送信する ID を決定する際の一般的なガイダンスについて詳しくは、[ プライバシーリクエストの ID データ ](../identity-data.md) のドキュメントを参照してください。

[!DNL Privacy Service] API は、個人データに対して、次の 2 種類のジョブリクエストをサポートしています。

* [アクセスや削除](#access-delete)：個人データにアクセス（読み取り）または削除します。
* [販売オプトアウト](#opt-out)：個人データを販売しないものとして指定します。

>[!IMPORTANT]
>
>アクセスリクエストと削除リクエストは 1 つの API 呼び出しとして組み合わせることができますが、オプトアウトリクエストは個別に行う必要があります。

### アクセス/削除ジョブの作成 {#access-delete}

この節では、API を使用してアクセスおよび削除ジョブリクエストを作成する方法を説明します。

**API 形式**

```http
POST /jobs
```

**リクエスト**

次のリクエストは、以下で説明するように、ペイロードで提供される属性によって構成された新しいジョブリクエストを作成します。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/privacy/jobs \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -d '{
    "companyContexts": [
      {
        "namespace": "imsOrgID",
        "value": "{ORG_ID}"
      }
    ],
    "users": [
      {
        "key": "DavidSmith",
        "action": ["access"],
        "userIDs": [
          {
            "namespace": "email",
            "value": "dsmith@acme.com",
            "type": "standard"
          },
          {
            "namespace": "ECID",
            "type": "standard",
            "value":  "443636576799758681021090721276",
            "isDeletedClientSide": false
          }
        ]
      },
      {
        "key": "user12345",
        "action": ["access","delete"],
        "userIDs": [
          {
            "namespace": "email",
            "value": "ajones@acme.com",
            "type": "standard"
          },
          {
            "namespace": "loyaltyAccount",
            "value": "12AD45FE30R29",
            "type": "integrationCode"
          }
        ]
      }
    ],
    "include": ["Analytics", "AudienceManager","profileService"],
    "expandIds": false,
    "priority": "normal",
    "mergePolicyId": 124,
    "regulation": "ccpa"
}'
```

| プロパティ | 説明 |
| --- | --- |
| `companyContexts` **(必須)** | 組織の認証情報を含む配列。リストに表示される各識別子には、次の属性が含まれます。 <ul><li>`namespace`：識別子の名前空間。</li><li>`value`：識別子の値。</li></ul>識別子の 1 つが `namespace` として `imsOrgId` を使用し、その `value` には組織の一意の ID が含まれている **必須**。 <br/><br/>追加の識別子は、組織に属するアドビ会社との統合を識別する、製品固有の会社修飾子（例：`Campaign`）にすることができます。有効な値には、アカウント名、クライアントコード、テナント ID、その他のアプリケーション識別子が含まれます。 |
| `users` **(必須)** | アクセスまたは削除する情報を持つユーザーの少なくとも 1 人のコレクションを含む配列。1 回のリクエストで最大 1,000 人のユーザーを指定できます。 各ユーザーオブジェクトには、次の情報が含まれます。 <ul><li>`key`：応答データ内の個別のジョブ ID を修飾するために使用されるユーザーの識別子。この値に対して一意の、簡単に識別できる文字列を選択し、後で簡単に参照または参照できるようにすることをお勧めします。</li><li>`action`：ユーザーのデータに対して実行する必要のあるアクションをリストする配列。実行するアクションに応じて、この配列には `access`、`delete` またはその両方を含める必要があります。</li><li>`userIDs`：ユーザーの ID のコレクションです。1 人のユーザーが持つことのできる ID の数は 9 個に制限されます。各 ID は`namespace`、`value`、および名前空間修飾子（`type`）で構成されます。これらの必須プロパティの詳細については、[付録](appendix.md)を参照してください。</li></ul> `users` と `userIDs` の詳細については、[トラブルシューティングガイド](../troubleshooting-guide.md#user-ids)を参照してください。 |
| `include` **(必須)** | 処理に含めるアドビ製品の配列。この値がない場合や空の場合、リクエストは拒否されます。組織が統合している製品のみを含めます。詳しくは、付録の「[受け入れられる製品値](appendix.md)」の節を参照してください。 |
| `expandIDs` | オプションのプロパティで、`true` に設定すると、アプリケーションの ID を処理するための最適化を表します（現在は [!DNL Analytics] でのみサポートされています）。 省略した場合、この値はデフォルトで `false` になります。 |
| `priority` | リクエストの処理の優先度を設定する、Adobe Analytics で使用されるオプションのプロパティです。指定できる値は、`normal` および `low` です。`priority` を省略した場合のデフォルトの動作は `normal` です。 |
| `mergePolicyId` | リアルタイム顧客プロファイル（`profileService`）のプライバシーリクエストを行う際に、ID のステッチに使用する特定の [ 結合ポリシー ](../../profile/merge-policies/overview.md) の ID をオプションで指定できます。 結合ポリシーを指定すると、顧客に関するデータを返す際に、プライバシーリクエストにオーディエンス情報を含めることができます。 リクエストごとに指定できる結合ポリシーは 1 つだけです。 結合ポリシーが指定されていない場合、セグメント化情報は応答に含まれません。 |
| `regulation` **(必須)** | プライバシージョブの規制。 以下の値を使用できます。 <ul><li>`apa_aus`</li><li>`ccpa`</li><li>`cpra_usa`</li><li>`gdpr`</li><li>`hipaa_usa`</li><li>`lgpd_bra`</li><li>`nzpa_nzl`</li><li>`pdpa_tha`</li><li>`vcdpa_usa`</li></ul><br> 上記の値が表すプライバシー規制について詳しくは、[ サポートされる規制 ](../regulations/overview.md) の概要を参照してください。 |

{style="table-layout:auto"}

**応答** 

正常な応答は、新しく作成されたジョブの詳細を返します。

```json
{
    "jobs": [
        {
            "jobId": "6fc09b53-c24f-4a6c-9ca2-c6076b0842b6",
            "customer": {
                "user": {
                    "key": "DavidSmith",
                    "action": [
                        "access"
                    ]
                }
            }
        },
        {
            "jobId": "6fc09b53-c24f-4a6c-9ca2-c6076be029f3",
            "customer": {
                "user": {
                    "key": "user12345",
                    "action": [
                        "access"
                    ]
                }
            }
        },
        {
            "jobId": "6fc09b53-c24f-4a6c-9ca2-c6076bd023j1",
            "customer": {
                "user": {
                    "key": "user12345",
                    "action": [
                        "delete"
                    ]
                }
            }
        }
    ],
    "requestStatus": 1,
    "totalRecords": 3
}
```

| プロパティ | 説明 |
| --- | --- |
| `jobId` | ジョブの読み取り専用の、一意のシステム生成 ID。この値は、特定のジョブを検索する次の手順で使用されます。 |

{style="table-layout:auto"}

ジョブリクエストの送信が完了したら、次の、[ジョブのステータスを確認する](#check-status)手順に進むことができます。

## ジョブのステータスの確認 {#check-status}

`/jobs` エンドポイントに対するGETリクエストのパスにジョブの `jobId` を含めることで、特定のジョブに関する情報（現在の処理ステータスなど）を取得できます。

>[!IMPORTANT]
>
>以前に作成したジョブのデータは、ジョブの完了日から 30 日以内にのみ取得できます。

**API 形式**

```http
GET /jobs/{JOB_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{JOB_ID}` | 検索するジョブの ID。 この ID は、成功した API 応答で [ ジョブの作成 ](#create-job) および [ すべてのジョブのリスト ](#list) の `jobId` に返されます。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストは、リクエストパスで `jobId` が指定されたジョブの詳細を取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/privacy/jobs/6fc09b53-c24f-4a6c-9ca2-c6076b0842b6 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}'
```

**応答** 

正常な応答は、指定されたジョブの詳細を返します。

```json
{
    "jobId": "6fc09b53-c24f-4a6c-9ca2-c6076b0842b6",
    "requestId": "15700479082313109RX-899",
    "userKey": "David Smith",
    "action": "access",
    "status": "complete",
    "submittedBy": "{ACCOUNT_ID}",
    "createdDate": "10/02/2019 08:25 PM GMT",
    "lastModifiedDate": "10/02/2019 08:25 PM GMT",
    "userIds": [
        {
            "namespace": "email",
            "value": "dsmith@acme.com",
            "type": "standard",
            "namespaceId": 6,
            "isDeletedClientSide": false
        },
        {
            "namespace": "ECID",
            "value": "1123A4D5690B32A",
            "type": "standard",
            "namespaceId": 4,
            "isDeletedClientSide": false
        }
    ],
    "productResponses": [
        {
            "product": "Analytics",
            "retryCount": 0,
            "processedDate": "10/02/2019 08:25 PM GMT",
            "productStatusResponse": {
                "status": "complete",
                "message": "Success",
                "responseMsgCode": "PRVCY-6000-200",
                "responseMsgDetail": "Finished successfully."
            }
        },
        {
            "product": "Profile",
            "retryCount": 0,
            "processedDate": "10/02/2019 08:25 PM GMT",
            "productStatusResponse": {
                "status": "complete",
                "message": "Success",
                "responseMsgCode": "PRVCY-6000-200",
                "responseMsgDetail": "Success dataSetIds = [5dbb87aad37beb18a96feb61], Failed dataSetIds = []"
            }
        },
        {
            "product": "AudienceManager",
            "retryCount": 0,
            "processedDate": "10/02/2019 08:25 PM GMT",
            "productStatusResponse": {
                "status": "complete",
                "message": "Success",
                "responseMsgCode": "PRVCY-6054-200",
                "responseMsgDetail": "PARTIALLY COMPLETED- Data not found for some requests, check results for more info.",
                "results": {
                  "processed": ["1123A4D5690B32A"],
                  "ignored": ["dsmith@acme.com"]
                }
            }
        }
    ],
    "downloadURL": "http://...",
    "regulation": "ccpa"
}
```

| プロパティ | 説明 |
| --- | --- |
| `productStatusResponse` | `productResponses` 配列内の各オブジェクトには、特定の [!DNL Experience Cloud] アプリケーションに関するジョブの現在のステータスに関する情報が含まれます。 |
| `productStatusResponse.status` | ジョブの現在の状態カテゴリ。 [ 利用可能なステータスカテゴリ ](#status-categories) のリストと対応する意味については、次の表を参照してください。 |
| `productStatusResponse.message` | ステータスカテゴリに対応するジョブの特定のステータス。 |
| `productStatusResponse.responseMsgCode` | [!DNL Privacy Service] が受信する製品応答メッセージの標準コード。 メッセージの詳細は、`responseMsgDetail` に記載されています。 |
| `productStatusResponse.responseMsgDetail` | ジョブのステータスのより詳細な説明。 同様のステータスのメッセージは、製品間で異なる場合があります。 |
| `productStatusResponse.results` | 特定のステータスについては、`responseMsgDetail` でカバーされない追加情報を提供する `results` オブジェクトを返す製品もあります。 |
| `downloadURL` | ジョブのステータスが `complete` の場合、この属性はジョブの結果を ZIP ファイルとしてダウンロードする URL を指定します。このファイルは、ジョブの完了後 60 日間ダウンロードできます。 |

{style="table-layout:auto"}

### ジョブステータスカテゴリ {#status-categories}

次の表に、考えられる様々なジョブステータスカテゴリとそれに対応する意味を示します。

| 状態カテゴリ | 意味 |
| -------------- | -------- |
| `complete` | ジョブが完了し、（必要に応じて）すべてのアプリケーションからファイルがアップロードされます。 |
| `processing` | アプリケーションはジョブを確認し、現在処理中です。 |
| `submitted` | ジョブは、該当するすべてのアプリケーションに送信されます。 |
| `error` | ジョブを処理できませんでした。個々のジョブの詳細を取得することで、より具体的な情報を取得できます。 |

{style="table-layout:auto"}

>[!NOTE]
>
>処理中の依存する子ジョブがある場合、送信されたジョブは `processing` 状態のままになることがあります。

## 次の手順

これで、[!DNL Privacy Service] API を使用してプライバシージョブを作成および監視する方法がわかりました。 ユーザーインターフェイスを使用して同じタスクを実行する方法について詳しくは、「[Privacy Service UI の概要](../ui/overview.md)」を参照してください。
