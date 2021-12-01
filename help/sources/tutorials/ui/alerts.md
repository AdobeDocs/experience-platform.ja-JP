---
keywords: Experience Platform；ホーム；人気の高いトピック；アラート
description: データフローの作成時にアラートをサブスクライブして、フロー実行のステータス、成功または失敗に関するアラートメッセージを受け取ることができます。
title: UI でのコンテキスト内アラートへのサブスクライブ
hide: true
hidefromtoc: true
source-git-commit: a81d21f615206e644044c2f0f546a458d1aebbf3
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 24%

---

# アラートメッセージをサブスクライブして、UI でソースデータフローを監視する

Adobe Experience Platform では、Adobe Experience Platform アクティビティに関するイベントベースのアラートを登録できます。 アラートにより、ジョブが完了したか、ワークフロー内の特定のマイルストーンに達したか、何らかのエラーが発生したかを確認するために、[[!DNL Observability Insights] API](../../../observability/api/overview.md) をポーリングする必要が低減される、またはなくなります。

データフローの作成時にアラートをサブスクライブして、フロー実行のステータス、成功または失敗に関するアラートメッセージを受け取ることができます。

このドキュメントでは、アラートを購読し、フロー実行のアラートメッセージを受け取る手順を説明します。

## 概要

このドキュメントでは、Adobe Experience Platformの次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、次のコードを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。 [!DNL Platform] サービス。
* [Observability では、統計的な指標とイベント通知を使用して Platform アクティビティを監視できます。](../../../observability/home.md)[!DNL Observability Insights]
   * [アラート](../../../observability/alerts/overview.md):Platform 操作で特定の条件セットに達すると（システムがしきい値に達した場合に問題が発生する可能性があるなど）、Platform は、組織内でその条件を購読したユーザーにアラートメッセージを配信できます。

## UI でアラートを購読 {#subscribe-sources-alerts}

>[!CONTEXTUALHELP]
>id="platform_sources_alerts_subscribe"
>title="ソースアラートを購読"
>abstract="アラートを使用すると、ソースのデータフローのステータスに基づいて通知を受け取ることができます。 アラート通知を設定して、データフローが開始した、成功した、失敗した、またはデータを取り込まなかった場合に更新を取得できます。"
>text="Learn more in documentation"
