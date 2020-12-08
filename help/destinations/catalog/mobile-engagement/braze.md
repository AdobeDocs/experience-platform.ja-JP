---
keywords: mobile; braze; messaging;
title: ブレーズ先
seo-title: ブレーズ先
description: Brazeは、顧客と顧客の好みのブランドとの関連性が高く印象的なエクスペリエンスを強化する、包括的な顧客エンゲージメントプラットフォームです。
seo-description: Brazeは、顧客と顧客の好みのブランドとの関連性が高く印象的なエクスペリエンスを強化する、包括的な顧客エンゲージメントプラットフォームです。
translation-type: tm+mt
source-git-commit: 6b19cfa3c4a5327b6b7543123f631d0355995f09
workflow-type: tm+mt
source-wordcount: '953'
ht-degree: 5%

---


# （ベータ） [!DNL Braze] 宛先

>[!IMPORTANT]
>
>Adobe Experience Platformのブレーズ行き先は現在ベータ段階です。 ドキュメントと機能は変更される場合があります。

## 概要 {#overview}

送信先は、 [!DNL Braze] プロファイルデータの送信先に役立ち [!DNL Braze]ます。

[!DNL Braze] は、顧客と顧客が好むブランドの間の関連性の高い印象的なエクスペリエンスを強化する、包括的な顧客エンゲージメントプラットフォームです。

にプロファイルデータを送信するに [!DNL Braze]は、まず宛先に接続する必要があります。

## 宛先の仕様 {#destination-specs}

Note the following details that are specific to the [!DNL Braze] destination:

* 宛先に [IDをマップする限り](../../../identity-service/namespaces.md) 、任意の [!DNL Braze] IDを [!DNL Braze] 宛先に送信でき [`external_id`](https://www.braze.com/docs/api/basics/#external-user-id-explanation)ます。
* [!DNL Adobe Experience Platform] セグメントは、属性 [!DNL Braze] の下のにエクスポートされ `AdobeExperiencePlatformSegments` ます。

>[!NOTE]
>
>に追加のカスタム属性を送信すると、データポイントの使用量が増加する [!DNL Braze][!DNL Braze] 可能性があることに注意してください。 追加のカスタム属性を送信する前に、担当の [!DNL Braze] アカウントマネージャーにお問い合わせください。

## 使用例 {#use-cases}

マーケターとして、セグメントを組み込んだモバイルエンゲージメントの宛先でターゲットしたいと思い [!DNL Adobe Experience Platform]ます。 さらに、セグメントとプロファイルが更新されたら、そのプロファイルの属性に基づいて、パーソナライズされたエクスペリエンスを [!DNL Adobe Experience Platform] 配信したいと思い [!DNL Adobe Experience Platform]ます。

## 書き出しタイプ {#export-type}

**[!DNL Profile-based]**  — 目的のスキーマフィールド(例：フィールドマッピングに従って、電子メールアドレス、電話番号、姓)やIDを指定します。
[!DNL Adobe Experience Platform] セグメントは、属性 [!DNL Braze] の下のにエクスポートされ `AdobeExperiencePlatformSegments` ます。


## 宛先に接続 {#connect-destination}

**[!UICONTROL 接続]** / **[!UICONTROL 宛先]**、を選択し、「 [!DNL Braze]設定 ****」を選択します。

![ブレーズ先の設定](../../assets/catalog/mobile-engagement/braze/configure.png)

>[!NOTE]
>
>この宛先との接続が既に存在する場合は、宛先カードに **[!UICONTROL 「アクティブ化]** 」ボタンが表示されます。 「 **[!UICONTROL アクティブ化]** 」と「 **[!UICONTROL 設定]**」の違いについて詳しくは、表示先ワークスペースのドキュメントの「 [カタログ](../../ui/destinations-workspace.md#catalog) 」セクションを参照してください。
>
>![ブレーズ先をアクティブ化](../../assets/catalog/mobile-engagement/braze/activate.png)

「 [!UICONTROL アカウント][!DNL Braze] 」手順で、アカウントトークンを指定する必要があります。 これが君の [!DNL Braze][!DNL API] 鍵だ。 キーの取得方法の詳細な手順は、次を参照してください。 [!DNL API][REST APIキーの概要](https://www.braze.com/docs/api/api_key/)。 トークンを入力し、「 **[!UICONTROL Connect to destination]**」をクリックします。

![宛先アカウントのブレーズ手順](../../assets/catalog/mobile-engagement/braze/account.png)

「**[!UICONTROL Next]**」をクリックします。「 [!UICONTROL 認証] 」の手順で、 [!DNL Braze] 接続の詳細を入力する必要があります。
* **[!UICONTROL 名前]**:この宛先を認識するための名前を入力します。
* **[!UICONTROL 説明]**:この宛先を将来特定するのに役立つ説明を入力します。
* **[!UICONTROL エンドポイントインスタンス]**:使用するエンドポイントインスタンスを [!DNL Braze] 担当者に問い合わせます。
* **[!UICONTROL マーケティングの使用例]**:マーケティングの使用例は、目的のデータが目的のデータにエクスポートされることを示します。 Adobe定義のマーケティングの使用例から選択するか、独自のマーケティングの使用例を作成することができます。 マーケティングの使用例について詳しくは、「 [データガバナンス(Adobe Experience Platform](../../../rtcdp/privacy/data-governance-overview.md#destinations) )」ページを参照してください。 個々のAdobe定義マーケティングの使用例について詳しくは、 [データ使用ポリシーの概要を参照してください](../../../data-governance/policies/overview.md#core-actions)。

![ブレーズ認証手順](../../assets/catalog/mobile-engagement/braze/authentication.png)

「 **[!UICONTROL 作成先]**」をクリックします。 これで宛先が作成されました。You can click **[!UICONTROL Save &amp; Exit]** if you want to activate segments later, or you can select **[!UICONTROL Next]** to continue the workflow and select segments to activate. In either case, see the next section, [Activate Segments](#activate-segments), for the rest of the workflow.

## セグメントのアクティブ化 {#activate-segments}

セグメントのアクティベーションワークフローについて詳しくは、「[宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-destinations.md#select-attributes)」を参照してください。

## フィールドマッピング {#field-mapping}

オーディエンスデータをから宛先 [!DNL Adobe Experience Platform] に正しく送信するには、フィールドマッピング手順を実行する必要があり [!DNL Braze] ます。

マッピングは、アカウント内の [!DNL Experience Data Model] (XDM)スキーマフィールドと、ターゲット先からの対応するフィールド間のリンクの作成で構成され [!DNL Platform] ます。

XDMフィールドを [!DNL Braze] 宛先フィールドに正しくマップするには、次の手順に従います。

「 [!UICONTROL マッピング] 」の手順で、「 **[!UICONTROL 新しいマッピング]**」をクリックします。

![宛先マッピングを追加ブレーズ](../../assets/catalog/mobile-engagement/braze/mapping.png)

「 [!UICONTROL ソースフィールド] 」セクションで、空のフィールドの横にある矢印ボタンをクリックします。

![宛先ソースマッピングをブレーズ](../../assets/catalog/mobile-engagement/braze/mapping-source.png)

「 [!UICONTROL Select source field] 」ウィンドウで、XDMフィールドの2つのカテゴリの中から選択できます。
* [!UICONTROL 属性を選択]:XDMスキーマの特定のフィールドを [!DNL Braze] 属性にマップするには、このオプションを使用します。

![宛先マッピング元の属性をブレーズ](../../assets/catalog/mobile-engagement/braze/mapping-attributes.png)

* [!UICONTROL ID名前空間の選択]:ID [!DNL Platform] 名前空間を [!DNL Braze] 名前空間にマップするには、このオプションを使用します。

![宛先マッピング元の名前空間をブレーズ](../../assets/catalog/mobile-engagement/braze/mapping-namespaces.png)

ソースフィールドを選択し、「 **[!UICONTROL 選択]**」をクリックします。

「 [!UICONTROL ターゲットフィールド] 」セクションで、フィールドの右側のマッピングアイコンをクリックします。

![ブレーズ先ターゲットマッピング](../../assets/catalog/mobile-engagement/braze/mapping-target.png)

「 [!UICONTROL ターゲットを選択」フィールド] (Select Messure Field)ウィンドウでは、次の3つのターゲットフィールドから選択できます。
* [!UICONTROL 属性を選択]:XDM属性を標準 [!DNL Braze] 属性にマップするには、このオプションを使用します。
* [!UICONTROL ID名前空間の選択]:ID [!DNL Platform] 名前空間をID名前空間にマップするには、このオプションを使用し [!DNL Braze] ます。
* [!UICONTROL カスタム属性を選択]:このオプションを使用して、XDM属性を、ア [!DNL Braze][!DNL Braze] カウントで定義したカスタム属性にマップします。
* また、このオプションを使用して、既存のXDM属性の名前をに変更することもでき [!DNL Braze]ます。 例えば、のカスタム `lastName` 属性に `Last_Name` XDM属性をマッピングすると、 [!DNL Braze]属性が存在しない場合 `Last_Name` は、で [!DNL Braze]属性が作成され、 `lastName` XDM属性がマッピングされます。

![宛先ターゲットマッピングフィールドをブレーズ](../../assets/catalog/mobile-engagement/braze/mapping-target-fields.png)

ターゲットフィールドを選択し、「 **[!UICONTROL 選択]**」をクリックします。

これで、フィールドマッピングがリストに表示されます。

![宛先マッピングのブレーズ完了](../../assets/catalog/mobile-engagement/braze/mapping-complete.png)

さらにマッピングを追加するには、前の手順を繰り返します。

### 例 {#mapping-example}

XDMプロファイルスキーマとインスタンスに次の属性とIDが含まれているとします。 [!DNL Braze]

|  | XDMプロファイルスキーマ | [!DNL Braze] インスタンス |
|---|---|---|
| 属性 | <ul><li>person.name.firstName</code></li><li>person.name.lastName</code></li><li>mobilePhone.number</code></li></ul> | <ul><li>FirstName</code></li><li>LastName</code></li><li>電話番号</code></li></ul> |
| ID | <ul><li>Email</code></li><li>Google広告ID(GAID)</code></li><li>Apple ID For Advertisers (IDFA)</code></li></ul> | <ul><li>external_id</code></li></ul> |

正しいマッピングは次のようになります。

![宛先マッピングのブレーズの例](../../assets/catalog/mobile-engagement/braze/mapping-example.png)

## 書き出されたデータ {#exported-data}

データが正常に宛先にエクスポートされたかどうかを確認するには、アカウントを確認して [!DNL Braze] く [!DNL Braze] ださい。 [!DNL Adobe Experience Platform] セグメントは、属性 [!DNL Braze] の下のにエクスポートされ `AdobeExperiencePlatformSegments` ます。

## データの使用とガバナンス {#data-usage-governance}

すべての [!DNL Adobe Experience Platform] 宛先は、データ処理時のデータ使用ポリシーに準拠しています。 データ・ガバナンスの [!DNL Adobe Experience Platform] 実施方法の詳細については、「 [Data Governance in Real-time CDP](../../../rtcdp/privacy/data-governance-overview.md)」を参照してください。

