---
title: Adobe Commerce Destination Connector
description: Adobe CommerceとReal-Time CDPの商人が、Real-Time CDP内で作成および管理する顧客オーディエンスに合わせてカスタマイズされた、関連性の高いサイトのコンテンツとプロモーションを提供することで、買い物体験をパーソナライズする方法を説明します。
exl-id: f7aa3c6c-ba7a-440c-a4d7-5d7b50dbbc0d
source-git-commit: d6402f22ff50963b06c849cf31cc25267ba62bb1
workflow-type: tm+mt
source-wordcount: '720'
ht-degree: 59%

---

# Adobe Commerce Connection {#adobe-commerce}

## 概要 {#overview}

この [!DNL Adobe Commerce] 宛先コネクタを使用すると、アクティブ化する 1 つ以上のReal-Time CDPオーディエンスを選択できます [!DNL Adobe Commerce] 顧客に動的にパーソナライズされたエクスペリエンスを提供するアカウント。 内 [!DNL Adobe Commerce]その後、これらのReal-Time CDPオーディエンスを選択して、買い物かご内の個別オファー（「2 を購入すると 1 回無料になります」など）をパーソナライズできます。 また、ヒーローバナーを表示したり、プロモーションオファーを通じて製品の価格を変更したりすることもできます。これらはすべてAdobe Real-Time CDPオーディエンスにカスタマイズされます。

## 前提条件 {#prerequisites}

このコネクタは、Real-Time CDP Prime または Ultimate とAdobe Commerceを購入した顧客の宛先カタログで使用できます。

この宛先接続を使用するには、次へのアクセス権があることを確認してください。

- [Adobe Experience Platform](https://experience.adobe.com/)
- [Adobe 開発者コンソール](https://developer.adobe.com/developer-console/docs/guides/getting-started/)。開発者コンソールにアクセスすると、 [設定を完了](https://experienceleague.adobe.com/docs/commerce-admin/customers/customers-menu/audience-activation.html#configure-the-extension) Adobe Commerceの拡張機能の
- [Adobe Commerce Cloud バージョン 2.4.4 以降](https://business.adobe.com/jp/products/magento/magento-commerce.html)

Experience Platform で、以下を作成します。

- [スキーマ](../../../xdm/schema/composition.md)。作成するスキーマは、Adobe Commerce から取り込む予定のデータを表します。Commerce 固有のフィールドグループを含むスキーマの作成方法についての[詳細情報](https://experienceleague.adobe.com/docs/commerce-merchant-services/experience-platform-connector/fundamentals/update-xdm.html?lang=ja)。
- [データセット](../../../catalog/datasets/user-guide.md#create)。データセットは、データの集まりのためのストレージと管理の構成体です。このデータセットは、上で作成したスキーマから作成します。
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
- **[!UICONTROL データストリーム ID]**:これにより、ページへの応答に含まれるオーディエンスを含むデータ収集データストリームが決定されます。 ドロップダウンメニューには、宛先設定が有効になっているデータストリームのみが表示されます。詳しくは、[データストリームの設定](../../../edge/datastreams/overview.md)を参照してください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## オーディエンスを [!DNL Commerce] 宛先 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

読み取り [プロファイルリクエストの宛先へのプロファイルとオーディエンスのアクティブ化](../../ui/activate-edge-personalization-destinations.md) オーディエンスを [!DNL Commerce] 宛先。

## [!DNL Adobe Commerce] での次の手順

これで、 [!DNL Commerce] Experience Platform内の宛先、 [!DNL Audience Activation] 拡張 [!DNL Commerce] および [!DNL Commerce Admin] をクリックして、作成したReal-Time CDPオーディエンスをインポートします。 詳しくは、[[!DNL Commerce] ドキュメント](https://experienceleague.adobe.com/docs/commerce-admin/customers/customers-menu/audience-activation.html)を参照してください。

## Commerce における Audience Activation の検証 {#exported-data}

Real-Time CDP Audiences を [!DNL Adobe Commerce] アカウントを使用する場合、 _管理者_ サイドバー、次に移動 **[!UICONTROL 顧客]** > **[!UICONTROL リアルタイム CDP オーディエンス]**.

![Real-Time CDP Audiences ダッシュボード](../../assets/catalog/personalization/adobe-commerce/audience-library.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
