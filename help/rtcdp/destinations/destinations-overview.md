---
title: 宛先の概要
seo-title: 宛先の概要
description: 宛先は、リアルタイム顧客データプラットフォームからのデータのシームレスなアクティブ化を可能にする、宛先プラットフォームとの事前に構築された統合です。 アドビリアルタイム顧客データプラットフォームの宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用例に関する既知および不明なデータをアクティブ化できます。
seo-description: 宛先は、リアルタイム顧客データプラットフォームからのデータのシームレスなアクティブ化を可能にする、宛先プラットフォームとの事前に構築された統合です。 アドビリアルタイム顧客データプラットフォームの宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用例に関する既知および不明なデータをアクティブ化できます。
translation-type: tm+mt
source-git-commit: fe9f5a94b52e51fe317fc60efa7e140f4ed93cd0

---


# 宛先の概要

**宛先は** 、リアルタイム顧客データプラットフォームからのデータのシームレスなアクティブ化を可能にする、宛先プラットフォームとの事前に構築された統合です。 「宛先」を使用して **** 、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

## 宛先とソース

リアルタイムCDPの主な機能の1つは、ファーストパーティのデータを取り込み、ビジネスニーズに合わせてアクティブ化することです。 ソース **[!UICONTROL を使用し]** 、リアルタイムCDPと宛先にデータを取り込んで **[!UICONTROL 、リアルタイム]** CDPからデータをエクスポートします。

## 宛先の手順

* 宛先を使 **[!UICONTROL 用して]** 、プロフ [](/help/rtcdp/destinations/activate-destinations.md) ァイルやセグメントをアクティブ化し、マーケティング自動化プラットフォームやデジタル広告プラットフォームに送信します。
* リアルタイムCDP [で使用可能なすべての宛先の](/help/rtcdp/destinations/destinations-catalog.md) 、セルフサービスカタログから選択します。
* データを希望の宛先に定期的にエクスポートするようにスケジュールします。

## コントロール

「宛先」ワークスペースのコ [ントロールを使用し](/help/rtcdp/destinations/destinations-workspace.md) 、次のことができます。

* データをアクティブ化できる宛先プラットフォームのカタログを参照します。
* カタログ内の宛先へのデータフローを作成、編集、アクティブ化および無効化します。
* ストレージの場所にアカウントを作成するか、リアルタイムCDPをデスティネーションプラットフォームのアカウントにリンクします。
* 宛先に対してアクティブにするセグメントを選択します。
* 電子メールマーケ [ティングの宛先にセグメントをアクティブ化する際に](https://www.adobe.io/apis/experienceplatform/home/xdm/xdmservices.html#!api-specification/markdown/narrative/technical_overview/schema_registry/xdm_system/xdm_system_in_experience_platform.md) 、エクスポートするエクスペリエンスデータモデル(XDM)フィールドを選択します。

## リンク先のタイプとカテゴリ — ビデオの概要

Adobe Real-time CDPには、プロファイルエクスポート先とセグメントエクスポート先の2種類の宛先があります。 以下のビデオでは、2種類の宛先について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/29707?quality=12)

### プロファイルの書き出し先

プロファイルエクスポート先は、プロファイルや属性を含むファイルを生成します。 これらの宛先は生データを使用し、多くの場合、電子メールアドレスを主キーとして使用します。

### セグメントエクスポート先

セグメントのエクスポート先は、プロファイルとそれらが認定したセグメントを送信します。 これらの宛先では、セグメントIDまたはユーザーIDを使用します。

### 宛先カテゴリ

宛先カタログ内の宛先は、宛 [先カテゴリ](/help/rtcdp/destinations/destinations-catalog.md) (広告または電子メールマーケ&#x200B;**ティング** )別にグループ化されます ****。 各リンク先について詳しくは、「リンク先カタログ」を参照 [してください](/help/rtcdp/destinations/destinations-catalog.md)。

## 宛先とアクセス制御

リアル **[!UICONTROL タイム]** CDPの宛先機能は、Adobe Experience Platformのアクセス制御権限と連携します。 ユーザーの権限レベルに応じて、宛先を表示、管理およびアクティブ化できます。 個々の権限について詳しくは、「Adobe Experience Platformでのア [クセス制御」を参照して](https://www.adobe.io/apis/experienceplatform/home/permissions-and-sandboxes/permissions-and-sandboxes.html#!api-specification/markdown/narrative/technical_overview/access-control/access-control-overview.md) 、ページの下部まで下にスクロールします。

アクセス制御の詳細については、『 [Access control user guide』を参照してください](https://www.adobe.io/apis/experienceplatform/home/permissions-and-sandboxes/permissions-and-sandboxes.html#!api-specification/markdown/narrative/technical_overview/access-control/access-control-user-guide.md)。