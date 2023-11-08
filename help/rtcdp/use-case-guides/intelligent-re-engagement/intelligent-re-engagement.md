---
title: インテリジェントな再エンゲージメント
description: 主要なコンバージョンの瞬間に魅力的でつながりのあるエクスペリエンスを提供し、頻度の少ない顧客をインテリジェントに再エンゲージします。
exl-id: 13f6dbc9-7471-40bf-824d-27922be0d879
source-git-commit: d47ddc474fcaf19eaff8ddcd67139dec5c417720
workflow-type: tm+mt
source-wordcount: '3594'
ht-degree: 6%

---

# 顧客をインテリジェントに再エンゲージして戻す

>[!NOTE]
>
>これはサンプル実装です。このページのセグメント構文などの例は、単なる例に過ぎません。 実装が異なる場合があるので、例をガイドとして使用する必要があります。

インテリジェントで責任ある方法でコンバージョンを放棄した顧客を再び惹きつけます。 経験を持つラップした顧客を惹きつけ、コンバージョンを増やし、クライアントのライフタイムバリューを高めます。

リアルタイムの検討事項を採用し、消費者の品質と行動をすべて考慮に入れ、オンラインとオフラインの両方のイベントに基づいて迅速に再認定をおこないます。

![インテリジェントな再エンゲージメントの概要レベルの視覚的概要。](../intelligent-re-engagement/images/step-by-step.png)

## 使用例の概要 {#overview}

再エンゲージメントシナリオの例を通じて、スキーマ、データセット、オーディエンスを構築します。 また、 [!DNL Adobe Journey Optimizer] 宛先で有料メディア広告を作成するのに必要な広告です。 このガイドでは、以下に概要を示すユースケースジャーニーで、顧客の再エンゲージメントの例を使用します。

* **廃止された製品参照シナリオ** - Web サイトとモバイルアプリの両方で製品の閲覧を中止した顧客をターゲット設定します。
* **放棄された買い物かごのシナリオ**  — 買い物かごに製品を入れたが、Web サイトとモバイルアプリの両方でまだ購入されていない顧客をターゲットにします。
* **注文確認シナリオ** - Web サイトやモバイルアプリを通じた製品の購入に重点を置きます。

## 前提条件と計画 {#prerequisites-and-planning}

このユースケースを実装する手順を完了したら、次のReal-Time CDPおよびAdobe Journey Optimizer機能（使用順に一覧表示）を利用します。 これらすべての領域に必要な[属性ベースのアクセス制御権限](/help/access-control/home.md)があることを確認するか、必要な権限の付与をシステム管理者に依頼してください。

* [[!DNL Adobe Real-Time Customer Data Platform (Real-Time CDP)]](https://experienceleague.adobe.com/docs/platform-learn/tutorials/rtcdp/understanding-the-real-time-customer-data-platform.html)  — 複数のデータソースのデータを統合して、キャンペーンの燃料化をおこないます。 次に、このデータを使用して、キャンペーンオーディエンスを作成し、E メールや Web プロモーションタイルで使用するパーソナライズされたデータ要素（名前やアカウント関連の情報など）を表示します。 また、CDP は、E メールと Web( [!DNL Adobe Target]) をクリックします。
   * [スキーマ](/help/xdm/home.md)
   * [プロファイル](/help/profile/home.md)
   * [データセット](/help/catalog/datasets/overview.md)
   * [オーディエンス](/help/segmentation/home.md)
   * [[!DNL Adobe Journey Optimizer]](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/journey.html?lang=ja)
   * [宛先](/help/destinations/home.md)

* [[!DNL Adobe Journey Optimizer]](https://experienceleague.adobe.com/docs/journey-optimizer-learn/tutorials/introduction-to-journey-optimizer/introduction.html?lang=ja)  — 顧客とのつながり、状況に応じた、パーソナライズされたエクスペリエンスを提供できます。
   * [イベントまたはオーディエンスのトリガー](https://experienceleague.adobe.com/docs/journey-optimizer/using/offer-decisioning/collect-event-data/data-collection.html)
   * [オーディエンス/イベント](https://experienceleague.adobe.com/docs/journey-optimizer/using/audiences-profiles-identities/audiences/about-audiences.html?lang=ja)
   * [ジャーニーアクション](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/journey.html?lang=ja)

## 使用例の達成方法 {#achieve-use-case-instruction}

以下に、3 つの再エンゲージメントシナリオの例の概要を示します。

>[!BEGINTABS]

>[!TAB 廃止された製品参照シナリオ]

廃止された製品参照シナリオは、Web サイトとモバイルアプリの両方で廃止された製品参照をターゲットにします。 このシナリオは、製品が表示されたが、購入されなかった、または買い物かごに追加されなかった場合にトリガーされます。 この例では、過去 24 時間以内にリストが追加されなかった場合、3 日後にブランドエンゲージメントがトリガーされます。<p>![顧客のインテリジェントな廃止された製品ブラウズシナリオの概要レベルのビジュアル概要。](../intelligent-re-engagement/images/re-engagement-journey.png "顧客のインテリジェントな廃止された製品ブラウズシナリオの概要レベルのビジュアル概要。"){width="1920" zoomable="yes"}</p>

1. スキーマとデータセットを作成し、を有効にします。 [!UICONTROL プロファイル].
2. Web SDK、Mobile SDK、または API を使用して、Experience Platformにデータを取り込みます。 Analytics Data Connector も使用できますが、ジャーニーの遅延が発生する可能性があります。
3. 追加のプロファイル対応データを取り込みます。このデータは、ID グラフを使用して、認証済み Web やモバイルアプリの訪問者にリンクできます。
4. プロファイルのリストから焦点を絞ったオーディエンスを作成し、 **顧客** が過去 3 日間に契約を結んでいます
5. 廃止された製品参照ジャーニーは、で作成します。 [!DNL Adobe Journey Optimizer].
6. 必要に応じて、 **データパートナー** 対象の有料メディアの宛先に対するオーディエンスのアクティブ化。
7. [!DNL Adobe Journey Optimizer] は同意を確認し、設定された様々なアクションを送信します。

>[!TAB 放棄された買い物かごのシナリオ]

放棄された買い物かごシナリオは、製品が買い物かごに入れられたが、Web サイトとモバイルアプリの両方でまだ購入されていない場合に適用されます。 また、有料メディアキャンペーンは、この方法を使用して開始および停止されます。<p>![顧客が放棄した買い物かごシナリオの概要レベルの視覚的概要。](../intelligent-re-engagement/images/abandoned-cart-journey.png "顧客が放棄した買い物かごシナリオの概要レベルの視覚的概要。"){width="1920" zoomable="yes"}</p>

1. スキーマとデータセットを作成し、を有効にします。 [!UICONTROL プロファイル].
2. Web SDK、Mobile SDK、または API を使用して、Experience Platformにデータを取り込みます。 Analytics Data Connector も使用できますが、ジャーニーの遅延が発生する可能性があります。
3. 追加のプロファイル対応データを取り込みます。このデータは、ID グラフを使用して、認証済み Web やモバイルアプリの訪問者にリンクできます。
4. プロファイルのリストから焦点を絞ったオーディエンスを作成し、 **顧客** は買い物かごに品目を配置しましたが、まだ購入を完了していません。 The **[!UICONTROL 買い物かごに追加]** イベントは、30 分間待機し、購入を確認するタイマーを開始します。 購入が行われていない場合、 **顧客** が **[!UICONTROL 買い物かごを放棄]** オーディエンス。
5. 次の場所で買い物かごの放棄済みジャーニーを作成します。 [!DNL Adobe Journey Optimizer].
6. 必要に応じて、 **データパートナー** 対象の有料メディアの宛先に対するオーディエンスのアクティブ化。
7. [!DNL Adobe Journey Optimizer] は同意を確認し、設定された様々なアクションを送信します。

>[!TAB 注文確認シナリオ]

注文確認シナリオでは、Web サイトおよびモバイルアプリを通じた製品の購入に焦点を当てます。<p>![顧客注文確認シナリオの概要レベルの視覚的概要。](../intelligent-re-engagement/images/order-confirmation-journey.png "顧客注文確認シナリオの概要レベルの視覚的概要。"){width="1920" zoomable="yes"}</p>

1. スキーマとデータセットを作成し、を有効にします。 [!UICONTROL プロファイル].
2. Web SDK、Mobile SDK、または API を使用して、Experience Platformにデータを取り込みます。 Analytics Data Connector も使用できますが、ジャーニーの遅延が発生する可能性があります。
3. 追加のプロファイル対応データを取り込みます。このデータは、ID グラフを使用して、認証済み Web やモバイルアプリの訪問者にリンクできます。
4. 確認ジャーニーは、で作成します。 [!DNL Adobe Journey Optimizer].
5. [!DNL Adobe Journey Optimizer] は、優先チャネルを使用して注文確認メッセージを送信します。

>[!ENDTABS]

上記の概要に記載されている各手順を完了するには、以下の節をお読みください。詳細情報と詳細な手順へのリンクが用意されています。

### スキーマを作成してフィールドグループを指定する {#schema-design}

Experience Data Model(XDM) のリソースは、 [!UICONTROL スキーマ] のワークスペース [!DNL Adobe Experience Platform]. 次のツールで提供されるコアリソースを表示および調査できます。 [!DNL Adobe] （フィールドグループなど）を作成し、組織のカスタムリソースおよびスキーマを作成します。

作成に関する詳細 [スキーマ](/help/xdm/home.md)を参照し、 [スキーマの作成に関するチュートリアル](/help/xdm/tutorials/create-schema-ui.md) および [XDM を使用した顧客体験データのモデル化](https://experienceleague.adobe.com/docs/courses/using/experienceplatform-d-1-2021-1-xdm.html).

再エンゲージメントの使用例には、4 つのスキーマデザインが使用されます。 各スキーマでは、特定のフィールドを設定する必要があります。 スキーマをリアルタイム顧客プロファイルに含めるには、有効にする必要があります。 リアルタイム顧客プロファイルでスキーマの使用を有効にする方法について詳しくは、 [リアルタイム顧客プロファイルのスキーマの有効化](/help/xdm/ui/resources/schemas.md#enable-a-schema-for-real-time-customer-profile).

#### 顧客属性スキーマ

このスキーマは、顧客情報を構成するプロファイルデータを構造化し、参照するために使用されます。 このデータは通常、 [!DNL Adobe Experience Platform] CRM または類似のシステムを使用し、パーソナライゼーション、マーケティング同意、強化されたオーディエンス機能に使用される顧客の詳細を参照するために必要です。

顧客属性スキーマは、 [[!UICONTROL XDM 個人プロファイル]](/help/xdm/classes/individual-profile.md) クラス。次のフィールドグループが含まれます。

+++個人の連絡先の詳細（フィールドグループ）

[個人の連絡先の詳細](/help/xdm/field-groups/profile/personal-contact-details.md) は、個人の連絡先情報を記述する XDM Individual Profile クラスの標準スキーマフィールドグループです。

| フィールド | 要件 | 説明 |
| --- | --- | --- |
| `mobilePhone.number` | 必須 | SMS に使用される人の携帯電話番号。 |
| `personalEmail.address` | 必須 | 人物の電子メールアドレス。 |

+++

+++外部ソースシステム監査の詳細（フィールドグループ）

[外部ソースシステム監査属性](/help/xdm/data-types/external-source-system-audit-attributes.md) は、外部ソースシステムに関する監査の詳細を取り込む、標準の Experience Data Model(XDM) データ型です。

+++

+++同意および環境設定フィールドグループ（フィールドグループ）

The [同意および環境設定](/help/xdm/field-groups//profile/consents.md) フィールドグループは、同意および環境設定情報を取り込むための単一のオブジェクトタイプフィールド（同意）を提供します。

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

このフィールドグループは、ベストプラクティスに使用されます。

+++

#### 顧客のデジタルトランザクションスキーマ

このスキーマは、Web サイトや関連するデジタルプラットフォームで発生する顧客アクティビティを構成するイベントデータの構造化および参照に使用されます。 このデータは通常、 [!DNL Adobe Experience Platform] 経由 [Web SDK](/help/edge/home.md) とは、ジャーニーのトリガー、詳細なオンライン顧客分析、拡張オーディエンス機能に使用される様々な参照イベントやコンバージョンイベントを参照するために必要です。

顧客のデジタルトランザクションスキーマは、 [[!UICONTROL XDM ExperienceEvent]](/help/xdm/classes/experienceevent.md) クラス。

+++XDM ExperienceEvent （クラス）

| フィールド | 要件 | 説明 |
| --- | --- | --- |
| `_id` | 必須 | に取り込まれる個々のイベントを一意に識別します。 [!DNL Adobe Experience Platform]. |
| `timestamp` | 必須 | イベントが発生した時点の ISO 8601 タイムスタンプ（RFC 3339 Section 5.6 に従って形式設定）。このタイムスタンプは過去に設定する必要があります。 |
| `eventType` | 必須 | イベントのカテゴリのタイプを示す文字列。 |

+++

The [[!UICONTROL XDM ExperienceEvent]](/help/xdm/classes/experienceevent.md) クラスには、次のフィールドグループが含まれます。

+++エンドユーザー ID の詳細（フィールドグループ）

The [エンドユーザー ID の詳細](/help/xdm/field-groups/event/enduserids.md) フィールドグループは、複数のアプリケーションをまたいで個人の id 情報を表すために使用されるAdobeです。

| フィールド | 要件 | 説明 |
| --- | --- | --- |
| `endUserIDs._experience.emailid.authenticatedState` | 必須 | エンドユーザーの電子メールアドレス ID 認証状態。 |
| `endUserIDs._experience.emailid.id` | 必須 | エンドユーザーの電子メールアドレス ID。 |
| `endUserIDs._experience.emailid.namespace.code` | 必須 | エンドユーザーの電子メールアドレス ID 名前空間コード。 |
| `endUserIDs._experience.mcid.authenticatedState` | 必須 | [!DNL Adobe] Marketing CloudID(MCID) 認証済み状態。 MCID は、Experience CloudID(ECID) と呼ばれます。 |
| `endUserIDs._experience.mcid.id` | 必須 | [!DNL Adobe] Marketing CloudID(MCID)。 MCID は、Experience CloudID(ECID) と呼ばれます。 |
| `endUserIDs._experience.mcid.namespace.code` | 必須 | [!DNL Adobe] Marketing CloudID(MCID) 名前空間コード。 |

+++

+++外部ソースシステム監査の詳細（フィールドグループ）

External Source System Audit Attributes は、外部ソースシステムに関する監査の詳細を取り込む、標準の Experience Data Model(XDM) データ型です。

+++

#### 顧客オフライントランザクションスキーマ

このスキーマは、Web サイト外のプラットフォームで発生する顧客アクティビティを構成するイベントデータの構造化および参照に使用します。 このデータは通常、 [!DNL Adobe Experience Platform] を POS（または類似のシステム）から取得し、最も多くの場合、API 接続を介して Platform にストリーミングします。 ジャーニーのトリガー、深いオンラインとオフラインの顧客分析、高度なオーディエンス機能の利用に使用される様々なオフラインコンバージョンイベントを参照することが目的です。

顧客オフライントランザクションスキーマは、 [[!UICONTROL XDM ExperienceEvent]](/help/xdm/classes/experienceevent.md) クラス。

+++XDM ExperienceEvent （クラス）

| フィールド | 要件 | 説明 |
| --- | --- | --- |
| `_id` | 必須 | に取り込まれる個々のイベントを一意に識別します。 [!DNL Adobe Experience Platform]. |
| `timestamp` | 必須 | イベントが発生した時点の ISO 8601 タイムスタンプ（RFC 3339 Section 5.6 に従って形式設定）。このタイムスタンプは過去に設定する必要があります。 |
| `eventType` | 必須 | イベントのカテゴリのタイプを示す文字列。 |

+++

The [[!UICONTROL XDM ExperienceEvent]](/help/xdm/classes/experienceevent.md) クラスには、次のフィールドグループが含まれます。

+++コマースの詳細（フィールドグループ）

The [コマースの詳細](/help/xdm/field-groups/event/commerce-details.md) フィールドグループは、商品情報（SKU、名前、数量）や標準的な買い物かご操作（注文、チェックアウト、放棄）などのコマースデータを表すために使用します。

| フィールド | 要件 | 説明 |
| --- | --- | --- |
| `commerce.cart.cartID` | 必須 | 買い物かごの ID。 |
| `commerce.order.orderType` | 必須 | 製品の注文タイプを表すオブジェクト。 |
| `commerce.order.payments.paymentAmount` | 必須 | 製品注文の支払い額を表すオブジェクトです。 |
| `commerce.order.payments.paymentType` | 必須 | 製品注文の支払いタイプを表すオブジェクトです。 |
| `commerce.order.payments.transactionID` | 必須 | オブジェクト製品注文トランザクション ID。 |
| `commerce.order.purchaseID` | 必須 | オブジェクトの商品注文の購入 ID。 |
| `productListItems.name` | 必須 | 顧客が選択した商品を表す項目名のリスト。 |
| `productListItems.priceTotal` | 必須 | 顧客が選択した製品を表す項目のリストの合計価格。 |
| `productListItems.product` | 必須 | 選択した製品。 |
| `productListItems.quantity` | 必須 | 顧客が選択した製品を表す項目のリストの数。 |

+++

+++個人の連絡先の詳細（フィールドグループ）

[個人の連絡先の詳細](/help/xdm/field-groups/profile/personal-contact-details.md) は、個人の連絡先情報を記述する XDM Individual Profile クラスの標準スキーマフィールドグループです。

| フィールド | 要件 | 説明 |
| --- | --- | --- |
| `mobilePhone.number` | 必須 | SMS に使用される人の携帯電話番号。 |
| `personalEmail.address` | 必須 | 人物の電子メールアドレス。 |

+++

+++外部ソースシステム監査の詳細（フィールドグループ）

External Source System Audit Attributes は、外部ソースシステムに関する監査の詳細を取り込む、標準の Experience Data Model(XDM) データ型です。

+++

#### AdobeWeb コネクタのスキーマ

>[!NOTE]
>
>これは、 [[!DNL Adobe Analytics Source Connector]](/help/sources/connectors/adobe-applications/analytics.md).

このスキーマは、Web サイトや関連するデジタルプラットフォームで発生する顧客アクティビティを構成するイベントデータの構造化および参照に使用されます。 このスキーマは、顧客デジタルトランザクションスキーマに似ていますが、このスキーマは、顧客デジタルトランザクションスキーマが [Web SDK](/help/edge/home.md) はデータ収集のオプションではありません。したがって、このスキーマは、 [!DNL Adobe Analytics Source Connector] オンラインデータをに送信するには、以下を実行します。 [!DNL Adobe Experience Platform] プライマリまたはセカンダリのデータストリームとして。

The [!DNL Adobe] web コネクタスキーマは、 [[!UICONTROL XDM ExperienceEvent]](/help/xdm/classes/experienceevent.md) クラス。

+++XDM ExperienceEvent （クラス）

| フィールド | 要件 | 説明 |
| --- | --- | --- |
| `_id` | 必須 | に取り込まれる個々のイベントを一意に識別します。 [!DNL Adobe Experience Platform]. |
| `timestamp` | 必須 | イベントが発生した時点の ISO 8601 タイムスタンプ（RFC 3339 Section 5.6 に従って形式設定）。このタイムスタンプは過去に設定する必要があります。 |
| `eventType` | 必須 | イベントのカテゴリのタイプを示す文字列。 |

+++

The [[!UICONTROL XDM ExperienceEvent]](/help/xdm/classes/experienceevent.md) クラスには、次のフィールドグループが含まれます。

+++Adobe Analytics ExperienceEvent テンプレート（フィールドグループ）

The [Adobe Analytics ExperienceEvent](/help/xdm/field-groups/event/analytics-full-extension.md) フィールドグループは、Adobe Analyticsが収集する一般的な指標を取り込みます。

| フィールド | 要件 | 説明 |
| --- | --- | --- |
| `endUserIDs._experience.emailid.authenticatedState` | 必須 | エンドユーザーの電子メールアドレス ID 認証状態。 |
| `endUserIDs._experience.emailid.id` | 必須 | エンドユーザーの電子メールアドレス ID。 |
| `endUserIDs._experience.emailid.namespace.code` | 必須 | エンドユーザーの電子メールアドレス ID 名前空間コード。 |
| `endUserIDs._experience.mcid.authenticatedState` | 必須 | [!DNL Adobe] Marketing CloudID(MCID) 認証済み状態。 MCID は、Experience CloudID(ECID) と呼ばれます。 |
| `endUserIDs._experience.mcid.id` | 必須 | [!DNL Adobe] Marketing CloudID(MCID)。 MCID は、Experience CloudID(ECID) と呼ばれます。 |
| `endUserIDs._experience.mcid.namespace.code` | 必須 | [!DNL Adobe] Marketing CloudID(MCID) 名前空間コード。 |

+++

+++外部ソースシステム監査の詳細（フィールドグループ）

External Source System Audit Attributes は、外部ソースシステムに関する監査の詳細を取り込む、標準の Experience Data Model(XDM) データ型です。

+++

### スキーマからのデータセットの作成 {#create-datasets}

データセットは、データのグループのストレージと管理の構造です。 インテリジェントな再エンゲージメントシナリオの各スキーマには、独自のデータセットが必要です。

を作成する方法の詳細については、 [データセット](/help/catalog/datasets/overview.md) スキーマから、 [データセット UI ガイド](/help/catalog/datasets/user-guide.md).

>[!NOTE]
>
>スキーマを作成する手順と同様に、データセットをリアルタイム顧客プロファイルに含める必要があります。 リアルタイム顧客プロファイルでデータセットを使用できるようにする方法について詳しくは、 [データをリアルタイム顧客プロファイルに取り込む](https://experienceleague.adobe.com/docs/platform-learn/tutorials/profiles/bring-data-into-the-real-time-customer-profile.html?lang=ja).

### 同意とデータガバナンス {#privacy-consent}

>[!IMPORTANT]
>
>お客様に、ブランドからの通信を購読解除する機能を提供し、この選択を確実に受け入れることが法的要件です。 該当する法律について詳しくは、 [プライバシー規制の概要](https://experienceleague.adobe.com/docs/experience-platform/privacy/regulations/overview.html).

#### 同意ポリシー

再エンゲージメントパスを作成する場合は、次の追加を検討してください。 [同意ポリシー](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/consent/overview.html):

* 次の場合 `consents.marketing.email.val = "Y"` その後、電子メールを送信可能
* 次の場合 `consents.marketing.sms.val = "Y"` その後、SMS を送信可能
* 次の場合 `consents.marketing.push.val = "Y"` 次にプッシュ可能
* 次の場合 `consents.share.val = "Y"` 次に、広告を作成可能

#### データガバナンスのラベル付けと実施

再エンゲージメントパスを作成する場合は、次の追加を検討してください。 [データガバナンスラベル](/help/data-governance/labels/overview.md):

* 個人の電子メールアドレスは、デバイスではなく特定の個人を識別したり、特定の個人と連絡を取り合うために使用される、直接識別可能なデータとして使用されます。
   * `personalEmail.address = I1`

#### データ使用ポリシー

次の項目はありません： [データ使用ポリシー](/help/data-governance/policies/overview.md) 廃止された製品参照シナリオに必要です。 ただし、次の点を考慮する必要があります。

* 機密データの制限
* オンサイト広告の制限
* メールのターゲティングを制限
* クロスサイトターゲティングの制限
* 直接識別可能なデータと匿名データの組み合わせを制限

### オーディエンスを作成 {#create-audience}

再エンゲージメントのシナリオでは、オーディエンスを使用して、プロファイルストアのプロファイルのサブセットによって共有される特定の属性や行動を定義し、マーケティング可能な人々のグループを顧客ベースから区別します。 オーディエンスは、 [!DNL Adobe Experience Platform].

オーディエンスの作成方法について詳しくは、 [audience service UI ガイド](https://experienceleague.adobe.com/docs/experience-platform/segmentation/ui/overview.html#create-audience).

直接作成する方法の詳細 [オーディエンス](/help/segmentation/home.md)を読む [Audience Composition UI ガイド](/help/segmentation/ui/audience-composition.md).

Platform 派生オーディエンス定義を使用してオーディエンスを構築する方法について詳しくは、 [Audience Builder UI ガイド](/help/segmentation/ui/segment-builder.md).

>[!BEGINTABS]

>[!TAB 廃止された製品参照シナリオ]

このオーディエンスは、従来の「買い物かごの放棄」シナリオの強化として作成されます。 買い物かごの放棄では通常、特定の期間内に後続の購入をおこなわずに買い物かごへの追加に注力しますが、このオーディエンスは、特定の製品を閲覧したが買い物かごに追加せず、特定の期間内にサイトでフォローアップアクティビティをおこなわなかった場合に、 このオーディエンスは、この包含条件を満たす顧客のブランドを「最優先」に保ち、従来の e コマースモデルとは異なるデジタルプロパティを持つ顧客のためにも利用できます。

+++過去 3 日間のエンゲージメントなしで離脱した製品表示

次のイベントは、離脱した製品参照シナリオで使用され、ユーザーが製品をオンラインで表示し、次の 3 日後に（サイト訪問、アプリ訪問、オンライン購入、オフライン購入、買い物かごイベントへの追加）を関与しませんでした。

このオーディエンスを設定する際は、次のフィールドと条件が必要です。

* `eventType: commerce.productViews`
* および `THEN` （順次イベント）次を除外 `eventType: commerce.procuctListAdds` または `application.launch` または `web.webpagedetails.pageViews` または `commerce.purchases` （オンラインとオフラインの両方が含まれます）。
   * `Timestamp: > 3 days after productView`

+++

+++過去 3 日間のエンゲージメントを含む製品表示

次のイベントは、廃止された製品参照シナリオで使用され、ユーザーが製品をオンラインで表示し、次の 3 日後に（サイト訪問、アプリ訪問、オンライン購入、オフライン購入、買い物かごへの追加）をエンゲージしました。

このオーディエンスを設定する際は、次のフィールドと条件が必要です。

* `eventType: commerce.productViews`
* および `THEN` （順次イベント）次を含めます。 `eventType: commerce.procuctListAdds` または `application.launch` または `web.webpagedetails.pageViews` または `commerce.purchases` （オンラインとオフラインの両方が含まれます）。
   * `Timestamp: > 3 days after productView`

+++過去 1 日のエンゲージメントストリーミング

次のイベントは、過去 1 日にユーザーが関与した廃止された製品参照シナリオで使用されます（サイト訪問、アプリ訪問、オンライン購入、オフライン購入、買い物かごへの追加）。

このオーディエンスを設定する際は、次のフィールドと条件が必要です。

* `eventType: commerce.procuctListAdds or application.launch or web.webpagedetails.pageViews or commerce.purchases`
   * `Timestamp: in last 1 day` (ストリーミング)

+++

+++過去 3 日間のエンゲージメントバッチ

次のイベントは、過去 3 日間にユーザーが関与した廃止された製品参照シナリオで使用されます（サイト訪問、アプリ訪問、オンライン購入、オフライン購入、買い物かごへの追加）。

このオーディエンスを設定する際は、次のフィールドと条件が必要です。

* `EventType: commerce.procuctListAdds or application.launch or web.webpagedetails.pageViews or commerce.purchases`
   * `Timestamp: in last 3 days` (バッチ)

+++

>[!TAB 放棄された買い物かごのシナリオ]

このオーディエンスは、従来の「買い物かごの放棄」シナリオをサポートするために作成されています。 その目的は、買い物かごに商品を追加したが、最終的に購入に従わなかった顧客を見つけることです。 このオーディエンスは、顧客のブランドの「トップオブマインド」だけでなく、顧客が後で購入せずに残した製品も保つのに役立ちます。

次のイベントは、1 ～ 4 日前にユーザーが買い物かごに製品を追加したが、購入を完了しなかった、または買い物かごをクリアしなかった、放棄された買い物かごシナリオで使用されます。

このオーディエンスを設定する際は、次のフィールドと条件が必要です。

* `eventType: commerce.productListAdds`
   * `Timestamp: >= 1 days before now and <= 4 days before now `
* `eventType: commerce.purchases`
   * `Timestamp: <= 4 days before now`
* `eventType: commerce.productListRemovals`
   * `Timestamp: <= 4 days before now`

放棄された買い物かごシナリオの記述子は次のように表示されます。

`Include eventType = commerce.productListAdds between 30 min and 1440 minutes before now. exclude eventType = commerce.purchases 30 minutes before now OR eventType = commerce.productListRemovals AND Cart ID equals Product List Adds1 Cart ID (the inclusion event).`

>[!TAB 注文確認シナリオ]

このジャーニーでは、オーディエンスを作成する必要はありません。

>[!ENDTABS]

### Adobe Journey Optimizerでのジャーニーの設定 {#journey-setup}

>[!NOTE]
>
>[!DNL Adobe Journey Optimizer] では、図に表示されるすべての項目を網羅していません。 すべて [有料メディア広告](/help/destinations/catalog/social/overview.md) 次で作成： [!UICONTROL 宛先].

[[!DNL Adobe Journey Optimizer]](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/journey.html?lang=ja) は、顧客とのつながり、コンテキストに沿ったパーソナライズされたエクスペリエンスを提供するのに役立ちます。 カスタマージャーニーとは、顧客がブランドとやり取りするプロセス全体を指します。 各ユースケースのジャーニーには、具体的な情報が必要です。 以下に、各ジャーニーに必要な正確なデータを示します。

>[!BEGINTABS]

>[!TAB 廃止された製品参照シナリオ]

廃止された製品参照シナリオは、Web サイトとモバイルアプリの両方で廃止された製品参照をターゲットにします。<p>![顧客が放棄した製品閲覧シナリオの概要レベルの視覚概要。](../intelligent-re-engagement/images/re-engagement-journey.png "顧客が放棄した製品閲覧シナリオの概要レベルの視覚概要。"){width="1920" zoomable="yes"}</p>

+++イベント

* イベント 1：製品表示
   * スキーマ：顧客のデジタルトランザクション
   * フィールド:
      * `eventType`
   * 条件:
      * `eventType = commerce.productViews`
      * フィールド:
         * `commerce.productViews.id`
         * `commerce.productViews.value`
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

* イベント 2：買い物かごに追加
   * スキーマ：顧客のデジタルトランザクション
   * フィールド:
      * `eventType`
   * 条件:
      * `eventType = commerce.productListAdds`
      * フィールド:
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
   * スキーマ：顧客のデジタルトランザクション
   * フィールド:
      * `eventType`
   * 条件:
      * `eventType in application.launch, commerce.purchases, web.webpagedetails.pageViews`
      * フィールド:
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

+++キージャーニーロジック

* ジャーニー入口ロジック
   * 製品表示イベント

* 条件
   * 製品が最後に表示されてから、少なくとも 1 つのオンラインまたはオフラインの購入イベントを確認します。
      * スキーマ：顧客のデジタルトランザクション
      * `eventType = commerce.purchases`
      * `timestamp > timestamp of product last viewed`

   * 製品が最後に閲覧されてから少なくとも 1 回のオフライン購入を確認してください：
      * スキーマ：顧客オフライントランザクション
      * `eventType = commerce.purchases`
      * `timestamp > timestamp of product last viewed`

   * 条件 — ターゲットチャネルの選択
      * メール
         * `consents.marketing.email.val = y`
      * プッシュ
         * `consents.marketing.push.val=y`
      * SMS
         * `consents.marketing.sms.val = y`

   * チャネルのパーソナライズ
      * 製品表示に基づいてパーソナライズされたチャネルコンテンツ。

+++

>[!TAB 放棄された買い物かごのシナリオ]

放棄された買い物かごシナリオは、買い物かごに入れられたが、Web サイトとモバイルアプリの両方でまだ購入されていない製品をターゲットにします。<p>![顧客が放棄した買い物かごシナリオの概要レベルの視覚的概要。](../intelligent-re-engagement/images/abandoned-cart-journey.png "顧客が放棄した買い物かごシナリオの概要レベルの視覚的概要。"){width="1920" zoomable="yes"}</p>

+++イベント

* イベント 2：買い物かごに追加
   * スキーマ：顧客のデジタルトランザクション
   * フィールド:
      * `eventType`
   * 条件:
      * `eventType = commerce.productListAdds`
      * フィールド:
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
   * スキーマ：顧客のデジタルトランザクション
   * フィールド:
      * `eventType`
   * 条件:
      * `eventType = commerce.purchases`
      * フィールド:
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
   * スキーマ：顧客のデジタルトランザクション
   * フィールド:
      * `eventType`
   * 条件:
      * `eventType in application.launch, commerce.purchases, web.webpagedetails.pageViews`
      * フィールド:
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

+++キージャーニーロジック

* ジャーニー入口ロジック
   * `AddToCart` イベント

* 認証済みの AuthenticatedState

* 条件：買い物かごが最後に破棄された後のオフライン購入：
   * スキーマ：顧客オフライントランザクション
   * `eventType = commerce.purchases`
   * `timestamp > timestamp of cart was last abandoned`

* 条件：買い物かごが最後に破棄されてからクリアされた買い物かご：
   * スキーマ：顧客のデジタルトランザクション
   * `eventType = commerce.cartCleared`
   * `cartID` （買い物かごの ID）
   * `timestamp > timestamp of cart was last abandoned`

* ターゲットチャネルを選択（広い範囲に到達するには 1 つまたは複数のチャネルを選択）
   * メール
      * `consents.marketing.email.val = y`
   * プッシュ
      * `consents.marketing.push.val = y`
   * SMS
      * `consents.marketing.sms.val = y`
   * チャネルのパーソナライズ
      * 買い物かごの詳細情報を表示し、複数の製品を表形式で表示できます。

+++

>[!TAB 注文確認シナリオ]

注文確認シナリオでは、Web サイトおよびモバイルアプリを通じた製品の購入に焦点を当てます。<p>![顧客注文確認シナリオの概要レベルの視覚的概要。](../intelligent-re-engagement/images/order-confirmation-journey.png "顧客注文確認シナリオの概要レベルの視覚的概要。"){width="1920" zoomable="yes"}</p>

+++イベント

* イベント 4：オンライン購入
   * スキーマ：顧客のデジタルトランザクション
   * フィールド:
      * `eventType`
   * 条件:
      * `eventType = commerce.purchases`
      * フィールド:
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

+++キージャーニーロジック

* ジャーニー入口ロジック
   * 注文イベント

* 条件
   * 「ターゲットチャネル」を選択します（広い範囲に到達するには 1 つまたは複数のチャネルを選択します）。
      * 注文の確認は本質的に役立つと考えられるので、通常、同意の確認は不要です。
      * メール
      * プッシュ
      * SMS

   * チャネルコンテンツのパーソナライズ
      * 注文の詳細情報を表示し、表形式を使用して製品のリストを表示できます。

+++

>[!ENDTABS]

でのジャーニー作成の詳細 [!DNL Adobe Journey Optimizer]を読む [ジャーニーの概要ガイド](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/journey.html?lang=ja).

### 宛先での有料メディア広告の設定 {#paid-media-ads}

宛先フレームワークは、有料メディア広告に使用されます。 同意が確認されると、設定された様々な宛先に送信されます。 宛先について詳しくは、 [宛先の概要](/help/destinations/home.md) 文書。

#### 宛先に必要なデータ

ストリーミングオーディエンスの書き出し先 (Facebook、Google Customer Match、Google DV360 など ) は、顧客データからの様々な ID をサポートします。

* `personalEmail.address`
* `ECID`
* `mobilePhone.number`

放棄買い物かごオーディエンスはストリーミングオーディエンスとして評価されるので、この使用例の宛先フレームワークで使用できます。

* ストリーム/トリガー済み
   * [広告](/help/destinations/catalog/advertising/overview.md)/[Paid Media &amp; Social](/help/destinations/catalog/social/overview.md)
   * [Mobile](/help/destinations/catalog/mobile-engagement/overview.md)
   * [ストリーミング先](/help/destinations/catalog/streaming/http-destination.md)
   * [Destination SDKを使用して作成されたカスタムの宛先。](/help/destinations/destination-sdk/overview.md)。Real-Time CDP Ultimate のお客様は、プライベート [Destination SDKを使用したカスタム宛先](/help/destinations/destination-sdk/overview.md#productized-and-custom-integrations)
