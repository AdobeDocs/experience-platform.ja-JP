---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
title: 結合ポリシーユーザーガイド
topic: guide
translation-type: tm+mt
source-git-commit: 47c65ef5bdd083c2e57254189bb4a1f1d9c23ccc
workflow-type: tm+mt
source-wordcount: '1516'
ht-degree: 29%

---


# 結合ポリシーユーザーガイド

Adobe Experience Platformでは、複数のソースからデータフラグメントをまとめ、それらを組み合わせて、個々の顧客の完全な表示を確認できます。 When bringing this data together, merge policies are the rules that [!DNL Platform] uses to determine how data will be prioritized and what data will be combined to create that unified view.

例えば、顧客が複数のチャネルをまたがって自社のブランドとやり取りを行う場合、1人の顧客に関連する複数のプロファイルフラグメントが複数のデータセットに表示されます。 これらのフラグメントがPlatformに取り込まれると、それらのフラグメントが結合され、そのお客様用の単一のプロファイルが作成されます。 複数のソースからのデータが競合する場合(例えば、1つのフラグメントリストが顧客を「独身」、他のリストが「既婚」)、結合ポリシーによって個人のプロファイルに含める情報が決定されます。

RESTful API またはユーザーインターフェイスを介すると、新しい結合ポリシーの作成、既存のポリシーの管理、組織のデフォルトの結合ポリシーの設定をおこなえます。このガイドでは、Adobe Experience Platform ユーザーインターフェイスを使用して結合ポリシーを操作する手順を説明します。

If you would prefer to work with merge policies using the [!DNL Real-time Customer Profile] API, please follow the instructions outlined in the [merge policies API tutorial](../api/merge-policies.md).

## はじめに

This guide requires a working understanding of the various [!DNL Experience Platform] services involved with merge policies. このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

* [[!DNL Real-time Customer Profile]](../home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [[!DNL Identity Service]](../../identity-service/home.md):取り込ま [!DNL Real-time Customer Profile] れる異なるデータソースのIDをブリッジ化することで有効に [!DNL Platform]します。
* [[!DNL Experience Data Model (XDM)]](../../xdm/home.md):顧客体験データを [!DNL Platform] 整理する際に使用される標準化されたフレームワーク。

## 結合ポリシーの表示

Within the [!DNL Experience Platform] user interface, you can begin to work with merge policies and see a list of your organization&#39;s existing merge policies by selecting **[!UICONTROL Profiles]** in the left-rail and then selecting the **[!UICONTROL Merge policies]** tab.

![ポリシーの結合ランディングページ](../images/merge-policies/landing.png)

組織で使用できる各結合ポリシーの詳細は、ポリシー名、既定の結合ポリシー、スキーマなど、ランディングページに表示されます。

表示する詳細を選択する場合、または表示する列を追加する場合は、列セレクターアイコンを選択し、列名をクリックして表示に追加または削除します。

![](../images/merge-policies/adjust-view.png)

## 結合ポリシーの作成

新しい結合ポリシーを作成するには、「結合ポリシーを **[!UICONTROL 作成]**」を選択します。

![ポリシーの結合ランディングページ](../images/merge-policies/create-new.png)

「**[!UICONTROL 結合ポリシーを作成]**」画面が表示され、新しい結合ポリシーに関する重要な情報を指定できます。

![](../images/merge-policies/create.png)

* **[!UICONTROL 名前]**：結合ポリシーの名前は、説明的かつ簡潔にする必要があります。
* **[!UICONTROL スキーマ]**：結合ポリシーと関連付けられたスキーマです。この結合ポリシーを作成する XDM ポリシーを指定します。組織は、スキーマごとに複数の結合ポリシーを作成できます。
* **[!UICONTROL ID ステッチ]**：このフィールドでは、顧客の関連 ID を特定する方法を定義します。次の 2 つの値を使用できます。
   * **[!UICONTROL なし]**：ID ステッチを実行しない。
   * **[!UICONTROL 非公開グラフ]**：非公開の ID グラフに基づいて ID ステッチを実行します。
* **[!UICONTROL 属性の結合]**:プロファイルフラグメントには、個々の顧客のIDのリストのうち、1つのIDに関する情報のみが含まれます。 使用するIDグラフのタイプが複数のIDになる場合、プロファイル属性と優先度が競合する可能性があります。 Using &quot;[!UICONTROL Attribute merge]&quot; allows you to specify which dataset profile values to prioritize if a merge conflict occurs between key-value (record data) type datasets. 次の 2 つの値を使用できます。
   * **[!UICONTROL 順番に並べられたタイムスタンプ]**:競合がイベントすると、最近更新されたプロファイルが優先されます。 [!UICONTROL また、同じデータセット(複数のID] )内のデータを結合する場合やデータセットをまたがる場合に、システムのタイムスタンプよりも優先されるカスタムタイムスタンプもサポートされます。 詳しくは、次の [timestamp ordered](#timestamp-ordered) （タイムスタンプの順序）の節を参照してください。
   * **[!UICONTROL データセットの優先順位]** :競合をイベントする際、プロファイルフラグメントの到着元のデータセットに基づいて、そのフラグメントを優先します。 このオプションを選択する場合は、関連するデータセットと優先順位を選択する必要があります。 詳しくは、以下に示す[データセットの優先順位](#dataset-precedence)を参照してください。
* **[!UICONTROL 既定の結合ポリシー]**：組織のデフォルトとしてこの結合ポリシーを使用するかどうかを選択するトグルボタン。セレクターをオンに切り替えて新しいポリシーを保存すると、以前のデフォルトのポリシーが自動的に更新され、デフォルトのポリシーが無効になります。

### 順序付きタイムスタンプ {#timestamp-ordered}

プロファイルレコードをExperience Platformに取り込むと、取り込み時にシステムタイムスタンプを取得し、記録に追加する。 マージポリシーの **[!UICONTROL 属性マージ]** ・タイプとして「順序付き **[!UICONTROL タイムスタンプ]** 」が選択されている場合、プロファイルはシステムのタイムスタンプに基づいてマージされます。 つまり、記録がプラットフォームに取り込まれた時のタイムスタンプに基づいて結合が行われます。

場合によっては、カスタムタイムスタンプを指定し、マージポリシーでシステムタイムスタンプではなくカスタムタイムスタンプが適用される必要がある場合があります。 例えば、データのバックフィルや、レコードが不適切に取り込まれた場合のイベントの正しい順序の確保などがあります。

>[!NOTE]
>
>この機能は、データセット間での取り込みにのみ使用できます。 同じデータセットを使用してレコードが取り込まれると、デフォルトの置き換え動作が発生します。

### カスタムタイムスタンプの使用 {#custom-timestamps}

カスタムタイムスタンプを使用するには、「[!UICONTROL External Source System Audit Details Mixin]」をプロファイルスキーマに追加する必要があります。 追加したカスタムタイムスタンプは、この `lastUpdatedDate` フィールドを使用して入力できます。

フィールドに値が入力された状態でレコードを取り込むと、Experience Platformはその `lastUpdatedDate` フィールドを使用してデータセット間のレコードを結合します。 が存在しな `lastUpdatedDate` い場合、または入力されない場合、プラットフォームはシステムタイムスタンプを引き続き使用します。

>[!NOTE]
>
>同じレコードに更新を取り込む場合は、 `lastUpdatedDate` タイムスタンプを必ず入力してください。

次のスクリーンショットは、「[!UICONTROL External Source System Audit Details Mixin]」のフィールドを示しています。 UIを使用してスキーマを操作する手順(スキーマにミックスインを追加する方法など)については、UIを使用したスキーマ作成の [チュートリアルを参照してください](../../xdm/tutorials/create-schema-ui.md)。

![](../images/merge-policies/custom-timestamp-mixin.png)

APIを使用してカスタムタイムスタンプを操作するには、 [マージポリシーエンドポイントガイドの付録](../api/merge-policies.md) 、およびカスタムタイムスタンプ [の使用に関する節を参照してください](../api/merge-policies.md#custom-timestamps)。

### データセットの優先順位 {#dataset-precedence}

**[!UICONTROL 属性の結合]**&#x200B;値を選択する際に、「**[!UICONTROL データセットの優先順位]**」を選択すると、プロファイルフラグメントをその値の元のデータセットに基づいて優先させることができます。

例えば、あるデータセットに情報が存在し、他のデータセットのデータよりも優先度や信頼度が高い場合などに使用できます。

「 **[!UICONTROL データセットの優先順位]**」を選択すると、別のパネルが開き、「 **[!UICONTROL 使用可能なデータセット]** 」からデータセットを選択する必要があります（または、チェックボックスを使用してすべてを選択します）。 その後、これらのデータセットを「**[!UICONTROL 選択したデータセット]**」パネルにドラッグ＆ドロップし、正しい優先順位にドラッグできます。最上位のデータセットには最も高い優先順位が付けられ、2番目のデータセットには2番目に高い優先順位が付けられます。

![](../images/merge-policies/dataset-precedence.png)

Once you have finished creating the merge policy, select **[!UICONTROL Save]** to return to the **[!UICONTROL Merge policies]** tab where your new merge policy now appears in the list of policies.

## 結合ポリシーの編集

You can modify an existing merge policy through the [!UICONTROL Merge policies] tab by selecting the **[!UICONTROL Policy name]** for the merge policy you wish to edit.

![ポリシーの結合ランディングページ](../images/merge-policies/select-edit.png)

When the **[!UICONTROL Edit merge policy]** screen appears, you can make changes to the name, schema, ID stitching type, and attribute merge type, as well as select whether or not this policy will be the default merge policy for your organization.

>[!NOTE]
>
>結合ポリシー ID は編集できません。この ID は編集画面の上部に表示されます。これはシステムによって生成された読み取り専用の ID で、変更できません。

![](../images/merge-policies/edit-screen.png)

Once you have made the necessary changes, select **[!UICONTROL Save]** to return to the **[!UICONTROL Merge policies]** tab where the updated merge policy information is now visible.

![](../images/merge-policies/edited.png)

## データガバナンスポリシー違反

結合ポリシーを作成または更新すると、結合ポリシーが組織で定義されたデータ使用ポリシーに違反しているかどうかを確認するチェックが実行されます。Data usage policies are part of Adobe Experience Platform [!DNL Data Governance] and are rules that describe the kinds of marketing actions that you are allowed to, or restricted from, performing on specific [!DNL Platform] data. For example, if a merge policy was used to create a segment that activated to a third-party destination, and your organization had a data usage policy preventing the export of specific data to third parties, you would receive a &quot;[!UICONTROL Data governance policy violation detected]&quot; notification when attempting to save your merge policy.

この通知には、違反したデータ使用ポリシーのリストが含まれ、リストから選択すると、違反の詳細を表示できます。Upon selecting a violated policy, the **[!UICONTROL Data lineage]** tab provides the reason for the violation and the affected activations, each providing more detail into how the data usage policy has been violated.

Adobe Experience Platform 内でのデータガバナンスの実行方法について詳しくは、まず[データガバナンスの概要](../../data-governance/home.md)を読んでください。

![](../images/merge-policies/policy-violation.png)

## 次の手順

これで、IMS　組織の結合ポリシーを作成して設定しました。これらのポリシーを使用して、プロファイルデータからオーディエンスセグメントを作成できます。See the [Segmentation overview](../../segmentation/home.md) for more information on how to create and work with segments using [!DNL Experience Platform].