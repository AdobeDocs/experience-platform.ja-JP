---
keywords: Experience Platform；ホーム；IAB;IAB 2.0；同意；同意
solution: Experience Platform
title: IAB TCF 2.0同意データを取り込むデータセットの作成
topic-legacy: privacy events
description: このドキュメントでは、IAB TCF 2.0同意データを収集するために必要な2つのデータセットを設定する手順を説明します。
exl-id: 36b2924d-7893-4c55-bc33-2c0234f1120e
source-git-commit: 9b75a69cc6e31ea0ad77048a6ec1541df2026f27
workflow-type: tm+mt
source-wordcount: '1576'
ht-degree: 3%

---

# IAB TCF 2.0同意データを取り込むためのデータセットの作成

Adobe Experience PlatformがIAB [!DNL Transparency & Consent Framework](TCF)2.0に従って顧客の同意データを処理するには、そのデータを、スキーマにTCF 2.0の同意フィールドが含まれるデータセットに送信する必要があります。

特に、TCF 2.0の同意データを取り込むには、2つのデータセットが必要です。

* [!DNL XDM Individual Profile]クラスに基づくデータセット。[!DNL Real-time Customer Profile]で使用できるようになります。
* [!DNL XDM ExperienceEvent]クラスに基づくデータセット。

>[!IMPORTANT]
>
>Platformは、個々のプロファイルデータセットで収集されたTCF文字列のみを強制します。 このワークフローの一部としてデータストリームを作成するには、ExperienceEventデータセットが引き続き必要ですが、データをプロファイルデータセットに取り込むだけで済みます。 ExperienceEventデータセットは、同意の変更イベントを時間の経過と共に追跡する場合にも使用できますが、セグメントのアクティブ化時にこれらの値を適用する場合、では使用されません。

このドキュメントでは、これら2つのデータセットを設定する手順を説明します。 TCF 2.0用のPlatformデータ操作を設定する完全なワークフローの概要については、[IAB TCF 2.0コンプライアンスの概要](./overview.md)を参照してください。

## 前提条件

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [エクスペリエンスデータモデル（XDM）](../../../../xdm/home.md)[!DNL Experience Platform]： が顧客エクスペリエンスデータを整理する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../xdm/schema/composition.md)：XDM スキーマの基本的な構成要素について説明します。
* [Adobe Experience Platform Identity Service](../../../../identity-service/home.md):デバイスやシステムをまたいで、異なるデータソースから顧客IDを結び付けることができます。
   * [ID名前空間](../../../../identity-service/namespaces.md):顧客IDデータは、IDサービスで認識される特定のID名前空間で提供される必要があります。
* [リアルタイム顧客プロファイル](../../../../profile/home.md):を利用す [!DNL Identity Service] ると、データセットから詳細な顧客プロファイルをリアルタイムで作成できます。[!DNL Real-time Customer Profile] は、データレイクからデータを取り込み、顧客プロファイルを独立したデータストアに保持します。

## TCF 2.0フィールドグループ {#field-groups}

[!UICONTROL IAB TCF 2.0 Consent]スキーマフィールドグループは、TCF 2.0のサポートに必要な顧客の同意フィールドを提供します。 このフィールドグループには、次の2つのバージョンがあります。1つは[!DNL XDM Individual Profile]クラスと互換性があり、もう1つは[!DNL XDM ExperienceEvent]クラスと互換性があります。

以下の節では、取り込み時に予想されるデータを含め、これらの各フィールドグループの構造について説明します。

### プロファイルフィールドグループ {#profile-field-group}

[!DNL XDM Individual Profile]に基づくスキーマの場合、[!UICONTROL IAB TCF 2.0 Consent]フィールドグループには、顧客IDをTCF同意設定にマッピングする、単一のマップタイプフィールド`identityPrivacyInfo`が用意されています。 自動強制をおこなうには、このフィールドグループを、リアルタイム顧客プロファイルが有効なレコードベースのスキーマに含める必要があります。

このフィールドグループの構造と使用例について詳しくは、[リファレンスガイド](../../../../xdm/field-groups/profile/iab.md)を参照してください。

### イベントフィールドグループ {#event-field-group}

同意変更イベントを時間の経過と共に追跡する場合は、[!UICONTROL IAB TCF 2.0 Consent]フィールドグループを[!UICONTROL XDM ExperienceEvent]スキーマに追加します。

同意の変更イベントを経時的に追跡する予定がない場合は、このフィールドグループをイベントスキーマに含める必要はありません。 TCFの同意値を自動的に適用する場合、Experience Platformは、[profileフィールドグループ](#profile-field-group)に取り込まれた最新の同意情報のみを使用します。 イベントによってキャプチャされた同意の値は、自動実施ワークフローには関与しません。

このフィールドグループの構造と使用例について詳しくは、[リファレンスガイド](../../../../xdm/field-groups/event/iab.md)を参照してください。

## 顧客の同意スキーマの作成 {#create-schemas}

同意データをキャプチャするデータセットを作成するには、まずXDMスキーマを作成し、これらのデータセットのベースとする必要があります。

Platform UIで、左側のナビゲーションで「**[!UICONTROL スキーマ]**」を選択し、「[!UICONTROL スキーマ]」ワークスペースを開きます。 ここから、以下の節の手順に従って、必要な各スキーマを作成します。

>[!NOTE]
>
>同意データの取得に使用する既存のXDMスキーマがある場合は、新しいスキーマを作成する代わりに、これらのスキーマを編集できます。 ただし、既存のスキーマのリアルタイム顧客プロファイルでの使用が有効になっている場合、そのプライマリIDを、電子メールアドレスなど、興味に基づく広告での使用を禁止されている、直接識別可能なフィールドにすることはできません。 どのフィールドが制限されているかが不明な場合は、弁護士に相談してください。
>
>また、既存のスキーマを編集する場合は、改行しない追加の変更のみを加えることができます。 詳しくは、[スキーマ進化の原則](../../../../xdm/schema/composition.md#evolution)の節を参照してください。

### プロファイル同意スキーマの作成 {#profile-schema}

「**[!UICONTROL スキーマ]**&#x200B;を作成」を選択し、ドロップダウンメニューから「**[!UICONTROL XDM個別プロファイル]**」を選択します。

![](../../../images/governance-privacy-security/consent/iab/dataset/create-schema-profile.png)

**[!UICONTROL フィールドグループの追加]**&#x200B;ダイアログが表示され、すぐにフィールドグループをスキーマに追加できます。 ここから、リストから&#x200B;**[!UICONTROL IAB TCF 2.0 Consent]**&#x200B;を選択します。 オプションで、検索バーを使用して結果を絞り込み、フィールドグループを見つけやすくすることができます。 フィールドグループを選択したら、「**[!UICONTROL フィールドグループを追加]**」を選択します。

![](../../../images/governance-privacy-security/consent/iab/dataset/add-profile-privacy.png)

キャンバスが再び表示され、「`identityPrivacyInfo`」フィールドがスキーマ構造に追加されたことが示されます。

![](../../../images/governance-privacy-security/consent/iab/dataset/profile-privacy-structure.png)

スキーマにフィールドを追加する前に、ルートフィールドを選択し、右側のレールに&#x200B;**[!UICONTROL スキーマのプロパティ]**&#x200B;が表示されます。このレールで、スキーマの名前と説明を指定できます。

![](../../../images/governance-privacy-security/consent/iab/dataset/schema-details-profile.png)

名前と説明を指定したら、キャンバスの左側にある「**[!UICONTROL フィールドグループ]**」セクションの下にある「**[!UICONTROL 追加]**」を選択します。

![](../../../images/governance-privacy-security/consent/iab/dataset/add-field-group-profile.png)

ここから、ダイアログを使用して、次のフィールドグループをスキーマに追加します。

* [!UICONTROL IdentityMap]
* [!UICONTROL プロファイルのデータ取得領域]
* [!UICONTROL 人口統計の詳細]
* [!UICONTROL 個人の連絡先の詳細]

![](../../../images/governance-privacy-security/consent/iab/dataset/profile-all-field-groups.png)

既に[!DNL Real-time Customer Profile]での使用が有効になっている既存のスキーマを編集する場合は、「**[!UICONTROL 保存]**」を選択して変更を確定してから、[同意スキーマに基づくデータセットの作成](#dataset)の節に進みます。 新しいスキーマを作成する場合は、次の節で説明する手順に従います。

#### [!DNL Real-time Customer Profile]でのスキーマの使用を有効にします

Platformが受け取った同意データを特定の顧客プロファイルに関連付けるには、[!DNL Real-time Customer Profile]での使用を同意スキーマで有効にする必要があります。

>[!NOTE]
>
>この節に示すサンプルのスキーマでは、`identityMap`フィールドをプライマリIDとして使用しています。 別のフィールドをプライマリIDとして設定する場合は、Cookie IDなどの間接識別子を使用し、Eメールアドレスなどの興味/関心に基づく広告で使用できない直接識別可能なフィールドを使用しないようにします。 どのフィールドが制限されているかが不明な場合は、弁護士に相談してください。
>
>スキーマのプライマリIDフィールドを設定する手順については、『[スキーマ作成のチュートリアル](../../../../xdm/tutorials/create-schema-ui.md#identity-field)』を参照してください。

[!DNL Profile]のスキーマを有効にするには、左側のレールでスキーマの名前を選択して、「**[!UICONTROL スキーマのプロパティ]**」セクションを開きます。 ここから、「**[!UICONTROL プロファイル]**」切り替えボタンを選択します。

![](../../../images/governance-privacy-security/consent/iab/dataset/profile-enable-profile.png)

プライマリIDが見つからないことを示すポップオーバーが表示されます。 プライマリIDは`identityMap`フィールドに含まれるので、代替プライマリIDを使用するチェックボックスをオンにします。

![](../../../images/governance-privacy-security/consent/iab/dataset/missing-primary-identity.png)

最後に、「**[!UICONTROL 保存]**」を選択して変更を確定します。

![](../../../images/governance-privacy-security/consent/iab/dataset/profile-save.png)

### イベント同意スキーマの作成 {#event-schema}

**[!UICONTROL スキーマ]**&#x200B;ワークスペースで、「**[!UICONTROL スキーマを作成]**」を選択し、ドロップダウンから「**[!UICONTROL XDM ExperienceEvent]**」を選択します。

![](../../../images/governance-privacy-security/consent/iab/dataset/create-schema-event.png)

**[!UICONTROL フィールドグループの追加]**&#x200B;ダイアログが表示されます。 ここから、リストから&#x200B;**[!UICONTROL IAB TCF 2.0 Consent]**&#x200B;を選択します。 オプションで、検索バーを使用して結果を絞り込み、フィールドグループを見つけやすくすることができます。 フィールドグループを選択したら、「**[!UICONTROL フィールドグループを追加]**」を選択します。

>[!NOTE]
>
>このフィールドグループをイベントスキーマに含める必要があるのは、同意の変更イベントを経時的に追跡する場合のみです。 これらのイベントを追跡しない場合は、Web SDKを設定する際に、これらのフィールドを使用せずにイベントスキーマを使用できます。

![](../../../images/governance-privacy-security/consent/iab/dataset/add-event-privacy.png)

キャンバスが再び表示され、「`consentStrings`」フィールドがスキーマ構造に追加されたことが示されます。

![](../../../images/governance-privacy-security/consent/iab/dataset/event-privacy-structure.png)

スキーマにフィールドを追加する前に、ルートフィールドを選択し、右側のレールに&#x200B;**[!UICONTROL スキーマのプロパティ]**&#x200B;が表示されます。このレールで、スキーマの名前と説明を指定できます。

![](../../../images/governance-privacy-security/consent/iab/dataset/schema-details-event.png)

名前と説明を指定したら、キャンバスの左側にある「**[!UICONTROL フィールドグループ]**」セクションの下にある「**[!UICONTROL 追加]**」を選択します。

![](../../../images/governance-privacy-security/consent/iab/dataset/add-field-group-event.png)

ここから、上記の手順を繰り返して、次のフィールドグループをスキーマに追加します。

* [!UICONTROL IdentityMap]
* [!UICONTROL 環境の詳細]
* [!UICONTROL Webの詳細]
* [!UICONTROL 実装の詳細]

フィールドグループを追加したら、「**[!UICONTROL 保存]**」を選択して作業を完了します。

![](../../../images/governance-privacy-security/consent/iab/dataset/event-all-field-groups.png)

## 同意スキーマに基づくデータセットの作成 {#datasets}

上記に説明した必要な各スキーマに対して、顧客の同意データを最終的に取り込むデータセットを作成する必要があります。 レコードスキーマに基づくデータセットは[!DNL Real-time Customer Profile]に対して有効にする必要がありますが、時系列スキーマ&#x200B;**に基づくデータセットは[!DNL Profile]に対して有効にしないでください。**

最初に、左のナビゲーションで「**[!UICONTROL データセット]**」を選択し、右上隅の「**[!UICONTROL データセットを作成]**」を選択します。

![](../../../images/governance-privacy-security/consent/iab/dataset/dataset-create.png)

次のページで、「**[!UICONTROL Create dataset from schema]**」を選択します。

![](../../../images/governance-privacy-security/consent/iab/dataset/dataset-create-from-schema.png)

**[!UICONTROL スキーマからデータセットを作成]**&#x200B;ワークフローが表示され、**[!UICONTROL スキーマを選択]**&#x200B;の手順を開始します。 提供されたリストで、作成した同意スキーマの1つを探します。 オプションで、検索バーを使用して結果を絞り込み、スキーマを見つけやすくすることができます。 目的のスキーマの横にあるラジオボタンを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![](../../../images/governance-privacy-security/consent/iab/dataset/dataset-select-schema.png)

**[!UICONTROL データセットの設定]**&#x200B;手順が表示されます。「**[!UICONTROL 完了]**」を選択する前に、データセットの一意の、識別しやすい名前と説明を指定します。

![](../../../images/governance-privacy-security/consent/iab/dataset/dataset-configure.png)

新しく作成されたデータセットの詳細ページが表示されます。 データセットが時系列スキーマに基づいている場合、プロセスは完了です。 データセットがレコードスキーマに基づいている場合、プロセスの最後の手順は、[!DNL Real-time Customer Profile]で使用するデータセットを有効にすることです。

右側のレールで、「**[!UICONTROL プロファイル]**」切り替えを選択し、確認ポップオーバーで「**[!UICONTROL 有効]**」を選択して、[!DNL Profile]のスキーマを有効にします。

![](../../../images/governance-privacy-security/consent/iab/dataset/dataset-enable-profile.png)

上記の手順を再度実行して、TCF 2.0への準拠に必要なその他のデータセットを作成します。

## 次の手順

このチュートリアルに従って、顧客の同意データを収集するために使用できる2つのデータセットを作成しました。

* リアルタイム顧客プロファイルで使用できるレコードベースのデータセット。
* [!DNL Profile]に対して有効になっていない時系列ベースのデータセット。

これで、[IAB TCF 2.0の概要](./overview.md#merge-policies)に戻り、TCF 2.0への準拠のためのプラットフォーム設定のプロセスを続行できます。
