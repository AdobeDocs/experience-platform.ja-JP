---
title: 宛先の概要
seo-title: 宛先の概要
description: 宛先は、リアルタイム顧客データプラットフォームからのデータのシームレスなアクティベーションを可能にする、宛先プラットフォームとの事前に構築された統合です。アドビのリアルタイム顧客データプラットフォームの宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用例に既知および不明なデータをアクティブ化できます。
seo-description: 宛先は、リアルタイム顧客データプラットフォームからのデータのシームレスなアクティベーションを可能にする、宛先プラットフォームとの事前に構築された統合です。アドビのリアルタイム顧客データプラットフォームの宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用例に既知および不明なデータをアクティブ化できます。
translation-type: ht
source-git-commit: fe9f5a94b52e51fe317fc60efa7e140f4ed93cd0

---


# 宛先の概要

**宛先**&#x200B;は、リアルタイム顧客データプラットフォームからのデータのシームレスなアクティベーションを可能にする、宛先プラットフォームとの事前に構築された統合です。**宛先**&#x200B;を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

## 宛先とソース

Real-time CDP の主な機能の 1 つは、ファーストパーティーのデータを取り込み、ビジネスニーズに合わせてアクティブ化することです。**[!UICONTROL ソース]**&#x200B;を使用し、Real-time CDP と&#x200B;**[!UICONTROL 宛先]**&#x200B;にデータを取り込んで 、Real-time CDP からデータを書き出します。

## 宛先の手順

* **[!UICONTROL 宛先]**&#x200B;を使用して、プロファイルやセグメントを[アクティブ化](/help/rtcdp/destinations/activate-destinations.md)し、マーケティング自動化プラットフォームやデジタル広告プラットフォームに送信します。
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

>[!VIDEO](https://video.tv.adobe.com/v/29707?quality=12&captions=jpn)

### プロファイルの書き出し先

プロファイル書き出し先は、プロファイルや属性を含むファイルを生成します。これらの宛先は生データを使用し、多くの場合、電子メールアドレスを主キーとして使用します。

### セグメントの書き出し先

セグメントの書き出し先は、プロファイルとそれらが認定したセグメントを送信します。これらの宛先では、セグメント ID またはユーザー ID を使用します。

### 宛先カテゴリ

[宛先カタログ](/help/rtcdp/destinations/destinations-catalog.md)内の宛先は、宛先カテゴリ（**広告**&#x200B;または&#x200B;**電子メールマーケティング**）別にグループ化されます 。それぞれについて詳しくは、「[宛先カタログ](/help/rtcdp/destinations/destinations-catalog.md)」を参照してください。

## 宛先とアクセス制御

Real-time CDP の&#x200B;**[!UICONTROL 宛先]**&#x200B;機能は、Adobe Experience Platform のアクセス制御権限と連携します。ユーザーの権限レベルに応じて、宛先を表示、管理、アクティブ化できます。個々の権限について詳しくは、「[Adobe Experience Platform でのアクセス制御](https://www.adobe.io/apis/experienceplatform/home/permissions-and-sandboxes/permissions-and-sandboxes.html#!api-specification/markdown/narrative/technical_overview/access-control/access-control-overview.md)」を参照して 、ページの下部まで下にスクロールします。

アクセス制御の詳細については、『[アクセス制御ユーザーガイド](https://www.adobe.io/apis/experienceplatform/home/permissions-and-sandboxes/permissions-and-sandboxes.html#!api-specification/markdown/narrative/technical_overview/access-control/access-control-user-guide.md)』を参照してください。