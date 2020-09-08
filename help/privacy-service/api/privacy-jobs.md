---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ジョブ
topic: developer guide
translation-type: tm+mt
source-git-commit: e7bb3e8a418631e9220865e49a1651e4dc065daf
workflow-type: tm+mt
source-wordcount: '1782'
ht-degree: 76%

---


# プライバシージョブ

このドキュメントでは、API呼び出しを使用してプライバシージョブを操作する方法について説明します。 特に、 `/job`[!DNL Privacy Service] APIでのエンドポイントの使用について説明します。 Before reading this guide, refer to the [getting started section](./getting-started.md#getting-started) for important information that you need to know in order to successfully make calls to the API, including required headers and how to read example API calls.

## すべてのジョッブをリスト {#list}

You can view a list of all available privacy jobs within your organization by making a GET request to the `/jobs` endpoint.

**API 形式**

This request format uses a `regulation` query parameter on the `/jobs` endpoint, therefore it begins with a question mark (`?`) as shown below. 応答はページ分割され、他のクエリパラメーター（`page` および `size`）を使用して応答をフィルターできます。アンパサンド（`&`）を使用して、複数のパラメーターを区切ることができます。

```http
GET /jobs?regulation={REGULATION}
GET /jobs?regulation={REGULATION}&page={PAGE}
GET /jobs?regulation={REGULATION}&size={SIZE}
GET /jobs?regulation={REGULATION}&page={PAGE}&size={SIZE}
```

| パラメーター | 説明 |
| --- | --- |
| `{REGULATION}` | クエリする規制の種類。Accepted values are `gdpr`, `ccpa`, `lgpd_bra`, and `pdpa_tha`. |
| `{PAGE}` | 0 を基準とする番号を使用した、表示するデータのページ。デフォルトは `0` です。 |
| `{SIZE}` | 各ページに表示する結果の数。デフォルトは `1` で、最大は `100` です。最大値を超えると、API は 400 コードエラーを返します。 |

**リクエスト**

次のリクエストは、ページ 3（ページサイズ 50）から、IMS 組織内のすべてのジョブのページ分割リストを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/privacy/jobs?regulation=gdpr&page=2&size=50 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
```

**応答** 

正常な応答は、ジョブのリストを返し、各ジョブに `jobId` などの詳細などが含まれます。この例では、結果の 3 ページ目から始まる 50 個のリストのジョブが応答に含まれます。

### 後続のページへのアクセス

ページ分割された応答の次の結果セットを取得するには、`page` クエリパラメーターを 1 増やして、同じエンドポイントに対して別の API 呼び出しをおこないます。

## プライバシージョブの作成 {#create-job}

新しいジョブリクエストを作成する前に、まず、データにアクセス、削除、またはオプトアウトするデータ主体の識別情報を収集する必要があります。Once you have the required data, it must be provided in the payload of a POST request to the `/jobs` endpoint.

>[!NOTE]
>
> 互換性のある Adobe Experience Cloud アプリケーションは、データの主題を識別するために異なる値を使用します。アプリに必要な識別子について詳しくは、[Privacy Service および Experience Cloud アプリケーションの](../experience-cloud-apps.md)に関するガイドを参照してください。送信先IDの決定に関する一般的な手順については、プライバシー要求 [!DNL Privacy Service]の [IDデータに関するドキュメントを参照してください](../identity-data.md)。

The [!DNL Privacy Service] API supports two kinds of job requests for personal data:

* [アクセスや削除](#access-delete)：個人データにアクセス（読み取り）または削除します。
* [販売オプトアウト](#opt-out)：個人データを販売しないものとして指定します。

>[!IMPORTANT]
>
> アクセスリクエストと削除リクエストは 1 回の API 呼び出しとして組み合わせることができますが、オプトアウトリクエストは別々におこなう必要があります。

### アクセスおよび削除ジョブの作成 {#access-delete}

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
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -d '{
    "companyContexts": [
      {
        "namespace": "imsOrgID",
        "value": "{IMS_ORG}"
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
    "include": ["Analytics", "AudienceManager"],
    "expandIds": false,
    "priority": "normal",
    "analyticsDeleteMethod": "anonymize",
    "regulation": "ccpa"
}'
```

| プロパティ | 説明 |
| --- | --- |
| `companyContexts` **(必須)** | 組織の認証情報を含む配列。リストに表示される各識別子には、次の属性が含まれます。 <ul><li>`namespace`：識別子の名前空間。</li><li>`value`：識別子の値。</li></ul>識別子の 1 つが `imsOrgId` を `namespace` として使用し、`value` が IMS 組織の一意の ID を含む&#x200B;**必要**&#x200B;があります。<br/><br/>追加の識別子は、組織に属するアドビ会社との統合を識別する、製品固有の会社修飾子（例：`Campaign`）にすることができます。有効な値には、アカウント名、クライアントコード、テナント ID、その他のアプリケーション識別子が含まれます。 |
| `users` **(必須)** | アクセスまたは削除する情報を持つユーザーの少なくとも 1 人のコレクションを含む配列。単一のリクエストで提供できるユーザー ID は、最大 1,000 個です。各ユーザーオブジェクトには、次の情報が含まれます。 <ul><li>`key`：応答データ内の個別のジョブ ID を修飾するために使用されるユーザーの識別子。この値に対して一意の、簡単に識別できる文字列を選択し、後で簡単に参照または参照できるようにすることをお勧めします。</li><li>`action`：ユーザーのデータに対して実行する必要のあるアクションをリストする配列。実行するアクションに応じて、この配列には `access`、`delete` またはその両方を含める必要があります。</li><li>`userIDs`：ユーザーの ID のコレクションです。1 人のユーザーが持つことのできる ID の数は 9 個に制限されます。各 ID は`namespace`、`value`、および名前空間修飾子（`type`）で構成されます。これらの必須プロパティの詳細については、[付録](appendix.md)を参照してください。</li></ul> `users` と `userIDs` の詳細については、[トラブルシューティングガイド](../troubleshooting-guide.md#user-ids)を参照してください。 |
| `include` **(必須)** | 処理に含めるアドビ製品の配列。この値がない場合や空の場合、リクエストは拒否されます。組織が統合している製品のみを含めます。詳しくは、付録の「[受け入れられる製品値](appendix.md)」の節を参照してください。 |
| `expandIDs` | An optional property that, when set to `true`, represents an optimization for processing the IDs in the applications (currently only supported by [!DNL Analytics]). 省略した場合、この値はデフォルトで `false` になります。 |
| `priority` | リクエストの処理の優先度を設定する、Adobe Analytics で使用されるオプションのプロパティです。指定できる値は、`normal` および `low` です。`priority` を省略した場合のデフォルトの動作は `normal` です。 |
| `analyticsDeleteMethod` | Adobe Analytics での個人データの処理方法を指定するオプションのプロパティです。この属性には、次の 2 つの値を指定できます。 <ul><li>`anonymize`：特定のユーザー ID のコレクションによって参照されるすべてのデータは匿名になります。`analyticsDeleteMethod` を省略した場合のデフォルトの動作です。</li><li>`purge`：すべてのデータが完全に削除されます。</li></ul> |
| `regulation` **(必須)** | リクエストの規則。次の4つの値のいずれかにする必要があります。 <ul><li>`gdpr`</li><li>`ccpa`</li><li>`lgpd_bra`</li><li>`pdpa_tha`</li></ul> |

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

ジョブリクエストの送信が完了したら、次の、[ジョブのステータスを確認する](#check-status)手順に進むことができます。

### 販売オプトアウトオブジョブの作成 {#opt-out}

この節では、API を使用して販売オプトアウトジョブリクエストを作成する方法を説明します。

**API 形式**

```http
POST /jobs
```

**リクエスト**

次のリクエストは、以下で説明するように、ペイロードで提供される属性によって構成された新しいジョブリクエストを作成します。

```shell
curl -X POST \
  https://platform.adobe.io/data/privacy/gdpr/ \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -d '{
    "companyContexts": [
      {
        "namespace": "imsOrgID",
        "value": "{IMS_ORG}"
      }
    ],
    "users": [
      {
        "key": "DavidSmith",
        "action": ["opt-out-of-sale"],
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
        "action": ["opt-out-of-sale"],
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
    "include": ["Analytics", "AudienceManager"],
    "expandIds": false,
    "priority": "normal",
    "analyticsDeleteMethod": "anonymize",
    "regulation": "ccpa"
}'
```

| プロパティ | 説明 |
| --- | --- |
| `companyContexts` **(必須)** | 組織の認証情報を含む配列。リストに表示される各識別子には、次の属性が含まれます。 <ul><li>`namespace`：識別子の名前空間。</li><li>`value`：識別子の値。</li></ul>識別子の 1 つが `imsOrgId` を `namespace` として使用し、`value` が IMS 組織の一意の ID を含む&#x200B;**必要**&#x200B;があります。<br/><br/>追加の識別子は、組織に属するアドビ会社との統合を識別する、製品固有の会社修飾子（例：`Campaign`）にすることができます。有効な値には、アカウント名、クライアントコード、テナント ID、その他のアプリケーション識別子が含まれます。 |
| `users` **(必須)** | アクセスまたは削除する情報を持つユーザーの少なくとも 1 人のコレクションを含む配列。単一のリクエストで提供できるユーザー ID は、最大 1,000 個です。各ユーザーオブジェクトには、次の情報が含まれます。 <ul><li>`key`：応答データ内の個別のジョブ ID を修飾するために使用されるユーザーの識別子。この値に対して一意の、簡単に識別できる文字列を選択し、後で簡単に参照または参照できるようにすることをお勧めします。</li><li>`action`：データに対してリストがおこなう必要のあるアクションを指定する配列。販売オプトアウトリクエストの場合、配列には `opt-out-of-sale` 値のみを含める必要があります 。</li><li>`userIDs`：ユーザーの ID のコレクションです。1 人のユーザーが持つことのできる ID の数は 9 個に制限されます。各 ID は`namespace`、`value`、および名前空間修飾子（`type`）で構成されます。これらの必須プロパティの詳細については、[付録](appendix.md)を参照してください。</li></ul> `users` と `userIDs` の詳細については、[トラブルシューティングガイド](../troubleshooting-guide.md#user-ids)を参照してください。 |
| `include` **(必須)** | 処理に含めるアドビ製品の配列。この値がない場合や空の場合、リクエストは拒否されます。組織が統合している製品のみを含めます。詳しくは、付録の「[受け入れられる製品値](appendix.md)」の節を参照してください。 |
| `expandIDs` | An optional property that, when set to `true`, represents an optimization for processing the IDs in the applications (currently only supported by [!DNL Analytics]). 省略した場合、この値はデフォルトで `false` になります。 |
| `priority` | リクエストの処理の優先度を設定する、Adobe Analytics で使用されるオプションのプロパティです。指定できる値は、`normal` および `low` です。`priority` を省略した場合のデフォルトの動作は `normal` です。 |
| `analyticsDeleteMethod` | Adobe Analytics での個人データの処理方法を指定するオプションのプロパティです。この属性には、次の 2 つの値を指定できます。 <ul><li>`anonymize`：特定のユーザー ID のコレクションによって参照されるすべてのデータは匿名になります。`analyticsDeleteMethod` を省略した場合のデフォルトの動作です。</li><li>`purge`：すべてのデータが完全に削除されます。</li></ul> |
| `regulation` **(必須)** | リクエストの規則。次の4つの値のいずれかにする必要があります。 <ul><li>`gdpr`</li><li>`ccpa`</li><li>`lgpd_bra`</li><li>`pdpa_tha`</li></ul> |

**応答** 

正常な応答は、新しく作成されたジョブの詳細を返します。

```json
{
    "jobs": [
        {
            "jobId": "6fc09b53-c24f-4a6c-9ca2-c6076bd9vjs0",
            "customer": {
                "user": {
                    "key": "DavidSmith",
                    "action": [
                        "opt-out-of-sale"
                    ]
                }
            }
        },
        {
            "jobId": "6fc09b53-c24f-4a6c-9ca2-c6076bes0ewj2",
            "customer": {
                "user": {
                    "key": "user12345",
                    "action": [
                        "opt-out-of-sale"
                    ]
                }
            }
        }
    ],
    "requestStatus": 1,
    "totalRecords": 2
}
```

| プロパティ | 説明 |
| --- | --- |
| `jobId` | ジョブの読み取り専用の、一意のシステム生成 ID。この値は、次の手順で特定のジョブを検索するために使用されます。 |

ジョブリクエストの送信が正常に完了したら、ジョブのステータスを確認する次の手順に進むことができます。

## ジョブのステータスの確認 {#check-status}

エンドポイントへのGET要求のパスにそのジョブを含めると、そのジョブに関する情報（現在の処理ステータスなど） `jobId` を取得でき `/jobs` ます。

>[!IMPORTANT]
>
> 以前に作成したジョブのデータは、ジョブの完了日から 30 日間のみ取得できます。

**API 形式**

```http
GET /jobs/{JOB_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{JOB_ID}` | 検索するジョブのID。 このIDは、ジョブ `jobId` の [作成とすべてのジョブの](#create-job) 一覧表示に成功したAPI応答で返されます [](#list)。 |

**リクエスト**

次のリクエストは、リクエストパスで `jobId` が指定されたジョブの詳細を取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/privacy/jobs/6fc09b53-c24f-4a6c-9ca2-c6076b0842b6 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
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
| `productStatusResponse` | 配列内の各オブジェクトには、特定のアプリケーションに関するジョブの現在の状態に関する情報が含まれ `productResponses`[!DNL Experience Cloud] ます。 |
| `productStatusResponse.status` | ジョブの現在のステータスカテゴリ。 次の表に、 [使用可能なステータスカテゴリのリスト](#status-categories) 、および対応する意味を示します。 |
| `productStatusResponse.message` | ステータスカテゴリに対応する、ジョブの固有のステータス。 |
| `productStatusResponse.responseMsgCode` | が受信した製品応答メッセージの標準コード [!DNL Privacy Service]です。 メッセージの詳細は、に示し `responseMsgDetail`ます。 |
| `productStatusResponse.responseMsgDetail` | ジョブのステータスに関する詳細な説明。 同様のステータスのメッセージは、製品によって異なる場合があります。 |
| `productStatusResponse.results` | 特定のステータスの場合、一部の製品は、で扱われない追加情報を提供する `results` オブジェクトを返す場合があり `responseMsgDetail`ます。 |
| `downloadURL` | ジョブのステータスが `complete` の場合、この属性はジョブの結果を ZIP ファイルとしてダウンロードする URL を指定します。このファイルは、ジョブの完了後 60 日間ダウンロードできます。 |

### ジョブステータスカテゴリ {#status-categories}

次の表に、様々なジョブステータスカテゴリと、それに対応する意味を示します。

| ステータスカテゴリ | 意味 |
| -------------- | -------- |
| `complete` | ジョブが完了し、（必要に応じて）すべてのアプリケーションからファイルがアップロードされます。 |
| `processing` | アプリケーションはジョブを確認し、現在処理中です。 |
| `submitted` | ジョブは、該当するすべてのアプリケーションに送信されます。 |
| `error` | ジョブを処理できませんでした。個々のジョブの詳細を取得することで、より具体的な情報を取得できます。 |

>[!NOTE]
>
>A submitted job might remain in a `processing` state if it has a dependent child job that is still processing.

## 次の手順

You now know how to create and monitor privacy jobs using the [!DNL Privacy Service] API. ユーザーインターフェイスを使用して同じタスクを実行する方法について詳しくは、「[Privacy Service UI の概要](../ui/overview.md)」を参照してください。
