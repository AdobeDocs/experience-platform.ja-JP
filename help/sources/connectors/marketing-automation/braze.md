---
title: ブレーズ電流ソースの概要
description: Braze Currents からExperience Platformにデータをストリーミングする方法を説明します。
badge: ベータ版
source-git-commit: 64975ccb6a44730489427cef745f3dbce5bcedf1
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 18%

---

# [!DNL Braze Currents]

>[!NOTE]
>
>[!DNL Braze Currents] ソースはベータ版です。詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platformは、ストリーミングアプリケーションからデータを取り込む機能を備えています。 ストリーミングプロバイダーのサポートには以下が含まれます。 [!DNL Braze Currents].

[!DNL Braze] を使用すると、消費者とブランド間の顧客中心のインタラクションをリアルタイムで実現できます。 [!DNL Braze Currents] は、Braze プラットフォームのエンゲージメントイベントのリアルタイムデータストリームで、 [!DNL Braze] プラットフォーム。

## 前提条件

このガイドの手順を完了するには、次が必要です。

* へのログイン [Adobe Experience Platform](https://platform.adobe.com) 新しいストリーミングソース接続を作成する権限
* 次にログイン： [[!DNL Braze] dashboard](https://dashboard.braze.com/sign_in)、未使用 [Currents コネクタライセンス](https://www.braze.com/docs/user_guide/data_and_analytics/braze_currents)、およびコネクタを作成する権限。 詳しくは、 [設定要件 [!DNL Currents]](https://www.braze.com/docs/user_guide/data_and_analytics/braze_currents/setting_up_currents/#requirements).

### 必要な資格情報の収集

を正常に送信するには、以下を実行します。 [!DNL Braze Currents] データをExperience Platformに送信する場合は、次のExperience Platform資格情報を [!DNL Braze] UI ダッシュボード。

| フィールド | 説明 |
| --- | --- |
| クライアント ID | Experience Platformソースに関連付けられたクライアント ID。 |
| クライアント秘密鍵 | Experience Platform・ソースに関連付けられたクライアントの秘密鍵。 |
| テナント ID | Experience Platformソースに関連付けられたテナント ID。 |
| サンドボックス名 | Experience Platformソースに関連付けられたサンドボックス。 |
| データフロー ID | データソースに関連付けられたExperience Platformフロー ID。 |
| ストリーミングエンドポイント | Experience Platformソースに関連付けられたストリーミングエンドポイント。 **注意**: [!DNL Braze] は、これをバッチストリーミングエンドポイントに自動的に変換します。 |

これらの値の取得方法について詳しくは、 [Platform API の概要](../../../landing/api-authentication.md). の使用に関する情報 [!DNL Braze] ダッシュボード、読む [!DNL Braze] ～する方法に関するガイド [電流に移動](https://www.braze.com/docs/user_guide/data_and_analytics/braze_currents/setting_up_currents/#step-2-navigate-to-currents).

## 次の手順

このドキュメントを読むと、データを [!DNL Braze Currents] アカウントからExperience Platformへ。 次のガイドに進むことができます： [接続 [!DNL Braze Currents] ユーザーインターフェイスを使用してExperience Platformを設定するには](../../tutorials/ui/create/marketing-automation/braze.md).