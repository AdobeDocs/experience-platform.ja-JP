---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API;unified profile;Unified Profile;unified;Profile;rtcp;enable profile;Enable profile
solution: Adobe Experience Platform
title: リアルタイム顧客プロファイルユーザガイド
topic: guide
description: リアルタイム顧客プロファイルは、オンライン、オフライン、CRM、サードパーティデータなど複数のチャネルからのデータを組み合わせて、個々の顧客の全体像を作成します。このドキュメントは、Adobe Experience Platform ユーザーインターフェイスでリアルタイム顧客プロファイルと対話するためのガイドとして機能します。
translation-type: tm+mt
source-git-commit: bf99b08a1093a815687cc06372407949e170a0b3
workflow-type: tm+mt
source-wordcount: '1191'
ht-degree: 14%

---


# [!DNL Real-time Customer Profile] ユーザーガイド

[!DNL Real-time Customer Profile] オンライン、オフライン、CRM、サードパーティなどの複数のチャネルのデータを組み合わせて、各顧客の全体的な表示を作成します。

This document serves as a guide for interacting with [!DNL Real-time Customer Profile] in the Adobe Experience Platform user interface.

## はじめに

This user guide requires an understanding of the various [!DNL Experience Platform] services involved with managing [!DNL Real-time Customer Profiles]. このユーザガイドを読む前に、次のサービスのドキュメントを確認してください。

* [!DNL Real-time Customer Profile](../home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [!DNL Identity Service](../../identity-service/home.md):異なるデータソース [!DNL Real-time Customer Profile] に取り込まれる際に、アイデンティティを別々のデータソースからブリッジすることで有効に [!DNL Platform]します。
* [!DNL Experience Data Model (XDM)](../../xdm/home.md):顧客体験データを [!DNL Platform] 整理するための標準化されたフレームワーク。

## 概要

で、左側のナビゲーションで [!DNL Experience Platform UI](http://platform.adobe.com)プロファイル **[!UICONTROL (]** Overview _[!UICONTROL )をクリックし、「]_概要」タブを開きます。 このタブには、プロファイルの理解と使用の開始に役立つドキュメントとビデオへのリンクが含まれています。

![](../images/user-guide/profiles-overview.png)

## 参照

「 *[!UICONTROL 参照]* 」タブを選択して、プロファイルをIDで参照します。

![](../images/user-guide/profiles-browse.png)

### プロファイル指標 {#profile-metrics}

「 *[!UICONTROL 参照]* 」タブの右側には、プロファイルデータに関連するいくつかの重要な指標があります。この指標には、合計 [プロファイル数](#profile-count) 、名前空間別の [プロファイルのリストなどが含まれます](#profiles-by-namespace)。

これらのプロファイル指標は、組織のデフォルトの結合ポリシーを使用して評価されます。 デフォルトのマージポリシーの定義方法など、マージポリシーの操作について詳しくは、 [Merge Policiesユーザーガイドを参照してください](merge-policies.md)。

これらの指標に加えて、「プロファイル指標」セクションには *、指標が最後に評価された日時を示す* 最終更新日時も表示されます。

![](../images/user-guide/profiles-profile-metrics.png)

### プロファイル数 {#profile-count}

The profile count displays the total number of profiles your organization has within [!DNL Experience Platform], after your organization&#39;s default merge policy has merged together profile fragments to form a single profile for each individual customer. つまり、様々なチャネルでブランドとやり取りする 1 人の顧客に関連する複数のプロファイルフラグメントが組織に存在する場合でも、これらのフラグメントは、1 個人に関連しているため（デフォルトの結合ポリシーに従って）結合され、プロファイルの数が「1」として返されます。

プロファイル数には、属性（レコードデータ）を持つプロファイルと、Adobe Analyticsプロファイルなどの時系列(イベント)データのみを含むプロファイルの両方が含まれます。 プロファイル数は定期的に更新され、プラットフォーム内のプロファイルの総数を最新の状態に更新します。

レコードの取り込みによってカウントが5%以上増加または減少すると、ジョブがトリガされ、カウントが更新されます。 [!DNL Profile Store] ストリーミングデータワークフローの場合、5%増減のしきい値に達したかどうかを判断するために、1時間ごとにチェックが行われます。 ジョブが存在する場合は、プロファイル数を更新するためにジョブが自動的にトリガされます。 バッチ取り込みの場合、バッチをプロファイルストアに正常に取り込んでから15分以内に、5%の増減のしきい値に達すると、ジョブが実行され、プロファイル数が更新されます。

### 名前空間別プロファイル数 {#profiles-by-namespace}

名前空間別 *[!UICONTROL プロファイル数指標は]* 、プロファイルストア内の結合されたすべてのプロファイルにおける名前空間の合計数および分類を表示します。 1つのプロファイルに複数の名前空間が関連付けられている場合があるので、名前空間別のプロファイルの合計数(つまり、各名前空間に表示される値を合計した数)は、常にプロファイル数指標より多くなります。 例えば、ある顧客が複数の顧客で自社のブランドとやり取りした場合、複数の名前空間がそのチャネルに関連付けられます。

プロファイル数指標と同様に [、レコードの](#profile-count)[!DNL Profile Store] 取り込み先のカウントが5%以上増減すると、名前空間指標の更新を求めるジョブがトリガされます。 ストリーミングデータワークフローの場合、5%増減のしきい値に達したかどうかを判断するために、1時間ごとにチェックが行われます。 ジョブが存在する場合は、プロファイル数を更新するためにジョブが自動的にトリガされます。 バッチ取り込みの場合、5%の増減のしきい値に達した場合は、バッチを正常に取り込んで15分以内に [!DNL Profile Store]、ジョブが実行されて指標が更新されます。

### マージポリシー

「 **[!UICONTROL Merge policy]** 」セレクターを使用すると、組織のデフォルトの結合ポリシーが自動的に選択されます。 このマージポリシーを使用しない場合は、デフォルトのマージポリシーの `X` 横にあるを選択して、別のマージポリシーを選択できる *[!UICONTROL Select Merge policy]* （マージポリシーの選択）ダイアログを開くことができます。 結合ポリシーについて詳しくは、 [Merge Policiesユーザーガイドを参照してください](merge-policies.md)。

![](../images/user-guide/profiles-search-merge-policy.png)

### ID名前空間

「 **[!UICONTROL ID名前空間]** 」セレクターを開くと、検索するID名前空間を選択できるダイアログが開きます。また、フィルターアイコンを選択し、追加または削除する属性を選択して、検索から表示する属性をカスタマイズできます。

![](../images/user-guide/profiles-search-filter.png)

[ *[!UICONTROL ID名前空間を]* 選択 **ダイアログで、検索に使用する名前空間を選択するか、ダイアログの]** 検索バーを使用して名前空間名の入力を開始します。 追加の詳細を表示する名前空間を選択できます。検索する名前空間を見つけたら、ラジオボタンを選択して **[!UICONTROL Select]** （選択）を押し、続行します。

![](../images/user-guide/profiles-select-identity-namespace.png)

### ID値

「 **[!UICONTROL ID」名前空間を選択した後]**、「 *[!UICONTROL 参照]* 」タブに戻り、 **[!UICONTROL IDの値を入力できます]**。 この値は個々の顧客プロファイルに固有の値で、提供される名前空間に対して有効なエントリである必要があります。 例えば、「 **[!UICONTROL ID」名前空間]** 「Email」を選択するには、有効な電子メールアドレスの形式で **[!UICONTROL ID値が必要です]** 。

![](../images/user-guide/profiles-show-profile.png)

値を入力したら、「 **[!UICONTROL プロファイルを表示]** 」を選択すると、値に一致する1つのプロファイルが返されます。 プロファイルの詳細を表示する **[!UICONTROL プロファイルID]** を選択します。

![](../images/user-guide/profiles-display-profile.png)

### プロファイルの詳細 {#profile-detail}

**[!UICONTROL プロファイルIDを選択すると]**、「 _[!UICONTROL 詳細]_」タブが開きます。 表示されるプロファイル情報は、複数のプロファイルフラグメントから結合され、個々の顧客の単一の表示を形成しています。

![](../images/user-guide/profiles-profile-detail.png)

プロファイルが属する *[!UICONTROL 属性]*、 *[!UICONTROL イベント]*、 ** セグメントなど、プロファイルに関連する追加情報を表示できます。

![](../images/user-guide/profiles-attributes-events-segments.png)

## 結合ポリシー

Select the *[!UICONTROL Merge Policies]* tab to view a list of merge policies belonging to your organization. 表示される各ポリシーの名前、デフォルトの結合ポリシーかどうか、および適用先のスキーマが表示されます。

For more information on merge policies, see the [Merge Policies user guide](merge-policies.md).

![](../images/user-guide/profiles-merge-policies.png)

## 和集合スキーマ

「 *和集合スキーマ* 」タブを選択して、の和集合スキーマを表示し [!DNL Profile Store]ます。 A union schema is an amalgamation of all [!DNL Experience Data Model] (XDM) fields under the same class, whose schemas have been enabled for use in [!DNL Real-time Customer Profile]. 左側のリストでクラスを選択し、キャンバス内の和集合スキーマの構造を表示します。

例えば、「[!DNL XDM Profile]」を選択すると、ク [!DNL XDM Individual Profile] ラスの和集合スキーマが表示されます。

See the section on union schemas in the [schema composition guide](../../xdm/schema/composition.md) for more information on union schemas and their role in [!DNL Real-time Customer Profile].

![](../images/user-guide/profiles-union-schema.png)

## 次の手順

By reading this guide, you now know how to view and manage your [!DNL Profile] data using the [!DNL Experience Platform] UI. For information on how to leverage [!DNL Real-time Customer Profile] data to generate audience segments, see the [Segmentation documentation](../../segmentation/home.md).