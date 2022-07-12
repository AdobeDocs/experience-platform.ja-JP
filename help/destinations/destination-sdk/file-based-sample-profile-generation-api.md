---
description: このページでは、Destination SDKの/sample-profiles API エンドポイントを使用して、ソーススキーマに基づくサンプルプロファイルを生成する方法について説明します。 これらのサンプルプロファイルを使用して、ファイルベースの宛先設定をテストできます。
title: ソーススキーマに基づくサンプルプロファイルの生成
source-git-commit: 734d66cc881ab1b691c13ef446331d0c51851cf9
workflow-type: tm+mt
source-wordcount: '674'
ht-degree: 13%

---


# ソーススキーマに基づくサンプルプロファイルの生成

## 概要 {#overview}

ファイルベースの宛先をテストする最初の手順は、 `/sample-profiles` エンドポイント：既存のソーススキーマに基づいてサンプルプロファイルを生成します。

サンプルプロファイルは、プロファイルの JSON 構造を理解するのに役立ちます。 さらに、独自のプロファイルデータを使用してをカスタマイズし、宛先テストを実施するためのデフォルトが提供されます。

## はじめに {#getting-started}

続ける前に「[はじめる前に](./getting-started.md)」を参照し、必要な宛先オーサリング権限および必要なヘッダーの取得方法など、API の呼び出しを正常に行うために必要となる重要な情報を確認してください。

## 前提条件 {#prerequisites}

使用する前に `/sample-profiles` エンドポイントで、次の条件を満たしていることを確認します。

* Destination SDKを通じて作成された既存のファイルベースの宛先があり、 [宛先カタログ](../ui/destinations-workspace.md).
* 宛先 UI に、少なくとも 1 つのアクティベーションフローがExperience Platformされました。 この `/sample-profiles` エンドポイントは、アクティベーションフローで定義したソーススキーマに基づいてプロファイルを作成します。 詳しくは、 [activation チュートリアル](../ui/activate-batch-profile-destinations.md) を参照してください。
* API リクエストを正常に実行するには、テストする宛先インスタンスに対応する宛先インスタンス ID が必要です。 Platform UI で宛先との接続を参照する際に、API 呼び出しで使用する必要がある宛先インスタンス ID を URL から取得します。

   ![URL から宛先インスタンス ID を取得する方法を示す UI 画像。](assets/get-destination-instance-id.png)

## 宛先テスト用のサンプルプロファイルを生成 {#generate-sample-profiles}

に対してGETリクエストを実行することで、ソーススキーマに基づいてサンプルプロファイルを生成できます `/sample-profiles` endpoint は、テストする宛先の宛先インスタンス ID です。

**API 形式**

```http
GET /authoring/sample-profiles?destinationInstanceId={DESTINATION_INSTANCE_ID}&count={NUMBER_OF_GENERATED_PROFILES}
```

| クエリパラメーター | 説明 |
| -------- | ----------- |
| `destinationInstanceId` | サンプルプロファイルを生成する宛先インスタンスの ID。 詳しくは、 [前提条件](#prerequisites) の節を参照してください。 |
| `count` | *オプション*。生成するサンプルプロファイルの数。 パラメーターは、 `1 - 1000`. このプロパティを定義しない場合、API は単一のサンプルプロファイルを生成します。 |

**リクエスト**

次のリクエストでは、対応する `destinationInstanceId`.

```shell
curl -X GET 'https://platform.adobe.io/data/core/activation/authoring/sample-profiles?destinationInstanceId={DESTINATION_INSTANCE_ID}' \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

正常な応答は、指定された数のサンプルプロファイルと共に HTTP ステータス 200 を返します。このとき、セグメントメンバーシップ、ID、およびソース XDM スキーマに対応するプロファイル属性が返されます。

>[!NOTE]
>
> 応答は、宛先インスタンスで使用されるセグメントのメンバーシップ、ID、プロファイル属性のみを返します。 ソーススキーマに他のフィールドが含まれている場合でも、それらは無視されます。

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

![UI から API 応答のフィールドへのマッピングを示す画像です。](assets/sample-api-response-mapping.png)

| プロパティ | 説明 |
| -------- | ----------- |
| `segmentMembership` | 個人のセグメントメンバーシップを表す map オブジェクト。 詳しくは、 `segmentMembership`，読み取り [セグメントメンバーシップの詳細](../../xdm/field-groups/profile/segmentation.md). |
| `lastQualificationTime` | このプロファイルが最後にセグメントで認定された時刻のタイムスタンプ。 |
| `status` | セグメントのメンバーシップが現在のリクエストの一部として認識されたかどうかを示します。 次の値を使用できます。 <ul><li>`existing`:プロファイルは、リクエストの前に既にセグメントに含まれていて、引き続きメンバーシップを維持します。</li><li>`realized`:プロファイルは、現在のリクエストの一部としてセグメントに入っています。</li><li>`exited`:プロファイルは、現在のリクエストの一環としてセグメントから退出しています。</li></ul> |
| `identityMap` | 個々の ID の様々な値と、関連する名前空間を説明する map-type フィールドです。 詳しくは、 `identityMap`を参照してください。 [スキーマ構成の基盤](../../xdm/schema/composition.md#identityMap). |

{style=&quot;table-layout:auto&quot;}

## API エラー処理 {#api-error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順

このドキュメントを読むと、宛先で設定したソーススキーマに基づいてサンプルプロファイルを生成する方法がわかります [活性化フロー](../ui/activate-batch-profile-destinations.md).

これらのプロファイルをカスタマイズしたり、API から返されるプロファイルを次の場所に使用したりできるようになりました。 [ファイルベースの宛先設定をテストする](file-based-destination-testing-api.md).