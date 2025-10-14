---
keywords: Experience Platform;destination api；アドホックアクティベーション；アドホックオーディエンスのアクティブ化
solution: Experience Platform
title: アドホックアクティベーション API を介して、バッチ宛先に対するオーディエンスをアクティブ化します。
description: この記事では、アドホックアクティベーション API を介してオーディエンスをアクティブ化するためのエンドツーエンドのワークフローを、アクティベーション前に行われるセグメント化ジョブも含めて説明します。
type: Tutorial
exl-id: 1a09f5ff-0b04-413d-a9f6-57911a92b4e4
source-git-commit: 35429ec2dffacb9c0f2c60b608561988ea487606
workflow-type: tm+mt
source-wordcount: '1623'
ht-degree: 9%

---

# アドホックアクティベーション API を使用して、オンデマンドでオーディエンスをバッチ宛先に対してアクティブ化します

>[!IMPORTANT]
>
>Beta フェーズが完了すると、すべてのExperience Platform ユーザーが [!DNL ad-hoc activation API] を一般公開（GA）できるようになりました。 GA バージョンでは、API がバージョン 2 にアップグレードされました。 API が書き出し ID を必要としなくなったので、手順 4 （[&#x200B; 最新のオーディエンスエクスポートジョブ ID の取得 &#x200B;](#segment-export-id)）は不要になりました。
>
>詳しくは、このチュートリアルの後述の [&#x200B; アドホックアクティベーションジョブの実行 &#x200B;](#activation-job) を参照してください。

## 概要 {#overview}

アドホックアクティベーション API を使用すると、マーケターは、即時にアクティベーションが必要な状況で、宛先へのオーディエンスオーディエンスをプログラムによってすばやく効率的にアクティブ化できます。

アドホックアクティベーション API を使用して、完全なファイルを目的のファイル受信システムに書き出します。 アドホックオーディエンスのアクティベーションは、[&#x200B; バッチファイルベースの宛先 &#x200B;](../destination-types.md#file-based) でのみサポートされています。

次の図は、アドホックアクティベーション API を介してオーディエンスをアクティブ化するためのエンドツーエンドのワークフローを示しています。これには、24 時間ごとにExperience Platformで行われるセグメント化ジョブが含まれます。

![&#x200B; アドホックアクティベーション &#x200B;](../assets/api/ad-hoc-activation/ad-hoc-activation-overview.png)

## ユースケース {#use-cases}

### Flash の販売またはプロモーション

あるオンラインretailerは、限定フラッシュセールを準備中で、お客様に短い通知をしたいと考えています。 Experience Platform アドホックアクティベーション API を通じて、マーケティングチームはオーディエンスをオンデマンドで書き出し、プロモーションメールをすばやく顧客ベースに送信できます。

### 現在のイベントまたは最新ニュース

ホテルは次の日に悪天候を予想し、チームは到着したゲストに迅速に通知したいので、それに応じて計画することができます。 マーケティングチームは、Experience Platform アドホックアクティベーション API を使用して、オーディエンスをオンデマンドで書き出し、ゲストに通知できます。

### 統合テスト

IT 管理者は、Experience Platform アドホックアクティベーション API を使用して、オーディエンスをオンデマンドで書き出すことができます。これにより、Adobe Experience Platformとのカスタム統合をテストし、すべてが正しく機能していることを確認できます。

## ガードレール {#guardrails}

アドホックアクティベーション API を使用する場合は、次のガードレールに注意してください。

* 現在、各アドホックアクティベーションジョブは、最大 80 個のオーディエンスをアクティベートできます。 1 つのジョブにつき 80 個を超えるオーディエンスをアクティベートしようとすると、ジョブが失敗します。 この動作は、今後のリリースで変更される可能性があります。
* アドホックアクティベーションジョブを、スケジュールされた [&#x200B; オーディエンス書き出しジョブ &#x200B;](../../segmentation/api/export-jobs.md) と並行して実行することはできません。 アドホックアクティベーションジョブを実行する前に、スケジュールされたオーディエンス書き出しジョブが完了していることを確認します。 アクティブ化フローのステータスを監視する方法については、[&#x200B; 宛先データフローの監視 &#x200B;](../../dataflows/ui/monitor-destinations.md) を参照してください。 例えば、アクティベーションデータフローに **[!UICONTROL 処理中]** ステータスが表示されている場合は、完了するまで待ってからアドホックアクティベーションジョブを実行します。
* オーディエンスごとに複数の同時アドホックアクティベーションジョブを実行しないでください。

## セグメント化に関する考慮事項 {#segmentation-considerations}

Adobe Experience Platformは、スケジュールされたセグメント化ジョブを 24 時間ごとに 1 回実行します。 アドホックアクティベーション API は、最新のセグメント化結果に基づいて実行されます。

## 手順 1：前提条件 {#prerequisites}

Adobe Experience Platform API を呼び出す前に、次の前提条件を満たしていることを確認してください。

* Adobe Experience Platformにアクセスできる組織アカウントがある。
* Experience Platform アカウントでは、`developer` と `user` の役割がAdobe Experience Platform API 製品プロファイルに対して有効になっています。 アカウントでこれらの役割を有効にするには、[Admin Console](../../access-control/home.md) 管理者にお問い合わせください。
* Adobe IDがある。 Adobe IDがない場合は、[Adobe Developer Consoleに移動して &#x200B;](https://developer.adobe.com/console) 新しいアカウントを作成します。

## 手順 2：資格情報の収集 {#credentials}

Experience Platform API を呼び出すには、まず[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

* Authorization: Bearer `{ACCESS_TOKEN}`
* x-api-key： `{API_KEY}`
* x-gw-ims-org-id: `{ORG_ID}`

Experience Platform のリソースは、特定の仮想サンドボックスに分離することができます。Experience Platform API へのリクエストでは、操作を実行するサンドボックスの名前と ID を指定できます。 次に、オプションのパラメーターを示します。

* x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>Experience Platform のサンドボックスについて詳しくは、[サンドボックスの概要ドキュメ ント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* Content-Type: `application/json`

## 手順 3:Experience Platform UI でのアクティベーションフローの作成 {#activation-flow}

アドホックアクティベーション API を通じてオーディエンスをアクティブ化する前に、選択した宛先に対して、Experience Platform UI でアクティベーションフローを設定する必要があります。

これには、アクティベーションワークフローへの移行、オーディエンスの選択、スケジュールの設定、オーディエンスのアクティベートなどが含まれます。 UI または API を使用して、アクティベーションフローを作成できます。

* [Experience Platform UI を使用して、プロファイル書き出しのバッチ宛先へのアクティベーションフローを作成します](../ui/activate-batch-profile-destinations.md)
* [Flow Service API を使用して、プロファイル書き出しのバッチ宛先に接続し、データを有効化する](../api/connect-activate-batch-destinations.md)

## 手順 4：最新のオーディエンスエクスポートジョブ ID の取得（v2 では不要） {#segment-export-id}

>[!IMPORTANT]
>
>アドホックアクティベーション API の v2 では、最新のオーディエンスエクスポートジョブ ID を取得する必要はありません。 この手順をスキップして、次の手順に進むことができます。

バッチ宛先のアクティベーションフローを設定すると、スケジュールされたセグメント化ジョブは 24 時間ごとに自動的に実行を開始します。

アドホックアクティベーションジョブを実行する前に、最新のオーディエンスエクスポートジョブの ID を取得する必要があります。 この ID をアドホックアクティベーションジョブリクエストに渡す必要があります。

すべてのオーディエンス書き出しジョブのリストを取得するには、[&#x200B; こちら &#x200B;](../../segmentation/api/export-jobs.md#retrieve-list) に記載されている手順に従います。

応答で、以下のスキーマプロパティを含む最初のレコードを探します。

```
"schema":{
   "name":"_xdm.context.profile"
}
```

次に示すように、オーディエンス書き出しジョブ ID は `id` プロパティにあります。

![&#x200B; オーディエンスエクスポートジョブ ID](../assets/api/ad-hoc-activation/segment-export-job-id.png)


## 手順 5：アドホックアクティベーションジョブの実行 {#activation-job}

Adobe Experience Platformは、スケジュールされたセグメント化ジョブを 24 時間ごとに 1 回実行します。 アドホックアクティベーション API は、最新のセグメント化結果に基づいて実行されます。

>[!IMPORTANT]
>
>次の 1 回限りの制約に注意します。アドホックアクティベーションジョブを実行する前に、[&#x200B; 手順 3 - Experience Platform UI でのアクティベーションフローの作成 &#x200B;](#activation-flow) で設定したスケジュールに従って、オーディエンスが最初にアクティベートされた瞬間から少なくとも 1 時間が経過していることを確認してください。

アドホックアクティベーションジョブを実行する前に、オーディエンスのスケジュールされたオーディエンス書き出しジョブが完了していることを確認します。 アクティブ化フローのステータスを監視する方法については、[&#x200B; 宛先データフローの監視 &#x200B;](../../dataflows/ui/monitor-destinations.md) を参照してください。 例えば、アクティベーションデータフローに **[!UICONTROL 処理中]** ステータスが表示された場合は、完了するまで待ってから、アドホックアクティベーションジョブを実行して完全なファイルを書き出します。

オーディエンスの書き出しジョブが完了したら、アクティベーションをトリガーできます。

>[!NOTE]
>
>現在、各アドホックアクティベーションジョブは、最大 80 個のオーディエンスをアクティベートできます。 1 つのジョブにつき 80 個を超えるオーディエンスをアクティベートしようとすると、ジョブが失敗します。 この動作は、今後のリリースで変更される可能性があります。

### リクエスト {#request}

>[!IMPORTANT]
>
>アドホックアクティベーション API v2 を使用するには、リクエストに `Accept: application/vnd.adobe.adhoc.activation+json; version=2` ヘッダーを含める必要があります。

```shell
curl --location --request POST 'https://platform.adobe.io/data/core/activation/disflowprovider/adhocrun' \
--header 'x-gw-ims-org-id: 5555467B5D8013E50A494220@AdobeOrg' \
--header 'Authorization: Bearer {{token}}' \
--header 'x-sandbox-id: 6ef74723-3ee7-46a4-b747-233ee7a6a41a' \
--header 'x-sandbox-name: {sandbox-id}' \
--header 'Accept: application/vnd.adobe.adhoc.activation+json; version=2' \
--header 'Content-Type: application/json' \
--data-raw '{
   "activationInfo":{
      "destinationId1":[
         "segmentId1",
         "segmentId2"
      ],
      "destinationId2":[
         "segmentId2",
         "segmentId3"
      ]
   }
}'
```

| プロパティ | 説明 |
| -------- | ----------- |
| <ul><li>`destinationId1`</li><li>`destinationId2`</li></ul> | オーディエンスをアクティベートする宛先インスタンスの ID。 Experience Platform UI からこれらの ID を取得するには、「**[!UICONTROL 宛先]**/**[!UICONTROL 参照]** タブに移動し、目的の宛先行をクリックして、右側のパネルに宛先 ID を表示します。 詳しくは、[&#x200B; 宛先ワークスペードキュメント &#x200B;](/help/destinations/ui/destinations-workspace.md#browse) を参照してください。 |
| <ul><li>`segmentId1`</li><li>`segmentId2`</li><li>`segmentId3`</li></ul> | 選択した宛先に対してアクティブ化するオーディエンスの ID。 アドホック API を使用して、Experience Platformで生成されたオーディエンスと外部（カスタムアップロード）オーディエンスを書き出すことができます。 外部オーディエンスをアクティブ化する場合は、オーディエンス ID ではなく、システムで生成された ID を使用します。 システム生成 ID は、オーディエンス UI のオーディエンスの概要ビューで確認できます。<br> ![&#x200B; 選択すべきでないオーディエンス ID のビュー。](/help/destinations/assets/api/ad-hoc-activation/audience-id-do-not-use.png " 選択すべきでないオーディエンス ID のビュー "){width="100" zoomable="yes"}<br> ![&#x200B; 使用する必要があるシステム生成オーディエンス ID の表示。](/help/destinations/assets/api/ad-hoc-activation/system-generated-id-to-use.png " 使用する必要があるシステム生成オーディエンス ID の表示 "){width="100" zoomable="yes"} |

{style="table-layout:auto"}

### 書き出し ID を持つリクエスト {#request-export-ids}

<!--

>[!IMPORTANT]
>
>**Deprecated request type**. This example type describes the request type for the API version 1. In the v2 of the ad-hoc activation API, you do not need to include the latest audience export job ID.

-->

```shell
curl -X POST https://platform.adobe.io/data/core/activation/disflowprovider/adhocrun \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -d '
{
   "activationInfo":{
      "destinationId1":[
         "segmentId1",
         "segmentId2"
      ],
      "destinationId2":[
         "segmentId2",
         "segmentId3"
      ]
   },
   "exportIds":[
      "exportId1"
   ]
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| <ul><li>`destinationId1`</li><li>`destinationId2`</li></ul> | オーディエンスをアクティベートする宛先インスタンスの ID。 Experience Platform UI からこれらの ID を取得するには、「**[!UICONTROL 宛先]**/**[!UICONTROL 参照]** タブに移動し、目的の宛先行をクリックして、右側のパネルに宛先 ID を表示します。 詳しくは、[&#x200B; 宛先ワークスペードキュメント &#x200B;](/help/destinations/ui/destinations-workspace.md#browse) を参照してください。 |
| <ul><li>`segmentId1`</li><li>`segmentId2`</li><li>`segmentId3`</li></ul> | 選択した宛先に対してアクティブ化するオーディエンスの ID。 |
| <ul><li>`exportId1`</li></ul> | [&#x200B; オーディエンスの書き出し &#x200B;](../../segmentation/api/export-jobs.md#retrieve-list) ジョブの応答で返された ID。 [&#x200B; 手順 4：最新のオーディエンスエクスポートジョブ ID を取得する &#x200B;](#segment-export-id) を参照して、この ID を見つける方法を確認してください。 |

{style="table-layout:auto"}

### 応答 {#response}

リクエストが成功した場合は、HTTP ステータス 200 が返されます。

```shell
{
   "order":[
      {
         "segment":"db8961e9-d52f-45bc-b3fb-76d0382a6851",
         "order":"ef2dcbd6-36fc-49a3-afed-d7b8e8f724eb",
         "statusURL":"https://platform.adobe.io/data/foundation/flowservice/runs/88d6da63-dc97-460e-b781-fc795a7386d9"
      }
   ]
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `segment` | アクティブ化されたオーディエンスの ID。 |
| `order` | オーディエンスがアクティブ化された宛先の ID。 |
| `statusURL` | アクティベーションフローのステータス URL。 [Flow Service API](../../sources/tutorials/api/monitor.md) を使用して、フローの進行状況を追跡できます。 |

{style="table-layout:auto"}

## API エラー処理 {#api-error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Experience Platform トラブルシューティングガイドの [API ステータスコード &#x200B;](../../landing/troubleshooting.md#api-status-codes) および [&#x200B; リクエストヘッダーエラー &#x200B;](../../landing/troubleshooting.md#request-header-errors) を参照してください。

### アドホックアクティベーション API に固有の API エラーコードとメッセージ {#specific-error-messages}

アドホックアクティベーション API を使用する場合、この API エンドポイントに固有のエラーメッセージが表示される場合があります。 テーブルを確認して、表示されたときに対処する方法を理解します。

| エラーメッセージ | 解決策 |
|---------|----------|
| 実行 ID `segment ID` の注文 `dataflow ID` に対して、オーディエンス `flow run ID` に対する実行は既に行われています | このエラーメッセージは、アドホックアクティベーションフローが現在オーディエンスに対して進行中であることを示しています。 ジョブが終了するのを待ってから、アクティベーションジョブを再度トリガーします。 |
| セグメント `<segment name>` このデータフローの一部ではないか、スケジュール範囲外です。 | このエラーメッセージは、アクティブ化するように選択したオーディエンスがデータフローにマッピングされていないか、オーディエンスに対して設定されたアクティベーションスケジュールが期限切れか、まだ開始されていないことを示しています。 オーディエンスが実際にデータフローにマッピングされているかどうかを確認し、オーディエンスのアクティベーションスケジュールが現在の日付と重なっていることを確認します。 |

## 関連情報 {#related-information}

* [Flow Service API を使用したバッチ宛先への接続とデータの有効化](/help/destinations/api/connect-activate-batch-destinations.md)
* [（ベータ版）Experience Platform UI を使用した、オンデマンドによるバッチ保存先へのファイルの書き出し](/help/destinations/ui/export-file-now.md)