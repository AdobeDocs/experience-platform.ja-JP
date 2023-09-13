---
keywords: モバイル；ブレーズ；メッセージ；
title: Braze 接続
description: Braze は、顧客と顧客が好むブランドとの間の関連性の高い思い出に残るエクスペリエンスを強化する、包括的な顧客エンゲージメントプラットフォームです。
exl-id: 508e79ee-7364-4553-b153-c2c00cc85a73
source-git-commit: e2317201ae4810734714cea6c5d172ea6a542f5b
workflow-type: tm+mt
source-wordcount: '1096'
ht-degree: 35%

---

# [!DNL Braze] 接続

## 概要 {#overview}

The [!DNL Braze] の宛先は、次にプロファイルデータを送信する際に役立ちます： [!DNL Braze].

[!DNL Braze] は、顧客と顧客が好むブランドとの間の関連性の高い思い出に残るエクスペリエンスを強化する、包括的な顧客エンゲージメントプラットフォームです。

プロファイルデータをに送信するには [!DNL Braze]の場合、最初に宛先に接続する必要があります。

## 宛先の詳細 {#specifics}

次に示す詳細は、 [!DNL Braze] 宛先：

* [!DNL Adobe Experience Platform] オーディエンスは次の場所に書き出されます： [!DNL Braze] の下に `AdobeExperiencePlatformSegments` 属性。

>[!NOTE]
>
>追加のカスタム属性をに送信する際には、次のことに注意してください。 [!DNL Braze] は、 [!DNL Braze] データポイントの使用。 詳しくは、 [!DNL Braze] 追加のカスタム属性を送信する前に、アカウントマネージャーに問い合わせてください。

## ユースケース {#use-cases}

マーケターが、オーディエンスの組み込みにより、モバイルエンゲージメントの宛先でユーザーをターゲットにしたい [!DNL Adobe Experience Platform]. さらに、訪問者の属性に基づいて、パーソナライズされたエクスペリエンスをユーザーに提供したいと考えています [!DNL Adobe Experience Platform] プロファイル：オーディエンスとプロファイルが [!DNL Adobe Experience Platform].

## サポートされる ID {#supported-identities}

[!DNL Braze] では、以下の表で説明する ID のアクティベーションをサポートしています。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| external_id | カスタム [!DNL Braze] 任意の id のマッピングをサポートする識別子。 | 任意の [id](../../../identity-service/namespaces.md) から [!DNL Braze] 宛先にマッピングする場合は、 [!DNL Braze] [`external_id`](https://www.braze.com/docs/api/basics/#external-user-id-explanation). |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの起源 | サポートあり | 説明 |
---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [セグメント化サービス](../../../segmentation/home.md). |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/overview.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | セグメントのすべてのメンバーを、フィールドマッピングに従って、目的のスキーマフィールド（電子メールアドレス、電話番号、姓）や ID と共に書き出します。[!DNL Adobe Experience Platform] オーディエンスは次の場所に書き出されます： [!DNL Braze] の下に `AdobeExperiencePlatformSegments` 属性。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

* **[!UICONTROL アカウントトークンをブレーズ]**：これは、 [!DNL Braze] [!DNL API] キー。 詳細な手順については、 [!DNL API] キー： [REST API キーの概要](https://www.braze.com/docs/api/api_key/).

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：この宛先が将来認識される名前を入力します。
* **[!UICONTROL 説明]**：この宛先を将来識別するのに役立つ説明を入力します。
* **[!UICONTROL エンドポイントインスタンス]**：に質問します。 [!DNL Braze] 使用する必要のあるエンドポイントインスタンスを表します。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先にオーディエンスをアクティブ化する手順については、[ストリーミングオーディエンス書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

## マッピングに関する考慮事項 {#mapping-considerations}

オーディエンスデータを次の場所から正しく送信するには： [!DNL Adobe Experience Platform] から [!DNL Braze] の宛先に移動する場合は、フィールドマッピングの手順を実行する必要があります。

マッピングは、 [!DNL Experience Data Model] (XDM) スキーマフィールド ( [!DNL Platform] アカウントおよびターゲット宛先からの対応する同等物。

XDM フィールドを [!DNL Braze] 宛先フィールドに正しくマッピングするには、次の手順に従います。

Adobe Analytics の [!UICONTROL マッピング] ステップ、クリック **[!UICONTROL 新しいマッピングを追加]**.

![宛先の追加マッピングをブレーズ](../../assets/catalog/mobile-engagement/braze/mapping.png)

Adobe Analytics の [!UICONTROL ソースフィールド] 「 」セクションで、空のフィールドの横にある矢印ボタンをクリックします。

![宛先ソースマッピングを分割](../../assets/catalog/mobile-engagement/braze/mapping-source.png)

Adobe Analytics の [!UICONTROL ソースフィールドを選択] ウィンドウで、次の 2 つのカテゴリの XDM フィールドを選択できます。
* [!UICONTROL 属性を選択]：このオプションを使用して、XDM スキーマの特定のフィールドを [!DNL Braze] 属性。

![宛先マッピングのソース属性をブレーズ](../../assets/catalog/mobile-engagement/braze/mapping-attributes.png)

* [!UICONTROL ID 名前空間を選択]：このオプションを使用して、 [!DNL Platform] ID 名前空間を [!DNL Braze] 名前空間。

![宛先マッピングソース名前空間を分割](../../assets/catalog/mobile-engagement/braze/mapping-namespaces.png)

ソースフィールドを選択し、「 **[!UICONTROL 選択]**.

Adobe Analytics の [!UICONTROL ターゲットフィールド] 「 」セクションで、「 」フィールドの右側にあるマッピングアイコンをクリックします。

![宛先ターゲットマッピングを分解](../../assets/catalog/mobile-engagement/braze/mapping-target.png)

Adobe Analytics の [!UICONTROL ターゲットフィールドを選択] ウィンドウで、次の 2 つのカテゴリのターゲットフィールドを選択できます。
* [!UICONTROL ID 名前空間を選択]：このオプションを使用して、マッピングします。 [!DNL Platform] ID 名前空間への変換 [!DNL Braze] ID 名前空間。
* [!UICONTROL カスタム属性を選択]：このオプションを使用して、XDM 属性をカスタムにマッピングします [!DNL Braze] 属性 [!DNL Braze] アカウント。 <br> また、このオプションを使用して、既存の XDM 属性の名前を [!DNL Braze]. 例えば、 `lastName` カスタムの XDM 属性 `Last_Name` 属性 [!DNL Braze]、を作成します。 `Last_Name` 属性 [!DNL Braze]（まだ存在しない場合）、および `lastName` XDM 属性を含めています。

![宛先ターゲットマッピングフィールドを分割](../../assets/catalog/mobile-engagement/braze/mapping-target-fields.png)

ターゲットフィールドを選択し、「 **[!UICONTROL 選択]**.

これで、リストにフィールドマッピングが表示されます。

![宛先マッピングのブレーズの完了](../../assets/catalog/mobile-engagement/braze/mapping-complete.png)

さらにマッピングを追加するには、上記の手順を繰り返します。

## マッピングの例 {#mapping-example}

XDM プロファイルスキーマと [!DNL Braze] インスタンスには、次の属性と ID が含まれます。

|  | XDM プロファイルスキーマ | [!DNL Braze] インスタンス |
|---|---|---|
| 属性 | <ul><li><code>person.name.firstName</code></li><li><code>person.name.lastName</code></li><li><code>mobilePhone.number</code></li></ul> | <ul><li><code>FirstName</code></li><li><code>LastName</code></li><li><code>電話番号</code></li></ul> |
| ID | <ul><li><code>メール</code></li><li><code>Google Ad ID (GAID)</code></li><li><code>Apple Id For Advertisers(IDFA)</code></li></ul> | <ul><li><code>external_id</code></li></ul> |

正しいマッピングは次のようになります。

![宛先マッピングのブレーズの例](../../assets/catalog/mobile-engagement/braze/mapping-example.png)

## 書き出したデータ {#exported-data}

データがに正常に [!DNL Braze] の宛先に書き出されたかどうかを確認するには、[!DNL Braze] アカウントを確認します。 [!DNL Adobe Experience Platform] オーディエンスは次の場所に書き出されます： [!DNL Braze] の下に `AdobeExperiencePlatformSegments` 属性。

## トラブルシューティング {#troubleshooting}

**この宛先に対するオーディエンスをアクティブ化中にタイムアウトエラーが発生しました。 どうすればよいですか？**

この宛先に対するオーディエンスのアクティベーションにより、タイムアウトエラーが発生する場合があります。 このエラーは、アクティベーションの問題を示していませんでした。

タイムアウトエラーが発生した場合は、宛先プラットフォームでオーディエンスのサイズを確認します。 オーディエンスのサイズが正しい場合は、統合が期待どおりに動作します。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。詳しくは、 [!DNL Adobe Experience Platform] では、データガバナンスを強制します。詳しくは、 [データガバナンスの概要](../../../data-governance/home.md).
