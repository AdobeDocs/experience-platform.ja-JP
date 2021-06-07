---
keywords: モバイル；勇気づけメッセージ
title: 接続をブレーズ
description: Brazeは、顧客と顧客が好むブランドとの間の関連性の高い思い出に残るエクスペリエンスを強化する、包括的な顧客エンゲージメントプラットフォームです。
exl-id: 508e79ee-7364-4553-b153-c2c00cc85a73
source-git-commit: 66c3e81dfdbf6f6c3ff9a127fbca8943c0e32279
workflow-type: tm+mt
source-wordcount: '953'
ht-degree: 4%

---

# （ベータ版） [!DNL Braze]接続

>[!IMPORTANT]
>
>Adobe Experience PlatformのBrazeの宛先は現在ベータ版です。 ドキュメントと機能は変更される場合があります。

## 概要 {#overview}

[!DNL Braze]の宛先は、プロファイルデータを[!DNL Braze]に送信する際に役立ちます。

[!DNL Braze] は、顧客と顧客が好むブランドの間の関連性の高い思い出に残るエクスペリエンスを強化する、包括的な顧客エンゲージメントプラットフォームです。

プロファイルデータを[!DNL Braze]に送信するには、まず宛先に接続する必要があります。

## 宛先の詳細 {#specifics}

次の[!DNL Braze]宛先に固有の詳細に注意してください。

* [!DNL Adobe Experience Platform] セグメントは、属性のにエ [!DNL Braze] クスポート `AdobeExperiencePlatformSegments` されます。

>[!NOTE]
>
>追加のカスタム属性を[!DNL Braze]に送信すると、[!DNL Braze]データポイントの消費が増加する場合があることに注意してください。 追加のカスタム属性を送信する前に、[!DNL Braze]アカウントマネージャーにお問い合わせください。

## 使用例 {#use-cases}

マーケティング担当者の場合、[!DNL Adobe Experience Platform]でセグメントを構築し、モバイルエンゲージメントの宛先でユーザーをターゲットにしたいと考えています。 さらに、[!DNL Adobe Experience Platform]でセグメントとプロファイルが更新されしだい、[!DNL Adobe Experience Platform]プロファイルの属性に基づいて、パーソナライズされたエクスペリエンスをユーザーに提供したいと考えています。

## サポートされているID{#supported-identities}

[!DNL Braze] では、以下の表で説明するIDのアクティブ化をサポートしています。

| ターゲットID | 説明 | 注意点 |
|---|---|---|
| external_id | 任意のIDのマッピングをサポートするカスタムの[!DNL Braze]識別子。 | [!DNL Braze] [`external_id`](https://www.braze.com/docs/api/basics/#external-user-id-explanation)にマッピングしている限り、任意の[ID](../../../identity-service/namespaces.md)を[!DNL Braze]宛先に送信できます。 |

## エクスポートタイプ{#export-type}

**[!DNL Profile-based]** ：セグメントのすべてのメンバーを、目的のスキーマフィールド(例：フィールドマッピングに従って、電子メールアドレス、電話番号、姓)やID（またはその両方）を表示します。[!DNL Adobe Experience Platform] セグメントは、属性のにエ [!DNL Braze] クスポート `AdobeExperiencePlatformSegments` されます。

## 宛先に接続 {#connect-destination}

**[!UICONTROL 接続]** > **[!UICONTROL 宛先]**&#x200B;で、「[!DNL Braze]」を選択し、「**[!UICONTROL 設定]**」を選択します。

![Brazeの宛先の設定](../../assets/catalog/mobile-engagement/braze/configure.png)

>[!NOTE]
>
>この宛先との接続が既に存在する場合は、宛先カードに「**[!UICONTROL アクティブ化]**」ボタンが表示されます。 **[!UICONTROL アクティブ化]**&#x200B;と&#x200B;**[!UICONTROL 設定]**&#x200B;の違いについて詳しくは、宛先ワークスペースのドキュメントの[カタログ](../../ui/destinations-workspace.md#catalog)の節を参照してください。
>
>![ブレーズ先を有効化](../../assets/catalog/mobile-engagement/braze/activate.png)

[!UICONTROL アカウント]の手順で、[!DNL Braze]アカウントトークンを指定する必要があります。 これは[!DNL Braze] [!DNL API]キーです。 [!DNL API]キーの入手方法の詳細は、次のURLを参照してください。[REST APIキーの概要](https://www.braze.com/docs/api/api_key/)。 トークンを入力し、「**[!UICONTROL 宛先に接続]**」をクリックします。

![宛先アカウントの分類手順](../../assets/catalog/mobile-engagement/braze/account.png)

「**[!UICONTROL 次へ]**」をクリックします。[!UICONTROL 認証]手順で、[!DNL Braze]接続の詳細を入力する必要があります。
* **[!UICONTROL 名前]**:この宛先が将来認識される名前を入力します。
* **[!UICONTROL 説明]**:この宛先を将来識別するのに役立つ説明を入力します。
* **[!UICONTROL エンドポイントインスタンス]**:使用する必要のあ [!DNL Braze] るエンドポイントインスタンスを担当者に問い合わせてください。
* **[!UICONTROL マーケティングアクション]**:マーケティングアクションは、宛先に書き出すデータの目的を示します。Adobe定義のマーケティングアクションから選択することも、独自のマーケティングアクションを作成することもできます。 マーケティングアクションについて詳しくは、「Adobe Experience Platformの[データガバナンス](../../../data-governance/policies/overview.md) 」ページを参照してください。 個々のAdobe定義マーケティングアクションについて詳しくは、「[データ使用ポリシーの概要](../../../data-governance/policies/overview.md)」を参照してください。

![認証ステップをブレーズ](../../assets/catalog/mobile-engagement/braze/authentication.png)

「**[!UICONTROL 宛先を作成]**」をクリックします。 これで宛先が作成されました。後でセグメントをアクティブにする場合は、「**[!UICONTROL 保存して終了]**」をクリックします。または、「**[!UICONTROL 次へ]**」を選択してワークフローを続行し、アクティブ化するセグメントを選択します。 どちらの場合も、残りのワークフローについては、次の「[セグメントのアクティブ化](#activate-segments)」の節を参照してください。

## セグメントのアクティブ化 {#activate-segments}

セグメントのアクティベーションワークフローについて詳しくは、「[宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-destinations.md#select-attributes)」を参照してください。

## フィールドマッピング{#field-mapping}

オーディエンスデータを[!DNL Adobe Experience Platform]から[!DNL Braze]の宛先に正しく送信するには、フィールドマッピングの手順を実行する必要があります。

マッピングは、[!DNL Platform]アカウント内の[!DNL Experience Data Model](XDM)スキーマフィールドと、対応するターゲット宛先からの対応するフィールド間のリンクの作成で構成されます。

XDMフィールドを[!DNL Braze]宛先フィールドに正しくマッピングするには、次の手順に従います。

[!UICONTROL マッピング]の手順で、「**[!UICONTROL 新しいマッピングを追加]**」をクリックします。

![宛先の追加マッピングをブレーズ](../../assets/catalog/mobile-engagement/braze/mapping.png)

「[!UICONTROL ソースフィールド]」セクションで、空のフィールドの横にある矢印ボタンをクリックします。

![宛先ソースマッピングのブレーズ](../../assets/catalog/mobile-engagement/braze/mapping-source.png)

[!UICONTROL ソースフィールドを選択]ウィンドウで、次の2つのカテゴリのXDMフィールドを選択できます。
* [!UICONTROL 属性の選択]:このオプションを使用して、XDMスキーマの特定のフィールドを属性にマッピ [!DNL Braze] ングします。

![宛先マッピングのソース属性をブレーズ](../../assets/catalog/mobile-engagement/braze/mapping-attributes.png)

* [!UICONTROL ID名前空間の選択]:このオプションを使用して、ID名前空間を [!DNL Platform] 名前空間にマッピン [!DNL Braze] グします。

![宛先マッピング元の名前空間を分割](../../assets/catalog/mobile-engagement/braze/mapping-namespaces.png)

ソースフィールドを選択し、「**[!UICONTROL 選択]**」をクリックします。

「[!UICONTROL ターゲットフィールド]」セクションで、フィールドの右側にあるマッピングアイコンをクリックします。

![宛先ターゲットマッピングのブレーズ](../../assets/catalog/mobile-engagement/braze/mapping-target.png)

[!UICONTROL ターゲットフィールドを選択]ウィンドウで、次の3つのカテゴリのターゲットフィールドを選択できます。
* [!UICONTROL 属性の選択]:このオプションを使用して、XDM属性を標準属性にマッピン [!DNL Braze] グします。
* [!UICONTROL ID名前空間の選択]:ID名前空間をID名前空間にマ [!DNL Platform] ッピングするに [!DNL Braze] は、このオプションを使用します。
* [!UICONTROL カスタム属性の選択]:このオプションを使用して、XDM属性を、アカウントで定 [!DNL Braze] 義したカスタム属性にマッピン [!DNL Braze] グします。
* また、このオプションを使用して、既存のXDM属性の名前を[!DNL Braze]に変更することもできます。 例えば、`lastName` XDM属性を[!DNL Braze]のカスタム`Last_Name`属性にマッピングすると、[!DNL Braze]に`Last_Name`属性が作成されます（属性がまだ存在しない場合）。また、`lastName` XDM属性をマッピングします。

![宛先ターゲットマッピングフィールドの分類](../../assets/catalog/mobile-engagement/braze/mapping-target-fields.png)

ターゲットフィールドを選択し、「**[!UICONTROL 選択]**」をクリックします。

これで、フィールドマッピングがリストに表示されます。

![宛先マッピングのブレーズ完了](../../assets/catalog/mobile-engagement/braze/mapping-complete.png)

マッピングをさらに追加するには、上記の手順を繰り返します。

## マッピングの例{#mapping-example}

例えば、XDMプロファイルスキーマと[!DNL Braze]インスタンスに次の属性とIDが含まれているとします。

|  | XDMプロファイルスキーマ | [!DNL Braze] インスタンス |
|---|---|---|
| 属性 | <ul><li>person.name.firstName</code></li><li>person.name.lastName</code></li><li>mobilePhone.number</code></li></ul> | <ul><li>名</code></li><li>姓</code></li><li>電話番号</code></li></ul> |
| ID | <ul><li>メール</code></li><li>Google広告ID(GAID)</code></li><li>広告主(IDFA)</code>向けApple ID</li></ul> | <ul><li>external_id</code></li></ul> |

正しいマッピングは次のようになります。

![宛先マッピングのブレーズの例](../../assets/catalog/mobile-engagement/braze/mapping-example.png)

## エクスポートされたデータ{#exported-data}

データが[!DNL Braze]の宛先に正常に書き出されたかどうかを確認するには、[!DNL Braze]アカウントを確認します。 [!DNL Adobe Experience Platform] セグメントは、属性のにエ [!DNL Braze] クスポート `AdobeExperiencePlatformSegments` されます。

## データの使用とガバナンス{#data-usage-governance}

すべての[!DNL Adobe Experience Platform]宛先は、データを処理する際のデータ使用ポリシーに準拠しています。 [!DNL Adobe Experience Platform]によるデータガバナンスの強制方法について詳しくは、「[データガバナンスの概要](../../../data-governance/home.md)」を参照してください。
