---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ジョブ
topic: developer guide
translation-type: tm+mt
source-git-commit: 5699022d1f18773c81a0a36d4593393764cb771a

---


# プライバシージョブ

以降の節では、プライバシーサービスAPIのルートエンドポイント(`/`)を使用して行う呼び出しについて説明します。 各呼び出しには、一般的なAPI形式、必要なヘッダーを示すサンプルリクエスト、およびサンプル応答が含まれます。

## プライバシージョブの作成

新しいジョブリクエストを作成する前に、まず、データをアクセス、削除、または販売するデータの件名に関する識別情報を収集するオプトアウト必要があります。 必要なデータを取得したら、ルートエンドポイントへのPOST要求のペイロードでそのデータを提供する必要があります。

>[!NOTE] 互換性のあるAdobe Experience Cloudアプリケーションは、データの主題を識別するために異なる値を使用します。 アプリに必要な識別子につ [いて詳しくは、プライバシーサービスおよびExperience Cloudアプリケーションの](../experience-cloud-apps.md) 「ガイド」を参照してください。

プライバシーサービスAPIは、個人データに対する2種類のジョブリクエストをサポートしています。

* [アクセスまたは削除](#access-delete):個人データにアクセス（読み取り）または削除します。
* [販オプトアウト売中](#opt-out):個人データを販売しないものとしてマークします。

>[!IMPORTANT] アクセス要求と削除要求は1回のAPI呼び出しとして組み合わせることができますが、オプトアウト要求は別々に行う必要があります。

### アクセス/削除ジョブの作成 {#access-delete}

この節では、APIを使用してアクセス/削除ジョブリクエストを作成する方法を説明します。

**API形式**

```http
POST /
```

**リクエスト**

次のリクエストは、後述のようにペイロードで指定された属性で設定された新しいジョブリクエストを作成します。

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
| `companyContexts` **（必須）** | 組織の認証情報を含む配列。 リストに表示される各識別子には、次の属性が含まれます。 <ul><li>`namespace`:識別子の名前空間。</li><li>`value`:識別子の値。</li></ul>識別子の **1つが** 、IMS組織の一意のIDを含む `imsOrgId` 、その識別子をそ `namespace``value` の識別子として使用する必要があります。 <br/><br/>追加の識別子は、組織に属するアドビ会社との統合を識別する、製品固有のアプリケーション修飾子( `Campaign`例えば、)にすることができます。 有効な値には、アカウント名、クライアントコード、テナントID、その他のアプリケーション識別子が含まれます。 |
| `users` **（必須）** | アクセスまたは削除する情報を持つ少なくとも1人のユーザーのコレクションを含む配列。 単一のリクエストで提供できるユーザーIDは、最大1,000個です。 各ユーザーオブジェクトには、次の情報が含まれます。 <ul><li>`key`:応答データ内の個別のジョブIDを修飾するために使用される識別子。 この値に対して一意の、簡単に識別できる文字列を選択し、後で簡単に参照または参照できるようにすることをお勧めします。</li><li>`action`:データに対してリストが行う必要のあるアクションを指定する配列。 実行するアクションに応じて、この配列に、、、またはその両方を含め `access`る必 `delete`要があります。</li><li>`userIDs`:特定のユーザーのIDの集まり。 1人のユーザーが持つことのできるIDの数は9個に制限されます。 各IDは、、、および `namespace`名前空間 `value`修飾子(`type`)で構成されます。 これらの必須プ [ロパティの](appendix.md) 詳細については、付録を参照してください。</li></ul> |
| `include` **（必須）** | 処理に含めるアドビ製品の配列。 この値がない場合や空の場合、要求は拒否されます。 組織が統合している製品のみを含めます。 詳しくは、付録で受け [入れられる製品値](appendix.md) のセクションを参照してください。 |
| `expandIDs` | に設定した場合、アプリケーション内のIDを処 `true`理するための最適化を表すオプションのプロパティです（現在、Analyticsでのみサポートされています）。 If omitted, this value defaults to `false`. |
| `priority` | リクエストの処理の優先度を設定する、Adobe Analyticsで使用されるオプションのプロパティです。 指定できる値は、 `normal` およびで `low`す。 を省略 `priority` すると、デフォルトの動作はです `normal`。 |
| `analyticsDeleteMethod` | Adobe Analyticsでの個人データの処理方法を指定するオプションのプロパティです。 この属性には、次の2つの値を指定できます。 <ul><li>`anonymize`:特定のユーザーIDのコレクションによって参照されるすべてのデータは匿名になります。 を省略 `analyticsDeleteMethod` した場合のデフォルトの動作です。</li><li>`purge`:すべてのデータが完全に削除されます。</li></ul> |
| `regulation` **（必須）** | リクエストの規制（「gdpr」または「ccpa」）。 |

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
| `jobId` | ジョブの読み取り専用の、一意のシステム生成ID。 この値は、特定のジョブを検索する次の手順で使用されます。 |

ジョブリクエストの送信が完了したら、ジョブのステータスを確認する次 [の手順に進むことができます](#check-the-status-of-a-job)。

### オプトアウト・オブ・セール・ジョブの作成 {#opt-out}

この節では、APIを使用して販売中のオプトアウトジョブリクエストを作成する方法を説明します。

**API形式**

```http
POST /
```

**リクエスト**

次のリクエストは、後述のようにペイロードで指定された属性で設定された新しいジョブリクエストを作成します。

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
| `companyContexts` **（必須）** | 組織の認証情報を含む配列。 リストに表示される各識別子には、次の属性が含まれます。 <ul><li>`namespace`:識別子の名前空間。</li><li>`value`:識別子の値。</li></ul>識別子の **1つが** 、IMS組織の一意のIDを含む `imsOrgId` 、その識別子をそ `namespace``value` の識別子として使用する必要があります。 <br/><br/>追加の識別子は、組織に属するアドビ会社との統合を識別する、製品固有のアプリケーション修飾子( `Campaign`例えば、)にすることができます。 有効な値には、アカウント名、クライアントコード、テナントID、その他のアプリケーション識別子が含まれます。 |
| `users` **（必須）** | アクセスまたは削除する情報を持つ少なくとも1人のユーザーのコレクションを含む配列。 単一のリクエストで提供できるユーザーIDは、最大1,000個です。 各ユーザーオブジェクトには、次の情報が含まれます。 <ul><li>`key`:応答データ内の個別のジョブIDを修飾するために使用される識別子。 この値に対して一意の、簡単に識別できる文字列を選択し、後で簡単に参照または参照できるようにすることをお勧めします。</li><li>`action`:データに対してリストが行う必要のあるアクションを指定する配列。 販売停止リクエストの場合、配列には値のみを含める必要があります `opt-out-of-sale`。</li><li>`userIDs`:特定のユーザーのIDの集まり。 1人のユーザーが持つことのできるIDの数は9個に制限されます。 各IDは、、、および `namespace`名前空間 `value`修飾子(`type`)で構成されます。 これらの必須プ [ロパティの](appendix.md) 詳細については、付録を参照してください。</li></ul> |
| `include` **（必須）** | 処理に含めるアドビ製品の配列。 この値がない場合や空の場合、要求は拒否されます。 組織が統合している製品のみを含めます。 詳しくは、付録で受け [入れられる製品値](appendix.md) のセクションを参照してください。 |
| `expandIDs` | に設定した場合、アプリケーション内のIDを処 `true`理するための最適化を表すオプションのプロパティです（現在、Analyticsでのみサポートされています）。 If omitted, this value defaults to `false`. |
| `priority` | リクエストの処理の優先度を設定する、Adobe Analyticsで使用されるオプションのプロパティです。 指定できる値は、 `normal` およびで `low`す。 を省略 `priority` すると、デフォルトの動作はです `normal`。 |
| `analyticsDeleteMethod` | Adobe Analyticsでの個人データの処理方法を指定するオプションのプロパティです。 この属性には、次の2つの値を指定できます。 <ul><li>`anonymize`:特定のユーザーIDのコレクションによって参照されるすべてのデータは匿名になります。 を省略 `analyticsDeleteMethod` した場合のデフォルトの動作です。</li><li>`purge`:すべてのデータが完全に削除されます。</li></ul> |
| `regulation` **（必須）** | リクエストの規制（「gdpr」または「ccpa」）。 |

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
| `jobId` | ジョブの読み取り専用の、一意のシステム生成ID。 この値は、次の手順で特定のジョブを検索するために使用されます。 |

ジョブリクエストの送信が正常に完了したら、ジョブのステータスを確認する次の手順に進むことができます。

## ジョブのステータスの確認

前の手順で返さ `jobId` れた値の1つを使用して、現在の処理ステータスなど、そのジョブに関する情報を取得できます。

>[!IMPORTANT] 以前に作成したジョブのデータは、ジョブの完了日から30日以内にのみ取得できます。

**API形式**

```http
GET /{JOB_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{JOB_ID}` | 検索するジョブのID。前の手順の応答で `jobId` 返され [ます](#create-a-job-request)。 |

**リクエスト**

次のリクエストは、リクエストパスで指定されたジョブ `jobId` の詳細を取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/privacy/jobs/6fc09b53-c24f-4a6c-9ca2-c6076b0842b6 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
```

**応答**

成功した応答は、指定したジョブの詳細を返します。

```json
{
    "jobId": "527ef92d-6cd9-45cc-9bf1-477cfa1e2ca2",
    "requestId": "15700479082313109RX-899",
    "userKey": "David Smith",
    "action": "access",
    "status": "error",
    "submittedBy": "02b38adf-6573-401e-b4cc-6b08dbc0e61c@techacct.adobe.com",
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
                "status": "submitted",
                "message": "processing"
            }
        },
        {
            "product": "AudienceManager",
            "retryCount": 0,
            "processedDate": "10/02/2019 08:25 PM GMT",
            "productStatusResponse": {
                "status": "submitted",
                "message": "processing"
            }
        }
    ],
    "downloadURL": "http://...",
    "regulation": "ccpa"
}
```

| プロパティ | 説明 |
| --- | --- |
| `productStatusResponse` | ジョブの現在のステータス。 可能な各ステータスの詳細を以下の表に示します。 |
| `downloadURL` | ジョブのステータスがZIPファ `complete`イルの場合、この属性はジョブの結果をダウンロードするURLを指定します。 このファイルは、ジョブの完了後60日間ダウンロードできます。 |

### ジョブステータス応答

次の表に、様々なリストのステータスと、対応する意味を示します。

| ステータスコード | ステータスメッセージ | 意味 |
| ----------- | -------------- | -------- |
| 1 | 完了 | ジョブが完了し、（必要に応じて）すべてのアプリケーションからファイルがアップロードされます。 |
| 2 | 処理 | アプリケーションはジョブを確認し、現在処理中です。 |
| 3 | Submitted | ジョブは、該当するすべての申し込みに送信されます。 |
| 4 | エラー | ジョブの処理に失敗しました。個々のジョブの詳細を取得することで、より具体的な情報を取得できます。 |

>[!NOTE] 依存する子ジョブがまだ処理中の場合、送信済みのジョブは処理状態のままになる可能性があります。

## リスト

GET要求をルート(`/`)エンドポイントに送信することで、組織内で使用可能なすべてのジョブ要求のリストを表示できます。

**API形式**

このリクエストの形式では、ル `regulation` ート(`/`)エンドポイントでクエリパラメーターが使用されるので、次に示すように疑問符(`?`)で始まります。 応答はページ分割され、他のクエリパラメーター(および`page` )を使用し `size`て応答をフィルターできます。 アンパサンド(`&`)を使用して、複数のパラメーターを区切ることができます。

```http
GET ?regulation={REGULATION}
GET ?regulation={REGULATION}&page={PAGE}
GET ?regulation={REGULATION}&size={SIZE}
GET ?regulation={REGULATION}&page={PAGE}&size={SIZE}
```

| パラメーター | 説明 |
| --- | --- |
| `{REGULATION}` | クエリの種類。 指定できる値は、 `gdpr` およびで `ccpa`す。 |
| `{PAGE}` | 0を基準とする番号を使用して、表示するデータのページ。 デフォルトは `0` です。 |
| `{SIZE}` | 各ページに表示する結果の数。 デフォルトはで `1` 、最大はです `100`。 最大値を超えると、APIは400コードエラーを返します。 |

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

成功した応答は、ジョブのリストを返し、各ジョブにそのジョブの詳細などが含まれま `jobId`す。 この例では、結果の3ページ目から始まる50個のリストのジョブが応答に含まれます。

### 後続のページへのアクセス

ページ分割された応答の次の結果セットを取得するには、同じエンドポイントに対して別のAPI呼び出しを行い、 `page` クエリパラメーターを1増やします。

## 次の手順

プライバシーサービスAPIを使用してプライバシージョブを作成および監視する方法を理解しました。 ユーザーインターフェイスを使用して同じタスクを実行する方法について詳しくは、「プライバシーサービスのUIの [概要」を参照してくださ](../ui/overview.md)い。
