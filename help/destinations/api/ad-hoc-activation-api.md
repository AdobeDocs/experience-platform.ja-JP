---
keywords: Experience Platform、宛先 API、アドホックアクティベーション、セグメントのアドホックアクティブ化
solution: Experience Platform
title: （ベータ版）アドホックアクティベーション API を使用して、バッチ保存先に対するオーディエンスセグメントをアクティブ化します
description: この記事では、アクティベーションの前におこなわれるセグメント化ジョブなど、アドホックアクティベーション API を介してオーディエンスセグメントをアクティブ化するためのエンドツーエンドのワークフローについて説明します。
topic-legacy: tutorial
type: Tutorial
source-git-commit: 749fa5dc1e8291382408d9b1a0391c4c7f2b2a46
workflow-type: tm+mt
source-wordcount: '1065'
ht-degree: 12%

---


# （ベータ版）アドホックアクティベーション API を使用して、バッチ保存先に対するオーディエンスセグメントをアクティブ化します

>[!IMPORTANT]
>
>この [!DNL ad-hoc activation API] は、現在ベータ版です。 ドキュメントと機能は変更される場合があります。

## 概要 {#overview}

アドホックアクティベーション API を使用すると、マーケターは、即時アクティベーションが必要な状況に対し、すばやく効率的に、宛先に対するオーディエンスセグメントをプログラムでアクティブ化できます。

次の図に、24 時間ごとに Platform でおこなわれるセグメント化ジョブを含む、アドホックアクティベーション API を介してセグメントをアクティブ化するためのエンドツーエンドのワークフローを示します。

![ad-hoc-activation](../assets/api/ad-hoc-activation/ad-hoc-activation-overview.png)

>[!NOTE]
>
>アドホックオーディエンスのアクティベーションは、 [バッチファイルベースの宛先](../destination-types.md#file-based).

## ユースケース {#use-cases}

### Flashの売り上げまたはプロモーション

あるオンライン小売業者は、限定的なフラッシュ販売を準備しており、顧客に短い通知で通知したいと考えています。 Experience Platformアドホックアクティベーション API を使用して、マーケティングチームは、セグメントをオンデマンドで書き出し、プロモーション E メールを顧客ベースにすばやく送信できます。


### 最新のイベントまたは最新のニュース

ホテルは、翌日の悪天候を期待し、チームは到着したゲストに迅速に知らせたいので、適切な計画を立てることができます。 マーケティングチームは、Experience Platformアドホックアクティベーション API を使用して、セグメントをオンデマンドで書き出し、ゲストに通知できます。

### 統合テスト

IT マネージャーは、Experience Platformのアドホックアクティベーション API を使用して、セグメントをオンデマンドで書き出し、Adobe Experience Platformとのカスタム統合をテストして、すべてが正しく動作していることを確認できます。


## ガードレール {#guardrails}

アドホックアクティベーション API を使用する際は、次のガードレールに注意してください。

* 現在、各アドホックアクティベーションジョブは、最大 20 個のセグメントをアクティブ化できます。 1 つのジョブにつき 20 個を超えるセグメントをアクティブ化しようとすると、ジョブが失敗します。 この動作は、今後のリリースで変更される可能性があります。
* アドホックアクティベーションジョブは、スケジュール済みと並行して実行できません [セグメントの書き出しジョブ](../../segmentation/api/export-jobs.md). アドホックアクティベーションジョブを実行する前に、スケジュールされたセグメントの書き出しジョブが終了していることを確認します。 詳しくは、 [宛先のデータフロー監視](../../dataflows/ui/monitor-destinations.md) を参照してください。 例えば、アクティベーションデータフローに **[!UICONTROL 処理中]** のステータス。終了するのを待ってから、アドホックアクティベーションジョブを実行します。
* セグメントごとに複数の同時アドホックアクティベーションジョブを実行しないでください。

## セグメント化に関する考慮事項 {#segmentation-considerations}

Adobe Experience Platformは、24 時間に 1 回、スケジュールされたセグメント化ジョブを実行します。 アドホックアクティベーション API は、最新のセグメント化結果に基づいて実行されます。

## 手順 1:前提条件 {#prerequisites}

Adobe Experience Platform API を呼び出す前に、次の前提条件を満たしていることを確認してください。

* Adobe Experience Platformへのアクセス権を持つ IMS 組織アカウントがある。
* Experience Platformアカウントに `developer` および `user` 役割がAdobe Experience Platform API 製品プロファイルで有効になっていること お問い合わせ [Admin Console](../../access-control/home.md) 管理者：アカウントに対してこれらのロールを有効にします。
* あなたはAdobe IDを持っている。 Adobe IDがない場合は、 [Adobe開発者コンソール](https://developer.adobe.com/console) 新しいアカウントを作成します。

## 手順 2:資格情報の収集 {#credentials}

Platform API への呼び出しを実行する前に、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了する必要があります。認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

* Authorization: Bearer `{ACCESS_TOKEN}`
* x-api-key： `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

Experience Platform のリソースは、特定の仮想サンドボックスに分離することができます。Platform API へのリクエストでは、操作を実行するサンドボックスの名前と ID を指定できます。次に、オプションのパラメーターを示します。

* x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>Experience Platform のサンドボックスについて詳しくは、[サンドボックスの概要ドキュメ ント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* Content-Type: `application/json`

## 手順 3:Platform UI でのアクティベーションフローの作成 {#activation-flow}

アドホックアクティベーション API を使用してセグメントをアクティブ化する前に、まず、選択した宛先に対して、Platform UI でアクティベーションフローを設定する必要があります。

これには、アクティベーションワークフロー、セグメントの選択、スケジュールの設定、アクティブ化が含まれます。

バッチ保存先のアクティベーションフローを設定する方法の詳細については、次のチュートリアルを参照してください。 [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](../ui/activate-batch-profile-destinations.md).

## 手順 4:最新のセグメント書き出しジョブ ID を取得する {#segment-export-id}

バッチ宛先のアクティベーションフローを設定すると、スケジュールされたセグメント化ジョブは 24 時間ごとに自動的に実行されます。

アドホックアクティベーションジョブを実行する前に、最新のセグメント書き出しジョブの ID を取得する必要があります。 この ID は、アドホックアクティベーションジョブリクエストで渡す必要があります。

説明されている手順に従います [ここ](../../segmentation/api/export-jobs.md#retrieve-list) をクリックして、すべてのセグメント書き出しジョブのリストを取得します。

応答で、以下のスキーマプロパティを含む最初のレコードを探します。

```
"schema":{
   "name":"_xdm.context.profile"
}
```

セグメント書き出しジョブ ID は、 `id` プロパティに含まれます。

![セグメント書き出しジョブ ID](../assets/api/ad-hoc-activation/segment-export-job-id.png)


## 手順 5:アドホックアクティベーションジョブの実行 {#activation-job}

Adobe Experience Platformは、24 時間に 1 回、スケジュールされたセグメント化ジョブを実行します。 アドホックアクティベーション API は、最新のセグメント化結果に基づいて実行されます。

アドホックアクティベーションジョブを実行する前に、セグメントのスケジュール済みセグメントの書き出しジョブが終了していることを確認します。 詳しくは、 [宛先のデータフロー監視](../../dataflows/ui/monitor-destinations.md) を参照してください。 例えば、アクティベーションデータフローに **[!UICONTROL 処理中]** のステータス。終了するのを待ってから、アドホックアクティベーションジョブを実行します。

セグメントの書き出しジョブが完了したら、アクティベーションをトリガーできます。

>[!NOTE]
>
>現在、各アドホックアクティベーションジョブは、最大 20 個のセグメントをアクティブ化できます。 1 つのジョブにつき 20 個を超えるセグメントをアクティブ化しようとすると、ジョブが失敗します。 この動作は、今後のリリースで変更される可能性があります。

### リクエスト

```shell
curl -X POST https://platform.adobe.io/data/core/activation/disflowprovider/adhocrun \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
| <ul><li>`destinationId1`</li><li>`destinationId2`</li></ul> | セグメントをアクティブ化する宛先インスタンスの ID。 |
| <ul><li>`segmentId1`</li><li>`segmentId2`</li><li>`segmentId3`</li></ul> | 選択した宛先に対してアクティブ化するセグメントの ID。 |
| <ul><li>`exportId1`</li></ul> | ID が [セグメントの書き出し](../../segmentation/api/export-jobs.md#retrieve-list) ジョブ。 詳しくは、 [手順 4:最新のセグメント書き出しジョブ ID を取得する](#segment-export-id) を参照してください。 |

### 応答

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
| `segment` | アクティブ化されたセグメントの ID。 |
| `order` | セグメントがアクティブ化された宛先の ID。 |
| `statusURL` | アクティベーションフローのステータス URL。 フローの進行状況は、 [フローサービス API](../../sources/tutorials/api/monitor.md). |


## API エラー処理

宛先 SDK API エンドポイントは、Experience PlatformAPI エラーメッセージの一般的な原則に従います。 参照： [API ステータスコード](https://experienceleague.adobe.com/docs/experience-platform/landing/troubleshooting.html?lang=en#api-status-codes) および [リクエストヘッダーエラー](https://experienceleague.adobe.com/docs/experience-platform/landing/troubleshooting.html?lang=en#request-header-errors) （Platform トラブルシューティングガイド）を参照してください。
