---
keywords: Experience Platform;ホーム;人気の高いトピック;ソース;コネクタ;ソースコネクタ;ソース sdk;SDK;SDK
title: Sourceの送信
description: 次のドキュメントでは、Flow Service API を使用して新しいソースをテストおよび検証し、セルフサービスソース（バッチ SDK）を通じて新しいソースを統合する手順を説明します。
exl-id: 9e945ba1-51b6-40a9-b92f-e0a52b3f92fa
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '829'
ht-degree: 10%

---

# ソースの送信

セルフサービスソース（バッチ SDK）を使用して新しいソースをAdobe Experience Platformに統合する最後の手順は、検証のためにソースをテストすることです。 成功したら、Adobe担当者に連絡して、新しいソースを送信できます。

次のドキュメントでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用してソースをテストおよびデバッグする手順を説明します。

## はじめに

* Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 ](../../../landing/api-guide.md) を参照してください。
* Experience Platform API の資格情報の生成方法について詳しくは、[Experience Platform API の認証とアクセス ](../../../landing/api-authentication.md) に関するチュートリアルを参照してください。
* Experience Platform API の [!DNL Postman] の設定方法について詳しくは、[Developer Console との設定  [!DNL Postman]](../../../landing/postman.md) に関するチュートリアルを参照してください。
* テストとデバッグプロセスに役立つように、[ セルフサービスソースの検証コレクションと環境をこちらからダウンロードし ](../assets/sdk-verification.zip) 以下の手順に従ってください。

## ソースのテスト

ソースをテストするには、ソースに関連する適切な環境変数を指定しながら、[!DNL Postman] で [ セルフサービスソース検証コレクションおよび環境 ](../assets/sdk-verification.zip) を実行する必要があります。

テストを開始するには、まず [!DNL Postman] でコレクションと環境を設定する必要があります。 次に、テストする接続仕様 ID を指定します。

### `authSpecName` を指定

接続仕様 ID を入力したら、ベース接続に使用する `authSpecName` を指定する必要があります。 選択に応じて、`OAuth 2 Refresh Code` または `Basic Authentication` のいずれかになります。 `authSpecName` を指定したら、必要な資格情報を環境に含める必要があります。 例えば、`authSpecName` を `OAuth 2 Refresh Code` として指定する場合は、OAuth 2 に必要な資格情報（`host` と `accessToken`）を指定する必要があります。

### `sourceSpec` を指定

認証仕様パラメータを追加した状態で、次にソース仕様から必要なプロパティを追加する必要があります。 必要なプロパティは、`sourceSpec.spec.properties` にあります。 以下の [!DNL MailChimp Members] の例では、必須プロパティは `listId` のみです。これは `listId` と、[!DNL Postman] 環境に対応する ID 値を意味します。

```json
"spec": {
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "description": "Define user input parameters to fetch resource values.",
  "properties": {
    "listId": {
      "type": "string",
      "description": "listId for which members need to fetch."
    }
  }
}
```

認証およびソース仕様パラメーターを指定したら、残りの環境変数の設定を開始できます。参照については、次の表を参照してください。

>[!NOTE]
>
>以下に示すサンプル変数はすべて、更新が必要なプレースホルダー値です。ただし、`flowSpecificationId` と `targetConnectionSpecId` は固定値です。

| パラメーター | 説明 | 例 |
| --- | --- | --- |
| `x-api-key` | Experience Platform API への呼び出しの認証に使用される一意の ID。 サー `x-api-key` スの取得方法について詳しくは、[Experience Platform API の認証とアクセス ](../../../landing/api-authentication.md) に関するチュートリアルを参照してください。 | `c8d9a2f5c1e03789bd22e8efdd1bdc1b` |
| `x-gw-ims-org-id` | 製品およびサービスを所有またはライセンスし、そのメンバーへのアクセスを許可できる法人組織。 `x-gw-ims-org-id` ーザー情報の取得方法については、[Developer Console の設定および  [!DNL Postman]](../../../landing/postman.md) に関するチュートリアルを参照してください。 | `ABCEH0D9KX6A7WA7ATQE0TE@adobeOrg` |
| `authorizationToken` | Experience Platform API を呼び出すために必要な認証トークン。 サー `authorizationToken` スの取得方法について詳しくは、[Experience Platform API の認証とアクセス ](../../../landing/api-authentication.md) に関するチュートリアルを参照してください。 | `Bearer authorizationToken` |
| `schemaId` | ソースデータをExperience Platformで使用するには、必要に応じてターゲットスキーマを作成してソースデータを構造化する必要があります。 ターゲット XDM スキーマの作成手順について詳しくは、 [API を使用したスキーマの作成](../../../xdm/api/schemas.md)に関するチュートリアルを参照してください。 | `https://ns.adobe.com/{TENANT_ID}.schemas.0ef4ce0d390f0809fad490802f53d30b` |
| `schemaVersion` | スキーマに対応する一意のバージョン。 | `application/vnd.adobe.xed-full-notext+json; version=1` |
| `schemaAltId` | 新しいスキーマを作成する際に `schemaId` と共に返される `meta:altId`。 | `_{TENANT_ID}.schemas.0ef4ce0d390f0809fad490802f53d30b` |
| `dataSetId` | ターゲットデータセットの作成手順について詳しくは、 [API を使用したデータセットの作成](../../../catalog/api/create-dataset.md)に関するチュートリアルを参照してください。 | `5f3c3cedb2805c194ff0b69a` |
| `mappings` | マッピングセットを使用すると、ソーススキーマ内のデータと宛先スキーマのデータとのマッピング方法を定義できます。マッピングの作成方法に関する詳細な手順については、[API を使用したマッピングセットの作成 ](../../../data-prep/api/mapping-set.md) に関するチュートリアルを参照してください。 | `[{"destinationXdmPath":"person.name.firstName","sourceAttribute":"email.email_id","identity":false,"version":0},{"destinationXdmPath":"person.name.lastName","sourceAttribute":"email.activity.action","identity":false,"version":0}]` |
| `mappingId` | マッピングセットに対応する一意の ID。 | `bf5286a9c1ad4266baca76ba3adc9366` |
| `connectionSpecId` | ソースに対応する接続仕様 ID。 これは、[ 新しい接続仕様の作成 ](./create.md) 後に生成した ID です。 | `2e8580db-6489-4726-96de-e33f5f60295f` |
| `flowSpecificationId` | `RestStorageToAEP` のフロー仕様 ID。 **固定値です**。 | `6499120c-0b15-42dc-936e-847ea3c24d72` |
| `targetConnectionSpecId` | 取り込まれたデータが取り込まれたデータレイクのターゲット接続 ID。 **固定値です**。 | `c604ff05-7f1a-43c0-8e18-33bf874cb11c` |
| `verifyWatTimeInSecond` | フロー実行の完了を確認するときに従う、指定された時間間隔です。 | `40` |
| `startTime` | データフローに指定された開始時間。 開始時間は UNIX 時間でフォーマットする必要があります。 | `1597784298` |

環境変数をすべて指定したら、[!DNL Postman] インターフェイスを使用してコレクションの実行を開始できます。 [!DNL Postman] インターフェイスで、[!DNL Sources SSSs Verification Collection] の横にある省略記号（**...**）を選択し、「**コレクションを実行**」を選択します。

![ ランナー ](../assets/runner.png)

[!DNL Runner] インターフェイスが表示され、データフローの実行順序を設定できます。 「**SSS 検証コレクションを実行**」を選択して、コレクションを実行します。

>[!NOTE]
>
>Experience Platform UI でソースモニタリングダッシュボードを使用する場合は、実行オーダーチェックリストから **フローを削除** を無効にできます。 ただし、テストが完了したら、テストフローが削除されていることを確認する必要があります。

![run-collection](../assets/run-collection.png)

## ソースの送信

ソースがワークフロー全体を完了できるようになったら、Adobe担当者に連絡し、統合のためにソースを送信します。
