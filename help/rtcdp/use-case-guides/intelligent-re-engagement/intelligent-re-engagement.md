---
title: インテリジェントな再エンゲージメント
description: コンバージョンの重要な瞬間に、説得力のあるつながりのあるエクスペリエンスを提供し、まれな顧客をインテリジェントに再エンゲージします。
hide: true
hidefromtoc: true
source-git-commit: 43e365e20a2fd91a0e822eb6f66bb7db6fc218f5
workflow-type: tm+mt
source-wordcount: '2934'
ht-degree: 5%

---

# 顧客をインテリジェントに再び関与させて再来訪させる

インテリジェントで責任ある方法でコンバージョンを完了する前に、コンバージョンを放棄したクライアントを再び関与させます。 コンバージョンを強化し、クライアントのライフタイムバリューの増加を促すために、リマインダーではなく、エクスペリエンスを通じて顧客との関わりを深めました。

リアルタイムの検討事項を採用し、消費者の品質と行動をすべて考慮に入れ、オンラインとオフラインの両方のイベントに基づいて迅速に再認定をおこないます。

![インテリジェントな再エンゲージメントの概要レベルの視覚的概要を順を追って説明します。](../intelligent-re-engagement/images/step-by-step.png)

## 使用例の概要

再エンゲージメントジャーニーの例を通じて、スキーマ、データセット、オーディエンスを構築します。 また、 [!DNL Adobe Journey Optimizer] 宛先で有料メディア広告を作成するのに必要な広告です。 このガイドでは、以下に概要を示すユースケースジャーニーで、顧客の再エンゲージメントの例を使用します。

* **再エンゲージメントのジャーニー** - Web サイトとモバイルアプリの両方で製品の閲覧を中止した顧客をターゲットにします。
* **離脱した買い物かごのジャーニー**  — 買い物かごに商品を入れたが、Web サイトとモバイルアプリの両方でまだ購入されていない顧客をターゲットにします。
* **注文確認のジャーニー** :Web サイトやモバイルアプリを通じた製品の購入に重点を置きます。

## 前提条件と計画 {#prerequisites-and-planning}

このユースケースを実装する手順を完了したら、次のReal-Time CDP機能と UI 要素（使用順に一覧表示）を利用します。 これらの領域に必要な属性ベースのアクセス制御権限を持っていることを確認するか、必要な権限をシステム管理者に与えてもらってください。

* [ADOBE REAL-TIME CUSTOMER DATA PLATFORM(REAL-TIME CDP)](https://experienceleague.adobe.com/docs/platform-learn/tutorials/rtcdp/understanding-the-real-time-customer-data-platform.html)  — 複数のデータソースをまたいでデータを集計し、キャンペーンに燃料を供給します。 次に、このデータを使用して、キャンペーンオーディエンスを作成し、E メールや Web プロモーションタイルで使用するパーソナライズされたデータ要素（名前やアカウント関連の情報など）を表示します。 また、CDP は、(Adobe Targetを介して ) 電子メールと Web にまたがってオーディエンスをアクティブ化するためにも使用されます。
   * [スキーマ](/help/xdm/home.md)
   * [プロファイル](/help/profile/home.md)
   * [データセット](/help/catalog/datasets/overview.md)
   * [オーディエンス](/help/segmentation/home.md)
   * [Adobe Journey Optimizer](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/journey.html?lang=ja)
   * [宛先](/help/destinations/home.md)
   * [イベントまたはオーディエンスのトリガー](https://experienceleague.adobe.com/docs/journey-optimizer/using/offer-decisioning/collect-event-data/data-collection.html)
   * [オーディエンス/イベント](https://experienceleague.adobe.com/docs/journey-optimizer/using/audiences-profiles-identities/audiences/about-audiences.html)
   * [ジャーニーアクション](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/journey.html?lang=ja)

### ユースケースの達成方法：おおまかな概要 {#achieve-the-use-case-high-level}

以下に、3 つの再エンゲージメントジャーニーの例の概要を示します。

>[!BEGINTABS]

>[!TAB 再エンゲージメントジャーニー]

再エンゲージメントのジャーニーは、Web サイトとモバイルアプリの両方で製品の閲覧を中止したことをターゲットにします。 このジャーニーは、製品が表示されたが、購入されなかった、または買い物かごに追加されなかった場合にトリガーされます。 過去 24 時間以内にリストが追加されなかった場合、ブランドエンゲージメントは 3 日後にトリガーされます。<p>![顧客インテリジェントな再エンゲージメントジャーニーの概要を視覚的に説明します。](../intelligent-re-engagement/images/re-engagement-journey.png "顧客インテリジェントな再エンゲージメントジャーニーの概要を視覚的に説明します。"){width="1920" zoomable="yes"}</p>

1. 用にマークされたスキーマとデータセットを作成します [!UICONTROL プロファイル].
2. データは、Web SDK、Mobile Edge SDK、または API を介してExperience Platformに集計されます。 Analytics Data Connector も使用できますが、ジャーニーの遅延が発生する可能性があります。
3. プロファイルをReal-Time CDPに読み込み、ガバナンスポリシーを構築して、責任を持って使用するようにします。
4. プロファイルのリストから焦点を絞ったオーディエンスを作成し、 **顧客** が過去 3 日間に契約を結んでいます
5. 再エンゲージメントのジャーニーは、で作成します。 [!DNL Adobe Journey Optimizer].
6. 必要に応じて、 **データパートナー** 対象の有料メディアの宛先に対するオーディエンスのアクティブ化。
7. [!DNL Adobe Journey Optimizer] は同意を確認し、設定された様々なアクションを送信します。

>[!TAB 放棄された買い物かごのジャーニー]

廃止された買い物かごジャーニーは、買い物かごに入れられたが、Web サイトとモバイルアプリの両方でまだ購入されていない製品をターゲットにします。 また、有料メディアキャンペーンは、この方法を使用して開始および停止されます。<p>![顧客が離脱した買い物かごのジャーニーの概要を視覚的に説明します。](../intelligent-re-engagement/images/abandoned-cart-journey.png "顧客が離脱した買い物かごのジャーニーの概要を視覚的に説明します。"){width="1920" zoomable="yes"}</p>

1. 用にマークされたスキーマとデータセットを作成します [!UICONTROL プロファイル].
2. データは、Web SDK、Mobile Edge SDK、または API を介してExperience Platformに集計されます。 Analytics Data Connector も使用できますが、ジャーニーの遅延が発生する可能性があります。
3. プロファイルをReal-Time CDPに読み込み、ガバナンスポリシーを構築して、責任を持って使用するようにします。
4. プロファイルのリストから焦点を絞ったオーディエンスを作成し、 **顧客** は買い物かごに品目を配置しましたが、まだ購入を完了していません。 The **[!UICONTROL 買い物かごに追加]** イベントは、30 分間待機し、購入を確認するタイマーを開始します。 購入が行われていない場合、 **顧客** が **[!UICONTROL 買い物かごを放棄]** オーディエンス。
5. Adobe Journey Optimizerで放棄された買い物かごのジャーニーを作成する
6. 必要に応じて、 **データパートナー** 対象の有料メディアの宛先に対するオーディエンスのアクティブ化。
7. [!DNL Adobe Journey Optimizer] は同意を確認し、設定された様々なアクションを送信します。

>[!TAB 注文確認ジャーニー]

注文確認ジャーニーは、Web サイトやモバイルアプリを通じておこなわれた製品の購入に重点を置いています。<p>![顧客の注文確認ジャーニーの概要レベルの視覚的な概要。](../intelligent-re-engagement/images/order-confirmation-journey.png "顧客の注文確認ジャーニーの概要レベルの視覚的な概要。"){width="1920" zoomable="yes"}</p>

1. 用にマークされたスキーマとデータセットを作成します [!UICONTROL プロファイル].
2. データは、Web SDK、Mobile Edge SDK、または API を介してExperience Platformに集計されます。 Analytics Data Connector も使用できますが、ジャーニーの遅延が発生する可能性があります。
3. プロファイルをReal-Time CDPに読み込み、ガバナンスポリシーを構築して、責任を持って使用するようにします。
4. プロファイルのリストから焦点を絞ったオーディエンスを作成し、 **顧客** が購入されました。
5. 確認ジャーニーは、Adobe Journey Optimizerで作成します。
6. [!DNL Adobe Journey Optimizer] は、優先チャネルを使用して注文確認メッセージを送信します。

>[!ENDTABS]

## ユースケースの達成方法：手順 {#step-by-step-instructions}

上記の概要に記載されている各手順を完了するには、以下の節をお読みください。詳細情報と詳細な手順へのリンクが用意されています。

### 使用する UI 機能と要素 {#ui-functionality-and-elements}

ユースケースを実装する手順が完了したら、このドキュメントの最初に示すReal-Time CDPの機能と UI 要素を利用します。 これらの領域に必要な属性ベースのアクセス制御権限を持っていることを確認するか、必要な権限をシステム管理者に与えてもらってください。

### スキーマデザインの作成とフィールドグループの指定

Experience Data Model(XDM) のリソースは、 [!UICONTROL スキーマ] Adobe Experience Platformのワークスペース Adobeが提供するコアリソースを表示および調査し、組織のカスタムリソースおよびスキーマを作成できます。

スキーマの作成について詳しくは、 [スキーマの作成に関するチュートリアル](/help/xdm/tutorials/create-schema-ui.md)

再エンゲージメントジャーニーに使用されるスキーマデザインは 4 つあります。 各スキーマでは、特定のフィールドを設定する必要があります。また、強く推奨されるフィールドもいくつかあります。

#### 顧客属性スキーマ

顧客属性スキーマは、 [!UICONTROL XDM 個人プロファイル] クラス。次のフィールドグループが含まれます。

+++個人の連絡先の詳細（フィールドグループ）

[個人の連絡先の詳細](/help/xdm/field-groups/profile/personal-contact-details.md) は、個人の連絡先情報を記述する XDM Individual Profile クラスの標準スキーマフィールドグループです。

| フィールド | 要件 | 説明 |
| --- | --- | --- |
| `mobilePhone.number` | 必須 | SMS に使用される人の携帯電話番号。 |
| `personalEmail.address` | 必須 | 人物の電子メールアドレス。 |

+++

+++人口統計の詳細（フィールドグループ）

[人口統計の詳細](/help/xdm/field-groups/profile/demographic-details.md) は、XDM Individual Profile クラスの標準スキーマフィールドグループです。 フィールドグループは、個人に関する情報を表すルートレベルの個人オブジェクトを提供します。このオブジェクトのサブフィールドは、個人に関する情報を表します。

| フィールド | 要件 |
| --- | --- |
| `person.name.firstName` | 推奨 |
| `person.name.lastName` | 推奨 |

+++

+++外部ソースシステム監査の詳細（フィールドグループ）

[外部ソースシステム監査属性](/help/xdm/data-types/external-source-system-audit-attributes.md) は、外部ソースシステムに関する監査の詳細を取り込む、標準の Experience Data Model(XDM) データ型です。

+++

+++同意および環境設定フィールドグループ（フィールドグループ）

[同意および環境設定](/help/xdm/field-groups//profile/consents.md) フィールドグループは、同意および環境設定情報を取り込むための単一のオブジェクトタイプフィールド（同意）を提供します。

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

顧客のデジタルトランザクションスキーマは、 [!UICONTROL XDM ExperienceEvent] クラス。次のフィールドグループが含まれます。

+++Adobe Experience Platform Web SDK ExperienceEvent（フィールドグループ）

| フィールド | 要件 |
| --- | --- |
| `device.model` | 推奨 |
| `environment.browserDetails.userAgent` | 推奨 |

+++

+++Web の詳細（フィールドグループ）

Web 詳細は、XDM ExperienceEvent クラスの標準スキーマフィールドグループで、インタラクション、ページの詳細、リファラーなどの Web 詳細イベントに関する情報を記述するために使用されます。

| フィールド | 要件 | 説明 |
| --- | --- | --- |
| `web.webInteraction.linkClicks.id` | 推奨 | インタラクションに対応する Web リンクまたは URL の ID。 |
| `web.webInteraction.linkClicks.value` | 推奨 | インタラクションに対応する Web リンクまたは URL のクリック数。 |
| `web.webInteraction.name` | 推奨 | Web ページの名前。 |
| `web.webInteraction.URL` | 推奨 | Web ページの URL です。 |
| `web.webPageDetails.name` | 推奨 | Web インタラクションが発生した Web ページの名前。 |
| `web.webPageDetails.URL` | 推奨 | Web インタラクションが発生した Web ページの URL。 |
| `web.webReferrer.URL` | 推奨 | Web インタラクションのリファラーを表します。これは、現在の Web インタラクションが記録される直前に訪問者が来た URL です。 |

+++

+++コンシューマーエクスペリエンスイベント（フィールドグループ）

| フィールド | 要件 |
| --- | --- |
| `commerce.cart.cartID` | 推奨 |
| `commerce.cart.cartSource` | 推奨 |
| `commerce.cartAbandons.id` | 推奨 |
| `commerce.cartAbandons.value` | 推奨 |
| `commerce.order.orderType` | 推奨 |
| `commerce.order.payments.paymentAmount` | 推奨 |
| `commerce.order.payments.paymentType` | 推奨 |
| `commerce.order.payments.transactionID` | 推奨 |
| `commerce.order.priceTotal` | 推奨 |
| `commerce.order.purchaseID` | 推奨 |
| `commerce.productListAdds.id` | 推奨 |
| `commerce.productListAdds.value` | 推奨 |
| `commerce.productListOpens.id` | 推奨 |
| `commerce.productListOpens.value` | 推奨 |
| `commerce.productListRemoval.id` | 推奨 |
| `commerce.productListRemoval.value` | 推奨 |
| `commerce.productListViews.id` | 推奨 |
| `commerce.productListViews.value` | 推奨 |
| `commerce.productViews.id` | 推奨 |
| `commerce.productViews.value` | 推奨 |
| `commerce.purchases.id` | 推奨 |
| `commerce.purchases.value` | 推奨 |
| `marketing.campaignGroup` | 推奨 |
| `marketing.campaignName` | 推奨 |
| `marketing.trackingCode` | 推奨 |
| `productListItems.name` | 推奨 |
| `productListItems.priceTotal` | 推奨 |
| `productListItems.product` | 推奨 |
| `productListItems.quantity` | 推奨 |

+++

+++エンドユーザー ID の詳細（フィールドグループ）

| フィールド | 要件 | 説明 |
| --- | --- | --- |
| `endUserIDs._experience.emailid.authenticatedState` | 必須 | エンドユーザーの電子メールアドレス ID 認証状態。 |
| `endUserIDs._experience.emailid.id` | 必須 | エンドユーザーの電子メールアドレス ID。 |
| `endUserIDs._experience.emailid.namespace.code` | 必須 | エンドユーザーの電子メールアドレス ID 名前空間コード。 |
| `endUserIDs._experience.mcid.authenticatedState` | 必須 | Adobe Marketing Cloud ID(MCID) 認証済み状態。 MCID は、Experience CloudID(ECID) と呼ばれます。 |
| `endUserIDs._experience.mcid.id` | 必須 | Adobe Marketing Cloud ID(MCID)。 MCID は、Experience CloudID(ECID) と呼ばれます。 |
| `endUserIDs._experience.mcid.namespace.code` | 必須 | Adobe Marketing Cloud ID(MCID) 名前空間コード。 |

+++

+++クラス値（フィールドグループ）

| フィールド | 要件 |
| --- | --- |
| `eventType` | 必須 |
| `timestamp` | 必須 |

+++

+++外部ソースシステム監査の詳細（フィールドグループ）

External Source System Audit Attributes は、外部ソースシステムに関する監査の詳細を取り込む、標準の Experience Data Model(XDM) データ型です。

+++

#### 顧客オフライントランザクションスキーマ

顧客オフライントランザクションスキーマは、 [!UICONTROL XDM ExperienceEvent] クラス。次のフィールドグループが含まれます。

+++コマースの詳細（フィールドグループ）

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

| フィールド | 要件 | 説明 |
| --- | --- | --- |
| `mobilePhone.number` | 必須 | SMS に使用される人の携帯電話番号。 |
| `personalEmail.address` | 必須 | 人物の電子メールアドレス。 |

+++

+++クラス値（フィールドグループ）

| フィールド | 要件 |
| --- | --- |
| `eventType` | 必須 |
| `timestamp` | 必須 |

+++

+++外部ソースシステム監査の詳細（フィールドグループ）

External Source System Audit Attributes は、外部ソースシステムに関する監査の詳細を取り込む、標準の Experience Data Model(XDM) データ型です。

+++

#### AdobeWeb コネクタのスキーマ

AdobeWeb コネクタスキーマは、 [!UICONTROL XDM ExperienceEvent] クラス。次のフィールドグループが含まれます。

+++Adobe Analytics ExperienceEvent テンプレート（フィールドグループ）

| フィールド | 要件 | 説明 |
| --- | --- | --- |
| `web.webInteraction.linkClicks.id` | 推奨 | インタラクションに対応する Web リンクまたは URL の ID。 |
| `web.webInteraction.linkClicks.value` | 推奨 | インタラクションに対応する Web リンクまたは URL のクリック数。 |
| `web.webInteraction.name` | 推奨 | Web ページの名前。 |
| `web.webInteraction.URL` | 推奨 | Web ページの URL です。 |
| `web.webPageDetails.name` | 推奨 | Web インタラクションが発生した Web ページの名前。 |
| `web.webPageDetails.URL` | 推奨 | Web インタラクションが発生した Web ページの URL。 |
| `web.webReferrer.URL` | 推奨 | Web インタラクションのリファラーを表します。これは、現在の Web インタラクションが記録される直前に訪問者が来た URL です。 |
| `commerce.cart.cartID` | 推奨 | |
| `commerce.cart.cartSource` | 推奨 | |
| `commerce.cartAbandons.id` | 推奨 | |
| `commerce.cartAbandons.value` | 推奨 | |
| `commerce.order.orderType` | 推奨 | |
| `commerce.order.payments.paymentAmount` | 推奨 | |
| `commerce.order.payments.paymentType` | 推奨 | |
| `commerce.order.payments.transactionID` | 推奨 | |
| `commerce.order.priceTotal` | 推奨 | |
| `commerce.order.purchaseID` | 推奨 | |
| `commerce.productListAdds.id` | 推奨 | |
| `commerce.productListAdds.value` | 推奨 | |
| `commerce.productListOpens.id` | 推奨 | |
| `commerce.productListOpens.value` | 推奨 | |
| `commerce.productListRemoval.id` | 推奨 | |
| `commerce.productListRemoval.value` | 推奨 | |
| `commerce.productListViews.id` | 推奨 | |
| `commerce.productListViews.value` | 推奨 | |
| `commerce.productViews.id` | 推奨 | |
| `commerce.productViews.value` | 推奨 | |
| `commerce.purchases.id` | 推奨 | |
| `commerce.purchases.value` | 推奨 | |
| `marketing.campaignGroup` | 推奨 | |
| `marketing.campaignName` | 推奨 | |
| `marketing.trackingCode` | 推奨 | |
| `productListItems.name` | 推奨 | |
| `productListItems.priceTotal` | 推奨 | |
| `productListItems.product` | 推奨 | |
| `productListItems.quantity` | 推奨 | |
| `endUserIDs._experience.emailid.authenticatedState` | 必須 | エンドユーザーの電子メールアドレス ID 認証状態。 |
| `endUserIDs._experience.emailid.id` | 必須 | エンドユーザーの電子メールアドレス ID。 |
| `endUserIDs._experience.emailid.namespace.code` | 必須 | エンドユーザーの電子メールアドレス ID 名前空間コード。 |
| `endUserIDs._experience.mcid.authenticatedState` | 必須 | Adobe Marketing Cloud ID(MCID) 認証済み状態。 MCID は、Experience CloudID(ECID) と呼ばれます。 |
| `endUserIDs._experience.mcid.id` | 必須 | Adobe Marketing Cloud ID(MCID)。 MCID は、Experience CloudID(ECID) と呼ばれます。 |
| `endUserIDs._experience.mcid.namespace.code` | 必須 | Adobe Marketing Cloud ID(MCID) 名前空間コード。 |

+++

+++クラス値（フィールドグループ）

| フィールド | 要件 |
| --- | --- |
| `eventType` | 必須 |
| `timestamp` | 必須 |

+++

+++外部ソースシステム監査の詳細（フィールドグループ）

External Source System Audit Attributes は、外部ソースシステムに関する監査の詳細を取り込む、標準の Experience Data Model(XDM) データ型です。

+++

### スキーマからのデータセットの作成

データセットは、データのグループ ( 多くの場合、フィールド（行）とスキーマ（列）を含むテーブル ) のストレージと管理の構造です。 インテリジェントな再エンゲージメントジャーニーのすべてのスキーマには、1 つのデータセットがあります。

スキーマからデータセットを作成する方法について詳しくは、 [データセット UI ガイド](/help/catalog/datasets/user-guide.md).

>[!NOTE]
>
>スキーマを作成する手順と同様に、データセットをリアルタイム顧客プロファイルに含める必要があります。 リアルタイム顧客プロファイルでデータセットを使用する方法について詳しくは、 [スキーマの作成に関するチュートリアル](/help/xdm/tutorials/create-schema-ui.md#profile).

### プライバシー、同意、データガバナンス

#### 同意ポリシー

>[!IMPORTANT]
>
>顧客がブランドからの通信を購読解除する機能を提供することは、法的要件であり、この選択を確実に受け入れるためにも必要です。 該当する法律について詳しくは、[Experience Platformドキュメント](https://experienceleague.adobe.com/docs/experience-platform/privacy/regulations/overview.html)を参照してください。

再エンゲージメントパスを作成する場合は、次の同意ポリシーを考慮して使用する必要があります。

* 次の場合 `consents.marketing.email.val = "Y"` その後、電子メールを送信可能
* 次の場合 `consents.marketing.sms.val = "Y"` その後、SMS を送信可能
* 次の場合 `consents.marketing.push.val = "Y"` 次にプッシュ可能
* 次の場合 `consents.share.val = "Y"` 次に、広告を作成可能
* お客様の実装によって定義される必要性

#### DULE ラベルと実施

個人の電子メールアドレスは、デバイスではなく特定の個人を識別したり、特定の個人と連絡を取り合うために使用される、直接識別可能なデータとして使用されます。

* `personalEmail.address = I1`

#### マーケティングポリシー

再エンゲージメントジャーニーに追加のマーケティングポリシーは必要ありませんが、必要に応じて、以下を考慮する必要があります。

* 機密データの制限
* オンサイト広告の制限
* メールのターゲティングを制限
* クロスサイトターゲティングの制限
* 直接識別可能なデータと匿名データの組み合わせを制限

### オーディエンスの作成

#### ブランドの再エンゲージメントジャーニー用のオーディエンスの作成

再エンゲージメントジャーニーは、オーディエンスを使用して、プロファイルストアのプロファイルのサブセットで共有される特定の属性や行動を定義し、マーケティング可能な人々のグループを顧客ベースから区別します。 オーディエンスは、Adobe Experience Platform上で 2 つの異なる方法で作成できます。オーディエンスとして直接構成するか、Platform 派生のセグメント定義を通じて作成します。

オーディエンスを直接作成する方法について詳しくは、 [Audience Composition UI ガイド](/help/segmentation/ui/audience-composition.md).

Platform 派生セグメント定義を使用してオーディエンスを構築する方法について詳しくは、 [Audience Builder UI ガイド](/help/segmentation/ui/segment-builder.md).

>[!BEGINTABS]

>[!TAB 再エンゲージメントジャーニー]

次のイベントは、ユーザーが製品をオンラインで表示し、次の 24 時間に買い物かごに追加せず、その後 3 日間はブランドエンゲージメントがおこなわれなかった再エンゲージメントジャーニーに使用されます。

このオーディエンスを設定する際は、次のフィールドと条件が必要です。

* `EventType: commerce.productViews`
   * `Timestamp: <= 24 hours before now`
* `EventType is not: commerce.productListAdds`
   * `Timestamp: <= 24 hours before now, GAP(>= 3 days)`
* `EventType: application.launch or web.webpagedetails.pageViews or commerce.purchases`
   * `Timestamp: <= 2 days before now`

再エンゲージメントジャーニーの記述子は次のようになります。

`Include audience who have at least 1 EventType = ProductViews event THEN have at least 1 Any event where (EventType does not equal commerce.productListAdds) and occurs in last 24 hour(s) then after 3 days do not have any Any event where (EventType = application.launch or web.webpagedetails.pageViews or commerce.purchases) and occurs in last 2 day(s).`

>[!TAB 放棄された買い物かごのジャーニー]

次のイベントは、離脱した買い物かごへの移動に使用されます。このイベントでは、ユーザーは買い物かごに製品を追加しましたが、過去 24 時間に購入を完了しなかったか、買い物かごをクリアしませんでした。

このオーディエンスを設定する際は、次のフィールドと条件が必要です。

* `EventType: commerce.productListAdds`
   * `Timestamp: >= 30 minutes before now and <= 1440 minutes before now`
* `EventType: commerce.purchases`
   * `Timestamp: <= 30 minutes before now`
* `EventType: commerce.productListRemovals`
   * `Timestamp: <= 30 minutes before now`

放棄された買い物かごのジャーニーの記述子は、次のように表示されます。

`Include EventType = commerce.productListAdds between 30 min and 1440 minutes before now. exclude EventType = commerce.purchases 30 minutes before now OR EventType = commerce.productListRemovals AND Cart ID equals Product List Adds1 Cart ID (the inclusion event).`

>[!ENDTABS]

### Adobe Journey Optimizerでのジャーニーの設定

>[!NOTE]
>
>Adobe Journey Optimizerは、このページの上部にある図に表示されるすべてを網羅しているわけではありません。 すべての有料メディア広告は、で作成されます。 [!UICONTROL 宛先].

Adobe Journey Optimizerは、顧客とのつながり、コンテキストに応じた、パーソナライズされたエクスペリエンスを提供するのに役立ちます。 カスタマージャーニーとは、顧客がブランドとやり取りするプロセス全体を指します。 各ユースケースのジャーニーには、具体的な情報が必要です。 以下に、各ジャーニー分岐に必要な正確なデータを示します。

>[!BEGINTABS]

>[!TAB 再エンゲージメントジャーニー]

+++イベント

* 製品表示
   * スキーマ：顧客のデジタルトランザクション
   * フィールド:
      * `EventType`
   * 条件:
      * `EventType = commerce.productViews`
      * フィールド:
         * `Commerce.productViews.id`
         * `Commerce.productViews.value`
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

* 買い物かごに追加
   * スキーマ：顧客のデジタルトランザクション
   * フィールド:
      * `EventType`
   * 条件:
      * `EventType = commerce.productListAdds`
      * フィールド:
         * `Commerce.productListAdds.id`
         * `Commerce.productListAdds.value`
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

* ブランドエンゲージメント
   * スキーマ：顧客のデジタルトランザクション
   * フィールド:
      * `EventType`
   * 条件:
      * `EventType in application.launch, commerce.purchases, web.webpagedetails.pageViews`
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
         * `Commerce.purchases.id`
         * `Commerce.purchases.value`
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
      * スキーマ：顧客オフライントランザクション v.1
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

>[!TAB 放棄された買い物かごのジャーニー]

+++イベント

* 買い物かごに追加
   * スキーマ：顧客のデジタルトランザクション
   * フィールド:
      * `EventType`
   * 条件:
      * `EventType = commerce.productListAdds`
      * フィールド:
         * `Commerce.productListAdds.id`
         * `Commerce.productListAdds.value`
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

* オンライン購入
   * スキーマ：顧客のデジタルトランザクション
   * フィールド:
      * `EventType`
   * 条件:
      * `EventType = commerce.purchases`
      * フィールド:
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

* ブランドエンゲージメント
   * スキーマ：顧客のデジタルトランザクション
   * フィールド:
      * `EventType`
   * 条件:
      * `EventType in application.launch, commerce.purchases, web.webpagedetails.pageViews`
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
         * `Commerce.purchases.id`
         * `Commerce.purchases.value`
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
   * スキーマ：顧客オフライントランザクション v.1
   * `eventType = commerce.purchases`
   * `timestamp > timestamp of cart was last abandoned`

* 条件：買い物かごが最後に破棄されてからクリアされた買い物かご：
   * スキーマ：顧客デジタルトランザクション v.1
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

>[!TAB 注文確認ジャーニー]

+++イベント

* オンライン購入
   * スキーマ：顧客のデジタルトランザクション
   * フィールド:
      * `EventType`
   * 条件:
      * `EventType = commerce.purchases`
      * フィールド:
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

でのジャーニー作成の詳細 [Adobe Journey Optimizer]を読む [ジャーニーの概要ガイド](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/journey.html?lang=ja).

### 宛先での有料メディア広告の設定

宛先フレームワークは、有料メディア広告に使用されます。 同意が確認されると、設定された様々な宛先に送信されます。 例えば、ダイレクトメール、E メール、プッシュ、SMS などです。

#### 宛先に必要なデータ

ストリーミングセグメントの書き出し先 (Facebook、Google Customer Match、Google DV360 など ) は、顧客データからの様々な ID をサポートします。

* `personalEmail.address`
* `ECID`
* `mobilePhone.number`

「買い物かごセグメントを放棄」はストリーミングなので、この使用例では宛先フレームワークで使用できます。

* ストリーム/トリガー済み
   * [広告](/help/destinations/catalog/advertising/overview.md)/[Paid Media &amp; Social](/help/destinations/catalog/social/overview.md)
   * [Mobile](/help/destinations/catalog/mobile-engagement/overview.md)
   * [ストリーミング先](/help/destinations/catalog/streaming/http-destination.md)
   * [カスタムDestination SDK](/help/destinations/destination-sdk/overview.md)

* 3 時間ごとのファイル/スケジュール
   * [電子メールマーケティング](/help/destinations/catalog/email-marketing/overview.md)
