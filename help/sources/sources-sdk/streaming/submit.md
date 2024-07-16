---
title: Sourceのテストと送信
description: 次のドキュメントでは、Flow Service API を使用して新しいソースをテストおよび検証し、セルフサービスソース（ストリーミング SDK）を通じて新しいソースを統合する手順を説明します。
exl-id: 2ae0c3ad-1501-42ab-aaaa-319acea94ec2
badge: ベータ版
source-git-commit: 256857103b4037b2cd7b5b52d6c5385121af5a9f
workflow-type: tm+mt
source-wordcount: '1265'
ht-degree: 22%

---

# ソースのテストと送信

>[!NOTE]
>
>セルフサービスソース Streaming SDK はベータ版です。 ベータラベル付きソースの使用について詳しくは、[ ソースの概要 ](../../home.md#terms-and-conditions) を参照してください。

セルフサービスソース（ストリーミング SDK）を使用して新しいソースをAdobe Experience Platformに統合する最後の手順は、新しいソースをテストして送信することです。 接続仕様を完了し、ストリーミングフロー仕様を更新したら、API または UI を使用してソースの機能のテストを開始できます。 成功したら、Adobe担当者に連絡して、新しいソースを送信できます。

次のドキュメントでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用してソースをテストおよびデバッグする手順を説明します。

## はじめに

* Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../../landing/api-guide.md)のガイドを参照してください。
* Platform API の資格情報の生成方法について詳しくは、[Experience PlatformAPI の認証とアクセス ](../../../landing/api-authentication.md) に関するチュートリアルを参照してください。
* Platform API の [!DNL Postman] の設定方法について詳しくは、[ 開発者コンソールとの設定  [!DNL Postman]](../../../landing/postman.md) のチュートリアルを参照してください。
* テストとデバッグプロセスに役立つように、[ セルフサービスソースの検証コレクションと環境をこちらからダウンロードし ](../assets/sdk-verification.zip) 以下の手順に従ってください。

## API を使用したソースのテスト

API を使用してソースをテストするには、ソースに関連する適切な環境変数を指定しながら、[!DNL Postman] で [ セルフサービスソース検証コレクションおよび環境 ](../assets/sdk-verification.zip) を実行する必要があります。

テストを開始するには、まず [!DNL Postman] でコレクションと環境を設定する必要があります。 次に、テストする接続仕様 ID を指定します。

>[!NOTE]
>
>以下に示すサンプル変数はすべて、更新が必要なプレースホルダー値です。ただし、`flowSpecificationId` と `targetConnectionSpecId` は固定値です。

| パラメーター | 説明 | 例 |
| --- | --- | --- |
| `x-api-key` | Experience PlatformAPI への呼び出しの認証に使用される一意の ID。 サー `x-api-key` スの取得方法について詳しくは、[Experience Platform API の認証とアクセス ](../../../landing/api-authentication.md) に関するチュートリアルを参照してください。 | `c8d9a2f5c1e03789bd22e8efdd1bdc1b` |
| `x-gw-ims-org-id` | 製品およびサービスを所有またはライセンスし、そのメンバーへのアクセスを許可できる法人組織。 `x-gw-ims-org-id` ーザー情報の取得方法については、[Developer Console の設定および  [!DNL Postman]](../../../landing/postman.md) に関するチュートリアルを参照してください。 | `ABCEH0D9KX6A7WA7ATQE0TE@adobeOrg` |
| `authorizationToken` | Experience PlatformAPI を呼び出すために必要な認証トークン。 サー `authorizationToken` スの取得方法について詳しくは、[Experience Platform API の認証とアクセス ](../../../landing/api-authentication.md) に関するチュートリアルを参照してください。 | `Bearer authorizationToken` |
| `schemaId` | ソースデータを Platform で使用するには、必要に応じてターゲットスキーマを作成してソースデータを構造化する必要があります。 ターゲット XDM スキーマの作成手順について詳しくは、 [API を使用したスキーマの作成](../../../xdm/api/schemas.md)に関するチュートリアルを参照してください。 | `https://ns.adobe.com/{TENANT_ID}.schemas.0ef4ce0d390f0809fad490802f53d30b` |
| `schemaVersion` | スキーマに対応する一意のバージョン。 | `application/vnd.adobe.xed-full-notext+json; version=1` |
| `schemaAltId` | 新しいスキーマを作成する際に `schemaId` と共に返される `meta:altId`。 | `_{TENANT_ID}.schemas.0ef4ce0d390f0809fad490802f53d30b` |
| `dataSetId` | ターゲットデータセットの作成手順について詳しくは、 [API を使用したデータセットの作成](../../../catalog/api/create-dataset.md)に関するチュートリアルを参照してください。 | `5f3c3cedb2805c194ff0b69a` |
| `mappings` | マッピングセットを使用すると、ソーススキーマ内のデータと宛先スキーマのデータとのマッピング方法を定義できます。マッピングの作成方法に関する詳細な手順については、[API を使用したマッピングセットの作成 ](../../../data-prep/api/mapping-set.md) に関するチュートリアルを参照してください。 | `[{"destinationXdmPath":"person.name.firstName","sourceAttribute":"email.email_id","identity":false,"version":0},{"destinationXdmPath":"person.name.lastName","sourceAttribute":"email.activity.action","identity":false,"version":0}]` |
| `mappingId` | マッピングセットに対応する一意の ID。 | `bf5286a9c1ad4266baca76ba3adc9366` |
| `connectionSpecId` | ソースに対応する接続仕様 ID。 これは、[ 新しい接続仕様の作成 ](./create.md) 後に生成した ID です。 | `2e8580db-6489-4726-96de-e33f5f60295f` |
| `flowSpecificationId` | `GenericStreamingAEP` のフロー仕様 ID。 **固定値です**。 | `e77fde5a-22a8-11ed-861d-0242ac120002` |
| `targetConnectionSpecId` | 取り込まれたデータが取り込まれたデータレイクのターゲット接続 ID。 **固定値です**。 | `c604ff05-7f1a-43c0-8e18-33bf874cb11c` |
| `verifyWatTimeInSecond` | フロー実行の完了を確認するときに従う、指定された時間間隔です。 | `40` |
| `startTime` | データフローに指定された開始時間。 開始時間は UNIX 時間でフォーマットする必要があります。 | `1597784298` |

環境変数をすべて指定したら、[!DNL Postman] インターフェイスを使用してコレクションの実行を開始できます。 [!DNL Postman] インターフェイスで、[!DNL Sources SSSs Verification Collection] の横にある省略記号（**...**）を選択し、「**コレクションを実行**」を選択します。

![ ランナー ](../assets/runner.png)

[!DNL Runner] インターフェイスが表示され、データフローの実行順序を設定できます。 「**SSS 検証コレクションを実行**」を選択して、コレクションを実行します。

>[!NOTE]
>
>Platform UI でソース監視ダッシュボードを使用する場合は、実行順序チェックリストから **フローを削除** を無効にできます。 ただし、テストが完了したら、テストフローが削除されていることを確認する必要があります。

![run-collection](../assets/run-collection.png)

## UI を使用したソースのテスト

UI でソースをテストするには、Platform UI で組織のサンドボックスのソースカタログに移動します。 ここから、新しいソースが *ストリーミング* カテゴリの下に表示されます。

サンドボックスで新しいソースを使用できるようになったら、ソースワークフローに従って機能をテストする必要があります。 開始するには、「**[!UICONTROL 設定]**」を選択します。

![ 新しいストリーミングソースを表示するソースカタログ ](../assets/testing/catalog-test.png)

[!UICONTROL データを追加]手順が表示されます。ソースがデータをストリーミングできるかどうかをテストするには、インターフェイスの左側を使用して [ サンプル JSON データ ](../assets/testing/raw.json.zip) をアップロードします。 データがアップロードされると、インターフェイスの右側が更新され、データのファイル階層のプレビューが表示されます。 「**[!UICONTROL 次へ]**」を選択して次に進みます。

![ 取り込み前にデータのアップロードとプレビューを行えるソースワークフローのデータを追加ステップ ](../assets/testing/add-data-test.png)

[!UICONTROL データフロー詳細]ページでは、既存のデータセットと新しいデータセットのどちらを使用するかを選択できます。このプロセスの間に、プロファイルに取り込むデータを設定し、[!UICONTROL  エラー診断 ] や [!UICONTROL  部分取り込み ] などの設定を有効にすることもできます。

テストするには、「**[!UICONTROL 新しいデータセット]**」を選択し、出力データセット名を入力します。 この手順では、データセットにさらに情報を追加するためのオプションの説明を入力することもできます。 次に、「[!UICONTROL 詳細検索]」オプションを使用するか、ドロップダウンメニューで既存のスキーマのリストをスクロールして、マッピングするスキーマを選択します。スキーマを選択したら、データフローの名前と説明を指定します。

終了したら、「**[!UICONTROL 次へ]**」を選択します。

![ ソースワークフローのデータフローの詳細手順。](../assets/testing/dataflow-details-test.png)

[!UICONTROL マッピング]手順が表示され、ソーススキーマのソースフィールドを、ターゲットスキーマの適切なターゲット XDM フィールドにマッピングするためのインターフェイスが提供されます。

Platform は、選択したターゲットスキーマまたはデータセットに基づいて、自動マッピングされたフィールドに対してインテリジェントなレコメンデーションを提供します。 マッピングルールは、ユースケースに合わせて手動で調整できます。 必要に応じて、フィールドを直接マッピングするか、データ準備機能を使用してソースデータを変換して計算値を導き出すかを選択できます。マッパーインターフェイスと計算フィールドの使用に関する包括的な手順については、[ データ準備 UI ガイド」を参照してください ](../../../data-prep/ui/mapping.md)

ソースデータが正常にマッピングされたら、「**[!UICONTROL 次へ]**」を選択します。

![ ソースワークフローのマッピングステップ ](../assets/testing/mapping-test.png)

**[!UICONTROL レビュー]**&#x200B;手順が表示され、新しいデータフローを作成する前に確認できます。詳細は、次のカテゴリに分類されます。

* **[!UICONTROL 接続]**：アカウント名、ソースのタイプ、および使用しているストリーミングクラウドストレージソースに固有のその他の情報を表示します。
* **[!UICONTROL データセットの割り当てとフィールドのマッピング]**：データフローに使用するターゲットデータセットとスキーマを表示します。

データフローをレビューしたら、「**[!UICONTROL 終了]**」を選択し、データフローが作成されるまでしばらく待ちます。

![ ソースワークフローのレビュー手順。](../assets/testing/review-test.png)

最後に、データフローのストリーミングエンドポイントを取得します。 このエンドポイントは、Webhook をサブスクライブするために使用され、ストリーミングソースがExperience Platformと通信できるようになります。 ストリーミングエンドポイントを取得するには、作成したデータフローの [!UICONTROL  データフローアクティビティ ] ページに移動し、[!UICONTROL  プロパティ ] パネルの下部からエンドポイントをコピーします。

![ データフローアクティビティのストリーミングエンドポイント。](../assets/testing/endpoint-test.png)

## ソースの送信

ソースがワークフロー全体を完了できるようになったら、Adobe担当者に連絡し、他のExperience Platform組織と統合するためにソースを送信します。
