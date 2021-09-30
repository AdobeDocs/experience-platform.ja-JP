---
keywords: target のパーソナライゼーション；宛先；experience platform ターゲットの宛先；adobe target の宛先；
title: Adobe Target接続
description: Adobe Targetは、Web サイトやモバイルアプリなどのすべてのインバウンド顧客インタラクションで、リアルタイム、 1:1 および AI を利用したパーソナライゼーションと実験を提供するアプリケーションです。
source-git-commit: caccd096c9165139d9b966bbfcb311456276192a
workflow-type: tm+mt
source-wordcount: '391'
ht-degree: 13%

---


# Adobe Target connection（ベータ版） {#adobe-target-connection}

## 概要 {#overview}

>[!IMPORTANT]
>
>Adobe Experience PlatformのAdobe Target接続は現在ベータ版です。 ドキュメントと機能は変更される場合があります。

Adobe Targetは、Web サイトやモバイルアプリなどのすべてのインバウンド顧客インタラクションで、リアルタイム、1:1 、 AI を利用したパーソナライゼーションと実験を提供するアプリケーションです。

Adobe Targetは、Adobe Experience Platformのパーソナライゼーション接続です。

## 前提条件 {#prerequisites}

この統合は、[Adobe Experience Platform Web SDK](../../../edge/home.md) によって実行されます。 この宛先を使用するには、この SDK を使用する必要があります。

## 書き出しタイプ {#export-type}

**プロファイルリクエスト**  - Adobe Targetの宛先にマッピングされるすべてのセグメントを、単一のプロファイルでリクエストします。

## ユースケース {#use-cases}

**ホームページバナーのパーソナライズ**

レンタル販売の会社は、Adobe Experience Platformでの顧客セグメントの資格に基づいて、自社のホームページをバナー付きでパーソナライズしたいと考えています。 会社は、パーソナライズされたエクスペリエンスを得るオーディエンスを選択し、それらを Target オファーのターゲット条件としてAdobe Targetに送信できます。

## 宛先に接続 {#connect}

この宛先に接続するには、[ 宛先の設定に関するチュートリアル ](../../ui/connect-destination.md) で説明されている手順に従います。

Adobe Experience Platformは、会社のAdobe Targetインスタンスに自動的に接続します。 認証は不要です。

### 接続パラメーター {#parameters}

[ この宛先を設定 ](../../ui/connect-destination.md) する際は、次の情報を指定する必要があります。

* **名前**：この宛先の名前を入力します。
* **説明**：宛先の説明を入力します。例えば、この宛先を使用しているキャンペーンを指定できます。このフィールドはオプションです。
* **データストリーム ID**:これにより、ページへの応答に含まれるデータ収集データストリームを決定します。ドロップダウンメニューには、宛先設定が有効になっているデータストリームのみが表示されます。 詳しくは、[ データストリームの設定 ](../../../edge/fundamentals/datastreams.md) を参照してください。

## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対するオーディエンスセグメントをアクティブ化する手順については、[ プロファイルリクエストの宛先へのプロファイルとセグメントのアクティブ化 ](../../ui/activate-profile-request-destinations.md) を参照してください。

## エクスポートされたデータ {#exported-data}

Adobe Targetは、Adobe Experience Platform Edge Network からプロファイルデータを読み取るので、データは書き出されません。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] の宛先はすべて、データを処理する際のデータ使用ポリシーに準拠しています。 [!DNL Adobe Experience Platform] によるデータガバナンスの強制方法について詳しくは、「[ データガバナンスの概要 ](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja)」を参照してください。
