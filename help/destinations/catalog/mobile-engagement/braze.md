---
keywords: モバイル；勇気づけメッセージ；
title: 接続をブレーズ
description: Braze は、顧客と顧客が好むブランドとの間の関連性の高い思い出に残るエクスペリエンスを強化する、包括的な顧客エンゲージメントプラットフォームです。
exl-id: 508e79ee-7364-4553-b153-c2c00cc85a73
source-git-commit: 3d7151645bc90a2dcbd6b31251ed459029ab77c9
workflow-type: tm+mt
source-wordcount: '757'
ht-degree: 1%

---

# [!DNL Braze] 接続

## 概要 {#overview}

[!DNL Braze] の宛先は、プロファイルデータを [!DNL Braze] に送信する際に役立ちます。

[!DNL Braze] は、顧客と顧客が好むブランドとの間の関連性の高い思い出に残るエクスペリエンスを強化する、包括的な顧客エンゲージメントプラットフォームです。

プロファイルデータを [!DNL Braze] に送信するには、まず宛先に接続する必要があります。

## 宛先の詳細 {#specifics}

次の [!DNL Braze] 宛先に固有の詳細に注意してください。

* [!DNL Adobe Experience Platform] セグメントは、属性の下のにエ [!DNL Braze] クスポート `AdobeExperiencePlatformSegments` されます。

>[!NOTE]
>
>追加のカスタム属性を [!DNL Braze] に送信すると、[!DNL Braze] データポイントの消費が増加する場合があることに注意してください。 追加のカスタム属性を送信する前に、[!DNL Braze] アカウントマネージャーにお問い合わせください。

## ユースケース {#use-cases}

マーケターの場合、[!DNL Adobe Experience Platform] でセグメントを構築し、モバイルエンゲージメントの宛先でユーザーをターゲットにしたいと考えています。 さらに、[!DNL Adobe Experience Platform] でセグメントとプロファイルが更新されしだい、[!DNL Adobe Experience Platform] プロファイルの属性に基づいて、パーソナライズされたエクスペリエンスをユーザーに提供したいと考えています。

## サポートされる ID {#supported-identities}

[!DNL Braze] では、以下の表で説明する ID のアクティブ化をサポートしています。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| external_id | 任意の ID のマッピングをサポートするカスタム [!DNL Braze] 識別子。 | [!DNL Braze] [`external_id`](https://www.braze.com/docs/api/basics/#external-user-id-explanation) にマッピングしていれば、任意の [ID](../../../identity-service/namespaces.md) を [!DNL Braze] の宛先に送信できます。 |

## 書き出しタイプ {#export-type}

**[!DNL Profile-based]** ：セグメントのすべてのメンバーを、目的のスキーマフィールド ( 例：フィールドマッピングに従った、電子メールアドレス、電話番号、姓 ) や ID。[!DNL Adobe Experience Platform] セグメントは、属性の下のにエ [!DNL Braze] クスポート `AdobeExperiencePlatformSegments` されます。

## 宛先に接続 {#connect}

この宛先に接続するには、[ 宛先の設定に関するチュートリアル ](../../ui/connect-destination.md) で説明されている手順に従います。

### 接続パラメーター {#parameters}

[ この宛先を設定 ](../../ui/connect-destination.md) する際は、次の情報を指定する必要があります。

* **[!UICONTROL アカウントトークンをブレーズ]**:これが鍵 [!DNL Braze] [!DNL API] です。[!DNL API] キーを入手する方法について詳しくは、以下を参照してください。[REST API キーの概要 ](https://www.braze.com/docs/api/api_key/)
* **[!UICONTROL 名前]**:この宛先が将来認識される名前を入力します。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明を入力します。
* **[!UICONTROL エンドポイントインスタンス]**:使用する必要のあ [!DNL Braze] るエンドポイントインスタンスを担当者に問い合わせてください。

## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、[ ストリーミングセグメントの書き出し先へのオーディエンスデータのアクティブ化 ](../../ui/activate-segment-streaming-destinations.md) を参照してください。

## マッピングの考慮事項 {#mapping-considerations}

オーディエンスデータを [!DNL Adobe Experience Platform] から [!DNL Braze] の宛先に正しく送信するには、フィールドマッピングの手順を実行する必要があります。

マッピングは、[!DNL Platform] アカウント内の [!DNL Experience Data Model] (XDM) スキーマフィールドと、対応するターゲット宛先の対応するフィールドとの間のリンクの作成で構成されます。

XDM フィールドを [!DNL Braze] 宛先フィールドに正しくマッピングするには、次の手順に従います。

[!UICONTROL  マッピング ] の手順で、「**[!UICONTROL 新しいマッピングを追加]**」をクリックします。

![宛先の追加マッピングをブレーズ](../../assets/catalog/mobile-engagement/braze/mapping.png)

「[!UICONTROL  ソースフィールド ]」セクションで、空のフィールドの横にある矢印ボタンをクリックします。

![宛先のソースマッピングを分割](../../assets/catalog/mobile-engagement/braze/mapping-source.png)

[!UICONTROL  ソースフィールドを選択 ] ウィンドウで、次の 2 つのカテゴリの XDM フィールドを選択できます。
* [!UICONTROL 属性を選択]:このオプションを使用して、XDM スキーマの特定のフィールドを属性にマッピング [!DNL Braze] します。

![宛先マッピングのソース属性をブレーズ](../../assets/catalog/mobile-engagement/braze/mapping-attributes.png)

* [!UICONTROL ID 名前空間の選択]:ID 名前空間を名前空間にマッピ [!DNL Platform] ングするには、このオプションを使 [!DNL Braze] 用します。

![宛先マッピングのソース名前空間を分割](../../assets/catalog/mobile-engagement/braze/mapping-namespaces.png)

ソースフィールドを選択し、「**[!UICONTROL 選択]**」をクリックします。

「[!UICONTROL  ターゲットフィールド ]」セクションで、フィールドの右側にあるマッピングアイコンをクリックします。

![宛先ターゲットマッピングのブレーズ](../../assets/catalog/mobile-engagement/braze/mapping-target.png)

[!UICONTROL  ターゲットフィールドを選択 ] ウィンドウで、次の 2 つのカテゴリのターゲットフィールドを選択できます。
* [!UICONTROL ID 名前空間の選択]:ID 名前空間を ID 名前空間にマ [!DNL Platform] ッピングするには、こ [!DNL Braze] のオプションを使用します。
* [!UICONTROL カスタム属性の選択]:このオプションを使用して、XDM 属性を、アカウントで定 [!DNL Braze] 義したカスタム属性にマッピ [!DNL Braze] ングします。<br> また、このオプションを使用して、既存の XDM 属性の名前をに変更できま [!DNL Braze]す。例えば、`lastName` XDM 属性を [!DNL Braze] のカスタム `Last_Name` 属性にマッピングすると、[!DNL Braze] に `Last_Name` 属性が作成されます（まだ存在しない場合）。また、`lastName` XDM 属性をその属性にマッピングします。

![宛先ターゲットマッピングフィールドの分類](../../assets/catalog/mobile-engagement/braze/mapping-target-fields.png)

ターゲットフィールドを選択し、「**[!UICONTROL 選択]**」をクリックします。

これで、リストにフィールドマッピングが表示されます。

![宛先マッピングのブレーズ完了](../../assets/catalog/mobile-engagement/braze/mapping-complete.png)

マッピングをさらに追加するには、上記の手順を繰り返します。

## マッピングの例 {#mapping-example}

XDM プロファイルスキーマと [!DNL Braze] インスタンスに次の属性と ID が含まれているとします。

|  | XDM プロファイルスキーマ | [!DNL Braze] インスタンス |
|---|---|---|
| 属性 | <ul><li>person.name.firstName</code></li><li>person.name.lastName</code></li><li>mobilePhone.number</code></li></ul> | <ul><li>名 </code></li><li>姓 </code></li><li>電話番号 </code></li></ul> |
| ID | <ul><li>メール</code></li><li>Google広告 ID (GAID)</code></li><li>Apple Id For Advertisers(IDFA)</code></li></ul> | <ul><li>external_id</code></li></ul> |

正しいマッピングは次のようになります。

![宛先マッピングのブレーズの例](../../assets/catalog/mobile-engagement/braze/mapping-example.png)

## エクスポートされたデータ {#exported-data}

データが [!DNL Braze] の宛先に正常に書き出されたかどうかを確認するには、[!DNL Braze] アカウントを確認します。 [!DNL Adobe Experience Platform] セグメントは、属性の下のにエ [!DNL Braze] クスポート `AdobeExperiencePlatformSegments` されます。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] の宛先はすべて、データを処理する際のデータ使用ポリシーに準拠しています。 [!DNL Adobe Experience Platform] によるデータガバナンスの強制方法について詳しくは、[ データガバナンスの概要 ](../../../data-governance/home.md) を参照してください。
