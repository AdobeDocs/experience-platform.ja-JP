---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: プライバシージョブ API エンドポイント
description: Privacy ServiceAPI を使用して、Experience Cloudアプリケーションのプライバシージョブを管理する方法について説明します。
exl-id: 74a45f29-ae08-496c-aa54-b71779eaeeae
source-git-commit: 21347074ed6160511888d4b543133dfd1ec4d35c
workflow-type: tm+mt
source-wordcount: '1549'
ht-degree: 58%

---

# プライバシージョブエンドポイント

このドキュメントでは、API 呼び出しを使用してプライバシージョブを操作する方法について説明します。 特に、 `/job` エンドポイント [!DNL Privacy Service] API このガイドを読む前に、 [入門ガイド](./getting-started.md) を参照してください。

>[!NOTE]
>
>顧客からの同意またはオプトアウトリクエストを管理する場合は、 [同意エンドポイントガイド](./consent.md).

## すべてのジョッブをリスト {#list}

に対してGETリクエストをおこなうことで、組織内で使用可能なすべてのプライバシージョブのリストを表示できます `/jobs` endpoint.

**API 形式**

このリクエストの形式では、 `regulation` クエリパラメーター `/jobs` エンドポイントの場合、疑問符 (`?`) を使用します。 応答はページ分割され、他のクエリパラメーター（`page` および `size`）を使用して応答をフィルターできます。アンパサンド（`&`）を使用して、複数のパラメーターを区切ることができます。

```http
GET /jobs?regulation={REGULATION}
GET /jobs?regulation={REGULATION}&page={PAGE}
GET /jobs?regulation={REGULATION}&size={SIZE}
GET /jobs?regulation={REGULATION}&page={PAGE}&size={SIZE}
```

| パラメーター | 説明 |
| --- | --- |
| `{REGULATION}` | クエリする規制の種類。指定できる値は次のとおりです。 <ul><li>`apa_aus`</li><li>`ccpa`</li><li>`cpra_usa`</li><li>`gdpr`</li><li>`hipaa_usa`</li><li>`lgpd_bra`</li><li>`nzpa_nzl`</li><li>`pdpa_tha`</li><li>`vcdpa_usa`</li></ul><br>概要については、 [サポート規制](../regulations/overview.md) 上記の値が表すプライバシー規制に関する詳細。 |
| `{PAGE}` | 0 を基準とする番号を使用した、表示するデータのページ。デフォルトは `0` です。 |
| `{SIZE}` | 各ページに表示する結果の数。デフォルトは `1` で、最大は `100` です。最大値を超えると、API は 400 コードエラーを返します。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストは、ページ 3（ページサイズ 50）から、IMS 組織内のすべてのジョブのページ分割リストを取得します。

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
>Privacy Serviceは、データ主体および消費者の権利に関するリクエストに対してのみ使用されます。 データのクリーンアップやメンテナンスにPrivacy Serviceを使用する方法は、サポートされていないか、許可されていません。 Adobeは、適時にそれを果たす法的義務を負う。 したがって、Privacy Serviceの読み込みテストは実稼動環境のみであり、有効なプライバシーリクエストの不要なバックログを作成するので、許可されません。
>
>毎日のハードアップロード制限が設定され、サービスの不正使用を防ぐことができるようになりました。 システムを悪用したユーザーは、サービスへのアクセスを無効にします。 その後、彼らと共に、彼らの行動に対処し、Privacy Serviceの許容可能な使用について話し合うための会合が開かれる。

新しいジョブリクエストを作成する前に、まず、データにアクセス、削除、またはオプトアウトするデータ主体の識別情報を収集する必要があります。必要なデータを取得したら、そのデータを、 `/jobs` endpoint.

>[!NOTE]
>
> 互換性のある Adobe Experience Cloud アプリケーションは、データの主題を識別するために異なる値を使用します。アプリに必要な識別子について詳しくは、[Privacy Service および Experience Cloud アプリケーションの](../experience-cloud-apps.md)に関するガイドを参照してください。送信先 ID の決定に関する一般的なガイダンスについて [!DNL Privacy Service]を参照し、 [プライバシーリクエストの id データ](../identity-data.md).

この [!DNL Privacy Service] API は、個人データに対する 2 種類のジョブリクエストをサポートしています。

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
    "analyticsDeleteMethod": "anonymize",
    "mergePolicyId": 124,
    "regulation": "ccpa"
}'
```

| プロパティ | 説明 |
| --- | --- |
| `companyContexts` **(必須)** | 組織の認証情報を含む配列。リストに表示される各識別子には、次の属性が含まれます。 <ul><li>`namespace`：識別子の名前空間。</li><li>`value`：識別子の値。</li></ul>識別子の 1 つが `imsOrgId` を `namespace` として使用し、`value` が IMS 組織の一意の ID を含む&#x200B;**必要**&#x200B;があります。<br/><br/>追加の識別子は、組織に属するアドビ会社との統合を識別する、製品固有の会社修飾子（例：`Campaign`）にすることができます。有効な値には、アカウント名、クライアントコード、テナント ID、その他のアプリケーション識別子が含まれます。 |
| `users` **(必須)** | アクセスまたは削除する情報を持つユーザーの少なくとも 1 人のコレクションを含む配列。単一のリクエストで提供できるユーザー ID は、最大 1,000 個です。各ユーザーオブジェクトには、次の情報が含まれます。 <ul><li>`key`：応答データ内の個別のジョブ ID を修飾するために使用されるユーザーの識別子。この値に対して一意の、簡単に識別できる文字列を選択し、後で簡単に参照または参照できるようにすることをお勧めします。</li><li>`action`：ユーザーのデータに対して実行する必要のあるアクションをリストする配列。実行するアクションに応じて、この配列には `access`、`delete` またはその両方を含める必要があります。</li><li>`userIDs`：ユーザーの ID のコレクションです。1 人のユーザーが持つことのできる ID の数は 9 個に制限されます。各 ID は`namespace`、`value`、および名前空間修飾子（`type`）で構成されます。これらの必須プロパティの詳細については、[付録](appendix.md)を参照してください。</li></ul> `users` と `userIDs` の詳細については、[トラブルシューティングガイド](../troubleshooting-guide.md#user-ids)を参照してください。 |
| `include` **(必須)** | 処理に含めるアドビ製品の配列。この値がない場合や空の場合、リクエストは拒否されます。組織が統合している製品のみを含めます。詳しくは、付録の「[受け入れられる製品値](appendix.md)」の節を参照してください。 |
| `expandIDs` | に設定した場合のオプションのプロパティです。 `true`は、アプリケーションで ID を処理するための最適化を表します ( 現在、は [!DNL Analytics]) をクリックします。 省略した場合、この値はデフォルトで `false` になります。 |
| `priority` | リクエストの処理の優先度を設定する、Adobe Analytics で使用されるオプションのプロパティです。指定できる値は、`normal` および `low` です。`priority` を省略した場合のデフォルトの動作は `normal` です。 |
| `analyticsDeleteMethod` | Adobe Analytics での個人データの処理方法を指定するオプションのプロパティです。この属性には、次の 2 つの値を指定できます。 <ul><li>`anonymize`：特定のユーザー ID のコレクションによって参照されるすべてのデータは匿名になります。`analyticsDeleteMethod` を省略した場合のデフォルトの動作です。</li><li>`purge`：すべてのデータが完全に削除されます。</li></ul> |
| `mergePolicyId` | リアルタイム顧客プロファイル (`profileService`) を使用する場合は、必要に応じて特定の [結合ポリシー](../../profile/merge-policies/overview.md) ID ステッチに使用する 結合ポリシーを指定すると、プライバシーリクエストには、顧客のデータを返す際にセグメント情報を含めることができます。 1 回のリクエストにつき、1 つの結合ポリシーのみ指定できます。 結合ポリシーが指定されていない場合、セグメント化情報は応答に含まれません。 |
| `regulation` **(必須)** | プライバシージョブの規則です。 次の値を使用できます。 <ul><li>`apa_aus`</li><li>`ccpa`</li><li>`cpra_usa`</li><li>`gdpr`</li><li>`hipaa_usa`</li><li>`lgpd_bra`</li><li>`nzpa_nzl`</li><li>`pdpa_tha`</li><li>`vcdpa_usa`</li></ul><br>概要については、 [サポート規制](../regulations/overview.md) 上記の値が表すプライバシー規制に関する詳細。 |

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

特定のジョブの `jobId` を `/jobs` endpoint.

>[!IMPORTANT]
>
> 以前に作成したジョブのデータは、ジョブの完了日から 30 日間のみ取得できます。

**API 形式**

```http
GET /jobs/{JOB_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{JOB_ID}` | 検索するジョブの ID。 この ID は、以下で返されます。 `jobId` ( [ジョブの作成](#create-job) および [すべてのジョブのリスト](#list). |

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
| `productStatusResponse` | 各オブジェクト ( `productResponses` 配列には、特定の [!DNL Experience Cloud] アプリケーション。 |
| `productStatusResponse.status` | ジョブの現在のステータスカテゴリ。 以下の表で [利用可能なステータスカテゴリ](#status-categories) そしてそれに対応する意味も |
| `productStatusResponse.message` | ステータスカテゴリに対応するジョブの固有のステータス。 |
| `productStatusResponse.responseMsgCode` | で受信した製品応答メッセージの標準コード [!DNL Privacy Service]. メッセージの詳細は、次の場所に表示されます。 `responseMsgDetail`. |
| `productStatusResponse.responseMsgDetail` | ジョブのステータスの詳細。 類似のステータスのメッセージは、製品によって異なる場合があります。 |
| `productStatusResponse.results` | 一部の製品では、特定のステータスに対して `results` 次の条件に該当しない追加情報を提供するオブジェクト `responseMsgDetail`. |
| `downloadURL` | ジョブのステータスが `complete` の場合、この属性はジョブの結果を ZIP ファイルとしてダウンロードする URL を指定します。このファイルは、ジョブの完了後 60 日間ダウンロードできます。 |

{style="table-layout:auto"}

### ジョブステータスカテゴリ {#status-categories}

次の表に、様々な可能性のある様々なジョブステータスカテゴリと、それに対応する意味を示します。

| ステータスカテゴリ | 意味 |
| -------------- | -------- |
| `complete` | ジョブが完了し、（必要に応じて）すべてのアプリケーションからファイルがアップロードされます。 |
| `processing` | アプリケーションはジョブを確認し、現在処理中です。 |
| `submitted` | ジョブは、該当するすべてのアプリケーションに送信されます。 |
| `error` | ジョブを処理できませんでした。個々のジョブの詳細を取得することで、より具体的な情報を取得できます。 |

{style="table-layout:auto"}

>[!NOTE]
>
>送信されたジョブが `processing` 依存する子ジョブがまだ処理中の場合は、状態。

## 次の手順

これで、 [!DNL Privacy Service] API ユーザーインターフェイスを使用して同じタスクを実行する方法について詳しくは、「[Privacy Service UI の概要](../ui/overview.md)」を参照してください。
