---
keywords: Experience Platform；ホーム；IAB;IAB 2.0；同意；同意
solution: Experience Platform
title: IAB TCF 2.0同意データを取り込むためのデータセットの作成
topic-legacy: privacy events
description: このドキュメントでは、IAB TCF 2.0の同意データを収集するために必要な2つのデータセットを設定する手順を説明します。
exl-id: 36b2924d-7893-4c55-bc33-2c0234f1120e
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1646'
ht-degree: 3%

---

# IAB TCF 2.0の同意データを取り込むためのデータセットの作成

Adobe Experience PlatformがIAB [!DNL Transparency & Consent Framework] (TCF) 2.0に従って顧客の同意データを処理するには、そのデータをTCF 2.0の同意フィールドを含むスキーマのデータセットに送信する必要があります。

特に、TCF 2.0の同意データを取り込むには、次の2つのデータセットが必要です。

* [!DNL XDM Individual Profile]クラスに基づくデータセット。[!DNL Real-time Customer Profile]での使用が有効になっています。
* [!DNL XDM ExperienceEvent]クラスに基づくデータセット。

このドキュメントでは、IAB TCF 2.0の同意データを収集するために、これら2つのデータセットを設定する手順を説明します。 TCF 2.0用のプラットフォームデータ操作を設定するための完全なワークフローの概要については、[IAB TCF 2.0コンプライアンスの概要](./overview.md)を参照してください。

## 前提条件

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [エクスペリエンスデータモデル（XDM）](../../../../xdm/home.md)[!DNL Experience Platform]： が顧客エクスペリエンスデータを整理する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../xdm/schema/composition.md)：XDM スキーマの基本的な構成要素について説明します。
* [Adobe Experience Platform・アイデンティティ・サービス](../../../../identity-service/home.md):デバイスやシステム間で異なるデータソースの顧客IDをブリッジできます。
   * [ID名前空間](../../../../identity-service/namespaces.md):顧客IDデータは、IDサービスで認識される特定のID名前空間で提供する必要があります。
* [リアルタイム顧客プロファイル](../../../../profile/home.md):を使用 [!DNL Identity Service] すると、データセットから詳細な顧客プロファイルをリアルタイムで作成できます。[!DNL Real-time Customer Profile] data Lakeからデータを取り込み、顧客プロファイルを個別のデータストアに維持します。

## [!UICONTROL プライバシー] の詳細Mixinの構造  {#structure}

[!UICONTROL プライバシーの詳細]ミックスインは、TCF 2.0のサポートに必要な顧客の同意フィールドを提供します。 このミックスインには2つのバージョンがあります。1つは[!DNL XDM Individual Profile]クラスと互換性があり、もう1つは[!DNL XDM ExperienceEvent]クラスと互換性があります。

以下の節では、取り込み時に予想されるデータを含め、これらの各ミックスインの構造について説明します。

### プロファイルミックスイン{#profile-mixin}

[!DNL XDM Individual Profile]に基づくスキーマの場合、[!UICONTROL プライバシーの詳細]ミックスインには、顧客IDをTCFの同意の環境設定にマッピングする、単一のマップタイプフィールド`xdm:identityPrivacyInfo`が用意されています。 次のJSONは、データ取り込み時に`xdm:identityPrivacyInfo`が期待するデータの種類の例です。

```json
{
  "xdm:identityPrivacyInfo": {
      "ECID": {
        "13782522493631189": {
          "xdm:identityIABConsent": {
            "xdm:consentTimestamp": "2020-04-11T05:05:05Z",
            "xdm:consentString": {
              "xdm:consentStandard": "IAB TCF",
              "xdm:consentStandardVersion": "2.0",
              "xdm:consentStringValue": "BObdrPUOevsguAfDqFENCNAAAAAmeAAA.PVAfDObdrA.DqFENCAmeAENCDA",
              "xdm:gdprApplies": true,
              "xdm:containsPersonalData": false
            }
          }
        }
      }
    }
}
```

例に示すように、`xdm:identityPrivacyInfo`の各ルートレベルキーは、IDサービスで認識されるID名前空間に対応しています。 次に、各名前空間プロパティに、その名前空間の顧客の対応するID値とキーが一致するサブプロパティが少なくとも1つ必要です。 この例では、顧客は`13782522493631189`のExperience CloudID(`ECID`)値で識別されます。

>[!NOTE]
>
>上記の例では、1つの名前空間/値のペアを使用して顧客のIDを表していますが、他の名前空間にもキーを追加でき、各名前空間には複数のID値を設定できます。各ID値には、それぞれ独自のTCF同意の設定が含まれます。

識別値オブジェクト内には、`xdm:identityIABConsent`という単一のフィールドが含まれます。 このオブジェクトは、指定したID名前空間および値に対する顧客のTCF同意値を取得します。 このフィールドに含まれるサブプロパティを以下に示します。

| プロパティ | 説明 |
| --- | --- |
| `xdm:consentTimestamp` | TCFの同意値が変更されたときの[ISO 8601](https://www.ietf.org/rfc/rfc3339.txt)タイムスタンプ。 |
| `xdm:consentString` | 顧客の更新された同意データおよびその他のコンテキスト情報を含むオブジェクト。 このオブジェクトの必須サブプロパティについては、[同意文字列プロパティ](#consent-string)のセクションを参照してください。 |

### イベントミックスイン{#event-mixin}

[!DNL XDM ExperienceEvent]に基づくスキーマの場合、[!UICONTROL プライバシーの詳細]ミックスインには、単一の配列型フィールドが用意されています。`xdm:consentStrings`。 この配列の各項目は、プロファイルミックスインの`xdm:consentString`フィールドと同様、TCF同意文字列に必要なプロパティを含むオブジェクトである必要があります。 これらのサブプロパティの詳細については、[次のセクション](#consent-string)を参照してください。

```json
{
  "xdm:consentStrings": [
    {
      "xdm:consentStandard": "IAB TCF",
      "xdm:consentStandardVersion": "2.0",
      "xdm:consentStringValue": "BObdrPUOevsguAfDqFENCNAAAAAmeAAA.PVAfDObdrA.DqFENCAmeAENCDA",
      "xdm:gdprApplies": true,
      "xdm:containsPersonalData": false
    }
  ]
}
```

### 同意文字列プロパティ{#consent-string}

[!UICONTROL プライバシーの詳細]ミックスインの両方のバージョンには、顧客のTCF同意文字列を説明する必要なフィールドを取り込む1つ以上のオブジェクトが必要です。 以下に、これらのプロパティについて説明します。

| プロパティ | 説明 |
| --- | --- |
| `xdm:consentStandard` | データが適用される同意フレームワーク。 TCF準拠の場合、値は`IAB TCF`にする必要があります。 |
| `xdm:consentStandardVersion` | `xdm:consentStandard`が示す同意フレームワークのバージョン番号です。 TCF 2.0に準拠するため、値は`2.0`にする必要があります。 |
| `xdm:consentStringValue` | 顧客が選択した設定に基づいて、同意管理プラットフォーム(CMP)によって生成された同意文字列です。 |
| `xdm:gdprApplies` | GDPRがこの顧客に適用されるかどうかを示すboolean値。 TCF 2.0が強制されるためには、値を`true`に設定する必要があります。 含めない場合は、デフォルトで`true`に設定されます。 |
| `xdm:containsPersonalData` | 同意の更新に個人データが含まれているかどうかを示すboolean値です。 含めない場合は、デフォルトで`false`に設定されます。 |

## 顧客の同意スキーマの作成{#create-schemas}

同意データを取り込むデータセットを作成するには、まずXDMスキーマを作成し、これらのデータセットを基にしてデータセットを作成する必要があります。

プラットフォームUIの左側のナビゲーションで&#x200B;**[!UICONTROL スキーマ]**&#x200B;を選択し、[!UICONTROL スキーマ]ワークスペースを開きます。 次のセクションの手順に従って、必要な各スキーマを作成します。

>[!NOTE]
>
>同意データを取り込むために使用する既存のXDMスキーマがある場合は、新しいスキーマを作成する代わりに、それらのデータを編集できます。 ただし、既存のスキーマがリアルタイム顧客プロファイルでの使用を有効にしている場合、そのプライマリIDを、電子メールアドレスなどの利子ベースの広告で使用することを禁止する、直接識別可能なフィールドにすることはできません。 制限されているフィールドが不明な場合は、弁護士にお問い合わせください。
>
>また、既存のスキーマを編集する場合は、加算（改行なし）変更のみを行うことができます。 詳しくは、[スキーマ進化の原則](../../../../xdm/schema/composition.md#evolution)の節を参照してください。

### レコードベースの同意スキーマ{#profile-schema}を作成します

**[!UICONTROL スキーマ]**&#x200B;ワークスペースで、「**[!UICONTROL スキーマを作成]**」を選択し、ドロップダウンから「**[!UICONTROL XDM個々のプロファイル]**」を選択します。

![](../../../images/governance-privacy-security/consent/iab/dataset/create-schema-profile.png)

[!DNL Schema Editor]が表示され、キャンバスのスキーマの構造が示されます。 右側のレールを使用してスキーマの名前と説明を入力し、キャンバスの左側の&#x200B;**[!UICONTROL ミックスイン]**&#x200B;セクションの追加下の&#x200B;****&#x200B;を選択します。

![](../../../images/governance-privacy-security/consent/iab/dataset/add-mixin-profile.png)

**[!UICONTROL mixin追加]**&#x200B;ダイアログが表示されます。 ここから、リストから「**[!UICONTROL プライバシーの詳細]**」を選択します。 オプションで、検索バーを使用して結果を絞り込み、Mixinを見つけやすくすることができます。 ミックスインを選択したら、**[!UICONTROL ミックスイン追加]**&#x200B;を選択します。

![](../../../images/governance-privacy-security/consent/iab/dataset/add-profile-privacy.png)

キャンバスが再表示され、`identityPrivacyInfo`フィールドがスキーマ構造に追加されたことが示されます。

![](../../../images/governance-privacy-security/consent/iab/dataset/profile-privacy-structure.png)

ここから上記の手順を繰り返し、以下のミックスインをスキーマに追加します。

* [!UICONTROL IdentityMap]
* [!UICONTROL プロファイル用のデータ取得領域]
* [!UICONTROL 人口統計の詳細]
* [!UICONTROL 個人の連絡先の詳細]

![](../../../images/governance-privacy-security/consent/iab/dataset/profile-all-mixins.png)

既に[!DNL Real-time Customer Profile]での使用が有効になっている既存のスキーマを編集する場合は、**[!UICONTROL 「保存]**」を選択して変更を確認し、[同意スキーマ](#dataset)に基づくデータセットの作成の節に進みます。 新しいスキーマを作成する場合は、次のサブセクションで説明されている手順に従ってください。

#### スキーマを[!DNL Real-time Customer Profile]で使用できるようにする

プラットフォームが受け取った同意データを特定の顧客プロファイルに関連付けるには、[!DNL Real-time Customer Profile]での使用に対して同意スキーマを有効にする必要があります。

>[!NOTE]
>
>この節に示す例のスキーマでは、`identityMap`フィールドを主IDとして使用しています。 別のフィールドを主IDとして設定する場合は、cookie IDなどの間接識別子を使用し、電子メールアドレスなどの利子ベースの広告で使用するのを禁止する直接識別子を使用しないようにしてください。 制限されているフィールドが不明な場合は、弁護士にお問い合わせください。
>
>スキーマのプライマリIDフィールドを設定する手順については、[スキーマ作成チュートリアル](../../../../xdm/tutorials/create-schema-ui.md#identity-field)を参照してください。

[!DNL Profile]のスキーマを有効にするには、左側のパネルでスキーマの名前を選択し、右側のパネルの&#x200B;**[!UICONTROL スキーマのプロパティ]**&#x200B;ダイアログを開きます。 ここから、**[!UICONTROL プロファイル]**&#x200B;切り替えボタンを選択します。

![](../../../images/governance-privacy-security/consent/iab/dataset/profile-enable-profile.png)

プライマリIDが見つからないことを示すポーバーが表示されます。 プライマリIDは`identityMap`フィールドに格納されるので、代替のプライマリIDを使用する場合は、チェックボックスをオンにします。

![](../../../images/governance-privacy-security/consent/iab/dataset/missing-primary-identity.png)

最後に、「**[!UICONTROL 保存]**」を選択して変更を確認します。

![](../../../images/governance-privacy-security/consent/iab/dataset/profile-save.png)

### 時系列ベースの同意スキーマ{#event-schema}を作成します

**[!UICONTROL スキーマ]**&#x200B;ワークスペースで、「**[!UICONTROL スキーマを作成]**」を選択し、ドロップダウンから「**[!UICONTROL XDM ExperienceEvent]**」を選択します。

![](../../../images/governance-privacy-security/consent/iab/dataset/create-schema-event.png)

[!DNL Schema Editor]が表示され、キャンバスのスキーマの構造が示されます。 右側のレールを使用してスキーマの名前と説明を入力し、キャンバスの左側の&#x200B;**[!UICONTROL ミックスイン]**&#x200B;セクションの追加下の&#x200B;****&#x200B;を選択します。

![](../../../images/governance-privacy-security/consent/iab/dataset/add-mixin-event.png)

**[!UICONTROL mixin追加]**&#x200B;ダイアログが表示されます。 ここから、リストから「**[!UICONTROL プライバシーの詳細]**」を選択します。 オプションで、検索バーを使用して結果を絞り込み、Mixinを見つけやすくすることができます。 ミックスインを選択したら、**[!UICONTROL ミックスイン追加]**&#x200B;を選択します。

![](../../../images/governance-privacy-security/consent/iab/dataset/add-event-privacy.png)

キャンバスが再表示され、`consentStrings`配列がスキーマ構造に追加されたことが示されます。

![](../../../images/governance-privacy-security/consent/iab/dataset/event-privacy-structure.png)

ここから上記の手順を繰り返し、以下のミックスインをスキーマに追加します。

* [!UICONTROL IdentityMap]
* [!UICONTROL 環境の詳細]
* [!UICONTROL Web詳細]
* [!UICONTROL 実装の詳細]

ミックスインを追加したら、「**[!UICONTROL 保存]**」を選択して終了します。

![](../../../images/governance-privacy-security/consent/iab/dataset/event-all-mixins.png)

## 同意スキーマに基づいてデータセットを作成する{#datasets}

前述の必須スキーマごとに、顧客の同意データを最終的に取り込むデータセットを作成する必要があります。 レコードスキーマに基づくデータセットは[!DNL Real-time Customer Profile]に対して有効にする必要がありますが、時系列スキーマ&#x200B;**に基づくデータセットは[!DNL Profile]-enabledにしないでください。**

最初に、左のナビゲーションで「**[!UICONTROL データセット]**」を選択し、右上隅の「**[!UICONTROL データセットを作成]**」を選択します。

![](../../../images/governance-privacy-security/consent/iab/dataset/dataset-create.png)

次のページで、「**[!UICONTROL スキーマからデータセットを作成]**」を選択します。

![](../../../images/governance-privacy-security/consent/iab/dataset/dataset-create-from-schema.png)

**[!UICONTROL スキーマ]**&#x200B;からデータセットを作成ワークフローが表示され、**[!UICONTROL スキーマを選択]**&#x200B;の手順から開始します。 提供されたリストで、前に作成した同意スキーマの1つを探します。 オプションで検索バーを使用して結果を絞り込み、スキーマを見つけやすくできます。 目的のスキーマの横にあるラジオボタンを選択し、「**[!UICONTROL 次へ]**」を選択して次に進みます。

![](../../../images/governance-privacy-security/consent/iab/dataset/dataset-select-schema.png)

**[!UICONTROL データセットの設定]**&#x200B;手順が表示されます。「**[!UICONTROL 完了]**」を選択する前に、データセットの一意で識別しやすい名前と説明を指定します。

![](../../../images/governance-privacy-security/consent/iab/dataset/dataset-configure.png)

新しく作成したデータセットの詳細ページが表示されます。 データセットが時系列スキーマに基づいている場合は、プロセスが完了します。 データセットがレコードスキーマに基づいている場合は、プロセスの最後の手順として、[!DNL Real-time Customer Profile]で使用するデータセットを有効にします。

右側のパネルで、**[!UICONTROL プロファイル]**&#x200B;の切り替えを選択し、確認プロバーで&#x200B;**[!UICONTROL 有効]**&#x200B;を選択して、[!DNL Profile]のスキーマを有効にします。

![](../../../images/governance-privacy-security/consent/iab/dataset/dataset-enable-profile.png)

上記の手順を再度実行し、TCF 2.0準拠に必要なその他のデータセットを作成します。

## 次の手順

このチュートリアルに従うと、顧客の同意データを収集するために使用できる2つのデータセットが作成されます。

* リアルタイム顧客プロファイルでの使用が有効になっている、レコードベースのデータセットです。
* [!DNL Profile]に対して有効になっていない時系列ベースのデータセット。

これで、[IAB TCF 2.0 overview](./overview.md#merge-policies)に戻り、TCF 2.0準拠のためのプラットフォームの設定プロセスを続行できます。
