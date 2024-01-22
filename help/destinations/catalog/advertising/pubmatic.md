---
title: PubMatic Connect
description: PubMatic は、将来のプログラム的なデジタルマーケティングサプライチェーンを提供することで、顧客価値を最大化します。 PubMatic Connect は、プラットフォームテクノロジーと専用サービスを組み合わせ、インベントリとデータのパッケージ化とトランザクションの方法を強化します。
last-substantial-update: 2023-12-14T00:00:00Z
source-git-commit: c3ef732ee82f6c0d56e89e421da0efc4fbea2c17
workflow-type: tm+mt
source-wordcount: '923'
ht-degree: 43%

---


# PubMatic 接続先 {#pubmatic-connect}

## 概要 {#overview}

用途 [!DNL PubMatic Connect] 将来のプログラム的なデジタルマーケティングサプライチェーンを提供することで、顧客価値を最大化する。 [!DNL PubMatic Connect] プラットフォームテクノロジーと専用サービスを組み合わせ、インベントリとデータのパッケージ化とトランザクションの方法を強化します。

この宛先を使用して、オーディエンスデータをに送信します。 [!DNL PubMatic Connect] プラットフォーム。

>[!IMPORTANT]
>
>宛先コネクタとドキュメントページは、 [!DNL PubMatic] チーム。 お問い合わせや更新のご依頼は、 `support@pubmatic.com`.

## ユースケース {#use-cases}

[!DNL PubMatic Connect] 宛先を使用する方法とタイミングを理解しやすくするために、Adobe Experience Platform のお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

### モバイル、Web および CTV プラットフォームでのユーザーのターゲティング {#targeting}

パブリッシャーまたはデータプロバイダーが、Adobe Experience Platformからにオーディエンスを送信する [!DNL PubMatic Connect] 様々な識別子を使用して、モバイル、web および CTV プラットフォーム上のユーザーをターゲットに設定する。

## 前提条件 {#prerequisites}

お問い合わせ [!DNL PubMatic] アカウントが正しく設定されていることをアカウントマネージャーに確認して、オーディエンスセグメントのオンボーディングをサポートします。 また、この宛先を使用するためのすべての関連情報が揃っていることを確認し、セットアップ中にサポートを提供します。

## サポートされている ID {#supported-identities}

[!DNL PubMatic Connect] では、以下の表で説明する id のアクティブ化をサポートしています。 [ID](/help/identity-service/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
| --------------- | ------ | --- |
| GAID | Google Advertising ID | ソース ID が GAID 名前空間の場合は、GAID ターゲット ID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、IDFA ターゲット ID を選択します。 |
| extern_id | カスタムユーザー ID | ソース ID がカスタム名前空間の場合は、このターゲット ID を選択します。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの起源 | サポートあり | 説明 |
| --- | --------- | ------ |
| [!DNL Segmentation Service] | ✓ | Experience Platform [セグメント化サービス](../../../segmentation/home.md). |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/overview.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
| --- | --- | --- |
| 書き出しタイプ | **[!UICONTROL セグメントの書き出し]** | PubMatic Connect の宛先で使用される識別子（名前、電話番号など）を使用して、セグメント（オーディエンス）のすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。セグメントの評価に基づいてExperience Platformでプロファイルが更新されると、コネクタは更新を宛先プラットフォームに送信します。 詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
> 宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

![認証方法](../../assets/catalog/advertising/pubmatic/authenticate-destination.png)

- **[!UICONTROL ベアラートークン]**：宛先を認証するためのベアラートークンを入力します。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![宛先の詳細](../../assets/catalog/advertising/pubmatic/destination-details.png)

- **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
- **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
- **[!UICONTROL データパートナー ID]**: [!DNL PubMatic] この統合のアカウント。
- **[!UICONTROL デフォルトの国コード]**：プロファイルに何も指定されていない場合にすべての ID に適用する必要があるデフォルトの国コード。
- **[!UICONTROL アカウント ID]**: [!DNL PubMatic Connect] アカウント ID。
- **[!UICONTROL アカウントタイプ]**: [!DNL PubMatic] プラットフォームアカウント。 お問い合わせ [!DNL PubMatic] どの項目を選択するかについて質問がある場合は、アカウントマネージャー。 使用できる選択肢は次のとおりです。
   - [!UICONTROL 投稿者]
   - [!UICONTROL DEMAND_PARTNER]
   - [!UICONTROL 購入者]

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
>
> - データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>
> - 書き出す _id_、 **[!UICONTROL ID グラフを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。](../../assets/overview/export-identities-to-destination.png "ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。"){width="100" zoomable="yes"}

この宛先にオーディエンスセグメントをアクティベートする手順は、[ストリーミングセグメントの書き出し宛先へのプロファイルとセグメントのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### 属性と ID のマッピング {#map}

ソースフィールドを選択しています。

- 識別子を選択します ( 通常、名前空間（IDFA やカスタム ID 名前空間など）。

ターゲットフィールドの選択：

- お問い合わせ [!DNL PubMatic] この手順で正しい UID タイプに関する情報を取得するには、アカウントマネージャー。
- を選択します。 [!DNL PubMatic UID] 最初の手順で選択した識別子に一致する番号を入力します。

![属性と ID のマッピング](../..//assets/catalog/advertising/pubmatic/export-identities-to-destination.png)

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

The [!DNL PubMatic] UI では、データが正しくプッシュされたかどうか、およびセグメントが使用可能かどうかを確認できます。 データが [!DNL PubMatic] UI を更新します。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
