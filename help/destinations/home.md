---
keywords: RTCDP;CDP;Real-time Customer Data Platform;real time customer data platform;real time cdp;cdp;destinations;destination;rtcdp
title: 宛先の概要
seo-title: 宛先の概要
description: クロスチャネルのマーケティングキャンペーン、電子メール、ターゲット広告などの宛先に対し、Platform データを活用します。
seo-description: 宛先は、リアルタイム顧客データプラットフォームからのデータのシームレスなアクティベーションを可能にする、宛先プラットフォームとの事前定義済みの統合です。Real-time Customer Data PlatformのDestinationsを使用すると、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告など、様々な使用例に関する既知および不明なデータをアクティブ化できます。
translation-type: tm+mt
source-git-commit: 5f120a716cc3396ef7749463bb6052a8ced2fbb4
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 63%

---


# [!DNL Destinations] 概要 {#overview}

![宛先の概要バナー](./assets/overview/destinations-overview-banner.png)

**[!DNL Destinations]** は、目的のプラットフォームと事前にビルドされた統合機能で、リアルタイム顧客データプラットフォームからのデータをシームレスにアクティベーションできます。 宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

## 宛先とソース {#destinations-and-sources}

リアルタイム CDP の主な機能の 1 つは、ファーストパーティーのデータを取り込み、ビジネスニーズに合わせてアクティブ化することです。ソースを使用し、リアルタイム CDP と宛先にデータを取得して、リアルタイム CDP からデータを書き出します。

## 宛先の手順 {#steps}

* リアルタイム CDP で使用可能なすべての宛先の[セルフサービスカタログ](./catalog/overview.md)から選択します。
* **[!UICONTROL 宛先]**&#x200B;を使用して、プロファイルやセグメントを[アクティブ化](./ui/activate-destinations.md)し、マーケティング自動化プラットフォームやデジタル広告プラットフォームに送信します。
* データを希望の宛先に定期的に書き出しするようにスケジュールします。

## コントロール {#controls}

「[宛先](./ui/destinations-workspace.md)」ワークスペースのコントロールを使用すると、次のことができます。

* データをアクティブ化できる宛先プラットフォームのカタログを参照する。
* カタログ内の宛先へのデータフローを作成、編集、アクティブ化、無効化する。
* ストレージの場所にアカウントを作成するか、リアルタイム CDP を宛先プラットフォームのアカウントにリンクする。
* 宛先に対してアクティブにするセグメントを選択する。
* 電子メールマーケティングの宛先にセグメントをアクティブ化する際に書き出す[エクスペリエンスデータモデル（XDM）フィールド](../xdm/home.md)を選択します。

## 宛先のタイプとカテゴリ {#types-and-categories}

詳しくは、「[宛先のタイプとカテゴリ](./destination-types.md)」の概要を参照してください。

## 宛先とアクセス制御 {#access-controls}

リアルタイム CDP の宛先機能は、Adobe Experience Platform のアクセス制御権限と連携します。ユーザーの権限レベルに応じて、宛先を表示、管理、アクティブ化できます。個々の権限について詳しくは、「[Adobe Experience Platform でのアクセス制御](../access-control/home.md)」を参照して、ページの下部まで下にスクロールします。

アクセス制御の詳細については、「[アクセス制御ユーザーガイド](../access-control/ui/overview.md)」を参照してください。

## [!DNL Data Governance] 宛先へのデータのアクティブ化に関する制限 {#data-governance}

データ・ガバナンスは、次を通じて、リアルタイムCDPの宛先に適用されます。

* *マーケティングの使用例* 。
* *特定の使用ラベルを含むデータを、特定のマーケティング使用例のある宛先に対してアクティブ化できないように制限するデータ使用ポリシー* 。

マー [!DNL Data Governance] ケティングの使用事例 [とデータ・ポリシー違反の](../rtcdp/privacy/data-governance-overview.md#destinations) 解決についての詳細は、「 Real-time CDP 」のドキュメントを参照してください [](../rtcdp/privacy/data-governance-overview.md#enforcement)。

宛先作成ワークフローでのマーケティングの使用例の選択の詳細については、Real-time CDPの異なる宛先タイプについて、次のページを参照してください。

* [広告の表示先 — Google Ad Manager ](./catalog/advertising/google-ad-manager.md)
* [広告の表示先 — Google Ads](./catalog/advertising/google-ads-destination.md)
* [広告の表示先 — Google Display &amp; Video 360 ](./catalog/advertising/google-dv360.md)
* [クラウドストレージの宛先](./catalog/cloud-storage/workflow.md)
* [電子メールマーケティングの宛先](./catalog/email-marketing/overview.md)
* [ソーシャルネットワークの宛先](./catalog/social/workflow.md)

セグメントアクティベーションワークフローでのデータポリシー違反の詳細については、「宛先へのプロファイルとセグメントの [アクティブ化](./ui/activate-destinations.md#review)」の確認手順を参照してください。