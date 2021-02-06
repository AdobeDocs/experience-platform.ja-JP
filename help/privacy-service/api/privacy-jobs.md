---
keywords: Experience Platform；ホーム；人気の高いトピック
solution: Experience Platform
title: プライバシージョブAPIエンドポイント
topic: developer guide
description: Privacy ServiceAPIを使用してExperience Cloudアプリのプライバシージョブを管理する方法について説明します。
translation-type: tm+mt
source-git-commit: 698639d6c2f7897f0eb4cce2a1f265a0f7bb57c9
workflow-type: tm+mt
source-wordcount: '1344'
ht-degree: 66%

---


# プライバシージョブエンドポイント

このドキュメントでは、API呼び出しを使用してプライバシージョブを操作する方法について説明します。 特に、[!DNL Privacy Service] APIでの`/job`エンドポイントの使用をカバーしています。 このガイドを読む前に、[はじめに](./getting-started.md#getting-started)を参照し、必要なヘッダーやAPI呼び出し例の読み方など、APIを正しく呼び出すために必要な重要な情報を確認してください。

>[!NOTE]
>
>お客様からの同意またはオプトアウト要求を管理する場合は、[同意エンドポイントガイド](./consent.md)を参照してください。

## すべてのジョッブをリスト {#list}

`/jobs`エンドポイントにGETリクエストを行うことで、組織内で使用可能なすべてのプライバシージョブのリストを表示できます。

**API 形式**

このリクエスト形式では、`/jobs`エンドポイントに`regulation`クエリパラメーターが使用されているので、次に示す疑問符(`?`)で始まります。 応答はページ分割され、他のクエリパラメーター（`page` および `size`）を使用して応答をフィルターできます。アンパサンド（`&`）を使用して、複数のパラメーターを区切ることができます。

```http
GET /jobs?regulation={REGULATION}
GET /jobs?regulation={REGULATION}&page={PAGE}
GET /jobs?regulation={REGULATION}&size={SIZE}
GET /jobs?regulation={REGULATION}&page={PAGE}&size={SIZE}
```

| パラメーター | 説明 |
| --- | --- |
| `{REGULATION}` | クエリする規制の種類。次の値を指定できます。 <ul><li>`gdpr` (ヨーロッパ和集合)</li><li>`ccpa` （カリフォルニア）</li><li>`lgpd_bra` （ブラジル）</li><li>`nzpa_nzl` (New Zealand)</li><li>`pdpa_tha` （タイ）</li></ul> |
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

新しいジョブリクエストを作成する前に、まず、データにアクセス、削除、またはオプトアウトするデータ主体の識別情報を収集する必要があります。必要なデータを取得したら、`/jobs`エンドポイントへのPOST要求のペイロードで指定する必要があります。

>[!NOTE]
>
> 互換性のある Adobe Experience Cloud アプリケーションは、データの主題を識別するために異なる値を使用します。アプリに必要な識別子について詳しくは、[Privacy Service および Experience Cloud アプリケーションの](../experience-cloud-apps.md)に関するガイドを参照してください。[!DNL Privacy Service]に送信するIDの決定に関する一般的なガイダンスについては、](../identity-data.md)のプライバシー要求[のIDデータのドキュメントを参照してください。

[!DNL Privacy Service] APIは、個人データに対する2種類のジョブリクエストをサポートしています。

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
| `expandIDs` | `true`に設定した場合に、アプリケーション内のIDを処理するための最適化を表すオプションのプロパティです（現在、[!DNL Analytics]でのみサポートされています）。 省略した場合、この値はデフォルトで `false` になります。 |
| `priority` | リクエストの処理の優先度を設定する、Adobe Analytics で使用されるオプションのプロパティです。指定できる値は、`normal` および `low` です。`priority` を省略した場合のデフォルトの動作は `normal` です。 |
| `analyticsDeleteMethod` | Adobe Analytics での個人データの処理方法を指定するオプションのプロパティです。この属性には、次の 2 つの値を指定できます。 <ul><li>`anonymize`：特定のユーザー ID のコレクションによって参照されるすべてのデータは匿名になります。`analyticsDeleteMethod` を省略した場合のデフォルトの動作です。</li><li>`purge`：すべてのデータが完全に削除されます。</li></ul> |
| `regulation` **(必須)** | プライバシー業務に関する規則。 次の値を指定できます。 <ul><li>`gdpr` (ヨーロッパ和集合)</li><li>`ccpa` （カリフォルニア）</li><li>`lgpd_bra` （ブラジル）</li><li>`nzpa_nzl` （ニュージーランド）</li><li>`pdpa_tha` （タイ）</li></ul> |

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

## ジョブのステータスの確認 {#check-status}

`/jobs`エンドポイントへのGET要求のパスに、現在の処理ステータスなど、特定のジョブに関する情報を取得できます。`jobId`

>[!IMPORTANT]
>
> 以前に作成したジョブのデータは、ジョブの完了日から 30 日間のみ取得できます。

**API 形式**

```http
GET /jobs/{JOB_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{JOB_ID}` | 検索するジョブのID。 [ジョブ](#create-job)および[すべてのジョブ](#list)をリストするジョブとの作成に成功したAPI応答で、`jobId`の下にこのIDが返されます。 |

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
| `productStatusResponse` | `productResponses`配列内の各オブジェクトには、特定の[!DNL Experience Cloud]アプリケーションに関するジョブの現在の状態に関する情報が含まれます。 |
| `productStatusResponse.status` | ジョブの現在のステータスカテゴリ。 [利用可能なステータスカテゴリ](#status-categories)のリストと、対応する意味については、以下の表を参照してください。 |
| `productStatusResponse.message` | ステータスカテゴリに対応する、ジョブの固有のステータス。 |
| `productStatusResponse.responseMsgCode` | [!DNL Privacy Service]が受信した製品応答メッセージの標準コード。 メッセージの詳細は`responseMsgDetail`の下に表示されます。 |
| `productStatusResponse.responseMsgDetail` | ジョブのステータスに関する詳細な説明。 同様のステータスのメッセージは、製品によって異なる場合があります。 |
| `productStatusResponse.results` | 特定のステータスの場合、一部の製品は`responseMsgDetail`では扱われない追加情報を提供する`results`オブジェクトを返す場合があります。 |
| `downloadURL` | ジョブのステータスが `complete` の場合、この属性はジョブの結果を ZIP ファイルとしてダウンロードする URL を指定します。このファイルは、ジョブの完了後 60 日間ダウンロードできます。 |

### ジョブステータスカテゴリ{#status-categories}

次の表に、様々なジョブステータスカテゴリと、それに対応する意味を示します。

| ステータスカテゴリ | 意味 |
| -------------- | -------- |
| `complete` | ジョブが完了し、（必要に応じて）すべてのアプリケーションからファイルがアップロードされます。 |
| `processing` | アプリケーションはジョブを確認し、現在処理中です。 |
| `submitted` | ジョブは、該当するすべてのアプリケーションに送信されます。 |
| `error` | ジョブを処理できませんでした。個々のジョブの詳細を取得することで、より具体的な情報を取得できます。 |

>[!NOTE]
>
>依存する子ジョブがまだ処理中の場合、送信されたジョブは`processing`状態のままになる可能性があります。

## 次の手順

これで、[!DNL Privacy Service] APIを使用してプライバシージョブを作成および監視する方法がわかりました。 ユーザーインターフェイスを使用して同じタスクを実行する方法について詳しくは、「[Privacy Service UI の概要](../ui/overview.md)」を参照してください。
