---
title: PubMatic 接続
description: PubMatic は、将来のプログラムによるデジタルマーケティングのサプライチェーンを提供することで、顧客価値を最大化します。 PubMatic Connect は、プラットフォーム技術と専用サービスを組み合わせて、在庫とデータのパッケージ化および取引方法を強化します。
last-substantial-update: 2023-12-14T00:00:00Z
exl-id: 21e07d2c-9a6a-4cfa-a4b8-7ca48613956c
source-git-commit: c35b43654d31f0f112258e577a1bb95e72f0a971
workflow-type: tm+mt
source-wordcount: '923'
ht-degree: 43%

---

# PubMatic Connect の宛先 {#pubmatic-connect}

## 概要 {#overview}

使用方法 [!DNL PubMatic Connect] 将来のプログラム的なデジタルマーケティングサプライチェーンを提供することで、顧客価値を最大化する。 [!DNL PubMatic Connect] は、プラットフォームテクノロジーと専用サービスを組み合わせて、在庫とデータのパッケージ化および取引方法を強化します。

この宛先を使用して、オーディエンスデータをに送信します [!DNL PubMatic Connect] プラットフォーム。

>[!IMPORTANT]
>
>宛先コネクタとドキュメントページは、によって作成および管理されます。 [!DNL PubMatic] チーム。 お問い合わせや更新のリクエストについては、 `support@pubmatic.com`.

## ユースケース {#use-cases}

[!DNL PubMatic Connect] 宛先を使用する方法とタイミングを理解しやすくするために、Adobe Experience Platform のお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

### モバイル、web および CTV プラットフォームでのユーザーのターゲティング {#targeting}

公開者またはデータプロバイダーが、オーディエンスをAdobe Experience Platformからに送信するとします。 [!DNL PubMatic Connect] モバイル、Web および CTV プラットフォーム上で広範囲の識別子を使用してユーザーをターゲットにします。

## 前提条件 {#prerequisites}

に問い合わせる [!DNL PubMatic] アカウントマネージャー：アカウントが正しく設定され、オーディエンスセグメントのオンボーディングをサポートしていることを確認します。 また、この宛先を使用するための関連する詳細がすべて用意され、設定中にサポートが提供されます。

## サポートされている ID {#supported-identities}

[!DNL PubMatic Connect] では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
| --------------- | ------ | --- |
| GAID | GOOGLE ADVERTISING ID | ソース ID が GAID 名前空間の場合は、GAID ターゲット ID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、IDFA ターゲット ID を選択します。 |
| extern_id | カスタムユーザー ID | ソース ID がカスタム名前空間の場合は、このターゲット ID を選択します。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
| --- | --------- | ------ |
| [!DNL Segmentation Service] | ✓ | Experience Platformを通じて生成されたオーディエンス [セグメント化サービス](../../../segmentation/home.md). |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
| --- | --- | --- |
| 書き出しタイプ | **[!UICONTROL セグメントの書き出し]** | PubMatic Connect 宛先で使用される識別子（名前、電話番号など）を使用して、セグメント（オーディエンス）のすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。セグメント評価に基づいてExperience Platform内でプロファイルが更新されると、コネクタは更新を宛先プラットフォームに送信します。 詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

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
- **[!UICONTROL データパートナー ID]**：で設定されたデータパートナー ID [!DNL PubMatic] この統合用のアカウント。
- **[!UICONTROL デフォルトの国コード]**：プロファイルで指定されていない場合にすべての ID に適用されるデフォルトの国コード。
- **[!UICONTROL アカウント ID]**：あなたの [!DNL PubMatic Connect] アカウント ID。
- **[!UICONTROL アカウントタイプ]**：のアカウントタイプ [!DNL PubMatic] platform アカウント。 に問い合わせる [!DNL PubMatic] アカウントマネージャー（ご質問がある場合） 使用できる選択肢は次のとおりです。
   - [!UICONTROL 公開者]
   - [!UICONTROL DEMAND_PARTNER]
   - [!UICONTROL 購入者]

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
>
> - データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>
> - エクスポートする _id_、が必要です **[!UICONTROL ID グラフの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![宛先に対してオーディエンスをアクティブ化するために、ワークフローで強調表示されている ID 名前空間を選択します。](../../assets/overview/export-identities-to-destination.png "宛先に対してオーディエンスをアクティブ化するために、ワークフローで強調表示されている ID 名前空間を選択します。"){width="100" zoomable="yes"}

この宛先にオーディエンスセグメントをアクティベートする手順は、[ストリーミングセグメントの書き出し宛先へのプロファイルとセグメントのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### 属性と ID のマッピング {#map}

ソースフィールドを選択中：

- 識別子（通常は IDFA やカスタム ID 名前空間などの名前空間）を選択します。

ターゲットフィールドを選択：

- に問い合わせる [!DNL PubMatic] アカウントマネージャー：この手順で UID タイプが正しいかに関する情報を取得します。
- 「」を選択します [!DNL PubMatic UID] 最初の手順で選択した識別子に一致する番号を入力します。

![属性と ID のマッピング](../..//assets/catalog/advertising/pubmatic/export-identities-to-destination.png)

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

この [!DNL PubMatic] UI を使用すると、データが正しくプッシュされているかどうか、およびセグメントが使用可能かどうかを確認できます。 にデータがプッシュされてから最大 24 時間かかる場合があります [!DNL PubMatic] 更新する UI。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
