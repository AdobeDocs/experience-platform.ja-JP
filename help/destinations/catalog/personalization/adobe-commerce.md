---
title: Adobe Commerce宛先コネクタ
description: Adobe CommerceとReal-Time CDPのマーチャントが、Real-Time CDPで作成および管理される顧客オーディエンスに合わせてカスタマイズされた、関連性の高いサイトのコンテンツとプロモーションを提供することで、ショッピングエクスペリエンスをパーソナライズする方法を説明します。
exl-id: f7aa3c6c-ba7a-440c-a4d7-5d7b50dbbc0d
source-git-commit: 70556134a96260ae111c71ee288d4d646481270b
workflow-type: tm+mt
source-wordcount: '668'
ht-degree: 42%

---

# Adobe Commerce連携 {#adobe-commerce}

## 概要 {#overview}

[!DNL Adobe Commerce] 宛先コネクタを使用すると、[!DNL Adobe Commerce] アカウントに対してアクティブ化する 1 つ以上のReal-Time CDP オーディエンスを選択し、動的にパーソナライズされたエクスペリエンスを買い物客に提供できます。 [!DNL Adobe Commerce] 内でこれらのReal-Time CDP オーディエンスを選択して、「2 つ買うと 1 つ無料」など、買い物かご内の個別オファーをパーソナライズできます。 また、ヒーローバナーを表示したり、プロモーションオファーを通じて製品の価格を変更したりすることもできます。これらはすべてAdobe Real-Time CDP オーディエンスに合わせてカスタマイズされています。

## 前提条件 {#prerequisites}

Real-Time CDP PrimeまたはUltimateとAdobe Commerceを購入したお客様の宛先カタログで、このコネクタをご利用いただけます。

この宛先接続を使用するには、次へのアクセス権があることを確認します。

- [Adobe Experience Platform](https://experience.adobe.com/)
- [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started/)。 Developer Console にアクセスすると、Adobe Commerceで拡張機能の [&#x200B; 設定を完了 &#x200B;](https://experienceleague.adobe.com/docs/commerce-admin/customers/customers-menu/audience-activation.html?lang=ja#configure-the-extension) するために必要なサービスアカウントと資格情報を確認できます。
- [Adobe Commerce バージョン 2.4.4 以降 &#x200B;](https://business.adobe.com/jp/products/commerce.html)

Experience Platform で、以下を作成します。

- [スキーマ](../../../xdm/schema/composition.md)。作成するスキーマは、Adobe Commerce から取り込む予定のデータを表します。Commerce 固有のフィールドグループを含むスキーマの作成方法についての[詳細情報](https://experienceleague.adobe.com/docs/commerce-merchant-services/data-connection/fundamentals/update-xdm.html?lang=ja)。
- [&#x200B; データセット &#x200B;](../../../catalog/datasets/user-guide.md#create)。 データセットは、データの集まりのためのストレージと管理の構成体です。 このデータセットは、上記で作成したスキーマから作成します。
- [データストリーム](../../../datastreams/overview.md#create)。Adobe Experience Platform から他の Adobe DX 製品にデータを送信できるようにする ID。この ID は、特定の Adobe Commerce インスタンス内の特定の web サイトに関連付ける必要があります。このデータストリームを作成する場合は、上で作成した XDM スキーマを指定します。

前提条件を満たしたら、[!DNL Commerce] 宛先に接続します。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Manage Destinations]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

[!DNL Adobe Commerce] 宛先へ接続する手順は次のとおりです。

1. [Experience Platform インターフェイス &#x200B;](https://experience.adobe.com/platform/) で、**[!UICONTROL Destinations]**/**[!UICONTROL Catalog]** に移動します。
1. **[!UICONTROL Personalization]** を選択します。
1. ハイライトするAdobe Commerceの宛先を選択し、「**[!UICONTROL Set up]**」を選択します。
1. [宛先設定のチュートリアル](../../ui/connect-destination.md)に示されている手順に従います。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

- **[!UICONTROL Name]**：この宛先に希望する名前を入力します。
- **[!UICONTROL Description]**：宛先の説明を入力します。 例えば、この宛先を使用しているキャンペーンを指定できます。このフィールドはオプションです。
- **[!UICONTROL Integration alias]**：この値は、JSON オブジェクト名としてExperience Platform Web SDKに送信されます。
- **[!UICONTROL Datastream ID]**：これにより、ページへの応答に含まれるオーディエンスを含むデータ収集データストリームが決定されます。 ドロップダウンメニューには、宛先設定が有効になっているデータストリームのみが表示されます。詳しくは、[データストリームの設定](../../../datastreams/overview.md)を参照してください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

## [!DNL Commerce] の宛先に対するオーディエンスのアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

[&#x200B; しい宛先にオーディエンスをアクティブ化する手順は、](../../ui/activate-edge-personalization-destinations.md) プロファイルリクエストの宛先へのプロファイルとオーディエンスのアクティブ化 [!DNL Commerce] をお読みください。

## [!DNL Adobe Commerce] での次の手順

Experience Platform内の [!DNL Commerce] の宛先の設定が完了しました。次は [!DNL Audience Activation] に [!DNL Commerce] 拡張機能をインストールし、[!DNL Commerce Admin] を設定して、作成したReal-Time CDP オーディエンスを読み込む必要があります。 詳しくは、[[!DNL Commerce] ドキュメント](https://experienceleague.adobe.com/docs/commerce-admin/customers/customers-menu/audience-activation.html?lang=ja)を参照してください。

## Commerce における Audience Activation の検証 {#exported-data}

[!DNL Adobe Commerce] アカウントに対してReal-Time CDP オーディエンスをアクティブ化すると、_管理者_ サイドバーで **[!UICONTROL Customers]** > **[!UICONTROL Real-Time CDP Audience]** に移動すると、それらのオーディエンスが使用可能になります。

![Real-Time CDP オーディエンスダッシュボード &#x200B;](../../assets/catalog/personalization/adobe-commerce/audience-library.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
