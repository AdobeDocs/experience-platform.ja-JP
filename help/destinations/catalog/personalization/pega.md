---
title: （V1） Pega CDH リアルタイムオーディエンス接続
description: Adobe Experience Platformの Pega Customer Decision Hub リアルタイムオーディエンス宛先を使用して、プロファイル属性とオーディエンスメンバーシップデータを Pega Customer Decision Hub に送信し、次善のアクションの意思決定を行います。
exl-id: 0546da5d-d50d-43ec-bbc2-9468a7db4d90
source-git-commit: 71de5b0d3e9c4413caa911fbe174e74c0e409d89
workflow-type: tm+mt
source-wordcount: '1075'
ht-degree: 18%

---

# Pega CDH リアルタイムオーディエンス接続

>[!IMPORTANT]
>
>このバージョンの Pega Customer Decision Hub リアルタイムオーディエンス宛先は、1 つの Pega Customer Decision アプリケーションのみをサポートします。 複数の Pega Customer Decision Hub アプリケーションが設定されている場合は、[ （V2） Pega CDH Realtime Audience 宛先コネクタ ](./pega-v2.md) を使用する必要があります。

## 概要 {#overview}

Adobe Experience Platformの [!DNL Pega Customer Decision Hub] リアルタイムオーディエンスの宛先を使用して、プロファイル属性とオーディエンスメンバーシップデータを [!DNL Pega Customer Decision Hub] に送信し、次善のアクションの意思決定を行います。

Adobe Experience Platformのプロファイルオーディエンスメンバーシップは、[!DNL Pega Customer Decision Hub] に読み込まれると、アダプティブモデルの予測因子として使用でき、次善のアクションの意思決定を目的とした適切なコンテキストデータや行動データを提供するのに役立ちます。

>[!IMPORTANT]
>
>この宛先コネクタとドキュメントページは、Pegasystems が作成および管理します。 お問い合わせや更新のリクエストについては、Pega に直接お問い合わせください [ こちら ](mailto:support@pega.com)。

## ユースケース

[!DNL Customer Decision Hub] の宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platformのお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

### 通信業

マーケターは、[!DNL Pega Customer Decision Hub] が顧客エンゲージメントのために提供する、データサイエンスモデルベースの次に最適なアクションのインサイトを活用したいと考えています。 [!DNL Pega Customer Decision Hub] れは、お客様の意図に大きく依存しています。例えば、「Interested_In_5G」、「Interested_in_Unlimited_Dataplan」、「Interest_in_iPhone_accessories」などです。

### 金融機関

マーケターは、年金プランや退職金プランのニュースレターを購読または購読解除した顧客に対するオファーを最適化したいと考えています。 金融サービス企業は、独自の CRM からAdobe Experience Platformに複数の顧客 ID を取り込み、独自のオフラインデータからオーディエンスを作成し、オーディエンスをエントリおよび離脱するプロファイルを送信して、アウトバウンドチャネルでの次善 [!DNL Pega Customer Decision Hub] 策（NBA）の意思決定に使用できます。

## 前提条件 {#prerequisites}

この宛先を使用してAdobe Experience Platformからデータを書き出す前に、[!DNL Pega Customer Decision Hub] で次の前提条件を満たしていることを確認してください。

* [Adobe Experience Platform プロファイルとオーディエンスメンバーシップの統合コンポーネント ](https://docs.pega.com/bundle/components/page/customer-decision-hub/components/adobe-membership-component.html) を [!DNL Pega Customer Decision Hub] インスタンスに設定します。
* [!DNL Pega Customer Decision Hub] インスタンスで、OAuth 2.0[ クライアント資格情報を使用したクライアント登録 ](https://docs.pega.com/bundle/platform/page/platform/security/configure-oauth-2-client-registration.html) 付与タイプを設定します。
* [!DNL Pega Customer Decision Hub] インスタンスでAdobe オーディエンスメンバーシップデータフローを設定するには、[ リアルタイム実行データフロー ](https://docs.pega.com/bundle/platform/page/platform/decision-management/data-flow-run-real-time-create.html) を使用します。

## サポートされている ID {#supported-identities}

[!DNL Pega Customer Decision Hub] では、以下の表で説明するカスタムユーザー ID のアクティベーションをサポートしています。 詳しくは、[ID](/help/identity-service/features/namespaces.md) を参照してください。

| ターゲット ID | 説明 |
|---|---|
| *顧客 ID* | [!DNL Pega Customer Decision Hub] およびAdobe Experience Platformでプロファイルを一意に識別する共通のユーザー ID |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | 識別子（*顧客 ID*）、属性（姓、名、場所など）およびオーディエンスメンバーシップデータを含む、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常にオンの API ベースの接続です。 Experience Platformでプロファイルが更新されるとすぐに、オーディエンスの評価に基づいて、コネクタは更新を宛先プラットフォームに送信します。 詳しくは、[ ストリーミングの宛先 ](/help/destinations/destination-types.md#streaming-destinations) を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

#### OAuth 2 クライアント資格情報認証 {#oauth-2-client-credentials-authentication}

![OAuth 2 とクライアント資格情報認証を使用して、Pega CDH 宛先に接続できる UI 画面の画像 ](../../assets/catalog/personalization/pega/pega-api-authentication-oauth2-client-credentials.png)

以下のフィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

* **[!UICONTROL アクセストークン URL]**：お使いの [!DNL Pega Customer Decision Hub] インスタンスの OAuth 2 アクセストークン URL。
* **[!UICONTROL クライアント ID]**:[!DNL Pega Customer Decision Hub] インスタンスで生成した OAuth 2 [!DNL client ID]。
* **[!UICONTROL クライアント秘密鍵]**:[!DNL Pega Customer Decision Hub] インスタンスで生成した OAuth 2 [!DNL client secret]。

### 宛先の詳細の入力 {#destination-details}

[!DNL Pega Customer Decision Hub] への認証接続を確立したら、宛先の次の情報を指定します。

![Pega CDH 宛先の詳細に関する入力済みフィールドを示す UI 画面の画像 ](../../assets/catalog/personalization/pega/pega-connect-destination.png)

宛先の詳細を設定するには、必須フィールドに入力し、「**[!UICONTROL 次へ]**」を選択します。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL Pega CDH ホスト名]**：プロファイルが JSON データとして書き出される Pega Customer Decision Hub ホスト名。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティブ化する手順については、[ ストリーミングプロファイル書き出し宛先に対するオーディエンスデータのアクティブ化 ](../../ui/activate-streaming-profile-destinations.md) を参照してください。

### 宛先属性 {#attributes}

[[!UICONTROL 属性を選択]](../../ui/activate-streaming-profile-destinations.md#select-attributes)の手順では、[和集合スキーマ](../../../profile/home.md#profile-fragments-and-union-schemas)から一意の ID を選択することをお勧めします。宛先に書き出す一意の ID およびその他の XDM フィールドを選択します。

### マッピングの例：[!DNL Pega Customer Decision Hub] でのプロファイル更新のアクティブ化 {#mapping-example}

プロファイルを [!DNL Pega Customer Decision Hub] に書き出す際の正しい ID マッピングの例を以下に示します。

ソースフィールドを選択中：

* ID （例：CustomerID）を、Adobe Experience Platformおよび [!DNL Pega Customer Decision Hub] でプロファイルを一意に識別するソース ID として選択します。
* [!DNL Pega Customer Decision Hub] で書き出しと更新が必要な XDM ソースプロファイル属性の変更を選択します。

ターゲットフィールドを選択：

* `CustomerID` 名前空間をターゲット ID として選択します。
* 対応する XDM ソースプロファイル属性にマッピングする必要がある宛先プロファイル属性名を選択します。

![ID マッピング ](../../assets/catalog/personalization/pega/pega-source-destination-mapping.png)

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

プロファイルのオーディエンスメンバーシップが正常に更新されると、オーディエンスの識別子、名前およびステータスが Pega マーケティングオーディエンスメンバーシップデータストアに挿入されます。 以下に示すように、メンバーシップデータは、[!DNL Pega Customer Decision Hub] の顧客プロファイルDesignerを使用して顧客と関連付けられます。
![ 顧客プロファイル Designerを使用して、Adobe オーディエンスメンバーシップデータを顧客に関連付けることができる UI 画面の画像 ](../../assets/catalog/personalization/pega/pega-profile-designer-associate.png)

次に示すように、オーディエンスメンバーシップデータは、次善のアクションの意思決定のために Pega の次善のアクションのDesignerエンゲージメントポリシーで使用されます。
![Pega の次善アクションDesignerのエンゲージメントポリシーの条件としてオーディエンスメンバーシップフィールドを追加できる UI 画面の画像 ](../../assets/catalog/personalization/pega/pega-profile-designer-engagement.png)

以下に示すように、顧客オーディエンスメンバーシップデータフィールドがアダプティブモデルの予測因子として追加されます。
![ 予測スタジオを使用して、オーディエンスメンバーシップフィールドをアダプティブモデルの予測因子として追加できる UI 画面の画像 ](../../assets/catalog/personalization/pega/pega-profile-designer-adaptivemodel.png)

## その他のリソース {#additional-resources}

詳しくは、次の [!DNL Pega] ドキュメントリソースを参照してください。

* [OAuth 2.0 クライアント登録の設定 ](https://docs.pega.com/bundle/platform/page/platform/security/configure-oauth-2-client-registration.html)
* [ データフローのリアルタイム実行の作成 ](https://docs.pega.com/bundle/platform/page/platform/decision-management/data-flow-run-real-time-create.html)
* [ 顧客プロファイルDesignerでの顧客レコードの管理 ](https://docs.pega.com/bundle/customer-decision-hub/page/customer-decision-hub/implement/profile-designer-data-management.html)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
