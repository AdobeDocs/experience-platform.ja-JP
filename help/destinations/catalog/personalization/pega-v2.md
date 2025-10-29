---
title: （V2） Pega CDH リアルタイムオーディエンス接続
description: Adobe Experience Platformの Pega Customer Decision Hub リアルタイムオーディエンス宛先を使用して、プロファイル属性とオーディエンスメンバーシップデータを Pega Customer Decision Hub に送信し、次善のアクションの意思決定を行います。
exl-id: cbb998f9-c268-4d65-87d8-fab56c0844dc
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '1000'
ht-degree: 14%

---

# （V2） Pega CDH リアルタイムオーディエンス接続

## 概要 {#overview}

Adobe Experience Platformの（V2） [!DNL Pega CDH Realtime Audience] 宛先を使用して、プロファイル属性とオーディエンスメンバーシップデータを [!DNL Pega Customer Decision Hub] に送信し、次善のアクションの意思決定を行います。

Adobe Experience Platformのプロファイルオーディエンスメンバーシップは、[!DNL Pega Customer Decision Hub] に読み込まれると、アダプティブモデルの予測因子として使用でき、次善のアクションの意思決定を目的とした適切なコンテキストデータや行動データを提供するのに役立ちます。

>[!IMPORTANT]
>
>この宛先コネクタとドキュメントページは、Pegasystems が作成および管理します。 お問い合わせや更新のリクエストについては、Pega に直接お問い合わせください [&#x200B; こちら &#x200B;](mailto:support@pega.com)。

## ユースケース {#use-cases}

[!DNL Customer Decision Hub] の宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platformのお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

### 通信業 {#telecommunications}

マーケターは、[!DNL Pega Customer Decision Hub] が顧客エンゲージメントのために提供する、データサイエンスモデルベースの次善のアクションからのインサイトを活用したいと考えています。 [!DNL Pega Customer Decision Hub] は、「Interested_In_5G」、「Interested_in_Unlimited_Dataplan」、「Interest_in_iPhone_accessories」などの顧客の意図に大きく依存して、アウトバウンドチャネルでの次善アクション（NBA）の意思決定を決定します。

### 金融機関 {#financial-services}

マーケターは、年金プランや退職金プランのニュースレターを購読または購読解除した顧客に対するオファーを最適化したいと考えています。 金融サービス企業は、自社の CRM からAdobe Experience Platformに複数の顧客 ID を取り込み、自社のオフラインデータからオーディエンスを作成し、オーディエンスをエントリおよび離脱するプロファイルを送信して、アウトバウンドチャネルでの次善の策 [!DNL Pega Customer Decision Hub] （NBA）を決定できます。

## 前提条件 {#prerequisites}

この宛先を使用してAdobe Experience Platformからデータを書き出す前に、[!DNL Pega Customer Decision Hub] で次の前提条件を満たしていることを確認してください。

* [Adobe Experience Platform プロファイルとオーディエンスメンバーシップの統合コンポーネント &#x200B;](https://docs.pega.com/bundle/components/page/customer-decision-hub/components/adobe-membership-component.html) を [!DNL Pega Customer Decision Hub] インスタンスに設定します。
* [&#x200B; インスタンスで、OAuth 2.0](https://docs.pega.com/bundle/platform/page/platform/security/configure-oauth-2-client-registration.html) クライアント資格情報を使用したクライアント登録 [!DNL Pega Customer Decision Hub] 付与タイプを設定します。
* [&#x200B; インスタンスでAdobe オーディエンスメンバーシップデータフローを設定するには、](https://docs.pega.com/bundle/platform/page/platform/decision-management/data-flow-run-real-time-create.html) リアルタイム実行データフロー [!DNL Pega Customer Decision Hub] を使用します。

## サポートされている ID {#supported-identities}

[!DNL Pega Customer Decision Hub] では、以下の表で説明するカスタムユーザー ID のアクティベーションをサポートしています。 詳しくは、[ID](/help/identity-service/features/namespaces.md) を参照してください。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| `CustomerID` | 顧客 ID | [!DNL Pega Customer Decision Hub] およびAdobe Experience Platformでプロファイルを一意に識別する共通ユーザー ID。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Profile-based]** | 識別子（*顧客 ID*）、属性（姓、名、場所など）およびオーディエンスメンバーシップデータを含む、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常にオンの API ベースの接続です。 Experience Platformでプロファイルが更新されるとすぐに、オーディエンスの評価に基づいて、コネクタは更新を宛先プラットフォームに送信します。 詳しくは、[&#x200B; ストリーミングの宛先 &#x200B;](/help/destinations/destination-types.md#streaming-destinations) を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

#### OAuth 2 クライアント資格情報認証 {#oauth-2-client-credentials-authentication}

![OAuth 2 とクライアント資格情報認証を使用して、Pega CDH 宛先に接続できる UI 画面の画像 &#x200B;](../../assets/catalog/personalization/pega/pega-api-authentication-oauth2-client-credentials.png)

以下のフィールドに入力し、**[!UICONTROL Connect to destination]** を選択します。

* **[!UICONTROL Access Token URL]**：お使いの [!DNL Pega Customer Decision Hub] インスタンスの OAuth 2 アクセストークン URL。
* **[!UICONTROL Client ID]**:[!DNL client ID] インスタンスで生成した OAuth 2 [!DNL Pega Customer Decision Hub]。
* **[!UICONTROL Client Secret]**:[!DNL client secret] インスタンスで生成した OAuth 2 [!DNL Pega Customer Decision Hub]。

### 宛先の詳細の入力 {#destination-details}

[!DNL Pega Customer Decision Hub] への認証接続を確立したら、宛先の次の情報を指定します。

![Pega CDH 宛先の詳細に関する入力済みフィールドを示す UI 画面の画像 &#x200B;](../../assets/catalog/personalization/pega/pega-connect-destination-v2.png)

宛先の詳細を設定するには、必須フィールドに入力し、「**[!UICONTROL Next]**」を選択します。

* **[!UICONTROL Name]**：今後この宛先を認識するための名前。
* **[!UICONTROL Description]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL Pega CDH Host Name]**：プロファイルを JSON データとして書き出す Pega Customer Decision Hub ホスト名。
* **[!UICONTROL Application alias]**:Customer Decision Hub アカウント用に設定したアプリケーションエイリアス。 詳しくは、[&#x200B; インスタンスでの &#x200B;](https://docs.pega.com/bundle/platform/page/platform/user-experience/adding-application-url-alias.html) アプリケーション URL エイリアスの追加 [!DNL Pega Customer Decision Hub] を参照してください。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。<br> ![&#x200B; 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティブ化する手順については、[&#x200B; ストリーミングプロファイル書き出し宛先に対するオーディエンスデータのアクティブ化 &#x200B;](../../ui/activate-streaming-profile-destinations.md) を参照してください。

### マッピング {#mapping}

[!UICONTROL Mapping] の手順では、宛先に書き出す一意の ID を結合スキーマおよび他の XDM フィールドから選択します。

### マッピングの例：[!DNL Pega Customer Decision Hub] でのプロファイル更新のアクティブ化 {#mapping-example}

プロファイルを [!DNL Pega Customer Decision Hub] に書き出す際の正しい ID マッピングの例を以下に示します。

* Adobe Experience Platformおよび [!DNL Pega Customer Decision Hub] でプロファイルを一意に識別するソース ID を選択します。 例：`CustomerID`。
* 選択したソースプロファイル属性をマッピングする宛先プロファイル属性を選択します。

![ID マッピング &#x200B;](../../assets/catalog/personalization/pega/pega-source-destination-mapping.png)

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

プロファイルのオーディエンスメンバーシップが正常に更新されると、オーディエンスの識別子、名前およびステータスが Pega マーケティングオーディエンスメンバーシップデータストアに挿入されます。 以下に示すように、[!DNL Pega Customer Decision Hub] の顧客プロファイル Designerを使用して、メンバーシップデータがお客様に関連付けられます。

![&#x200B; 顧客プロファイル Designerを使用して、Adobe オーディエンスメンバーシップデータを顧客に関連付けることができる UI 画面の画像 &#x200B;](../../assets/catalog/personalization/pega/pega-profile-designer-associate.png)

次に示すように、オーディエンスメンバーシップデータは、次善のアクションの意思決定のために、Pega の次善のアクションのDesigner エンゲージメントポリシーで使用されます。

![Pega の次善アクションDesignerのエンゲージメントポリシーの条件としてオーディエンスメンバーシップフィールドを追加できる UI 画面の画像 &#x200B;](../../assets/catalog/personalization/pega/pega-profile-designer-engagement.png)

以下に示すように、顧客オーディエンスメンバーシップデータフィールドがアダプティブモデルの予測因子として追加されます。

![&#x200B; 予測スタジオを使用して、オーディエンスメンバーシップフィールドをアダプティブモデルの述語として追加できる UI 画面の画像 &#x200B;](../../assets/catalog/personalization/pega/pega-profile-designer-adaptivemodel.png)

## その他のリソース {#additional-resources}

詳しくは、次の [!DNL Pega] ドキュメントを参照してください。

* [OAuth 2.0 クライアント登録の設定 &#x200B;](https://docs.pega.com/bundle/platform/page/platform/security/configure-oauth-2-client-registration.html)
* [&#x200B; データフローのリアルタイム実行の作成 &#x200B;](https://docs.pega.com/bundle/platform/page/platform/decision-management/data-flow-run-real-time-create.html)
* [&#x200B; 顧客プロファイルDesignerでの顧客レコードの管理 &#x200B;](https://docs.pega.com/bundle/customer-decision-hub/page/customer-decision-hub/implement/profile-designer-data-management.html)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
