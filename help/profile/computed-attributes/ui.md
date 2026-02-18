---
title: 計算属性 UI ガイド
description: Adobe Experience Platform UI を使用して計算済み属性を作成、表示、更新する方法について説明します。
exl-id: bc621167-6dba-473e-90e4-aac7ceb6579a
source-git-commit: c2832821ea6f9f630e480c6412ca07af788efd66
workflow-type: tm+mt
source-wordcount: '1497'
ht-degree: 7%

---

# 計算属性 UI ガイド

>[!NOTE]
>
>計算済み属性にアクセスするには、適切な権限（**計算済み属性を表示** と **計算属性を管理**）が必要です。 必要な権限について詳しくは、[ アクセス制御ドキュメント ](../../access-control/home.md) を参照してください。 これらの権限の適用方法については、[ 権限の管理ガイド ](../../access-control/ui/permissions.md) を参照してください。

Adobe Experience Platformでは、計算済み属性は、イベントレベルのデータをプロファイルレベルの属性に集計するために使用される関数です。 これらの関数は自動的に計算され、セグメント化、アクティベーションおよびパーソナライゼーションで使用できます。

このドキュメントでは、Adobe Experience Platform UI を使用して計算済み属性を作成および更新する方法について説明します。

## はじめに

この UI ガイドを使用するには、[!DNL Experience Platform] の管理に関連する様々な [!DNL Real-Time Customer Profiles] サービスについて理解している必要があります。 このガイドを読む前、または UI を使用する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-Time Customer Profile]](../home.md)：複数のソースから集計したデータに基づいて、統合されたリアルタイム顧客プロファイルを提供します。
- [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md)：[!DNL Experience Platform] が、カスタマーエクスペリエンスデータを整理する際に使用する、標準化されたフレームワーク。

## 計算属性の表示 {#view}

Experience Platform UI で、左側のナビゲーションの「**[!UICONTROL Profiles]**」を選択し、**[!UICONTROL Computed attributes]** に続いて、組織で使用可能な計算済み属性のリストを表示します。 これには、計算属性の名前、説明、前回の評価日および前回の評価ステータスに関する情報が含まれます。

![ 「[!UICONTROL Profile]」セクションと「[!UICONTROL Computed attributes]」タブがハイライト表示され、計算済み属性の参照ページへのアクセス方法がユーザーに示されます。](./images/ui/browse.png)

表示するフィールドを選択するには、![ 列を設定アイコン ](/help/images/icons/column-settings.png) を選択して、表示するフィールドを追加または削除します。

| フィールド | 説明 |
| ----- | ----------- |
| [!UICONTROL Name] | 計算属性の表示名。 |
| [!UICONTROL Description] | 計算属性の説明。 |
| [!UICONTROL Evaluation method] | 計算属性の評価方法。 現時点では、**バッチ** のみがサポートされています。 |
| [!UICONTROL Last evaluated] | このタイムスタンプは、最後に成功した評価実行を表します。 このタイムスタンプの **前** に発生したイベントのみが、最後に成功した評価で考慮されます。 |
| [!UICONTROL Last evaluation status] | 最後の評価実行で計算属性が正常に計算されたかどうかを示すステータス。 使用可能な値は **[!UICONTROL Success]** または **[!UICONTROL Failed]** です。 |
| [!UICONTROL Refresh frequency] | 計算属性の更新頻度を示します。 可能な値には、時間別、日別、週別、月別があります。 |
| [!UICONTROL Fast refresh] | この計算属性に対して高速更新が有効になっているかどうかを示す値。 高速更新が有効な場合、計算属性は、毎週、隔週または毎月ではなく、毎日に更新されます。 この値は、ルックバック期間が週単位より大きい計算属性にのみ適用されます。 |
| [!UICONTROL Lifecycle status] | 計算属性の現在のステータス。 次の 3 つのステータスが考えられます。 <ul><li>**[!UICONTROL Draft]:** 計算属性には、スキーマに作成されたフィールドがまだ **ありません**。 この状態で、計算属性を編集できます。 </li><li>**[!UICONTROL Published]:** 計算属性にはスキーマで作成されたフィールドがあり、使用する準備が整っています。 この状態では、計算属性 **編集** できません。</li><li>**[!UICONTROL Inactive]:** 計算属性は無効です。 非アクティブステータスについて詳しくは、[FAQ ページ ](./faq.md#inactive-status) を参照してください。 </li> |
| [!UICONTROL Created] | 計算属性が作成された日時を示すタイムスタンプ。 |
| [!UICONTROL Last modified] | 計算属性が最後に変更された日時を示すタイムスタンプ。 |

また、表示される計算済み属性をライフサイクルステータスに基づいてフィルタリングすることもできます。 「![funnel](/help/images/icons/filter.png)」アイコンを選択します。

![ フィルターアイコンがハイライト表示されている様子 ](./images/ui/select-filter.png)

ステータス（[!UICONTROL Draft]、[!UICONTROL Published]、[!UICONTROL Inactive]）で計算済み属性をフィルタリングするように選択できるようになりました。

![ 計算済み属性のフィルタリングに使用できるオプションがハイライト表示されています。 これらのオプションには、[!UICONTROL Draft]、[!UICONTROL Published]、[!UICONTROL Inactive] などがあります。](./images/ui/view-filters.png)

さらに、計算属性を選択して、その詳細を表示できます。 計算属性の詳細ページについて詳しくは、[ 計算属性の詳細を表示 ](#view-details) を参照してください。

## 計算属性の作成 {#create}

新しい計算属性を作成するには、「**[!UICONTROL Create computed attribute]**」を選択して新しい計算属性ワークフローを入力します。

![ 「[!UICONTROL Create computed attributes]」ボタンがハイライト表示され、「計算属性の作成」ページへのアクセス方法がユーザーに示されている様子 ](./images/ui/create.png)

**[!UICONTROL Create computed attribute]** ページが表示されます。 このページでは、作成する計算属性の基本情報を追加できます。

| フィールド | 説明 |
| ----- | ----------- |
| [!UICONTROL Display name] | 計算属性が認識される名前。 この表示名は、計算属性ごとに一意に保つ必要があります。 ベストプラクティスとして、この表示名には計算属性に関連する識別子を含めてください。 例えば、「過去 7 日間の靴の購入合計」などです。 |
| [!UICONTROL Field name] | 他のダウンストリームサービスで計算属性を参照するために使用される名前。 この名前は、表示名から自動的に派生し、キャメルケースで記述されます。 |
| [!UICONTROL Description] | 作成しようとしている計算属性の説明。 |

![[!UICONTROL Basic information] ページの [!UICONTROL Create computed attribute] のセクションがハイライト表示されている様子 ](./images/ui/basic-information.png)

計算属性の詳細を追加したら、ルールの定義を開始できます。

### イベントのフィルター条件を指定

ルールを作成するには、まず「**[!UICONTROL Events]**」セクションから属性を選択して、集計するイベントをフィルタリングします。 現在、非配列タイプのイベント属性のみがサポートされています。

![ 「[!UICONTROL Events]」セクションがハイライト表示されている様子 ](./images/ui/events.png)

計算属性定義で使用する属性を選択したら、この値の比較対象を選択できます。

![ 使用可能な比較タイプが表示されます。](./images/ui/select-comparison.png)

### 集計関数の適用

これで、条件付き出力からフィールドに関数を適用できます。 最初に、集計関数タイプを選択します。 使用可能なオプションには、[!UICONTROL Sum]、[!UICONTROL Min]、[!UICONTROL Max]、[!UICONTROL Count]、[!UICONTROL Most Recent] などがあります。 これらの関数について詳しくは、計算済み属性の概要の [ 関数 ](./overview.md#functions) の節を参照してください。

![ 計算属性関数が表示されます。](./images/ui/select-function.png)

関数を選択したら、集計するフィールドを選択できます。 選択可能なフィールドは、選択した関数によって異なります。

![ ハイライト表示されたフィールドに、関数を集計するために選択している属性が表示されます。](./images/ui/select-eligible-field.png)

### ルックバック期間

集計関数を適用した後、計算属性のルックバック期間を定義する必要があります。 このルックバック期間は、イベントを集計する時間の長さを指定します。 このルックバック期間は、時間、日、週、月の単位で指定できます。

![ ルックバック期間がハイライト表示されている様子 ](./images/ui/select-lookback-duration.png)

### 高速更新 {#fast-refresh}

>[!CONTEXTUALHELP]
>id="platform_profile_computedAttributes_fastRefresh"
>title="高速更新"
>abstract="高速更新を使用すると、属性を最新の状態に保つことができます。このオプションを有効にすると、ルックバック期間が長くても、計算属性を毎日更新し、ユーザーアクティビティに迅速に対応できます。この値は、ルックバック期間が週単位より大きい計算属性にのみ適用されます。"

集計関数を適用する際に、ルックバック期間が 1 週間を超える場合は、高速更新を有効にできます。

![ 「[!UICONTROL Fast Refresh]」チェックボックスがハイライト表示されている様子 ](./images/ui/enable-fast-refresh.png)

高速更新を使用すると、属性を最新の状態に保つことができます。このオプションを有効にすると、ルックバック期間が長くても計算済み属性を毎日更新できるので、ユーザーアクティビティに迅速に対応できます。

高速更新について詳しくは、計算属性の概要の [ 高速更新の節 ](./overview.md#fast-refresh) を参照してください。

これらの手順が完了したので、この計算属性をドラフトとして保存するか、すぐに公開するかを選択できるようになりました。

![ 「[!UICONTROL Save as draft]」ボタンと「[!UICONTROL Publish]」ボタンがハイライト表示されている様子 ](./images/ui/draft-or-publish.png)

## 計算属性の詳細の表示 {#view-details}

計算属性の詳細を表示するには、[!UICONTROL **参照**] ページで詳細を表示する計算属性を選択します。

![ 計算属性がハイライト表示されている様子 ](./images/ui/select.png)

ページのコンテンツは、計算属性が **[!UICONTROL Published]** であるか、**[!UICONTROL Draft]** であるかによって異なります。

### 公開済みの計算属性 {#published}

公開済みの計算属性を選択すると、計算属性詳細ページが表示されます。

![ 計算属性の詳細ページが表示されます。](./images/ui/details.png)

このページには、計算属性の詳細の概要、値分布を示すグラフ、および計算属性に適合するサンプルプロファイルが表示されます。

>[!NOTE]
>
>値の配分は、サンプリングジョブの時点でのプロファイルの属性値の配分を反映します。 サンプルプロファイルの計算属性値は、一部のサンプルプロファイルの最新の結合プロファイル値を反映しています。

### ドラフト計算属性 {#draft}

ドラフトの計算属性を選択すると、**[!UICONTROL Edit computed attributes]** のページが表示されます。 このページでは、[!UICONTROL Create computed attributes] ページと同様に、計算属性の基本情報とその定義を編集してから、ドラフトを更新したり公開したりできます。

![[!UICONTROL Edit computed attributes] ページが表示されます。](./images/ui/edit.png)

## 計算済み属性の使用 {#usage}

>[!IMPORTANT]
>
>セグメント定義の **最新** 関数で計算属性を使用する場合は、計算属性オブジェクトに値とタイムスタンプ値の **両方** を含める **必要があります**。
>
>例えば、「有効なメールアドレスを持つすべてのプロファイル」を検索するセグメント定義を作成する場合、メールアドレスのフィールドには、最新の関数を持つ計算属性が入力され、メールアドレスの値が存在する **とメールアドレスのタイムスタンプが存在する** の両方を含める **必要があります**。

計算属性を作成したら、他のダウンストリームサービスで **公開** 計算属性を使用できます。 計算済み属性はプロファイルの結合スキーマで作成されたプロファイル属性フィールドなので、リアルタイム顧客プロファイルの計算済み属性値を検索したり、それらをオーディエンスで使用したり、宛先に対してアクティブ化したり、Adobe Journey Optimizerのジャーニーでパーソナライゼーションに使用したりできます。

>[!NOTE]
>
>計算済み属性 **使用できません** オーディエンス **コンポジション** で使用できます。

![ セグメントビルダーが表示され、セグメント定義構成の一部として計算属性が表示されます。](./images/ui/use-ca.png)

## 次の手順

計算属性について詳しくは、[ 計算属性の概要 ](./overview.md) を参照してください。 API を使用して計算済み属性を作成および設定する方法について詳しくは、[ 計算属性開発者ガイド ](./api.md) を参照してください。
