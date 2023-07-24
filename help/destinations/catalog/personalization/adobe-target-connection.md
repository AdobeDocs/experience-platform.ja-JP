---
keywords: ターゲットのパーソナライゼーション;宛先;Experience Platform ターゲットの宛先:Adobe Target の宛先;
title: Adobe Target 接続
description: Adobe Target は、web サイトやモバイルアプリなど、すべてのインバウンド顧客とのインタラクションで、AI を利用したリアルタイムのパーソナライズと実験の機能を提供するアプリケーションです。
exl-id: 3e3c405b-8add-4efb-9389-5ad695bc9799
source-git-commit: 2005238d2e06ed91fd4b0835be38a4b7b8ecf3b4
workflow-type: tm+mt
source-wordcount: '1186'
ht-degree: 51%

---

# Adobe Target 接続 {#adobe-target-connection}

## 宛先の変更ログ {#changelog}

| リリース月 | 更新タイプ | 説明 |
|---|---|---|
| 2023年6月 | 機能とドキュメントの更新 | 2023 年 6 月以降は、新しいAdobe Targetの宛先接続を設定する際に、オーディエンスを共有するAdobe Targetワークスペースを選択できます。 詳しくは、[接続パラメーター](#parameters)の節を参照してください。また、ワークスペースについて詳しくは、Adobe Target での[ワークスペースの設定](https://experienceleague.adobe.com/docs/target-learn/tutorials/administration/set-up-workspaces.html?lang=ja)に関するチュートリアルを参照してください。 |
| 2023年5月 | 機能とドキュメントの更新 | 2023 年 5 月現在、 **[!UICONTROL Adobe Target]** 接続サポート [属性ベースのパーソナライゼーション](../../ui/activate-edge-personalization-destinations.md#map-attributes) およびは、すべてのお客様が一般に利用できます。 |

{style="table-layout:auto"}

## 概要 {#overview}

Adobe Target は、web サイトやモバイルアプリなど、すべてのインバウンド顧客とのインタラクションで、AI を利用したリアルタイムのパーソナライズと実験の機能を提供するアプリケーションです。

Adobe Targetは、Adobe Experience Platformの宛先カタログのパーソナライゼーション接続です。

Experience PlatformでのAdobe Target接続の設定方法の概要については、以下のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/3418799/?quality=12&learn=on)

## 前提条件 {#prerequisites}

### データストリーム ID {#datastream-id}

Adobe Target接続を [datastream ID の使用](#parameters)を使用する場合、 [Adobe Experience Platform Web SDK](../../../edge/home.md) 実装済み

datastream ID を使用せずにAdobe Target接続を設定する場合、Web SDK を実装する必要はありません。

>[!IMPORTANT]
>
>[!DNL Adobe Target] 接続を作成する前に、[同じページと次のページのパーソナライズのためのパーソナライズ機能宛先の設定](../../ui/activate-edge-personalization-destinations.md)を行う方法を参照してください。このガイドでは、複数の Experience Platform コンポーネントをまたいで、同じページおよび次のページのパーソナライゼーションの使用例に必要な設定手順を説明します。同じページと次のページのパーソナライゼーションでは、Adobe Target接続を設定する際にデータストリーム ID を使用する必要があります。

### Adobe Targetの前提条件 {#prerequisites-in-adobe-target}

Adobe Targetで、ユーザーが以下を持っていることを確認します。

* へのアクセス [デフォルトのワークスペース](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/property-channel.html?lang=en#default-workspace);
* この **承認者** [役割](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/property-channel.html?lang=en#roles-and-permissions).

権限の付与の詳細を表示： [Target Premium](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html?lang=en#section_8C425E43E5DD4111BBFC734A2B7ABC80) および [Target Standard](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/users/user-management.html?lang=en#roles-permissions).

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!DNL Profile request]** | 単一のプロファイルに対して、Adobe Targetの宛先にマッピングされているすべてのオーディエンスを要求しています。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 [ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!CONTEXTUALHELP]
>id="platform_destinations_target_datastream"
>title="データストリーム ID について"
>abstract="このオプションは、オーディエンスを含めるデータ収集データストリームを決定します。ドロップダウンメニューには、ターゲット設定が有効になっているデータストリームのみが表示されます。エッジセグメント化を使用するには、データストリーム ID を選択する必要があります。なにも選択しないと、エッジセグメント化を使用するすべてのユースケースが無効になります。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/personalization/adobe-target-connection.html?lang=ja#parameters" text="データストリームの選択についての詳細"

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

Adobe Experience Platform は、会社の Adobe Target インスタンスに自動的に接続します。認証は必要ありません。

### 接続パラメーター {#parameters}

>[!CONTEXTUALHELP]
>id="platform_destinations_target_workspace"
>title="Adobe Target Workspaces について"
>abstract="オーディエンスを共有する Adobe Target Workspaces を選択します。Adobe Target 接続ごとに 1 つのワークスペースを選択できます。アクティブ化すると、該当する Experience Platform データ使用ラベルに従って、オーディエンスは選択したワークスペースにルーティングされます。"
>additional-url="https://experienceleague.adobe.com/docs/target-learn/tutorials/administration/set-up-workspaces.html?lang=ja" text="Adobe Target Workspaces の詳細情報"

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **名前**：この宛先に希望する名前を入力します。
* **説明**：宛先についての説明を入力します。例えば、この宛先を使用しているキャンペーンを指定できます。このフィールドはオプションです。
* **データストリーム ID**:これにより、オーディエンスを含めるデータ収集データストリームが決まります。 ドロップダウンメニューには、Target サービスとAdobe Experience Platformサービスが有効になっているデータストリームのみが表示されます。 詳しくは、 [データストリームの設定](../../../datastreams/configure.md#aep) Adobe Experience PlatformとAdobe Targetのデータストリームを設定する方法について詳しくは、こちらを参照してください。
   * **[!UICONTROL なし]**:Adobe Targetのパーソナライゼーションを設定する必要があるが、 [Experience PlatformWeb SDK](../../../edge/home.md). このオプションを使用する場合、Experience Platformから Target に書き出されたオーディエンスは、次セッションのパーソナライゼーションのみをサポートし、エッジセグメント化は無効になります。 詳しくは後述のテーブルを参照してください.

  | Adobe Target実装（Web SDK を使用しない） | Web SDK の実装 |
  |---|---|
  | <ul><li>データストリームは不要です。 Adobe Targetは、 [at.js](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/overview.html?lang=ja), [サーバーサイド](https://experienceleague.adobe.com/docs/target-dev/developer/overview.html?lang=en#server-side-implementation)または [ハイブリッド](https://experienceleague.adobe.com/docs/target-dev/developer/overview.html?lang=en#hybrid-implementation) 実装メソッド。</li><li>[エッジセグメント化](../../../segmentation/ui/edge-segmentation.md) はサポートされていません。</li><li>[同じページと次のページのパーソナライゼーション](../../ui/activate-edge-personalization-destinations.md) はサポートされていません。</li><li>オーディエンスとプロファイル属性は、次の場合にのみAdobe Target接続に共有できます。 *デフォルトの実稼動サンドボックス*.</li><li>データストリーム ID を使用せずに次のセッションのパーソナライゼーションを設定するには、 [at.js](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/at-js/how-atjs-works.html?lang=en).</li></ul> | <ul><li>Adobe TargetとExperience Platformがサービスとして設定されたデータストリームが必要です。</li><li>エッジのセグメント化は期待どおりに動作します。</li><li>[同じページと次のページのパーソナライゼーション](../../ui/activate-edge-personalization-destinations.md) はサポートされています。</li><li>他のサンドボックスからのオーディエンスおよびプロファイル属性の共有がサポートされます。</li></ul> |

* **Workspace**:Adobe Target [workspace](https://experienceleague.adobe.com/docs/target-learn/tutorials/administration/set-up-workspaces.html?lang=ja) オーディエンスの共有先となるもの。 Adobe Target 接続ごとに 1 つのワークスペースを選択できます。アクティベーション時に、該当する [Experience Platformデータ使用ラベル](../../../data-governance/labels/overview.md).

>[!NOTE]
>
>に対してカスタム Target ワークスペースを使用する場合 [属性を含む同じページと次のページのパーソナライゼーション](../../ui/activate-edge-personalization-destinations.md)、 [選択したオーディエンス](../../ui/activate-edge-personalization-destinations.md#select-audiences) 選択した Target ワークスペースに送信されます。 この [マップされた属性](../../ui/activate-edge-personalization-destinations.md#mapping) がデフォルトの Target ワークスペースに送信されます。
><br>
>この動作は、将来のアップデートで変更されます。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対するオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

読み取り [エッジパーソナライゼーションの宛先に対するオーディエンスのアクティブ化](../../ui/activate-edge-personalization-destinations.md) を参照してください。

## 書き出したデータ {#exported-data}

Adobe Target は、Adobe Experience Platform Edge Network からプロファイルデータを読み取るので、データはエクスポートされません。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja)を参照してください。
