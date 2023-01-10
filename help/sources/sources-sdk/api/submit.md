---
keywords: Experience Platform;ホーム;人気の高いトピック;ソース;コネクタ;ソースコネクタ;ソース sdk;SDK;SDK
title: ソースを送信
description: 次のドキュメントでは、フローサービス API を使用して新しいソースをテストおよび検証し、セルフサービスソース（バッチ SDK）を使用して新しいソースを統合する手順を説明します。
exl-id: 9e945ba1-51b6-40a9-b92f-e0a52b3f92fa
source-git-commit: 59dfa862388394a68630a7136dee8e8988d0368c
workflow-type: tm+mt
source-wordcount: '826'
ht-degree: 16%

---

# ソースの送信

セルフサービスソース（バッチ SDK）を使用して新しいソースをAdobe Experience Platformに統合する最後の手順は、ソースを検証用にテストすることです。 成功したら、Adobe担当者に問い合わせて、新しいソースを送信できます。

次のドキュメントでは、 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

* Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../../landing/api-guide.md)のガイドを参照してください。
* Platform API の資格情報を生成する方法について詳しくは、 [Experience PlatformAPI の認証とアクセス](../../../landing/api-authentication.md).
* の設定方法について詳しくは、 [!DNL Postman] Platform API については、 [開発者コンソールの設定および [!DNL Postman]](../../../landing/postman.md).
* テストおよびデバッグプロセスに役立つように、 [セルフサービスソースの検証コレクションと環境をここに](../assets/sdk-verification.zip) およびは、以下に示す手順に従います。

## ソースをテスト

ソースをテストするには、 [セルフサービスソースの検証コレクションと環境](../assets/sdk-verification.zip) オン [!DNL Postman] を使用して、ソースに関連する適切な環境変数を指定する場合。

テストを開始するには、まず、次のようにしてコレクションと環境を設定する必要があります。 [!DNL Postman]. 次に、テストする接続仕様 ID を指定します。

###  `authSpecName`

接続仕様 ID を入力したら、 `authSpecName` をベース接続に使用している。 選択に応じて、次のいずれかを選択します。 `OAuth 2 Refresh Code` または  `Basic Authentication`. 指定した後、 `authSpecName`を使用する場合は、必要な資格情報を環境に含める必要があります。 例えば、 `authSpecName` as `OAuth 2 Refresh Code`の場合、OAuth 2 に必要な資格情報 ( `host` および `accessToken`.

###  `sourceSpec`

認証仕様パラメータを追加したら、次にソース仕様から必要なプロパティを追加する必要があります。 必要なプロパティは、 `sourceSpec.spec.properties`. の場合、 [!DNL MailChimp Members] 以下の例では、必須プロパティは `listId`は、を意味します。 `listId` また、 [!DNL Postman] 環境。

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

認証およびソース指定パラメーターを指定したら、残りの環境変数の入力を開始できます。以下の表を参照してください。

>[!NOTE]
>
>以下の変数の例はすべて、更新が必要なプレースホルダー値です ( ただし、 `flowSpecificationId` および `targetConnectionSpecId`：固定値です。

| パラメーター | 説明 | 例 |
| --- | --- | --- |
| `x-api-key` | Experience PlatformAPI の呼び出しを認証するために使用される一意の識別子。 に関するチュートリアルを参照してください。 [Experience PlatformAPI の認証とアクセス](../../../landing/api-authentication.md) 」を参照してください。 `x-api-key`. | `c8d9a2f5c1e03789bd22e8efdd1bdc1b` |
| `x-gw-ims-org-id` | 製品やサービスを所有またはライセンスし、そのメンバーへのアクセスを許可できる企業エンティティ。 に関するチュートリアルを参照してください。 [開発者コンソールの設定および [!DNL Postman]](../../../landing/postman.md) を参照してください。 `x-gw-ims-org-id` 情報。 | `ABCEH0D9KX6A7WA7ATQE0TE@adobeOrg` |
| `authorizationToken` | Experience PlatformAPI の呼び出しを完了するために必要な認証トークン。 に関するチュートリアルを参照してください。 [Experience PlatformAPI の認証とアクセス](../../../landing/api-authentication.md) 」を参照してください。 `authorizationToken`. | `Bearer authorizationToken` |
| `schemaId` | ソースデータを Platform で使用するには、必要に応じてターゲットスキーマを作成してソースデータを構造化する必要があります。 ターゲット XDM スキーマの作成手順について詳しくは、 [API を使用したスキーマの作成](../../../xdm/api/schemas.md)に関するチュートリアルを参照してください。 | `https://ns.adobe.com/{TENANT_ID}.schemas.0ef4ce0d390f0809fad490802f53d30b` |
| `schemaVersion` | スキーマに対応する一意のバージョン。 | `application/vnd.adobe.xed-full-notext+json; version=1` |
| `schemaAltId` | この `meta:altId` それは横に返される  `schemaId` 新しいスキーマを作成する際に使用します。 | `_{TENANT_ID}.schemas.0ef4ce0d390f0809fad490802f53d30b` |
| `dataSetId` | ターゲットデータセットの作成手順について詳しくは、 [API を使用したデータセットの作成](../../../catalog/api/create-dataset.md)に関するチュートリアルを参照してください。 | `5f3c3cedb2805c194ff0b69a` |
| `mappings` | マッピングセットを使用すると、ソーススキーマ内のデータと宛先スキーマのデータとのマッピング方法を定義できます。マッピングの作成方法について詳しくは、 [API を使用したマッピングセットの作成](../../../data-prep/api/mapping-set.md). | `[{"destinationXdmPath":"person.name.firstName","sourceAttribute":"email.email_id","identity":false,"version":0},{"destinationXdmPath":"person.name.lastName","sourceAttribute":"email.activity.action","identity":false,"version":0}]` |
| `mappingId` | マッピングセットに対応する一意の ID。 | `bf5286a9c1ad4266baca76ba3adc9366` |
| `connectionSpecId` | ソースに対応する接続仕様 ID。 これは、次の後に生成された ID です [新しい接続仕様の作成](./create.md). | `2e8580db-6489-4726-96de-e33f5f60295f` |
| `flowSpecificationId` | のフロー仕様 ID `RestStorageToAEP`. **これは固定値です**. | `6499120c-0b15-42dc-936e-847ea3c24d72` |
| `targetConnectionSpecId` | 取り込んだデータが格納されるデータレイクのターゲット接続 ID。 **これは固定値です**. | `c604ff05-7f1a-43c0-8e18-33bf874cb11c` |
| `verifyWatTimeInSecond` | フロー実行の完了を確認する際に従う指定された時間間隔。 | `40` |
| `startTime` | データフローの指定された開始時間。 開始時刻は UNIX 時刻でフォーマットする必要があります。 | `1597784298` |

すべての環境変数を指定したら、 [!DNL Postman] インターフェイス。 内 [!DNL Postman] インターフェイスで、省略記号 (**...**) を [!DNL Sources SSSs Verification Collection] 次に、 **コレクションを実行**.

![ランナー](../assets/runner.png)

この [!DNL Runner] インターフェイスが表示され、データフローの実行順序を設定できます。 選択 **SSS 検証コレクションを実行** コレクションを実行します。

>[!NOTE]
>
>無効にできます **フローを削除** Platform UI でソース監視ダッシュボードを使用する場合は、実行順序チェックリストから ただし、テストが完了したら、テストフローが削除されていることを確認する必要があります。

![run-collection](../assets/run-collection.png)

## ソースの送信

ソースがワークフロー全体を完了したら、Adobe担当者に問い合わせて、統合用のソースを送信できます。
