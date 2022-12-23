---
title: （ベータ版）Adobe Commerce 宛先コネクタ
description: Adobe Commerce と Real-Time CDP を使用して、Real-Time CDP で作成および管理される顧客セグメントに合わせてカスタマイズされた、関連性の高いサイトのコンテンツとプロモーションを提供することで、ショッピングエクスペリエンスをパーソナライズする方法を説明します。
exl-id: f7aa3c6c-ba7a-440c-a4d7-5d7b50dbbc0d
source-git-commit: 638a778d1d999ab6a1726333f9cde0a0b4fad57b
workflow-type: tm+mt
source-wordcount: '691'
ht-degree: 100%

---

# （ベータ版）Adobe Commerce 接続 {#adobe-commerce}

## 概要 {#overview}

>[!IMPORTANT]
> 
>**[!UICONTROL Adobe Commerce]** コネクタはベータ版で、一部のお客様のみご利用いただけます。

[!DNL Adobe Commerce] 宛先コネクタを使用すると、[!DNL Adobe Commerce] アカウントにアクティブ化する 1 つ以上の Real-Time CDP セグメントを選択し、動的にパーソナライズされたエクスペリエンスを買い物客に提供できるようになります。[!DNL Adobe Commerce] 内でこれらの Real-Time CDP セグメントを選択して、「2 つ買うと 1 つ無料」などの、買い物かご内の独自のオファーをパーソナライズできます。また、ヒーローバナーを表示したり、プロモーションオファーを通じて製品の価格を変更したりすることもできます。これらはすべて Adobe Real-Time CDP セグメント用にカスタマイズされています。

<!--## Use cases {#use-cases}

To help you better understand how and when you should use the *YourDestination* destination, here are sample use cases that Adobe Experience Platform customers can solve by using this destination.

### Use case #1 {#use-case-1}

*For mobile messaging platforms:*

*A home rental and sales platform wants to push mobile notifications to customers' Android and iOS devices to let them know that there are 100 updated listings in the area where they previously searched for a rental.*

### Use case #2 {#use-case-2}

*For social network platforms:*

*An athletic apparel brand wants to reach existing customers through their social media accounts. The apparel brand can ingest email addresses from their own CRM to Adobe Experience Platform, build segments from their own offline data, and send these segments to YourDestination, to display ads in their customers' social media feeds.*-->

## 前提条件 {#prerequisites}

この拡張機能は、Real-time CDP Prime または Ultimate と Adobe Commerce を購入した一部のベータ版のお客様の宛先カタログで利用できます。

ベータ版のお客様は、次の項目にアクセスできます。

- [Adobe Experience Platform](https://experience.adobe.com/)
- [Adobe 開発者コンソール](https://developer.adobe.com/developer-console/docs/guides/getting-started/)
- [Adobe Commerce Cloud バージョン 2.4.4 以降](https://business.adobe.com/jp/products/magento/magento-commerce.html)

Experience Platform で、以下を作成します。

- [スキーマ](../../../xdm/schema/composition.md)。作成するスキーマは、Adobe Commerce から取り込む予定のデータを表します。Commerce 固有のフィールドグループを含むスキーマの作成方法についての[詳細情報](https://experienceleague.adobe.com/docs/commerce-merchant-services/experience-platform-connector/fundamentals/update-xdm.html?lang=ja)。
- [データセット](../../../catalog/datasets/user-guide.md#create)。データセットは、データの集まりのためのストレージと管理の構成体です。上記で作成したスキーマからこのデータセットを作成する必要があります。
- [データストリーム](../../../edge/datastreams/overview.md#create)。Adobe Experience Platform から他の Adobe DX 製品にデータを送信できるようにする ID。この ID は、特定の Adobe Commerce インスタンス内の特定の web サイトに関連付ける必要があります。このデータストリームを作成する場合は、上で作成した XDM スキーマを指定します。

前提条件を満たしたら、[!DNL Commerce] 宛先に接続します。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

[!DNL Adobe Commerce] 宛先へ接続する手順は次のとおりです。

1. [Platform インターフェイス](https://experience.adobe.com/platform/)で、**[!UICONTROL 宛先]**／**[!UICONTROL カタログ]**&#x200B;に移動します。
1. **[!UICONTROL Personalization]** を選択します。
1. ハイライトする Adobe Commerce の宛先を選択し、「**[!UICONTROL 設定]**」を選択します。
1. [宛先設定のチュートリアル](../../ui/connect-destination.md)に示されている手順に従います。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

- **[!UICONTROL 名前]**：この宛先に希望する名前を入力します。
- **[!UICONTROL 説明]**：宛先についての説明を入力します。例えば、この宛先を使用しているキャンペーンを指定できます。このフィールドはオプションです。
- **[!UICONTROL 統合エイリアス]**：この値は、JSON オブジェクト名として Experience Platform Web SDK に送信されます。
- **[!UICONTROL データストリーム ID]**：これにより、ページへの応答にセグメントを含めるデータ収集データストリームが決定されます。ドロップダウンメニューには、宛先設定が有効になっているデータストリームのみが表示されます。詳しくは、[データストリームの設定](../../../edge/datastreams/overview.md)を参照してください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## [!DNL Commerce] 宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

[!DNL Commerce] 宛先にオーディエンスセグメントをアクティブ化する手順は、[プロファイルリクエストの宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-profile-request-destinations.md)をお読みください。

## [!DNL Adobe Commerce] での次の手順

Experience Platform 内の [!DNL Commerce] 宛先の設定が完了しました。次は [!DNL Commerce Admin] を設定して、作成した Real-time CDP セグメントを読み込む必要があります。詳しくは、[[!DNL Commerce] ドキュメント](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/customer-segment-rtcdp.html?lang=ja)を参照してください。

## Commerce における Audience Activation の検証 {#exported-data}

[!DNL Adobe Commerce] アカウントに対して Real-Time CDP セグメントをアクティブ化すると、買い物かごの価格ルールを作成する際に [!DNL Admin] でそれらのセグメントが利用できるようになります。

![Adobe Commerce 管理者](../../assets/catalog/personalization/adobe-commerce/rtcdp-in-admin.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
