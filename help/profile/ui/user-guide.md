---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API;unified profile;Unified Profile;unified;Profile;rtcp;enable profile;Enable profile;Union schema;UNION PROFILE;union profile
title: リアルタイム顧客プロファイルUIガイド
topic: guide
description: リアルタイム顧客プロファイルは、オンライン、オフライン、CRM、サードパーティデータなど複数のチャネルからのデータを組み合わせて、個々の顧客の全体像を作成します。このドキュメントは、Adobe Experience Platform ユーザーインターフェイスでリアルタイム顧客プロファイルと対話するためのガイドとして機能します。
translation-type: tm+mt
source-git-commit: 295315ab052ff151bea9037567bf2690d705367b
workflow-type: tm+mt
source-wordcount: '1245'
ht-degree: 8%

---


# [!DNL Real-time Customer Profile] UIガイド

[!DNL Real-time Customer Profile] オンライン、オフライン、CRM、サードパーティなどの複数のチャネルのデータを組み合わせて、各顧客の全体的な表示を作成します。 This document serves as a guide for interacting with [!DNL Real-time Customer Profile] data in the Adobe Experience Platform user interface (UI).

## はじめに

このUIガイドでは、管理に関連する様々な [!DNL Experience Platform] サービスについて理解している必要があり [!DNL Real-time Customer Profiles]ます。 このガイドを読む前、またはUIで作業する前に、次のサービスのドキュメントを確認してください。

* [[!DNL Real-time Customer Profile]](../home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [[!DNL Identity Service]](../../identity-service/home.md):異なるデータソース [!DNL Real-time Customer Profile] に取り込まれる際に、アイデンティティを別々のデータソースからブリッジすることで有効に [!DNL Platform]します。
* [[!DNL Experience Data Model (XDM)]](../../xdm/home.md):顧客体験データを [!DNL Platform] 整理する際に使用される標準化されたフレームワーク。

## 概要

[[!DNL Experience Platform] UI](http://platform.adobe.com)( **[!UICONTROL UI]** )の左側のナビゲーションで **[!UICONTROL プロファイルを選択し、「]** 概要」タブを開きます。 このタブには、プロファイルの理解と使用の開始に役立つドキュメントとビデオへのリンクが含まれています。

![](../images/user-guide/profiles-overview.png)

## 参照

「 **[!UICONTROL 参照]** 」タブを選択して、プロファイルをIDで参照します。

![](../images/user-guide/profiles-browse.png)

### プロファイル指標 {#profile-metrics}

「 **[!UICONTROL 参照]** 」タブの右側には、プロファイルデータに関連するいくつかの重要な指標があります。この指標には、合計 [プロファイル数](#profile-count) 、名前空間別の [プロファイルのリストなどが含まれます](#profiles-by-namespace)。

これらのプロファイル指標は、組織のデフォルトの結合ポリシーを使用して評価されます。 デフォルトのマージポリシーの定義方法など、マージポリシーの操作について詳しくは、 [Merge Policiesユーザーガイドを参照してください](merge-policies.md)。

これらの指標に加えて、「プロファイル指標」セクションには、指標が最後に評価された日時を示す最終更新日時も表示されます。

![](../images/user-guide/profiles-profile-metrics.png)

### プロファイル数 {#profile-count}

The profile count displays the total number of profiles your organization has within [!DNL Experience Platform], after your organization&#39;s default merge policy has merged together profile fragments to form a single profile for each individual customer. つまり、様々なチャネルでブランドとやり取りする 1 人の顧客に関連する複数のプロファイルフラグメントが組織に存在する場合でも、これらのフラグメントは、1 個人に関連しているため（デフォルトの結合ポリシーに従って）結合され、プロファイルの数が「1」として返されます。

プロファイル数には、属性（レコードデータ）を持つプロファイルと、Adobe Analyticsプロファイルなどの時系列(イベント)データのみを含むプロファイルの両方が含まれます。 プロファイル数は定期的に更新され、プラットフォーム内のプロファイルの総数を最新の状態に更新します。

ストアへのレコードの取り込みがカウントを5%以上増減すると、ジョブがトリガされ、カウントが更新される。 [!DNL Profile] ストリーミングデータワークフローの場合、5%増減のしきい値に達したかどうかを判断するために、1時間ごとにチェックが行われます。 ジョブが存在する場合は、プロファイル数を更新するためにジョブが自動的にトリガされます。 バッチ取り込みの場合、バッチをプロファイルストアに正常に取り込んでから15分以内に、5%の増減のしきい値に達すると、ジョブが実行され、プロファイル数が更新されます。

### 名前空間別プロファイル数 {#profiles-by-namespace}

名前空間別 **[!UICONTROL プロファイル数指標は]** 、プロファイルストア内の結合されたすべてのプロファイルにおける名前空間の合計数および分類を表示します。 1つのプロファイルに複数の名前空間が関連付けられている場合があるので、名前空間別のプロファイルの合計数(つまり、各名前空間に表示される値を合計した数)は、常にプロファイル数指標より多くなります。 例えば、ある顧客が複数のチャネルで自社のブランドとやり取りした場合、複数の名前空間がその個々の顧客に関連付けられます。

プロファイル数指標と同様に [、レコードを](#profile-count)[!DNL Profile] ストアに取り込むと、カウントが5%以上増減すると、ジョブがトリガーされて名前空間指標が更新されます。 ストリーミングデータワークフローの場合、5%増減のしきい値に達したかどうかを判断するために、1時間ごとにチェックが行われます。 ジョブが存在する場合は、プロファイル数を更新するためにジョブが自動的にトリガされます。 バッチ取り込みの場合、5%の増減のしきい値に達した場合は、バッチを [!DNL Profile] ストアに取り込んで15分以内にジョブが実行され、指標が更新されます。

### マージポリシー

「 **[!UICONTROL Merge policy]** 」セレクターを使用すると、組織のデフォルトの結合ポリシーが自動的に選択されます。 このマージポリシーを使用しない場合は、デフォルトのマージポリシーの `X` 横にあるを選択して、 **[!UICONTROL Select merge policy]** （マージポリシーの選択）ダイアログを開き、別のマージポリシーを選択できます。 結合ポリシーとプラットフォーム内でのその役割について詳しくは、 [結合ポリシーのUIガイドを参照してください](merge-policies.md)。

![](../images/user-guide/profiles-search-merge-policy.png)

### ID名前空間

「 **[!UICONTROL ID名前空間]** 」セレクターを開くと、検索するID名前空間を選択できるダイアログが開きます。また、フィルターアイコンを選択し、追加または削除する属性を選択して、検索から表示する属性をカスタマイズできます。

![](../images/user-guide/profiles-search-filter.png)

[ **[!UICONTROL ID名前空間の]** 選択]ダイアログで、検索に使用する名前空間を選択するか、ダイアログの検索バーを使用して名前空間の名前を入力し始めます。 追加の詳細を表示する名前空間を選択できます。使用する名前空間を見つけたら、ラジオボタンを選択して **[!UICONTROL Select]** （選択）を押して続行します。

![](../images/user-guide/profiles-select-identity-namespace.png)

### ID値

ID名前空間を選択した後、「 **[!UICONTROL 参照]** 」タブに戻り、 **[!UICONTROL ID値を入力できます]**。 この値は個々の顧客プロファイルに固有の値で、提供される名前空間に対して有効なエントリである必要があります。 例えば、「電子メール」というID名前空間を選択するには、有効な電子メールアドレスの形式のID値が必要です。

![](../images/user-guide/profiles-show-profile.png)

値を入力したら、「 **[!UICONTROL プロファイルを表示]** 」を選択すると、値に一致する1つのプロファイルが返されます。 プロファイルの詳細を表示する **[!UICONTROL プロファイルID]** を選択します。

![](../images/user-guide/profiles-display-profile.png)

### プロファイルの詳細 {#profile-detail}

**[!UICONTROL プロファイルIDを選択すると]**、「 **[!UICONTROL 詳細]** 」タブが開きます。 The profile information displayed on the **[!UICONTROL Detail]** tab has been merged together from multiple profile fragments to form a single view of the individual customer. 基本属性、リンクされたID、チャネル環境設定などの顧客の詳細情報も含まれます。 表示されるデフォルトのフィールドは、組織レベルで変更して、希望のプロファイル属性を表示することもできます。 属性の追加と削除、ダッシュボードパネルのサイズ変更の手順など、これらのフィールドのカスタマイズの詳細については、『 [プロファイルの詳細カスタマイズガイド](profile-customization.md)』を参照してください。

![](../images/user-guide/profiles-profile-detail.png)

個々のプロファイルに関連する追加情報を表示するには、利用可能な別のタブを選択します。 これらのタブには、属性、イベントおよびセグメントのメンバーシップが含まれ、プロファイルが現在資格を持っているセグメントを示します。

![](../images/user-guide/profiles-attributes-events-segments.png)

## 結合ポリシー

メイン **[!UICONTROL プロファイル]** (Merge Policies **[!UICONTROL )メニューで、「]** Merge Policies」タブを選択して、組織に属するマージポリシーのリストを表示します。 表示される各ポリシーは、名前、それがデフォルトのマージポリシーかどうか、および適用先のスキーマクラスを表示します。

For more information on merge policies, see the [merge policies UI guide](merge-policies.md).

![](../images/user-guide/profiles-merge-policies.png)

## 和集合スキーマ {#union-schema}

メイン **[!UICONTROL プロファイル]** (Main Menu)メニューで **[!UICONTROL 、「]** 和集合スキーマ」(Select Data)タブを選択して、取り込むデータの和集合スキーマを表示します。 A union schema is an amalgamation of all [!DNL Experience Data Model] (XDM) fields under the same class, whose schemas have been enabled for use in [!DNL Real-time Customer Profile].

和集合スキーマについて詳しくは、『 [和集合スキーマUIガイド](union-schema.md)』を参照してください。

![](../images/user-guide/profiles-union-schema.png)

## 次の手順

By reading this guide, you now know how to view and manage your [!DNL Profile] data using the [!DNL Experience Platform] UI. Real-time Customer Comment APIを使用したプロファイルデータの操作方法について詳しくは、 [プロファイル開発ガイドを参照してください](../api/overview.md)。