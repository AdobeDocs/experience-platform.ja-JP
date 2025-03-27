---
title: Snowflake接続
description: 非公開リストを使用して、Snowflake アカウントにデータを書き出します。
hide: true
hidefromtoc: true
badgeBeta: label="ベータ版" type="Informative"
source-git-commit: 2893356e36fde4759ffe76e01296fcb8811ab8e7
workflow-type: tm+mt
source-wordcount: '675'
ht-degree: 51%

---


# Snowflake接続 {#snowflake-destination}

>[!IMPORTANT]
>
>この宛先コネクタはベータ版で、一部のお客様のみご利用いただけます。 アクセス権をリクエストするには、Adobe担当者にお問い合わせください。

## 概要 {#overview}

非公開リストを使用して、Snowflake アカウントにデータを書き出します。

## 前提条件 {#prerequisites}

Snowflake接続を設定する前に、次の前提条件を満たしていることを確認してください。

* [!DNL Snowflake] アカウントにアクセスできます。
* お使いのSnowflake アカウントは非公開リストに登録されています。 自分または社内でSnowflakeに対するアカウント管理者権限を持つユーザーがこの機能を設定できます。

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | [!DNL Snowflake] 宛先で使用される識別子（氏名、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先を認証するには、「**[!UICONTROL 宛先に接続]**」を選択します。

![ 宛先への認証方法を示すサンプルスクリーンショット ](../../assets/catalog/cloud-storage/snowflake/authenticate-destination.png)

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![ 宛先の詳細を入力する方法を示すサンプルスクリーンショット ](../../assets/catalog/cloud-storage/snowflake/configure-destination-details.png)

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL Snowflake アカウント ID]**：お使いのSnowflake アカウント ID。 例：`adobe-123456`。
* **[!UICONTROL アカウントの確認]**:「Snowflake アカウント ID の確認」をオンにして、アカウント ID が正しく、自分に属していることを確認します。

>[!IMPORTANT]
>
> 宛先名とExperience Platform サンドボックス名で使用される特殊文字は、Snowflakeではアンダースコア（`_`）に自動変換されます。 混乱を避けるために、宛先とサンドボックス名には特殊文字を使用しないでください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)に関するガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### 属性のマッピング {#map}

Snowflakeの宛先では、プロファイル属性とカスタム属性とのマッピングをサポートしています。

![Snowflakeの宛先のマッピング画面を示す Platform ユーザーインターフェイス画像。](../../assets/catalog/cloud-storage/snowflake/mapping.png)

ターゲット属性は、「**[!UICONTROL 属性名]** フィールドに指定した属性名を使用して、Snowflakeで自動的に作成されます。

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

データが正しく書き出されたことをSnowflake アカウントで確認します。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。