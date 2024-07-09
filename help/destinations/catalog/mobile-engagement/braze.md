---
keywords: モバイル；braze; メッセージング；
title: Braze 接続
description: Braze は、顧客と好きなブランドの間で関連性の高い思い出に残るエクスペリエンスを強化する包括的なカスタマーエンゲージメントプラットフォームです。
exl-id: 508e79ee-7364-4553-b153-c2c00cc85a73
source-git-commit: c35b43654d31f0f112258e577a1bb95e72f0a971
workflow-type: tm+mt
source-wordcount: '1125'
ht-degree: 31%

---

# [!DNL Braze] 接続

## 概要 {#overview}

この [!DNL Braze] 宛先は、プロファイルデータをに送信するのに役立ちます [!DNL Braze].

[!DNL Braze] は、顧客と好きなブランドの間で、関連性の高い思い出に残るエクスペリエンスを強化する包括的なカスタマーエンゲージメントプラットフォームです。

プロファイルデータをに送信する [!DNL Braze]最初に宛先に接続する必要があります。

## 宛先の詳細 {#specifics}

に固有な次の詳細に注意してください [!DNL Braze] 宛先：

* [!DNL Adobe Experience Platform] オーディエンスはに書き出されます。 [!DNL Braze] の下 `AdobeExperiencePlatformSegments` 属性。

>[!NOTE]
>
>追加のカスタム属性をに送信する必要があります [!DNL Braze] の増加の原因となる可能性があります [!DNL Braze] データポイントの使用。 ご相談ください [!DNL Braze] 追加のカスタム属性を送信する前にアカウントマネージャー

## ユースケース {#use-cases}

マーケターとして、オーディエンスが組み込まれたモバイルエンゲージメントの宛先のユーザーをターゲットにしたいと考えています [!DNL Adobe Experience Platform]. さらに、顧客の属性に基づいて、顧客にパーソナライズされたエクスペリエンスを提供したいと考えています [!DNL Adobe Experience Platform] オーディエンスおよびプロファイルがで更新されるとすぐにプロファイルが更新されます。 [!DNL Adobe Experience Platform].

## サポートされる ID {#supported-identities}

[!DNL Braze] では、以下の表で説明する ID のアクティベーションをサポートしています。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| external_id | カスタム [!DNL Braze] 任意の id のマッピングをサポートする識別子。 | 次のいずれかを送信できます [id](../../../identity-service/features/namespaces.md) に [!DNL Braze] 宛先（にマッピングする限り） [!DNL Braze] [`external_id`](https://www.braze.com/docs/api/basics/#external-user-id-explanation). |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platformを通じて生成されたオーディエンス [セグメント化サービス](../../../segmentation/home.md). |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | セグメントのすべてのメンバーを、フィールドマッピングに従って、必要なスキーマフィールド（例：メールアドレス、電話番号、姓）や ID と共に書き出します。[!DNL Adobe Experience Platform] オーディエンスはに書き出されます。 [!DNL Braze] の下 `AdobeExperiencePlatformSegments` 属性。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

* **[!UICONTROL Braze アカウントトークン]**：これはです [!DNL Braze] [!DNL API] キー。 の取得方法について詳しくは、こちらを参照してください [!DNL API] ここでキーを押す： [REST API キーの概要](https://www.braze.com/docs/api/api_key/).

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前を入力します。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明を入力します。
* **[!UICONTROL エンドポイントインスタンス]**：に質問する [!DNL Braze] 使用する必要があるエンドポイントインスタンスを表します

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* エクスポートする *id*、が必要です **[!UICONTROL ID グラフの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![宛先に対してオーディエンスをアクティブ化するために、ワークフローで強調表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png "宛先に対してオーディエンスをアクティブ化するために、ワークフローで強調表示されている ID 名前空間を選択します。"){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティブ化する手順については、[ストリーミングオーディエンス書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

## マッピングに関する考慮事項 {#mapping-considerations}

からオーディエンスデータを正しく送信するには [!DNL Adobe Experience Platform] に [!DNL Braze] 宛先については、フィールドマッピングの手順を実行する必要があります。

マッピングは、リンク間のリンクの作成で構成されます [!DNL Experience Data Model] の（XDM）スキーマフィールド [!DNL Platform] アカウントおよびターゲットの宛先からの対応する同等のもの。

XDM フィールドを [!DNL Braze] 宛先フィールドに正しくマッピングするには、次の手順に従います。

が含まれる [!UICONTROL マッピング] ステップ、クリック **[!UICONTROL 新しいマッピングを追加]**.

![Braze 宛先追加マッピング](../../assets/catalog/mobile-engagement/braze/mapping.png)

が含まれる [!UICONTROL Source フィールド] セクションで、空のフィールドの横にある矢印ボタンをクリックします。

![Braze の宛先Sourceのマッピング](../../assets/catalog/mobile-engagement/braze/mapping-source.png)

が含まれる [!UICONTROL ソースフィールドを選択] ウィンドウで、次の 2 つのカテゴリの XDM フィールドから選択できます。
* [!UICONTROL 属性を選択]：このオプションを使用して、XDM スキーマから特定のフィールドをにマッピングします [!DNL Braze] 属性。

![Braze 宛先マッピングSource属性](../../assets/catalog/mobile-engagement/braze/mapping-attributes.png)

* [!UICONTROL ID 名前空間を選択]：このオプションを使用してをマッピングします [!DNL Platform] への ID 名前空間 [!DNL Braze] 名前空間。

![Braze 宛先マッピング Source名前空間](../../assets/catalog/mobile-engagement/braze/mapping-namespaces.png)

ソースフィールドを選択し、 **[!UICONTROL を選択]**.

が含まれる [!UICONTROL ターゲットフィールド] セクションで、フィールドの右側にあるマッピングアイコンをクリックします。

![Braze の宛先ターゲットマッピング](../../assets/catalog/mobile-engagement/braze/mapping-target.png)

が含まれる [!UICONTROL ターゲットフィールドを選択] ウィンドウで、次の 2 つのカテゴリのターゲットフィールドから選択できます。
* [!UICONTROL ID 名前空間を選択]：このオプションを使用してマッピングします [!DNL Platform] id 名前空間を次へ [!DNL Braze] id 名前空間。
* [!UICONTROL カスタム属性を選択]：このオプションを使用して、XDM 属性をカスタムにマッピングします [!DNL Braze] で定義した属性 [!DNL Braze] アカウント。 <br> また、このオプションを使用して、既存の XDM 属性の名前をに変更することもできます [!DNL Braze]. 例えば、 `lastName` カスタムへの XDM 属性 `Last_Name` 属性： [!DNL Braze]。が作成されます `Last_Name` 属性： [!DNL Braze]を選択します（存在しない場合）。 `lastName` これに対する XDM 属性。

![宛先ターゲットマッピングフィールドを分析](../../assets/catalog/mobile-engagement/braze/mapping-target-fields.png)

ターゲットフィールドを選択し、 **[!UICONTROL を選択]**.

これで、リストにフィールドマッピングが表示されます。

![Braze の宛先マッピングの完了](../../assets/catalog/mobile-engagement/braze/mapping-complete.png)

さらにマッピングを追加するには、前の手順を繰り返します。

## マッピングの例 {#mapping-example}

XDM プロファイルスキーマと [!DNL Braze] インスタンスには、次の属性と ID が含まれます。

|  | XDM プロファイルスキーマ | [!DNL Braze] Instance |
|---|---|---|
| 属性 | <ul><li><code>person.name.firstName</code></li><li><code>person.name.lastName</code></li><li><code>mobilePhone.number</code></li></ul> | <ul><li><code>FirstName</code></li><li><code>LastName</code></li><li><code>電話番号</code></li></ul> |
| ID | <ul><li><code>電子メール</code></li><li><code>Google広告 ID （GAID）</code></li><li><code>広告主のApple ID （IDFA）</code></li></ul> | <ul><li><code>external_id</code></li></ul> |

正しいマッピングは次のようになります。

![Braze の宛先マッピングの例](../../assets/catalog/mobile-engagement/braze/mapping-example.png)

## 書き出したデータ {#exported-data}

データがに正常に [!DNL Braze] の宛先に書き出されたかどうかを確認するには、[!DNL Braze] アカウントを確認します。 [!DNL Adobe Experience Platform] オーディエンスはに書き出されます。 [!DNL Braze] の下 `AdobeExperiencePlatformSegments` 属性。

## トラブルシューティング {#troubleshooting}

**この宛先に対してオーディエンスをアクティブ化する際に、タイムアウトエラーが発生しました。 どうすればよいですか？**

この宛先に対する Audience Activation によって、タイムアウトエラーが発生する場合があります。 このエラーは、アクティベーションの問題を示しているわけではありません。

タイムアウトエラーが発生した場合は、宛先プラットフォームでオーディエンスサイズを確認します。 オーディエンスサイズが正しい場合、統合は期待どおりに動作しています。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。方法について詳しくは、 [!DNL Adobe Experience Platform] データガバナンスを実施します。を参照してください。 [データガバナンスの概要](../../../data-governance/home.md).
