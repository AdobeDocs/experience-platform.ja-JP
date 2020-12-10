---
keywords: Experience Platform;home;popular topics;streaming;streaming ingestion;troubleshooting;streaming ingestion troubleshooting;streaming ingestion faq;faq;
solution: Experience Platform
title: ストリーミング取り込みのトラブルシューティング
topic: troubleshooting
description: このドキュメントでは、Adobe Experience Platform でのストリーミングの取り込みに関するよくある質問に対する回答を示します。
translation-type: tm+mt
source-git-commit: cfdaf72b7f4bf190877006ccd4cc6a7fd014adc2
workflow-type: tm+mt
source-wordcount: '1008'
ht-degree: 67%

---


# ストリーミング取り込みのトラブルシューティングガイド

このドキュメントでは、Adobe Experience Platform でのストリーミングの取り込みに関するよくある質問に対する回答を示します。For questions and troubleshooting related to other [!DNL Platform] services, including those that are encountered across all [!DNL Platform] APIs, please refer to the [Experience Platform troubleshooting guide](../../landing/troubleshooting.md).

Adobe Experience Platform [!DNL Data Ingestion] provides RESTful APIs that you can use to ingest data into [!DNL Experience Platform]. 取り込んだデータは、個々の顧客プロファイルをほぼリアルタイムで更新するために使用され、パーソナライズされた関連性の高いエクスペリエンスを複数のチャネルで配信できます。サービスと様々な 取り込み方法の詳細については、[データ取り込みの概要](../home.md)を参照してください。ストリーミング取り込み APIの使用方法に関する手順については、[ストリーミング取り込みの概要](../streaming-ingestion/overview.md)を参照してください。

## FAQ

次に、ストリーミング取り込みに関するよくある質問に対する回答の一覧を示します。

### 送信するペイロードが正しくフォーマットされていることを確認する方法を教えてください。

[!DNL Data Ingestion] ( [!DNL Experience Data Model] XDM)スキーマを活用して、入力データの形式を検証します。 事前に定義された XDM データの構造に準拠しないデータを送信すると、スキーマの取り込みに失敗します。For more information on XDM and its use in [!DNL Experience Platform], see the [XDM System overview](../../xdm/home.md).

ストリーミング取り込みでは、同期および非同期の 2 つの検証モードがサポートされます。それぞれの検証方法では、失敗したデータの処理方法が異なります。

開発プロセスでは、**同期検証**&#x200B;を使用する必要があります。検証に失敗したレコードは削除され、失敗理由を示すエラーメッセージが返されます（例：&quot;Invalid XDM Message Format&quot;）。

実稼動環境では、**非同期検証**&#x200B;を使用する必要があります。Any malformed data that does not pass validation is sent to the [!DNL Data Lake] as a failed batch file, where it can be retrieved later for further analysis.

同期検証と非同期検証について詳しくは、[ストリーミング検証の概要](../quality/streaming-validation.md)を参照してください。検証に失敗したバッチを表示する手順については、[失敗したバッチの取得](../quality/retrieve-failed-batches.md)に関するガイドを参照してください。

### Can I validate a request payload before sending it to [!DNL Platform]?

Request payloads can only be evaluated after they have been sent to [!DNL Platform]. 同期検証を実行すると、有効なペイロードは生成された JSON オブジェクト、無効なペイロードはエラーメッセージを返します。During asynchronous validation, the service detects and sends any malformed data to the [!DNL Data Lake] where it can later be retrieved for analysis. 詳しくは、[ストリーミング検証の概要](../quality/streaming-validation.md)を参照してください。

### サポートされていないエッジで同期検証がリクエストされた場合はどうなりますか？

リクエストされた場所で同期検証がサポートされていない場合、501 エラー応答が返されます。同期検証について詳しくは、[ストリーミング検証の概要](../quality/streaming-validation.md)を参照してください。

### データが信頼できるソースからのみ収集されるようにする方法

[!DNL Experience Platform] 保護されたデータ収集をサポートします。 認証済みのデータ収集が有効な場合、クライアントは JSON Web トークン（JWT）と IMS 組織 ID をリクエストヘッダーとして送信する必要があります。For more information on how to send authenticated data to [!DNL Platform], please see the guide on [authenticated data collection](../tutorials/create-authenticated-streaming-connection.md).

### データのストリーミングの遅延は何で [!DNL Real-time Customer Profile]すか。

Streamed events are generally reflected in [!DNL Real-time Customer Profile] in under 60 seconds. 実際の待機時間は、データ量、メッセージサイズ、帯域幅の制限によって異なる場合があります。

### 同じ API リクエストに複数のメッセージを含めることはできますか？

You can group multiple messages within a single request payload and stream them to [!DNL Platform]. 正しく使用した場合、1 つのリクエスト内で複数のメッセージをグループ化すると、データ操作をうまく最適化できます。詳細については、[複数のメッセージを 1 件のリクエストで送信する方法](../tutorials/streaming-multiple-messages.md)のチュートリアルを参照してください。

### 自分が送信するデータが受信されたかどうかを確認する方法を教えてください。

All data that is sent to [!DNL Platform] (successfully or otherwise) is stored as batch files before being persisted in datasets. バッチの処理ステータスは、送信先のデータセット内に表示されます。

[Experience Platform ユーザーインターフェイス](https://platform.adobe.com)でデータセットのアクティビティを確認することで、データが正常に取り込まれたかどうかを確認できます。左側のナビゲーションで「**[!UICONTROL データセット]**」をクリックし、データセットのリストを表示します。表示されたリストからストリーミング先のアクティビティセットを選択すると、その&#x200B;**[!UICONTROL データセットアクティビティ]**&#x200B;ページを開き、選択した期間に送信されたすべてのバッチが表示されます。For more information about using [!DNL Experience Platform] to monitor data streams, see the guide on [monitoring streaming data flows](../quality/monitor-data-ingestion.md).

If your data failed to ingest and you want to recover it from [!DNL Platform], you can retrieve the failed batches by sending their IDs to the [!DNL Data Access API]. 詳しくは、[失敗したバッチの取得](../quality/retrieve-failed-batches.md)に関するガイドを参照してください。

### ストリーミングデータがデータレイクで使用できないのはなぜですか。

There are a variety of reasons why batch ingestion may fail to reach the [!DNL Data Lake], such as invalid formatting, missing data, or system errors. To determine why your batch failed, you must retrieve the batch using the [!DNL Data Ingestion Service API] and view its details. 失敗したバッチを取得する手順について詳しくは、[失敗したバッチの取得](../quality/retrieve-failed-batches.md)に関するガイドを参照してください。

### API リクエストに対して返された応答を解析する方法を教えてください。

応答を解析するには、最初にサーバー応答コードをチェックし、要求が受け入れられたかどうかを確認します。成功応答が返された場合は、`responses` 配列オブジェクトを確認して、取り込みタスクのステータスを判断します。

成功した単一のメッセージ API リクエストは、ステータスコード 200 を返します。バッチ処理されたメッセージ API リクエストが成功（または部分的に成功）すると、ステータスコード 207 が返されます。

次の JSON は、2 つのメッセージを含む API リクエストの応答オブジェクトの例です。1 つは成功したもの、1 つは失敗したものです。正常にストリーミングされるメッセージは、`xactionId` プロパティを返します。ストリーミングに失敗したメッセージは、`statusCode` プロパティと応答 `message`、および詳細情報が返されます。

```JSON
{
    "inletId": "9b0cb233972f3b0092992284c7353f5eead496218e8441a79b25e9421ea127f5",
    "batchId": "1565638336649:1750:244",
    "receivedTimeMs": 1565638336705,
    "responses": [
        {
            "xactionId": "1565650704337:2124:92:3"
        },
        {
            "statusCode": 400,
            "message": "inletId: [9b0cb233972f3b0092992284c7353f5eead496218e8441a
                79b25e9421ea127f5] 
                imsOrgId: [{IMS_ORG}] 
                Message has unknown xdm format"
        }
    ]
}
```

### 送信メッセージが受信されないのはなぜで [!DNL Real-time Customer Profile]すか？

If [!DNL Real-time Customer Profile] rejects a message, it is most likely due to incorrect identity information. これは、ID に無効な値または名前空間を指定した結果です。

ID 名前空間には、デフォルトとカスタムの 2 タイプがあります。When using custom namespaces, make sure the namespace has been registered within [!DNL Identity Service]. デフォルトおよびカスタム名前空間の使用について詳しくは、[ID 名前空間の概要](../../identity-service/namespaces.md)を参照してください。

[[!DNL Experience Platform UI]](https://platform.adobe.com) を使用して、メッセージの取り込みに失敗した理由の詳細を確認できます。左側のナビゲーションで「**[!UICONTROL 監視]**」をクリックし、「**[!UICONTROL エンドツーエンドのストリーミング]**」タブを表示して、選択した期間にストリーミングされたメッセージバッチを表示します。
