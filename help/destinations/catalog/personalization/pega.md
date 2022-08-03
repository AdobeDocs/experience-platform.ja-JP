---
title: Pega Customer Decision Hub 接続
description: Adobe Experience Platformの Pega Customer Decision Hub の宛先を使用して、プロファイル属性とセグメントメンバーシップデータを Pega Customer Decision Hub に送信し、次に最適な判定をおこないます。
exl-id: 0546da5d-d50d-43ec-bbc2-9468a7db4d90
source-git-commit: f06afec31b7fa550a612280b8ad665b8393ee2e3
workflow-type: tm+mt
source-wordcount: '1003'
ht-degree: 7%

---

# Pega Customer Decision Hub 接続

## 概要 {#overview}

以下を使用： [!DNL Pega Customer Decision Hub] の宛先 (Adobe Experience Platformの宛先 )：プロファイル属性とセグメントメンバーシップデータをに送信します。 [!DNL Pega Customer Decision Hub] 次に最適な判定をおこなう場合。

に読み込まれたときの、Adobe Experience Platformからのプロファイルセグメントメンバーシップ [!DNL Pega Customer Decision Hub]は、アダプティブモデルで予測子として使用し、次に最適な判定をおこなうために、適切なコンテキストデータと行動データを配信するのに役立ちます。

>[!IMPORTANT]
>
>このドキュメントページは Pegasystems によって作成されました。 お問い合わせや更新のご依頼は、Pega に直接お問い合わせください [ここ](mailto:support@pega.com).

## ユースケース

をいつどのように使用するかをより深く理解するのに役立ちます。 [!DNL Customer Decision Hub] の宛先について、Adobe Experience Platformのお客様がこの宛先を使用して解決できる使用例を以下に示します。

### 通信業

マーケターは、データサイエンスモデルベースの次に最適なアクション ( [!DNL Pega Customer Decision Hub] 顧客エンゲージメント用に。 [!DNL Pega Customer Decision Hub] は、「Interested_In_5G」、「Interested_in_Unlimited_Dataplan」、「Interest_in_iPhone_accessories」など、顧客の意図に大きく依存しています。

### 金融機関

マーケターは、年金プランまたはリタイアメントプランのニュースレターを購読または購読解除した顧客に対するオファーを最適化したいと考えています。 金融サービス会社は、複数の顧客 ID を独自の CRM からAdobe Experience Platformに取り込み、独自のオフラインデータからセグメントを作成し、セグメントを開始および終了するプロファイルをに送信できます。 [!DNL Pega Customer Decision Hub] 送信チャネルでの次善の策 (NBA) 判定。

## 前提条件 {#prerequisites}

この宛先を使用してAdobe Experience Platformからデータを書き出す前に、次の前提条件を満たしていることを確認してください。 [!DNL Pega Customer Decision Hub]:

* Adobeセグメントメンバーシップコンポーネントを [!DNL Pega Customer Decision Hub] インスタンス。
* OAuth 2.0 の設定 [クライアント資格情報を使用したクライアント登録](https://docs.pega.com/security/87/creating-and-configuring-oauth-20-client-registration) 付与タイプを [!DNL Pega Customer Decision Hub] インスタンス。
* 設定 [リアルタイム実行データフロー](https://docs.pega.com/decision-management/87/creating-real-time-run-data-flows) (Adobeセグメントメンバーシップデータフロー ) [!DNL Pega Customer Decision Hub] インスタンス。

## サポートされる ID {#supported-identities}

[!DNL Pega Customer Decision Hub] では、以下の表で説明するカスタムユーザー ID のアクティベーションをサポートしています。 詳しくは、 [id](/help/identity-service/namespaces.md).

| ターゲット ID | 説明 |
|---|---|
| *CustomerID* | プロファイルを一意に識別する共通のユーザー ID [!DNL Pega Customer Decision Hub] とAdobe Experience Platform |

{style=&quot;table-layout:auto&quot;}

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | 識別子 (*CustomerID*)、属性（姓、名、場所など） およびセグメントメンバーシップデータ。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミング先は、常に API ベースの接続です。 Experience Platformでプロファイルが更新されるとすぐに、セグメント評価に基づいて、コネクタが更新を宛先プラットフォームに送信します。 詳しくは、 [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations). |

{style=&quot;table-layout:auto&quot;}

## 宛先への接続 {#connect}

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションに記載されているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

#### OAuth 2 クライアント資格情報認証 {#oauth-2-client-credentials-authentication}

![クライアント資格情報認証を使用して OAuth 2 を使用し、Pega CDH の宛先に接続できる UI 画面の画像](../../assets/catalog/personalization/pega/pega-api-authentication-oauth2-client-credentials.png)

以下のフィールドに入力し、を選択します。 **[!UICONTROL 宛先に接続]**:

* **[!UICONTROL トークン URL にアクセス]**:の OAuth 2 アクセストークン URL [!DNL Pega Customer Decision Hub] インスタンス。
* **[!UICONTROL クライアント ID]**:OAuth 2 [!DNL client ID] が [!DNL Pega Customer Decision Hub] インスタンス。
* **[!UICONTROL クライアント秘密鍵]**:OAuth 2 [!DNL client secret] が [!DNL Pega Customer Decision Hub] インスタンス。

### 宛先の詳細を入力 {#destination-details}

への認証接続を確立した後 [!DNL Pega Customer Decision Hub]に設定し、宛先に次の情報を入力します。

![Pega CDH の宛先の詳細に関する完了済みフィールドを示す UI 画面の画像](../../assets/catalog/personalization/pega/pega-connect-destination.png)

宛先の詳細を設定するには、必須フィールドに入力し、「 」を選択します。 **[!UICONTROL 次へ]**.

* **[!UICONTROL 名前]**:将来この宛先を認識するための名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL ホスト名]**:プロファイルが JSON データとして書き出される Pega Customer Decision Hub ホスト名。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

詳しくは、 [ストリーミングプロファイルの書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-streaming-profile-destinations.md) を参照してください。

### 宛先属性 {#attributes}

内 [[!UICONTROL 属性を選択]](../../ui/activate-streaming-profile-destinations.md#select-attributes) 手順に従い、Adobeでは、 [和集合スキーマ](../../../profile/home.md#profile-fragments-and-union-schemas). 宛先に書き出す一意の ID およびその他の XDM フィールドを選択します。

### マッピングの例：プロファイル更新のアクティブ化 [!DNL Pega Customer Decision Hub]

プロファイルをに書き出す際の正しい ID マッピングの例を以下に示します。 [!DNL Pega Customer Decision Hub].

ソースフィールドを選択しています。

* 識別子を選択します ( 例：(CustomerID) をAdobe Experience Platformでプロファイルを一意に識別するソース ID として、 [!DNL Pega Customer Decision Hub].
* で書き出して更新する必要がある XDM ソースプロファイル属性の変更を選択します。 [!DNL Pega Customer Decision Hub].

ターゲットフィールドの選択：

* を選択します。 `CustomerID` 名前空間をターゲット ID として使用します。
* 対応する XDM ソースプロファイル属性にマッピングする必要がある宛先プロファイル属性名を選択します。

![ID マッピング](../../assets/catalog/personalization/pega/pega-source-destination-mapping.png)

## エクスポートされたデータ/データエクスポートの検証 {#exported-data}

プロファイルのセグメントメンバーシップの更新が成功すると、セグメント識別子、名前、ステータスが Pega マーケティングセグメントメンバーシップデータストアに挿入されます。 メンバーシップデータは、 [!DNL Pega Customer Decision Hub]、以下に示すように。
![顧客プロファイルデザイナーを使用して顧客セグメントメンバーシップデータをAdobeに関連付けることができる UI 画面の画像](../../assets/catalog/personalization/pega/pega-profile-designer-associate.png)

セグメントメンバーシップデータは、次に示すように、Pega Next-Best-Action Designer Engagement Polices で、次に最適な判定を行うために使用されます。
![Pega の次に最適なアクションデザイナーのエンゲージメントポリシーの条件としてセグメントメンバーシップフィールドを追加できる UI 画面の画像](../../assets/catalog/personalization/pega/pega-profile-designer-engagment.png)

次に示すように、顧客セグメントメンバーシップデータフィールドは、アダプティブモデルで述語として追加されます。
![UI 画面の画像です。この画面では、Prediction Studio を使用して、アダプティブモデルの述語としてセグメントメンバーシップフィールドを追加できます](../../assets/catalog/personalization/pega/pega-profile-designer-adaptivemodel.png)

## その他のリソース {#additional-resources}

詳しくは、 [OAuth 2.0 クライアント登録の設定](https://docs.pega.com/security/87/creating-and-configuring-oauth-20-client-registration) in [!DNL Pega Customer Decision Hub].

詳しくは、 [データフローのリアルタイム実行の作成](https://docs.pega.com/decision-management/87/creating-real-time-run-data-flows) in [!DNL Pega Customer Decision Hub].

詳しくは、 [顧客プロファイルデザイナーでの顧客レコードの管理](https://docs.pega.com/whats-new-pega-platform/manage-customer-records-customer-profile-designer-86).

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。詳しくは、 [!DNL Adobe Experience Platform] データガバナンスを強制します。詳しくは、 [データガバナンスの概要](/help/data-governance/home.md).
