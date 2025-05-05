---
title: RainFocus ソースの概要
description: RainFocus アカウントからExperience Platformにイベント管理と分析データを取り込む方法を学ぶ
last-substantial-update: 2023-06-21T00:00:00Z
badge: ベータ版
exl-id: 88e333e3-2b93-4d66-8412-efadea58ac46
source-git-commit: b4334b4f73428f94f5a7e5088f98e2459afcaf3c
workflow-type: tm+mt
source-wordcount: '969'
ht-degree: 6%

---

# [!DNL RainFocus]

>[!NOTE]
>
>[!DNL RainFocus] ソースはベータ版です。ベータラベル付きソースの使用について詳しくは、[ ソースの概要 ](../../home.md#terms-and-conditions) を参照してください。

[!DNL RainFocus] は、イベントを宣伝し、オーディエンスを構築するために使用できるプラットフォームです。 [!DNL RainFocus] を使用すると、美しいプロモーションページを作成したり、キャンペーンのパフォーマンスを追跡したり、登録コンバージョンを最適化したりできます。

Adobe Experience PlatformとReal-time Customer Data Platformの [!DNL RainFocus] ソースを使用すると、参加者エクスペリエンスイベントで顧客データプロファイルをリアルタイムで自動的にエンリッチメントできます。 有効にすると、エクスペリエンスイベントは自動的にReal-Time CDPにストリーミングされ、強力なオーディエンスのセグメント化、データ分析およびダウンストリームの宛先やCustomer Journey AnalyticsやAdobe Journey Optimizerなどのアプリケーションを使用した参加者ジャーニーのアクティブ化が可能になります。

>[!IMPORTANT]
>
>このソースコネクタとドキュメントページは、[!DNL RainFocus] チームが作成および管理します。 お問い合わせや更新のリクエストについては、clientcare<span>@rainfocus.comまで直接ご連絡いただくか、[[!DNL RainFocus]  ヘルプセンター ](https://help.rainfocus.com/hc/en-us) をご覧ください。

## 前提条件

Experience Platformで [!DNL RainFocus] 統合をアクティベートするには、次の前提条件を満たす必要があります。

[Adobe Developer ポータルでのAdobe サービスアカウント（JWT）の作成 ](https://developer.adobe.com/developer-console/docs/guides/authentication/ServiceAccountIntegration/)

>[!IMPORTANT]
>
>Adobeは最近、OAuth を優先して JWT トークンを廃止すると発表しました。 この変更に対応するために、[!DNL RainFocus] ソースは近い将来 OAuth に移行される予定です。

### 必要な資格情報の収集

[!DNL RainFocus] をExperience Platformに接続するには、[!DNL RainFocus] で次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| クライアント ID | クライアント ID は、Adobe Developer ポータルのAdobe サービスアカウントから取得できます。 | `b9c32a63e7d41a0f87d3e8b52a16e7a2` |
| クライアント秘密鍵 | クライアントの秘密鍵は、Adobe Developer ポータルのAdobe サービスアカウントから取得できます。 | `k1b-p-umplcjtg_arnw-R-Bx44bybu` |
| テクニカルアカウント ID | テクニカルアカウント ID は、Adobe Developer ポータルのAdobe サービスアカウントから取得できます。 | `B3F9D2E8A64C573D21ABFE97@techacct.adobe.com` |
| 組織 ID | 組織 ID は、Adobe Developer ポータルのAdobe サービスアカウントから取得できます | `D9A6F3BCE82FD147C50E3A19@techacct.adobe.com` |

### XDM スキーマを作成し、ID フィールドを定義します {#create-an-xdm-schema-and-define-the-identity-field}

[!DNL RainFocus] からのエクスペリエンスイベントをExperience Platformに保存するには、エクスペリエンスデータモデル（XDM）スキーマを作成して、[!DNL RainFocus] から送信される可能なフィールドとデータタイプを保存できるデータセットを記述する必要があります。

[!DNL RainFocus] では、デフォルトで送信されるすべてのデータをカバーする次のフィールドを使用することをお勧めします。

次のフィールドグループも推奨されます（プレフィックスで示されます）。

* 出席者
* 出展者
* リード
* Session
* SessionTime

**スキーマには、次のフィールドが含まれている必要があります。**

| フィールド | タイプ | 例 | 説明 |
| --- | --- | --- | --- |
| `attendee.registered` | 文字列 | ○ | 出席者が登録済みと見なされるかどうかを決定するフラグ。 |
| `attendee.attendeeId` | 文字列 | 1619119968857001fvLB | [!DNL RainFocus] の出席者 ID。 |
| `attendee.externalId` | 文字列 | 1666809456617001wyPj | 組織によって指定された外部 ID。 |
| `attendee.clientId` | 文字列 | 8EFC1F57631CAFE70A495ECB@8f3f1f5c631caf3e495fd8.e | 出席者 SSO クライアント ID。 |
| `attendee.email` | 文字列 | user<span>@company.com | 出席者の E メール アドレス。 |
| `transmissionId` | 文字列 | 1680309557133001YHhz | データプッシュに使用される一意の ID。 |
| `eventType` | 文字列 | SessionScheduled | 参加者エクスペリエンスイベントの名前。 |
| `timestamp` | 日時 | 2023-04-01T00:41:57.000Z | データプッシュのタイムスタンプ。 |
| `event.name` | 文字列 | Adobe Summit 2023 | 送信が行われたイベントの名前。 |
| `exhibitor.exhibitorId` | 文字列 | 1680309557133001YHhz | 出展者の [!DNL RainFocus] 識別子。 |
| `exhibitor.externalId` | 文字列 | 1666809514105001SJN | クライアントシステムにおける出展者の識別子。 |
| `exhibitor.name` | 文字列 | IBM | 出展者の名前。 |
| `lead.leadId` | 文字列 | 1666809456617001wyPj | リードの [!DNL RainFocus] 識別子。 |
| `lead.note` | 文字列 | | |
| `session.sessionId` | 文字列 | 1666809373585001t4aV | セッションの [!DNL RainFocus] 識別子。 |
| `session.externalId` | 文字列 | 1666809456617001wyPj | クライアントシステムのセッションの識別子。 |
| `session.code` | 文字列 | GS3 | セッションのコード。 |
| `session.title` | 文字列 | インスピレーション基調講演 | セッションのタイトル。 |
| `session.length` | 整数 | 90 | セッションの長さ。 |
| `sessiontime.sessiontimeId` | 文字列 | 1673033149739001OJLZ | セッション時間の [!DNL RainFocus] 識別子。 |
| `sessiontime.startTime` | 文字列 | 2023-03-22 10:00:00 | セッションの開始時間。 |
| `sessiontime.endTime` | 文字列 | 2023-03-22 10:00:00 | セッションの終了時刻。 |
| `sessiontime.room` | 文字列 | B32 | セッションに使用するルーム。 |

{style="table-layout:auto"}

[!DNL RainFocus] データのスキーマを作成するには、次のドキュメントを参照して、API または UI を使用してスキーマを作成する手順を確認してください。

* [UI を使用したスキーマの作成](../../../xdm/tutorials/create-schema-ui.md)
* [API を使用したスキーマの作成](../../../xdm/tutorials/create-schema-api.md)

>[!IMPORTANT]
>
>* スキーマは、**XDM ExperienceEvent クラス** を拡張する必要があります。
>* スキーマに **プライマリ ID** が含まれ、**プロファイルに対して有効** になっていることを確認する必要があります。 詳しくは、[UI での ID フィールドの定義 ](https://experienceleague.adobe.com/docs/experience-platform/xdm/ui/fields/identity.html?lang=ja) に関するガイドを参照してください
>* ID （メール）の例は、SHA256 メールや ECID など、別の適切な識別子に置き換えることができます。

### RainFocus での統合プロファイルの作成 {#create-an-integration-profile-in-rainfocus}

サービスアカウントと XDM スキーマの準備が整ったら、[!DNL RainFocus] プラットフォームを通じて [!DNL Integration Profile] をアクティブ化できます。 [!DNL Integration Profile] は、Experience Platformへのデータのストリーミングを担当します。

[[!DNL RainFocus] platform](https://app.rainfocus.com) にログインします。 プライマリナビゲーションで「**[!DNL Libraries]**」を選択し、「**[!DNL Integration Profiles]**」を選択します。

![ ライブラリと統合プロファイルが選択された RainFocus UI。](/help/sources/images/tutorials/create/rainfocus/rainfocus_integration-profile.png)

新しいプロファイルを作成するには、**（`+`）** アイコンを選択します。 次に、「**Adobe Real-time Customer Data Platform**」を選択し、「**OK**」を選択します。

![RainFocus UI の統合プロファイルを作成ウィンドウ ](/help/sources/images/tutorials/create/rainfocus/rainfocus_integration-profile-select.png)

次に、Adobe Developer ポータルプロジェクトで取得した資格情報を指定します。

* **クライアント ID**
* **クライアント秘密鍵**
* **テクニカルアカウント ID**
* **組織 ID**



資格情報を指定したら、「**[!DNL Save]**」を選択します。新しい [!DNL Integration Profile] が [!DNL RainFocus] ダッシュボードに表示されます。

作成した [!DNL Integration Profile] を選択すると、既に設定済みの事前定義済み **プッシュタイプ** のリストが表示されます。 これらは、発生時にExperience Platformに送信される [ エクスペリエンスイベント ](https://experienceleague.adobe.com/docs/experience-platform/xdm/classes/experienceevent.html?lang=ja) です。

![RainFocus ダッシュボードの事前定義済みのプッシュタイプのリスト。](/help/sources/images/tutorials/create/rainfocus/rainfocus_integration-profile-setup.png)

サンプル JSON ペイロードのコピーを取得するには、「**[!DNL Sample JSON Payload]**」を選択します。 次に、サンプルの JSON ペイロードをハイライト表示してコピーし、**拡張子.json を持つ新しいファイルに保存** ます。 これは、Experience Platformの後半で [ マッピング設定 ](../../tutorials/ui/create/analytics/rainfocus.md#mapping) に使用されます。

![RainFocus ダッシュボードのサンプル JSON ペイロード ](/help/sources/images/tutorials/create/rainfocus/rainfocus_integration-profile-json.png)

>[!TIP]
>
>**設定がまだ完了していません**：データフローを作成したら、[!DNL RainFocus] ダッシュボードに戻って **ストリーミングエンドポイント URL** と **データフロー ID** を指定して [!DNL Integration Profile] を完了する必要があります。

## 次の手順

このドキュメントでは、[!DNL RainFocus] アカウントからExperience Platformにデータをストリーミングするために必要な前提条件の設定を完了しました。 これで、ユーザーインターフェイスを使用した [Experience Platformへの接続  [!DNL RainFocus]  に関するガイドに進むことができ ](../../tutorials/ui/create/analytics/rainfocus.md) す。
