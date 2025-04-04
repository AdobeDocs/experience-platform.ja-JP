---
keywords: Experience Platform；ホーム；IAB;IAB 2.0；同意；同意
solution: Experience Platform
title: IAB TCF 2.0 同意データを取得するためのデータセットの作成
description: このドキュメントでは、IAB TCF 2.0 の同意データを収集するために必要な 2 つのデータセットを設定する手順を説明します。
role: Developer
feature: Consent, Schemas, Datasets
exl-id: 36b2924d-7893-4c55-bc33-2c0234f1120e
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1680'
ht-degree: 3%

---

# IAB TCF 2.0 同意データを取得するためのデータセットの作成

Adobe Experience Platformが IAB [!DNL Transparency & Consent Framework] （TCF） 2.0 に従って顧客の同意データを処理するには、そのデータが、TCF 2.0 の同意フィールドをスキーマに含むデータセットに送信される必要があります。

特に、TCF 2.0 同意データを取得するには、次の 2 つのデータセットが必要です。

* [!DNL Real-Time Customer Profile] での使用が有効になっている、[!DNL XDM Individual Profile] クラスに基づくデータセット。
* [!DNL XDM ExperienceEvent] クラスに基づくデータセット。

>[!IMPORTANT]
>
>Experience Platformでは、個々のプロファイルデータセットで収集された TCF 文字列のみが適用されます。 データストリームをこのワークフローの一部として作成するには ExperienceEvent データセットが引き続き必要ですが、データをプロファイルデータセットに取り込むだけでかまいません。 同意変更イベントを経時的に追跡する場合は、ExperienceEvent データセットを引き続き使用できますが、これらの値は、セグメントのアクティベーション時に適用される場合には使用されません。

このドキュメントでは、これら 2 つのデータセットを設定する手順を説明します。 TCF 2.0 用のExperience Platform データ操作を設定する完全なワークフローの概要については、[IAB TCF 2.0 コンプライアンスの概要 ](./overview.md) を参照してください。

## 前提条件

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [エクスペリエンスデータモデル（XDM）](../../../../xdm/home.md)：[!DNL Experience Platform] が顧客エクスペリエンスデータを整理する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../xdm/schema/composition.md)：XDM スキーマの基本的な構成要素について説明します。
* [Adobe Experience Platform ID サービス ](../../../../identity-service/home.md)：異なるデータソースの顧客 ID をデバイスやシステムをまたいで結び付けることができます。
   * [ID 名前空間 ](../../../../identity-service/features/namespaces.md)：顧客 ID データは、ID サービスで認識される特定の ID 名前空間の下で提供される必要があります。
* [ リアルタイム顧客プロファイル ](../../../../profile/home.md):[!DNL Identity Service] を活用して、データセットから詳細な顧客プロファイルをリアルタイムで作成できます。 [!DNL Real-Time Customer Profile] はデータレイクからデータを取り込み、顧客プロファイルを独立したデータストアに保持します。

## TCF 2.0 フィールドグループ {#field-groups}

[!UICONTROL IAB TCF 2.0 同意詳細 ] スキーマフィールドグループは、TCF 2.0 のサポートに必要な顧客同意フィールドを提供します。 このフィールドグループには 2 つのバージョンがあります。1 つは [!DNL XDM Individual Profile] クラスと互換性があり、もう 1 つは [!DNL XDM ExperienceEvent] クラスと互換性があります。

以下の節では、取り込み時に期待されるデータを含め、これらの各フィールドグループの構造について説明します。

### プロファイルフィールドグループ {#profile-field-group}

[!DNL XDM Individual Profile] に基づくスキーマの場合、[!UICONTROL IAB TCF 2.0 同意詳細 ] フィールドグループは、顧客 ID を TCF 同意環境設定にマッピングする、単一のマップタイプフィールド `identityPrivacyInfo` を提供します。 自動適用を実行するには、このフィールドグループをリアルタイム顧客プロファイルに対して有効なレコードベースのスキーマに含める必要があります。

構造とユースケースについて詳しくは、このフィールドグループの [ リファレンスガイド ](../../../../xdm/field-groups/profile/iab.md) を参照してください。

### イベントフィールドグループ {#event-field-group}

同意変更イベントを経時的に追跡する場合は、[!UICONTROL IAB TCF 2.0 同意の詳細 ] フィールドグループを [!UICONTROL XDM ExperienceEvent] スキーマに追加できます。

同意変更イベントを経時的に追跡する計画がない場合は、このフィールドグループをイベントスキーマに含める必要はありません。 TCF 同意値を自動的に適用する場合、Experience Platformでは、[ プロファイルフィールドグループ ](#profile-field-group) に取り込まれた最新の同意情報のみを使用します。 イベントによって取り込まれた同意値は、自動適用ワークフローには関係しません。

構造とユースケースについて詳しくは、このフィールドグループの [ リファレンスガイド ](../../../../xdm/field-groups/event/iab.md) を参照してください。

## 顧客同意スキーマの作成 {#create-schemas}

同意データを取り込んだデータセットを作成するには、まずデータセットのベースとなる XDM スキーマを作成する必要があります。

前の節で説明したように、ダウンストリーム Experience Platform ワークフローで同意を実施するには、[!UICONTROL XDM 個人プロファイル ] クラスを使用するスキーマが必要です。 また、同意の変化を経時的に追跡する場合は、[!UICONTROL XDM ExperienceEvent] に基づいて個別のスキーマをオプションで作成できます。 両方のスキーマに `identityMap` フィールドと適切な TCF 2.0 フィールドグループが含まれている必要があります。

Experience Platform UI で、左側のナビゲーションで「**[!UICONTROL スキーマ]**」を選択し、「[!UICONTROL  スキーマ ] ワークスペースを開きます。 ここから、以下の節の手順に従って、必要な各スキーマを作成します。

>[!NOTE]
>
>同意データのキャプチャに代わりに使用する既存の XDM スキーマがある場合は、新しいスキーマを作成する代わりに、それらのスキーマを編集できます。 ただし、既存のスキーマがリアルタイム顧客プロファイルでの使用に対して有効になっている場合、そのプライマリ ID は、メールアドレスなどの興味/関心に基づく広告に使用することが禁止されている、直接識別可能なフィールドにはできません。 どのフィールドが制限されているかわからない場合は、法務担当者に問い合わせてください。
>
>また、既存のスキーマを編集する場合は、追加（改行なし）の変更のみ行うことができます。 詳しくは、[ スキーマ進化の原則 ](../../../../xdm/schema/composition.md#evolution) の節を参照してください。

### プロファイル同意スキーマの作成 {#profile-schema}

**[!UICONTROL スキーマを作成]** を選択し、ドロップダウンメニューから **[!UICONTROL XDM 個人プロファイル]** を選択します。

![](../../../images/governance-privacy-security/consent/iab/dataset/create-schema-profile.png)

**[!UICONTROL フィールドグループを追加]** ダイアログが表示され、スキーマへのフィールドグループの追加をすぐに開始できます。 ここから、リストから「**[!UICONTROL IAB TCF 2.0 同意の詳細]**」を選択します。 必要に応じて、検索バーを使用して結果を絞り込み、フィールドグループを見つけやすくすることができます。

![](../../../images/governance-privacy-security/consent/iab/dataset/add-profile-privacy.png)

次に、リストから **[!UICONTROL IdentityMap]** フィールドグループを見つけて、選択します。 右側のパネルに両方のフィールドグループが表示されたら、「**[!UICONTROL フィールドグループを追加]**」を選択します。

![](../../../images/governance-privacy-security/consent/iab/dataset/add-profile-identitymap.png)

キャンバスが再び表示され、`identityPrivacyInfo` フィールドと `identityMap` フィールドがスキーマ構造に追加されたことが示されます。

![](../../../images/governance-privacy-security/consent/iab/dataset/profile-privacy-structure.png)

スキーマにフィールドを追加する前に、ルートフィールドを選択して、右側のパネルに **[!UICONTROL スキーマのプロパティ]** を表示します。ここで、スキーマの名前と説明を指定できます。

![](../../../images/governance-privacy-security/consent/iab/dataset/schema-details-profile.png)

名前と説明を入力したら、キャンバスの左側にある「**[!UICONTROL フィールドグループ]**」セクションの下の「**[!UICONTROL 追加]** を選択して、スキーマにさらにフィールドを追加できます。

![](../../../images/governance-privacy-security/consent/iab/dataset/add-field-group-profile.png)

[!DNL Real-Time Customer Profile] での使用が既に有効になっている既存のスキーマを編集する場合は、「**[!UICONTROL 保存]**」を選択して、[ 同意スキーマに基づくデータセットの作成 ](#dataset) に関する節に進む前に、変更を確認します。 新しいスキーマを作成する場合は、引き続き、以下のサブセクションで説明する手順に従ってください。

#### [!DNL Real-Time Customer Profile] で使用するためのスキーマの有効化 

Experience Platformが受け取った同意データを特定の顧客プロファイルに関連付けるには、[!DNL Real-Time Customer Profile] で使用できるように同意スキーマを有効にする必要があります。

>[!NOTE]
>
>この節で示すスキーマの例では、`identityMap` フィールドをプライマリ ID として使用しています。 別のフィールドをプライマリ ID として設定する場合は、メールアドレスなどの興味/関心に基づく広告で使用が禁止されている直接識別可能なフィールドではなく、Cookie ID などの間接識別子を使用していることを確認します。 どのフィールドが制限されているかわからない場合は、法務担当者に問い合わせてください。
>
>スキーマのプライマリ ID フィールドを設定する手順については、[[!UICONTROL  スキーマ ] UI ガイド ](../../../../xdm/ui/fields/identity.md) を参照してください。

このスキーマを [!DNL Profile] 用に有効にするには、左側のパネルでスキーマの名前を選択して、「**[!UICONTROL スキーマのプロパティ]** セクションを開きます。 ここから、「**[!UICONTROL プロファイル]** 切り替えボタンを選択します。

![](../../../images/governance-privacy-security/consent/iab/dataset/profile-enable-profile.png)

プライマリ ID が見つからないことを示すポップオーバーが表示されます。 プライマリ ID は `identityMap` フィールドに含まれるので、代替プライマリ ID を使用するチェックボックスをオンにします。

![](../../../images/governance-privacy-security/consent/iab/dataset/missing-primary-identity.png)

最後に、「**[!UICONTROL 保存]** を選択して、変更を確定します。

![](../../../images/governance-privacy-security/consent/iab/dataset/profile-save.png)

### イベント同意スキーマの作成 {#event-schema}

>[!NOTE]
>
>イベント同意スキーマは、同意変更イベントを経時的に追跡するためにのみ使用され、ダウンストリームの実施ワークフローには関与しません。 同意の時間的変化を追跡しない場合は、[ 同意データセットの作成 ](#datasets) に関する次の節に進みます。

**[!UICONTROL スキーマ]** ワークスペースで「**[!UICONTROL スキーマを作成]**」を選択し、ドロップダウンから「**[!UICONTROL XDM ExperienceEvent]**」を選択します。

![](../../../images/governance-privacy-security/consent/iab/dataset/create-schema-event.png)

**[!UICONTROL フィールドグループを追加]** ダイアログが表示されます。 ここから、リストから「**[!UICONTROL IAB TCF 2.0 同意の詳細]**」を選択します。 必要に応じて、検索バーを使用して結果を絞り込み、フィールドグループを見つけやすくすることができます。


![](../../../images/governance-privacy-security/consent/iab/dataset/add-event-privacy.png)

次に、リストから **[!UICONTROL IdentityMap]** フィールドグループを見つけて、選択します。 右側のパネルに両方のフィールドグループが表示されたら、「**[!UICONTROL フィールドグループを追加]**」を選択します。

![](../../../images/governance-privacy-security/consent/iab/dataset/add-event-identitymap.png)

キャンバスが再び表示され、`consentStrings` フィールドと `identityMap` フィールドがスキーマ構造に追加されたことが示されます。

![](../../../images/governance-privacy-security/consent/iab/dataset/event-privacy-structure.png)

スキーマにフィールドを追加する前に、ルートフィールドを選択して、右側のパネルに **[!UICONTROL スキーマのプロパティ]** を表示します。ここで、スキーマの名前と説明を指定できます。

![](../../../images/governance-privacy-security/consent/iab/dataset/schema-details-event.png)

名前と説明を入力したら、キャンバスの左側にある「**[!UICONTROL フィールドグループ]**」セクションの下の「**[!UICONTROL 追加]** を選択して、スキーマにさらにフィールドを追加できます。

![](../../../images/governance-privacy-security/consent/iab/dataset/add-field-group-event.png)

必要なフィールドグループを追加したら、「**[!UICONTROL 保存]**」を選択して作業を終了します。

![](../../../images/governance-privacy-security/consent/iab/dataset/event-all-field-groups.png)

## 同意スキーマに基づくデータセットの作成 {#datasets}

上記の必須スキーマごとに、最終的に顧客の同意データを取り込むデータセットを作成する必要があります。 [!DNL Real-Time Customer Profile] に対してレコードスキーマに基づくデータセットを有効にする必要がありますが、時系列スキーマに基づくデータセット **有効にしない** を有効にして [!DNL Profile] ださい。

開始するには、左側のナビゲーションで **[!UICONTROL データセット]** を選択し、右上隅で **[!UICONTROL データセットを作成]** を選択します。

![](../../../images/governance-privacy-security/consent/iab/dataset/dataset-create.png)

次のページで、「**[!UICONTROL スキーマからデータセットを作成]**」を選択します。

![](../../../images/governance-privacy-security/consent/iab/dataset/dataset-create-from-schema.png)

**[!UICONTROL スキーマからデータセットを作成]** ワークフローが表示されるので、**[!UICONTROL スキーマを選択]** 手順から開始します。 提供されたリストで、以前に作成した同意スキーマの 1 つを見つけます。 オプションで、検索バーを使用して結果を絞り込み、スキーマを見つけやすくすることができます。 目的のスキーマの横にあるラジオボタンを選択し、「**[!UICONTROL 次へ]** を選択して続行します。

![](../../../images/governance-privacy-security/consent/iab/dataset/dataset-select-schema.png)

**[!UICONTROL データセットの設定]**&#x200B;手順が表示されます。「完了 **[!UICONTROL を選択する前に、データセットの一意で簡単に識別できる名前と説明を指]** します。

![](../../../images/governance-privacy-security/consent/iab/dataset/dataset-configure.png)

新しく作成されたデータセットの詳細ページが表示されます。 データセットが時系列スキーマに基づいている場合、プロセスは完了です。 データセットがレコードスキーマに基づいている場合、プロセスの最後の手順は、[!DNL Real-Time Customer Profile] で使用するデータセットを有効にすることです。

右側のパネルで「**[!UICONTROL プロファイル]**」切り替えスイッチを選択し、確認ポップオーバーで **[!UICONTROL 有効]** を選択して、スキーマを有効に [!DNL Profile] ます。

![](../../../images/governance-privacy-security/consent/iab/dataset/dataset-enable-profile.png)

イベントベースのデータセットのスキーマを作成した場合は、上記の手順に再度従います。

## 次の手順

このチュートリアルでは、顧客同意データの収集に使用できるデータセットを 1 つ以上作成しました。

* リアルタイム顧客プロファイルでの使用が有効になっている、レコードベースのデータセット。 **（必須）**
* [!DNL Profile] に対して有効になっていない時系列ベースのデータセット。 （オプション）

これで、[IAB TCF 2.0 の概要 ](./overview.md#merge-policies) に戻って、TCF 2.0 準拠のExperience Platformを設定するプロセスを続けることができます。
