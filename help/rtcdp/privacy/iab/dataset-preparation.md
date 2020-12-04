---
keywords: Experience Platform;home;IAB;IAB 2.0;consent;Consent
solution: Experience Platform
title: リアルタイム顧客データプラットフォームでのIAB TCF 2.0のサポート
topic: privacy events
description: このドキュメントでは、IAB TCF 2.0の同意データを収集するために必要な2つのデータセットを設定する手順を説明します。
translation-type: tm+mt
source-git-commit: fa667d86c089c692f22cfd1b46f3f11b6e9a68d7
workflow-type: tm+mt
source-wordcount: '1369'
ht-degree: 3%

---


# IAB TCF 2.0の同意データを取り込むためのデータセットの作成

IAB [!DNL Real-time Customer Data Platform][!DNL Transparency & Consent Framework] (TCF)2.0に従って顧客の同意データを処理するには、そのデータを、TCF 2.0の同意フィールドを含むスキーマのデータセットに送信する必要があります。

特に、TCF 2.0の同意データを取り込むには、次の2つのデータセットが必要です。

* クラスに基づくデータセット。で使用可能になっ [!DNL XDM Individual Profile] てい [!DNL Real-time Customer Profile]ます。
* クラスに基づくデータセット [!DNL XDM ExperienceEvent] 。

このドキュメントでは、IAB TCF 2.0の同意データを収集するために、これら2つのデータセットを設定する手順を説明します。 TCF 2.0を設定するための完全なワークフローの概要につ [!DNL Real-time CDP] いては、 [IAB TCF 2.0コンプライアンスの概要を参照してください](./overview.md)。

## 前提条件

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [エクスペリエンスデータモデル（XDM）](../../../xdm/home.md)[!DNL Experience Platform]： が顧客エクスペリエンスデータを整理する際に使用する標準化されたフレームワーク。
   * [Basics of schema composition](../../../xdm/schema/composition.md)：XDM スキーマの基本的な構成要素について説明しています。
   * [UIでのスキーマの作成](../../../xdm/tutorials/create-schema-ui.md):スキーマエディタの基本操作に関するチュートリアルです。
* [Adobe Experience Platform・アイデンティティ・サービス](../../../identity-service/home.md):デバイスやシステム間で異なるデータソースの顧客IDをブリッジできます。
* [リアルタイム顧客プロファイル](../../../profile/home.md):を使用 [!DNL Identity Service] すると、データセットから詳細な顧客プロファイルをリアルタイムで作成できます。 [!DNL Real-time Customer Profile] data Lakeからデータを取り込み、顧客プロファイルを個別のデータストアに維持します。

## 同意スキーマ構造 {#structure}

TCF 2.0のサポートに必要な顧客の同意フィールドを提供する2つのXDMミックスインがあります。レコードベースのデータ([!DNL XDM Individual Profile])用と時系列ベースのデータ([!DNL XDM ExperienceEvent])用に1つずつ、

| スキーマ | 説明 |
| --- | --- |
| プロファイルプライバシーミックスイン | このミックスインは、顧客の現在の同意の環境設定をキャプチャします。 有効なスキーマで使用する場合、このMixinで提供される値は、顧客のデータに対する同意の実施の適用方法に関する真実の源泉と見なされます。 [!DNL Profile] |
| [!DNL Experience Event] プライバシーミックスン | このミックスインは、特定の時点における顧客の同意の好みをキャプチャします。 これらのフィールドに取り込まれたデータは、時間の経過に伴う顧客の同意の好みの変更を追跡するために使用できます。 |

各ミックスインの使用例は異なりますが、ミックスインで提供される特定のフィールドはほぼ同じです。 これらのフィールドについては、次の節で詳しく説明します。

### 同意ミックスインフィールド {#privacy-mixin}

各プライバシーミックスインの構造やフィールドの種類は異なりますが、共に属性を提供します。この属性は、TCF 2.0の強制を行うために必要なサブフィールドです。 `xdm:consentString` これらのフィールドの構造と、期待される値のタイプを以下に示します。

```json
{
  "xdm:consentString": {
    "xdm:consentStandard": "IAB TCF",
    "xdm:consentStandardVersion": "2.0",
    "xdm:consentStringValue": "BObdrPUOevsguAfDqFENCNAAAAAmeAAA.PVAfDObdrA.DqFENCAmeAENCDA",
    "xdm:gdprApplies": true,
    "xdm:containsPersonalData": false
  }
}
```

| プロパティ | 説明 |
| --- | --- |
| `xdm:consentString` | 顧客の更新された同意データおよびその他のコンテキスト情報が含まれます。 |
| `xdm:consentStandard` | データが適用される同意フレームワーク。 TCF準拠の場合、値は「IAB TCF」にする必要があります。 |
| `xdm:consentStandardVersion` | に示す同意フレームワークのバージョン番号 `xdm:consentStandard`です。 TCF 2.0に準拠する場合、値は「2.0」にする必要があります。 |
| `xdm:consentStringValue` | 顧客が選択した同意設定に基づいて生成された同意文字列。 |
| `xdm:gdprApplies` | GDPRがこの顧客に適用されるかどうかを示すboolean値。 TCF 2.0が強制されるためには、値を「true」に設定する必要があります。 含めない場合のデフォルトは「false」です。 |
| `xdm:containsPersonalData` | 同意の更新に個人データが含まれているかどうかを示すboolean値です。 含めない場合のデフォルトは「false」です。 |

## 顧客の同意スキーマの作成 {#create-schemas}

プラットフォームUIで、左側のナビゲーションの **[!UICONTROL スキーマ]** をクリックし、 **[!UICONTROL スキーマ]** ワークスペースを開きます。 次のセクションの手順に従って、必要な各スキーマを作成します。

>[!NOTE]
>
>同意データを取り込むために使用する既存のXDMスキーマがある場合は、新しいスキーマを作成する代わりに、それらのデータを編集できます。 ただし、既存のスキーマを編集する場合は、変更を破るのを避けるために、スキーマの進化の [原則に従うこと](../../../xdm/schema/composition.md#evolution) が重要です。

### レコードベースの同意スキーマの作成 {#profile-schema}

**[!UICONTROL スキーマ]** ワークスペースの「 **[!UICONTROL 参照] 」タブで、**[!DNL XDM Individual Profile] クラスに基づいて新しいスキーマを作成します。 スキーマエディタ内でスキーマを開いたら、キャンバスの左側にある **[!UICONTROL Mixins]****[!UICONTROL (]** ミックスイン)セクションの下のをクリックします。

![](../assets/iab/add-mixin-profile.png)

**[!UICONTROL Mixin]** ダイアログが表示されます。 ここから、リストから **[!UICONTROL プロファイルのプライバシー]** を選択します。 オプションで、検索バーを使用して結果を絞り込み、Mixinを見つけやすくすることができます。 ミックスインを選択したら、 **[!UICONTROL 追加「ミックスイン]**」をクリックします。

![](../assets/iab/add-profile-privacy.png)

[スキーマエディタ]キャンバスが再表示され、追加した同意文字列フィールドの構造を確認できます。

![](../assets/iab/profile-privacy-structure.png)

ここから上記の手順を繰り返し、以下のミックスインをスキーマに追加します。

* [!UICONTROL IdentityMap]
* [!UICONTROL プロファイル用のデータ取得領域]
* [!UICONTROL プロファイル担当者の詳細]
* [!UICONTROL プロファイルの個人情報]

![](../assets/iab/profile-all-mixins.png)

での使用が既に有効になっている既存のスキーマを編集する場合は、「 [!DNL Real-time Customer Profile]保存 **[!UICONTROL 」をクリックして変更を確認した後で、同意スキーマに基づくデータセット]** の [作成に関する節に進みます](#dataset)。 新しいスキーマを作成する場合は、次のサブセクションで説明されている手順に従ってください。

#### スキーマを [!DNL Real-time Customer Profile]

受け取った同意データを特定の顧客プロファイル [!DNL Real-time CDP] に関連付けるには、その同意スキーマをで使用できるようにする必要があり [!DNL Real-time Customer Profile]ます。

>[!NOTE]
>
>この節に示す例のスキーマでは、その `identityMap` フィールドを主IDとして使用しています。 別のフィールドを主IDとして設定する場合は、cookie IDなどの間接識別子を使用し、電子メールアドレスなどの利子ベースの広告で使用するのを禁止する直接識別子を使用しないようにしてください。 制限されているフィールドが不明な場合は、弁護士にお問い合わせください。
>
>スキーマのプライマリIDフィールドを設定する手順については、 [スキーマ作成のチュートリアルを参照してください](../../../xdm/tutorials/create-schema-ui.md#identity-field)。

スキーマを有効にするに [!DNL Profile]は、左側のパネルでスキーマの名前をクリックし、右側のパネルで **[!UICONTROL スキーマのプロパティ]** ダイアログを開きます。 ここから、 **[!UICONTROL プロファイル]** 切り替えボタンをクリックします。

![](../assets/iab/profile-enable-profile.png)

プライマリIDが見つからないことを示すポーバーが表示されます。 プライマリIDがidentityMapフィールドに含まれるので、代替のプライマリIDを使用するチェックボックスを選択します。

<img src="../assets/iab/missing-primary-identity.png" width="600" /><br>

最後に、「 **[!UICONTROL 保存]** 」をクリックして変更を確認します。

![](../assets/iab/profile-save.png)

### 時系列ベースの同意スキーマの作成 {#event-schema}

**[!UICONTROL スキーマ]** ワークスペースの「 **[!UICONTROL 参照]** 」タブで、 [!DNL XDM ExperienceEvent] クラスに基づいて新しいスキーマを作成します。 スキーマエディタ内でスキーマを開いたら、キャンバスの左側にある **[!UICONTROL Mixins]****[!UICONTROL (]** ミックスイン)セクションの下のをクリックします。

![](../assets/iab/add-mixin-event.png)

**[!UICONTROL Mixin]** ダイアログが表示されます。 ここから、リストから **[!UICONTROL エクスペリエンスイベントのプライバシーミックスイン]** を選択します。 オプションで、検索バーを使用して結果を絞り込み、Mixinを見つけやすくすることができます。 ミックスインを選択したら、 **[!UICONTROL 追加「ミックスイン]**」をクリックします。

![](../assets/iab/add-event-privacy.png)

[スキーマエディタ]キャンバスが再表示され、追加された同意文字列フィールドが表示されます。

![](../assets/iab/event-privacy-structure.png)

ここから上記の手順を繰り返し、以下のミックスインをスキーマに追加します。

* [!UICONTROL IdentityMap]
* [!UICONTROL ExperienceEvent環境の詳細]
* [!UICONTROL ExperienceEvent Webの詳細]
* [!UICONTROL ExperienceEventの導入の詳細]

ミックスインを追加したら、「 **[!UICONTROL 保存]**」をクリックして終了します。

![](../assets/iab/event-all-mixins.png)

## 同意スキーマに基づいてデータセットを作成 {#datasets}

前述の必須スキーマごとに、顧客の同意データを最終的に取り込むデータセットを作成する必要があります。 スキーマに基づくデータセットはに対して有効にする必要がありますが、 [!DNL XDM Individual Profile] スキーマに基づくデータセットは [!DNL Real-time Customer Profile][!DNL XDM ExperienceEvent][!DNL Profile]有効にしないでください。

まず、左のナビゲーションで **[!UICONTROL Datasets]** (データセット **[!UICONTROL )を選択し、右上隅の「データセットを]** 作成」をクリックします。

![](../assets/iab/dataset-create.png)

On the next page, select **[!UICONTROL Create dataset from schema]**.

![](../assets/iab/dataset-create-from-schema.png)

「 **[!UICONTROL スキーマからデータセットを]** 作成 **[!UICONTROL 」ワークフローが表示されます。このワークフローは、]** スキーマを選択手順から開始します。 提供されたリストで、前に作成した同意スキーマの1つを探します。 オプションで検索を使用して結果を絞り込み、スキーマを見つけやすくできます。 スキーマの横にあるラジオボタンをクリックして選択し、「 **[!UICONTROL 次へ]** 」をクリックして続行します。

![](../assets/iab/dataset-select-schema.png)

**[!UICONTROL データセットの設定]**&#x200B;手順が表示されます。「 **[!UICONTROL 完了」をクリックする前に、データセットの一意でわかりやすい名前と説明を入力します]**。

![](../assets/iab/dataset-configure.png)

新しく作成したデータセットの詳細ページが表示されます。 データセットが [!DNL XDM ExperienceEvent] スキーマに基づいている場合は、プロセスが完了します。 データセットが [!DNL XDM Individual Profile] スキーマに基づいている場合は、プロセスの最後のステップとして、データセットを有効にしてで使用できるようにし [!DNL Real-time Customer Profile]ます。 右側のパネルで、 **[!UICONTROL プロファイル]** /切り替えボタンをクリックして、データセットを有効にします。

![](../assets/iab/dataset-enable-profile.png)

上記の手順を再度実行し、TCF 2.0準拠に必要なその他のデータセットを作成します。

## 次の手順

このチュートリアルに従うと、顧客の同意データを収集するために使用できる2つのデータセットが作成されます。

* ご使用の [!DNL Profile]スキーマに基づ [!DNL XDM Individual Profile] いて有効になったデータセット。
* 有効になっていない [!DNL XDM ExperienceEvent] スキーマに基づくデータセット [!DNL Profile]。

これで、 [IAB TCF 2.0の概要に戻って](./overview.md#merge-policies) 、TCF 2.0準拠の設定プロセス [!DNL Real-time CDP] を続行できます。