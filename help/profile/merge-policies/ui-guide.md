---
title: 結合ポリシー UI ガイド
type: Documentation
description: Adobe Experience Platform ユーザーインターフェイスを使用して結合ポリシーを操作する方法について説明します。
exl-id: 0489217a-6a53-428c-a531-fd0a0e5bb71f
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '2334'
ht-degree: 39%

---


# 結合ポリシー UI ガイド

Adobe Experience Platform では、個々の顧客の全体像を把握するために、複数のソースから得られたデータフラグメントを統合し組み合わせることができます。このデータを統合する際に、[!DNL Experience Platform] では、データの優先順位を設定する方法と統合ビューを作成するために組み合わせるデータを決定するためのルールとして、結合ポリシーを使用します。

RESTful API またはユーザーインターフェイスを介すると、新しい結合ポリシーの作成、既存のポリシーの管理、組織のデフォルトの結合ポリシーの設定をおこなえます。このガイドでは、Adobe Experience Platform ユーザーインターフェイス（UI）を使用して結合ポリシーを扱う手順を説明します。

結合ポリシーと、Experience Platform 内での結合ポリシーの役割について詳しくは、まず[結合ポリシーの概要](overview.md)をお読みください。

## はじめに

このガイドを利用するには、[!DNL Experience Platform] のいくつかの重要な機能に関する実用的な知識が必要です。このガイドに従う前に、次のサービスのドキュメントを確認してください。

* [リアルタイム顧客プロファイル](../home.md)：複数のソースから集計したデータに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
* [Adobe Experience Platform ID サービス ](../../identity-service/home.md)：異なるデータソースの ID を [!DNL Experience Platform] に取り込むことで、リアルタイム顧客プロファイルを有効にします。
* [エクスペリエンスデータモデル（XDM）](../../xdm/home.md)：[!DNL Experience Platform] が顧客エクスペリエンスデータを整理する際に使用する標準化されたフレームワーク。

## 結合ポリシーの表示 {#view-merge-policies}

>[!CONTEXTUALHELP]
>id="platform_errors_uplib_101221_404"
>title="結合ポリシーが見つかりません"
>abstract="つまり、Experience Platform でリクエストされた結合ポリシーを見つけることができませんでした。このエラーを解決するには、次のいずれかの解決策を試してください。<ul><li>URL に正しい結合ポリシー ID がリストされていることを確認する。</li><li>アクセスしようとしている結合ポリシーの組織とサンドボックスの組み合わせが正しいことを確認する。</li></ul>"

[!DNL Experience Platform] UI 内で結合ポリシーの操作を開始するには、左側のナビゲーションで「**[!UICONTROL Profiles]**」を選択し、「**[!UICONTROL Merge Policies]**」タブを選択します。

このタブには、組織の既存の結合ポリシーの一覧と各結合ポリシーの詳細（ポリシー名、デフォルトの結合ポリシーであるかどうか、結合ポリシーが関連するスキーマクラスなど）が含まれています。

![ 結合ポリシーの参照ページが表示されます。](../images/merge-policies/landing.png)

表示する詳細を選択するか、表示に列を追加するには、「列設定アイコン ![ を選択し ](../../images/icons/column-settings.png) 列名を選択して、列を表示に追加または表示から削除します。

![ 結合ポリシーの参照ページをカスタマイズするために使用できるオプションが表示されます。](../images/merge-policies/adjust-view.png)

## 結合ポリシーの作成 {#create-a-merge-policy}

新しい結合ポリシーを作成するには、「結合ポリシー」タブで「**[!UICONTROL Create merge policy]**」を選択して、新しい結合ポリシーワークフローを入力します。

![作成ボタンがハイライト表示された結合ポリシーランディングページ](../images/merge-policies/create-new.png)

**[!UICONTROL New merge policy]** ワークフローでは、一連のガイド付き手順を通じて、新しい結合ポリシーに関する重要な情報を入力する必要があります。 これらの手順の概要を以降の節で説明します。

![「新しい結合ポリシー」ワークフロー](../images/merge-policies/create.png)

## [!UICONTROL Configure] {#configure}

ワークフローの最初の手順では、基本情報を入力して結合ポリシーを設定できます。こうした情報には、次のものが含まれます。

* **[!UICONTROL Name]**：結合ポリシーの名前は、説明的かつ簡潔にする必要があります。
* **[!UICONTROL Schema class]**：結合ポリシーに関連付けられた XDM スキーマクラス。 この結合ポリシーを作成するスキーマクラスを指定します。組織は、スキーマクラスごとに複数の結合ポリシーを作成できます。現在、UI では [!UICONTROL XDM Individual Profile] クラスのみ使用できます。 **[!UICONTROL View Union Schema]** を選択すると、スキーマクラスの和集合スキーマをプレビューできます。 詳しくは、次の「[和集合スキーマの表示](#view-union-schema)」節を参照してください。
* **[!UICONTROL ID stitching]**：このフィールドでは、顧客の関連 ID を特定する方法を定義します。 ID ステッチには 2 つの値が考えられます。選択した ID ステッチのタイプがデータに与える影響を理解することが重要です。詳しくは、[結合ポリシーの概要](overview.md)を参照してください。
   * **[!UICONTROL None]**:ID のステッチを実行しません。
   * **[!UICONTROL Private Graph]**：プライベート ID グラフに基づいて ID ステッチを実行します。
* **[!UICONTROL Default merge policy]**：組織のデフォルトとしてこの結合ポリシーを使用するかどうかを選択するトグルボタン。 セレクターをオンに切り替えると、組織のデフォルトの結合ポリシーを変更することに対する確認を求める警告が表示されます。デフォルトの結合ポリシーについて詳しくは、[結合ポリシーの概要](overview.md)を参照してください。
  ![ 結合ポリシーがデフォルトの結合ポリシーとして設定された場合の動作を説明するポップオーバー。](../images/merge-policies/create-make-default.png)
* **[!UICONTROL Active-On-Edge Merge Policy]**：この結合ポリシーをエッジでアクティブにするかどうかを選択できるトグルボタン。 すべてのプロファイルコンシューマーがエッジで同じビューを使用していることを確認するために、結合ポリシーをエッジでアクティブとしてマークできます。オーディエンスをエッジでアクティブ化（エッジオーディエンスとしてマーク）するには、エッジでアクティブとマークされた結合ポリシーに結び付ける必要があります。 オーディエンスがエッジでアクティブとしてマークされている結合ポリシーに結び付けられて **ない** 場合、そのオーディエンスはエッジでアクティブとマークされず、ストリーミングオーディエンスとしてマークされます。 さらに、組織内の各サンドボックスは、エッジでアクティブな **1** 結合ポリシーのみを持つことができます。

必須フィールドに入力したら、「**[!UICONTROL Next]**」を選択してワークフローを続行できます。

![ 「次へ」ボタンがハイライト表示された設定画面の完了。](../images/merge-policies/create-complete.png)

## [!UICONTROL View Union Schema] {#view-union-schema}

結合ポリシーを作成または編集する際に、**[!UICONTROL View Union Schema]** を選択すると、選択したスキーマクラスの結合スキーマを表示できます。

![ 新しい結合ポリシーワークフローで「和集合スキーマを表示」ボタンがハイライト表示されます。](../images/merge-policies/view-union-schema.png)

これにより、結合 [!UICONTROL View Union Schema] イアログが開き、結合スキーマに関連付けられている、要因となるすべてのスキーマ、ID および関係が表示されます。 このダイアログを使用して、和集合スキーマを調べることができます。方法は、Experience Platform UI の「[!UICONTROL Union Schema]」セクションの「[!UICONTROL Profiles]」タブにアクセスする場合と同じです。

和集合スキーマの詳細（結合ポリシーワークフローに表示される「[!UICONTROL Union Schema]」タブや [!UICONTROL View Union Schema] ダイアログでの操作方法など）については、[ 和集合スキーマ UI ガイド ](../ui/union-schema.md) を参照してください。

![ 和集合スキーマを表示ダイアログ ](../images/merge-policies/view-union-schema-dialog.png)

## [!UICONTROL Select Profile datasets] {#select-profile-datasets}

**[!UICONTROL Select Profile datasets]** 画面で、結合ポリシーに使用する **[!UICONTROL Merge method]** を選択する必要があります。 また、この画面には、前の画面で選択したスキーマクラスに関連する、組織内の [!UICONTROL Profile datasets] ーザーの合計数が表示されます。

選択した結合方法に応じて、すべてのプロファイルデータセットが最後に更新された順序（順序付きタイムスタンプ）で結合されるか、結合ポリシーに含めるプロファイルデータセットと結合する順序（データセットの優先順位）を選択する必要があります。

結合メソッドの詳細情報は、[結合ポリシーの概要](overview.md)を参照してください。

>[!BEGINTABS]

>[!TAB  順序付きタイムスタンプ ]

結合メソッドとして **[!UICONTROL Timestamp ordered]** を選択すると、最近更新されたデータセットの属性が優先されます。 これは、すべてのプロファイルデータセットに適用されます。

>[!NOTE]
>
>**[!UICONTROL Profile datasets]** の横にある角括弧で囲まれた数（例えば、図の `(37)`）は、含まれるプロファイルデータセットの合計数を示します。

![ 順序付きタイムスタンプ結合メソッドが選択されていることを示す画像 ](../images/merge-policies/timestamp-ordered.png)

>[!TAB  データセットの優先順位 ]

結合メソッドとして **[!UICONTROL Dataset precedence]** を選択するには、プロファイルデータセットを選択し、手動で優先順位を付ける必要があります。 リストされる各データセットには、最後に取得されたバッチのステータスも含まれます。また、バッチがそのデータセットに取得されていないことを示す通知も表示されます。

データセットリストから最大 50 個のデータセットを選択して、結合ポリシーに含めることができます。

>[!NOTE]
>
>**[!UICONTROL Profile datasets]** の横にある角括弧で囲まれた数（例えば、図の `(37)`）は、選択可能なプロファイルデータセットの合計数を示します。

データセットを選択すると、データセットが「**[!UICONTROL Select datasets]**」セクションに追加され、データセットをドラッグ&amp;ドロップして、必要な優先順位に従って並べ替えることができます。 リストでデータセットが調整されると、データセットの横に表示される序数（1、2、3 など）が更新され、優先度が表示されます（1 が最も高い優先度 、次に 2 以降）。

データセットを選択すると、「**[!UICONTROL Union schema]**」セクションも更新され、各データセットがデータを提供する結合スキーマ内のフィールドが表示されます。 UI のビジュアライゼーションの操作方法など、和集合スキーマについて詳しくは、[和集合スキーマ UI ガイド](../ui/union-schema.md)を参照してください。

![ 選択されているデータセットの優先順位を示す画像と、そのオプションが選択されている場合に選択する必要がある設定 ](../images/merge-policies/dataset-precedence.png)

>[!ENDTABS]

## [!UICONTROL Select ExperienceEvent datasets] {#select-experienceevent-datasets}

ワークフローの次の手順では、ExperienceEvent データセットを選択する必要があります。この画面は、[[!UICONTROL Select Profile datasets]](#select-profile-datasets) の画面で選択した結合メソッドの影響を受けます。

>[!BEGINTABS]

>[!TAB  順序付きタイムスタンプ ]

プロファイルデータセットの結合メソッドとして **[!UICONTROL Timestamp ordered]** を選択した場合は、最近更新された ExperienceEvent データセットの属性もここで優先されます。

>[!NOTE]
>
>**[!UICONTROL ExperienceEvent datasets]** の横にある角括弧で囲まれた数（例えば、図の `(1)`）は、結合ポリシー設定画面で選択したスキーマクラスに関連する、組織が作成した ExperienceEvent データセットの合計数を示します。

![ スキーマクラスに関連付けることができる ExperienceEvent データセットの合計数が表示されます。](../images/merge-policies/timestamp-experienceevent.png)

>[!TAB  データセットの優先順位 ]

プロファイルデータセットの結合メソッドとして **[!UICONTROL Dataset precedence]** を選択した場合は、含める ExperienceEvent データセットを選択する必要があります。 データセットリストから最大 50 個の ExperienceEvent データセットを選択できます。

>[!NOTE]
>
>**[!UICONTROL ExperienceEvent datasets]** の横にある角括弧で囲まれた数（例えば、図の `(1)`）は、結合ポリシー設定画面で選択したスキーマクラスに関連する、組織が作成した ExperienceEvent データセットの合計数を示します。

データセットを選択すると、[!UICONTROL Select datasets] のセクションに表示されます。

ExperienceEvent データセットは手動で並べ替えることはできません。同じプロファイルフラグメントに属する場合、ExperienceEvent データセットの属性がプロファイルデータセットに追加されます。

プロファイルデータセットの選択と同様に、ExperienceEvent データセットを選択すると、「**[!UICONTROL Union schema]**」セクションも更新され、各データセットがデータを提供する結合スキーマのフィールドが表示されます。 UI のビジュアライゼーションの操作方法など、和集合スキーマについて詳しくは、[ 和集合スキーマ UI ガイド ](../ui/union-schema.md) を参照してください。

![ 選択可能な ExperienceEvent データセットが表示されます。](../images/merge-policies/dataset-precedence-experienceevent.png)

>[!ENDTABS]

## [!UICONTROL Review] {#review}

ワークフローの最後の手順は、結合ポリシーを確認することです。**[!UICONTROL Review]** 画面には、選択した ID ステッチ方法、選択した結合方法、含まれたデータセットなど、結合ポリシーに関する情報が表示されます。 （含まれるすべてのプロファイルまたは ExperienceEvent データセットを表示するには、データセットの数を選択してドロップダウンリストを展開します）。

また、レビュー画面には、結合ポリシーを使用したサンプルプロファイルレコードを示す **[!UICONTROL Preview data]** の表も含まれています。 これにより、結合ポリシーを保存する前に、顧客プロファイルがどのように表示されるかをプレビューできます。

**[!UICONTROL Finish]** を選択して作成ワークフローを完了する前に、結合ポリシーの設定とプレビューデータを慎重に確認してください。

>[!BEGINTABS]

>[!TAB  順序付きタイムスタンプ ]

結合ポリシーの結合メソッドとして **[!UICONTROL Timestamp ordered]** を選択した場合、プロファイルデータセットのリストには、スキーマクラスに関連する組織が作成したすべてのデータセットがタイムスタンプの順に含まれます。 ExperienceEvent データセットのリストには、選択したスキーマクラスに対して組織が作成したすべてのデータセットが含まれ、プロファイルデータセットに追加されます。

**[!UICONTROL Preview data]** の表に、データセットのタイムスタンプ順序に基づくサンプルプロファイルレコードを示します。 これにより、結合ポリシーを保存する前に、顧客プロファイルがどのように表示されるかをプレビューできます。

「**[!UICONTROL Finish]**」を選択して、新しい結合ポリシーを作成します。

![ レビューページが表示されます。 このページでは、新しく作成した結合ポリシーの詳細を確認できます。](../images/merge-policies/timestamp-review.png)

>[!TAB  データセットの優先順位 ]

結合ポリシーの結合方法として **[!UICONTROL Dataset precedence]** を選択した場合、プロファイルデータセットと ExperienceEvent データセットのリストには、作成ワークフローで選択したプロファイルデータセットと ExperienceEvent データセットのみが含まれます。 プロファイルデータセットの順序は、作成時に指定した優先順位と一致する必要があります。一致しない場合は、「[!UICONTROL Back]」ボタンを使用して前のワークフローステップに戻り、優先度を調整します。

**[!UICONTROL Preview data]** の表に、選択したデータセットを使用したサンプルプロファイルレコードを示します。 これにより、結合ポリシーを保存する前に、顧客プロファイルがどのように表示されるかをプレビューできます。

「**[!UICONTROL Finish]**」を選択して、新しい結合ポリシーを作成します。

![ レビューページが表示されます。 このページでは、新しく作成した結合ポリシーの詳細を確認できます。](../images/merge-policies/dataset-precedence-review.png)

>[!ENDTABS]

## 結合ポリシーの編集 {#edit}

「[!UICONTROL Merge Policies]」タブで、[!DNL XDM Individual Profile] クラス用に作成した既存の結合ポリシーを変更できます。変更するには、編集する結合ポリシーの **[!UICONTROL Policy name]** を選択します。

**[!UICONTROL Edit merge policy]** の画面が表示されたら、名前と [!UICONTROL ID stitching] メソッドを変更し、このポリシーが組織のデフォルトの結合ポリシーであるかどうかを変更できます。

「**[!UICONTROL Next]**」を選択して、結合ポリシーのワークフローを続行し、結合ポリシーに含まれる結合メソッドとデータセットを更新します。

![ 結合ポリシーの編集ワークフローが表示されます。](../images/merge-policies/edit-screen.png)

必要な変更を加えたら、結合ポリシーを確認し、「**[!UICONTROL Finish]**」を選択して変更を保存し、「[!UICONTROL Merge policies]」タブに戻ります。

>[!WARNING]
>
>結合ポリシーを変更すると、データの競合を解決する方法が変わるので、セグメント化とプロファイルの結果に影響を与える可能性があります。結合ポリシーの変更は、保存する前に慎重に確認してください。

![ ポリシーの編集ワークフローのレビューページが表示されます。](../images/merge-policies/edit-review.png)

## データガバナンスポリシー違反

結合ポリシーを作成または更新すると、結合ポリシーが組織で定義されたデータ使用ポリシーに違反しているかどうかを確認するチェックが実行されます。データ使用ポリシーは、Adobe Experience Platform データガバナンスの一部であり、特定の [!DNL Experience Platform] データに対して実行が許可される、または制限されるマーケティングアクションの種類を記述したルールです。

例えば、組織のデータ使用ポリシーによって特定のデータをサードパーティに書き出すことを防止している場合、結合ポリシーを使用してサードパーティの宛先に対してアクティブ化されたオーディエンスを作成し、結合ポリシーを保存しようとすると、**[!UICONTROL Data governance policy violation detected]** の通知が表示されます。

この通知には、違反したデータ使用ポリシーのリストが含まれ、リストから選択すると、違反の詳細を表示できます。違反ポリシーを選択すると、「**[!UICONTROL Data lineage]**」タブに違反理由と影響を受けるアクティベーションが表示され、データ使用ポリシーの違反の詳細を確認できます。

Adobe Experience Platform 内でのデータガバナンスの実行方法について詳しくは、まず[データガバナンスの概要](../../data-governance/home.md)を読んでください。

## 次の手順

これで、組織の結合ポリシーを作成して設定できました。これらのポリシーを使用して、Experience Platform内の顧客プロファイルの表示を調整し、プロファイルデータからオーディエンスを作成できます。 [ UI と API を使用してオーディエンスを作成および操作する方法について詳しくは、](../../segmentation/home.md) セグメントの概要 [!DNL Experience Platform] を参照してください。
