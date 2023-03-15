---
keywords: ターゲットのパーソナライゼーション;宛先;Experience Platform ターゲットの宛先:Adobe Target の宛先;
title: Adobe Target 接続
description: Adobe Target は、web サイトやモバイルアプリなど、すべてのインバウンド顧客とのインタラクションで、AI を利用したリアルタイムのパーソナライズと実験の機能を提供するアプリケーションです。
exl-id: 3e3c405b-8add-4efb-9389-5ad695bc9799
source-git-commit: f97b667f8d4dc311683b018bb1c1792aae871648
workflow-type: tm+mt
source-wordcount: '1011'
ht-degree: 54%

---

# Adobe Target 接続 {#adobe-target-connection}

## 宛先の変更ログ {#changelog}

>[!IMPORTANT]
>
>拡張Adobe Target V2 宛先コネクタのベータリリースにより、宛先カタログに 2 つのAdobe Targetカードが表示される場合がありました。
>Adobe Target V2 宛先コネクタは現在ベータ版で、一部のお客様のみが利用できます。 Adobe V1 カードが提供する機能に加えて、Target V2 コネクタにより、[マッピング手順](/help/destinations/ui/activate-profile-request-destinations.md#map-attributes)をアクティベーションワークフローに追加します。これにより、プロファイル属性を Adobe Target にマッピングし、属性ベースの同じページおよび次のページのパーソナライゼーションを有効にできます。

![2 つのAdobe Target宛先カードの横並び表示の画像。](/help/destinations/assets/catalog/personalization/adobe-target-connection/adobe-target-side-by-side-view.png)

## 概要 {#overview}

Adobe Target は、web サイトやモバイルアプリなど、すべてのインバウンド顧客とのインタラクションで、AI を利用したリアルタイムのパーソナライズと実験の機能を提供するアプリケーションです。

Adobe Targetは、Adobe Experience Platformの宛先カタログのパーソナライゼーション接続です。

## 前提条件 {#prerequisites}

### データストリーム ID {#datastream-id}

Adobe Target接続を [datastream ID の使用](#parameters)を使用する場合、 [Adobe Experience Platform Web SDK](../../../edge/home.md) 実装済み

datastream ID を使用せずにAdobe Target接続を設定する場合、Web SDK を実装する必要はありません。

>[!IMPORTANT]
>
>[!DNL Adobe Target] 接続を作成する前に、[同じページと次のページのパーソナライズのためのパーソナライズ機能宛先の設定](../../ui/configure-personalization-destinations.md)を行う方法を参照してください。このガイドでは、複数の Experience Platform コンポーネントをまたいで、同じページおよび次のページのパーソナライゼーションの使用例に必要な設定手順を説明します。同じページと次のページのパーソナライゼーションでは、Adobe Target接続を設定する際にデータストリーム ID を使用する必要があります。

### Adobe Targetの前提条件 {#prerequisites-in-adobe-target}

Adobe Targetで、ユーザーが以下を持っていることを確認します。

* へのアクセス [デフォルトのワークスペース](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/property-channel.html?lang=en#default-workspace);
* この **承認者** [役割](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/property-channel.html?lang=en#roles-and-permissions).

権限の付与の詳細を表示： [Target Premium](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html?lang=en#section_8C425E43E5DD4111BBFC734A2B7ABC80) および [Target Standard](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/users/user-management.html?lang=en#roles-permissions).

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!DNL Profile request]** | 単一のプロファイルに対して、Adobe Targetの宛先にマッピングされているすべてのセグメントを要求しています。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。セグメント評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## ユースケース {#use-cases}

**ホームページバナーのパーソナライズ**

家庭用品のレンタルと販売を行う会社が、Adobe Experience Platform での顧客セグメント選定に基づいて、自身のホームページをバナー付きでパーソナライズしたいと考えています。会社は、パーソナライズされたエクスペリエンスを体験できるオーディエンスを選択し、それらのオーディエンスを Target オファーのターゲッティング条件として Adobe Target に送信できます。

## 宛先への接続 {#connect}

>[!CONTEXTUALHELP]
>id="platform_destinations_target_datastream"
>title="データストリーム ID について"
>abstract="このオプションは、セグメントに含めるデータ収集データストリームを決定します。 ドロップダウンメニューには、Target 設定が有効になっているデータストリームのみが表示されます。 エッジセグメント化を使用するには、データストリーム ID を選択する必要があります。 「なし」を選択すると、エッジセグメント化を使用するすべてのユースケースが無効になります。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/personalization/adobe-target-connection.html#parameters" text="データストリームの選択の詳細"

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

Adobe Experience Platform は、会社の Adobe Target インスタンスに自動的に接続します。認証は必要ありません。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **名前**：この宛先に希望する名前を入力します。
* **説明**：宛先についての説明を入力します。例えば、この宛先を使用しているキャンペーンを指定できます。このフィールドはオプションです。
* **データストリーム ID**:これにより、セグメントを含めるデータ収集データストリームが決まります。 ドロップダウンメニューには、Target サービスとAdobe Experience Platformサービスが有効になっているデータストリームのみが表示されます。 詳しくは、 [データストリームの設定](../../../edge/datastreams/configure.md#aep) Adobe Experience PlatformとAdobe Targetのデータストリームを設定する方法について詳しくは、こちらを参照してください。
   * **[!UICONTROL なし]**:Adobe Targetのパーソナライゼーションを設定する必要があるが、 [Experience PlatformWeb SDK](../../../edge/home.md). このオプションを使用する場合、Experience Platformから Target に書き出されたセグメントは、次回のセッションのパーソナライゼーションのみをサポートし、エッジのセグメント化は無効になります。 詳しくは後述のテーブルを参照してください.

| データストリームが選択されていません | データストリームが選択されました |
|---|---|
| <ul><li>[エッジセグメント化](../../../segmentation/ui/edge-segmentation.md) はサポートされていません。</li><li>[同じページと次のページのパーソナライゼーション](../../ui/configure-personalization-destinations.md) はサポートされていません。</li><li>セグメントをAdobe Target接続に共有できるのは、 *デフォルトの実稼動サンドボックス*.</li><li>データストリーム ID を使用せずに次のセッションのパーソナライゼーションを設定するには、 [at.js](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/at-js/how-atjs-works.html?lang=en).</li></ul> | <ul><li>エッジのセグメント化は期待どおりに動作します。</li><li>[同じページと次のページのパーソナライゼーション](../../ui/configure-personalization-destinations.md) はサポートされています。</li><li>他のサンドボックスでは、セグメントの共有がサポートされています。</li></ul> |

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]**[に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先にオーディエンスセグメントを有効化する手順については、[プロファイルリクエストの宛先へのプロファイルとセグメントの有効化](../../ui/activate-profile-request-destinations.md)を参照してください。

## 書き出したデータ {#exported-data}

Adobe Target は、Adobe Experience Platform Edge Network からプロファイルデータを読み取るので、データはエクスポートされません。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja)を参照してください。
