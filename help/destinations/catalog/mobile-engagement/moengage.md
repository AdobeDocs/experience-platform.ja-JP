---
title: Moengage 接続
description: Moengage は、消費者とブランドの間の顧客中心インタラクションをリアルタイムで強化する顧客エンゲージメントプラットフォームです。
last-substantial-update: 2023-10-11T00:00:00Z
exl-id: 051f1a10-3c41-4c0a-b187-bf80de0565f0
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '1036'
ht-degree: 35%

---

# [!DNL Moengage] 接続

## 概要 {#overview}

[!DNL Moengage] の宛先を使用して、Adobe データ（ユーザー属性、セグメント、イベント）を MoEngage にリアルタイムに接続およびマッピングします。 その後、顧客はこのデータに基づいて行動し、パーソナライズされたターゲット設定されたエクスペリエンスを提供できます。

Adobeを使用すると、統合は非常にシンプルで直感的になります。 任意のAdobe ユーザープロファイルを取得し、MoEngage ユーザー属性にマッピングするだけです。

>[!IMPORTANT]
>
>この宛先コネクタとドキュメントページは、*Moengage* チームが作成および管理します。 お問い合わせや更新のリクエストについては、*`https://help.moengage.com/hc/en-us`まで直接ご連絡ください。*

## ユースケース {#use-cases}

マーケターが [!DNL Moengage] キャンペーンを通じて、（Adobe Experience Platformで作成された）ユーザーセグメントのターゲットを設定したいと考えています。 また、Adobe Experience Platform プロファイルの属性に基づいてキャンペーンコンテンツをパーソナライズする必要もあります。 この統合により、Adobe Experience Platformでセグメントとプロファイルが更新されるとすぐに、MoEngage でユーザーと属性が更新されます。

## 前提条件 {#prerequisites}

Adobe Experience Platform データを [!DNL Moengage] に送信する前に、次の前提条件に注意してください。

* Adobe Experience Platformで MoEngage の宛先を使用するには、まず [!DNL Moengage] アカウントにアクセスできる必要があります。 次のページにアクセスして、MoEngage アカウントに新規登録またはログインしてください：https://app.moengage.com


## サポートされる ID {#supported-identities}

[!DNL Moengage] では、以下の表で説明する ID のアクティベーションをサポートしています。

| ターゲット ID | 説明 | 注意点 |
|---|------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| user_id | [!DNL Moengage] システム内のユーザープロファイルを一意に識別する一意の識別子。 | この識別子は、文字列タイプをサポートします。 user_id または anonymous_id のいずれかが必要です |
| anonymous_id | 不明なユーザープロファイルのもう 1 つの識別子（システムに存在しないプロファイルを意味します）。 | この識別子は、文字列タイプをサポートします。 user_id または anonymous_id のいずれかが必要です |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | 識別子（user_id、anonymous_id）と、で定義されたカスタム属性を [!DNL Moengage] に書き出したセグメント（オーディエンス）のすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。セグメント評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

![Moengage 宛先認証 &#x200B;](../../assets/catalog/mobile-engagement/moengage/authentication.png)

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横にアスタリスクが表示される場合は、そのフィールドが必須であることを示します。
![Moengage 宛先認証 &#x200B;](../../assets/catalog/mobile-engagement/moengage/settings.png)
* **[!UICONTROL ユーザー名]**：ダッシュボードの設定ページのデータアプリ ID[!DNL Moengage] す。
* **[!UICONTROL パスワード]**：ダッシュボードの設定ページのデータアプリキー [!DNL Moengage] す。

![Moengage 宛先認証 &#x200B;](../../assets/catalog/mobile-engagement/moengage/destination_details.png)

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL 地域]**：アプリ *データセンター*。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先にオーディエンスセグメントをアクティブ化する手順は、[ストリーミングセグメント書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

### 属性と ID のマッピング {#map}

オーディエンスデータを [!DNL Adobe Experience Platform] から [!DNL Moengage] の宛先に正しく送信するには、フィールドマッピングの手順を実行する必要があります。

マッピングは、[!DNL Experience Platform] アカウントの [!DNL Experience Data Model] （XDM）スキーマフィールドと、ターゲット宛先から対応する同等のスキーマフィールドとの間にリンクを作成して構成されます。

XDM フィールドを [!DNL Moengage] 宛先フィールドに正しくマッピングするには、次の手順に従います。

[!UICONTROL &#x200B; マッピング &#x200B;] ステップで、「**[!UICONTROL チェックボックス]**」を選択します。

![Moengage 宛先追加マッピング &#x200B;](../../assets/catalog/mobile-engagement/moengage/segments.png)

[!UICONTROL &#x200B; マッピング &#x200B;] 手順で、「**[!UICONTROL 新しいマッピングを追加]**」を選択します。

![Moengage 宛先追加マッピング &#x200B;](../../assets/catalog/mobile-engagement/moengage/mapping.png)

「[!UICONTROL Source フィールド &#x200B;]」セクションで、空のフィールドの横にある矢印ボタンを選択します。

![Moengage 宛先Sourceのマッピング &#x200B;](../../assets/catalog/mobile-engagement/moengage/mapping-source.png)

[!UICONTROL &#x200B; ソースフィールドを選択 &#x200B;] ウィンドウでは、XDM フィールドの次の 2 つのカテゴリから選択できます。
* [!UICONTROL &#x200B; 属性を選択 &#x200B;]：このオプションを使用して、XDM スキーマから特定のフィールドを属性にマッピン [!DNL Moengage] します。

![Moengage 宛先マッピングSource属性 &#x200B;](../../assets/catalog/mobile-engagement/moengage/mapping-attributes.png)

ソースフィールドを選択してから、「**[!UICONTROL 選択]** を選択します。

「[!UICONTROL &#x200B; ターゲットフィールド &#x200B;]」セクションで、フィールドの右側にあるマッピングアイコンを選択します。

![Moengage 宛先ターゲットマッピング &#x200B;](../../assets/catalog/mobile-engagement/moengage/mapping-target.png)

[!UICONTROL &#x200B; ターゲットフィールドを選択 &#x200B;] ウィンドウでは、次の 2 つのカテゴリのターゲットフィールドから選択できます。
* [!UICONTROL ID 名前空間を選択 &#x200B;]：このオプションを使用して、ID 名前空間 [!DNL Experience Platform]ID 名前空間にマッピン [!DNL Moengage] します。
* [!UICONTROL &#x200B; カスタム属性を選択 &#x200B;]：このオプションを使用して、XDM 属性を [!DNL Moengage] アカウントで定義したカスタム [!DNL Moengage] 属性にマッピングします。 <br> また、このオプションを使用して、既存の XDM 属性の名前を [!DNL Moengage] に変更することもできます。 例えば、`lastName` XDM 属性を [!DNL Moengage] のカスタム `Last_Name` 属性にマッピングすると、`Last_Name` 属性が存在しない場合は [!DNL Moengage] に作成し、`lastName` XDM 属性をマッピングします。

![Moengage 宛先ターゲットマッピングフィールド &#x200B;](../../assets/catalog/mobile-engagement/moengage/mapping-target-fields.png)

ターゲットフィールドを選択し、「**[!UICONTROL 選択]** を選択します。

これで、リストにフィールドマッピングが表示されます。

![Moengage 宛先マッピングの完了 &#x200B;](../../assets/catalog/mobile-engagement/moengage/mapping-complete.png)

さらにマッピングを追加するには、前の手順を繰り返します。

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

データがに正常に [!DNL Moengage] の宛先に書き出されたかどうかを確認するには、[!DNL Moengage] アカウントのユーザープロファイルに移動します。 ここには、自動的に作成された `AEPSegments` という名前のユーザー属性と、Adobe Experience Platformの前の手順でマッピングされたその他のカスタム属性が表示されます。

`AEPSegments` は、[!DNL Moengage] の配列型の属性です。 Experience Platformでユーザーが関連付けられているすべてのAdobe オーディエンス名が一覧表示されます。


![Moengage 宛先マッピングの完了 &#x200B;](../../assets/catalog/mobile-engagement/moengage/validation.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
