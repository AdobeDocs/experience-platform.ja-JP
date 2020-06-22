---
title: 宛先の概要
seo-title: 宛先の概要
description: 宛先は、リアルタイム顧客データプラットフォームからのデータのシームレスなアクティベーションを可能にする、宛先プラットフォームとの事前に構築された統合です。アドビのリアルタイム顧客データプラットフォームの宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用例に既知および不明なデータをアクティブ化できます。
seo-description: 宛先は、リアルタイム顧客データプラットフォームからのデータのシームレスなアクティベーションを可能にする、宛先プラットフォームとの事前に構築された統合です。アドビのリアルタイム顧客データプラットフォームの宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用例に既知および不明なデータをアクティブ化できます。
translation-type: tm+mt
source-git-commit: a61a2a4d9d51c402bb50153c06a93d255a3613cb
workflow-type: tm+mt
source-wordcount: '532'
ht-degree: 61%

---


# 宛先の概要 {#overview}

![宛先の概要バナー](/help/rtcdp/destinations/assets/destinations-overview-banner.png)

**宛先**&#x200B;は、リアルタイム顧客データプラットフォームからのデータのシームレスなアクティベーションを可能にする、宛先プラットフォームとの事前に構築された統合です。宛先を使用して、チャネル間のマーケティングキャンペーン、電子メールキャンペーン、ターゲットを絞った広告など、様々な使用例に関する既知および不明なデータをアクティブ化できます。

## 宛先とソース {#destinations-and-sources}

Real-time CDP の主な機能の 1 つは、ファーストパーティーのデータを取り込み、ビジネスニーズに合わせてアクティブ化することです。ソースを使用して、データをリアルタイムCDPに取り込み、宛先を使用して、リアルタイムCDPからデータをエクスポートします。

## 宛先の手順 {#steps}

* Real-time CDP で使用可能なすべての宛先の[セルフサービスカタログ](/help/rtcdp/destinations/destinations-catalog.md)から選択します。
* **[!UICONTROL 宛先]**&#x200B;を使用して、プロファイルやセグメントを[アクティブ化](/help/rtcdp/destinations/activate-destinations.md)し、マーケティング自動化プラットフォームやデジタル広告プラットフォームに送信します。
* データを希望の宛先に定期的に書き出しするようにスケジュールします。

## コントロール {#controls}

「[宛先](/help/rtcdp/destinations/destinations-workspace.md)」ワークスペースのコントロールを使用すると、次のことができます。

* データをアクティブ化できる宛先プラットフォームのカタログを参照する。
* カタログ内の宛先へのデータフローを作成、編集、アクティブ化、無効化する。
* ストレージの場所にアカウントを作成するか、Real-time CDP を宛先プラットフォームのアカウントにリンクする。
* 宛先に対してアクティブにするセグメントを選択する。
* 電子メールマーケティングの宛先にセグメントをアクティブ化する際に書き出す[エクスペリエンスデータモデル（XDM）フィールド](../../xdm/home.md)を選択します。

## Destination types and categories {#types-and-categories}

詳しくは、「 [送信先のタイプとカテゴリの概要](/help/rtcdp/destinations/destination-types.md)」を参照してください。

## 宛先とアクセス制御 {#access-controls}

リアルタイムCDPの宛先機能は、Adobe Experience Platformアクセス制御権限と連携します。 ユーザーの権限レベルに応じて、宛先を表示、管理、アクティブ化できます。個々の権限について詳しくは、「[Adobe Experience Platform でのアクセス制御](../../access-control/home.md)」を参照して 、ページの下部まで下にスクロールします。

アクセス制御の詳細については、『[アクセス制御ユーザーガイド](../../access-control/ui/overview.md)』を参照してください。

## 宛先へのデータのアクティブ化に関するデータ・ガバナンス制限 {#data-governance}

データ・ガバナンスは、次を通じて、リアルタイムCDPの宛先に適用されます。

* *マーケティングの使用例* 。
* *特定の使用ラベルを含むデータを、特定のマーケティング用途の宛先に対してアクティブ化できないように制限するデータ使用ポリシー* 。

マー [ケティングの使用事例](/help/rtcdp/privacy/data-governance-overview.md#destinations) 、およびデータ・ポリシー違反の [解決についての詳細は、Real-time CDPのドキュメントの「Data Governance」を参照してください](/help/rtcdp/privacy/data-governance-overview.md#enforcement)。

宛先作成ワークフローでのマーケティングの使用例の選択の詳細については、Real-time CDPの異なる宛先タイプについて、次のページを参照してください。

* [広告の表示先 — Google Ad Manager ](/help/rtcdp/destinations/google-ad-manager-destination.md)
* [広告の表示先 — Google Ads](/help/rtcdp/destinations/google-ads-destination.md)
* [広告の表示先 — Google Display &amp; Video 360 ](/help/rtcdp/destinations/google-dv360-destination.md)
* [クラウドストレージの宛先](/help/rtcdp/destinations/cloud-storage-destinations-workflow.md)
* [電子メールマーケティングの宛先 ](/help/rtcdp/destinations/email-marketing-destinations.md)
* [ソーシャルネットワークの宛先](/help/rtcdp/destinations/social-network-destinations-workflow.md)

セグメントアクティベーションワークフローでのデータポリシー違反の詳細については、「宛先へのプロファイルとセグメントの [アクティブ化](/help/rtcdp/destinations/activate-destinations.md)」の手順7を参照してください。