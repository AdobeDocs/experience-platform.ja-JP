---
title: Gainsight PX 接続
description: Gainsight PX の宛先を使用して、Gainsight PX プラットフォームにセグメント情報を送信します。
last-substantial-update: 2024-02-20T00:00:00Z
source-git-commit: dff460f0b0d365d3d643744544642d9f9488e18a
workflow-type: tm+mt
source-wordcount: '890'
ht-degree: 37%

---


# Gainsight PX 接続 {#gainsight-px}

## 概要 {#overview}

[[!DNL Gainsight PX]](https://www.gainsight.com/product-experience/) は、製品チームが製品の使用方法を理解し、フィードバックを収集し、製品のチュートリアルなどのアプリ内エンゲージメントを作成して、ユーザーのオンボーディングと製品の導入を促進できる製品エクスペリエンスプラットフォームです。

>[!IMPORTANT]
>
>宛先コネクタとドキュメントページは、 *Gainsight PX* チーム。 お問い合わせや更新のご依頼は、 *`pxsupport@gainsight.com`*.

## ユースケース {#use-cases}

をいつどのように使用するかをより深く理解するのに役立ちます。 *Gainsight PX* の宛先について、Adobe Experience Platformのお客様がこの宛先を使用して解決できる使用例を以下に示します。

### アプリ内エンゲージメントのターゲティング {#targeting-in-app-engagements}

ある SaaS 会社は、Gainsight PX 上に構築されたアプリ内ガイドを通じて顧客との関わりを高めたいと考えています。 このエンゲージメントを受け取るオーディエンスは、Adobe Experience Platform上に構築されています。 Gainsight PX の宛先は、オーディエンスを受け取り、Gainsight PX 環境内で使用できるようにします。

## 前提条件 {#prerequisites}

* 次の項目に連絡： [!DNL Gainsight] サポートチームに問い合わせ、サブスクリプション用の外部セグメント機能のアクティベーションをリクエストします。
* を使用して、PX サブスクリプションの OAuth Secret 値を生成します。 **[!UICONTROL 新しい秘密鍵を生成]** ボタン [会社の詳細ページ](https://app.aptrinsic.com/settings/subscription)
  ![Gainsight PX の会社の詳細画面に「新しい秘密鍵を生成」ボタンが表示される](../../assets/catalog/analytics/gainsight-px/generate_oauth_secret.png)

## サポートされている ID {#supported-identities}

Gainsight PX では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](../../../identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 |
|---|----|
| IdentifyID | Gainsight PX とAdobe Experience Platformでユーザーを一意に識別する共通のユーザー識別子 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの起源 | サポートあり | 説明 |
|---|---|---|
| [!DNL Segmentation Service] | ✓ | Experience Platform [セグメント化サービス](../../../segmentation/home.md). |
| カスタムアップロード | X | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/overview.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---|---|---|
| 書き出しタイプ | **[!UICONTROL セグメントの書き出し]** | [!DNL Gainsight PX] 宛先で使用される識別子（氏名、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンスの評価に基づいてExperience Platformでプロファイルが更新されると、コネクタは更新を宛先プラットフォームに送信します。 詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

![認証のスクリーンショット](../../assets/catalog/analytics/gainsight-px/auth-screen.png)

* **[!UICONTROL パスワード]**：へのログインに使用するパスワード [[!DNL Gainsight PX]](https://app.aptrinsic.com)
* **[!UICONTROL クライアント ID]**: [会社の詳細ページ](https://app.aptrinsic.com/settings/subscription)
* **[!UICONTROL クライアント秘密鍵]**：の下部で生成される OAuth 秘密鍵 [会社の詳細ページ](https://app.aptrinsic.com/settings/subscription) （内） [!DNL Gainsight PX] UI
* **[!UICONTROL ユーザー名]**：へのログインに使用する電子メール [[!DNL Gainsight PX]](https://app.aptrinsic.com) UI

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![「名前」フィールドと「説明」フィールドの入力方法を示す、Experience Platformユーザーインターフェイスの宛先の詳細画面](../../assets/catalog/analytics/gainsight-px/destination_details.png)

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
>
>* データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]**[に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。
>* 書き出す *id*、 **[!UICONTROL ID グラフを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png "ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。"){width="100" zoomable="yes"}

この宛先にオーディエンスセグメントをアクティベートする手順は、[ストリーミングセグメントの書き出し宛先へのプロファイルとセグメントのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### ID のマッピング {#map}

この宛先は、プロファイル属性と ID 名前空間のマッピングをサポートします。 ターゲットマッピングは常に **[!UICONTROL IDENTIFY_ID]** id 名前空間。

マッピングの設定方法をより深く理解するには、以下の例を参照してください。

#### プロファイル属性のマッピング {#map-profile-attribute}

以下の例では、source フィールドは IDENTIFY_ID ターゲット名前空間にマッピングされる XDM プロファイル属性です。

![ID 名前空間の例マッピング画面で、ソースとターゲットの値を選択する方法を示します](../../assets/catalog/analytics/gainsight-px/mapping_attribute.png)

#### ID 名前空間のマッピング {#map-identity-namespace}

以下の例では、ソースフィールドは ID 名前空間 (**[!UICONTROL ECID]**) は、 **[!UICONTROL IDENTIFY_ID]** ターゲット名前空間。

![ソースとターゲットの値を選択する方法を示す属性のサンプルマッピング画面](../../assets/catalog/analytics/gainsight-px/mapping_identities.png)

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

セグメント化データは、Experience Platformから Gainsight PX にストリーミングされます。

セグメントメタデータは、 [!DNL Gainsight PX] UI

![Gainsight PX のセグメントリスト画面で、外部セグメントを表示。](../../assets/catalog/analytics/gainsight-px/segment_metadata.png)

セグメントメンバーシップ情報は、Audience Explorer 画面の「セグメント」タブに表示されます。 [!DNL Gainsight PX] UI

![Gainsight PX の Audience Explorer 画面に、ユーザーに関連するセグメントが表示される。](../../assets/catalog/analytics/gainsight-px/PX_Segments.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
