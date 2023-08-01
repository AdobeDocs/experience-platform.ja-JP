---
title: RainFocus ソースの概要
description: RainFocus アカウントからExperience Platformにイベント管理と分析データを取り込む方法を説明します
last-substantial-update: 2023-06-21T00:00:00Z
badge: ベータ版
source-git-commit: 1ed82798125f32fe392f2a06a12280ac61f225c6
workflow-type: tm+mt
source-wordcount: '998'
ht-degree: 8%

---

# [!DNL RainFocus]

>[!NOTE]
>
>[!DNL RainFocus] ソースはベータ版です。詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

[!DNL RainFocus] は、イベントを促進し、オーディエンスを構築するために使用できるプラットフォームです。 以下を使用できます。 [!DNL RainFocus] 美しいプロモーションページを作成し、キャンペーンのパフォーマンスを追跡し、登録コンバージョンを最適化します。

以下を使用します。 [!DNL RainFocus] Adobe Experience PlatformとReal-time Customer Data Platformのソースを使用して、リアルタイムで出席者エクスペリエンスイベントを使用して顧客データプロファイルを自動的にエンリッチメントします。 有効にすると、エクスペリエンスイベントがReal-Time CDPに自動的にストリーミングされ、Customer Journey AnalyticsやAdobe Journey Optimizerなどのダウンストリームの宛先やアプリケーションを使用した、強力なオーディエンスのセグメント化、データ分析、出席者のジャーニーの有効化が可能になります。

>[!IMPORTANT]
>
>このソースコネクタとドキュメントページは、 [!DNL RainFocus] チーム。 お問い合わせや更新のご依頼は、clientcare から直接お問い合わせください。<span>@rainfocus.comまたは [[!DNL RainFocus] ヘルプセンター](https://help.rainfocus.com/hc/en-us)

## 前提条件

次の前提条件を満たしてから、 [!DNL RainFocus] Experience Platformの統合：

[Adobe Developer Portal でのAdobe サービスアカウント (JWT) の作成](https://developer.adobe.com/developer-console/docs/guides/authentication/ServiceAccountIntegration/)

>[!IMPORTANT]
>
>Adobeは最近、OAuth を優先する JWT トークンの廃止を発表しました。 この変更に対応するために、 [!DNL RainFocus] ソースは近い将来に OAuth に移行されます。

### 必要な資格情報の収集

接続するには [!DNL RainFocus] をExperience Platformするには、次の接続プロパティの値を [!DNL RainFocus]:

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| クライアント ID | クライアント ID は、Adobe Developer Portal のAdobe サービスアカウントから取得できます。 | `b9c32a63e7d41a0f87d3e8b52a16e7a2` |
| クライアント秘密鍵 | クライアントの秘密鍵は、Adobe DeveloperポータルのAdobe サービスアカウントから取得できます。 | `k1b-p-umplcjtg_arnw-R-Bx44bybu` |
| テクニカルアカウント ID | テクニカルアカウント ID は、Adobe Developer Portal のAdobe サービスアカウントから取得できます。 | `B3F9D2E8A64C573D21ABFE97@techacct.adobe.com` |
| 組織 ID | 組織 ID は、Adobe Developer Portal のAdobe サービスアカウントから取得できます | `D9A6F3BCE82FD147C50E3A19@techacct.adobe.com` |

### XDM スキーマの作成と ID フィールドの定義 {#create-an-xdm-schema-and-define-the-identity-field}

エクスペリエンスイベントを保存するための条件： [!DNL RainFocus] Experience Platformでは、Experience Data Model (XDM) スキーマを作成して、から送信される可能性のあるフィールドとデータタイプを保存できるデータセットを記述する必要があります。 [!DNL RainFocus].

[!DNL RainFocus] では、次のフィールドを推奨しています。このフィールドには、デフォルトで送信されるすべてのデータが含まれています。

次のフィールドグループも推奨されます（プレフィックスで示します）。

* 出席者
* 出展者
* リード
* Session
* SessionTime

**スキーマには、次のフィールドが含まれている必要があります。**

| フィールド | タイプ | 例 | 説明 |
| --- | --- | --- | --- |
| `attendee.registered` | 文字列 | ○ | 出席者が登録済みと見なされるかどうかを指定するフラグです。 |
| `attendee.attendeeId` | 文字列 | 1619119968857001fvLB | の出席者 ID [!DNL RainFocus]. |
| `attendee.externalId` | 文字列 | 1666809456617001wyPj | 組織で指定された外部 ID。 |
| `attendee.clientId` | 文字列 | 8EFC1F57631CAFE70A495ECB@8f3f1f5c631caf3e495fd8.e | 出席者の SSO クライアント ID。 |
| `attendee.email` | 文字列 | ユーザー<span>@company.com | 出席者の電子メールアドレス。 |
| `transmissionId` | 文字列 | 1680309557133001YHhz | データのプッシュに使用される一意の ID。 |
| `eventType` | 文字列 | SessionScheduled | 出席者エクスペリエンスイベントの名前。 |
| `timestamp` | 日時 | 2023-04-01T00:41:57.000Z | データプッシュのタイムスタンプ。 |
| `event.name` | 文字列 | Adobe Summit 2023 | 送信が発生したイベントの名前。 |
| `exhibitor.exhibitorId` | 文字列 | 1680309557133001YHhz | The [!DNL RainFocus] 出展者の識別子。 |
| `exhibitor.externalId` | 文字列 | 1666809514105001lSJN | クライアントシステムでの出展者の識別子。 |
| `exhibitor.name` | 文字列 | IBM | 出展者の名前。 |
| `lead.leadId` | 文字列 | 1666809456617001wyPj | The [!DNL RainFocus] リードの識別子。 |
| `lead.note` | 文字列 | | |
| `session.sessionId` | 文字列 | 1666809373585001t4aV | The [!DNL RainFocus] セッションの識別子。 |
| `session.externalId` | 文字列 | 1666809456617001wyPj | クライアントシステムでのセッションの識別子。 |
| `session.code` | 文字列 | GS3 | セッションのコード。 |
| `session.title` | 文字列 | Inspiration Keynote | セッションのタイトル。 |
| `session.length` | 整数 | 90 | セッションの長さ。 |
| `sessiontime.sessiontimeId` | 文字列 | 1673033149739001OJLZ | The [!DNL RainFocus] セッション時間の識別子。 |
| `sessiontime.startTime` | 文字列 | 2023-03-22 10:00:00 | セッションの開始時刻。 |
| `sessiontime.endTime` | 文字列 | 2023-03-22 10:00:00 | セッションの終了時間。 |
| `sessiontime.room` | 文字列 | B32 | セッションに使用された部屋。 |

{style="table-layout:auto"}

のスキーマを作成するには、以下を実行します。 [!DNL RainFocus] データを使用する場合は、次のドキュメントを読んで、API または UI を使用してスキーマを作成する手順を確認してください。

* [UI を使用したスキーマの作成](../../../xdm/tutorials/create-schema-ui.md)
* [API を使用してスキーマを作成する](../../../xdm/tutorials/create-schema-api.md)

>[!IMPORTANT]
>
>* スキーマは、 **XDM ExperienceEvent クラス。**
>* スキーマに **プライマリ ID**、およびは **プロファイルで有効**. 詳しくは、 [UI での id フィールドの定義](https://experienceleague.adobe.com/docs/experience-platform/xdm/ui/fields/identity.html)
>* この例の ID（電子メール）を、sha256 電子メールや ECID などの別の適切な識別子に置き換えることができます。

### RainFocus での統合プロファイルの作成 {#create-an-integration-profile-in-rainfocus}

サービスアカウントと XDM スキーマの準備が整ったら、 [!DNL Integration Profile] から [!DNL RainFocus] プラットフォーム。 The [!DNL Integration Profile] は、データをExperience Platformにストリーミングします。

にログインします。 [[!DNL RainFocus] platform](https://app.rainfocus.com). プライマリナビゲーションで、「 」を選択します。 **[!DNL Libraries]** 次に、「 **[!DNL Integration Profiles]**

![ライブラリと統合プロファイルが選択された RainFocus UI です。](/help/sources/images/tutorials/create/rainfocus/rainfocus_integration-profile.png)

新しいプロファイルを作成するには、 **(`+`)** アイコン。 次に、「 **Adobe Real-time Customer Data Platform** 次に、「 **OK**.

![RainFocus UI の統合プロファイルの作成ウィンドウ。](/help/sources/images/tutorials/create/rainfocus/rainfocus_integration-profile-select.png)

次に、Adobe Developer Portal プロジェクトで取得した資格情報を指定します。

* **クライアント ID**
* **クライアント秘密鍵**
* **テクニカルアカウント ID**
* **組織 ID**



資格情報を指定したら、「 」を選択します。 **[!DNL Save]**&#x200B;新しい [!DNL Integration Profile] 次に示す [!DNL RainFocus] ダッシュボード。

を選択します。 [!DNL Integration Profile] 作成したばかりの **プッシュタイプ** は既に設定されています。 これらは [エクスペリエンスイベント](https://experienceleague.adobe.com/docs/experience-platform/xdm/classes/experienceevent.html) これらは、発生時にExperience Platformに送信されます。

![RainFocus ダッシュボードの事前定義済みのプッシュタイプのリスト。](/help/sources/images/tutorials/create/rainfocus/rainfocus_integration-profile-setup.png)

サンプルの JSON ペイロードのコピーを取得するには、「 **[!DNL Sample JSON Payload]**. 次に、サンプルの JSON ペイロードをハイライト表示してコピーし、 **拡張子を.json にして新しいファイルに保存します。**. これは後でのExperience Platformで使用 [マッピング設定](../../tutorials/ui/create/analytics/rainfocus.md#mapping).

![RainFocus ダッシュボードのサンプル JSON ペイロード。](/help/sources/images/tutorials/create/rainfocus/rainfocus_integration-profile-json.png)

>[!TIP]
>
>**設定はまだ完了していません**：データフローを作成したら、に戻る必要があります。 [!DNL RainFocus] ダッシュボードを使用して、 [!DNL Integration Profile] 次の項目を指定して、 **ストリーミングエンドポイント URL** および **データフロー ID**.

## 次の手順

このドキュメントを読むと、データを [!DNL RainFocus] アカウントからExperience Platformへ。 次のガイドに進むことができます： [接続 [!DNL RainFocus] ユーザーインターフェイスを使用してExperience Platformを設定するには](../../tutorials/ui/create/analytics/rainfocus.md).