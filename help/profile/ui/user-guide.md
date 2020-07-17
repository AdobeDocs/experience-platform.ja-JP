---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
solution: Adobe Experience Platform
title: リアルタイム顧客プロファイルユーザーガイド
topic: guide
translation-type: tm+mt
source-git-commit: f910351d49de9c4a18a444b99b7f102f4ce3ed5b
workflow-type: tm+mt
source-wordcount: '1147'
ht-degree: 0%

---


# [!DNL Real-time Customer Profile] ユーザーガイド

[!DNL Real-time Customer Profile] オンライン、オフライン、CRM、サードパーティなどの複数のチャネルのデータを組み合わせて、各顧客の全体的な表示を作成します。

このドキュメントは、Adobe Experience Platformユーザーインターフェイスでの対話操作のため [!DNL Real-time Customer Profile] のガイドとして機能します。

## はじめに

このユーザーガイドでは、管理に関連する様々な [!DNL Experience Platform] サービスについて理解している必要があり [!DNL Real-time Customer Profiles]ます。 このユーザーガイドを読む前に、次のサービスのドキュメントを確認してください。

* [!DNL Real-time Customer Profile](../home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [!DNL Identity Service](../../identity-service/home.md): 異なるデータソース [!DNL Real-time Customer Profile] に取り込まれる際に、アイデンティティを別々のデータソースからブリッジすることで有効に [!DNL Platform]します。
* [!DNL Experience Data Model (XDM)](../../xdm/home.md): 顧客体験データを [!DNL Platform] 整理するための標準化されたフレームワーク。

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

プロファイル数には、組織のデフォルトの結合ポリシーがプロファイルフラグメントを結合して個々の顧客 [!DNL Experience Platform]に対して1つのプロファイルを形成した後に、組織が持つプロファイルの合計数が表示されます。 つまり、様々なチャネルでブランドとやり取りする1人の顧客に関連する複数のプロファイルフラグメントを組織内に持つことができますが、これらのフラグメントは（デフォルトの結合ポリシーに従って）結合され、「1」プロファイルの数が返されます。

また、プロファイル数には、属性（レコードデータ）を持つプロファイルと、時系列(イベント)データのみを含むプロファイル(アドビのAnalyticsプロファイルなど)が含まれます。 プロファイル数は定期的に更新され、Platform内のプロファイルの総数が最新の状態になります。

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

### プロファイルの詳細

**[!UICONTROL プロファイルIDを選択すると]**、「 _[!UICONTROL 詳細]_」タブが開きます。 このページには、基本属性、リンクされたID、使用可能な連絡先チャネルなど、選択したプロファイルに関する情報が表示されます。 表示されるプロファイル情報は、複数のプロファイルフラグメントから結合され、個々の顧客の単一の表示を形成しています。

![](../images/user-guide/profiles-profile-detail.png)

プロファイルが属する *[!UICONTROL 属性]*、 *[!UICONTROL イベント]*、 ** セグメントなど、プロファイルに関連する追加情報を表示できます。

![](../images/user-guide/profiles-attributes-events-segments.png)

## 結合ポリシー

「 *[!UICONTROL マージポリシー]* 」タブを選択して、組織に属するマージポリシーのリストを表示します。 表示される各ポリシーは、名前、既定の結合ポリシーかどうか、および適用先のスキーマを表示します。

For more information on merge policies, see the [Merge Policies user guide](merge-policies.md).

![](../images/user-guide/profiles-merge-policies.png)

## 和集合スキーマ

「 *和集合スキーマ* 」タブを選択して、の和集合スキーマを表示し [!DNL Profile Store]ます。 和集合スキーマとは、同じクラスに属するすべての [!DNL Experience Data Model] (XDM)フィールドを結合したもので、そのスキーマはで使用可能になって [!DNL Real-time Customer Profile]います。 左側のリストでクラスを選択し、キャンバス内の和集合スキーマの構造を表示します。

例えば、「[!DNL XDM Profile]」を選択すると、ク [!DNL XDM Individual Profile] ラスの和集合スキーマが表示されます。

和集合のスキーマとその役割について詳しくは、『 [スキーマ構成ガイド](../../xdm/schema/composition.md) 』の「和集合のスキーマ」の節を参照してくだ [!DNL Real-time Customer Profile]さい。

![](../images/user-guide/profiles-union-schema.png)

## 次の手順

このガイドを読むと、UIを使用した [!DNL Profile][!DNL Experience Platform] データの表示と管理の方法がわかります。 データを活用してオーディエンスセグメントを生成する方法について詳しくは、セグメント化ドキュメントを参照してください [!DNL Real-time Customer Profile][](../../segmentation/home.md)。