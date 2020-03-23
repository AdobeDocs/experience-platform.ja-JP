---
title: 宛先の概要
seo-title: 宛先の概要
description: 宛先は、リアルタイム顧客データプラットフォームからのデータのシームレスなアクティベーションを可能にする、宛先プラットフォームとの事前に構築された統合です。アドビのリアルタイム顧客データプラットフォームの宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用例に既知および不明なデータをアクティブ化できます。
seo-description: 宛先は、リアルタイム顧客データプラットフォームからのデータのシームレスなアクティベーションを可能にする、宛先プラットフォームとの事前に構築された統合です。アドビのリアルタイム顧客データプラットフォームの宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用例に既知および不明なデータをアクティブ化できます。
translation-type: tm+mt
source-git-commit: b4b02913467c43d004574a1aa5cb2f793964ad1e

---


# 宛先の概要

**宛先**&#x200B;は、リアルタイム顧客データプラットフォームからのデータのシームレスなアクティベーションを可能にする、宛先プラットフォームとの事前に構築された統合です。**宛先**&#x200B;を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

## 宛先とソース

Real-time CDP の主な機能の 1 つは、ファーストパーティーのデータを取り込み、ビジネスニーズに合わせてアクティブ化することです。Use **[!UICONTROL Sources]** to ingest data into Real-time CDP and **[!UICONTROL Destinations]** to export data from Real-time CDP.

## 宛先の手順

* Use **[!UICONTROL Destinations]** to [activate](/help/rtcdp/destinations/activate-destinations.md) and send profiles or segments to marketing automation platforms, digital advertising platforms, and more.
* Real-time CDP で使用可能なすべての宛先の[セルフサービスカタログ](/help/rtcdp/destinations/destinations-catalog.md)から選択します。
* データを希望の宛先に定期的に書き出しするようにスケジュールします。

## コントロール

「[宛先](/help/rtcdp/destinations/destinations-workspace.md)」ワークスペースのコントロールを使用すると、次のことができます。

* データをアクティブ化できる宛先プラットフォームのカタログを参照する。
* カタログ内の宛先へのデータフローを作成、編集、アクティブ化、無効化する。
* ストレージの場所にアカウントを作成するか、Real-time CDP を宛先プラットフォームのアカウントにリンクする。
* 宛先に対してアクティブにするセグメントを選択する。
* 電子メールマーケティングの宛先にセグメントをアクティブ化する際に書き出す[エクスペリエンスデータモデル（XDM）フィールド](https://www.adobe.io/apis/experienceplatform/home/xdm/xdmservices.html#!api-specification/markdown/narrative/technical_overview/schema_registry/xdm_system/xdm_system_in_experience_platform.md)を選択します。

## 宛先の種類とカテゴリ — ビデオの概要

Adobe Real-time CDP には、プロファイルの書き出し先とセグメントの書き出し先の 2 種類の宛先があります。以下のビデオでは、2 種類の宛先について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/29707?quality=12)

### プロファイルの書き出し先

プロファイル書き出し先は、プロファイルや属性を含むファイルを生成します。これらの宛先は生データを使用し、多くの場合、電子メールアドレスを主キーとして使用します。

### セグメントの書き出し先

セグメントの書き出し先は、プロファイルとそれらが認定したセグメントを送信します。これらの宛先では、セグメント ID またはユーザー ID を使用します。

### 宛先カテゴリ

The destinations in the [Destinations catalog](/help/rtcdp/destinations/destinations-catalog.md) are grouped by destination category (**Advertising**, **Cloud storage**, or **Email marketing**). それぞれについて詳しくは、「[宛先カタログ](/help/rtcdp/destinations/destinations-catalog.md)」を参照してください。

## 宛先とアクセス制御

The **[!UICONTROL Destinations]** functionality in Real-time CDP works with Adobe Experience Platform access control permissions. ユーザーの権限レベルに応じて、宛先を表示、管理、アクティブ化できます。個々の権限について詳しくは、「[Adobe Experience Platform でのアクセス制御](https://www.adobe.io/apis/experienceplatform/home/permissions-and-sandboxes/permissions-and-sandboxes.html#!api-specification/markdown/narrative/technical_overview/access-control/access-control-overview.md)」を参照して 、ページの下部まで下にスクロールします。

アクセス制御の詳細については、『[アクセス制御ユーザーガイド](https://www.adobe.io/apis/experienceplatform/home/permissions-and-sandboxes/permissions-and-sandboxes.html#!api-specification/markdown/narrative/technical_overview/access-control/access-control-user-guide.md)』を参照してください。