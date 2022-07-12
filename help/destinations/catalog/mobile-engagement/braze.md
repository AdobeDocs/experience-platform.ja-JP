---
keywords: モバイル；勇気づけメッセージ；
title: Braze 接続
description: Braze は、顧客と顧客が好むブランドとの間の関連性の高い思い出に残るエクスペリエンスを強化する、包括的な顧客エンゲージメントプラットフォームです。
exl-id: 508e79ee-7364-4553-b153-c2c00cc85a73
source-git-commit: fd2019feb25b540612a278cbea5bf5efafe284dc
workflow-type: tm+mt
source-wordcount: '1001'
ht-degree: 8%

---

# [!DNL Braze] 接続

## 概要 {#overview}

この [!DNL Braze] の宛先は、次にプロファイルデータを送信する際に役立ちます： [!DNL Braze].

[!DNL Braze] は、顧客と顧客が好むブランドとの間の関連性の高い思い出に残るエクスペリエンスを強化する、包括的な顧客エンゲージメントプラットフォームです。

プロファイルデータをに送信するには [!DNL Braze]の場合、最初に宛先に接続する必要があります。

## 宛先の詳細 {#specifics}

次に示す、 [!DNL Braze] 宛先：

* [!DNL Adobe Experience Platform] セグメントは次の場所に書き出されます： [!DNL Braze] の下に `AdobeExperiencePlatformSegments` 属性。

>[!NOTE]
>
>追加のカスタム属性をに送信する際には、 [!DNL Braze] が [!DNL Braze] データポイントの使用。 詳しくは、 [!DNL Braze] 追加のカスタム属性を送信する前に、アカウントマネージャーに問い合わせてください。

## ユースケース {#use-cases}

マーケターが、セグメントを組み込んで、モバイルエンゲージメントの宛先でユーザーをターゲットにしたい [!DNL Adobe Experience Platform]. さらに、訪問者の属性に基づいて、パーソナライズされたエクスペリエンスをユーザーに提供したいと考えています [!DNL Adobe Experience Platform] プロファイル：セグメントとプロファイルが [!DNL Adobe Experience Platform].

## サポートされる ID {#supported-identities}

[!DNL Braze] では、以下の表で説明する id のアクティブ化をサポートしています。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| external_id | カスタム [!DNL Braze] 任意の id のマッピングをサポートする識別子。 | 任意の [id](../../../identity-service/namespaces.md) から [!DNL Braze] 宛先にマッピングする場合は、 [!DNL Braze] [`external_id`](https://www.braze.com/docs/api/basics/#external-user-id-explanation). |

{style=&quot;table-layout:auto&quot;}

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | セグメントのすべてのメンバーを、目的のスキーマフィールド ( 例：フィールドマッピングに従った電子メールアドレス、電話番号、姓 ) や ID。[!DNL Adobe Experience Platform] セグメントは次の場所に書き出されます： [!DNL Braze] の下に `AdobeExperiencePlatformSegments` 属性。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は、API ベースの接続です。 セグメント評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 詳細を表示 [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations). |

{style=&quot;table-layout:auto&quot;}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションに記載されているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先を認証するには、必須フィールドに入力し、「 」を選択します。 **[!UICONTROL 宛先に接続]**.

* **[!UICONTROL アカウントトークンをブレーズ]**:これがあなたの [!DNL Braze] [!DNL API] キー。 詳細な手順については、 [!DNL API] キー： [REST API キーの概要](https://www.braze.com/docs/api/api_key/).

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。 UI でフィールドの横にアスタリスクが表示される場合は、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**:この宛先が将来認識される名前を入力します。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明を入力します。
* **[!UICONTROL エンドポイントインスタンス]**:に尋ねる [!DNL Braze] 使用する必要のあるエンドポイントインスタンスを表します。

### アラートの有効化 {#enable-alerts}

アラートを有効にして、宛先へのデータフローのステータスに関する通知を受け取ることができます。 リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートの詳細については、 [UI を使用した宛先アラートの購読](../../ui/alerts.md).

宛先接続の詳細の指定が完了したら、 **[!UICONTROL 次へ]**.

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

詳しくは、 [ストリーミングセグメントの書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md) を参照してください。

## マッピングに関する考慮事項 {#mapping-considerations}

オーディエンスデータを次の場所から正しく送信するには： [!DNL Adobe Experience Platform] から [!DNL Braze] の宛先に移動する場合は、フィールドマッピングの手順を実行する必要があります。

マッピングは、 [!DNL Experience Data Model] (XDM) スキーマフィールド ( [!DNL Platform] アカウントおよびターゲット宛先からの対応する同等物。

XDM フィールドを [!DNL Braze] 宛先フィールドは、次の手順に従います。

内 [!UICONTROL マッピング] ステップ、クリック **[!UICONTROL 新しいマッピングを追加]**.

![宛先の追加マッピングをブレーズ](../../assets/catalog/mobile-engagement/braze/mapping.png)

内 [!UICONTROL ソースフィールド] 「 」セクションで、空のフィールドの横にある矢印ボタンをクリックします。

![宛先ソースマッピングを分割](../../assets/catalog/mobile-engagement/braze/mapping-source.png)

内 [!UICONTROL ソースフィールドを選択] ウィンドウで、次の 2 つのカテゴリの XDM フィールドを選択できます。
* [!UICONTROL 属性を選択]:このオプションを使用して、XDM スキーマの特定のフィールドを [!DNL Braze] 属性。

![宛先マッピングソース属性をブレーズ](../../assets/catalog/mobile-engagement/braze/mapping-attributes.png)

* [!UICONTROL ID 名前空間を選択]:このオプションを使用して、 [!DNL Platform] ID 名前空間を [!DNL Braze] 名前空間。

![宛先マッピングソース名前空間を分割](../../assets/catalog/mobile-engagement/braze/mapping-namespaces.png)

ソースフィールドを選択し、「 **[!UICONTROL 選択]**.

内 [!UICONTROL ターゲットフィールド] 「 」セクションで、「 」フィールドの右側にあるマッピングアイコンをクリックします。

![宛先ターゲットマッピングを分解](../../assets/catalog/mobile-engagement/braze/mapping-target.png)

内 [!UICONTROL ターゲットフィールドを選択] ウィンドウで、次の 2 つのカテゴリのターゲットフィールドを選択できます。
* [!UICONTROL ID 名前空間を選択]:このオプションを使用して、 [!DNL Platform] ID 名前空間への変換 [!DNL Braze] ID 名前空間。
* [!UICONTROL カスタム属性を選択]:XDM 属性をカスタムにマッピングするには、このオプションを使用します。 [!DNL Braze] 属性 [!DNL Braze] アカウント <br> また、このオプションを使用して、既存の XDM 属性の名前を [!DNL Braze]. 例えば、 `lastName` カスタムの XDM 属性 `Last_Name` 属性 [!DNL Braze]を作成し、 `Last_Name` 属性 [!DNL Braze]（まだ存在しない場合）、 `lastName` XDM 属性を含めています。

![宛先ターゲットマッピングフィールドを分割](../../assets/catalog/mobile-engagement/braze/mapping-target-fields.png)

ターゲットフィールドを選択し、「 **[!UICONTROL 選択]**.

これで、リストにフィールドマッピングが表示されます。

![宛先マッピングのブレーズの完了](../../assets/catalog/mobile-engagement/braze/mapping-complete.png)

さらにマッピングを追加するには、上記の手順を繰り返します。

## マッピングの例 {#mapping-example}

XDM プロファイルスキーマと [!DNL Braze] インスタンスには、次の属性と ID が含まれます。

|  | XDM プロファイルスキーマ | [!DNL Braze] インスタンス |
|---|---|---|
| 属性 | <ul><li>person.name.firstName</code></li><li>person.name.lastName</code></li><li>mobilePhone.number</code></li></ul> | <ul><li>名</code></li><li>姓</code></li><li>電話番号</code></li></ul> |
| ID | <ul><li>メール</code></li><li>Google Ad ID (GAID)</code></li><li>Apple Id For Advertisers(IDFA)</code></li></ul> | <ul><li>external_id</code></li></ul> |

正しいマッピングは次のようになります。

![宛先マッピングのブレーズの例](../../assets/catalog/mobile-engagement/braze/mapping-example.png)

## 書き出したデータ {#exported-data}

データがに正常に書き出されたかどうかを確認するには、以下を実行します。 [!DNL Braze] 宛先、 [!DNL Braze] アカウント [!DNL Adobe Experience Platform] セグメントは次の場所に書き出されます： [!DNL Braze] の下に `AdobeExperiencePlatformSegments` 属性。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。詳しくは、 [!DNL Adobe Experience Platform] では、データガバナンスを強制します。詳しくは、 [データガバナンスの概要](../../../data-governance/home.md).
