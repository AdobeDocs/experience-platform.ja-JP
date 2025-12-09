---
keywords: Experience Platform；ホーム；人気のトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；スキーマ；スキーマ；
solution: Experience Platform
title: UI でのスキーマの作成と編集
description: Experience Platform ユーザーインターフェイスでスキーマを作成および編集する方法の基本について説明します。
exl-id: be83ce96-65b5-4a4a-8834-16f7ef9ec7d1
source-git-commit: 491588dab1388755176b5e00f9d8ae3e49b7f856
workflow-type: tm+mt
source-wordcount: '4635'
ht-degree: 3%

---

# UI でのスキーマの作成と編集 {#create-edit-schemas-in-ui}

このガイドでは、Adobe Experience Platform UI で組織の Experience Data Model （XDM）スキーマを作成、編集、管理する方法の概要について説明します。

>[!IMPORTANT]
>
>XDM スキーマは非常にカスタマイズ可能なので、スキーマの作成に必要な手順は、スキーマで取得するデータの種類によって異なる場合があります。 そのため、このドキュメントでは、UI でスキーマを使用して実行できる基本的なインタラクションのみを扱い、クラス、スキーマフィールドグループ、データタイプ、フィールドのカスタマイズなど、関連する手順は除外します。
>
>スキーマ作成プロセスの完全なツアーについては、[ スキーマ作成チュートリアル ](../../tutorials/create-schema-ui.md) に従って、完全なサンプルスキーマを作成し、[!DNL Schema Editor] の多くの機能を理解してください。

## 前提条件 {#prerequisites}

このガイドでは、XDM システムに関する十分な知識が必要です。 Experience Platform エコシステムにおける XDM の役割の概要については [XDM の概要 ](../../home.md) を、スキーマの構築方法の概要については [ スキーマ構成の基本 ](../../schema/composition.md) を参照してください。

## 新しいスキーマの作成 {#create}

[!UICONTROL Schemas] ワークスペースで、右上隅の「**[!UICONTROL Create schema]**」を選択します。 「スキーマタイプを選択」ドロップダウンメニューが開き、[!UICONTROL Standard] または [!UICONTROL Relational] スキーマのオプションが表示されます。

![[!UICONTROL Create Schema] がハイライト表示され、「スキーマタイプを選択」ドロップダウンが表示されたスキーマ ワークスペース ](../../images/ui/resources/schemas/create-schema.png)。

## リレーショナルスキーマを作成 {#create-relational-schema}

>[!AVAILABILITY]
>
>Data Mirrorおよびリレーショナルスキーマは、Adobe Journey Optimizer **オーケストレートキャンペーン** のライセンスホルダーが利用できます。 ライセンスとイネーブルメント機能に応じて、Customer Journey Analytics ユーザー向けの **限定リリース** としても使用できます。 アクセスについては、Adobe担当者にお問い合わせください。

レコードをきめ細かく制御できる、構造化されたリレーショナルスタイルのスキーマを定義する場合は、「**[!UICONTROL Relational]**」を選択します。 リレーショナルスキーマは、プライマリキーの適用、レコードレベルのバージョン管理、プライマリキーと外部キーを介したスキーマレベルの関係をサポートしています。 また、CHANGE DATA CAPTURE を使用した増分取り込みに最適化されており、Campaign オーケストレーション、Data Distiller、B2B の実装で使用される複数のデータモデルもサポートされています。

詳しくは、[Data Mirror](../../data-mirror/overview.md) または [ リレーショナルスキーマ ](../../schema/relational.md) の概要を参照してください。

### 手動で作成 {#create-manually}

>[!AVAILABILITY]
>
>DDL ファイルのアップロードは、Adobe Journey Optimizer Orchestrated Campaign のライセンス所有者のみが使用できます。 UI の表示が異なる場合があります。

**[!UICONTROL Create a relational schema]** ダイアログが表示されます。 **[!UICONTROL Create manually]** または [**[!UICONTROL Upload DDL file]**](#upload-ddl-file) を選択して、スキーマ構造を定義できます。

**[!UICONTROL Create a relational schema]** ダイアログで、「**[!UICONTROL Create manually]**」を選択し、「**[!UICONTROL Next]**」を選択します。

![ 「手動で作成」が選択され、「次へ」がハイライト表示されたリレーショナルスキーマを作成ダイアログ ](../../images/ui/resources/schemas/relational-dialog.png)

**[!UICONTROL Relational schema details]** ページが表示されます。 スキーマの表示名と説明（オプション）を入力し、「**[!UICONTROL Finish]**」を選択してスキーマを作成します。

![[!UICONTROL Schema display name]、[!UICONTROL Description]、[!UICONTROL Finish] がハイライト表示されたリレーショナルスキーマの詳細ビュー ](../../images/ui/resources/schemas/relational-details.png)

スキーマエディターが開き、スキーマ構造を定義するための空のキャンバスが表示されます。 通常どおりフィールドを追加できます。

#### バージョン識別子フィールドを追加 {#add-version-identifier}

バージョントラッキングを有効にし、チェンジデータキャプチャをサポートするには、スキーマでバージョン識別子フィールドを指定する必要があります。 スキーマエディターで、プラス（![A プラスアイコンを選択します。](/help/images/icons/plus.png)）アイコンをクリックして、新しいフィールドを追加します。

`updateSequence` などのフィールド名を入力し、**[!UICONTROL DateTime]** または **[!UICONTROL Number]** のデータタイプを選択します。

右側のパネルで「**[!UICONTROL Version Identifier]**」チェックボックスを有効にし、「**[!UICONTROL Apply]**」を選択してフィールドを確定します。

![`updateSequence` という名前の日時フィールドが追加され、「バージョン識別子」チェックボックスが選択されているスキーマエディター ](../../images/ui/resources/schemas/add-version-identifier.png)

>[!IMPORTANT]
>
>リレーショナルスキーマには、レコードレベルのアップデートとチェンジ データキャプチャの取り込みをサポートするバージョン識別子フィールドを含める必要があります。

関係を定義するには、スキーマエディターで **[!UICONTROL Add Relationship]** を選択して、スキーマレベルのプライマリ/外部キーの関係を作成します。 詳しくは、[ スキーマレベルの関係の追加 ](../../tutorials/relationship-ui.md#relationship-field) に関するチュートリアルを参照してください。

次に、必要に応じて [ プライマリキーを定義 ](../fields/identity.md#define-a-identity-field) に進み、[ フィールドを追加 ](#add-field-groups) します。 Experience Platform ソースでチェンジ・データ・キャプチャを有効にする方法のガイダンスについては、[ チェンジ・データ・キャプチャ・インジェスト・ガイド ](../../../sources/tutorials/api/change-data-capture.md) を参照してください。

>[!NOTE]
>
>保存すると、[!UICONTROL Type] のサイドバーの [!UICONTROL  Schema properties] フィールドは、これが [!UICONTROL Relational] スキーマであることを示します。 これは、スキーマインベントリ表示の詳細サイドバーにも表示されます。
>![リレーショナルタイプがハイライト表示された空のリレーショナルスキーマ構造を示すスキーマエディターキャンバス。](../../images/ui/resources/schemas/relational-empty-canvas.png)

### DDL ファイルのアップロード {#upload-ddl-file}

>[!AVAILABILITY]
>
>DDL ファイルのアップロードは、Adobe Journey Optimizer Orchestrated Campaign のライセンス所有者のみが使用できます。

このワークフローを使用して、DDL ファイルをアップロードすることでスキーマを定義します。 **[!UICONTROL Create a relational schema]** ダイアログで「**[!UICONTROL Upload DDL file]**」を選択し、システムからローカルの DDL ファイルをドラッグするか、「**[!UICONTROL Choose files]**」を選択します。 Experience Platformはスキーマを検証し、ファイルのアップロードが成功した場合は緑のチェックマークを表示します。 「**[!UICONTROL Next]**」を選択して、アップロードを確定します。

![ 選択して [!UICONTROL Upload DDL file] がハイライト表示され [!UICONTROL Next] リレーショナルスキーマを作成ダイアログ ](../../images/ui/resources/schemas/upload-ddl-file.png)

[!UICONTROL Select entities and fields to import] ダイアログが表示され、スキーマをプレビューできます。 スキーマ構造を確認し、ラジオボタンとチェックボックスを使用して、各エンティティにプライマリキーとバージョン識別子が指定されていることを確認します。

>[!IMPORTANT]
>
>テーブル構造には、**プライマリキー** と **バージョン識別子** （日時または数値タイプの `updateSequence` フィールドなど）を含める必要があります。
>
>CHANGE DATA CAPTURE の取り込みの場合、増分処理を有効にするには、String 型の `_change_request_type` という名前の特別な列も必要です。 このフィールドは、データ変更のタイプを示します（例：`u` （アップサート）または `d` （削除））。

取り込み時には必要ですが、`_change_request_type` などのコントロール列はスキーマには格納されず、最終的なスキーマ構造には表示されません。 すべてが正しいと思われる場合は、「**[!UICONTROL Done]**」を選択してスキーマを作成します。

>[!NOTE]
>
>DDL アップロードでサポートされるファイルの最大サイズは 10 MB です。

![ インポートフィールドが表示されハイライト表示されたリレーショナルスキーマレビュービュ [!UICONTROL Finish]。](../../images/ui/resources/schemas/entities-and-files-to-inport.png)


スキーマがスキーマエディターで開き、構造を調整してから保存できます。

次に、[ フィールドを追加 ](#add-field-groups) に進み、必要に応じて [ スキーマレベルの関係を追加 ](../../tutorials/relationship-ui.md#relationship-field) に進みます。

Experience Platform ソースでチェンジ・データ・キャプチャを有効にする方法のガイダンスについては、[ チェンジ・データ・キャプチャ・インジェスト・ガイド ](../../../sources/tutorials/api/change-data-capture.md) を参照してください。

## 標準スキーマの作成 {#standard-based-creation}

「スキーマタイプを選択」ドロップダウンメニューから「標準スキーマタイプ」を選択すると、[!UICONTROL Create a schema] のダイアログが表示されます。 このダイアログでは、フィールドとフィールドグループを追加して手動でスキーマを作成するか、CSV ファイルをアップロードして ML アルゴリズムを使用してスキーマを生成するかを選択できます。 ダイアログからスキーマ作成ワークフローを選択します。

![ ワークフローオプションと「選択」がハイライト表示されたスキーマを作成ダイアログ ](../../images/ui/resources/schemas/create-a-schema-dialog.png)

### [!BADGE Beta]{type=Informative} 手動または ML で支援されたスキーマの作成 {#manual-or-assisted}

ML アルゴリズムを使用して、CSV ファイルに基づいてスキーマ構造をレコメンデーションする方法については、[ 機械学習を利用したスキーマ作成ガイド ](../ml-assisted-schema-creation.md) を参照してください。 この UI ガイドは、手動作成ワークフローを中心としています。

### 手動でのスキーマ作成 {#manual-creation}

[!UICONTROL Create schema] ワークフローが表示されます。 スキーマの基本クラスを選択するには、「**[!UICONTROL Individual Profile]**」、「**[!UICONTROL Experience Event]**」または「**[!UICONTROL Other]**」を選択し、続いて「**[!UICONTROL Next]**」を選択して選択内容を確認します。 これらのクラスについて詳しくは、[[!UICONTROL XDM individual profile]](../../classes/individual-profile.md) および [[!UICONTROL XDM ExperienceEvent]](../../classes/experienceevent.md) のドキュメントを参照してください。

![3 つのクラスオプションと [!UICONTROL Create schema] がハイライト表示された [!UICONTROL Next] ワークフロー ](../../images/ui/resources/schemas/schema-class-options.png)

**[!UICONTROL Other]** を選択すると、使用可能なクラスのリストが表示されます。 ここから、既存のクラスを参照およびフィルタリングできます。

![[!UICONTROL Create schema] のセクションで [!UICONTROL Other] がハイライト表示されている [!UICONTROL Schema details] ワークフロー ](../../images/ui/resources/schemas/other-schema-details.png)

カスタムクラスと標準クラスのどちらかに基づいてクラスをフィルタリングするラジオボタンを選択します。 また、業界に基づいて使用可能な結果をフィルタリングしたり、検索フィールドを使用して特定のクラスを検索したりすることもできます。

![ 検索バー、[!UICONTROL Create schema]、[!UICONTROL Custom] がハイライト表示された [!UICONTROL Industries] ワークフロー ](../../images/ui/resources/schemas/filter-and-search.png)

適切なクラスを決定できるように、各クラスには情報アイコンとプレビューアイコンがあります。 情報アイコン（![ 情報アイコン。](/help/images/icons/info.png)）をクリックすると、関連付けられているクラスと業界の説明を提供するダイアログが開きます。

![ 選択したクラスの情報アイコンとツールヒントがハイライト表示されている様子 ](../../images/ui/resources/schemas/class-info.png)

プレビューアイコン（![ プレビューアイコン。](/help/images/icons/preview.png)）を選択すると、スキーマ図とそのプロパティを含むクラスのプレビューダイアログが開きます。

![ スキーマ図とクラスのプロパティを使用した、選択したクラスのプレビュー。](../../images/ui/resources/schemas/class-preview.png)

任意の行を選択してクラスを選択し、「**[!UICONTROL Next]**」を選択して選択内容を確定します。

![ 使用可能なクラスのテーブルから選択されたクラスがハイライト表示され [!UICONTROL Create schema] いる [!UICONTROL Next] ワークフロー ](../../images/ui/resources/schemas/select-class.png)

クラスを選択すると、「[!UICONTROL Name and review]」セクションが表示されます。 このセクションでは、スキーマを識別するための名前と説明を指定します。&#x200B;キャンバスにスキーマの基本構造（クラスによって提供される）が表示され、選択したクラスとスキーマ構造を確認できます。

テキストフィールドに、人間にとってわかりやすい [!UICONTROL Schema display name] を入力します。 次に、スキーマの識別に役立つ適切な説明を入力します。 スキーマ構造をレビューし、設定に満足したら、「**[!UICONTROL Finish]**」を選択してスキーマを作成します。

![[!UICONTROL Name and review]、[!UICONTROL Create schema]、[!UICONTROL Schema display name] がハイライト表示された [!UICONTROL Description] ワークフローの [!UICONTROL Finish] のセクション。](../../images/ui/resources/schemas/name-and-review.png)

スキーマエディターが表示され、キャンバスにスキーマの構造が表示されます。 必要に応じて、[ クラスへのフィールドの追加 ](../../ui/resources/classes.md#add-fields) を開始できます。

![ キャンバスに表示されたスキーマの構造を持つスキーマエディター。](../../images/ui/resources/schemas/edit.png)

## 既存のスキーマの編集 {#edit}

>[!NOTE]
>
>スキーマを保存してデータ取り込みに使用すると、スキーマに追加の変更を加えることのみ可能です。 詳しくは、[ スキーマ進化のルール ](../../schema/composition.md#evolution) を参照してください。

既存のスキーマを編集するには、「**[!UICONTROL Browse]**」タブを選択してから、編集するスキーマの名前を選択します。 検索バーを使用して、使用可能なオプションのリストを絞り込むこともできます。

![ スキーマがハイライト表示されたスキーマワークスペース。](../../images/ui/resources/schemas/edit-schema.png)

>[!TIP]
>
>ワークスペースの検索機能とフィルター機能を使用すると、スキーマを見つけやすくなります。 詳しくは、[XDM リソースの調査 ](../explore.md) に関するガイドを参照してください。

スキーマを選択すると、キャンバスにスキーマの構造が表示された [!DNL Schema Editor] が表示されます。 スキーマが使用している場合は、スキーマへの [ フィールドグループの追加 ](#add-field-groups) または [ 個々のフィールドの追加 ](#add-individual-fields) これらのグループから）、[ フィールド表示名の編集 ](#display-names) または [ 既存のカスタムフィールドグループの編集 ](./field-groups.md#edit) を実行できるようになりました。

## その他のアクション {#more}

また、スキーマエディター内でクイックアクションを実行して、スキーマの JSON 構造をコピーしたり、リアルタイム顧客プロファイルが有効になっていない場合や関連付けられたデータセットがある場合はスキーマを削除したりできます。 ビューの上部にある「[!UICONTROL More]」を選択すると、クイックアクションを含むドロップダウンが表示されます。

JSON 構造をコピー機能を使用すると、スキーマとデータパイプラインを作成している間、サンプルペイロードがどのように見えるかを確認できます。 これは、ID マップなど、スキーマ内に複雑なオブジェクトマップ構造がある場合に特に便利です。

![ 「その他」ボタンがハイライト表示され、ドロップダウンオプションが表示されたスキーマエディター。](../../images/tutorials/create-schema/more-actions.png)

## 表示名の切替 {#display-name-toggle}

スキーマエディターでは、便宜上、元のフィールド名と、より人間が読み取り可能な表示名との間で変更する切替スイッチを提供しています。 この柔軟性により、フィールドの検出性が向上し、スキーマを編集できます。切替スイッチは、スキーマエディタービューの右上にあります。

>[!NOTE]
>
>フィールド名から表示名への変更は単なる表面的なもので、ダウンストリームリソースは変更されません。

![[!UICONTROL Show display names for fields] がハイライト表示されたスキーマエディター ](../../images/ui/resources/schemas/display-name-toggle.png)

標準フィールドグループの表示名はシステムで生成されますが、[ 表示名 ](#display-names) の節で説明されているようにカスタマイズできます。 表示名は、マッピングやデータセットのプレビューを含む、複数の UI 表示に反映されます。 デフォルト設定はオフで、フィールド名は元の値で表示されます。

## スキーマへのフィールドグループの追加 {#add-field-groups}

>[!NOTE]
>
>この節では、既存のフィールドグループをスキーマに追加する方法を説明します。 新しいカスタムフィールドグループを作成する場合は、代わりに [ フィールドグループの作成と編集 ](./field-groups.md#create) に関するガイドを参照してください。

[!DNL Schema Editor] 内でスキーマを開くと、フィールドグループを使用してスキーマにフィールドを追加できます。 開始するには、左側のパネルで **[!UICONTROL Add]** の横にある「**[!UICONTROL Field groups]**」を選択します。

![[!UICONTROL Add] のセクションの [!UICONTROL Field groups] がハイライト表示されたスキーマエディター。](../../images/ui/resources/schemas/add-field-group-button.png)

ダイアログが表示され、スキーマに対して選択できるフィールドグループのリストが表示されます。 フィールドグループは 1 つのクラスにのみ適合するので、スキーマの選択されたクラスに関連付けられているフィールドグループのみが表示されます。 デフォルトでは、リストに表示されるフィールドグループは、組織内での使用率に基づいて並べ替えられます。

![[!UICONTROL Add field groups] の列がハイライト表示された [!UICONTROL Popularity] ダイアログ ](../../images/ui/resources/schemas/field-group-popularity.png)

追加するフィールドの一般的なアクティビティまたはビジネス領域がわかっている場合は、左側のパネルで 1 つ以上の業種カテゴリを選択して、表示されるフィールドグループのリストをフィルタリングします。

![[!UICONTROL Add field groups] のフィルターと [!UICONTROL Industry] の列がハイライト表示された [!UICONTROL Industry] ダイアログ ](../../images/ui/resources/schemas/industry-filter.png)

>[!NOTE]
>
>XDM での業界固有のデータモデリングに関するベストプラクティスについて詳しくは、[ 業界データモデル ](../../schema/industries/overview.md) のドキュメントを参照してください。

また、検索バーを使用して、目的のフィールドグループを見つけることもできます。 クエリと名前が一致するフィールドグループがリストの上部に表示されます。 **[!UICONTROL Standard Fields]** の下に、目的のデータ属性を説明するフィールドを含むフィールドグループが表示されます。

![[!UICONTROL Add field groups] 検索機能がハイライト表示された [!UICONTROL Standard fields] ダイアログ ](../../images/ui/resources/schemas/field-group-search.png)

スキーマに追加するフィールドグループ名の横にあるチェックボックスをオンにします。 リストから複数のフィールドグループを選択でき、選択した各フィールドグループが右側のパネルに表示されます。

![ チェックボックス選択機能がハイライト表示された [!UICONTROL Add field groups] ダイアログ ](../../images/ui/resources/schemas/add-field-group.png)

>[!TIP]
>
>リストに表示されたフィールドグループの場合、情報アイコン（![ 情報アイコン ](/help/images/icons/info.png)）にポインタを合わせるかフォーカスを合わせると、フィールドグループが取得するデータの種類の簡単な説明が表示されます。 また、プレビューアイコン（![ プレビューアイコン ](/help/images/icons/preview.png)）を選択して、フィールドグループが提供するフィールドの構造を表示してから、スキーマに追加することもできます。

フィールドグループを選択したら、「**[!UICONTROL Add field groups]**」を選択して、スキーマに追加します。

![ フィールドグループが選択されハイライト表示された [!UICONTROL Add field groups] ダイアログ [!UICONTROL Add field groups] 表示されます。](../../images/ui/resources/schemas/add-field-group-finish.png)

キャンバスに表された、フィールドグループが提供するフィールドと共に [!DNL Schema Editor] が再び表示されます。

![ スキーマの例が表示された [!DNL Schema Editor]。](../../images/ui/resources/schemas/field-groups-added.png)

>[!NOTE]
>
>スキーマエディター内では、標準（Adobeが生成した）クラスおよびフィールドグループは、南京錠アイコン ![ 南京錠アイコン](/help/images/icons/lock-closed.png)。南京錠は、クラスまたはフィールドグループ名の横の左側のパネルに表示されるほか、システム生成リソースの一部であるスキーマ図のフィールドの横にも表示されます。
>
>![ 南京錠アイコンがハイライトされたスキーマエディター ](../../images/ui/explore/schema-editor-padlock-icon.png)

フィールドグループをスキーマに追加した後、必要に応じて、オプションで [ 既存のフィールドを削除 ](#remove-fields) または [ 新しいカスタムフィールドを追加 ](#add-fields) をこれらのグループに追加できます。

### フィールドグループから追加されたフィールドを削除 {#remove-fields}

フィールドグループをスキーマに追加したら、フィールドグループからグローバルにフィールドを削除したり、現在のスキーマからローカルにフィールドを非表示にしたりできます。 意図しないスキーマの変更を回避するには、これらのアクションの違いを理解することが重要です。

>[!IMPORTANT]
>
>「**[!UICONTROL Remove]**」を選択すると、フィールドグループ自体からフィールドが削除され、そのフィールドグループを使用する *すべて* スキーマに影響を与えます。
>**フィールドグループを含むすべてのスキーマからフィールドを削除する** 場合を除き、このオプションを使用しないでください。

フィールドグループからフィールドを削除するには、キャンバスでフィールドを選択し、右側のパネルで「**[!UICONTROL Remove]**」を選択します。 この例では、`taxId` グループの **[!UICONTROL Demographic Details]** フィールドを示しています。

![[!DNL Schema Editor] がハイライト表示された [!UICONTROL Remove]。 この操作により、単一のフィールドが削除されます。](../../images/ui/resources/schemas/remove-single-field.png)

フィールドグループ自体から削除せずにスキーマから複数のフィールドを非表示にするには、**[!UICONTROL Manage related fields]** オプションを使用します。 キャンバスでグループから任意のフィールドを選択し、右側のパネルで「**[!UICONTROL Manage related fields]**」を選択します。

![[!DNL Schema Editor] がハイライト表示された [!UICONTROL Manage related fields]。](../../images/ui/resources/schemas/manage-related-fields.png)

フィールドグループの構造を示すダイアログが表示されます。 チェックボックスを使用して、含めるフィールドを選択または選択解除します。

![ 選択したフィールドと [!UICONTROL Manage related fields] がハイライト表示された [!UICONTROL Confirm] ダイアログ ](../../images/ui/resources/schemas/select-fields.png)

「**[!UICONTROL Confirm]**」を選択してキャンバスを更新し、選択したフィールドを反映します。


![ 追加されたフィールド ](../../images/ui/resources/schemas/fields-added.png)

### フィールドを削除または非推奨（廃止予定）にする際のフィールドの動作 {#field-removal-deprecation-behavior}

次の表を使用して、各アクションの範囲を理解します。

| アクション | 現在のスキーマにのみ適用されます | フィールドグループを変更します | 他のスキーマに影響 | 説明 |
|--------------------------|--------------------------------|----------------------|-----------------------|-------------|
| **フィールドを削除** | × | ○ | ○ | フィールドグループからフィールドを削除します。 これにより、そのグループを使用するすべてのスキーマから削除されます。 |
| **関連フィールドの管理** | ○ | × | × | 現在のスキーマのみからフィールドを非表示にします。 フィールドグループは変更されません。 |
| **フィールドを廃止** | × | ○ | ○ | フィールドグループ内のフィールドを非推奨（廃止予定）としてマークします。 どのスキーマでも使用できなくなりました。 |

>[!NOTE]
>
>この動作は、レコードベースとイベントベースの両方のスキーマで一貫しています。

### フィールドグループへのカスタムフィールドの追加 {#add-fields}

フィールドグループをスキーマに追加した後は、そのグループの追加フィールドを定義できます。 ただし、1 つのスキーマのフィールドグループに追加されたフィールドは、同じフィールドグループを使用する他のすべてのスキーマにも表示されます。

また、標準フィールドグループにカスタムフィールドが追加されると、そのフィールドグループはカスタムフィールドグループに変換され、元の標準フィールドグループは使用できなくなります。

標準フィールドグループにカスタムフィールドを追加する場合は、[ 以下の節 ](#custom-fields-for-standard-groups) を参照して、具体的な手順を確認してください。 カスタムフィールドグループにフィールドを追加する場合は、フィールドグループ UI ガイドの [ カスタムフィールドグループの編集 ](./field-groups.md) の節を参照してください。

既存のフィールドグループを変更しない場合は、[ 新しいカスタムフィールドグループを作成 ](./field-groups.md#create) して、代わりに追加のフィールドを定義できます。

## スキーマへの個々のフィールドの追加 {#add-individual-fields}

特定の使用例でフィールドグループ全体を追加したくない場合は、スキーマエディターを使用して、個々のフィールドをスキーマに直接追加できます。 代わりに [ 標準フィールドグループから個々のフィールドを追加する ](#add-standard-fields) または [ 独自のカスタムフィールドを追加する ](#add-custom-fields) ことができます。

>[!IMPORTANT]
>
>スキーマエディターの機能を使用すると、個々のフィールドをスキーマに直接追加できますが、XDM スキーマのすべてのフィールドがそのクラスまたはクラスと互換性のあるフィールドグループによって提供される必要があるという事実は変更されません。 以下の節で説明するように、個々のフィールドはすべて、スキーマに追加される際の重要な手順として、引き続きクラスまたはフィールドグループに関連付けられます。

### 標準フィールドを追加 {#add-standard-fields}

標準フィールドグループからスキーマに直接フィールドを追加でき、対応するフィールドグループを事前に知る必要はありません。 標準フィールドをスキーマに追加するには、キャンバスでスキーマ名の横にあるプラス（**+**）アイコンを選択します。 スキーマ構造に **[!UICONTROL Untitled Field]** プレースホルダーが表示されます。また、右側のパネルが更新されて、フィールドを設定するためのコントロールが表示されます。

![ フィールドプレースホルダー ](../../images/ui/resources/schemas/root-custom-field.png)

「**[!UICONTROL Field name]**」の下で、追加するフィールドの名前の入力を開始します。 クエリに一致する標準フィールドが自動的に検索され、属するフィールドグループを含め、**[!UICONTROL Recommended Standard Fields]** の下にリストされます。

![ 推奨される標準フィールド ](../../images/ui/resources/schemas/standard-field-search.png)

一部の標準フィールドは同じ名前を共有しますが、構造は元のフィールドグループによって異なる場合があります。 標準フィールドがフィールドグループ構造内の親オブジェクト内にネストされている場合、子フィールドが追加されると、親フィールドもスキーマに含まれます。

標準フィールドの横にあるプレビューアイコン（![ プレビューアイコン ](/help/images/icons/preview.png)）を選択すると、そのフィールドグループの構造が表示され、ネストの仕組みをより深く理解できます。 標準フィールドをスキーマに追加するには、プラスアイコン（![ プラスアイコン ](/help/images/icons/add-circle.png)）を選択します。

![ 標準フィールドを追加 ](../../images/ui/resources/schemas/add-standard-field.png)

キャンバスが更新され、スキーマに追加された標準フィールドが表示されます。これには、フィールドグループ構造内にネストされた親フィールドも含まれます。 フィールドグループの名前は、左側のパネルの「**[!UICONTROL Field groups]**」にも表示されます。 同じフィールドグループからさらにフィールドを追加する場合は、右側のパネルで「**[!UICONTROL Manage related fields]**」を選択します。

![ 追加された標準フィールド ](../../images/ui/resources/schemas/standard-field-added.png)

### カスタムフィールドを追加 {#add-custom-fields}

標準フィールドのワークフローと同様に、独自のカスタムフィールドをスキーマに直接追加することもできます。

スキーマのルートレベルにフィールドを追加するには、キャンバスでスキーマ名の横にあるプラス（**+**）アイコンを選択します。 スキーマ構造に **[!UICONTROL Untitled Field]** プレースホルダーが表示されます。また、右側のパネルが更新されて、フィールドを設定するためのコントロールが表示されます。

![ ルートカスタムフィールド ](../../images/ui/resources/schemas/root-custom-field.png)

追加するフィールドの名前の入力を開始すると、一致する標準フィールドの検索が自動的に開始されます。 代わりに新しいカスタムフィールドを作成するには、**（[!UICONTROL New Field]）** が付加された上部のオプションを選択します。

![ 新しいフィールド ](../../images/ui/resources/schemas/custom-field-search.png)

フィールドに表示名とデータタイプを指定したら、次にフィールドを親 XDM リソースに割り当てます。 スキーマでカスタムクラスを使用する場合は、代わりに [ フィールドを割り当てられたクラスに追加 ](#add-to-class) または [ フィールドグループ ](#add-to-field-group) を選択できます。 ただし、スキーマで標準クラスを使用している場合は、カスタムフィールドをフィールドグループに割り当てるだけです。

#### カスタムフィールドグループへのフィールドの割り当て {#add-to-field-group}

>[!NOTE]
>
>この節では、フィールドをカスタムフィールドグループに割り当てる方法についてのみ説明します。 標準フィールドグループを新しいカスタムフィールドで拡張する場合は、[ 標準フィールドグループへのカスタムフィールドの追加 ](#custom-fields-for-standard-groups) の節を参照してください。

「**[!UICONTROL Assign to]**」で、「**[!UICONTROL Field Group]**」を選択します。 スキーマで標準クラスを使用する場合、これが唯一の利用可能なオプションであり、デフォルトで選択されています。

次に、関連付ける新規フィールドのフィールドグループを選択する必要があります。 提供されたテキスト入力でフィールドグループの名前を入力し始めます。 入力と一致する既存のカスタムフィールドグループがある場合は、ドロップダウンリストに表示されます。 または、一意の名前を入力して、新しいフィールドグループを作成することもできます。

![ フィールドグループを選択 ](../../images/ui/resources/schemas/select-field-group.png)

>[!WARNING]
>
>既存のカスタムフィールドグループを選択すると、そのフィールドグループを使用する他のスキーマも、変更を保存した後で、新しく追加されたフィールドを継承します。 このため、このタイプの伝播が必要な場合は、既存のフィールドグループのみを選択します。 そうでない場合は、代わりに新しいカスタムフィールドグループを作成することを選択する必要があります。

リストからフィールドグループを選択したら、「**[!UICONTROL Apply]**」を選択します。

![Apply フィールド ](../../images/ui/resources/schemas/apply-field.png)

新しいフィールドはキャンバスに追加され、標準の XDM フィールドとの競合を避けるために、[ テナント ID](../../api/getting-started.md#know-your-tenant_id) の下に名前空間が設定されます。 新しいフィールドを関連付けたフィールドグループは、左側のパネルの「**[!UICONTROL Field groups]**」にも表示されます。

![ テナント ID](../../images/ui/resources/schemas/tenantId.png)

>[!NOTE]
>
>選択したカスタムフィールドグループによって提供される残りのフィールドは、デフォルトでスキーマから削除されます。 これらのフィールドの一部をスキーマに追加する場合は、グループに属するフィールドを選択し、右側のパネルで「**[!UICONTROL Manage related fields]**」を選択します。

#### フィールドをカスタムクラスに割り当てる {#add-to-class}

「**[!UICONTROL Assign to]**」で、「**[!UICONTROL Class]**」を選択します。 の下の入力フィールドは、現在のスキーマのカスタムクラスの名前に置き換えられ、新しいフィールドがこのクラスに割り当てられることを示します。

![ 新しいフィールド割り当てに対して [!UICONTROL Class] オプションが選択されていること。](../../images/ui/resources/schemas/assign-field-to-class.png)

必要に応じてフィールドの設定を続行し、完了したら「**[!UICONTROL Apply]**」を選択します。

新しいフィールドに ![[!UICONTROL Apply] を選択しています。](../../images/ui/resources/schemas/assign-field-to-class-apply.png)

新しいフィールドはキャンバスに追加され、標準の XDM フィールドとの競合を避けるために、[ テナント ID](../../api/getting-started.md#know-your-tenant_id) の下に名前空間が設定されます。 左側のパネルでクラス名を選択すると、クラスの構造の一部として新しいフィールドが表示されます。

![ キャンバスに表示される、カスタムクラスの構造に適用された新しいフィールド ](../../images/ui/resources/schemas/assign-field-to-class-applied.png)

### 標準フィールドグループの構造へのカスタムフィールドの追加 {#custom-fields-for-standard-groups}

作業中のスキーマに、標準フィールドグループが提供するオブジェクトタイプのフィールドがある場合、独自のカスタムフィールドを標準オブジェクトに追加できます。

>[!WARNING]
>
>1 つのスキーマのフィールドグループに追加されたフィールドは、同じフィールドグループを使用する他のすべてのスキーマにも表示されます。 また、標準フィールドグループにカスタムフィールドが追加されると、そのフィールドグループはカスタムフィールドグループに変換され、元の標準フィールドグループは使用できなくなります。
>
>この機能のベータ版に参加すると、以前にカスタマイズした標準フィールドグループを通知するダイアログが表示されます。 **[!UICONTROL Acknowledge]** を選択すると、リストされたリソースがカスタムフィールドグループに変換されます。
>
>![ 標準フィールドグループを変換するための確認ダイアログ ](../../images/ui/resources/schemas/beta-extension-confirmation.png)

開始するには、標準フィールドグループで提供されるオブジェクトのルートの横にあるプラス（**+**）アイコンを選択します。

![ 標準オブジェクトへのフィールドの追加 ](../../images/ui/resources/schemas/add-field-to-standard-object.png)

標準フィールドグループを変換するかどうかを確認する警告メッセージが表示されます。 「**[!UICONTROL Continue creating field group]**」を選択して次に進みます。

![ フィールドグループの変換を確認 ](../../images/ui/resources/schemas/confirm-field-group-conversion.png)

キャンバスが再び表示され、新しいフィールドには名称未設定のプレースホルダーが表示されます。 標準フィールドグループの名前に「（[!UICONTROL Extended]）」が追加され、元のバージョンから変更されたことを示しています。 ここから、右側のパネルのコントロールを使用してフィールドのプロパティを定義します。

![ 標準オブジェクトに追加されたフィールド ](../../images/ui/resources/schemas/standard-field-group-converted.png)

変更を適用すると、標準オブジェクト内のテナント ID 名前空間の下に新しいフィールドが表示されます。 このネストされた名前空間は、同じフィールドグループを使用する他のスキーマでの変更が壊れるのを防ぐために、フィールドグループ自体の中でのフィールド名の競合を防ぎます。

![ 標準オブジェクトに追加されたフィールド ](../../images/ui/resources/schemas/added-to-standard-object.png)

## リアルタイム顧客プロファイルのスキーマを有効にする {#profile}

>[!CONTEXTUALHELP]
>id="platform_schemas_enableforprofile"
>title="プロファイルのスキーマを有効にする"
>abstract="スキーマがプロファイルで有効になっている場合、このスキーマから作成されたデータセットは、異なるソースからのデータを結合して構築した各顧客の全体像である、リアルタイムの顧客プロファイルの構築に関与します。スキーマを使用してプロファイルにデータを取り込むと、そのスキーマを無効にすることはできなくなります。詳しくは、ドキュメントを参照してください。"

[ リアルタイム顧客プロファイル ](../../../profile/home.md) は、異なるソースのデータを結合して、個々の顧客の完全なビューを構築します。 スキーマで取得されたデータをこのプロセスに参加させる場合は、スキーマを [!DNL Profile] で使用できるようにする必要があります。

>[!IMPORTANT]
>
>スキーマの [!DNL Profile] を有効にするには、プライマリ ID フィールドが定義されている必要があります。 詳しくは、[ID フィールドの定義 ](../fields/identity.md) に関するガイドを参照してください。

スキーマを有効にするには、まず、左側のパネルでスキーマの名前を選択し、次に、右側のパネルで「**[!UICONTROL Profile]**」切り替えスイッチを選択します。

![](../../images/ui/resources/schemas/profile-toggle.png)

ポップオーバーが表示され、スキーマを有効にして保存すると、無効にできないことを警告します。 「**[!UICONTROL Enable]**」を選択して続行します。

![](../../images/ui/resources/schemas/profile-confirm.png)

[!UICONTROL Profile] の切り替えを有効にした状態でキャンバスが再び表示されます。

>[!IMPORTANT]
>
>スキーマはまだ保存されていないので、スキーマをリアルタイム顧客プロファイルに参加させることに関する考えを変えた場合、これは返されないポイントです。有効なスキーマを保存すると、無効にできなくなります。 **[!UICONTROL Profile]** 切り替えスイッチをもう一度選択して、スキーマを無効にします。

プロセスを終了するには、「**[!UICONTROL Save]**」を選択してスキーマを保存します。

![](../../images/ui/resources/schemas/profile-enabled.png)

これで、スキーマをリアルタイム顧客プロファイルで使用できるようになります。 Experience Platformが、このスキーマに基づくデータセットにデータを取り込むと、そのデータは、統合されたプロファイルデータに組み込まれます。

## スキーマフィールドの表示名の編集 {#display-names}

クラスを割り当て、スキーマにフィールドグループを追加すると、フィールドが標準またはカスタムの XDM リソースによって提供されているかどうかに関係なく、スキーマのフィールドの表示名を編集できます。

>[!NOTE]
>
>標準クラスまたはフィールドグループに属するフィールドの表示名は、特定のスキーマのコンテキストでのみ編集できます。 つまり、あるスキーマで標準フィールドの表示名を変更しても、同じ関連クラスまたはフィールドグループを使用する他のスキーマには影響しません。
>
>スキーマのフィールドの表示名を変更すると、その変更は、そのスキーマに基づく既存のデータセットにすぐに反映されます。

**[!UICONTROL Show display names for fields]** をオンにして、フィールド名を表示名に変更します。 スキーマフィールドの表示名を編集するには、キャンバスでフィールドを選択します。 右側のパネルの「**[!UICONTROL Display name]**」の下に新しい名前を入力します。

![](../../images/ui/resources/schemas/display-name.png)

右側のパネルで「**[!UICONTROL Apply]**」を選択すると、キャンバスが更新されて、フィールドの新しい表示名が表示されます。 「**[!UICONTROL Save]**」を選択して、スキーマに変更を適用します。

![](../../images/ui/resources/schemas/display-name-changed.png)

## スキーマのクラスの変更 {#change-class}

スキーマが保存される前の初期作成プロセス中の任意の時点で、スキーマのクラスを変更できます。

>[!WARNING]
>
>スキーマのクラスの再割り当ては、細心の注意を払って行う必要があります。 フィールドグループは特定のクラスにのみ適合するので、クラスを変更すると、キャンバスと追加したフィールドがリセットされます。

クラスを再割り当てするには、キャンバスの左側にある **[!UICONTROL Assign]** を選択します。

![](../../images/ui/resources/schemas/assign-class-button.png)

使用可能なすべてのクラスのリストを表示するダイアログが表示されます。このリストには、組織で定義されているクラス（「[!UICONTROL Customer]」を持つ）と、Adobeで定義されている標準クラスが含まれます。

リストからクラスを選択して、ダイアログの右側に説明を表示します。 **[!UICONTROL Preview class structure]** を選択して、クラスに関連付けられたフィールドとメタデータを表示することもできます。 「**[!UICONTROL Assign class]**」を選択して続行します。

![](../../images/ui/resources/schemas/assign-class.png)

新しいダイアログが開き、新しいクラスを割り当てることを確認するように求められます。 「**[!UICONTROL Assign]**」を選択して確定します。

![](../../images/ui/resources/schemas/assign-confirm.png)

クラスの変更を確認すると、キャンバスがリセットされ、すべてのコンポジションの進行状況が失われます。

## 次の手順 {#next-steps}

このドキュメントでは、Experience Platform UI でスキーマを作成および編集する際の基本について説明しました。 カスタムフィールドグループや一意のユースケースのデータタイプの作成など、UI で完全なスキーマを構築するための包括的なワークフローについては、[ スキーマ作成チュートリアル ](../../tutorials/create-schema-ui.md) を確認することを強くお勧めします。

[!UICONTROL Schemas] workspace の機能について詳しくは、[[!UICONTROL Schemas] workspace の概要を参照してください ](../overview.md)

[!DNL Schema Registry] API でスキーマを管理する方法については、[ スキーマエンドポイントガイド ](../../api/schemas.md) を参照してください。
