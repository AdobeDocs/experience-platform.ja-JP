---
keywords: モバイル；モバイルエンゲージメントの宛先；LINE;LINE モバイルエンゲージメントの宛先
title: LINE 接続
description: LINE の宛先を使用すると、Platform セグメントにプロファイルを追加し、接続されたユーザーにパーソナライズされたエクスペリエンスを配信できます。
last-substantial-update: 2022-11-08T00:00:00Z
exl-id: 9981798a-61f2-4a09-9a33-57e63eb36d43
source-git-commit: 83778bc5d643f69e0393c0a7767fef8a4e8f66e9
workflow-type: tm+mt
source-wordcount: '1183'
ht-degree: 46%

---

# [!DNL LINE] 接続

## 概要 {#overview}

[[!DNL LINE]](https://line.me/en/) は、人物、サービスおよび情報をつなぎ、チャットアプリからエンターテインメント、ソーシャルおよび日々のアクティビティのハブに成長した人気のコミュニケーションプラットフォームです。

この [!DNL Adobe Experience Platform] [宛先](/help/destinations/home.md) は [[!DNL LINE] メッセージング API](https://developers.line.biz/en/reference/messaging-api/). 内で、接続としてExperience Platformセグメントからプロファイルをアクティブ化できます [!DNL LINE] ビジネスニーズに合わせて

[!DNL LINE] は、Bearer トークンを認証メカニズムとして使用し、 [!DNL LINE] メッセージング API への認証手順 [!DNL LINE] インスタンスは下、内 [宛先に対する認証](#authenticate) 」セクションに入力します。

## ユースケース {#use-cases}

マーケターは、セグメントを組み込んで、モバイルエンゲージメントの宛先でユーザーをターゲット設定できます [!DNL Adobe Experience Platform]. さらに、エクスペリエンスの属性に基づいて、パーソナライズされたエクスペリエンスを配信できます [!DNL Adobe Experience Platform] プロファイル：セグメントとプロファイルが [!DNL Adobe Experience Platform].

## 前提条件 {#prerequisites}

### [!DNL LINE] 前提条件 {#prerequisites-destination}

Platform から [!DNL LINE] アカウントにデータを書き出すには、[!DNL LINE] で次の前提条件に注意してください。

#### [!DNL LINE] アカウントが必要です {#prerequisites-account}

を登録して、 [!DNL LINE] アカウントを作成します。 アカウントを作成するには：

1. 次に移動： [!DNL LINE] [アカウントログイン](https://account.line.biz/login?redirectUri=https%3A%2F%2Fmanager.line.biz%2F) ページ
2. 選択 **[!UICONTROL アカウントの作成]**.

#### の収集 [!DNL LINE channel access token (long-lived)] から [!DNL LINE] 開発者コンソール {#gather-credentials}

Platform がにアクセスできるようにするには、以下を実行します。 [!DNL LINE] リソース、 *[!DNL Channel access token (long-lived)]* 望みの [!DNL LINE] *メッセージング API* チャネル。

1. 次を使用してログイン： [!DNL LINE] アカウント [[!DNL LINE] 開発者コンソール](https://developers.line.biz/console).
1. 次に、 *[!DNL Providers]* リストを開き、 *[!DNL Provider]* 興味を持って最後に選択する *メッセージング API* チャネルを開き、設定にアクセスします。 初めてデベロッパーコンソールにアクセスする場合は、 [[!DNL LINE] ドキュメント](https://developers.line.biz/en/docs/messaging-api/getting-started/) をクリックして、プロバイダーの作成に必要な手順を完了します。
1. 最後に、 ***[!DNL Channel access token]*** セクションを開き、 ***[!DNL Channel access token (long-lived)]*** 内で必要な値 [宛先に対する認証](#authenticate) 手順

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| `[!DNL Channel access token (long-lived)]` | お使いの「[!DNL LINE Channel access token (long-lived)]」。 | `aaa2112XSMWqLXR7..........nyilFU=` |

詳しくは、 [[!DNL LINE] ドキュメント](https://developers.line.biz/en/docs/messaging-api/getting-started/) チャネルの作成や、既存のチャネルへのチャネルの追加に関するガイダンス [!DNL LINE] を通じて説明する [!DNL LINE] 開発者コンソール。

## サポートされる ID {#supported-identities}

[!DNL LINE] では、次の表で説明する id の更新と書き出しをサポートしています。 [ID](/help/identity-service/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 |
|---|---|
| 広告主 (IFA) の ID | ソース ID が IFA の場合に、広告主 (IFA) のターゲット ID を選択します *(Apple ID for Advertisers)* または GAID *(Google Advertising ID) 名前空間。 |
| LINE ユーザー ID | ソース ID が LINE ユーザー ID の場合は、ユーザー ID ターゲット ID を選択します。 |

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | セグメント（オーディエンス）のすべてのメンバーを、 [!DNL LINE] 宛先。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。セグメント評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style=&quot;table-layout:auto&quot;}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

**[!UICONTROL 宛先]**／**[!UICONTROL カタログ]**&#x200B;内で [!DNL LINE] を検索します。または、 **[!UICONTROL モバイルエンゲージメント]** カテゴリ。

### 宛先に対する認証 {#authenticate}

宛先を認証するには、「 **[!UICONTROL 宛先に接続]**」を選択します。
![認証方法を示す Platform UI のスクリーンショット。](../../assets/catalog/mobile-engagement/line/authenticate-destination.png)

以下の必須のフィールドに入力します。
* **[!UICONTROL Bearer トークン]**:お使いの [!DNL LINE Channel access token (long-lived)] から [!DNL LINE] 開発者コンソール。 詳しくは、 [資格情報を収集](#gather-credentials) 」セクションに入力します。

指定した詳細が有効な場合、UI で&#x200B;**[!UICONTROL 接続済み]**&#x200B;ステータスに緑色のチェックマークが付きます。その後、次の手順に進むことができます。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横にアスタリスクが表示される場合は、そのフィールドが必須であることを示します。
![宛先の詳細を示す Platform UI のスクリーンショット。](../../assets/catalog/mobile-engagement/line/destination-details.png)

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL オーディエンスタイプ]**:選択 **[!UICONTROL 広告主 (IFA) の ID]** 書き出す id のタイプがの場合 *広告主 (IFA) の ID*. 選択 **[!UICONTROL LINE ユーザー ID]** 書き出す id のタイプがの場合 *LINE ユーザー ID*. 詳しくは、 [サポートされる ID](#supported-identities) 「 」セクションを参照してください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
>
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]**[に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先にオーディエンスセグメントをアクティベートする手順は、[ストリーミングセグメントの書き出し宛先へのプロファイルとセグメントのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### 属性と ID のマッピング {#map}

Adobe Experience Platform から [!DNL LINE] 宛先にオーディエンスデータを正しく送信するには、フィールドマッピングの手順を実行する必要があります。マッピングは、Platform アカウント内の Experience Data Model（XDM）スキーマフィールドと、ターゲット宛先から対応する同等のスキーマフィールドとの間にリンクを作成して構成されます。 XDM フィールドを [!DNL LINE] 宛先フィールドに正しくマッピングするには、次の手順に従います。

ソース ID に応じて、次のターゲット ID 名前空間をマッピングする必要があります。 |ターゲット ID |ソースフィールド |ターゲットフィールド | | — | — | — | |広告主 (IFA) の ID | `IDFA` または `GAID` | `LineId` | | LINE ユーザー ID | `UserID` | `LineId` |

ターゲット ID が *LINE ユーザー ID* 以下が必要です。
![ターゲット ID に LINE ユーザー ID を使用する場合の、Target マッピングを示す Platform UI のスクリーンショット例。](../../assets/catalog/mobile-engagement/line/mappings-userid.png)

ターゲット ID が *広告主 (IFA) の ID* 以下が必要です。
![ターゲット ID に広告主 (IFA) の ID を使用する場合の、Target マッピングを示す Platform UI のスクリーンショット例。](../../assets/catalog/mobile-engagement/line/mappings-idfa.png)

## データの書き出しを検証する {#exported-data}

データのエクスポートが正常にExperience Platformされると、 [!DNL LINE] 宛先が以下の範囲内に新しいオーディエンスを作成 [!DNL LINE] 選択したセグメント名を使用します。

宛先が正しく設定されていることを検証するには、次の手順に従います。

1. In [!DNL LINE]、にログインします。 [マネージャコンソール](https://manager.line.biz/).

1. 次に、に移動します。 **[!UICONTROL データ制御]** > **[!UICONTROL オーディエンス]** をクリックし、 **[!UICONTROL オーディエンス名]** 列。

1. 更新されたボリュームは、セグメント内のカウントと一致します。

1. この *タイプ* 列に記載されている **[!UICONTROL UserID]** 書き出した id のタイプがの場合 *UserID*. 同様に、 *タイプ* 列に記載されている **[!UICONTROL モバイル広告 ID]** 書き出した id のタイプがの場合 *IDFA*.

内の設定例 [!DNL LINE] は次のように表示されます。
![オーディエンスの量を示す LINE UI のスクリーンショット。](../../assets/catalog/mobile-engagement/line/audience-volume.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
