---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ストリーミング取り込みのトラブルシューティング
topic: troubleshooting
translation-type: tm+mt
source-git-commit: bfbf2074a9dcadd809de043d62f7d2ddaa7c7b31
workflow-type: tm+mt
source-wordcount: '993'
ht-degree: 0%

---


# ストリーミング取り込みのトラブルシューティングガイド

このドキュメントでは、Adobe Experience Platformでのストリーミング取り込みに関するよくある質問と、その回答を示します。 すべてのAPIで遭遇する問題など、他の [!DNL Platform] サービスに関する質問やトラブルシューティングについては、 [!DNL Platform] Experience Platformのトラブルシューティングガイドを参照してください [](../../landing/troubleshooting.md)。

Adobe Experience Platform [!DNL Data Ingestion] は、データを取り込むために使用できるRESTful APIを提供し [!DNL Experience Platform]ます。 取り込んだデータを使用して個々の顧客プロファイルをほぼリアルタイムで更新し、パーソナライズされた関連性のあるエクスペリエンスを複数のチャネルにわたって配信できます。 サービスと様々な [取り込み方法の詳細については、](../home.md) データ取り込みの概要を参照してください。 ストリーミング取り込みAPIの使用方法に関する手順については、 [ストリーミング取り込みの概要を参照してください](../streaming-ingestion/overview.md)。

## FAQ

次に、ストリーミング取り込みに関するよくある質問に対する回答のリストを示します。

### 送信するペイロードが正しくフォーマットされていることを知るにはどうしたらよいですか。

[!DNL Data Ingestion] ( [!DNL Experience Data Model] XDM)スキーマを活用して、入力データの形式を検証します。 事前定義されたXDMスキーマの構造に適合しないデータを送信すると、取り込みに失敗します。 XDMとでのその使用について詳し [!DNL Experience Platform]くは、「 [XDM System overview](../../xdm/home.md)」を参照してください。

ストリーミング取り込みでは、次の2つの検証モードがサポートされます。 同期および非同期。 失敗したデータの処理は、検証方法ごとに異なります。

**同期検証は** 、開発プロセスで使用する必要があります。 検証に失敗したレコードは削除され、失敗した理由に関するエラーメッセージが返されます(例： &quot;XDMメッセージ形式が無効です&quot;)。

**実稼働環境では非同期検証** (Asynchronous)を使用する必要があります。 検証に合格しない形式のデータは、失敗したバッチファイル [!DNL Data Lake] としてに送信され、後でそのデータを取得して、以降の分析を確保できます。

同期検証と非同期検証について詳しくは、「 [ストリーミング検証の概要](../quality/streaming-validation.md)」を参照してください。 検証に失敗したバッチを表示する手順については、失敗したバッチの [取得に関するガイドを参照してください](../quality/retrieve-failed-batches.md)。

### リクエストのペイロードをに送信する前に検証できま [!DNL Platform]すか。

リクエストペイロードは、に送信された後でのみ評価でき [!DNL Platform]ます。 同期検証を実行すると、有効なペイロードは入力されたJSONオブジェクトを返し、無効なペイロードはエラーメッセージを返します。 非同期検証中に、サービスは不正なデータを検出し、分析のために後で取得でき [!DNL Data Lake] る場所に送信します。 See the [streaming validation overview](../quality/streaming-validation.md) for more information.

### 同期検証がサポートされていないエッジで同期検証が要求された場合はどうなりますか？

要求された場所で同期検証がサポートされていない場合、501エラー応答が返されます。 同期検証の詳細については、 [ストリーミング検証の概要](../quality/streaming-validation.md) を参照してください。

### データが信頼できるソースからのみ収集されるようにする方法

[!DNL Experience Platform] 保護されたデータ収集をサポートします。 認証済みデータ収集が有効な場合、クライアントはJSON Web Token(JWT)とIMS組織IDをリクエストヘッダーとして送信する必要があります。 認証済みデータの送信先について詳しくは [!DNL Platform]、 [認証済みデータ収集に関するガイドを参照してください](../tutorials/create-authenticated-streaming-connection.md)。

### データのストリーミングの遅延は何で [!DNL Real-time Customer Profile]すか。

ストリーム化されたイベントは、通常、60秒未満 [!DNL Real-time Customer Profile] で反映されます。 実際の待ち時間は、データ量、メッセージサイズ、帯域幅の制限によって異なる場合があります。

### 同じAPIリクエストに複数のメッセージを含めることはできますか。

1つのリクエストペイロード内で複数のメッセージをグループ化し、それらをにストリーミングでき [!DNL Platform]ます。 正しく使用すれば、1つのリクエスト内で複数のメッセージをグループ化することは、データ操作を最適化する優れた方法です。 詳細については、リクエスト内で複数のメッセージを [送信する方法のチュートリアルをお読みください](../tutorials/streaming-multiple-messages.md) 。

### 送信するデータが受信されているかどうかを知る方法

に送信されるすべてのデータ [!DNL Platform] （正常に送信されるか、その他の方法で送信される）は、データセットに保持される前にバッチファイルとして保存されます。 バッチの処理ステータスは、送信先のデータセット内に表示されます。

データセットのアクティビティを確認するには、 [Experience Platformユーザーインターフェイスを使用して、データが正常に取り込まれたかどうかを確認します](https://platform.adobe.com)。 左側のナビゲーションで **[!UICONTROL 「Datasets]** 」をクリックして、データセットのリストを表示します。 表示されたリストからストリーミング先のデータセットを選択し、 *[!UICONTROL データセットアクティビティ]* ページを開き、選択した期間に送信されたすべてのバッチを表示します。 を使用してデータストリームを監視する方法 [!DNL Experience Platform] について詳しくは、ストリーミングデータフローの [監視に関するガイドを参照してください](../quality/monitor-data-flows.md)。

データの取り込みに失敗し、そのデータを復元する場合 [!DNL Platform]は、にIDを送信して、失敗したバッチを取得でき [!DNL Data Access API]ます。 詳しくは、失敗したバッチの [取得に関するガイド](../quality/retrieve-failed-batches.md) を参照してください。

### データレイクでストリーミングデータが利用できないのはなぜですか。

バッチ取り込みが失敗する理由は、無効なフォーマット、データの欠落、システムエラーな [!DNL Data Lake]ど、様々です。 バッチが失敗した理由を判断するには、および表示を使用してバッチを取得する必要があり [!DNL Data Ingestion Service API] ます。 失敗したバッチを取得する手順について詳しくは、失敗したバッチの [取得に関するガイドを参照してください](../quality/retrieve-failed-batches.md)。

### APIリクエストに対して返された応答を解析する方法を教えてください。

最初にサーバーの応答コードをチェックして、要求が受け入れられたかどうかを確認することで、応答を解析できます。 正常な応答コードが返された場合は、 `responses` 配列オブジェクトを確認して取り込みタスクのステータスを判断できます。

成功した単一メッセージのAPIリクエストは、ステータスコード200を返します。 バッチ処理されたメッセージAPIリクエストが成功した（または一部成功した）場合は、ステータスコード207を返します。

次のJSONは、2つのメッセージを持つAPIリクエストの応答オブジェクトの例です。 1つは成功し、1つは失敗しました。 プロパティを正常にストリーミング再生するメッセージ `xactionId` です。 プロパティと応答を返すストリーミングに失敗したメッセージで、 `statusCode` 詳細な情報 `message` が返されます。

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

メッセージを [!DNL Real-time Customer Profile] 拒否した場合は、ID情報が正しくないことが原因の可能性が高くなります。 これは、IDに対して無効な値または名前空間を指定した結果である可能性があります。

ID名前空間には次の2種類があります。 defaultとcustom. カスタム名前空間を使用する場合は、名前空間がに登録されていることを確認してくだ [!DNL Identity Service]さい。 デフォルトおよびカスタム [名前空間の使用について詳しくは、「](../../identity-service/namespaces.md) ID名前空間の概要」を参照してください。

メッセージの取り込みに失敗した理由の詳細につ [!DNL Experience Platform UI](https://platform.adobe.com) いては、を参照してください。 左側のナビゲーションで **[!UICONTROL 「]** Monitoring _[!UICONTROL 」をクリックし、「]_Streaming end-to-end」タブを表示して、選択した期間にストリーミングされたメッセージバッチを表示します。