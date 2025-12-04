---
title: Experience Platformでのイベント複製の処理
description: Adobe Experience Platformでのイベント複製の処理方法を説明します
exl-id: ac8c3ee8-52cf-459c-b283-16ed32d2976d
source-git-commit: bb90bbddf33bc4b0557026a0f34965ac37475c65
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 0%

---

# Adobe Experience Platformでのイベント複製の処理

Adobe Experience Platformは、信頼性を最大限に高めると同時に、増え続けるデータ量に合わせて拡張できるように設計された、高度に分散されたシステムです。

リアルタイムのデータ収集では、[&#x200B; エクスペリエンスイベント &#x200B;](/help/xdm/classes/experienceevent.md) は、Web SDKや [&#x200B; モバイル SDK](https://developer.adobe.com/client-sdks/home/) などのクライアントサイドソースからEdge Networkを介して収集され、Experience Platformの処理レイヤーとストレージレイヤーに配信されます。 これらのレイヤーは、Experience Platform、[Real-Time CDP](/help/rtcdp/home.md)、[Customer Journey Analytics](https://experienceleague.adobe.com/docs/analytics-platform/using/cja-overview/cja-overview.html?lang=ja)、[Adobe Journey Optimizer](https://experienceleague.adobe.com/docs/journey-optimizer/using/ajo-home.html?lang=ja) などのソリューションを構成します。

エクスペリエンスイベントの損失を最小限に抑えるために、クライアントサイド SDK と内部Experience Platform配信サービスは、イベントが正常に収集されたことを示すメッセージを受け取ります。

その確認が受信されない場合、クライアントサイド SDK または内部Experience Platform配信サービスは再試行し、トリガーが再度送信されます。

これは、一時的なエラーを処理する際のベストプラクティスです。 副作用は、重複したイベントを導入する可能性です。

一時的なエラーを処理するためのベストプラクティスをより深く理解するには、この記事 [&#x200B; 一時的なエラー処理 &#x200B;](https://learn.microsoft.com/en-us/azure/architecture/best-practices/transient-faults) を参照してください。

## イベント複製のシナリオ {#scenarios}

イベントの重複は、次のような様々なシナリオで発生する可能性があります（ただし、これに限定されません）。

* クライアントサイド SDK と [!DNL Edge Network] 間のネットワーク関連の問題。 Edge Networkとお客様の間の接続はパブリックインターネットを介して行われるため、これらの問題は、インターネットサービスプロバイダーの障害、モバイル信号の損失、またはその他のネットワークの障害に起因する可能性があります。
* 内部Experience Platform自動スケーリングイベント。 場合によっては、クラウドインフラのボラティリティが原因で、データのリバランスが発生することがあります。

Adobe Experience Platform データ収集レイヤーは、「少なくとも 1 回」の処理をサポートするように設計されています。 その結果、まれに限られた状況でイベントの重複が発生することがあります。

「少なくとも 1 回」の処理について詳しくは、[&#x200B; メッセージ配信の保証 &#x200B;](https://docs.confluent.io/kafka/design/delivery-semantics.html) に関する記事を参照してください。

## イベントの重複排除オプション {#deduplication}

重複イベントの影響を受けやすいビジネスシナリオの場合、Experience Platformでは、次に示すように、ダウンストリームのストレージシステムで複数のイベント重複排除方法を使用します。

* 同じ `_id` を持つイベントが [!DNL Profile store] に既に存在する場合、Real-Time CDP プロファイルストアはイベントをドロップします。 詳しくは、[XDM ExperienceEvent クラス &#x200B;](/help/xdm/classes/experienceevent.md) に関するドキュメントを参照してください。
* Customer Journey Analyticsでは、値を繰り返さずにカウントするように指標を設定できます。 この方法について詳しくは、[&#x200B; 指標の重複排除コンポーネントの設定 &#x200B;](https://experienceleague.adobe.com/docs/analytics-platform/using/cja-dataviews/component-settings/metric-deduplication.html?lang=ja) に関するドキュメントを参照してください。
* Experience Platform クエリサービスでは、行内のデータの一部のみが重複情報なので、行全体を計算から除外したり特定のフィールドセットを無視したりする必要がある場合に、データ重複排除をサポートします。 詳しくは、[&#x200B; クエリサービスでのデータ重複排除 &#x200B;](/help/query-service/key-concepts/deduplication.md) に関するドキュメントを参照してください。

>[!NOTE]
>
>上記のユースケース以外でイベント複製の問題が発生した場合は、Adobe担当者に連絡し、ユースケースに関する詳細を提供してください。
