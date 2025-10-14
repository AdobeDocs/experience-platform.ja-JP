---
title: Braze Currens Sourceの概要
description: Braze Current からExperience Platformにデータをストリーミングする方法を説明します。
badge: ベータ版
exl-id: dd304e10-26e5-4586-ab39-8fe3294b19c9
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '359'
ht-degree: 12%

---

# [!DNL Braze Currents]

>[!NOTE]
>
>[!DNL Braze Currents] ソースはベータ版です。ベータラベル付きソースの使用について詳しくは、[&#x200B; ソースの概要 &#x200B;](../../home.md#terms-and-conditions) を参照してください。

Adobe Experience Platformを使用すると、データを外部ソースから取得しながら、Experience Platform サービスを使用して、受信データの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platformは、ストリーミングアプリケーションからのデータ取り込みをサポートしています。 ストリーミングプロバイダーのサポートには、[!DNL Braze Currents] が含まれます。

[!DNL Braze] は、消費者とブランドの間で、顧客中心のやり取りをリアルタイムで強化します。 [!DNL Braze Currents] は、Braze プラットフォームからのエンゲージメントイベントのリアルタイムデータストリームです。これは、[!DNL Braze] プラットフォームからの最も堅牢かつ詳細な書き出しです。

## 前提条件

このガイドの手順を完了するには、以下が必要です。

* [Adobe Experience Platformにログインし &#x200B;](https://platform.adobe.com) 新しいストリーミングソース接続を作成する権限。
* [[!DNL Braze]  ダッシュボード &#x200B;](https://dashboard.braze.com/sign_in) へのログイン、未使用の [Current Connector ライセンス &#x200B;](https://www.braze.com/docs/user_guide/data_and_analytics/braze_currents)、コネクタを作成するための権限。 詳しくは、[&#x200B; 設定の要件  [!DNL Currents]](https://www.braze.com/docs/user_guide/data_and_analytics/braze_currents/setting_up_currents/#requirements) を参照してください。

### 必要な資格情報の収集

[!DNL Braze Currents] データをExperience Platformに正常に送信するには、[!DNL Braze] UI ダッシュボードに次のExperience Platform資格情報を入力する必要があります。

| フィールド | 説明 |
| --- | --- |
| クライアント ID | Experience Platform ソースに関連付けられたクライアント ID。 |
| クライアント秘密鍵 | Experience Platform ソースに関連付けられたクライアントシークレット。 |
| テナント ID | Experience Platform ソースに関連付けられたテナント ID。 |
| サンドボックス名 | Experience Platform ソースに関連付けられたサンドボックス。 |
| データフロー ID | Experience Platform ソースに関連付けられたデータフロー ID。 |
| ストリーミングエンドポイント | Experience Platform ソースに関連付けられたストリーミングエンドポイント。 **メモ**:[!DNL Braze] はこれをバッチストリーミングエンドポイントに自動的に変換します。 |

これらの値の取得方法について詳しくは、[Experience Platform API の概要 &#x200B;](../../../landing/api-authentication.md) を参照してください。 [!DNL Braze] ダッシュボードの使用方法については、[!DNL Braze] ガイドで [&#x200B; 電流に移動 &#x200B;](https://www.braze.com/docs/user_guide/data_and_analytics/braze_currents/setting_up_currents/#step-2-navigate-to-currents) する方法を参照してください。

## 次の手順

このドキュメントでは、[!DNL Braze Currents] アカウントからExperience Platformにデータをストリーミングするために必要な前提条件を説明しました。 これで、ユーザーインターフェイスを使用した [Experience Platformへの接続  [!DNL Braze Currents]  に関するガイドに進むことができ &#x200B;](../../tutorials/ui/create/marketing-automation/braze.md) す。
