---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: 同意データと環境設定データを取り込むためのデータセットの設定
topic-legacy: getting started
description: Adobe Experience Platformで同意データと環境設定データを取り込むためのExperience Data Model(XDM)スキーマとデータセットの設定方法について説明します。
exl-id: 61ceaa2a-c5ac-43f5-b118-502bdc432234
source-git-commit: 656d772335c2f5ae58b471b31bfbd6dfa82490cd
workflow-type: tm+mt
source-wordcount: '1573'
ht-degree: 4%

---

# 同意データと基本設定データを取り込むためのデータセットの設定

Adobe Experience Platformが顧客の同意/環境設定データを処理するには、そのデータを、同意と他の権限に関連するフィールドを含むスキーマのデータセットに送信する必要があります。 特に、このデータセットは[!DNL XDM Individual Profile]クラスに基づいており、[!DNL Real-time Customer Profile]での使用を有効にする必要があります。

このドキュメントでは、データセットを設定して同意データを処理する手順をExperience Platformします。 Platformで同意/環境設定データを処理するための完全なワークフローの概要については、[同意処理の概要](./overview.md)を参照してください。

>[!IMPORTANT]
>
>このガイドの例では、顧客の同意の値を表す標準化されたフィールドセットを使用します。[[!UICONTROL 同意と環境設定の詳細]スキーマフィールドグループ](../../../../xdm/field-groups/profile/consents.md)で定義されます。 これらのフィールドの構造は、多くの一般的な同意収集の使用例をカバーする効率的なデータモデルを提供することを目的としています。
>
>ただし、独自のデータモデルに従って同意を表す独自のフィールドグループを定義することもできます。 次のオプションに基づいて、ビジネスニーズに合った同意データモデルの承認を得るには、法務チームに問い合わせてください。
>
>* 標準化された同意フィールドグループ
>* 組織で作成されたカスタムの同意フィールドグループ
>* 標準化された同意フィールドグループと、カスタムの同意フィールドグループが提供する追加フィールドの組み合わせ


## 前提条件

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [エクスペリエンスデータモデル（XDM）](../../../../xdm/home.md)[!DNL Experience Platform]： が顧客エクスペリエンスデータを整理する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../xdm/schema/composition.md)：XDM スキーマの基本的な構成要素について説明します。
* [リアルタイム顧客プロファイル](../../../../profile/home.md):様々なソースの顧客データを完全な統合ビューに統合し、顧客とのやり取りごとに実用的なタイムスタンプ付きの説明を提供します。

>[!IMPORTANT]
>
>このチュートリアルでは、顧客属性情報の取得に使用するPlatformの[!DNL Profile]スキーマを理解していることを前提としています。 同意データの収集方法に関係なく、このスキーマはリアルタイム顧客プロファイル](../../../../xdm/ui/resources/schemas.md#profile)に対して有効にする必要があります。 [さらに、スキーマのプライマリIDを、電子メールアドレスなどの興味/関心に基づく広告での使用を禁止されている、直接識別可能なフィールドにすることはできません。 どのフィールドが制限されているかが不明な場合は、弁護士に相談してください。

## [!UICONTROL 同意と環境設定の詳細フ] ィールドグループ構造 {#structure}

[!UICONTROL Consent and Preference Details]フィールドグループは、スキーマに標準化された同意フィールドを提供します。 現在、このフィールドグループは、[!DNL XDM Individual Profile]クラスに基づくスキーマとのみ互換性があります。

フィールドグループには、1つのオブジェクトタイプフィールド`consents`が用意されています。このフィールドのサブプロパティは、一連の標準化された同意フィールドを取り込みます。 次のJSONは、データ取得時に`consents`が想定されるデータの種類の例です。

```json
{
  "consents": {
    "collect": {
      "val": "y",
    },
    "share": {
      "val": "y",
    },
    "personalize": {
      "content": {
        "val": "y"
      }
    },
    "marketing": {
      "preferred": "email",
      "any": {
        "val": "y"
      },
      "push": {
        "val": "n",
        "reason": "Too Frequent",
        "time": "2019-01-01T15:52:25+00:00"
      }
    },
    "idSpecific": {
      "email": {
        "jdoe@example.com": {
          "marketing": {
            "email": {
              "val": "n"
            }
          }
        }
      }
    }
  },
  "metadata": {
    "time": "2019-01-01T15:52:25+00:00"
  }
}
```

>[!NOTE]
>
>`consents`のサブプロパティの構造と意味について詳しくは、[[!UICONTROL 同意と環境設定の詳細]フィールドグループ](../../../../xdm/field-groups/profile/consents.md)の概要を参照してください。

## [!DNL Profile]スキーマに必要なフィールドグループを追加する {#add-field-group}

Adobe標準を使用して同意データを収集するには、次の2つのフィールドグループを含むプロファイル対応スキーマが必要です。

* [!UICONTROL 同意と環境設定の詳細]
* [!UICONTROL IdentityMap] （Platform WebまたはモバイルSDKを使用して同意シグナルを送信する場合に必要）

プラットフォームUIで、左側のナビゲーションから「**[!UICONTROL スキーマ]**」を選択し、「**[!UICONTROL 参照]**」タブを選択して既存のスキーマのリストを表示します。 ここから、同意フィールドを追加する[!DNL Profile]対応スキーマの名前を選択します。 この節のスクリーンショットでは、例として、[スキーマ作成チュートリアル](../../../../xdm/tutorials/create-schema-ui.md)で作成した「ロイヤルティメンバー」スキーマを使用します。

![](../../../images/governance-privacy-security/consent/adobe/dataset-prep/select-schema.png)

>[!TIP]
>
>ワークスペースの検索およびフィルタリング機能を使用すると、スキーマを見つけやすくなります。 詳しくは、[XDMリソース](../../../../xdm/ui/explore.md)のガイドを参照してください。

[!DNL Schema Editor]が表示され、キャンバスにスキーマの構造が表示されます。 キャンバスの左側で、「**[!UICONTROL フィールドグループ]**」セクションの下にある「**[!UICONTROL 追加]**」を選択します。

![](../../../images/governance-privacy-security/consent/adobe/dataset-prep/add-field-group.png)

**[!UICONTROL フィールドグループを追加]**&#x200B;ダイアログが表示されます。 ここから、リストから「**[!UICONTROL 同意と環境設定の詳細]**」を選択します。 オプションで、検索バーを使用して結果を絞り込み、フィールドグループを見つけやすくすることができます。

![](../../../images/governance-privacy-security/consent/adobe/dataset-prep/field-group-dialog.png)

次に、リストから&#x200B;**[!UICONTROL IdentityMap]**&#x200B;フィールドグループを探し、同じように選択します。 両方のフィールドグループが右側のレールに表示されたら、「**[!UICONTROL フィールドグループを追加]**」を選択します。

![](../../../images/governance-privacy-security/consent/adobe/dataset-prep/identitymap.png)

キャンバスが再び表示され、 `consents`フィールドと`identityMap`フィールドがスキーマ構造に追加されたことが示されます。 標準フィールドグループで取り込まれない追加の同意フィールドと環境設定フィールドが必要な場合は、[スキーマへのカスタム同意フィールドと環境設定フィールドの追加](#custom-consent)に関する付録の節を参照してください。 それ以外の場合は、「**[!UICONTROL 保存]**」を選択して、変更をスキーマに確定します。

![](../../../images/governance-privacy-security/consent/adobe/dataset-prep/save-schema.png)

>[!IMPORTANT]
>
>新しいスキーマを作成する場合、またはプロファイルに対して有効になっていない既存のスキーマを編集する場合は、保存する前に[プロファイル](../../../../xdm/ui/resources/schemas.md#profile)のスキーマを有効にする必要があります。

編集したスキーマが、Platform Web SDKデータストリームで指定された[!UICONTROL プロファイルデータセット]で使用されている場合、そのデータセットには新しい同意フィールドが含まれるようになります。 これで、[同意処理ガイド](./overview.md#merge-policies)に戻り、同意データを処理するためのExperience Platformの設定プロセスを続行できます。 このスキーマのデータセットをまだ作成していない場合は、次の節の手順に従います。

## 同意スキーマに基づくデータセットの作成 {#dataset}

同意フィールドを含むスキーマを作成したら、顧客の同意データを最終的に取り込むデータセットを作成する必要があります。 このデータセットは[!DNL Real-time Customer Profile]に対して有効にする必要があります。

最初に、左のナビゲーションで「**[!UICONTROL データセット]**」を選択し、右上隅の「**[!UICONTROL データセットを作成]**」を選択します。

![](../../../images/governance-privacy-security/consent/adobe/dataset-prep/create-dataset.png)

次のページで、「**[!UICONTROL Create dataset from schema]**」を選択します。

![](../../../images/governance-privacy-security/consent/adobe/dataset-prep/from-schema.png)

**[!UICONTROL スキーマからデータセットを作成]**&#x200B;ワークフローが表示され、**[!UICONTROL スキーマを選択]**&#x200B;の手順を開始します。 提供されたリストで、作成した同意スキーマの1つを探します。 オプションで、検索バーを使用して結果を絞り込み、スキーマを見つけやすくすることができます。 目的のスキーマの横にあるラジオボタンを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![](../../../images/governance-privacy-security/consent/adobe/dataset-prep/select-dataset-schema.png)

**[!UICONTROL データセットの設定]**&#x200B;手順が表示されます。「**[!UICONTROL 完了]**」を選択する前に、データセットの一意の、識別しやすい名前と説明を指定します。

![](../../../images/governance-privacy-security/consent/adobe/dataset-prep/dataset-details.png)

新しく作成されたデータセットの詳細ページが表示されます。 データセットが時系列スキーマに基づいている場合、プロセスは完了です。 データセットがレコードスキーマに基づいている場合、プロセスの最後の手順は、[!DNL Real-time Customer Profile]で使用するデータセットを有効にすることです。

右側のレールで、「**[!UICONTROL プロファイル]**」切り替えを選択します。

![](../../../images/governance-privacy-security/consent/adobe/dataset-prep/profile-toggle.png)

最後に、確認ポップオーバーで「**[!UICONTROL 有効]**」を選択して、[!DNL Profile]のスキーマを有効にします。

![](../../../images/governance-privacy-security/consent/adobe/dataset-prep/enable-dataset.png)

これで、データセットが保存され、[!DNL Profile]での使用が有効になります。 Platform Web SDKを使用して同意データをプロファイルに送信する場合は、[datastream](../../../../edge/fundamentals/datastreams.md)を設定する際に、このデータセットを[!UICONTROL プロファイルデータセット]として選択する必要があります。

## 次の手順

このチュートリアルに従って、[!DNL Profile]が有効なスキーマに同意フィールドを追加しました。このデータセットは、Platform Web SDKまたは直接XDM取得を使用して同意データを取得するために使用されます。

[同意処理の概要](./overview.md#merge-policies)に戻り、同意データを処理するためのExperience Platformの設定を続行できます。

## 付録

次の節では、顧客の同意データと環境設定データを取り込むデータセットの作成に関する追加情報を示します。

### スキーマへのカスタムの同意フィールドと環境設定フィールドの追加 {#custom-consent}

標準の[!UICONTROL Consent and Preference Details]フィールドグループで表される以外の同意シグナルを追加で取得する必要がある場合は、カスタムXDMコンポーネントを使用して、特定のビジネスニーズに合わせて同意スキーマを拡張できます。 この節では、これらのシグナルをプロファイルに取り込むための同意スキーマのカスタマイズ方法の基本原則について説明します。

>[!IMPORTANT]
>
>Platform WebおよびモバイルSDKは、同意変更コマンドのカスタムフィールドをサポートしていません。 現在、カスタム同意フィールドをプロファイルに取り込む唯一の方法は、[バッチ取り込み](../../../../ingestion/batch-ingestion/overview.md)または[ソース接続](../../../../sources/home.md)を使用することです。

[!UICONTROL 同意と環境設定の詳細]フィールドグループを同意データの構造のベースラインとして使用し、構造全体を一から作成するのではなく、必要に応じて追加のフィールドを追加することを強くお勧めします。

標準フィールドグループの構造にカスタムフィールドを追加するには、まずカスタムフィールドグループを作成する必要があります。 [!UICONTROL 同意と環境設定の詳細]フィールドグループをスキーマに追加した後、**[!UICONTROL フィールドグループ]**&#x200B;セクションで&#x200B;**プラス(+)**&#x200B;アイコンを選択し、**[!UICONTROL 新しいフィールドグループ]**&#x200B;を作成します。 フィールドグループの名前と説明（オプション）を入力し、「**[!UICONTROL フィールドグループを追加]**」を選択します。

![](../../../images/governance-privacy-security/consent/adobe/dataset-prep/add-custom-field-group.png)

[!DNL Schema Editor]が再び表示され、左側のレールで新しいカスタムフィールドグループが選択されます。 キャンバスに、カスタムフィールドをスキーマ構造に追加できるコントロールが表示されます。 新しい同意または環境設定フィールドを追加するには、`consents`オブジェクトの横にある&#x200B;**プラス(+)**&#x200B;アイコンを選択します。

![](../../../images/governance-privacy-security/consent/adobe/dataset-prep/add-custom-field.png)

`consents`オブジェクト内に新しいフィールドが表示されます。 標準のXDMオブジェクトにカスタムフィールドを追加するので、新しいフィールドはテナントIDに名前空間が割り当てられたオブジェクトの下に作成されます。

![](../../../images/governance-privacy-security/consent/adobe/dataset-prep/nested-tenantId.png)

**[!UICONTROL フィールドのプロパティ]**&#x200B;の下の右側のレールで、フィールドの名前と説明を入力します。 フィールドの&#x200B;**[!UICONTROL 型]**&#x200B;を選択する場合は、カスタムの同意または環境設定フィールドに適した標準データ型を使用する必要があります。

* [[!UICONTROL 一般的な同意フィールド]](../../../../xdm/data-types/consent-field.md)
* [[!UICONTROL 汎用マーケティング環境設定フィールド]](../../../../xdm/data-types/marketing-field.md)
* [[!UICONTROL 購読付きの汎用マーケティング環境設定フィールド]](../../../../xdm/data-types/marketing-field-subscriptions.md)
* [[!UICONTROL 汎用パーソナライゼーション設定フィールド]](../../../../xdm/data-types/personalization-field.md)

終了したら、「**[!UICONTROL 適用]**」を選択します。

![](../../../images/governance-privacy-security/consent/adobe/dataset-prep/field-properties.png)

同意または環境設定フィールドがスキーマ構造に追加されます。 右側のレールに表示される[!UICONTROL パス]には、`_tenantId`名前空間が含まれています。 データ操作でこのフィールドへのパスを参照する場合は必ず、この名前空間を含める必要があります。

![](../../../images/governance-privacy-security/consent/adobe/dataset-prep/field-added.png)

必要な同意フィールドと環境設定フィールドの追加を続行するには、上記の手順に従います。 終了したら、「**[!UICONTROL 保存]**」を選択して変更を確定します。

このスキーマのデータセットを作成していない場合は、[データセット](#dataset)の作成の節に進んでください。
