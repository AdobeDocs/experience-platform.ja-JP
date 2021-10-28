---
keywords: ターゲットのパーソナライゼーション；宛先；experience platform ターゲットの宛先；adobe target の宛先；
title: Adobe Target接続（ベータ版）
description: Adobe Targetは、Web サイト、モバイルアプリなどのすべてのインバウンド顧客インタラクションで、リアルタイム、1:1 および AI を利用したパーソナライゼーションと実験を提供するアプリケーションです。
exl-id: 3e3c405b-8add-4efb-9389-5ad695bc9799
source-git-commit: fae3d9a5aff3e84354831026e9724e1c85d32b5c
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 11%

---

# Adobe Target接続（ベータ版） {#adobe-target-connection}

## 概要 {#overview}

>[!IMPORTANT]
>
>Adobe Experience PlatformのAdobe Target接続は現在ベータ版です。 ドキュメントと機能は変更される場合があります。

Adobe Targetは、Web サイト、モバイルアプリなどのすべてのインバウンド顧客インタラクションで、AI を利用したパーソナライゼーションと実験をリアルタイムで提供するアプリケーションです。

Adobe Targetは、Adobe Experience Platformのパーソナライゼーション接続です。

## 前提条件 {#prerequisites}

この統合は、 [Adobe Experience Platform Web SDK](../../../edge/home.md). この宛先を使用するには、この SDK を使用する必要があります。

## 書き出しタイプ {#export-type}

**プロファイルリクエスト** ：単一のプロファイルに対して、Adobe Targetの宛先にマッピングされているすべてのセグメントを要求しています。

## ユースケース {#use-cases}

**ホームページバナーのパーソナライズ**

あるレンタル販売会社は、Adobe Experience Platformでの顧客セグメントの資格に基づいて、自社のホームページをバナー付きでパーソナライズしたいと考えています。 会社は、パーソナライズされたエクスペリエンスを取得するオーディエンスを選択し、それらを Target オファーのターゲット条件としてAdobe Targetに送信できます。

## 宛先に接続 {#connect}

>[!CONTEXTUALHELP]
>id="platform_destinations_target_datastream"
>title="データストリーム ID について"
>abstract="このオプションは、ページへの応答にセグメントを含めるデータ収集データストリームを決定します。 ドロップダウンメニューには、宛先設定が有効になっているデータストリームのみが表示されます。 宛先を設定する前に、データストリームを設定する必要があります。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/datastreams.html?lang=en" text="データストリームの設定方法を説明します。"

この宛先に接続するには、 [宛先設定のチュートリアル](../../ui/connect-destination.md).

Adobe Experience Platformは、会社のAdobe Targetインスタンスに自動的に接続します。 認証は必要ありません。

### 接続パラメーター {#parameters}

While [設定](../../ui/connect-destination.md) この宛先には、次の情報を指定する必要があります。

* **名前**：この宛先の名前を入力します。
* **説明**：宛先の説明を入力します。例えば、この宛先を使用しているキャンペーンを指定できます。このフィールドはオプションです。
* **データストリーム ID**:これにより、ページへの応答にセグメントを含めるデータ収集データストリームが決定されます。 ドロップダウンメニューには、宛先設定が有効になっているデータストリームのみが表示されます。 詳しくは、 [データストリームの設定](../../../edge/fundamentals/datastreams.md) を参照してください。

## この宛先へのセグメントのアクティブ化 {#activate}

読み取り [プロファイルリクエストの宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-profile-request-destinations.md) を参照してください。

## 書き出されたデータ {#exported-data}

Adobe Targetは、Adobe Experience Platform Edge Network からプロファイルデータを読み取るので、データは書き出されません。

## データの使用とガバナンス {#data-usage-governance}

すべて [!DNL Adobe Experience Platform] の宛先は、データを処理する際のデータ使用ポリシーに準拠しています。 詳しくは、 [!DNL Adobe Experience Platform] データガバナンスを強制し、 [データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja).
