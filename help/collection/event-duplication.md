---
title: Experience Platformでのイベント重複処理
description: Adobe Experience Platformでのイベント重複の処理方法を説明します
source-git-commit: 89cdb0832009bcee31b4339f021bc5a0ce254752
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---


# Adobe Experience Platformでのイベントの複製処理

Adobe Experience Platformは、増え続けるデータ量に対応しながら、信頼性を最大限に高めるように設計された、高度に分散されたシステムです。

リアルタイムデータ収集の場合、 [エクスペリエンスイベント](../xdm/classes/experienceevent.md) を使用して収集 [Edge Network](../edge/home.md#edge-network)、クライアント側のソース ( [Web SDK](../edge/home.md) または [モバイル SDK](https://developer.adobe.com/client-sdks/home/)、Experience Platform処理およびストレージレイヤーに配信されます。 これらのレイヤーは、Experience Platform、 [Real-Time CDP](../rtcdp/home.md), [Customer Journey Analytics](https://experienceleague.adobe.com/docs/analytics-platform/using/cja-overview/cja-overview.html?lang=ja)、および [Adobe Journey Optimizer](https://experienceleague.adobe.com/docs/journey-optimizer/using/ajo-home.html?lang=ja).

エクスペリエンスイベントの損失を最小限に抑えるために、クライアント側 SDK および内部Experience Platform配信サービスは、イベントが正常に収集されたことを確認する必要があります。

その確認を受け取らない場合、クライアント側 SDK または内部 Platform 配信サービストリガーは再試行され、 Experience イベントが再送信されます。

これは、一時的なエラーの処理に関するベストプラクティスです。 副次的な影響は、重複したイベントが発生する可能性です。

一時的なエラーの処理に関するベストプラクティスについて詳しくは、 [一時的な故障処理](https://learn.microsoft.com/en-us/azure/architecture/best-practices/transient-faults).

## イベントの重複シナリオ {#scenarios}

イベントの重複は、次のような様々なシナリオで発生する可能性がありますが、これらに限定されません。

* クライアント側 SDK と [!DNL Edge Network]. お客様と Edge ネットワーク間の接続はパブリックインターネットを通じておこなわれるので、これらの問題は、インターネットサービスプロバイダーの障害、モバイル信号の消失、その他のネットワーク障害が原因で発生する可能性があります。
* 内部Experience Platformの自動スケーリングイベント。 クラウドインフラストラクチャの変動が原因で、データのバランスが取れる場合があります。

Adobe Experience Platformデータ収集レイヤーは、「少なくとも 1 回」の処理をサポートするように設計されています。 その結果、まれな状況で、イベントの重複が発生する可能性があります。

「少なくとも 1 回」の処理について詳しくは、 [メッセージ配信保証](https://docs.confluent.io/kafka/design/delivery-semantics.html).

## イベントの重複排除オプション {#deduplication}

重複イベントを区別するビジネスシナリオでは、Experience Platformは、下記に説明する方法など、ダウンストリームストレージシステムで複数のイベント重複排除メソッドを使用します。

* Real-Time CDP Profile Store は、同じ `_id` は既に [!DNL Profile Store]. 次のドキュメントを参照してください： [XDM ExperienceEvent クラス](../xdm/classes/experienceevent.md) を参照してください。
* Customer Journey Analyticsを使用すると、指標を設定して、値を繰り返しカウントしないようにできます。 この方法については、 [指標の重複排除コンポーネントの設定](https://experienceleague.adobe.com/docs/analytics-platform/using/cja-dataviews/component-settings/metric-deduplication.html?lang=ja).
* Experience Platformクエリサービスは、行の一部のみが重複した情報なので、行全体を計算から削除するか、特定のフィールドのセットを無視する必要がある場合に、データの重複排除をサポートします。 関するドキュメントを参照してください。 [クエリサービスでのデータ重複排除](../query-service/key-concepts/deduplication.md) を参照してください。

>[!NOTE]
>
>上記の使用例以外でAdobeの重複の問題が発生した場合は、イベント担当者に連絡し、使用例の詳細を伝えてください。