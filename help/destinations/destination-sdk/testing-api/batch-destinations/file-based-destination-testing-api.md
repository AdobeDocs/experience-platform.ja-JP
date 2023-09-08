---
description: このページでは、/testing/destinationInstance API エンドポイントを使用して、ファイルベースの宛先が正しく設定されているかどうかをテストしたり、設定された宛先に対するデータフローの整合性を検証したりする方法を説明します。
title: サンプルプロファイルを使用したファイルベースの宛先のテスト
exl-id: 75f76aec-245b-4f07-8871-c64a710db9f6
source-git-commit: 9ac6b075af3805da4dad0dd6442d026ae96ab5c7
workflow-type: ht
source-wordcount: '827'
ht-degree: 100%

---

# サンプルプロファイルを使用したファイルベースの宛先のテスト

## 概要 {#overview}

このページでは、`/testing/destinationInstance` API エンドポイントを使用して、ファイルベースの宛先が正しく設定されているかどうかをテストしたり、設定された宛先に対するデータフローの整合性を検証したりする方法を説明します。

呼び出しに[サンプルプロファイル](file-based-sample-profile-generation-api.md)を追加してもしなくても、エンドポイントをテストするためのリクエストを行うことができます。リクエスト時に任意のプロファイルを送信しない場合、API は、サンプルプロファイルを自動的に生成して、リクエストに追加します。

自動生成されたサンプルプロファイルには、汎用データが含まれます。カスタムの、より直感的なプロファイルデータで宛先をテストしたい場合、[サンプルプロファイル生成 API](file-based-sample-profile-generation-api.md) を使用して、サンプルプロファイルを生成してから、その応答をカスタマイズして、`/testing/destinationInstance` エンドポイントに対するリクエストに含めます。

## はじめに {#getting-started}

続行する前に、「[はじめる前に](../../getting-started.md)」を参照し、API の呼び出しを正常に行うために必要となる重要な情報（必要な宛先オーサリング権限および必要なヘッダーの取得方法など）を確認してください。

## 前提条件 {#prerequisites}

`/testing/destinationInstance` エンドポイントを使用する前に、以下の条件を満たしていることを確認してください。

* Destination SDK で作成した既存のファイルベースの宛先があり、[宛先カタログ](../../../ui/destinations-workspace.md)で確認できる。
* Experience Platform UI で、宛先に対して少なくとも 1 つのアクティベーションフローを作成している。
* API リクエストを成功させるには、テストする宛先インスタンスに対応する宛先インスタンス ID が必要です。Platform UI で宛先との接続を参照する際に、URL から、API 呼び出しで使用する必要がある宛先インスタンス ID を取得します。

  ![URL から宛先インスタンス ID を取得する方法を示す UI 画像。](../../assets/testing-api/get-destination-instance-id.png)
* *オプション*：API 呼び出しに追加したサンプルプロファイルで宛先設定をテストしたい場合は、[/sample-profiles](file-based-sample-profile-generation-api.md) エンドポイントを使用して、既存のソーススキーマに基づいてサンプルプロファイルを生成します。サンプルプロファイルを提供しない場合、API がサンプルプロファイルを生成して、応答で返します。

## 呼び出しにプロファイルを追加しないで宛先設定をテスト {#test-without-adding-profiles}

**API 形式**

```http
POST /authoring/testing/destinationInstance/{DESTINATION_INSTANCE_ID}
```

**リクエスト**

```shell
curl -X POST 'https://platform.adobe.io/data/core/activation/authoring/testing/destinationInstance/{DESTINATION_INSTANCE_ID}' \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

| パスパラメーター | 説明 |
| -------- | ----------- |
| `{DESTINATION_INSTANCE_ID}` | サンプルプロファイルを生成する宛先インスタンスの ID。この ID の取得方法について詳しくは、[前提条件](#prerequisites)の節を参照してください。 |

**応答**

応答が成功すると、HTTP ステータス 200 が、応答ペイロードと共に返されます。

```json
{
   "activations":[
      {
         "segment":"6fa55d3a-18e1-4f65-95ed-ac8fdb03b45b",
         "flowRun":"81150d76-7909-46b6-83f4-fc855a92de07"
      },
      {
         "segment":"5fa55d3a-18e1-4f65-95ed-ac8fdb03b45b",
         "flowRun":"4706780a-2ab3-4d33-8c76-7c87fd318cd8"
      }
   ],
   "results":"/authoring/testing/destinationInstance/fd3449fb-b929-45c8-9f3d-06b9d6aac328/results?flowRunIds=4706780a-2ab3-4d33-8c76-7c87fd318cd8,81150d76-7909-46b6-83f4-fc855a92de07",
   "inputProfiles":[
      {
         "segmentMembership":{
            "ups":{
               "fea8d394-5a8c-4cea-bebc-df020ce37f5c":{
                  "lastQualificationTime":"2022-01-13T11:33:28.211895Z",
                  "status":"realized"
               },
               "5fa55d3a-18e1-4f65-95ed-ac8fdb03b45b":{
                  "lastQualificationTime":"2022-01-13T11:33:28.211893Z",
                  "status":"realized"
               }
            }
         },
         "personalEmail":{
            "address":"john.smith@abc.com"
         },
         "identityMap":{
            "crmid":[
               {
                  "id":"crmid-P1A7l"
               }
            ]
         },
         "person":{
            "name":{
               "firstName":"string",
               "lastName":"string"
            }
         }
      }
   ]
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `activations` | アクティブ化された各オーディエンスのオーディエンス ID およびフロー実行 ID を返します。アクティベーションエントリ（および関連する生成されたファイル）の数は、宛先インスタンスにマッピングされたオーディエンスの数に等しくなります。<br><br> 例：2 つのオーディエンスを宛先インスタンスにマッピングした場合、`activations` 配列には、2 つのエントリが含まれます。アクティブ化された各オーディエンスは、1 つの書き出されたファイルに対応します。 |
| `results` | 宛先インスタンス ID およびフロー実行 ID を返します。これらを[結果 API](file-based-destination-results-api.md) を呼び出すのに使用して、より詳細に統合をテストできます。 |
| `inputProfiles` | API によって自動生成されたサンプルプロファイルを返します。 |

{style="table-layout:auto"}

## 呼び出しにプロファイルを追加して宛先設定をテスト {#test-with-added-profiles}

カスタムの、より直感的なプロファイルデータで宛先をテストするには、[/sample-profiles](file-based-sample-profile-generation-api.md) エンドポイントから取得した応答を自分で選択した値でカスタマイズして、`/testing/destinationInstance` エンドポイントに対するリクエストにカスタムプロファイルを含めることができます。

**API 形式**

```http
POST  /testing/destinationInstance/{DESTINATION_INSTANCE_ID}
```

**リクエスト**

```shell
curl -X POST 'https://platform.adobe.io/data/core/activation/authoring/testing/destinationInstance/{DESTINATION_INSTANCE_ID}' 
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
 {
   "profiles":[
      {
         "segmentMembership":{
            "ups":{
               "fea8d394-5a8c-4cea-bebc-df020ce37f5c":{
                  "lastQualificationTime":"2022-01-13T11:33:28.211895Z",
                  "status":"realized"
               },
               "5fa55d3a-18e1-4f65-95ed-ac8fdb03b45b":{
                  "lastQualificationTime":"2022-01-13T11:33:28.211893Z",
                  "status":"realized"
               }
            }
         },
         "personalEmail":{
            "address":"michaelsmith@example.com"
         },
         "identityMap":{
            "crmid":[
               {
                  "id":"Custom CRM ID"
               }
            ]
         },
         "person":{
            "name":{
               "firstName":"Michael",
               "lastName":"Smith"
            }
         }
      }
   ]
}'
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{DESTINATION_INSTANCE_ID}` | テストしている宛先の宛先インスタンス ID。サンプルプロファイルを生成する宛先インスタンスの ID。この ID の取得方法について詳しくは、[前提条件](#prerequisites)の節を参照してください。 |
| `profiles` | 1 つまたは複数のプロファイルを含めることができる配列。[サンプルプロファイル API エンドポイント](file-based-sample-profile-generation-api.md)を使用して、この API 呼び出しで使用するためのプロファイルを生成します。 |

**応答**

応答が成功すると、HTTP ステータス 200 が、応答ペイロードと共に返されます。

```json
{
   "activations":[
      {
         "segment":"6fa55d3a-18e1-4f65-95ed-ac8fdb03b45b",
         "flowRun":"81150d76-7909-46b6-83f4-fc855a92de07"
      },
      {
         "segment":"5fa55d3a-18e1-4f65-95ed-ac8fdb03b45b",
         "flowRun":"4706780a-2ab3-4d33-8c76-7c87fd318cd8"
      }
   ],
   "results":"/authoring/testing/destinationInstance/fd3449fb-b929-45c8-9f3d-06b9d6aac328/results?flowRunIds=4706780a-2ab3-4d33-8c76-7c87fd318cd8,81150d76-7909-46b6-83f4-fc855a92de07",
   "inputProfiles":[
      {
         "segmentMembership":{
            "ups":{
               "fea8d394-5a8c-4cea-bebc-df020ce37f5c":{
                  "lastQualificationTime":"2022-01-13T11:33:28.211895Z",
                  "status":"realized"
               },
               "5fa55d3a-18e1-4f65-95ed-ac8fdb03b45b":{
                  "lastQualificationTime":"2022-01-13T11:33:28.211893Z",
                  "status":"realized"
               }
            }
         },
         "personalEmail":{
            "address":"michaelsmith@example.com"
         },
         "identityMap":{
            "crmid":[
               {
                  "id":"Custom CRM ID"
               }
            ]
         },
         "person":{
            "name":{
               "firstName":"Michael",
               "lastName":"Smith"
            }
         }
      }
   ]
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `activations` | アクティブ化された各オーディエンスのオーディエンス ID およびフロー実行 ID を返します。アクティベーションエントリ（および関連する生成されたファイル）の数は、宛先インスタンスにマッピングされたオーディエンスの数に等しくなります。<br><br> 例：2 つのオーディエンスを宛先インスタンスにマッピングした場合、`activations` 配列には、2 つのエントリが含まれます。アクティブ化された各オーディエンスは、1 つの書き出されたファイルに対応します。 |
| `results` | 宛先インスタンス ID およびフロー実行 ID を返します。これらを[結果 API](file-based-destination-results-api.md) を呼び出すのに使用して、より詳細に統合をテストできます。 |
| `inputProfiles` | API リクエストで渡されたカスタムサンプルプロファイルを返します。 |

## API エラー処理 {#api-error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順

このドキュメントでは、ファイルベースの宛先設定をテストする方法を確認しました。

有効な API 応答を受信している場合、宛先は正しく機能しています。アクティベーションフローに関するさらに詳細な情報を確認したい場合は、応答から `results` プロパティを使用して、[詳細なアクティベーション結果を表示](file-based-destination-results-api.md)できます。

公開されている宛先を作成している場合、これで、レビュー用にアドビに[宛先設定を送信](../../guides/submit-destination.md)できるようになりました。
