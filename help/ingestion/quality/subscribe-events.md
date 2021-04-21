---
keywords: Experience Platform；ホーム；人気のあるトピック；データ取り込み通知；通知；購読イベント；データ取り込みステータスイベント；ステータスイベント；購読；ステータス通知；
solution: Experience Platform
title: データ取り込み通知
topic-legacy: overview
description: Adobe Experience Platformは、取り込みプロセスの監視を支援するため、プロセスの各ステップで公開された一連のイベントを登録し、取り込まれたデータの状態と発生し得る障害を通知する。
exl-id: fd34e1ab-f6f6-44f0-88ee-7020e9322c39
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '681'
ht-degree: 29%

---

# データ取得通知

Adobe Experience Platform でデータを取得するプロセスは、複数の手順で構成されます。[!DNL Platform]に取り込む必要のあるデータファイルを特定すると、取り込みプロセスが開始され、データが正常に取り込まれるか、または失敗するまで各ステップが連続して行われます。 取得処理は、[Adobe データ取得 API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml) を使用するか、Experience Platform のユーザーインターフェイスを使用して開始することができます。[!DNL Experience Platform]

[!DNL Platform]に読み込まれたデータは、目的のデータストア、[!DNL Data Lake]データストア、または[!DNL Real-time Customer Profile]データストアに到達するために複数の手順を経る必要があります。 各手順では、データの処理やデータの検証が行われ、データが次の手順に渡される前にデータが保存されます。取得されるデータの量によっては、この処理に時間がかかる場合があり、検証、セマンティクスまたは処理エラーが原因でプロセスが失敗する可能性が常にあります。失敗した場合は、データの問題を修正し、修正したデータファイルを使用して取得プロセス全体を再開する必要があります。

[!DNL Experience Platform]は、取り込みプロセスの監視を支援するため、プロセスの各ステップで公開された一連のイベントを登録し、取り込まれたデータの状態と発生する可能性のあるエラーを通知する。

## データインジェスト通知用のWebフックの登録

データインジェスト通知を受け取るには、[Adobe開発者コンソール](https://www.adobe.com/go/devs_console_ui)を使用して、WebフックをExperience Platform統合に登録する必要があります。

これを行う方法の詳細な手順については、[ [!DNL Adobe I/O Event] 通知](../../observability/notifications/subscribe.md)のサブスクライブのチュートリアルに従ってください。

>[!IMPORTANT]
>
>購読プロセス中に、イベントプロバイダーとして&#x200B;**[!UICONTROL プラットフォーム通知]**&#x200B;を選択し、プロンプトが表示されたら&#x200B;**[!UICONTROL データ取り込み通知]**&#x200B;イベント購読を選択します。

## データ取り込み通知の受信

Webフックの登録が完了し、新しいデータが取り込まれると、イベント通知の受信開始を設定できます。 これらのイベントは、Webフック自体を使用して表示するか、Adobe開発者コンソールでプロジェクトのイベント登録の概要にある[**[!UICONTROL Debug Tracing]**]タブを選択して表示できます。

以下のJSONは、バッチインジェストイベントが失敗した場合にWebフックに送信される通知ペイロードの例です。

```json
{
  "event_id": "93a5b11a-b0e6-4b29-ad82-81b1499cb4f2",
  "event": {
    "xdm:ingestionId": "01EGK8H8HF9JGFKNDCABHGA24G",
    "xdm:customerIngestionId": "01EGK8H8HF9JGFKNDCABHGA24G",
    "xdm:imsOrg": "{IMS_ORG}",
    "xdm:completed": 1598374341560,
    "xdm:datasetId": "5e55b556c2ae4418a8446037",
    "xdm:eventCode": "ing_load_failure",
    "xdm:sandboxName": "prod",
    "sentTime": "1598374341595",
    "processStartTime": 1598374342614,
    "transformedTime": 1598374342621,
    "header": {
      "_adobeio": {
        "imsOrgId": "{IMS_ORG}",
        "providerMetadata": "aep_observability_catalog_events",
        "eventCode": "platform_event"
      }
    }
  }
}
```

| プロパティ | 説明 |
| --- | --- |
| `event_id` | 通知用の、システム生成の一意のID。 |
| `event` | 通知をトリガーしたイベントの詳細を含むオブジェクトです。 |
| `event.xdm:datasetId` | インジェストイベントが適用されるデータセットのID。 |
| `event.xdm:eventCode` | データセットに対してトリガーされたイベントのタイプを示すステータスコード。 特定の値とその定義については、[付録](#event-codes)を参照してください。 |

イベント通知の完全なスキーマを表示するには、[パブリックGitHubリポジトリ](https://github.com/adobe/xdm/blob/master/schemas/notifications/ingestion.schema.json)を参照してください。

## 次の手順

プロジェクトに[!DNL Platform]通知を登録すると、[!UICONTROL プロジェクトの概要]から表示が受け取ったイベントを確認できます。 イベントのトレース方法の詳細な手順については、[Adobe I/Oイベント](https://www.adobe.io/apis/experienceplatform/events/docs.html#!adobedocs/adobeio-events/master/support/tracing.md)のトレースガイドを参照してください。

## 付録

次の節では、データインジェスト通知ペイロードの解釈に関する追加情報を説明します。

### 使用可能なステータス通知イベント {#event-codes}

次の表に、サブスクライブ可能なデータインジェストステータス通知をリストします。

| イベントコード | プラットフォームサービス | ステータス | イベントの説明 |
| --- | ---------------- | ------ | ----------------- |
| `ing_load_success` | [!DNL Data Ingestion] | 成功 | バッチが[!DNL Data Lake]内のデータセットに取り込まれました。 |
| `ing_load_failure` | [!DNL Data Ingestion] | 失敗 | バッチを[!DNL Data Lake]内のデータセットに取り込めませんでした。 |
| `ps_load_success` | [!DNL Real-time Customer Profile] | 成功 | バッチが[!DNL Profile]データストアに正常に取り込まれました。 |
| `ps_load_failure` | [!DNL Real-time Customer Profile] | 失敗 | バッチを[!DNL Profile]データストアに取り込めませんでした。 |
| `ig_load_success` | [!DNL Identity Service] | 成功 | データが正常にIDグラフに読み込まれました。 |
| `ig_load_failure` | [!DNL Identity Service] | 失敗 | データをIDグラフに読み込めませんでした。 |

>[!NOTE]
>
> すべてのデータ取得通知に対して 1 つのイベントトピックのみが提供されます。異なるステータスを区別するために、イベントコードを使用できます。
