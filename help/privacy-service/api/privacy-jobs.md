---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ジョブ
topic: developer guide
translation-type: tm+mt
source-git-commit: 5b32c1955fac4f137ba44e8189376c81cdbbfc40
workflow-type: tm+mt
source-wordcount: '1795'
ht-degree: 2%

---


# プライバシージョブ

このドキュメントでは、API呼び出しを使用してプライバシージョブを操作する方法について説明します。 特に、 `/job`[!DNL Privacy Service] APIでのエンドポイントの使用について説明します。 このガイドを読む前に、 [はじめにの節を参照し](./getting-started.md#getting-started) 、必要なヘッダーやAPI呼び出し例の読み方など、APIを正しく呼び出すために必要な重要な情報を確認してください。

## すべてのジョブのリスト {#list}

エンドポイントにGETリクエストを行うことで、組織内で使用可能なすべてのプライバシージョブのリストを表示でき `/jobs` ます。

**API形式**

このリクエストの形式では、エンドポイントの `regulation` クエリパラメーターが使用されるので、 `/jobs` 次に示すように、疑問符(`?`)で始まります。 応答はページ分割され、他のクエリパラメーター(`page` および `size`)を使用して応答をフィルターできます。 アンパサンド(`&`)を使用して、複数のパラメーターを区切ることができます。

```http
GET /jobs?regulation={REGULATION}
GET /jobs?regulation={REGULATION}&page={PAGE}
GET /jobs?regulation={REGULATION}&size={SIZE}
GET /jobs?regulation={REGULATION}&page={PAGE}&size={SIZE}
```

| パラメーター | 説明 |
| --- | --- |
| `{REGULATION}` | クエリする規則のタイプ。 指定できる値は、 `gdpr`、 `ccpa`および `pdpa_tha`です。 |
| `{PAGE}` | 0を基準とするページ番号を使用して、表示するデータのページ。 デフォルトは `0` です。 |
| `{SIZE}` | 各ページに表示する結果の数。 デフォルトはです。最大値 `1` はで `100`す。 最大値を超えると、APIは400コードエラーを返します。 |

**リクエスト**

次のリクエストは、ページサイズが50の3番目のページから、IMS組織内のすべてのジョブのページ分割リストを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/privacy/jobs?regulation=gdpr&page=2&size=50 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
```

**応答**

正常な応答は、ジョブのリストを返します。各ジョブには、ジョブなどの詳細が含まれ `jobId`ます。 この例では、結果の3ページ目から始まる50個のジョブのリストが応答に含まれます。

### 後続のページへのアクセス

ページ分割応答の次の結果セットを取得するには、 `page` クエリパラメーターを1増やしながら、同じエンドポイントに対して別のAPI呼び出しを行う必要があります。

## プライバシージョブの作成 {#create-job}

新しいジョブリクエストを作成する前に、まず、アクセス、削除、または販売するデータを持つデータサブジェクトに関する識別情報を収集する必要があオプトアウトります。 必要なデータを取得したら、エンドポイントへのPOST要求のペイロードで指定する必要があり `/jobs` ます。

>[!NOTE]
>
>互換性のあるAdobe Experience Cloudアプリケーションでは、データの件名を識別するために様々な値が使用されます。 Privacy ServiceおよびExperience Cloudに必要なIDの詳細については、 [アプリケーション](../experience-cloud-apps.md) （複数可）に関するガイドを参照してください。 送信先IDの決定に関する一般的な手順については、プライバシー要求 [!DNL Privacy Service]の [IDデータに関するドキュメントを参照してください](../identity-data.md)。

この [!DNL Privacy Service] APIは、個人データに対する2種類のジョブリクエストをサポートしています。

* [アクセス/削除](#access-delete): 個人データにアクセス（読み取り）または削除します。
* [販売オプトアウト中](#opt-out): 個人データを販売しないものとしてマークします。

>[!IMPORTANT]
>
>アクセス要求と削除要求は1回のAPI呼び出しとして組み合わせることができますが、オプトアウト要求は個別に行う必要があります。

### アクセスジョブまたは削除ジョブの作成 {#access-delete}

この節では、APIを使用してアクセス/削除ジョブリクエストを行う方法を説明します。

**API形式**

```http
POST /jobs
```

**リクエスト**

次のリクエストは、以下に示すように、ペイロードで指定された属性で設定された新しいジョブリクエストを作成します。

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
| `companyContexts` **(必須)** | 組織の認証情報を含む配列。 リストに表示される各識別子には、次の属性が含まれます。 <ul><li>`namespace`: 識別子の名前空間。</li><li>`value`: 識別子の値。</li></ul>識別子の1つがIMS組織の一意のID **を** 含む、その識別子がその識別子として使用されてい `imsOrgId``namespace``value` る必要があります。 <br/><br/>追加の識別子には、製品固有の会社修飾子(例えば `Campaign`)を使用できます。この修飾子は、組織に属するAdobeアプリケーションとの統合を識別します。 有効な値には、アカウント名、クライアントコード、テナントID、その他のアプリケーション識別子が含まれます。 |
| `users` **(必須)** | アクセスまたは削除する情報を持つ少なくとも1人のユーザーのコレクションを含む配列。 1回のリクエストで提供できるユーザーIDは最大1000個です。 各userオブジェクトには、次の情報が含まれます。 <ul><li>`key`: 応答データ内の個別のジョブIDを修飾するために使用されるユーザーの識別子。 ベストプラクティスとして、この値を簡単に参照または後で参照できるように、一意で識別しやすい文字列を選択します。</li><li>`action`: ユーザーのデータに対して行うアクションをリストする配列。 実行するアクションに応じて、この配列には、、、 `access`、またはその両方を含める必要があり `delete`ます。</li><li>`userIDs`: ユーザーのIDの集まり。 1人のユーザーが持つことのできるIDの数は9個に制限されます。 各IDは、 `namespace`、、 `value`および名前空間修飾子(`type`)で構成されます。 必要なプロパティの詳細については、 [付録](appendix.md) を参照してください。</li></ul> との詳細については、『トラブルシューティングガイド』 `users` を参照し `userIDs`てください [](../troubleshooting-guide.md#user-ids)。 |
| `include` **(必須)** | 処理に含めるアドビ製品の配列。 この値がない場合や空の場合は、要求は拒否されます。 組織が統合を持つ製品のみを含めます。 詳細については、付録の [受け入れられる製品値](appendix.md) の節を参照してください。 |
| `expandIDs` | に設定した場合に、アプリケーション内のIDを処理するための最適化を表すオプションのプロパティ `true`です(現在、は、でのみサポートされてい [!DNL Analytics]ます)。 If omitted, this value defaults to `false`. |
| `priority` | リクエストの処理の優先度を設定する、アドビAnalyticsが使用するオプションのプロパティです。 指定できる値は `normal` とで `low`す。 を省略 `priority` した場合のデフォルトの動作はで `normal`す。 |
| `analyticsDeleteMethod` | アドビのAnalyticsでの個人データの処理方法を指定するオプションのプロパティです。 この属性には、次の2つの値を指定できます。 <ul><li>`anonymize`: 特定のユーザーIDのコレクションによって参照されるすべてのデータは匿名になります。 を省略 `analyticsDeleteMethod` した場合の動作はデフォルトです。</li><li>`purge`: すべてのデータが完全に削除されます。</li></ul> |
| `regulation` **(必須)** | 請求の規則 次の3つの値のいずれかにする必要があります。 <ul><li>gdpr</li><li>ccpa</li><li>pdpa_tha</li></ul> |

**応答**

正常に応答すると、新しく作成されたジョブの詳細が返されます。

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
| `jobId` | ジョブに対する、一意で読み取り専用のシステム生成ID。 この値は、特定のジョブを検索する次の手順で使用されます。 |

ジョブリクエストが正常に送信されたら、次のステップに進んで、ジョブのステータス [を確認できます](#check-status)。

### 販売中止のジョブの作成 {#opt-out}

この節では、APIを使用して販売中止のジョブリクエストを作成する方法を説明します。

**API形式**

```http
POST /jobs
```

**リクエスト**

次のリクエストは、以下に示すように、ペイロードで指定された属性で設定された新しいジョブリクエストを作成します。

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
| `companyContexts` **(必須)** | 組織の認証情報を含む配列。 リストに表示される各識別子には、次の属性が含まれます。 <ul><li>`namespace`: 識別子の名前空間。</li><li>`value`: 識別子の値。</li></ul>識別子の1つがIMS組織の一意のID **を** 含む、その識別子がその識別子として使用されてい `imsOrgId``namespace``value` る必要があります。 <br/><br/>追加の識別子には、製品固有の会社修飾子(例えば `Campaign`)を使用できます。この修飾子は、組織に属するAdobeアプリケーションとの統合を識別します。 有効な値には、アカウント名、クライアントコード、テナントID、その他のアプリケーション識別子が含まれます。 |
| `users` **(必須)** | アクセスまたは削除する情報を持つ少なくとも1人のユーザーのコレクションを含む配列。 1回のリクエストで提供できるユーザーIDは最大1000個です。 各userオブジェクトには、次の情報が含まれます。 <ul><li>`key`: 応答データ内の個別のジョブIDを修飾するために使用されるユーザーの識別子。 ベストプラクティスとして、この値を簡単に参照または後で参照できるように、一意で識別しやすい文字列を選択します。</li><li>`action`: データに対して行う必要のあるアクションをリストする配列。 販売中止のリクエストの場合、配列には値のみを含める必要があり `opt-out-of-sale`ます。</li><li>`userIDs`: ユーザーのIDの集まり。 1人のユーザーが持つことのできるIDの数は9個に制限されます。 各IDは、 `namespace`、、 `value`および名前空間修飾子(`type`)で構成されます。 必要なプロパティの詳細については、 [付録](appendix.md) を参照してください。</li></ul> との詳細については、『トラブルシューティングガイド』 `users` を参照し `userIDs`てください [](../troubleshooting-guide.md#user-ids)。 |
| `include` **(必須)** | 処理に含めるアドビ製品の配列。 この値がない場合や空の場合は、要求は拒否されます。 組織が統合を持つ製品のみを含めます。 詳細については、付録の [受け入れられる製品値](appendix.md) の節を参照してください。 |
| `expandIDs` | に設定した場合に、アプリケーション内のIDを処理するための最適化を表すオプションのプロパティ `true`です(現在、は、でのみサポートされてい [!DNL Analytics]ます)。 If omitted, this value defaults to `false`. |
| `priority` | リクエストの処理の優先度を設定する、アドビAnalyticsが使用するオプションのプロパティです。 指定できる値は `normal` とで `low`す。 を省略 `priority` した場合のデフォルトの動作はで `normal`す。 |
| `analyticsDeleteMethod` | アドビのAnalyticsでの個人データの処理方法を指定するオプションのプロパティです。 この属性には、次の2つの値を指定できます。 <ul><li>`anonymize`: 特定のユーザーIDのコレクションによって参照されるすべてのデータは匿名になります。 を省略 `analyticsDeleteMethod` した場合の動作はデフォルトです。</li><li>`purge`: すべてのデータが完全に削除されます。</li></ul> |
| `regulation` **(必須)** | 請求の規則 次の3つの値のいずれかにする必要があります。 <ul><li>gdpr</li><li>ccpa</li><li>pdpa_tha</li></ul> |

**応答**

正常に応答すると、新しく作成されたジョブの詳細が返されます。

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
| `jobId` | ジョブに対する、一意で読み取り専用のシステム生成ID。 この値は、次の手順で特定のジョブを検索するために使用します。 |

ジョブリクエストが正常に送信されたら、次のステップに進んで、ジョブのステータスを確認できます。

## ジョブのステータスの確認 {#check-status}

エンドポイントへのGET要求のパスにそのジョブを含めると、現在の処理ステータスなど、特定のジョブに関する情報 `jobId` を取得でき `/jobs` ます。

>[!IMPORTANT]
>
>以前に作成したジョブのデータは、ジョブの完了日から30日以内にのみ取得できます。

**API形式**

```http
GET /jobs/{JOB_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{JOB_ID}` | 検索するジョブのID。 このIDは、ジョブ `jobId` の [作成とすべてのジョブの](#create-job) 一覧表示に成功したAPI応答で返されます [](#list)。 |

**リクエスト**

次のリクエストは、リクエストパスで指定されたジョブの詳細 `jobId` を取得します。

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
| `downloadURL` | ジョブのステータスがZIPファイルの場合 `complete`、この属性はジョブの結果をダウンロードするためのURLを提供します。 このファイルは、ジョブの完了後60日間ダウンロードできます。 |

### ジョブステータスカテゴリ {#status-categories}

次の表に、様々なジョブステータスカテゴリと、それに対応する意味を示します。

| ステータスカテゴリ | 意味 |
| -------------- | -------- |
| 完了 | ジョブが完了し、（必要に応じて）各アプリケーションからファイルがアップロードされます。 |
| 処理 | アプリケーションはジョブを確認し、現在処理中です。 |
| Submitted | ジョブは、該当するすべての申し込みに送信されます。 |
| エラー | ジョブの処理でエラーが発生しました。個々のジョブの詳細を取得することで、より具体的な情報を取得できる場合があります。 |

>[!NOTE]
>
>依存する子ジョブがまだ処理中の場合、送信されたジョブは、処理状態のままになる可能性があります。

## 次の手順

これで、 [!DNL Privacy Service] APIを使用したプライバシージョブの作成および監視方法を理解できました。 ユーザインターフェイスを使用して同じタスクを実行する方法について詳しくは、 [Privacy ServiceUIの概要を参照してください](../ui/overview.md)。
