---
title: 1 回限りの顧客価値をライフタイム価値に進化
description: 特定の顧客の属性、行動、過去の購入に基づいて、最適な補完的な製品やサービスを提供するパーソナライズされたキャンペーンを作成する方法を説明します。
feature: Use Cases
exl-id: 45f72b5e-a63b-44ac-a186-28bac9cdd442
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '3181'
ht-degree: 2%

---

# 1 回限りの顧客価値をライフタイム価値に進化

>[!IMPORTANT]
> 
>* このページでは、記載されているユースケースを実現するための、Real-Time CDPおよびAdobe Journey Optimizerの実装例を示します。 ページに記載されている数値、認定条件、その他のフィールドは、規範的な数値としてではなく、ガイドとして使用してください。
>* このユースケースを完了するには、Real-Time CDPおよびAdobe Journey Optimizerのライセンスを取得している必要があります。 詳しくは、以下の [ 前提条件と計画の節 ](#prerequisites-and-planning) を参照してください。

生涯価値のユースケースに 1 回限りの顧客価値を実装して、ブランドエンゲージメントとブランドロイヤルティを推進します。 [Real-Time CDP](/help/rtcdp/home.md) および [Journey Optimizer&rbrace; で拡張されたExperience Platformの機能を使用して、複数のチャネルまたはジャーニーで接続されたカスタマーエクスペリエンスを構築し ](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/ajo-home) す。

ターゲットとしているペルソナは、過去 3 か月以内に購入を行った、プロパティへのまれな訪問者です。

プロパティを訪問し、提供する製品やサービスを散発的に購入する顧客を考えてみましょう。 これらの顧客にアピールするパーソナライズされたキャンペーンを作成して、ブランドが 1 回限りの価値ではなく、長期的な価値を提供できるようにする場合があります。 次の方法を学びます。

* データの収集と管理
* オーディエンスを作成
* Adobe Journey Optimizerでこれらのオーディエンスをターゲットにし、Real-Time CDPでアクティブ化するジャーニーを作成します。

![ ステップバイステップの 1 回限りの値をライフタイム値に進化させる高レベルの視覚的な概要 ](../evolve-one-time-value-lifetime-value/images/diagram-business-use-case.png){zoomable="yes"}

## 前提条件と計画 {#prerequisites-and-planning}

社内で、ブランドロイヤルティを高めるビジネス目標と目標を定義していることを考慮します。 これは、顧客エンゲージメントとロイヤルティを高めるためにユースケースを実行することを意味します。

これを実現するために必要なテクノロジは、2 つのExperience Platform アプリ [Real-Time CDP](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html?lang=ja) と [Adobe Journey Optimizer](https://experienceleague.adobe.com/docs/journey-optimizer/using/get-started/get-started.html?lang=ja) で構成されています。 以下に、ユースケースの実装時に使用する 2 つのアプリの様々な機能と UI 要素を示します。

>[!TIP]
>
>これらすべての領域に必要な[属性ベースのアクセス制御権限](/help/access-control/abac/end-to-end-guide.md)があることを確認するか、必要な権限の付与をシステム管理者に依頼してください。

* [[!DNL Adobe Real-Time Customer Data Platform (Real-Time CDP)]](https://experienceleague.adobe.com/docs/platform-learn/tutorials/rtcdp/understanding-the-real-time-customer-data-platform.html?lang=ja)：複数のデータソースのデータを統合して、キャンペーンに燃料を供給します。 その後、このデータを使用して、キャンペーンオーディエンスを作成し、メールおよび web プロモタイルで使用されるパーソナライズされたデータ要素（名前やアカウントに関連する情報など）を表示します。 最後に、Real-Time CDPを使用して、有料メディアの宛先に対してオーディエンスをアクティブ化することもできます。
   * [スキーマ](/help/xdm/home.md)
   * [プロファイル](/help/profile/home.md)
   * [データセット](/help/catalog/datasets/overview.md)
   * [オーディエンス](/help/segmentation/home.md)
   * [宛先](/help/destinations/home.md)
* [[!DNL Adobe Journey Optimizer]](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/journey.html?lang=ja)：ジャーニーを設計し、トリガーを設定して、訪問者に対処する適切なメッセージを作成します。
   * [ イベントまたはオーディエンスのトリガー](https://experienceleague.adobe.com/docs/journey-optimizer/using/offer-decisioning/collect-event-data/data-collection.html?lang=ja)
   * [ オーディエンスとイベント ](https://experienceleague.adobe.com/docs/journey-optimizer/using/audiences-profiles-identities/audiences/about-audiences.html?lang=ja)
   * [ジャーニー](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/journey.html?lang=ja)

## Real-Time CDPとJourney Optimizerのアーキテクチャ

以下は、Real-Time CDPとJourney Optimizerの様々なコンポーネントのアーキテクチャの概要です。 次の図は、このページで説明するユースケースを達成するために、データ収集からジャーニーやキャンペーンを通じて宛先に対してアクティブ化される時点までの、2 つのExperience Platform アプリを介したデータのフローを示しています。

![ アーキテクチャの全体的なビジュアルの概要。](/help/rtcdp/use-case-guides/evolve-one-time-value-lifetime-value/images/architecture-diagram.png){zoomable="yes"}

## ユースケースの達成方法：概要 {#achieve-the-use-case-high-level}

以下は、ワークフローの概要、ジャーニーワークフローとアクティベーションワークフローの組み合わせです。

以下に示すサンプルワークフローでは、特定の条件を満たす顧客を探し、それらの顧客を Web サイトまたはアプリに戻す誘惑をします。 プロパティに対するアクティビティが制限されるのではなく、より反復的な方法で返されるジャーニーに設定することを検討しています。 訪問者をプロパティに戻そうとしているとします。その後、訪問者が戻ったら、ジャーニーにエントリして、サイトで繰り返し購入を行います。 ここで設定するキャンペーンの上限は、顧客とのエンゲージメントが月に 1 回です。

まず、値の高い顧客と頻度の低い顧客のオーディエンスにメッセージを送信します。 次に、このメッセージが過去 30 日以内に送信されたかどうかを確認します。 まだエントリしていない場合は、新しい購読プログラムなどに関するジャーニーにエントリできます。 その後、数日間待つことができます（この例では 7 日間）。 この時間が経過しても、メッセージに記載されているサブスクリプションを購入していない場合は、宛先を介して有料のメディア広告を配信できます。 サブスクリプションを購入した場合は、注文確認ジャーニーにエントリしてもらい、ユースケースを完了することができます。

>[!IMPORTANT]
>
>このページで後述するように、スキーマに [ 専用の同意フィールドグループ ](#customer-attributes-schema) を含め、[ 同意ポリシーの実装 ](#privacy-consent) により、すべてのアクションとワークフローがプライバシーと同意ファーストの方法で実装されます。

>[!BEGINSHADEBOX]

![ ステップバイステップの 1 回限りの値をライフタイム値に進化させる高レベルの視覚的な概要 ](../evolve-one-time-value-lifetime-value/images/step-by-step.png){zoomable="yes"}

1. スキーマとデータセットを作成し、これらを [!UICONTROL &#x200B; プロファイル &#x200B;] 用にマークします。
2. データは、Web SDK、Mobile Edge SDK、API を使用して収集され、Experience Platformに統合されます。 Analytics Data Connector も利用できますが、ジャーニー遅延が発生する可能性があります。
3. プロファイルをReal-Time CDPに読み込み、責任ある使用を確保するためにガバナンスポリシーを構築します。
4. プロファイルのリストから焦点を当てたオーディエンスを作成し、価値の高い顧客と頻度の低い顧客を確認します。
5. [!DNL Adobe Journey Optimizer] で 2 つのジャーニーを作成します。1 つは新しい購読プログラムについてユーザーにメッセージを送信するため、もう 1 つは後で購入を確認するようメッセージを送信するためです。
6. 必要に応じて、サブスクリプションを購入していない顧客のオーディエンスを、目的の有料メディアの宛先に対してアクティブ化します。

>[!ENDSHADEBOX]

## ユースケースの達成方法 {#achieve-use-case-instruction}

上記の概要の各手順を完了するには、以下の節を参照してください。この節には、詳細情報や詳細な手順へのリンクが記載されています。

### 使用する UI 機能と要素 {#ui-functionality-and-elements}

このユースケースを実装する手順を完了する際は、このドキュメントの最初に示したReal-Time CDP、Adobe Journey Optimizer機能、UI 要素を使用します。 これらのすべての領域に対して必要な属性ベースのアクセス制御権限があることを確認するか、システム管理者に必要な権限の付与を依頼してください。

### スキーマデザインの作成とフィールドグループの指定 {#schema-design}

エクスペリエンスデータモデル（XDM）リソースは、[!DNL Adobe Experience Platform] の [!UICONTROL &#x200B; スキーマ &#x200B;] ワークスペースで管理されます。 [!DNL Adobe] が提供するコアリソース（例：[!UICONTROL &#x200B; フィールドグループ &#x200B;]）を表示および調査し、組織のカスタムリソースおよびスキーマを作成できます。

[ スキーマ ](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html?lang=ja) の作成について詳しくは、[ スキーマの作成チュートリアル ](/help/xdm/tutorials/create-schema-ui.md) を参照してください。

このサンプル実装では、1 回限りの値をライフタイム値に進化させるユースケースに使用できるスキーマデザインがいくつかあります。 各スキーマには、設定が必要な特定の必須フィールドと、提案される一部のフィールドが含まれています。

Adobeでは、サンプル実装に基づいて、このユースケースを達成するために次の 3 つのスキーマを作成することをお勧めします。

* [ 顧客属性スキーマ ](#customer-attributes-schema) （プロファイルスキーマ）
* [ 顧客デジタルトランザクションスキーマ ](#customer-digital-transactions-schema) （エクスペリエンスイベントスキーマ）
* [ 顧客オフライントランザクションスキーマ ](#customer-offline-transactions-schema) （エクスペリエンスイベントスキーマ）

#### 顧客属性スキーマ {#customer-attributes-schema}

このスキーマを使用して、顧客情報を構成するプロファイルデータを構造化および参照します。 このデータは、通常、CRM または同様のシステムを介して [!DNL Adobe Experience Platform] に取り込まれ、パーソナライゼーション、マーケティングの同意、強化されたセグメント化機能に使用される顧客の詳細を参照するために必要です。

![ フィールドグループがハイライト表示された顧客属性スキーマ ](/help/rtcdp/use-case-guides/evolve-one-time-value-lifetime-value/images/customer-attributes-schema.png)

顧客属性スキーマは、次のフィールドグループを含む [!UICONTROL XDM 個人プロファイル &#x200B;] クラスで表されます。

+++人口統計の詳細（フィールドグループ）

[ デモグラフィックの詳細 ](/help/xdm/field-groups/profile/demographic-details.md) は、XDM 個人プロファイル クラスの標準スキーマフィールドグループです。 フィールドグループは、ルートレベルの person オブジェクトを提供し、そのサブフィールドは個々のユーザーに関する情報を記述します。

+++

+++個人の連絡先の詳細（フィールドグループ）

[ 個人の連絡先の詳細 ](/help/xdm/field-groups/profile/personal-contact-details.md) は、XDM 個人プロファイル クラスの標準スキーマフィールドグループで、個人の連絡先情報を説明します。

+++

+++外部Source システム監査の詳細（フィールドグループ）

[ 外部Source システム監査属性 ](/help/xdm/data-types/external-source-system-audit-attributes.md) は、外部ソースシステムに関する監査詳細を取得する標準の Experience Data Model （XDM）データタイプです。

+++

+++同意および環境設定フィールドグループ（フィールドグループ）

[ 同意および環境設定 ](/help/xdm/field-groups/profile/consents.md) フィールドグループには、同意と環境設定の情報を取り込むために、単一のオブジェクトタイプのフィールドである同意が用意されています。

+++

#### 顧客デジタルトランザクションスキーマ {#customer-digital-transactions-schema}

このスキーマは、Web サイトまたは他の関連するデジタルプラットフォームで発生する顧客アクティビティを構成するイベントデータを構造化し、参照するために使用されます。 このデータは通常、[Web SDK](/help/web-sdk/home.md) を介して [!DNL Adobe Experience Platform] に取り込まれ、ジャーニーのトリガーに使用される様々な参照およびコンバージョンイベント、オンライン顧客分析の詳細、セグメント化機能の強化を参照するために必要です。

![ フィールドグループがハイライト表示された顧客デジタルトランザクションスキーマ ](/help/rtcdp/use-case-guides/evolve-one-time-value-lifetime-value/images/customer-digital-transactions-schema.png)

顧客のデジタルトランザクションスキーマは、次のフィールドグループを含む [!UICONTROL XDM ExperienceEvent] クラスで表されます。

+++Adobe Experience Platform Web SDK ExperienceEvent （フィールドグループ）

| フィールド | 要件 |
| --- | --- |
| `device.model` | 候補 |
| `environment.browserDetails.userAgent` | 候補 |

+++

+++Web 詳細（フィールドグループ）

[Web の詳細 ](/help/xdm/field-groups/event/web-details.md) は、XDM ExperienceEvent クラスの標準スキーマフィールドグループで、インタラクション、ページの詳細、リファラーなどの web の詳細イベントに関する情報を記述するために使用されます。

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

[ エンドユーザー ID 詳細 ](/help/xdm/field-groups/event/enduserids.md) フィールドグループには、訪問時にサイトで認証されているかどうか、ユーザーの ID に関する情報など、ユーザーに関する様々な情報が含まれています。

+++

+++外部Source システム監査の詳細（フィールドグループ）

外部Source システム監査属性は、外部ソースシステムに関する監査詳細を取得する標準の Experience Data Model （XDM）データタイプです。

+++

#### 顧客のオフライントランザクションスキーマ {#customer-offline-transactions-schema}

このスキーマは、web サイト外のプラットフォームで発生する顧客アクティビティを構成するイベントデータを構造化し、参照するために使用されます。 このデータは、通常、POS （または同様のシステム）から [!DNL Adobe Experience Platform] に取り込まれ、ほとんどの場合、API 接続を介してExperience Platformにストリーミングされます。 [ バッチ取り込み ](/help/ingestion/batch-ingestion/getting-started.md) を参照してください。 その目的は、ジャーニーのトリガー、オンラインとオフラインの詳細な顧客分析、強化されたセグメント化機能に使用される様々なオフラインコンバージョンイベントを参照することです。

![ フィールドグループがハイライト表示された顧客のオフライントランザクションスキーマ ](/help/rtcdp/use-case-guides/evolve-one-time-value-lifetime-value/images/customer-offline-transactions-schema.png)

顧客のオフライントランザクションスキーマは、次のフィールドグループを含む [!UICONTROL XDM ExperienceEvent] クラスによって表されます。

+++Commerceの詳細（フィールドグループ）

[Commerceの詳細 ](/help/xdm/field-groups/event/commerce-details.md) は、商品 [!DNL XDM ExperienceEvent] 報（SKU、名前、数量）や標準の買い物かご操作（注文、チェックアウト、放棄）などのコマースデータを表すために使用される、商品クラスの標準スキーマフィールドグループです。

+++

+++個人の連絡先の詳細（フィールドグループ）

[[!UICONTROL &#x200B; 個人の連絡先の詳細 &#x200B;]](/help/xdm/field-groups/profile/personal-contact-details.md) は、[!DNL XDM Individual Profile] クラスの標準スキーマフィールドグループで、個人の連絡先情報を説明します。

+++

+++外部Source システム監査の詳細（フィールドグループ）

外部Source システム監査属性は、外部ソースシステムに関する監査詳細を取得する標準の Experience Data Model （XDM）データタイプです。

+++

#### Adobe web コネクタスキーマ {#adobe-web-connector-schema}

>[!NOTE]
>
>[!DNL Adobe Analytics Data Connector] を使用している場合、これはオプションの実装です。

このスキーマは、Web サイトまたは他の関連するデジタルプラットフォームで発生する顧客アクティビティを構成するイベントデータを構造化し、参照するために使用されます。 このスキーマは、Customer Digital Transactions スキーマと似ていますが、Web SDKがデータ収集のオプションでない場合に使用できる点が異なります。 そのため、[!DNL Adobe Analytics Data Connector] を利用してオンラインデータを [!DNL Adobe Experience Platform] にプライマリまたはセカンダリデータストリームとして送信する場合に、このスキーマを使用できます。

![ フィールドグループがハイライト表示されたAdobe web コネクタスキーマ ](/help/rtcdp/use-case-guides/evolve-one-time-value-lifetime-value/images/adobe-web-schema.png)

[!DNL Adobe] web コネクタスキーマは、次のフィールドグループを含む [!UICONTROL XDM ExperienceEvent] クラスで表されます。

+++Adobe Analytics ExperienceEvent テンプレート（フィールドグループ）

[[!UICONTROL Adobe Analytics ExperienceEvent 完全拡張 &#x200B;]](/help/xdm/field-groups/event/analytics-full-extension.md) は、Adobe Analyticsが収集する共通のメトリクスをキャプチャする標準スキーマフィールドグループです。

+++

### スキーマからのデータセットの作成 {#dataset-from-schema}

データセットは、データグループのストレージと管理の構造です。 このサンプル実装の実行に使用される各スキーマには、1 つのデータセットがあります。

スキーマから [ データセット ](/help/catalog/datasets/overview.md) を作成する方法について詳しくは、[ データセット UI ガイド ](/help/catalog/datasets/user-guide.md) を参照してください。

>[!NOTE]
>
>スキーマを作成する手順と同様に、データセットをリアルタイム顧客プロファイルに含めることができるようにする必要があります。 リアルタイム顧客プロファイルで使用するデータセットを有効にする方法について詳しくは、[ スキーマの作成チュートリアル ](/help/xdm/tutorials/create-schema-ui.md#profile) を参照してください。

### プライバシー、同意、データガバナンス {#privacy-consent}

#### 同意ポリシー

>[!IMPORTANT]
>
>ブランドからの連絡を登録解除する機能を顧客に提供し、この選択を確実に行うことは、法的要件です。 適用される法律について詳しくは、[ プライバシー規制の概要 ](https://experienceleague.adobe.com/docs/experience-platform/privacy/regulations/overview.html?lang=ja) を参照してください。

次の [ 同意ポリシー ](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/consent/overview.html?lang=ja) を実装し、訪問者に連絡する前に同意を求めることを検討してください。

* 電子メール `consents.marketing.email.val = "Y"` 送信できる場合
* その `consents.marketing.sms.val = "Y"`、SMS は可能です
* プッシュでき `consents.marketing.push.val = "Y"` 場合
* `consents.share.val = "Y"` の場合は、次のように広告できます

#### データガバナンスのラベルと適用

次の [ データガバナンスラベル ](/help/data-governance/labels/overview.md) の追加と適用を検討します。

* 個人のメールアドレスは、デバイスではなく特定の個人を識別したり、その個人と連絡を取ったりするために使用される、直接識別可能なデータとして利用されます。
   * `personalEmail.address = I1`

#### マーケティングポリシー

このユースケースの一部として作成するジャーニーには、[ マーケティングポリシー ](/help/data-governance/policies/overview.md) は必要ありません。 ただし、必要に応じて次のポリシーを考慮できます。

* 機密データの制限
* オンサイトAdvertisingの制限
* 電子メールターゲティングの制限
* クロスサイトターゲティングを制限
* 直接識別可能なデータと匿名データの組み合わせを制限

### オーディエンスを作成 {#create-audiences}

このユースケースでは、2 つのオーディエンスを作成して、プロファイルストアのプロファイルのサブセットで共有される特定の属性や行動を定義し、マーケティング可能なユーザーグループを区別する必要があります。 オーディエンスは、Adobe Experience Platformで複数の方法で作成できます。

* オーディエンスの作成方法について詳しくは、[ オーディエンスサービス UI ガイド ](https://experienceleague.adobe.com/docs/experience-platform/segmentation/ui/overview.html?lang=ja#create-audience) を参照してください。
* [ オーディエンス ](/help/segmentation/home.md) の作成方法について詳しくは、[ オーディエンス構成 UI ガイド ](/help/segmentation/ui/audience-composition.md) を参照してください。
* Experience Platformから派生したセグメント定義を使用してオーディエンスを作成する方法について詳しくは、[Audience Builder UI ガイド ](/help/segmentation/ui/segment-builder.md) を参照してください。

特に、次の画像に示すように、ユースケースの異なる手順で 2 つのオーディエンスを作成し使用する必要があります。

![ ハイライト表示されたオーディエンス ](/help/rtcdp/use-case-guides/evolve-one-time-value-lifetime-value/images/audiences-highlighted-in-diagram.png){zoomable="yes"}

>[!BEGINTABS]

>[!TAB Adobe Journey Optimizerの選定対象オーディエンス ]

この高価値かつ低頻度のオーディエンスには、新しい購読プログラムについて知らせるために、ジャーニーを通じて連絡するプロファイルが含まれています。 オーディエンスの詳細は次のとおりです。

* 説明：過去 3 か月間に合計 250 ドルを超える費用を費やしたプロファイル
* オーディエンスで必要なフィールドと条件：
   * イベント：`commerce.order.payments.paymentamount`
*集計合計：>= $250
   * イベントの種類：`commerce.purchases`
* タイムスタンプ：今から 3 か月前


>[!TAB  有料メディアオーディエンス ]

このオーディエンスは、過去 3 か月間に合計 250 ドルを超える費用を費やし、過去 7 日間に購入を行っていないプロファイルを含めるように作成されます。 オーディエンスの詳細は次のとおりです。

* 説明：過去 3 か月間に合計 250 ドル以上を費やし、過去 7 日間に購入していないプロファイル。
* 必要なフィールドと条件：
   * イベントの種類：`journey.feedback`
      * オペランド：= true
   * イベント：`experience.journeyOrchestration.stepEvents.nodeName`
      * オペランド : = JourneyStepEventTracker – 購読は購入されていません
      * タイムスタンプ：過去 7 日間
   * EventType が `commerce.purchases` ではありません
      * タイムスタンプ：今から 7 日前
   * イベント：SKU
      * 値：= `subscription`

>[!ENDTABS]

### Adobe Journey Optimizerでのジャーニー設定 {#journey-setup}

>[!NOTE]
>
>図に示されているすべてが [!DNL Adobe Journey Optimizer] に含まれるわけではありません。 すべての [ 有料メディア広告 ](/help/destinations/catalog/social/overview.md) は、[!UICONTROL &#x200B; 宛先 &#x200B;] [ ワークスペース ](/help/destinations/ui/destinations-workspace.md) に作成されます。

[[!DNL Adobe Journey Optimizer]](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/journey.html?lang=ja) は、顧客とのつながり、コンテキスト、パーソナライズされたエクスペリエンスを提供するのに役立ちます。 カスタマージャーニーは、顧客がブランドとやり取りするプロセス全体です。 各ユースケースのジャーニーには、特定の情報が必要です。

このユースケースを実現するには、次の 2 つの異なるジャーニーを作成する必要があります。

* ライフタイムジャーニー：高価値で低頻度の顧客に送信するメッセージが含まれます
* 電話に応答してサブスクリプションを購入するユーザーの注文確認ジャーニー。

![ ハイライト表示されたジャーニー](/help/rtcdp/use-case-guides/evolve-one-time-value-lifetime-value/images/journeys-highlighted-in-diagram.png){zoomable="yes"}

以下に、各ジャーニーブランチに必要な正確なデータを示します。

>[!BEGINTABS]

>[!TAB  ライフタイムジャーニー]

ライフタイムジャーニーは、過去 30 日以内にターゲットとならなかった高価値および低頻度の顧客のオーディエンスに対応します。 これらの顧客にメッセージが表示され、7 日間経過してもまだ購入されない場合は、有料メディア広告を表示できるオーディエンスに未購入者を含めることができます。 購入した場合は、注文確認ジャーニー（詳細は別のタブで説明）で購入者を設定できます。

![ ライフタイムジャーニーの大まかなビジュアル概要。](/help/rtcdp/use-case-guides/evolve-one-time-value-lifetime-value/images/lifetime-journey.png " ライフタイムジャーニーに対する 1 回限りの価値の概要の視覚的な概要。"){zoomable="yes"}

+++詳細なジャーニーロジック

上記のジャーニーは、次のロジックに従います。

1. オーディエンスを読み取り：上記のオーディエンスの節で作成した最初のオーディエンスに対して、[ オーディエンスを読み取りアクティビティ ](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/about-journey-building/read-audience.html?lang=ja) を使用します。

2. 条件 – 優先チャネル：[ 条件アクティビティ ](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/about-journey-building/condition-activity.html?lang=ja) を使用して、メール、SMS、プッシュ通知のどれを介して顧客に連絡するかを決定します。 3 つのアクションアクティビティを使用して、3 つのブランチを作成します。

3. 待機：購入をリッスンするまで待機するには、[ 待機アクティビティ ](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/about-journey-building/read-audience.html?lang=ja) を使用します。

4. 条件 – 過去 7 日間に購入したサブスクリプション :「条件」アクティビティを使用して、過去 7 日間に製品が購入されたかどうかをリッスンします。

5. JourneyStepEventTracker - サブスクリプションが購入されていません：メッセージを受信したにもかかわらずサブスクリプションをまだ購入していない訪問者には [ カスタムアクション ](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/about-journey-building/using-custom-actions.html?lang=ja) を使用します。 ジャーニーの最後のカスタム条件の一部として、`journey.feedback` イベントを作成し、[!UICONTROL ジャーニーステップイベント &#x200B;] スキーマに基づいてデータセットに追加します。 このイベントを使用して、サブスクリプションを購入していないオーディエンスをセグメント化し、有料メディア広告を介してターゲットにすることができます。

+++

>[!TAB  注文確認ジャーニー]

注文確認ジャーニーでは、購入が web サイトまたはモバイルアプリを通じて行われたかどうかに焦点を当てています。 顧客が会社のサブスクリプションなどの購入を正常に完了したら、注文確認ジャーニーにそれらを設定できます。

![ 顧客注文確認ジャーニーの大まかなビジュアル概要。](/help/rtcdp/use-case-guides/evolve-one-time-value-lifetime-value/images/order-confirmation-journey.png " 顧客注文確認ジャーニーの大まかなビジュアルの概要 "){zoomable="yes"}

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

   * チャネルコンテンツPersonalization
      * 注文の詳細情報を表示し、テーブル形式を使用して商品のリストを表示できます。

+++

>[!ENDTABS]

[!DNL Adobe Journey Optimizer] でジャーニーを作成する方法について詳しくは、[ ジャーニーの概要 ](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/journey.html?lang=ja) ガイドを参照してください。

### 有料メディア広告を表示する宛先の設定 {#paid-media-ads}

一部のユーザーは、新しいプログラムについてメッセージを送信した後も、サブスクリプションを購入していない可能性があります。 日数を待った後（このユースケースでは 7 日）、それらのユーザーに有料のメディア広告を表示して、サブスクリプションの購入を促すことができます。

有料メディア広告には、Real-Time CDPの宛先フレームワークを使用します。 利用可能な多数の広告宛先の 1 つを選択して、顧客に有料メディア広告を表示し、選択した宛先に対して [ 以前に作成した ](#create-audiences) 有料メディアオーディエンスをアクティブ化します。 使用可能な [ 広告 ](/help/destinations/catalog/advertising/overview.md) 宛先と [ ソーシャル ](/help/destinations/catalog/social/overview.md) 宛先の概要を参照してください。

宛先へのデータをアクティブ化する方法（例：[The Trade Desk](/help/destinations/catalog/advertising/tradedesk.md) または [Google Customer Match](/help/destinations/catalog/advertising/google-customer-match.md)）については、以下のドキュメントを参照してください。

* [新しい宛先接続の作成](/help/destinations/ui/connect-destination.md)
* [ストリーミングオーディエンスの書き出し先に対するオーディエンスデータの有効化](/help/destinations/ui/activate-segment-streaming-destinations.md)

## 次の手順 {#next-steps}

ジャーニーで低頻度ユーザーと高価値ユーザーを設定し、そのサブセットに有料メディア広告を表示することで、一部のユーザーを 1 回限りの価値から生涯価値の顧客に変え、ブランドロイヤルティと顧客エンゲージメントの指標を改善できればと考えています。

次に、Real-Time CDPでサポートされている他の使用例、例えば [ インテリジェントに顧客をリエンゲージする ](/help/rtcdp/use-case-guides/intelligent-re-engagement/intelligent-re-engagement.md) または [ 認証されていないユーザーにパーソナライズされたコンテンツを表示する ](/help/rtcdp/partner-data/onsite-personalization.md) を web プロパティで調べることができます。
