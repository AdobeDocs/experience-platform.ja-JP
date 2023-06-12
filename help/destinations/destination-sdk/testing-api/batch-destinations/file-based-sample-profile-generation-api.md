---
description: このページでは、Destination SDK から /sample-profiles API エンドポイントを使用して、ソーススキーマに基づいてサンプルプロファイルを生成する方法について説明します。これらのサンプルプロファイルを使用して、ファイルベースの宛先設定をテストできます。
title: ソーススキーマに基づくサンプルプロファイルの生成
exl-id: aea50d2e-e916-4ef0-8864-9333a4eafe80
source-git-commit: adf75720f3e13c066b5c244d6749dd0939865a6f
workflow-type: tm+mt
source-wordcount: '651'
ht-degree: 100%

---


# ソーススキーマに基づくサンプルプロファイルの生成

ファイルベースの宛先をテストする最初の手順は、`/sample-profiles` エンドポイントを使用して、既存のソーススキーマに基づいてサンプルプロファイルを生成することです。

サンプルプロファイルは、プロファイルの JSON 構造を理解するのに役立ちます。また、これをデフォルトとして、独自のプロファイルデータでカスタマイズすることで、より詳細に宛先をテストできます。

## はじめに {#getting-started}

続行する前に、「[はじめる前に](../../getting-started.md)」を参照し、API の呼び出しを正常に行うために必要となる重要な情報（必要な宛先オーサリング権限および必要なヘッダーの取得方法など）を確認してください。

## 前提条件 {#prerequisites}

`/sample-profiles` エンドポイントを使用する前に、以下の条件を満たしていることを確認してください。

* Destination SDK で作成した既存のファイルベースの宛先があり、[宛先カタログ](../../../ui/destinations-workspace.md)で確認できる。
* Experience Platform UI で、宛先に対して少なくとも 1 つのアクティベーションフローを作成している。`/sample-profiles` エンドポイントは、アクティベーションフローでしたソーススキーマに基づいてプロファイルを作成します。アクティベーションフローの作成方法について詳しくは、[アクティベーションチュートリアル](../../../ui/activate-batch-profile-destinations.md)を参照してください。
* API リクエストを成功させるには、テストする宛先インスタンスに対応する宛先インスタンス ID が必要です。Platform UI で宛先との接続を参照する際に、URL から、API 呼び出しで使用する必要がある宛先インスタンス ID を取得します。

  ![URL から宛先インスタンス ID を取得する方法を示す UI 画像。](../../assets/testing-api/get-destination-instance-id.png)

## 宛先テスト用のサンプルプロファイルの生成 {#generate-sample-profiles}

テストしたい宛先の宛先インスタンス ID を使用して `/sample-profiles` エンドポイントに対して GET リクエストを行うことで、ソーススキーマに基づいてサンプルプロファイルを生成できます。

**API 形式**

```http
GET /authoring/sample-profiles?destinationInstanceId={DESTINATION_INSTANCE_ID}&count={NUMBER_OF_GENERATED_PROFILES}
```

| クエリパラメーター | 説明 |
| -------- | ----------- |
| `destinationInstanceId` | サンプルプロファイルを生成する宛先インスタンスの ID。この ID の取得方法について詳しくは、[前提条件](#prerequisites)の節を参照してください。 |
| `count` | *オプション*。生成するサンプルプロファイルの数。このパラメーターは、`1 - 1000` の値を取ることができます。このプロパティが定義されていない場合は、API が 1 つのサンプルプロファイルを生成します。 |

**リクエスト**

以下のリクエストは、対応する `destinationInstanceId` を持つ宛先インスタンスで定義されたソーススキーマに基づいて、サンプルプロファイルを生成します。

```shell
curl -X GET 'https://platform.adobe.io/data/core/activation/authoring/sample-profiles?destinationInstanceId={DESTINATION_INSTANCE_ID}' \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

応答が成功すると、HTTP ステータス 200 が、指定された数のサンプルプロファイル（ソース XDM スキーマに対応するセグメントメンバーシップ、ID およびプロファイル属性を含む）と共に返されます。

>[!NOTE]
>
> 応答は、宛先インスタンスで使用されたセグメントメンバーシップ、ID およびプロファイル属性のみを返します。ソーススキーマに他のフィールドがあっても、それらは無視されます。

```json
[
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
```

![UI から API 応答のフィールドへのマッピングを示す画像。](../../assets/testing-api/batch-destinations/sample-api-response-mapping.png)

| プロパティ | 説明 |
| -------- | ----------- |
| `segmentMembership` | 個人のセグメントメンバーシップを記述するマップオブジェクト。`segmentMembership` について詳しくは、[セグメントメンバーシップの詳細](../../../../xdm/field-groups/profile/segmentation.md)を参照してください。 |
| `lastQualificationTime` | 前回、このプロファイルがセグメントに選定された際のタイムスタンプ。 |
| `status` | セグメントメンバーシップが現在のリクエストの一環として実現されたかどうかを示す文字列フィールド。以下の値を使用できます。 <ul><li>`realized`：プロファイルは、セグメントの一部です。</li><li>`exited`：プロファイルは、現在のリクエストの一環としてセグメントから外れています。</li></ul> |
| `identityMap` | 個人の様々な ID 値を、関連する名前空間と共に記述するマップタイプフィールド。`identityMap` について詳しくは、[スキーマ構成の基本](../../../../xdm/schema/composition.md#identityMap)を参照してください。 |

{style="table-layout:auto"}

## API エラー処理 {#api-error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順

このドキュメントでは、宛先[アクティベーションフロー](../../../ui/activate-batch-profile-destinations.md)で設定されたソーススキーマに基づいてサンプルプロファイルを生成する方法を確認しました。

これで、これらのプロファイルをカスタマイズしたり、API から返されたプロファイルをそのまま使用して、[ファイルベースの宛先設定をテスト](file-based-destination-testing-api.md)できるようになりました。
