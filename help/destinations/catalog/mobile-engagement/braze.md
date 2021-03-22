---
keywords: mobile;ブレーズ。メッセージ；
title: ブレーズ接続
description: Brazeは、顧客と顧客の好みのブランドとの関連性が高く印象的なエクスペリエンスを強化する、包括的な顧客エンゲージメントプラットフォームです。
translation-type: tm+mt
source-git-commit: 7d579d85d427c45f39d000288ed883c7ffd003bf
workflow-type: tm+mt
source-wordcount: '952'
ht-degree: 5%

---


# （ベータ版） [!DNL Braze]接続

>[!IMPORTANT]
>
>Adobe Experience Platformのブレーズ行き先は現在ベータ段階です。 ドキュメントと機能は変更される場合があります。

[!DNL Braze]宛先は、プロファイルデータを[!DNL Braze]に送信するのに役立ちます。

[!DNL Braze] は、顧客と顧客が好むブランドの間の関連性の高い印象的なエクスペリエンスを強化する、包括的な顧客エンゲージメントプラットフォームです。

プロファイルデータを[!DNL Braze]に送信するには、まず宛先に接続する必要があります。

## 宛先の仕様 {#destination-specs}

[!DNL Braze]宛先に固有の次の詳細をメモしておきます。

* [!DNL Adobe Experience Platform] セグメントは、この [!DNL Braze] 属性の `AdobeExperiencePlatformSegments` 下のにエクスポートされます。

>[!NOTE]
>
>追加のカスタム属性を[!DNL Braze]に送信すると、[!DNL Braze]データポイントの消費が増加する可能性があることに注意してください。 追加のカスタム属性を送信する前に、[!DNL Braze]アカウントマネージャにお問い合わせください。

## 使用例 {#use-cases}

マーケターとして、モバイルエンゲージメントの宛先でユーザーをターゲットし、[!DNL Adobe Experience Platform]にセグメントを作成したいと考えています。 さらに、[!DNL Adobe Experience Platform]でセグメントとプロファイルが更新され次第、[!DNL Adobe Experience Platform]プロファイルの属性に基づいてパーソナライズされたエクスペリエンスを提供したいと思います。

## サポートされるID{#supported-identities}

[!DNL Google Ad Manager] は、次の表に示すIDのアクティベーションをサポートしています。

| ターゲットID | 説明 | 注意点 |
|---|---|---|
| external_id | 任意のIDのマッピングをサポートするカスタム[!DNL Braze]識別子。 | [ID](../../../identity-service/namespaces.md)は、[!DNL Braze] [`external_id`](https://www.braze.com/docs/api/basics/#external-user-id-explanation)にマップする限り、[!DNL Braze]宛先に送信できます。 |

## エクスポートタイプ{#export-type}

**[!DNL Profile-based]**  — 目的のスキーマフィールド(例：フィールドマッピングに従って、電子メールアドレス、電話番号、姓)やIDを指定します。[!DNL Adobe Experience Platform] セグメントは、この [!DNL Braze] 属性の `AdobeExperiencePlatformSegments` 下のにエクスポートされます。

## 宛先に接続 {#connect-destination}

**[!UICONTROL 接続]**/**[!UICONTROL 宛先]**&#x200B;で、[!DNL Braze]を選択し、**[!UICONTROL 設定]**&#x200B;を選択します。

![ブレーズ先の設定](../../assets/catalog/mobile-engagement/braze/configure.png)

>[!NOTE]
>
>この宛先との接続が既に存在する場合は、宛先カードに「**[!UICONTROL Activate]**」ボタンが表示されます。 「**[!UICONTROL アクティブ化]**」と「**[!UICONTROL 設定]**」の違いについて詳しくは、保存先のワークスペースドキュメントの「[カタログ](../../ui/destinations-workspace.md#catalog)」の節を参照してください。
>
>![ブレーズ先をアクティブ化](../../assets/catalog/mobile-engagement/braze/activate.png)

[!UICONTROL アカウント]の手順で、[!DNL Braze]アカウントトークンを指定する必要があります。 これが[!DNL Braze] [!DNL API]キーです。 [!DNL API]キーの入手方法に関する詳細な説明は、こちらを参照してください。[REST APIキーの概要](https://www.braze.com/docs/api/api_key/)。 トークンを入力し、**[!UICONTROL 宛先に接続]**&#x200B;をクリックします。

![宛先アカウントのブレーズ手順](../../assets/catalog/mobile-engagement/braze/account.png)

「**[!UICONTROL Next]**」をクリックします。[!UICONTROL 認証]の手順で、[!DNL Braze]接続の詳細を入力する必要があります。
* **[!UICONTROL 名前]**:この宛先を認識するための名前を入力します。
* **[!UICONTROL 説明]**:この宛先を将来特定するのに役立つ説明を入力します。
* **[!UICONTROL エンドポイントインスタンス]**:使用するエンドポイントインスタンスを [!DNL Braze] 担当者に問い合わせます。
* **[!UICONTROL マーケティングアクション]**:マーケティングアクションは、データをエクスポート先にエクスポートする意図を示します。Adobe定義のマーケティングアクションから選択するか、独自のマーケティングアクションを作成することができます。 マーケティング活動の詳細については、「[Adobe Experience Platform](../../../data-governance/policies/overview.md)のデータガバナンス」ページを参照してください。 Adobe定義の個々のマーケティングアクションについて詳しくは、[データ使用ポリシーの概要](../../../data-governance/policies/overview.md)を参照してください。

![ブレーズ認証手順](../../assets/catalog/mobile-engagement/braze/authentication.png)

「**[!UICONTROL 宛先を作成]**」をクリックします。 これで宛先が作成されました。後でセグメントをアクティブにする場合は、「**[!UICONTROL 保存して終了]**」をクリックできます。または、「**[!UICONTROL 次へ]**」を選択してワークフローを続行し、アクティブにするセグメントを選択できます。 どちらの場合も、残りのワークフローについては、次の「[セグメントをアクティブにする](#activate-segments)」の節を参照してください。

## セグメントのアクティブ化 {#activate-segments}

セグメントのアクティベーションワークフローについて詳しくは、「[宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-destinations.md#select-attributes)」を参照してください。

## フィールドマッピング{#field-mapping}

オーディエンスデータを[!DNL Adobe Experience Platform]から[!DNL Braze]宛先に正しく送信するには、フィールドマッピング手順を実行する必要があります。

マッピングは、[!DNL Platform]アカウント内の[!DNL Experience Data Model] (XDM)スキーマフィールドと、ターゲット先から対応するリンクの作成で構成されます。

XDMフィールドを[!DNL Braze]宛先フィールドに正しくマップするには、次の手順に従います。

[!UICONTROL マッピング]の手順で、**[!UICONTROL 追加新しいマッピング]**&#x200B;をクリックします。

![宛先マッピングを追加ブレーズ](../../assets/catalog/mobile-engagement/braze/mapping.png)

「[!UICONTROL ソースフィールド]」セクションで、空のフィールドの横にある矢印ボタンをクリックします。

![宛先ソースマッピングをブレーズ](../../assets/catalog/mobile-engagement/braze/mapping-source.png)

[!UICONTROL ソースフィールドを選択]ウィンドウで、次の2つのカテゴリのXDMフィールドを選択できます。
* [!UICONTROL 属性を選択]:XDMスキーマの特定のフィールドを [!DNL Braze] 属性にマップするには、このオプションを使用します。

![宛先マッピング元の属性をブレーズ](../../assets/catalog/mobile-engagement/braze/mapping-attributes.png)

* [!UICONTROL ID名前空間の選択]:このオプションを使用して、 [!DNL Platform] ID名前空間を [!DNL Braze] 名前空間にマップします。

![宛先マッピング元の名前空間をブレーズ](../../assets/catalog/mobile-engagement/braze/mapping-namespaces.png)

ソースフィールドを選択し、[**[!UICONTROL 選択]**]をクリックします。

「[!UICONTROL ターゲットフィールド]」セクションで、フィールドの右側のマッピングアイコンをクリックします。

![ブレーズ先ターゲットマッピング](../../assets/catalog/mobile-engagement/braze/mapping-target.png)

[!UICONTROL ターゲットフィールドを選択]ウィンドウで、次の3つのカテゴリのターゲットフィールドから選択できます。
* [!UICONTROL 属性を選択]:XDM属性を標準 [!DNL Braze] 属性にマップするには、このオプションを使用します。
* [!UICONTROL ID名前空間の選択]:このオプションを使用して、 [!DNL Platform] ID名前空間を [!DNL Braze] ID名前空間にマップします。
* [!UICONTROL カスタム属性を選択]:このオプションを使用して、XDM属性を、ア [!DNL Braze] カウントで定義したカスタム [!DNL Braze] 属性にマップします。
* また、このオプションを使用して、既存のXDM属性の名前を[!DNL Braze]に変更することもできます。 例えば、`lastName` XDM属性を[!DNL Braze]のカスタム`Last_Name`属性にマッピングし、[!DNL Braze]に`Last_Name`属性が存在しない場合は作成し、`lastName` XDM属性をマッピングします。

![宛先ターゲットマッピングフィールドをブレーズ](../../assets/catalog/mobile-engagement/braze/mapping-target-fields.png)

ターゲットフィールドを選択し、「**[!UICONTROL 選択]**」をクリックします。

これで、フィールドマッピングがリストに表示されます。

![宛先マッピングのブレーズ完了](../../assets/catalog/mobile-engagement/braze/mapping-complete.png)

さらにマッピングを追加するには、前の手順を繰り返します。

## マッピングの例{#mapping-example}

例えば、XDMプロファイルスキーマと[!DNL Braze]インスタンスに次の属性とIDが含まれているとします。

|  | XDMプロファイルスキーマ | [!DNL Braze] インスタンス |
|---|---|---|
| 属性 | <ul><li>person.name.firstName</code></li><li>person.name.lastName</code></li><li>mobilePhone.number</code></li></ul> | <ul><li>名</code></li><li>姓</code></li><li>電話番号</code></li></ul> |
| ID | <ul><li>Email</code></li><li>Google広告ID (GAID)</code></li><li>Apple ID For Advertisers(IDFA)</code></li></ul> | <ul><li>external_id</code></li></ul> |

正しいマッピングは次のようになります。

![宛先マッピングのブレーズの例](../../assets/catalog/mobile-engagement/braze/mapping-example.png)

## エクスポートされたデータ{#exported-data}

データが[!DNL Braze]宛先に正常にエクスポートされたかどうかを確認するには、[!DNL Braze]アカウントを確認してください。 [!DNL Adobe Experience Platform] セグメントは、この [!DNL Braze] 属性の `AdobeExperiencePlatformSegments` 下のにエクスポートされます。

## データの使用とガバナンス{#data-usage-governance}

すべての[!DNL Adobe Experience Platform]宛先は、データ処理時のデータ使用ポリシーに準拠しています。 [!DNL Adobe Experience Platform]によるデータガバナンスの適用方法について詳しくは、[データガバナンスの概要](../../../data-governance/home.md)を参照してください。

