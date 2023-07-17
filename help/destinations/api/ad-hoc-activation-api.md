---
keywords: Experience Platform、宛先 API、アドホックアクティベーション、オーディエンスのアドホックアクティブ化
solution: Experience Platform
title: アドホックアクティベーション API を使用して、オーディエンスをバッチ保存先に対してアクティブ化します
description: この記事では、アクティブ化の前におこなわれるセグメント化ジョブなど、アドホックアクティベーション API を介してオーディエンスをアクティブ化するためのエンドツーエンドのワークフローについて説明します。
type: Tutorial
exl-id: 1a09f5ff-0b04-413d-a9f6-57911a92b4e4
source-git-commit: 6304dabb6125b7eddcac16bcbf8abcc36a4c9dc2
workflow-type: tm+mt
source-wordcount: '1550'
ht-degree: 13%

---

# アドホックアクティベーション API を使用して、オーディエンスをオンデマンドでバッチ保存先にアクティブ化します

>[!IMPORTANT]
>
>ベータ段階が完了すると、 [!DNL ad-hoc activation API] は、すべてのExperience Platformのお客様に対して一般提供 (GA) されました。 GA バージョンでは、API がバージョン 2 にアップグレードされています。 手順 4 ([最新のオーディエンス書き出しジョブ ID を取得する](#segment-export-id)) は、API は書き出し ID を必要としなくなったので、今後は必要ありません。
>
>詳しくは、 [アドホックアクティベーションジョブの実行](#activation-job) 詳しくは、このチュートリアルの後半を参照してください。

## 概要 {#overview}

アドホックアクティベーション API を使用すると、マーケターは、即時のアクティベーションが必要な状況に対して、すばやく効率的に、プログラムによってオーディエンスを宛先に対してアクティブ化できます。

アドホックアクティベーション API を使用して、完全なファイルを目的のファイル受信システムに書き出します。 アドホックオーディエンスのアクティベーションは、 [バッチファイルベースの宛先](../destination-types.md#file-based).

次の図に、24 時間ごとに Platform でおこなわれるセグメント化ジョブを含め、アドホックアクティベーション API を介してオーディエンスをアクティブ化するためのエンドツーエンドのワークフローを示します。

![ad-hoc-activation](../assets/api/ad-hoc-activation/ad-hoc-activation-overview.png)



## ユースケース {#use-cases}

### Flashの売り上げまたはプロモーション

あるオンライン小売業者は、限定的なフラッシュ販売を準備しており、顧客に短い通知で通知したいと考えています。 マーケティングチームは、Experience Platformアドホックアクティベーション API を使用して、オーディエンスをオンデマンドでエクスポートし、プロモーション E メールを顧客ベースにすばやく送信できます。

### 最新のイベントまたは最新のニュース

ホテルは、翌日の悪天候を期待し、チームは到着したゲストに迅速に知らせたいので、適切な計画を立てることができます。 マーケティングチームは、Experience Platformアドホックアクティベーション API を使用して、オーディエンスをオンデマンドでエクスポートし、ゲストに通知することができます。

### 統合テスト

IT 管理者は、Experience Platformのアドホックアクティベーション API を使用してオーディエンスをオンデマンドで書き出し、Adobe Experience Platformとのカスタム統合をテストして、すべてが正しく動作していることを確認できます。

## ガードレール {#guardrails}

アドホックアクティベーション API を使用する際は、次のガードレールに注意してください。

* 現在、各アドホックアクティベーションジョブは、最大 80 人のオーディエンスをアクティブ化できます。 1 回のジョブで 80 を超えるオーディエンスをアクティブ化しようとすると、ジョブが失敗します。 この動作は、今後のリリースで変更される可能性があります。
* アドホックアクティベーションジョブは、スケジュール済みと並行して実行できません [オーディエンスの書き出しジョブ](../../segmentation/api/export-jobs.md). アドホックアクティベーションジョブを実行する前に、スケジュールされたオーディエンスの書き出しジョブが終了していることを確認します。 詳しくは、 [宛先のデータフロー監視](../../dataflows/ui/monitor-destinations.md) を参照してください。 例えば、アクティベーションデータフローに **[!UICONTROL 処理中]** のステータス。終了するのを待ってから、アドホックアクティベーションジョブを実行します。
* オーディエンスごとに複数の同時アドホックアクティベーションジョブを実行しないでください。

## セグメント化に関する考慮事項 {#segmentation-considerations}

Adobe Experience Platformは、24 時間に 1 回、スケジュールされたセグメント化ジョブを実行します。 アドホックアクティベーション API は、最新のセグメント化結果に基づいて実行されます。

## 手順 1:前提条件 {#prerequisites}

Adobe Experience Platform API を呼び出す前に、次の前提条件を満たしていることを確認してください。

* Adobe Experience Platformへのアクセス権を持つ組織アカウントがある。
* Experience Platformアカウントに `developer` および `user` 役割がAdobe Experience Platform API 製品プロファイルで有効になっていること お問い合わせ [Admin Console](../../access-control/home.md) 管理者：アカウントに対してこれらのロールを有効にします。
* あなたはAdobe IDを持っている。 Adobe IDがない場合は、 [Adobe Developer Console](https://developer.adobe.com/console) 新しいアカウントを作成します。

## 手順 2:資格情報の収集 {#credentials}

Platform API への呼び出しを実行する前に、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

* Authorization: Bearer `{ACCESS_TOKEN}`
* x-api-key： `{API_KEY}`
* x-gw-ims-org-id: `{ORG_ID}`

Experience Platform のリソースは、特定の仮想サンドボックスに分離することができます。Platform API へのリクエストでは、操作を実行するサンドボックスの名前と ID を指定できます。次に、オプションのパラメーターを示します。

* x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>Experience Platform のサンドボックスについて詳しくは、[サンドボックスの概要ドキュメ ント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* Content-Type: `application/json`

## 手順 3:Platform UI でのアクティベーションフローの作成 {#activation-flow}

アドホックアクティベーション API を使用してオーディエンスをアクティブ化する前に、まず、選択した宛先に対して、Platform UI でアクティベーションフローを設定する必要があります。

これには、アクティベーションワークフロー、オーディエンスの選択、スケジュールの設定、アクティブ化が含まれます。 UI または API を使用して、アクティベーションフローを作成できます。

* [Platform UI を使用して、プロファイル書き出しの宛先をバッチ処理するためのアクティベーションフローを作成します](../ui/activate-batch-profile-destinations.md)
* [フローサービス API を使用して、バッチプロファイル書き出しの宛先に接続し、データをアクティブ化します](../api/connect-activate-batch-destinations.md)

## 手順 4:最新のオーディエンス書き出しジョブ ID の取得（v2 では不要） {#segment-export-id}

>[!IMPORTANT]
>
>アドホックアクティベーション API の v2 では、最新のオーディエンス書き出しジョブ ID を取得する必要はありません。 この手順をスキップして、次の手順に進むことができます。

バッチ宛先のアクティベーションフローを設定すると、スケジュールされたセグメント化ジョブは 24 時間ごとに自動的に実行されます。

アドホックアクティベーションジョブを実行する前に、最新のオーディエンス書き出しジョブの ID を取得する必要があります。 この ID は、アドホックアクティベーションジョブリクエストで渡す必要があります。

説明されている手順に従います [ここ](../../segmentation/api/export-jobs.md#retrieve-list) をクリックして、すべてのオーディエンス書き出しジョブのリストを取得します。

応答で、以下のスキーマプロパティを含む最初のレコードを探します。

```
"schema":{
   "name":"_xdm.context.profile"
}
```

オーディエンス書き出しジョブ ID は、 `id` プロパティに含まれます。

![オーディエンス書き出しジョブ ID](../assets/api/ad-hoc-activation/segment-export-job-id.png)


## 手順 5:アドホックアクティベーションジョブの実行 {#activation-job}

Adobe Experience Platformは、24 時間に 1 回、スケジュールされたセグメント化ジョブを実行します。 アドホックアクティベーション API は、最新のセグメント化結果に基づいて実行されます。

>[!IMPORTANT]
>
>次の 1 回限りの制約に注意してください。アドホックアクティベーションジョブを実行する前に、オーディエンスが最初にアクティブ化された時点から 20 分以上経過していることを確認してください。その時点が、 [手順 3 - Platform UI でアクティベーションフローを作成する](#activation-flow).

アドホックアクティベーションジョブを実行する前に、オーディエンスに対してスケジュールされたオーディエンスの書き出しジョブが終了していることを確認します。 詳しくは、 [宛先のデータフロー監視](../../dataflows/ui/monitor-destinations.md) を参照してください。 例えば、アクティベーションデータフローに **[!UICONTROL 処理中]** のステータス。完了するのを待ってから、アドホックアクティベーションジョブを実行して完全なファイルをエクスポートします。

オーディエンスの書き出しジョブが完了したら、アクティベーションをトリガーできます。

>[!NOTE]
>
>現在、各アドホックアクティベーションジョブは、最大 80 人のオーディエンスをアクティブ化できます。 1 回のジョブで 80 を超えるオーディエンスをアクティブ化しようとすると、ジョブが失敗します。 この動作は、今後のリリースで変更される可能性があります。

### リクエスト {#request}

>[!IMPORTANT]
>
>これには、 `Accept: application/vnd.adobe.adhoc.activation+json; version=2` リクエストのヘッダーを使用して、アドホックアクティベーション API の v2 を使用する必要があります。

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
| <ul><li>`destinationId1`</li><li>`destinationId2`</li></ul> | オーディエンスをアクティブ化する宛先インスタンスの ID。 これらの ID は、 **[!UICONTROL 宛先]** > **[!UICONTROL 参照]** 」タブに移動し、目的の宛先行をクリックして、右側のパネルに宛先 ID を表示します。 詳しくは、 [宛先 workspace に関するドキュメント](/help/destinations/ui/destinations-workspace.md#browse). |
| <ul><li>`segmentId1`</li><li>`segmentId2`</li><li>`segmentId3`</li></ul> | 選択した宛先に対してアクティブ化するオーディエンスの ID。 |

{style="table-layout:auto"}

### 書き出し ID を含むリクエスト（廃止予定） {#request-deprecated}

>[!IMPORTANT]
>
>**非推奨のリクエストタイプ**. このタイプの例は、API バージョン 1 のリクエストタイプを示します。 アドホックアクティベーション API の v2 では、最新のオーディエンス書き出しジョブ ID を含める必要はありません。

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
| <ul><li>`destinationId1`</li><li>`destinationId2`</li></ul> | オーディエンスをアクティブ化する宛先インスタンスの ID。 これらの ID は、 **[!UICONTROL 宛先]** > **[!UICONTROL 参照]** 」タブに移動し、目的の宛先行をクリックして、右側のパネルに宛先 ID を表示します。 詳しくは、 [宛先 workspace に関するドキュメント](/help/destinations/ui/destinations-workspace.md#browse). |
| <ul><li>`segmentId1`</li><li>`segmentId2`</li><li>`segmentId3`</li></ul> | 選択した宛先に対してアクティブ化するオーディエンスの ID。 |
| <ul><li>`exportId1`</li></ul> | ID が [オーディエンスの書き出し](../../segmentation/api/export-jobs.md#retrieve-list) ジョブ。 詳しくは、 [手順 4:最新のオーディエンス書き出しジョブ ID を取得する](#segment-export-id) を参照してください。 |

{style="table-layout:auto"}

### 応答 {#response}

正常な応答は、HTTP ステータス 200 を返します。

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
| `statusURL` | アクティベーションフローのステータス URL。 フローの進行状況は、 [フローサービス API](../../sources/tutorials/api/monitor.md). |

{style="table-layout:auto"}

## API エラー処理 {#api-error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../landing/troubleshooting.md#request-header-errors)を参照してください。

### アドホックアクティベーション API に固有の API エラーコードおよびメッセージ {#specific-error-messages}

アドホックアクティベーション API を使用する場合、この API エンドポイントに固有のエラーメッセージが表示されます。 表を見て、表示される際の対処方法を理解してください。

| エラーメッセージ | 解決策 |
|---------|----------|
| オーディエンスに対して既に実行中です `segment ID` 注文 `dataflow ID` 実行 id を使用 `flow run ID` | このエラーメッセージは、オーディエンスに対するアドホックアクティベーションフローが現在進行中であることを示します。 ジョブが完了するのを待ってから、アクティベーションジョブを再度トリガーします。 |
| セグメント `<segment name>` は、このデータフローに含まれていないか、スケジュール範囲外です。 | このエラーメッセージは、アクティブ化するように選択したオーディエンスがデータフローにマッピングされていない、またはオーディエンス用に設定されたアクティベーションスケジュールが期限切れか、まだ開始されていないことを示します。 オーディエンスが実際にデータフローにマッピングされているかどうかを確認し、オーディエンスのアクティベーションスケジュールが現在の日付と重複していることを確認します。 |

## 関連情報 {#related-information}

* [Flow Service API を使用したバッチ宛先への接続とデータの有効化](/help/destinations/api/connect-activate-batch-destinations.md)
* [（ベータ版）Experience Platform UI を使用した、オンデマンドによるバッチ保存先へのファイルの書き出し](/help/destinations/ui/export-file-now.md)