---
title: Experience Platformでのイベント複製の処理
description: Adobe Experience Platformでのイベント複製の処理方法を説明します
exl-id: ac8c3ee8-52cf-459c-b283-16ed32d2976d
source-git-commit: e52eb90b64ae9142e714a46017cfd14156c78f8b
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---

# Adobe Experience Platformでのイベント複製の処理

Adobe Experience Platformは、信頼性を最大限に高めると同時に、増え続けるデータ量に合わせて拡張できるように設計された、高度に分散されたシステムです。

リアルタイムのデータ収集の場合、 [エクスペリエンスイベント](../xdm/classes/experienceevent.md) 次を介して収集されます [Edge Network](../web-sdk/home.md#edge-network)などのクライアントサイドソースから [Web SDK](../web-sdk/home.md) または [Mobile SDK](https://developer.adobe.com/client-sdks/home/)Experience Platform処理およびストレージレイヤーに配信されます。 これらのレイヤーは、Experience Platformなどのソリューションを構成します。 [Real-Time CDP](../rtcdp/home.md), [Customer Journey Analytics](https://experienceleague.adobe.com/docs/analytics-platform/using/cja-overview/cja-overview.html?lang=ja)、および [Adobe Journey Optimizer](https://experienceleague.adobe.com/docs/journey-optimizer/using/ajo-home.html?lang=ja).

エクスペリエンスイベントの損失を最小限に抑えるために、クライアントサイド SDK と内部Experience Platform配信サービスでは、イベントが正常に収集されたことを示すメッセージが表示されます。

その確認が受信されない場合、クライアントサイド SDK または内部 Platform 配信サービスは再試行をトリガーし、エクスペリエンスイベントが再度送信されます。

これは、一時的なエラーを処理する際のベストプラクティスです。 副作用は、重複したイベントを導入する可能性です。

一時的なエラーを処理するためのベストプラクティスをより深く理解するには、次の記事を参照してください [一時的な障害処理](https://learn.microsoft.com/en-us/azure/architecture/best-practices/transient-faults).

## イベント複製のシナリオ {#scenarios}

イベントの重複は、次のような様々なシナリオで発生する可能性があります（ただし、これに限定されません）。

* クライアントサイド SDK ととの間のネットワーク関連の問題 [!DNL Edge Network]. お客様とEdge Network間の接続はパブリックインターネットを介して行われるため、これらの問題は、インターネットサービスプロバイダーの障害、モバイル信号の損失、またはその他のネットワークの障害に起因する可能性があります。
* 内部Experience Platformの自動スケーリングイベント。 場合によっては、クラウドインフラのボラティリティが原因で、データのリバランスが発生することがあります。

Adobe Experience Platform データ収集レイヤーは、「少なくとも 1 回」の処理をサポートするように設計されています。 その結果、まれに限られた状況でイベントの重複が発生することがあります。

「少なくとも 1 回」の処理について詳しくは、の記事を参照してください。 [メッセージ配信の保証](https://docs.confluent.io/kafka/design/delivery-semantics.html).

## イベントの重複排除オプション {#deduplication}

重複イベントの影響を受けやすいビジネス環境の場合、Experience Platformでは、次に示すように、ダウンストリームのストレージシステムで複数のイベント重複排除方式を使用します。

* Real-Time CDP プロファイルストアは、同じを持つイベントが発生すると、イベントをドロップします `_id` はすでにに存在します [!DNL Profile store]. のドキュメントを参照してください。 [XDM ExperienceEvent クラス](../xdm/classes/experienceevent.md) を参照してください。
* Customer Journey Analyticsを使用すると、指標を設定して、値を繰り返さずにカウントできます。 この方法については、のドキュメントを参照してください。 [指標の重複排除コンポーネントの設定](https://experienceleague.adobe.com/docs/analytics-platform/using/cja-dataviews/component-settings/metric-deduplication.html?lang=ja).
* Experience Platformクエリ サービスでは、行内のデータの一部のみが重複データなので、行全体を計算から除外したり特定のフィールド セットを無視したりする必要がある場合に、データ重複排除をサポートします。 に関するドキュメントを参照してください [クエリサービスでのデータ重複排除](../query-service/key-concepts/deduplication.md) を参照してください。

>[!NOTE]
>
>上記のユースケース以外でイベント複製の問題が発生した場合は、Adobe担当者に連絡し、ユースケースに関する詳細を提供してください。
