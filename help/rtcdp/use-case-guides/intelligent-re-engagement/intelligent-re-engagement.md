---
title: インテリジェントな再エンゲージメント
description: 重要なコンバージョン期間中に魅力的でつながりのあるエクスペリエンスを提供し、頻度の低い顧客をインテリジェントに再び惹き付けます。
feature: Use Cases
exl-id: 13f6dbc9-7471-40bf-824d-27922be0d879
source-git-commit: e52eb90b64ae9142e714a46017cfd14156c78f8b
workflow-type: tm+mt
source-wordcount: '3894'
ht-degree: 3%

---

# インテリジェントな再エンゲージメントで通じて顧客の再来訪を促す

>[!NOTE]
>
>これは実装例であり、このページでの例（セグメント構文など）は単なる例です。 実装方法は異なる場合があるので、例を参考にしてください。

コンバージョンを放棄した顧客を、インテリジェントかつ責任ある方法で再び関与させます。 失効した顧客のエクスペリエンスを活用してコンバージョン率を高め、クライアントの生涯価値を高めます。

リアルタイムの考慮事項を採用し、すべての消費者の資質と行動を考慮に入れ、オンラインとオフラインの両方のイベントに基づいて迅速な再認定を提供します。

以下は、Real-Time CDPとJourney Optimizerの様々なコンポーネントのアーキテクチャの概要です。 次の図は、このページで説明するユースケースを達成するために、データ収集からジャーニーやキャンペーンを通じて宛先に対してアクティブ化される時点までの、2 つのExperience Platformアプリを介したデータのフローを示しています。

![インテリジェントな再エンゲージメントの概要を視覚的に把握](../intelligent-re-engagement/images/step-by-step.png)

## ユースケースの概要 {#overview}

再エンゲージメントシナリオの例を通じて作業しながら、スキーマ、データセット、オーディエンスを作成します。 また、でサンプルジャーニーを設定するために必要な機能についても説明します [!DNL Adobe Journey Optimizer] そして、宛先で有料メディア広告を作成するために必要なもの。 このガイドでは、以下に概説するユースケースジャーニーで、顧客のリエンゲージメントの例を使用します。

* **製品の参照シナリオを破棄** - web サイトとモバイルアプリの両方で製品の参照を放棄した顧客をターゲットにします。
* **放棄された買い物かごシナリオ**  – 商品を買い物かごに入れたが、web サイトとモバイルアプリの両方でまだ購入されていない顧客をターゲットにします。
* **注文確認シナリオ** - web サイトやモバイルアプリを通じて行われた製品の購入に焦点を当てます。

## 前提条件と計画 {#prerequisites-and-planning}

このユースケースを実装する手順を完了すると、次のReal-Time CDPおよびAdobe Journey Optimizer機能（使用順序にリストされている）を利用できるようになります。 これらすべての領域に必要な[属性ベースのアクセス制御権限](/help/access-control/home.md)があることを確認するか、必要な権限の付与をシステム管理者に依頼してください。

* [[!DNL Adobe Real-Time Customer Data Platform (Real-Time CDP)]](https://experienceleague.adobe.com/docs/platform-learn/tutorials/rtcdp/understanding-the-real-time-customer-data-platform.html) - データソース間でデータを統合して、キャンペーンに燃料を供給します。 その後、このデータを使用して、キャンペーンオーディエンスを作成し、メールおよび web プロモタイルで使用されるパーソナライズされたデータ要素（名前やアカウントに関連する情報など）を表示します。 CDP は、メールおよび web をまたいでオーディエンスをアクティブ化するためにも使用されます（経由） [!DNL Adobe Target]）に設定します。
   * [スキーマ](/help/xdm/home.md)
   * [プロファイル](/help/profile/home.md)
   * [データセット](/help/catalog/datasets/overview.md)
   * [オーディエンス](/help/segmentation/home.md)
   * [[!DNL Adobe Journey Optimizer]](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/journey.html)
   * [宛先](/help/destinations/home.md)

* [[!DNL Adobe Journey Optimizer]](https://experienceleague.adobe.com/docs/journey-optimizer-learn/tutorials/introduction-to-journey-optimizer/introduction.html?lang=ja)  – は、顧客とのつながり、コンテキスト、パーソナライズされたエクスペリエンスを提供するのに役立ちます。
   * [イベントまたはオーディエンスのトリガー](https://experienceleague.adobe.com/docs/journey-optimizer/using/offer-decisioning/collect-event-data/data-collection.html)
   * [オーディエンス/イベント](https://experienceleague.adobe.com/docs/journey-optimizer/using/audiences-profiles-identities/audiences/about-audiences.html?lang=ja)
   * [ジャーニーアクション](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/journey.html)

## ユースケースの達成方法 {#achieve-use-case-instruction}

以下は、3 つの再エンゲージメントシナリオの例の概要です。

>[!BEGINTABS]

>[!TAB 製品の参照シナリオを破棄]

放棄された製品参照シナリオは、web サイトとモバイルアプリの両方での放棄された製品参照をターゲットにします。 このシナリオがトリガーされるのは、製品が表示されたが、購入や買い物かごへの追加は行われなかった場合です。 この例では、過去 24 時間以内にリストの追加がない場合、ブランドエンゲージメントは 3 日後にトリガーされます。<p>![お客様のインテリジェントな放棄された製品の参照シナリオの全体的な視覚的概要。](../intelligent-re-engagement/images/re-engagement-journey.png "お客様のインテリジェントな放棄された製品の参照シナリオの全体的な視覚的概要。"){width="1920" zoomable="yes"}</p>

1. スキーマとデータセットを作成して、以下を有効にします [!UICONTROL Profile].
2. Web SDK、Mobile SDK または API を使用して、データをExperience Platformに取り込みます。 Analytics ソースコネクタも利用できますが、ジャーニー遅延が発生する可能性があります。
3. 追加のプロファイル対応データを取り込みます。これは、ID グラフを使用して、認証済み web およびモバイルアプリの訪問者にリンクできます。
4. プロファイルのリストから対象オーディエンスを作成して、 **顧客** 過去 3 日間に婚約しました。
5. 放棄された製品参照ジャーニーをで作成します。 [!DNL Adobe Journey Optimizer].
6. 必要に応じて、を操作します **データパートナー** オーディエンスを目的の有料メディア宛先にアクティブ化する。
7. [!DNL Adobe Journey Optimizer] 同意を確認し、設定された様々なアクションを送信します。

>[!TAB 放棄された買い物かごシナリオ]

放棄された買い物かごシナリオが適用されるのは、製品が買い物かごに入れられているが、web サイトとモバイルアプリの両方でまだ購入されていない場合です。 また、この方法を使用して、有料メディアキャンペーンを開始および停止します。<p>![顧客が買い物かごの放棄シナリオの概要レベルの視覚的な概要。](../intelligent-re-engagement/images/abandoned-cart-journey.png "顧客が買い物かごの放棄シナリオの概要レベルの視覚的な概要。"){width="1920" zoomable="yes"}</p>

1. スキーマおよびデータセットを作成して、次を有効にします [!UICONTROL Profile].
2. Web SDK、Mobile SDK または API を使用して、データをExperience Platformに取り込みます。 Analytics ソースコネクタも利用できますが、ジャーニー遅延が発生する可能性があります。
3. 追加のプロファイル対応データを取り込みます。これは、ID グラフを使用して、認証済み web およびモバイルアプリの訪問者にリンクできます。
4. プロファイルのリストから対象オーディエンスを作成して、 **顧客** 様が買い物かごに商品を入れましたが、購入は完了していません。 この **[!UICONTROL カートに追加]** イベントは、30 分間待った後に購入を確認するタイマーを開始します。 購入されていない場合は、 **顧客** がに追加されました **[!UICONTROL カートを放棄]** オーディエンス。
5. 放棄された買い物かごジャーニーをで作成します。 [!DNL Adobe Journey Optimizer].
6. 必要に応じて、を操作します **データパートナー** オーディエンスを目的の有料メディア宛先にアクティブ化する。
7. [!DNL Adobe Journey Optimizer] 同意を確認し、設定された様々なアクションを送信します。

>[!TAB 注文確認シナリオ]

注文確認シナリオは、web サイトとモバイルアプリを通じて行われた製品の購入に焦点を当てています。<p>![顧客注文確認シナリオの全体的な視覚的な概要。](../intelligent-re-engagement/images/order-confirmation-journey.png "顧客注文確認シナリオの全体的な視覚的な概要。"){width="1920" zoomable="yes"}</p>

1. スキーマとデータセットを作成して、以下を有効にします [!UICONTROL Profile].
2. Web SDK、Mobile SDK または API を使用して、データをExperience Platformに取り込みます。 Analytics ソースコネクタも利用できますが、ジャーニー遅延が発生する可能性があります。
3. 追加のプロファイル対応データを取り込みます。これは、ID グラフを使用して、認証済み web およびモバイルアプリの訪問者にリンクできます。
4. 確認ジャーニーはで作成します。 [!DNL Adobe Journey Optimizer].
5. [!DNL Adobe Journey Optimizer] 優先チャネルを使用して注文確認メッセージを送信します。

>[!ENDTABS]

上記の概要の各手順を完了するには、以下の節を参照してください。この節には、詳細情報や詳細な手順へのリンクが用意されています。

### スキーマの作成とフィールドグループの指定 {#schema-design}

エクスペリエンスデータモデル（XDM）リソースは、 [!UICONTROL スキーマ] ワークスペース： [!DNL Adobe Experience Platform]. が提供するコアリソースを表示および調査できます。 [!DNL Adobe] （例えば、フィールドグループ）を作成し、組織のカスタムリソースとスキーマを作成します。

の作成について詳しくは、 [スキーマ](/help/xdm/home.md)を参照してください。 [スキーマの作成チュートリアル。](/help/xdm/tutorials/create-schema-ui.md) および [XDM を使用したカスタマーエクスペリエンスデータのモデル化](https://experienceleague.adobe.com/docs/courses/using/experienceplatform-d-1-2021-1-xdm.html).

再エンゲージメントのユースケースには、4 つのスキーマデザインが使用されます。 各スキーマには、設定する特定のフィールドが必要です。 リアルタイム顧客プロファイルにスキーマを含めることができるようにする必要があります。 リアルタイム顧客プロファイルで使用するスキーマを有効にする方法について詳しくは、以下を参照してください [リアルタイム顧客プロファイルのスキーマの有効化](/help/xdm/ui/resources/schemas.md#enable-a-schema-for-real-time-customer-profile).

#### 顧客属性スキーマ

このスキーマは、顧客情報を構成するプロファイルデータを構造化して参照するために使用されます。 このデータは通常、に取り込まれます。 [!DNL Adobe Experience Platform] crm または同様のシステムを通じて、パーソナライゼーション、マーケティングの同意、オーディエンスの強化機能に使用される顧客の詳細を参照するために必要です。

顧客属性スキーマは、 [[!UICONTROL XDM 個人プロファイル]](/help/xdm/classes/individual-profile.md) 以下のフィールドグループを含むクラス。

+++個人の連絡先の詳細（フィールドグループ）

[個人の連絡先の詳細](/help/xdm/field-groups/profile/personal-contact-details.md) は、XDM 個人プロファイル クラスの標準スキーマフィールドグループで、個人の連絡先情報を記述します。

| フィールド | 説明 |
| --- | --- |
| `mobilePhone.number` | SMS に使用される人物の携帯電話番号。 |
| `personalEmail.address` | 人物の電子メールアドレス。 |

+++

+++外部ソースシステム監査詳細（フィールドグループ）

[外部ソースシステム監査属性](/help/xdm/data-types/external-source-system-audit-attributes.md) は、外部ソースシステムに関する監査詳細をキャプチャする標準のエクスペリエンスデータモデル（XDM）データタイプです。

+++

+++同意および環境設定フィールドグループ（フィールドグループ）

この [同意および環境設定](/help/xdm/field-groups//profile/consents.md) フィールドグループは、同意と環境設定に関する情報を取り込むために、単一のオブジェクトタイプのフィールド「同意」を提供します。

| フィールド | 要件 |
| --- | --- |
| `consents.marketing.email.val` | 必須 |
| `consents.marketing.preferred` | 必須 |
| `consents.marketing.push.val` | 必須 |
| `consents.marketing.sms.val` | 必須 |
| `consents.personalize.content.val` | 必須 |
| `consents.share.val` | 必須 |

+++

+++プロファイルテストの詳細（フィールドグループ）

このフィールドグループを使用すると、テストプロファイルを使用して、公開前にジャーニーをテストできます。 テストプロファイルの作成について詳しくは、 [テストプロファイルの作成のチュートリアル](https://experienceleague.adobe.com/docs/journeys/using/building-journeys/about-journey-building/creating-test-profiles.html) および [ジャーニーチュートリアルのテスト](https://experienceleague.adobe.com/docs/journeys/using/building-journeys/testing-the-journey.html).

+++

#### 顧客デジタルトランザクションスキーマ

このスキーマは、Web サイトまたは関連するデジタルプラットフォームで発生する顧客アクティビティを構成するイベントデータを構造化し、参照するために使用されます。 このデータは通常、に取り込まれます。 [!DNL Adobe Experience Platform] 経由 [Web SDK](/help/web-sdk/home.md) ジャーニーのトリガーに使用される様々な参照およびコンバージョンイベント、詳細なオンライン顧客分析、強化されたオーディエンス機能およびパーソナライズされたメッセージングを参照する際に必要です。

顧客デジタルトランザクションスキーマは、 [[!UICONTROL XDM ExperienceEvent]](/help/xdm/classes/experienceevent.md) クラス。

+++XDM ExperienceEvent （クラス）

この [[!UICONTROL XDM ExperienceEvent]](/help/xdm/classes/experienceevent.md) クラスには、次のフィールドグループが含まれます。

| フィールド | 説明 |
| --- | --- |
| `_id` | に取り込まれる個々のイベントを一意に識別します [!DNL Adobe Experience Platform]. |
| `timestamp` | イベントが発生した時点の ISO 8601 タイムスタンプ（RFC 3339 セクション 5.6 を準拠した書式設定）。このタイムスタンプは過去の日付にする必要があります。 |
| `eventType` | イベントのカテゴリのタイプを示す文字列。 |

+++

+++エンドユーザー ID 詳細（フィールドグループ）

この [エンドユーザー ID 詳細](/help/xdm/field-groups/event/enduserids.md) フィールドグループは、複数のAdobeアプリケーションをまたいで個人の id 情報を記述するために使用されます。

| フィールド | 説明 |
| --- | --- |
| `endUserIDs._experience.emailid.authenticatedState` | エンドユーザーのメールアドレス ID が認証された状態。 |
| `endUserIDs._experience.emailid.id` | エンドユーザーのメールアドレス ID。 |
| `endUserIDs._experience.emailid.namespace.code` | エンドユーザーのメールアドレス ID 名前空間コード。 |
| `endUserIDs._experience.mcid.authenticatedState` | [!DNL Adobe] Marketing CloudID （MCID）の認証状態。 MCID はExperience CloudID （ECID）になりました。 |
| `endUserIDs._experience.mcid.id` | [!DNL Adobe] Marketing CloudID （MCID）。 MCID はExperience CloudID （ECID）になりました。 |
| `endUserIDs._experience.mcid.namespace.code` | [!DNL Adobe] Marketing CloudID （MCID）名前空間コード。 |

+++

+++Commerceの詳細（フィールドグループ）

この [Commerceの詳細](/help/xdm/field-groups/event/commerce-details.md) フィールドグループは、商品情報（SKU、名前、数量）や標準の買い物かご操作（注文、チェックアウト、放棄）などのコマースデータを記述するために使用されます。

| フィールド | 説明 |
| --- | --- |
| `commerce.cart.cartID` | 買い物かごの ID。 |
| `commerce.order.orderType` | 製品の注文タイプを説明するオブジェクト。 |
| `commerce.order.payments.paymentAmount` | 製品の注文支払い金額を説明するオブジェクト。 |
| `commerce.order.payments.paymentType` | 製品の注文の支払いタイプを説明するオブジェクト。 |
| `commerce.order.payments.transactionID` | オブジェクト製品注文トランザクション ID。 |
| `commerce.order.purchaseID` | 対象製品注文の購入 ID。 |
| `productListItems.name` | 顧客が選択した商品を表す項目名のリスト。 |
| `productListItems.priceTotal` | 顧客が選択した商品を表す商品のリストの合計価格。 |
| `productListItems.product` | 選択された製品。 |
| `productListItems.quantity` | 顧客が選択した商品を表す品目のリストの数量。 |

+++

+++外部ソースシステム監査詳細（フィールドグループ）

外部ソースシステム監査属性は、外部ソースシステムに関する監査詳細を取得する標準の Experience Data Model （XDM）データタイプです。

+++

#### 顧客のオフライントランザクションスキーマ

このスキーマは、web サイト外のプラットフォームで発生する顧客アクティビティを構成するイベントデータを構造化し、参照するために使用されます。 このデータは通常、に取り込まれます。 [!DNL Adobe Experience Platform] POS （または同様のシステム）から、ほとんどの場合、API 接続を介して Platform にストリーミングされます。 その目的は、ジャーニーのトリガーに使用される様々なオフラインコンバージョンイベント、詳細なオンラインとオフラインの顧客分析、強化されたオーディエンス機能およびパーソナライズされたメッセージを参照することです。

顧客のオフライントランザクションスキーマは、 [[!UICONTROL XDM ExperienceEvent]](/help/xdm/classes/experienceevent.md) クラス。

+++XDM ExperienceEvent （クラス）

この [[!UICONTROL XDM ExperienceEvent]](/help/xdm/classes/experienceevent.md) クラスには、次のフィールドグループが含まれます。

| フィールド | 説明 |
| --- | --- |
| `_id` | に取り込まれる個々のイベントを一意に識別します [!DNL Adobe Experience Platform]. |
| `timestamp` | イベントが発生した時点の ISO 8601 タイムスタンプ（RFC 3339 セクション 5.6 を準拠した書式設定）。このタイムスタンプは過去の日付にする必要があります。 |
| `eventType` | イベントのカテゴリのタイプを示す文字列。 |

+++

+++Commerceの詳細（フィールドグループ）

この [Commerceの詳細](/help/xdm/field-groups/event/commerce-details.md) フィールドグループは、商品情報（SKU、名前、数量）や標準の買い物かご操作（注文、チェックアウト、放棄）などのコマースデータを記述するために使用されます。

| フィールド | 説明 |
| --- | --- |
| `commerce.cart.cartID` | 買い物かごの ID。 |
| `commerce.order.orderType` | 製品の注文タイプを説明するオブジェクト。 |
| `commerce.order.payments.paymentAmount` | 製品の注文支払い金額を説明するオブジェクト。 |
| `commerce.order.payments.paymentType` | 製品の注文の支払いタイプを説明するオブジェクト。 |
| `commerce.order.payments.transactionID` | オブジェクト製品注文トランザクション ID。 |
| `commerce.order.purchaseID` | 対象製品注文の購入 ID。 |
| `productListItems.name` | 顧客が選択した商品を表す項目名のリスト。 |
| `productListItems.priceTotal` | 顧客が選択した商品を表す商品のリストの合計価格。 |
| `productListItems.product` | 選択された製品。 |
| `productListItems.quantity` | 顧客が選択した商品を表す品目のリストの数量。 |

+++

+++個人の連絡先の詳細（フィールドグループ）

[個人の連絡先の詳細](/help/xdm/field-groups/profile/personal-contact-details.md) は、XDM 個人プロファイル クラスの標準スキーマフィールドグループで、個人の連絡先情報を記述します。

| フィールド | 説明 |
| --- | --- |
| `mobilePhone.number` | SMS に使用される人物の携帯電話番号。 |
| `personalEmail.address` | 人物の電子メールアドレス。 |

+++

+++外部ソースシステム監査詳細（フィールドグループ）

外部ソースシステム監査属性は、外部ソースシステムに関する監査詳細を取得する標準の Experience Data Model （XDM）データタイプです。

+++

#### Adobe web コネクタスキーマ

>[!NOTE]
>
>を使用する場合、これはオプションの実装です。 [[!DNL Adobe Analytics Source Connector]](/help/sources/connectors/adobe-applications/analytics.md).

このスキーマは、Web サイトまたは関連するデジタルプラットフォームで発生する顧客アクティビティを構成するイベントデータを構造化し、参照するために使用されます。 このスキーマは、顧客デジタルトランザクションスキーマに似ていますが、次の場合に使用されるという点で異なります [Web SDK](/help/web-sdk/home.md) はデータ収集用のオプションではありません。したがって、を使用するときは、このスキーマが必要です。 [!DNL Adobe Analytics Source Connector] オンラインデータをに送信する [!DNL Adobe Experience Platform] プライマリまたはセカンダリデータストリームとして。

この [!DNL Adobe] web コネクタスキーマはで表されます。 [[!UICONTROL XDM ExperienceEvent]](/help/xdm/classes/experienceevent.md) クラス。

+++XDM ExperienceEvent （クラス）

この [[!UICONTROL XDM ExperienceEvent]](/help/xdm/classes/experienceevent.md) クラスには、次のフィールドグループが含まれます。

| フィールド | 説明 |
| --- | --- |
| `_id` | に取り込まれる個々のイベントを一意に識別します [!DNL Adobe Experience Platform]. |
| `timestamp` | イベントが発生した時点の ISO 8601 タイムスタンプ（RFC 3339 セクション 5.6 を準拠した書式設定）。このタイムスタンプは過去の日付にする必要があります。 |
| `eventType` | イベントのカテゴリのタイプを示す文字列。 |

+++

+++Adobe Analytics ExperienceEvent テンプレート（フィールドグループ）

この [Adobe Analytics ExperienceEvent](/help/xdm/field-groups/event/analytics-full-extension.md) フィールドグループは、Adobe Analyticsが収集する一般的な指標をキャプチャします。

| フィールド | 説明 |
| --- | --- |
| `endUserIDs._experience.emailid.authenticatedState` | エンドユーザーのメールアドレス ID が認証された状態。 |
| `endUserIDs._experience.emailid.id` | エンドユーザーのメールアドレス ID。 |
| `endUserIDs._experience.emailid.namespace.code` | エンドユーザーのメールアドレス ID 名前空間コード。 |
| `endUserIDs._experience.mcid.authenticatedState` | [!DNL Adobe] Marketing CloudID （MCID）の認証状態。 MCID はExperience CloudID （ECID）になりました。 |
| `endUserIDs._experience.mcid.id` | [!DNL Adobe] Marketing CloudID （MCID）。 MCID はExperience CloudID （ECID）になりました。 |
| `endUserIDs._experience.mcid.namespace.code` | [!DNL Adobe] Marketing CloudID （MCID）名前空間コード。 |

+++

+++外部ソースシステム監査詳細（フィールドグループ）

外部ソースシステム監査属性は、外部ソースシステムに関する監査詳細を取得する標準の Experience Data Model （XDM）データタイプです。

+++

### スキーマからのデータセットの作成 {#create-datasets}

データセットは、データグループのストレージと管理の構造です。 インテリジェントな再エンゲージメントシナリオの各スキーマには、独自のデータセットが必要です。

を作成する方法について詳しくは、 [データセット](/help/catalog/datasets/overview.md) スキーマから、を読み取ります。 [データセット UI ガイド](/help/catalog/datasets/user-guide.md).

>[!NOTE]
>
>スキーマを作成する手順と同様に、データセットをリアルタイム顧客プロファイルに含めることができるようにする必要があります。 リアルタイム顧客プロファイルで使用するデータセットを有効にする方法について詳しくは、のチュートリアルを参照してください。 [リアルタイム顧客プロファイルへのデータの取り込み](https://experienceleague.adobe.com/docs/platform-learn/tutorials/profiles/bring-data-into-the-real-time-customer-profile.html?lang=ja).

### 同意とデータガバナンス {#privacy-consent}

>[!IMPORTANT]
>
>ブランドからの連絡を登録解除する機能を顧客に提供し、この選択を確実に行うことは、法的要件です。 適用される法律の詳細については、を参照してください [プライバシー規制の概要](https://experienceleague.adobe.com/docs/experience-platform/privacy/regulations/overview.html).

#### 同意ポリシー

再エンゲージメントパスを作成する場合は、次の項目を追加することを検討してください [同意ポリシー](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/consent/overview.html):

* 次の場合 `consents.marketing.email.val = "Y"` その後、電子メールで送信できます
* 次の場合 `consents.marketing.sms.val = "Y"` その後、SMS を送信できる
* 次の場合 `consents.marketing.push.val = "Y"` その後、プッシュできます
* 次の場合 `consents.share.val = "Y"` 次に、広告可能

#### データガバナンスのラベル付けと実施

再エンゲージメントパスを作成する場合は、次の項目を追加することを検討してください [データガバナンスラベル](/help/data-governance/labels/overview.md):

* 個人の電子メールアドレスは、デバイスではなく特定の個人を識別したり、特定の個人と連絡を取ったりするために使用される、直接識別可能なデータとして利用されます。
   * `personalEmail.address = I1`

#### データ使用ポリシー

がありません [データ使用ポリシー](/help/data-governance/policies/overview.md) 放棄された製品の参照シナリオの場合は必須です。 ただし、次の点を考慮する必要があります。

* 機密データの制限
* オンサイト広告の制限
* 電子メールターゲティングの制限
* クロスサイトターゲティングを制限
* 直接識別可能なデータと匿名データの組み合わせを制限

### オーディエンスを作成 {#create-audience}

再エンゲージメントシナリオでは、オーディエンスを使用して、プロファイルストアのプロファイルのサブセットで共有される特定の属性や行動を定義し、マーケティング可能なユーザーグループを顧客ベースと区別します。 オーディエンスは、で複数の方法で作成できます [!DNL Adobe Experience Platform].

オーディエンスの作成方法について詳しくは、 [audience service UI ガイド](https://experienceleague.adobe.com/docs/experience-platform/segmentation/ui/overview.html#create-audience).

直接作成方法について詳しくは、こちらを参照してください [オーディエンス](/help/segmentation/home.md)、を読み取ります [オーディエンス構成 UI ガイド](/help/segmentation/ui/audience-composition.md).

Platform から派生したオーディエンスの定義を使用してオーディエンスを作成する方法について詳しくは、以下を参照してください [Audience Builder UI ガイド](/help/segmentation/ui/segment-builder.md).

>[!BEGINTABS]

>[!TAB 製品の参照シナリオを破棄]

このオーディエンスは、従来の「買い物かごの放棄」シナリオの機能強化として作成されます。 通常、買い物かごの放棄は、特定の期間内に後続の購入なしに買い物かごへの追加に焦点を当てますが、このオーディエンスは、特定の製品を閲覧したが買い物かごに追加せず、特定の期間内にサイトでフォローアップアクティビティを行わなかった可能性のある以前のエンゲージメント、特に検索します。 このオーディエンスは、このインクルージョン条件を満たす顧客にとってブランドを「第一」に考えるのに役立ちます。また、デジタルプロパティが従来の e コマースモデルとは異なる可能性がある顧客にも活用できます。

+++過去 3 日間にエンゲージメントのない放棄された製品表示

次のイベントは、ユーザーが製品をオンラインで閲覧し、その後 3 日以内にエンゲージ（サイト訪問、アプリ訪問、オンライン購入、オフライン購入、買い物かごに追加イベント）しなかった、放棄された製品参照シナリオに使用されます。

このオーディエンスを設定する場合、次のフィールドと条件が必要です。

* `eventType: commerce.productViews`
* および `THEN` （順次イベント）除外 `eventType: commerce.productListAdds` および `application.launch` および `web.webpagedetails.pageViews` および `commerce.purchases` （オンラインとオフラインの両方を含む）
   * `Timestamp: > 3 days after productView`
* `Timestamp: > 4 days`

+++

+++過去 3 日間のエンゲージメントに関する製品表示

次のイベントは、ユーザーが製品をオンラインで閲覧し、後 3 日以内にエンゲージ（サイト訪問、アプリ訪問、オンライン購入、オフライン購入、買い物かごに追加イベント）した、放棄された製品参照シナリオに使用されます。

このオーディエンスを設定する場合、次のフィールドと条件が必要です。

* `eventType: commerce.productViews`
* および `THEN` （順次イベント）次を含む `eventType: commerce.productListAdds` または `application.launch` または `web.webpagedetails.pageViews` または `commerce.purchases` （オンラインとオフラインの両方を含む）
   * `Timestamp: > 3 days after productView`
* `Timestamp: > 4 days`
+++

+++過去 1 日間のエンゲージメントストリーミング

次のイベントは、過去 1 日間にユーザーが関与した（サイト訪問、アプリ訪問、オンライン購入、オフライン購入、買い物かごに追加イベント）放棄された製品参照シナリオに使用されます。

このオーディエンスを設定する場合、次のフィールドと条件が必要です。

* `eventType: commerce.productListAdds OR application.launch OR web.webpagedetails.pageViews OR commerce.purchases`
   * `Timestamp: in last 1 day` （ストリーミング）

+++

+++過去 3 日間のエンゲージメントバッチ

次のイベントは、過去 3 日間にユーザーが関与した（サイト訪問、アプリ訪問、オンライン購入、オフライン購入、買い物かごに追加イベント）放棄された製品参照シナリオに使用されます。

このオーディエンスを設定する場合、次のフィールドと条件が必要です。

* `EventType: commerce.productListAdds OR application.launch OR web.webpagedetails.pageViews OR commerce.purchases`
   * `Timestamp: in last 3 days` （バッチ）

+++

>[!TAB 放棄された買い物かごシナリオ]

このオーディエンスは、従来の「買い物かごの放棄」シナリオをサポートするために作成されます。 その目的は、買い物かごに製品を追加したが、最終的に購入を遂行しなかった顧客を見つけることです。 このオーディエンスは、顧客のブランドを「最優先」にするだけでなく、その後の購入でブランドが残した製品を維持するのに役立ちます。

次のイベントは、放棄された買い物かごシナリオで使用されます。このシナリオでは、ユーザーは 1 ～ 4 日前に買い物かごに製品を追加したものの、購入を完了していないか、買い物かごをクリアしていません。

このオーディエンスを設定する場合、次のフィールドと条件が必要です。

* `eventType: commerce.productListAdds`
   * `Timestamp: >= 1 days before now AND <= 4 days before now `
* `eventType: commerce.purchases`
   * `Timestamp: <= 4 days before now`
* `eventType: commerce.productListRemovals`
   * `Timestamp: <= 4 days before now`

放棄された買い物かごシナリオの記述子は、次のように表示されます。

`Include eventType = commerce.productListAdds between 30 min and 1440 minutes before now. exclude eventType = commerce.purchases 30 minutes before now OR eventType = commerce.productListRemovals AND Cart ID equals Product List Adds1 Cart ID (the inclusion event).`

>[!TAB 注文確認シナリオ]

このジャーニーでは、オーディエンスを作成する必要はありません。

>[!ENDTABS]

### Adobe Journey Optimizerでのジャーニー設定 {#journey-setup}

>[!NOTE]
>
>[!DNL Adobe Journey Optimizer] 図に示されているすべての内容が含まれるわけではありません。 すべて [有料メディア広告](/help/destinations/catalog/social/overview.md) 作成先 [!UICONTROL 宛先].

[[!DNL Adobe Journey Optimizer]](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/journey.html) は、顧客とのつながり、コンテキスト、パーソナライズされたエクスペリエンスを提供するのに役立ちます。 カスタマージャーニーは、顧客がブランドとやり取りするプロセス全体です。 各ユースケースのジャーニーには、特定の情報が必要です。 以下に、各ジャーニーで必要な正確なデータを示します。

>[!BEGINTABS]

>[!TAB 製品の参照シナリオを破棄]

放棄された製品参照シナリオは、web サイトとモバイルアプリの両方での放棄された製品参照をターゲットにします。<p>![お客様が製品の参照シナリオを放棄した、全体的な視覚的な概要。](../intelligent-re-engagement/images/re-engagement-journey.png "お客様が製品の参照シナリオを放棄した、全体的な視覚的な概要。"){width="1920" zoomable="yes"}</p>

+++イベント

イベントを使用すると、ジャーニーをまとめてトリガーし、ジャーニーの過程にある個人にリアルタイムでメッセージを送信できます。イベントの詳細については、 [一般的なイベントガイド](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/about-journey-building/general-events.html).

* イベント 1：製品表示回数
   * スキーマ：顧客デジタルトランザクション
   * フィールド :
      * `eventType`
   * 条件：
      * `eventType = commerce.productViews`
      * フィールド :
         * `eventType`
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

* イベント 2：買い物かごに追加
   * スキーマ：顧客デジタルトランザクション
   * フィールド :
      * `eventType`
   * 条件：
      * `eventType = commerce.productListAdds`
      * フィールド :
         * `commerce.productListAdds.id`
         * `commerce.productListAdds.value`
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
         * `commerce.cart.cartID`
         * `endUserIDs._experience.emailid.authenticatedState`
         * `endUserIDs._experience.emailid.id`
         * `endUserIDs._experience.emailid.namespace.code`
         * `_id`

* イベント 3：ブランドエンゲージメント
   * スキーマ：顧客デジタルトランザクション
   * フィールド :
      * `eventType`
   * 条件：
      * `eventType in application.launch, commerce.purchases, web.webpagedetails.pageViews`
      * フィールド :
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
         * `web.webpagedetails.URL`
         * `web.webpagedetails.isHomePage`
         * `web.webpagedetails.name`
         * `endUserIDs._experience.emailid.authenticatedState`
         * `endUserIDs._experience.emailid.id`
         * `endUserIDs._experience.emailid.namespace.code`
         * `_id`
         * `commerce.purchases.id`
         * `commerce.purchases.value`
         * `shipping.address.city`
         * `shipping.address.countryCode`
         * `shipping.address.postalCode`
         * `shipping.address.state`
         * `shipping.address.street1`
         * `shipping.address.street2`
         * `shipping.shipDate`
         * `shipping.trackingNumber`
         * `shipping.trackingURL`

+++

+++ジャーニーキャンバスキーロジック

ジャーニーキャンバスキーロジックでは、特定のイベントを識別し、イベントが発生した後に実行されるアクションを設定する必要があります。

* ジャーニーエントリロジック
   * 製品表示イベント

* 条件
   * 製品が最後に表示されてから、少なくとも 1 つのオンラインまたはオフラインでの購入イベントを確認します。
      * スキーマ：顧客デジタルトランザクション
      * `eventType = commerce.purchases`
      * `timestamp > timestamp of product last viewed`

   * 最後に表示された製品以降に、少なくとも 1 回のオフライン購入を確認します。
      * スキーマ：顧客のオフライントランザクション
      * `eventType = commerce.purchases`
      * `timestamp > timestamp of product last viewed`

   * 条件 – ターゲットチャネルを選択します
      * メール
         * `consents.marketing.email.val = y`
      * プッシュ
         * `consents.marketing.push.val=y`
      * SMS
         * `consents.marketing.sms.val = y`

   * チャネルのパーソナライズ機能
      * 製品ビューに基づいてパーソナライズされたチャネルコンテンツ。

+++

>[!TAB 放棄された買い物かごシナリオ]

放棄された買い物かごシナリオは、買い物かごに入れられたが、web サイトとモバイルアプリの両方でまだ購入されていない製品をターゲットにします。<p>![顧客が買い物かごの放棄シナリオの概要レベルの視覚的な概要。](../intelligent-re-engagement/images/abandoned-cart-journey.png "顧客が買い物かごの放棄シナリオの概要レベルの視覚的な概要。"){width="1920" zoomable="yes"}</p>

+++イベント

イベントを使用すると、ジャーニーをまとめてトリガーし、ジャーニーの過程にある個人にリアルタイムでメッセージを送信できます。イベントの詳細については、 [一般的なイベントガイド](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/about-journey-building/general-events.html).

* イベント 2：買い物かごに追加
   * スキーマ：顧客デジタルトランザクション
   * フィールド :
      * `eventType`
   * 条件：
      * `eventType = commerce.productListAdds`
      * フィールド :
         * `commerce.productListAdds.id`
         * `commerce.productListAdds.value`
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
         * `commerce.cart.cartID`
         * `endUserIDs._experience.emailid.authenticatedState`
         * `endUserIDs._experience.emailid.id`
         * `endUserIDs._experience.emailid.namespace.code`
         * `_id`

* イベント 4：オンライン購入
   * スキーマ：顧客デジタルトランザクション
   * フィールド :
      * `eventType`
   * 条件：
      * `eventType = commerce.purchases`
      * フィールド :
         * `commerce.purchases.id`
         * `commerce.purchases.value`
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

* イベント 3：ブランドエンゲージメント
   * スキーマ：顧客デジタルトランザクション
   * フィールド :
      * `eventType`
   * 条件：
      * `eventType in application.launch, commerce.purchases, web.webpagedetails.pageViews`
      * フィールド :
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
         * `web.webpagedetails.URL`
         * `web.webpagedetails.isHomePage`
         * `web.webpagedetails.name`
         * `endUserIDs._experience.emailid.authenticatedState`
         * `endUserIDs._experience.emailid.id`
         * `endUserIDs._experience.emailid.namespace.code`
         * `_id`
         * `commerce.purchases.id`
         * `commerce.purchases.value`
         * `shipping.address.city`
         * `shipping.address.countryCode`
         * `shipping.address.postalCode`
         * `shipping.address.state`
         * `shipping.address.street1`
         * `shipping.address.street2`
         * `shipping.shipDate`
         * `shipping.trackingNumber`
         * `shipping.trackingURL`

+++

+++ジャーニーキャンバスキーロジック

ジャーニーキャンバスキーロジックでは、特定のイベントを識別し、イベントが発生した後に実行されるアクションを設定する必要があります。

* ジャーニーエントリロジック
   * `AddToCart` イベント

* 認証済みの AuthenticatedState

* 条件：買い物かごが最後に放棄された以降のオフライン購入：
   * スキーマ：顧客のオフライントランザクション
   * `eventType = commerce.purchases`
   * `timestamp > timestamp of cart was last abandoned`

* 条件：買い物かごが最後に放棄された後に買い物かごがクリアされた：
   * スキーマ：顧客デジタルトランザクション
   * `eventType = commerce.cartCleared`
   * `cartID` （カートの ID）
   * `timestamp > timestamp of cart was last abandoned`

* ターゲットチャネルを選択（リーチを広げるには 1 つまたは複数のチャネルを選択）
   * メール
      * `consents.marketing.email.val = y`
   * プッシュ
      * `consents.marketing.push.val = y`
   * SMS
      * `consents.marketing.sms.val = y`
   * チャネルのパーソナライズ機能
      * 買い物かごの詳細情報を表示し、複数の製品をテーブル形式で表示できます。

+++

>[!TAB 注文確認シナリオ]

注文確認シナリオは、web サイトとモバイルアプリを通じて行われた製品の購入に焦点を当てています。<p>![顧客注文確認シナリオの全体的な視覚的な概要。](../intelligent-re-engagement/images/order-confirmation-journey.png "顧客注文確認シナリオの全体的な視覚的な概要。"){width="1920" zoomable="yes"}</p>

+++イベント

イベントを使用すると、ジャーニーをまとめてトリガーし、ジャーニーの過程にある個人にリアルタイムでメッセージを送信できます。イベントの詳細については、 [一般的なイベントガイド](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/about-journey-building/general-events.html).

* イベント 4：オンライン購入
   * スキーマ：顧客デジタルトランザクション
   * フィールド :
      * `eventType`
   * 条件：
      * `eventType = commerce.purchases`
      * フィールド :
         * `commerce.purchases.id`
         * `commerce.purchases.value`
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

+++ジャーニーキャンバスキーロジック

ジャーニーキャンバスキーロジックでは、特定のイベントを識別し、イベントが発生した後に実行されるアクションを設定する必要があります。

* ジャーニーエントリロジック
   * 注文イベント

* 条件
   * ターゲットチャネルを選択します（リーチを広げるには 1 つまたは複数のチャネルを選択します）。
      * 注文確認は本質的に機能していると見なされるので、同意チェックは通常不要です。
      * メール
      * プッシュ
      * SMS

   * チャネルコンテンツのパーソナライゼーション
      * 注文の詳細情報を表示し、テーブル形式を使用して商品のリストを表示できます。

+++

>[!ENDTABS]

でのジャーニーの作成について詳しくは、こちらを参照してください [!DNL Adobe Journey Optimizer]、を読み取ります [ジャーニーガイドの概要](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/journey.html).

### 宛先での有料メディア広告の設定 {#paid-media-ads}

宛先フレームワークは、有料のメディア広告に使用されます。 同意が確認されると、設定された様々な宛先に送信されます。 宛先について詳しくは、を参照してください [宛先の概要](/help/destinations/home.md) ドキュメント。

#### 宛先に必要なデータ

ストリーミングオーディエンス書き出し宛先（Facebook、Google カスタマーマッチ、Google DV360 など）は、顧客データからの様々な ID をサポートします。

* `personalEmail.address`
* `ECID`
* `mobilePhone.number`

放棄された製品参照をアクティブ化し、買い物かごオーディエンスを有料メディア広告に放棄できます。

* ストリーム/トリガー
   * [広告](/help/destinations/catalog/advertising/overview.md)/[ペイドメディアおよびソーシャル](/help/destinations/catalog/social/overview.md)
   * [Mobile](/help/destinations/catalog/mobile-engagement/overview.md)
   * [ストリーミングの宛先](/help/destinations/catalog/streaming/http-destination.md)
   * [Destination SDKを使用して作成されたカスタムの宛先。](/help/destinations/destination-sdk/overview.md)。Real-Time CDP Ultimate をご利用のお客様は、プライベート [Destination SDKを使用したカスタムの宛先](/help/destinations/destination-sdk/overview.md#productized-and-custom-integrations)

## 次の手順 {#next-steps}

コンバージョンを放棄した顧客をインテリジェントで責任ある方法で再エンゲージすることで、コンバージョンが増加し、クライアントのライフタイム価値が高まることを願っています。

次に、Real-Time CDPでサポートされている他のユースケース（例えば、以下のもの）を確認できます [認証されていないユーザーに対するパーソナライズされたコンテンツの表示](/help/rtcdp/partner-data/onsite-personalization.md) web プロパティで。
