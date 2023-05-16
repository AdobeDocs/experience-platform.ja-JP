---
description: このページでは、/testing/destinationInstance API エンドポイントを使用して、ファイルベースの宛先が正しく設定されているかどうかをテストし、設定した宛先へのデータフローの整合性を検証する方法について説明します。
title: サンプルプロファイルを使用したファイルベースの宛先のテスト
exl-id: 75f76aec-245b-4f07-8871-c64a710db9f6
source-git-commit: ffd87573b93d642202e51e5299250a05112b6058
workflow-type: tm+mt
source-wordcount: '827'
ht-degree: 12%

---

# サンプルプロファイルを使用したファイルベースの宛先のテスト

## 概要 {#overview}

このページでは、 `/testing/destinationInstance` ファイルベースの宛先が正しく設定されているかどうかをテストし、設定した宛先へのデータフローの整合性を検証するための API エンドポイント。

テストエンドポイントには、を追加するかどうかに関わらず、リクエストをおこなうことができます。 [サンプルプロファイル](file-based-sample-profile-generation-api.md) を呼び出しに追加します。 リクエストでプロファイルを送信しない場合、API はサンプルプロファイルを自動的に生成し、リクエストに追加します。

自動生成されたサンプルプロファイルには、汎用のデータが含まれています。 より直感的にカスタムのプロファイルデータで宛先をテストする場合は、 [サンプルプロファイル生成 API](file-based-sample-profile-generation-api.md) サンプルプロファイルを生成し、その応答をカスタマイズして、 `/testing/destinationInstance` endpoint.

## はじめに {#getting-started}

続ける前に「[はじめる前に](../../getting-started.md)」を参照し、必要な宛先オーサリング権限および必要なヘッダーの取得方法など、API の呼び出しを正常に行うために必要となる重要な情報を確認してください。

## 前提条件 {#prerequisites}

使用する前に `/testing/destinationInstance` エンドポイントで、次の条件を満たしていることを確認します。

* Destination SDKを通じて作成された既存のファイルベースの宛先があり、 [宛先カタログ](../../../ui/destinations-workspace.md).
* 宛先 UI に、少なくとも 1 つのアクティベーションフローがExperience Platformされました。
* API リクエストを正常に実行するには、テストする宛先インスタンスに対応する宛先インスタンス ID が必要です。 Platform UI で宛先との接続を参照する際に、API 呼び出しで使用する必要がある宛先インスタンス ID を URL から取得します。

   ![URL から宛先インスタンス ID を取得する方法を示す UI 画像。](../../assets/testing-api/get-destination-instance-id.png)
* *オプション*:API 呼び出しにサンプルプロファイルを追加して宛先設定をテストする場合は、 [/sample-profiles](file-based-sample-profile-generation-api.md) エンドポイント：既存のソーススキーマに基づいてサンプルプロファイルを生成します。 サンプルプロファイルを指定しない場合、API はプロファイルを生成し、応答で返します。

## 呼び出しにプロファイルを追加せずに、宛先設定をテストする {#test-without-adding-profiles}

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
| `{DESTINATION_INSTANCE_ID}` | サンプルプロファイルを生成する宛先インスタンスの ID。 詳しくは、 [前提条件](#prerequisites) の節を参照してください。 |

**応答**

正常な応答は、HTTP ステータス 200 と応答ペイロードを返します。

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
| `activations` | アクティブ化された各セグメントのセグメント ID とフロー実行 ID を返します。 アクティベーションエントリ（および関連する生成されたファイル）の数は、宛先インスタンスでマッピングされたセグメントの数と等しくなります。 <br><br> 例：2 つのセグメントを宛先インスタンスにマッピングした場合、 `activations` 配列には 2 つのエントリが含まれます。 アクティブ化された各セグメントは、書き出された 1 つのファイルに対応します。 |
| `results` | の呼び出しに使用できる宛先インスタンス ID とフロー実行 ID を返します [結果 API](file-based-destination-results-api.md)を追加して、統合をさらにテストします。 |
| `inputProfiles` | API によって自動生成されたサンプルプロファイルを返します。 |

{style="table-layout:auto"}

## 呼び出しに追加されたプロファイルを使用して、宛先設定をテストする {#test-with-added-profiles}

より直感的なカスタムプロファイルデータで宛先をテストするには、 [/sample-profiles](file-based-sample-profile-generation-api.md) エンドポイントに任意の値を設定し、 `/testing/destinationInstance` endpoint.

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
| `{DESTINATION_INSTANCE_ID}` | テストする宛先の宛先インスタンス ID。  サンプルプロファイルを生成する宛先インスタンスの ID。 詳しくは、 [前提条件](#prerequisites) の節を参照してください。 |
| `profiles` | 1 つまたは複数のプロファイルを含めることができる配列。 以下を使用： [サンプルプロファイル API エンドポイント](file-based-sample-profile-generation-api.md) を使用して、この API 呼び出しで使用するプロファイルを生成します。 |

**応答**

正常な応答は、HTTP ステータス 200 と応答ペイロードを返します。

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
| `activations` | アクティブ化された各セグメントのセグメント ID とフロー実行 ID を返します。 アクティベーションエントリ（および関連する生成されたファイル）の数は、宛先インスタンスでマッピングされたセグメントの数と等しくなります。 <br><br> 例：2 つのセグメントを宛先インスタンスにマッピングした場合、 `activations` 配列には 2 つのエントリが含まれます。 アクティブ化された各セグメントは、書き出された 1 つのファイルに対応します。 |
| `results` | の呼び出しに使用できる宛先インスタンス ID とフロー実行 ID を返します [結果 API](file-based-destination-results-api.md)を追加して、統合をさらにテストします。 |
| `inputProfiles` | API リクエストで渡したカスタムサンプルプロファイルを返します。 |

## API エラー処理 {#api-error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順

このドキュメントを読んだ後、ファイルベースの宛先設定をテストする方法がわかりました。

有効な API 応答を受け取った場合、宛先は正しく動作しています。 アクティベーションフローの詳細を確認するには、 `results` 対する応答からのプロパティ [詳細なアクティベーション結果の表示](file-based-destination-results-api.md).

公開先を作成する場合、 [宛先設定を送信](../../guides/submit-destination.md) をAdobeに追加します。
