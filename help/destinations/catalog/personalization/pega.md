---
title: Pega Customer Decision Hub 接続
description: Adobe Experience Platformの Pega Customer Decision Hub の宛先を使用して、プロファイル属性とオーディエンスメンバーシップデータを Pega Customer Decision Hub に送信し、次に最適なアクションを決定します。
exl-id: 0546da5d-d50d-43ec-bbc2-9468a7db4d90
source-git-commit: 05e996f9e33e0d8be3d15a9ab3baaaf6d8152b5a
workflow-type: tm+mt
source-wordcount: '1047'
ht-degree: 24%

---

# Pega Customer Decision Hub 接続

## 概要 {#overview}

以下を使用します。 [!DNL Pega Customer Decision Hub] の宛先（プロファイル属性とオーディエンスメンバーシップデータをに送信） [!DNL Pega Customer Decision Hub] 次に最適な判定を行う場合。

に読み込まれたときの、Adobe Experience Platformからのプロファイルオーディエンスメンバーシップ [!DNL Pega Customer Decision Hub]は、アダプティブモデルで予測子として使用し、次に最適な判定をおこなうために、適切なコンテキストデータと行動データを配信するのに役立ちます。

>[!IMPORTANT]
>
>この宛先コネクタとドキュメントページは、Pegasystems によって作成および管理されます。 お問い合わせや更新のご依頼は、Pega に直接お問い合わせください [ここ](mailto:support@pega.com).

## ユースケース

[!DNL Customer Decision Hub] 宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platform のお客様がこの宛先を使用して解決できるサンプルユースケースを以下に示します。

### 通信業

マーケターは、データサイエンスモデルベースの次に最適なアクション ( [!DNL Pega Customer Decision Hub] 顧客エンゲージメント用に。 [!DNL Pega Customer Decision Hub] は、「Interested_In_5G」、「Interested_in_Unlimited_Dataplan」、「Interest_in_iPhone_accessories」など、顧客の意図に大きく依存しています。

### 金融機関

マーケターは、年金プランまたはリタイアメントプランのニュースレターを購読または購読解除した顧客に対するオファーを最適化したいと考えています。 金融サービス会社は、複数の顧客 ID を独自の CRM からAdobe Experience Platformに取り込み、独自のオフラインデータからオーディエンスを構築し、オーディエンスを開始および終了するプロファイルをに送信できます。 [!DNL Pega Customer Decision Hub] 送信チャネルでの次善の策 (NBA) 判定。

## 前提条件 {#prerequisites}

この宛先を使用してAdobe Experience Platformからデータを書き出す前に、次の前提条件を満たしていることを確認してください。 [!DNL Pega Customer Decision Hub]:

* を設定します。 [Adobe Experience Platformプロファイルとオーディエンスメンバーシップの統合コンポーネント](https://docs.pega.com/component/customer-decision-hub/adobe-experience-platform-profile-and-segment-membership-integration-component) の [!DNL Pega Customer Decision Hub] インスタンス。
* OAuth 2.0 の設定 [クライアント資格情報を使用したクライアント登録](https://docs.pega.com/security/87/creating-and-configuring-oauth-20-client-registration) 付与タイプを [!DNL Pega Customer Decision Hub] インスタンス。
* 設定 [リアルタイム実行データフロー](https://docs.pega.com/decision-management/87/creating-real-time-run-data-flows) (Adobeオーディエンスメンバーシップのデータフロー ) [!DNL Pega Customer Decision Hub] インスタンス。

## サポートされている ID {#supported-identities}

[!DNL Pega Customer Decision Hub] では、以下の表で説明するカスタムユーザー ID のアクティベーションをサポートしています。 詳しくは、 [id](/help/identity-service/namespaces.md).

| ターゲット ID | 説明 |
|---|---|
| *CustomerID* | プロファイルを一意に識別する共通のユーザー ID [!DNL Pega Customer Decision Hub] とAdobe Experience Platform |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | 識別子 (*CustomerID*)、属性（姓、名、場所など） および Audience Membership データ。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミング先は、常に API ベースの接続です。 Experience Platformでプロファイルが更新されるとすぐに、オーディエンスの評価に基づいて、コネクタは更新を宛先プラットフォームに送信します。 詳しくは、 [ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations). |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

#### OAuth 2 クライアント資格情報認証 {#oauth-2-client-credentials-authentication}

![クライアント資格情報認証を使用して OAuth 2 を使用し、Pega CDH の宛先に接続できる UI 画面の画像](../../assets/catalog/personalization/pega/pega-api-authentication-oauth2-client-credentials.png)

以下のフィールドに入力し、を選択します。 **[!UICONTROL 宛先に接続]**:

* **[!UICONTROL トークン URL にアクセス]**：の OAuth 2 アクセストークン URL [!DNL Pega Customer Decision Hub] インスタンス。
* **[!UICONTROL クライアント ID]**:OAuth 2 [!DNL client ID] が [!DNL Pega Customer Decision Hub] インスタンス。
* **[!UICONTROL クライアントの秘密鍵]**:OAuth 2 [!DNL client secret] が [!DNL Pega Customer Decision Hub] インスタンス。

### 宛先の詳細の入力 {#destination-details}

への認証接続を確立した後 [!DNL Pega Customer Decision Hub]に設定し、宛先に次の情報を入力します。

![Pega CDH の宛先の詳細に関する完了済みフィールドを示す UI 画面の画像](../../assets/catalog/personalization/pega/pega-connect-destination.png)

宛先の詳細を設定するには、必須フィールドに入力し、「 」を選択します。 **[!UICONTROL 次へ]**.

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL ホスト名]**：プロファイルが JSON データとして書き出される Pega Customer Decision Hub ホスト名。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。
>* 書き出す *id*、 **[!UICONTROL ID グラフを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png "ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。"){width="100" zoomable="yes"}

詳しくは、 [ストリーミングプロファイルの書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-streaming-profile-destinations.md) を参照してください。

### 宛先属性 {#attributes}

[[!UICONTROL 属性を選択]](../../ui/activate-streaming-profile-destinations.md#select-attributes)の手順では、[和集合スキーマ](../../../profile/home.md#profile-fragments-and-union-schemas)から一意の ID を選択することをお勧めします。宛先に書き出す一意の ID およびその他の XDM フィールドを選択します。

### マッピングの例：でのプロファイル更新のアクティブ化 [!DNL Pega Customer Decision Hub] {#mapping-example}

プロファイルをに書き出す際の正しい ID マッピングの例を以下に示します。 [!DNL Pega Customer Decision Hub].

ソースフィールドを選択しています。

* Adobe Experience Platformでプロファイルを一意に識別するソース ID として識別子（例： CustomerID）を選択し、 [!DNL Pega Customer Decision Hub].
* で書き出して更新する必要がある XDM ソースプロファイル属性の変更を選択します。 [!DNL Pega Customer Decision Hub].

ターゲットフィールドの選択：

* を選択します。 `CustomerID` 名前空間をターゲット ID として使用します。
* 対応する XDM ソースプロファイル属性にマッピングする必要がある宛先プロファイル属性名を選択します。

![ID マッピング](../../assets/catalog/personalization/pega/pega-source-destination-mapping.png)

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

プロファイルのオーディエンスメンバーシップの更新が成功すると、Pega マーケティングオーディエンスメンバーシップデータストアにオーディエンス識別子、名前、ステータスが挿入されます。 メンバーシップデータは、 [!DNL Pega Customer Decision Hub]、以下に示すように。
![顧客プロファイルデザイナーを使用して顧客オーディエンスメンバーシップAdobeを顧客に関連付けることができる UI 画面の画像](../../assets/catalog/personalization/pega/pega-profile-designer-associate.png)

オーディエンスメンバーシップデータは、次に示すように、Pega Next-Best-Action Designer Engagement ポリシーで、次に最適な判定を行うために使用されます。
![Pega の次に最適なアクションデザイナーのエンゲージメントポリシーの条件としてオーディエンスメンバーシップフィールドを追加できる UI 画面の画像](../../assets/catalog/personalization/pega/pega-profile-designer-engagment.png)

次に示すように、顧客オーディエンスメンバーシップデータフィールドは、アダプティブモデルで述語として追加されます。
![UI 画面の画像です。この画面では、Prediction Studio を使用して、アダプティブモデルでオーディエンスメンバーシップフィールドを述語として追加できます](../../assets/catalog/personalization/pega/pega-profile-designer-adaptivemodel.png)

## その他のリソース {#additional-resources}

詳しくは、 [OAuth 2.0 クライアント登録の設定](https://docs.pega.com/security/87/creating-and-configuring-oauth-20-client-registration) in [!DNL Pega Customer Decision Hub].

詳しくは、 [データフローのリアルタイム実行の作成](https://docs.pega.com/decision-management/87/creating-real-time-run-data-flows) in [!DNL Pega Customer Decision Hub].

詳しくは、 [顧客プロファイルデザイナーでの顧客レコードの管理](https://docs.pega.com/whats-new-pega-platform/manage-customer-records-customer-profile-designer-86).

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
