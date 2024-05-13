---
title: 1 回限りの顧客価値をライフタイム価値に進化
description: 特定の顧客の属性、行動、過去の購入に基づいて、最適な補完的な製品やサービスを提供するパーソナライズされたキャンペーンを作成する方法を説明します。
feature: Use Cases
exl-id: 45f72b5e-a63b-44ac-a186-28bac9cdd442
source-git-commit: 2f1008791a35f33a0379cba14b90334aebf83187
workflow-type: tm+mt
source-wordcount: '3179'
ht-degree: 2%

---

# 1 回限りの顧客価値をライフタイム価値に進化

>[!IMPORTANT]
> 
>* このページでは、記載されているユースケースを実現するための、Real-Time CDPおよびAdobe Journey Optimizerの実装例を示します。 ページに記載されている数値、認定条件、その他のフィールドは、規範的な数値としてではなく、ガイドとして使用してください。
>* このユースケースを完了するには、Real-Time CDPおよびAdobe Journey Optimizerのライセンスを取得している必要があります。 詳しくは、を参照してください。 [前提条件と計画セクション](#prerequisites-and-planning) さらに下。

生涯価値のユースケースに 1 回限りの顧客価値を実装して、ブランドエンゲージメントとブランドロイヤルティを推進します。 によって強化されたExperience Platformの力を使用して、複数のチャネルまたはジャーニーで接続されたカスタマーエクスペリエンスを構築します。 [Real-Time CDP](/help/rtcdp/home.md) および [Journey Optimizer](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/ajo-home).

ターゲットとしているペルソナは、過去 3 か月以内に購入を行った、プロパティへのまれな訪問者です。

プロパティを訪問し、提供する製品やサービスを散発的に購入する顧客を考えてみましょう。 これらの顧客にアピールするパーソナライズされたキャンペーンを作成して、ブランドが 1 回限りの価値ではなく、長期的な価値を提供できるようにする場合があります。 次の方法を学びます。

* データの収集と管理
* オーディエンスを作成
* Adobe Journey Optimizerでこれらのオーディエンスをターゲットにし、Real-Time CDPでアクティブ化するジャーニーを作成します。

![ステップバイステップ ワンタイム値をライフタイム値に進化する高レベルの視覚的な概要。](../evolve-one-time-value-lifetime-value/images/diagram-business-use-case.png){width="500" zoomable="yes"}

## 前提条件と計画 {#prerequisites-and-planning}

社内で、ブランドロイヤルティを高めるビジネス目標と目標を定義していることを考慮します。 これは、顧客エンゲージメントとロイヤルティを高めるためにユースケースを実行することを意味します。

そのために必要な技術は、2 つのExperience Platformアプリで構成されています [Real-Time CDP](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html?lang=ja) および [Adobe Journey Optimizer](https://experienceleague.adobe.com/docs/journey-optimizer/using/get-started/get-started.html?lang=ja). 以下に、ユースケースの実装時に使用する 2 つのアプリの様々な機能と UI 要素を示します。

>[!TIP]
>
>これらすべての領域に必要な[属性ベースのアクセス制御権限](/help/access-control/abac/end-to-end-guide.md)があることを確認するか、必要な権限の付与をシステム管理者に依頼してください。

* [[!DNL Adobe Real-Time Customer Data Platform (Real-Time CDP)]](https://experienceleague.adobe.com/docs/platform-learn/tutorials/rtcdp/understanding-the-real-time-customer-data-platform.html)：複数のデータソースのデータを統合して、キャンペーンに燃料を供給します。 その後、このデータを使用して、キャンペーンオーディエンスを作成し、メールおよび web プロモタイルで使用されるパーソナライズされたデータ要素（名前やアカウントに関連する情報など）を表示します。 最後に、Real-Time CDPを使用して、有料メディアの宛先に対してオーディエンスをアクティブ化することもできます。
   * [スキーマ](/help/xdm/home.md)
   * [プロファイル](/help/profile/home.md)
   * [データセット](/help/catalog/datasets/overview.md)
   * [オーディエンス](/help/segmentation/home.md)
   * [宛先](/help/destinations/home.md)
* [[!DNL Adobe Journey Optimizer]](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/journey.html)：ジャーニーを設計し、トリガーを設定して、訪問者に対処する適切なメッセージを作成します。
   * [イベントまたはオーディエンスのトリガー](https://experienceleague.adobe.com/docs/journey-optimizer/using/offer-decisioning/collect-event-data/data-collection.html?lang=ja)
   * [オーディエンスとイベント](https://experienceleague.adobe.com/docs/journey-optimizer/using/audiences-profiles-identities/audiences/about-audiences.html?lang=ja)
   * [ジャーニー](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/journey.html)

## Real-Time CDPとJourney Optimizerのアーキテクチャ

以下は、Real-Time CDPとJourney Optimizerの様々なコンポーネントのアーキテクチャの概要です。 次の図は、このページで説明するユースケースを達成するために、データ収集からジャーニーやキャンペーンを通じてアクティブ化された宛先に至るまでの、2 つのExperience Platformアプリを介したデータのフローを示しています。

![アーキテクチャの大まかなビジュアルの概要。](/help/rtcdp/use-case-guides/evolve-one-time-value-lifetime-value/images/architecture-diagram.png){width="600" zoomable="yes"}

## ユースケースの達成方法：概要 {#achieve-the-use-case-high-level}

以下は、ワークフローの概要、ジャーニーワークフローとアクティベーションワークフローの組み合わせです。

以下に示すサンプルワークフローでは、特定の条件を満たす顧客を探し、それらの顧客を Web サイトまたはアプリに戻す誘惑をします。 プロパティに対するアクティビティが制限されるのではなく、より反復的な方法で返されるジャーニーに設定することを検討しています。 訪問者をプロパティに戻そうとしているとします。その後、訪問者が戻ったら、ジャーニーにエントリして、サイトで繰り返し購入を行います。 ここで設定するキャンペーンの上限は、顧客とのエンゲージメントが月に 1 回です。

まず、値の高い顧客と頻度の低い顧客のオーディエンスにメッセージを送信します。 次に、このメッセージが過去 30 日以内に送信されたかどうかを確認します。 まだエントリしていない場合は、新しい購読プログラムなどに関するジャーニーにエントリできます。 その後、数日間待つことができます（この例では 7 日間）。 この時間が経過しても、メッセージに記載されているサブスクリプションを購入していない場合は、宛先を介して有料のメディア広告を配信できます。 サブスクリプションを購入した場合は、注文確認ジャーニーにエントリしてもらい、ユースケースを完了することができます。

>[!IMPORTANT]
>
>このページで後述するように、を使用します。 [スキーマ内の専用の同意フィールドグループ](#customer-attributes-schema) および基準 [同意ポリシーの実装](#privacy-consent)を使用すると、すべてのアクションとワークフローが、プライバシーと同意を優先する方法で実装されます。

>[!BEGINSHADEBOX]

![ステップバイステップ ワンタイム値をライフタイム値に進化する高レベルの視覚的な概要。](../evolve-one-time-value-lifetime-value/images/step-by-step.png){width="600" zoomable="yes"}

1. スキーマとデータセットを作成し、次の目的でマークします [!UICONTROL Profile].
2. データは、Web SDK、Mobile Edge SDK または API を介して収集され、Experience Platformに統合されます。 Analytics Data Connector も利用できますが、ジャーニー遅延が発生する可能性があります。
3. プロファイルをReal-Time CDPに読み込み、責任ある使用を確保するためにガバナンスポリシーを構築します。
4. プロファイルのリストから焦点を当てたオーディエンスを作成し、価値の高い顧客と頻度の低い顧客を確認します。
5. で 2 つのジャーニーを作成します。 [!DNL Adobe Journey Optimizer]1 つは新しいサブスクリプションプログラムについてユーザーにメッセージを送信するためのもので、もう 1 つは後で購入を確認するようメッセージを送信するためのものです。
6. 必要に応じて、サブスクリプションを購入していない顧客のオーディエンスを、目的の有料メディアの宛先に対してアクティブ化します。

>[!ENDSHADEBOX]

## ユースケースの達成方法 {#achieve-use-case-instruction}

上記の概要の各手順を完了するには、以下の節を参照してください。この節には、詳細情報や詳細な手順へのリンクが記載されています。

### 使用する UI 機能と要素 {#ui-functionality-and-elements}

このユースケースを実装する手順を完了する際は、このドキュメントの最初に示したReal-Time CDP、Adobe Journey Optimizer機能、UI 要素を使用します。 これらのすべての領域に対して必要な属性ベースのアクセス制御権限があることを確認するか、システム管理者に必要な権限の付与を依頼してください。

### スキーマデザインの作成とフィールドグループの指定 {#schema-design}

エクスペリエンスデータモデル（XDM）リソースは、 [!UICONTROL スキーマ] ワークスペース： [!DNL Adobe Experience Platform]. が提供するコアリソースを表示および調査できます。 [!DNL Adobe] （例： [!UICONTROL フィールドグループ]）を選択して、組織のカスタムリソースおよびスキーマを作成します。

の作成について詳しくは、 [スキーマ](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html?lang=ja)、を読み取ります [スキーマの作成チュートリアル。](/help/xdm/tutorials/create-schema-ui.md)

このサンプル実装では、1 回限りの値をライフタイム値に進化させるユースケースに使用できるスキーマデザインがいくつかあります。 各スキーマには、設定が必要な特定の必須フィールドと、提案される一部のフィールドが含まれています。

Adobeでは、サンプル実装に基づいて、このユースケースを達成するために次の 3 つのスキーマを作成することをお勧めします。

* [顧客属性スキーマ](#customer-attributes-schema) （プロファイルスキーマ）
* [顧客デジタルトランザクションスキーマ](#customer-digital-transactions-schema) （エクスペリエンスイベントスキーマ）
* [顧客のオフライントランザクションスキーマ](#customer-offline-transactions-schema) （エクスペリエンスイベントスキーマ）

#### 顧客属性スキーマ {#customer-attributes-schema}

このスキーマを使用して、顧客情報を構成するプロファイルデータを構造化および参照します。 このデータは通常、に取り込まれます。 [!DNL Adobe Experience Platform] crm または同様のシステムを通じて、パーソナライゼーション、マーケティングの同意、強化されたセグメント化機能に使用される顧客の詳細を参照するために必要です。

![フィールドグループがハイライト表示された顧客属性スキーマ](/help/rtcdp/use-case-guides/evolve-one-time-value-lifetime-value/images/customer-attributes-schema.png)

顧客属性スキーマは、 [!UICONTROL XDM 個人プロファイル] 以下のフィールドグループを含むクラス。

+++人口統計の詳細（フィールドグループ）

[人口統計の詳細](/help/xdm/field-groups/profile/demographic-details.md) は、XDM 個人プロファイルクラスの標準スキーマフィールドグループです。 フィールドグループは、ルートレベルの person オブジェクトを提供し、そのサブフィールドは個々のユーザーに関する情報を記述します。

+++

+++個人の連絡先の詳細（フィールドグループ）

[個人の連絡先の詳細](/help/xdm/field-groups/profile/personal-contact-details.md) は、XDM 個人プロファイル クラスの標準スキーマフィールドグループで、個々のユーザーの連絡先情報を記述します。

+++

+++外部ソースシステム監査詳細（フィールドグループ）

[外部ソースシステム監査属性](/help/xdm/data-types/external-source-system-audit-attributes.md) は、外部ソースシステムに関する監査詳細をキャプチャする標準のエクスペリエンスデータモデル（XDM）データタイプです。

+++

+++同意および環境設定フィールドグループ（フィールドグループ）

[同意および環境設定](/help/xdm/field-groups/profile/consents.md) フィールドグループは、同意と環境設定に関する情報を取り込むために、単一のオブジェクトタイプのフィールド「同意」を提供します。

+++

#### 顧客デジタルトランザクションスキーマ {#customer-digital-transactions-schema}

このスキーマは、Web サイトまたは他の関連するデジタルプラットフォームで発生する顧客アクティビティを構成するイベントデータを構造化し、参照するために使用されます。 このデータは通常、に取り込まれます。 [!DNL Adobe Experience Platform] 経由 [Web SDK](/help/web-sdk/home.md) は、ジャーニーのトリガーに使用される様々な参照およびコンバージョンイベント、詳細なオンライン顧客分析、強化されたセグメント化機能を参照する際に必要です。

![フィールドグループがハイライト表示された顧客デジタルトランザクションスキーマ](/help/rtcdp/use-case-guides/evolve-one-time-value-lifetime-value/images/customer-digital-transactions-schema.png)

顧客デジタルトランザクションスキーマは、 [!UICONTROL XDM ExperienceEvent] 以下のフィールドグループを含むクラス。

+++Adobe Experience Platform Web SDK ExperienceEvent （フィールドグループ）

| フィールド | 要件 |
| --- | --- |
| `device.model` | 候補 |
| `environment.browserDetails.userAgent` | 候補 |

+++

+++Web 詳細（フィールドグループ）

[Web の詳細](/help/xdm/field-groups/event/web-details.md) は、XDM ExperienceEvent クラスの標準スキーマフィールドグループで、インタラクション、ページの詳細、リファラーなどの、web の詳細イベントに関する情報を記述するために使用されます。

+++

+++消費者エクスペリエンスイベント（フィールドグループ）

このフィールドグループには、web プロパティに対するユーザーのアクション（購入やイベントの参照など）に関する様々な情報が含まれます。

| フィールド | 要件 |
| --- | --- |
| `commerce.cart.cartID` | 候補 |
| `commerce.cart.cartSource` | 候補 |
| `commerce.cartAbandons.id` | 候補 |
| `commerce.cartAbandons.value` | 候補 |
| `commerce.order.orderType` | 候補 |
| `commerce.order.payments.paymentAmount` | 候補 |
| `commerce.order.payments.paymentType` | 候補 |
| `commerce.order.payments.transactionID` | 候補 |
| `commerce.order.priceTotal` | 候補 |
| `commerce.order.purchaseID` | 候補 |
| `commerce.productListAdds.id` | 候補 |
| `commerce.productListAdds.value` | 候補 |
| `commerce.productListOpens.id` | 候補 |
| `commerce.productListOpens.value` | 候補 |
| `commerce.productListRemoval.id` | 候補 |
| `commerce.productListRemoval.value` | 候補 |
| `commerce.productListViews.id` | 候補 |
| `commerce.productListViews.value` | 候補 |
| `commerce.productViews.id` | 候補 |
| `commerce.productViews.value` | 候補 |
| `commerce.purchases.id` | 候補 |
| `commerce.purchases.value` | 候補 |
| `marketing.campaignGroup` | 候補 |
| `marketing.campaignName` | 候補 |
| `marketing.trackingCode` | 候補 |
| `productListItems.name` | 候補 |
| `productListItems.priceTotal` | 候補 |
| `productListItems.product` | 候補 |
| `productListItems.quantity` | 候補 |

+++

+++エンドユーザー ID 詳細（フィールドグループ）

この [エンドユーザー ID 詳細](/help/xdm/field-groups/event/enduserids.md) フィールドグループには、訪問時にサイトで認証されているかどうか、ユーザーの id に関する情報など、ユーザーに関する様々な情報が含まれています。

+++

+++外部ソースシステム監査詳細（フィールドグループ）

外部ソースシステム監査属性は、外部ソースシステムに関する監査詳細を取得する標準の Experience Data Model （XDM）データタイプです。

+++

#### 顧客のオフライントランザクションスキーマ {#customer-offline-transactions-schema}

このスキーマは、web サイト外のプラットフォームで発生する顧客アクティビティを構成するイベントデータを構造化し、参照するために使用されます。 このデータは通常、に取り込まれます。 [!DNL Adobe Experience Platform] POS （または同様のシステム）から、ほとんどの場合、API 接続を介して Platform にストリーミングされます。 詳細を読む [バッチ取り込み](/help/ingestion/batch-ingestion/getting-started.md). その目的は、ジャーニーのトリガー、オンラインとオフラインの詳細な顧客分析、強化されたセグメント化機能に使用される様々なオフラインコンバージョンイベントを参照することです。

![フィールドグループがハイライト表示された顧客のオフライントランザクションスキーマ](/help/rtcdp/use-case-guides/evolve-one-time-value-lifetime-value/images/customer-offline-transactions-schema.png)

顧客のオフライントランザクションスキーマは、 [!UICONTROL XDM ExperienceEvent] 以下のフィールドグループを含むクラス。

+++Commerceの詳細（フィールドグループ）

[Commerceの詳細](/help/xdm/field-groups/event/commerce-details.md) は、の標準スキーマフィールドグループです。 [!DNL XDM ExperienceEvent] 商品情報（SKU、名前、数量）や標準の買い物かご操作（注文、チェックアウト、放棄）などのコマースデータを記述するために使用されるクラス。

+++

+++個人の連絡先の詳細（フィールドグループ）

[[!UICONTROL 個人の連絡先の詳細]](/help/xdm/field-groups/profile/personal-contact-details.md) は、の標準スキーマフィールドグループです。 [!DNL XDM Individual Profile] クラス。個人の連絡先情報を記述します。

+++

+++外部ソースシステム監査詳細（フィールドグループ）

外部ソースシステム監査属性は、外部ソースシステムに関する監査詳細を取得する標準の Experience Data Model （XDM）データタイプです。

+++

#### Adobe web コネクタスキーマ {#adobe-web-connector-schema}

>[!NOTE]
>
>を使用する場合、これはオプションの実装です。 [!DNL Adobe Analytics Data Connector].

このスキーマは、Web サイトまたは他の関連するデジタルプラットフォームで発生する顧客アクティビティを構成するイベントデータを構造化し、参照するために使用されます。 このスキーマは、顧客デジタルトランザクションスキーマに似ていますが、Web SDK がデータ収集用のオプションでない場合に、このスキーマが可能であるという点が異なります。 そのため、を利用する際にこのスキーマを使用できます [!DNL Adobe Analytics Data Connector] オンラインデータをに送信する [!DNL Adobe Experience Platform] プライマリまたはセカンダリデータストリームとして。

![フィールドグループがハイライト表示されたAdobe web コネクタスキーマ](/help/rtcdp/use-case-guides/evolve-one-time-value-lifetime-value/images/adobe-web-schema.png)

この [!DNL Adobe] web コネクタスキーマはで表されます [!UICONTROL XDM ExperienceEvent] 以下のフィールドグループを含むクラス。

+++Adobe Analytics ExperienceEvent テンプレート（フィールドグループ）

[[!UICONTROL Adobe Analytics ExperienceEvent 完全拡張機能]](/help/xdm/field-groups/event/analytics-full-extension.md) は、Adobe Analyticsが収集する共通のメトリクスをキャプチャする、標準スキーマフィールドグループです。

+++

### スキーマからのデータセットの作成 {#dataset-from-schema}

データセットは、データグループのストレージと管理の構造です。 このサンプル実装の実行に使用される各スキーマには、1 つのデータセットがあります。

を作成する方法について詳しくは、 [データセット](/help/catalog/datasets/overview.md) スキーマから、を読み取ります。 [データセット UI ガイド](/help/catalog/datasets/user-guide.md).

>[!NOTE]
>
>スキーマを作成する手順と同様に、データセットをリアルタイム顧客プロファイルに含めることができるようにする必要があります。 リアルタイム顧客プロファイルで使用するデータセットを有効にする方法について詳しくは、 [スキーマの作成チュートリアル。](/help/xdm/tutorials/create-schema-ui.md#profile).

### プライバシー、同意、データガバナンス {#privacy-consent}

#### 同意ポリシー

>[!IMPORTANT]
>
>ブランドからの連絡を登録解除する機能を顧客に提供し、この選択を確実に行うことは、法的要件です。 適用される法律の詳細については、を参照してください [プライバシー規制の概要](https://experienceleague.adobe.com/docs/experience-platform/privacy/regulations/overview.html).

次の実装を検討してください [同意ポリシー](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/consent/overview.html) さらに、訪問者に連絡する前に同意を求めます。

* 次の場合 `consents.marketing.email.val = "Y"` その後、電子メールで送信できます
* 次の場合 `consents.marketing.sms.val = "Y"` その後、SMS を送信できる
* 次の場合 `consents.marketing.push.val = "Y"` その後、プッシュできます
* 次の場合 `consents.share.val = "Y"` 次に、広告可能

#### データガバナンスのラベルと適用

次の項目を追加し、適用することを検討してください [データガバナンスラベル](/help/data-governance/labels/overview.md):

* 個人のメールアドレスは、デバイスではなく特定の個人を識別したり、その個人と連絡を取ったりするために使用される、直接識別可能なデータとして利用されます。
   * `personalEmail.address = I1`

#### マーケティングポリシー

がありません [マーケティングポリシー](/help/data-governance/policies/overview.md) このユースケースの一部として作成するジャーニーで必要です。 ただし、必要に応じて次のポリシーを考慮できます。

* 機密データの制限
* オンサイト広告の制限
* 電子メールターゲティングの制限
* クロスサイトターゲティングを制限
* 直接識別可能なデータと匿名データの組み合わせを制限

### オーディエンスを作成 {#create-audiences}

このユースケースでは、2 つのオーディエンスを作成して、プロファイルストアのプロファイルのサブセットで共有される特定の属性や行動を定義し、マーケティング可能なユーザーグループを区別する必要があります。 オーディエンスは、Adobe Experience Platformで複数の方法で作成できます。

* オーディエンスの作成方法について詳しくは、 [Audience Service UI ガイド](https://experienceleague.adobe.com/docs/experience-platform/segmentation/ui/overview.html#create-audience).
* 作成方法については [オーディエンス](/help/segmentation/home.md)、を読み取ります [オーディエンス構成 UI ガイド](/help/segmentation/ui/audience-composition.md).
* Platform 派生セグメント定義を使用してオーディエンスを作成する方法について詳しくは、以下を参照してください [Audience Builder UI ガイド](/help/segmentation/ui/segment-builder.md).

特に、次の画像に示すように、ユースケースの異なる手順で 2 つのオーディエンスを作成し使用する必要があります。

![ハイライト表示されたオーディエンス](/help/rtcdp/use-case-guides/evolve-one-time-value-lifetime-value/images/audiences-highlighted-in-diagram.png){width="600" zoomable="yes"}

>[!BEGINTABS]

>[!TAB Adobe Journey Optimizerの対象読者]

この高価値かつ低頻度のオーディエンスには、新しい購読プログラムについて知らせるために、ジャーニーを通じて連絡するプロファイルが含まれています。 オーディエンスの詳細は次のとおりです。

* 説明：過去 3 か月間に合計 250 ドルを超える費用を費やしたプロファイル
* オーディエンスで必要なフィールドと条件：
   * イベント： `commerce.order.payments.paymentamount`
*集計合計：>= $250
   * EventType: `commerce.purchases`
* タイムスタンプ：今から 3 か月前


>[!TAB 有料メディアオーディエンス]

このオーディエンスは、過去 3 か月間に合計 250 ドルを超える費用を費やし、過去 7 日間に購入を行っていないプロファイルを含めるように作成されます。 オーディエンスの詳細は次のとおりです。

* 説明：過去 3 か月間に合計 250 ドル以上を費やし、過去 7 日間に購入していないプロファイル。
* 必要なフィールドと条件：
   * EventType: `journey.feedback`
      * オペランド：= true
   * イベント：`experience.journeyOrchestration.stepEvents.nodeName`
      * オペランド : = JourneyStepEventTracker – 購読は購入されていません
      * タイムスタンプ：過去 7 日間
   * EventType が次の値ではありません： `commerce.purchases`
      * タイムスタンプ：今から 7 日前
   * イベント：SKU
      * 値：= `subscription`

>[!ENDTABS]

### Adobe Journey Optimizerでのジャーニー設定 {#journey-setup}

>[!NOTE]
>
>[!DNL Adobe Journey Optimizer] 図に示されているすべての内容が含まれるわけではありません。 すべて [有料メディア広告](/help/destinations/catalog/social/overview.md) は、に作成されます。 [!UICONTROL 宛先] [workspace](/help/destinations/ui/destinations-workspace.md).

[[!DNL Adobe Journey Optimizer]](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/journey.html) は、顧客とのつながり、コンテキスト、パーソナライズされたエクスペリエンスを提供するのに役立ちます。 カスタマージャーニーは、顧客がブランドとやり取りするプロセス全体です。 各ユースケースのジャーニーには、特定の情報が必要です。

このユースケースを実現するには、次の 2 つの異なるジャーニーを作成する必要があります。

* ライフタイムジャーニー：高価値で低頻度の顧客に送信するメッセージが含まれます
* 電話に応答してサブスクリプションを購入するユーザーの注文確認ジャーニー。

![ハイライト表示されたジャーニー](/help/rtcdp/use-case-guides/evolve-one-time-value-lifetime-value/images/journeys-highlighted-in-diagram.png){width="600" zoomable="yes"}

以下に、各ジャーニーブランチに必要な正確なデータを示します。

>[!BEGINTABS]

>[!TAB 生涯ジャーニー]

ライフタイムジャーニーは、過去 30 日以内にターゲットとならなかった高価値および低頻度の顧客のオーディエンスに対応します。 これらの顧客にメッセージが表示され、7 日間経過してもまだ購入されない場合は、有料メディア広告を表示できるオーディエンスに未購入者を含めることができます。 購入した場合は、注文確認ジャーニー（詳細は別のタブで説明）で購入者を設定できます。

![ライフタイムジャーニーの概要レベルのビジュアル概要。](/help/rtcdp/use-case-guides/evolve-one-time-value-lifetime-value/images/lifetime-journey.png "ライフタイムジャーニーに 1 回限りの値を提供する、視覚的な概要の説明。"){width="600" zoomable="yes"}

+++詳細なジャーニーロジック

上記のジャーニーは、次のロジックに従います。

1. オーディエンスを読み取り：を使用 [オーディエンスを読み取りアクティビティ](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/about-journey-building/read-audience.html?lang=en) 上記のオーディエンスの節で作成した最初のオーディエンスの場合。

2. 条件 – 優先チャネル：を使用します [条件アクティビティ](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/about-journey-building/condition-activity.html) メール、SMS、プッシュ通知などを介して顧客に連絡する方法を決定します。 3 つのアクションアクティビティを使用して、3 つのブランチを作成します。

3. 待機：を使用 [待機アクティビティ](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/about-journey-building/read-audience.html) 購入をリッスンするまで待ちます。

4. 条件 – 過去 7 日間に購入したサブスクリプション :「条件」アクティビティを使用して、過去 7 日間に製品が購入されたかどうかをリッスンします。

5. JourneyStepEventTracker - サブスクリプションが購入されていません：を使用します [カスタムアクション](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/about-journey-building/using-custom-actions.html) メッセージを受信したにもかかわらず、まだサブスクリプションを購入していない訪問者の場合。 ジャーニーの最後のカスタム条件の一部として、を作成します `journey.feedback` イベントを有効化し、に基づいてデータセットに追加します [!UICONTROL ジャーニーステップイベント] スキーマ。 このイベントを使用して、サブスクリプションを購入していないオーディエンスをセグメント化し、有料メディア広告を介してターゲットにすることができます。

+++

>[!TAB 注文確認ジャーニー]

注文確認ジャーニーでは、購入が web サイトまたはモバイルアプリを通じて行われたかどうかに焦点を当てています。 顧客が会社のサブスクリプションなどの購入を正常に完了したら、注文確認ジャーニーにそれらを設定できます。

![顧客注文確認ジャーニーの大まかなビジュアル概要。](/help/rtcdp/use-case-guides/evolve-one-time-value-lifetime-value/images/order-confirmation-journey.png "顧客注文確認ジャーニーの大まかなビジュアル概要。"){width="600" zoomable="yes"}

+++ジャーニーロジック

確認ジャーニーで、以下の推奨イベント、フィールド、アクションを使用します。

* ジャーニーは、オンライン購入イベントによってトリガーされます
   * スキーマ：顧客デジタルトランザクション
   * フィールド :
      * `EventType`
   * 条件：
      * `EventType = commerce.purchases`
      * フィールド :
         * `Commerce.purchases.id`
         * `Commerce.purchases.value`
         * `eventType`
         * `identityMap.authenticatedState`
         * `identityMap.id`
         * `identityMap.primary`
         * `productListItems.SKU`
         * `productListItems.currencyCode`
         * `productListItems.name`
         * `productListItems.priceTotal`
         * `productListItems.product`
         * `productListItems.productImageUrl`
         * `productListItems.quantity`
         * `timestamp`
         * `endUserIDs._experience.emailid.authenticatedState`
         * `endUserIDs._experience.emailid.id`
         * `endUserIDs._experience.emailid.namespace.code`
         * `_id`

+++

+++キージャーニーロジック

* ジャーニーエントリロジック
   * 注文イベント

* 条件
   * ターゲットチャネルを選択します（リーチを広げるために 1 つまたは複数のチャネルを選択できます）。
      * 注文確認は本質的に機能していると見なされるので、同意チェックは通常不要です。
      * メール
      * プッシュ
      * SMS

   * チャネルコンテンツのパーソナライゼーション
      * 注文の詳細情報を表示し、テーブル形式を使用して商品のリストを表示できます。

+++

>[!ENDTABS]

でのジャーニーの作成について詳しくは、こちらを参照してください [!DNL Adobe Journey Optimizer]、を読み取ります [ジャーニーの概要](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/journey.html) ガイド。

### 有料メディア広告を表示する宛先の設定 {#paid-media-ads}

一部のユーザーは、新しいプログラムについてメッセージを送信した後も、サブスクリプションを購入していない可能性があります。 日数を待った後（このユースケースでは 7 日）、それらのユーザーに有料のメディア広告を表示して、サブスクリプションの購入を促すことができます。

有料メディア広告には、Real-Time CDPの宛先フレームワークを使用します。 多数ある広告宛先の 1 つを選択して、顧客に有料メディア広告を表示し、顧客の有料メディアオーディエンスをアクティブ化します [作成日時](#create-audiences) を任意の宛先に設定します。 利用可能なの概要を確認する [広告](/help/destinations/catalog/advertising/overview.md) および [ソーシャル](/help/destinations/catalog/social/overview.md) 宛先。

宛先へのデータをアクティブ化する方法を説明します（例：） [Trade Desk](/help/destinations/catalog/advertising/tradedesk.md) または [Google カスタマーマッチ](/help/destinations/catalog/advertising/google-customer-match.md)）を参照する場合は、以下のドキュメントを参照してください。

* [新しい宛先接続の作成](/help/destinations/ui/connect-destination.md)
* [ストリーミングオーディエンスの書き出し先に対するオーディエンスデータの有効化](/help/destinations/ui/activate-segment-streaming-destinations.md)

## 次の手順 {#next-steps}

ジャーニーで低頻度ユーザーと高価値ユーザーを設定し、そのサブセットに有料メディア広告を表示することで、一部のユーザーを 1 回限りの価値から生涯価値の顧客に変え、ブランドロイヤルティと顧客エンゲージメントの指標を改善できればと考えています。

次に、Real-Time CDPでサポートされている他のユースケース（例えば、以下のもの）を確認できます [インテリジェントに顧客をリエンゲージ](/help/rtcdp/use-case-guides/intelligent-re-engagement/intelligent-re-engagement.md) または [認証されていないユーザーに対するパーソナライズされたコンテンツの表示](/help/rtcdp/partner-data/onsite-personalization.md) web プロパティで。
