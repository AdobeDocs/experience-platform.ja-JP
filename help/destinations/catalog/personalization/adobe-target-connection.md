---
keywords: ターゲットのパーソナライゼーション;宛先;Experience Platform ターゲットの宛先:Adobe Target の宛先;
title: Adobe Target 接続
description: Adobe Target は、web サイトやモバイルアプリなど、すべてのインバウンド顧客とのインタラクションで、AI を利用したリアルタイムのパーソナライズと実験の機能を提供するアプリケーションです。
exl-id: 3e3c405b-8add-4efb-9389-5ad695bc9799
source-git-commit: fb0d8aedbb88aad8ed65592e0b706bd17840406b
workflow-type: tm+mt
source-wordcount: '591'
ht-degree: 76%

---

# Adobe Target 接続 {#adobe-target-connection}

## 概要 {#overview}

Adobe Target は、web サイトやモバイルアプリなど、すべてのインバウンド顧客とのインタラクションで、AI を利用したリアルタイムのパーソナライズと実験の機能を提供するアプリケーションです。

Adobe Target は、Adobe Experience Platform のパーソナライゼーション接続です。

## 前提条件 {#prerequisites}

この統合は、[Adobe Experience Platform Web SDK](../../../edge/home.md) によって実現します。宛先を使用するには、この SDK を使用する必要があります。

>[!IMPORTANT]
>
>[!DNL Adobe Target] 接続を作成する前に、[同じページと次のページのパーソナライズのためのパーソナライズ機能宛先の設定](../../ui/configure-personalization-destinations.md)を行う方法を参照してください。このガイドでは、複数の Experience Platform コンポーネントをまたいで、同じページおよび次のページのパーソナライゼーションの使用例に必要な設定手順を説明します。

## 書き出しタイプ および頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!DNL Profile request]** | 単一のプロファイルに対して、Adobe Targetの宛先にマッピングされているすべてのセグメントを要求しています。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は、API ベースの接続です。 セグメント評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 詳細を表示 [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations). |

{style=&quot;table-layout:auto&quot;}

## ユースケース {#use-cases}

**ホームページバナーのパーソナライズ**

家庭用品のレンタルと販売を行う会社が、Adobe Experience Platform での顧客セグメント選定に基づいて、自身のホームページをバナー付きでパーソナライズしたいと考えています。会社は、パーソナライズされたエクスペリエンスを体験できるオーディエンスを選択し、それらのオーディエンスを Target オファーのターゲッティング条件として Adobe Target に送信できます。

## 宛先への接続 {#connect}

>[!CONTEXTUALHELP]
>id="platform_destinations_target_datastream"
>title="データストリーム ID について"
>abstract="このオプションは、ページへの応答にセグメントを含めるデータ収集データストリームを決定します。ドロップダウンメニューには、宛先設定が有効になっているデータストリームのみが表示されます。宛先を設定する前に、データストリームを設定する必要があります。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/datastreams.html?lang=ja" text="データストリームの設定方法を説明します"

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

Adobe Experience Platform は、会社の Adobe Target インスタンスに自動的に接続します。認証は必要ありません。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **名前**：この宛先に任意の名前を入力します。
* **説明**：宛先についての説明を入力します。例えば、この宛先を使用しているキャンペーンを指定できます。このフィールドはオプションです。
* **データストリーム ID**：これにより、ページへの応答にセグメントを含めるデータ収集データストリームが決定されます。ドロップダウンメニューには、宛先設定が有効になっているデータストリームのみが表示されます。詳しくは、[データストリームの設定](../../../edge/datastreams/overview.md)を参照してください。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先にオーディエンスセグメントを有効化する手順については、[プロファイルリクエストの宛先へのプロファイルとセグメントの有効化](../../ui/activate-profile-request-destinations.md)を参照してください。

## 書き出したデータ {#exported-data}

Adobe Target は、Adobe Experience Platform Edge Network からプロファイルデータを読み取るので、データはエクスポートされません。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja)を参照してください。
