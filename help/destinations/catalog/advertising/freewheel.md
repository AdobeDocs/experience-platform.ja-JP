---
title: FreeWheel接続
description: コネクテッド TV、ディスプレイ、ビデオインベントリをまたいで、Adobe Experience PlatformからFreeWheelにオーディエンスをアクティベートしてプログラマティック広告を行う方法について説明します。
hide: true
hidefromtoc: true
badge: label="ベータ版" type="Informative"
exl-id: 1f1d3e57-a8ef-4971-b3d1-43521bd158bb
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '1525'
ht-degree: 25%

---

# [!DNL FreeWheel] 接続 {#freewheel}

>[!AVAILABILITY]
>
>[!DNL FreeWheel]の宛先は現在Betaにあり、一部のお客様のみが利用できます。 アクセスをリクエストするには、Adobe担当者にお問い合わせください。

## 概要 {#overview}

[!DNL FreeWheel]は、コネクテッド TV （CTV）、ビデオ、ディスプレイ広告在庫をまたいでプログラマティックな売買を可能にするグローバルな広告テクノロジープラットフォームです。 [!DNL FreeWheel]は、広告主と世界中のプレミアムメディア所有者を結びつけるデータ主導型のマーケットプレイスを提供しています。

この宛先を使用して、[!DNL Adobe Experience Platform]から[!DNL FreeWheel]までのオーディエンスを送信します。 オーディエンスは毎日のバッチファイルとして配信され、[!DNL FreeWheel]件の案件とキャンペーンでターゲティングするために利用できるようになります。

## 前提条件 {#prerequisites}

[!DNL FreeWheel]へのオーディエンスをアクティブ化する前に、次の要件を確認してください。

* **FreeWheel ネットワーク ID**：有効な[!DNL FreeWheel] ネットワーク IDが必要です。 これは、アカウントの設定時に[!DNL FreeWheel]によって提供されます。

## サポートされている ID {#supported-identities}

[!DNL FreeWheel]は、次の表に示すIDのアクティブ化をサポートしています。 これらのIDに加えて、[!DNL FreeWheel] アカウントで使用可能な任意のIDを使用できます。 以下の表に記載されていないIDをマッピングする方法については、[属性とIDのマッピング ](#map)を参照してください。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| `idfa` | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、このターゲット ID を選択します。 |
| `aaid` | ANDROID ADVERTISING ID | ソース ID が GAID 名前空間の場合は、このターゲット ID を選択します。 |
| `ctv` | コネクテッド TV デバイス ID | CTV デバイスをターゲティングする場合は、このターゲット IDを選択します。 |
| `ip` | IPv4 アドレス | このターゲット IDを選択して、IP アドレスに基づいてユーザーをターゲティングします。 有効なIPv4 アドレスを含むプロファイル属性をマッピングするか、計算フィールドを使用して値を導き出します。 |
| `ipv6` | IPv6 アドレス | このターゲット IDを選択して、IPv6 アドレスに基づいてユーザーをターゲティングします。 有効なIPv6 アドレスを含むプロファイル属性をマッピングするか、計算フィールドを使用して値を導き出します。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの由来 | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ○ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md)を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの生成元 | ○ | このカテゴリには、[!DNL Segmentation Service]を通じて生成されたオーディエンス以外のすべてのオーディエンスのオリジンが含まれます。 [様々なオーディエンスの起源](/help/segmentation/ui/audience-portal.md#customize)について読みます。 次に例を示します。 <ul><li>カスタムアップロードオーディエンス [がCSV ファイルからExperience Platformに](../../../segmentation/ui/audience-portal.md#import-audience)をインポートしました。</li><li>類似オーディエンス，</li><li>連合オーディエンス，</li><li>[!DNL Adobe Journey Optimizer]などの他のExperience Platform アプリで生成されたオーディエンス</li><li>その他。</li></ul> |

{style="table-layout:auto"}

オーディエンスのデータタイプ別にサポートされるオーディエンス：

| オーディエンスのデータタイプ | サポートあり | 説明 | ユースケース |
|--------------------|-----------|-------------|-----------|
| [人物オーディエンス ](/help/segmentation/types/people-audiences.md) | ○ | 顧客プロファイルにもとづいて、マーケティング施策の特定のグループをターゲットにすることができます。 | CTV リターゲティング，リーチ抑制 |
| [ アカウントオーディエンス ](/help/segmentation/types/account-audiences.md) | × | アカウントベースドマーケティング戦略のために、特定の組織内の個人をターゲットにします。 | B2B マーケティング |
| [見込みオーディエンス ](/help/segmentation/types/prospect-audiences.md) | × | まだ顧客ではないが、ターゲットオーディエンスと特徴を共有する個人をターゲットにします。 | サードパーティデータによる見込み顧客の開拓 |
| [ データセットの書き出し](/help/catalog/datasets/overview.md) | × | [!DNL Adobe Experience Platform] データ レイクに保存されている構造化データのコレクション。 | レポート，データサイエンスワークフロー |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Profile-based]** | オーディエンスのすべてのメンバーを、目的のID フィールドと共に、[宛先アクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes)のマッピング手順で選択した値で書き出します。 |
| 書き出し頻度 | **[!UICONTROL Batch]** | 最初の書き出しは、アクティブ化されたオーディエンスに適格なすべてのプロファイルの完全なスナップショットです。 その後の書き出しは、新しいオーディエンスの選定（追加）とオーディエンスの終了（削除）を含む日次の増分更新です。 設定可能な完全なオーディエンス更新間隔（4、8または12週間）も使用でき、日次の増分に加えて定期的な完全な書き出しをトリガーします。 完全な書き出しには、現在選定されているプロファイルのみが含まれます。 オーディエンスの終了は含まれず、毎日の増分更新を通じてのみ配信されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage Destinations]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

[!DNL FreeWheel]宛先への認証は、Adobeによって自動的に処理されます。 認証時に資格情報やAPI キーは必要ありません。 Adobeは、お客様の代わりに[!DNL FreeWheel]への安全な接続を管理します。

FreeWheel宛先の認証手順の![ スクリーンショット。](../../assets/catalog/advertising/freewheel/connect-destination.png)

**[!UICONTROL Connect to destination]**&#x200B;を選択して、宛先の詳細ステップに進みます。

### 宛先の詳細を入力 {#destination-details}

>[!CONTEXTUALHELP]
>id="platform_destinations_freewheel_backfill"
>title="完全なオーディエンスの更新間隔"
>abstract="毎日の増分更新に加えて、完全なオーディエンスの書き出しが [!DNL FreeWheel] に送信される間隔を選択します。 完全なオーディエンスの書き出しを使用すると、オーディエンスメンバーの有効期限が [!DNL FreeWheel] に切れるのを防ぐので、キャンペーンの実行中にターゲットメンバーの急激な減少が発生することはありません。 4 週間、8 週間、12 週間から選択できます。"

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

FreeWheelの宛先の詳細を入力する方法を示す![ サンプルのスクリーンショット。](../../assets/catalog/advertising/freewheel/destination-details.png)

* **[!UICONTROL Name]**：今後この宛先を認識する際に使用する名前。
* **[!UICONTROL Description]**：今後この宛先を特定するのに役立つ説明です。
* **[!UICONTROL Region]**: アカウントがホストされている[!DNL FreeWheel]地域。 次のいずれかのオプションを選択します。
   * **[!UICONTROL US East]**
   * **[!UICONTROL Europe]**
   * **[!UICONTROL Asia Pacific]**
* **[!UICONTROL FreeWheel network ID]**: [!DNL FreeWheel] ネットワーク ID。 この値は[!DNL FreeWheel]によって提供され、[!DNL FreeWheel] プラットフォーム内の組織を一意に識別します。
* **[!UICONTROL Full audience refresh interval]**：毎日の増分更新に加えて、完全なオーディエンス書き出しが[!DNL FreeWheel]に送信される頻度。 完全なオーディエンスの書き出しを使用すると、オーディエンスメンバーの有効期限が [!DNL FreeWheel] に切れるのを防ぐので、キャンペーンの実行中にターゲットメンバーの急激な減少が発生することはありません。 ドロップダウンから間隔を選択します。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の提供が完了したら、**[!UICONTROL Next]**&#x200B;を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID*&#x200B;をエクスポートするには、**[!UICONTROL View Identity Graph]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。<br> ![ ワークフローで強調表示されているID名前空間を選択して、オーディエンスを宛先にアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png " ワークフローで強調表示されたID名前空間を選択して、オーディエンスを宛先にアクティブ化します。"){width="100" zoomable="yes"}

この宛先に対してオーディエンスをアクティブ化する手順については、[バッチプロファイル書き出し宛先に対するオーディエンスデータのアクティブ化](/help/destinations/ui/activate-batch-profile-destinations.md)を参照してください。

### オーディエンスの書き出しをスケジュール {#schedule}

FreeWheel アクティベーション ワークフローのスケジュール設定ステップの![ スクリーンショット。](../../assets/catalog/advertising/freewheel/scheduling.png)

**[!UICONTROL Scheduling]** ステップで、各オーディエンスの書き出しスケジュールを設定します。 [!DNL FreeWheel]はハイブリッド書き出しモデルを使用しています。アクティブ化された各オーディエンスの最初の書き出しは完全なスナップショットで、その後、毎日の増分更新が続きます。

次のフィールドを設定します。

* **[!UICONTROL File export options]**: **[!UICONTROL Export incremental files]**&#x200B;は事前に選択されており、サポートされている唯一のオプションです。 最初の書き出しには、すべての適格プロファイルの完全なスナップショットが自動的に含まれます。 その後の書き出しでは、前回の書き出し以降、新しいオーディエンスの選定と離脱のみが提供されます。
* **[!UICONTROL Frequency]**: **[!UICONTROL Daily]**&#x200B;を選択します。 [!DNL FreeWheel]は、毎日の増分ファイル配信を期待しています。
* **[!UICONTROL Scheduled start time]**：日次エクスポートを実行するUTCの時間を入力します。
* **[!UICONTROL Date]**: アクティベーションの開始日と終了日を設定します。 開始日は、最初の完全なスナップショットエクスポートがいつ送信されるかを決定します。

>[!NOTE]
>
>フル書き出し（初期スナップショットと定期的なフル更新の両方）には、現在選定されているプロファイルのみが含まれます。 オーディエンスの終了は完全な書き出しに含まれず、毎日の増分更新を通じてのみ配信されます。

### 属性と ID のマッピング {#map}

マッピング手順で、Experience Platform プロファイルからソースフィールドを選択し、[!DNL FreeWheel]でサポートされているID タイプにマッピングします。 少なくとも1つのマッピングが必要です。

>[!IMPORTANT]
>
>[!DNL FreeWheel]でサポートされているID タイプは、ID名前空間ではなく、マッピング UIで&#x200B;**ターゲット属性**&#x200B;として表示されます。

[!DNL FreeWheel] アカウントが[ サポートされているID](#supported-identities) テーブルにリストされていないID タイプをサポートしている場合は、定義済みのリストから選択するのではなく、ターゲットフィールドにID名を手動で入力して、ID タイプにマッピングできます。

マッピング手順でターゲット フィールドに直接入力されたカスタム ID名を示す![ スクリーンショット。](../../assets/catalog/advertising/freewheel/custom-identity.png)

次に、マッピングの例を示します。 実際のマッピングは、プロファイルスキーマと[!DNL FreeWheel] アカウントがサポートするID タイプによって異なります。

| ソースフィールド | ターゲットフィールド |
| --- | --- |
| `identityMap.IDFA` | `idfa` |
| `identityMap.GAID` | `aaid` |
| `homeAddress.ipAddress` | `ip` |

{style="table-layout:auto"}

>[!NOTE]
>
>必須マッピングは適用されません。 ただし、少なくとも1つの有効なID マッピングを持たないプロファイルは、書き出されたファイルに含まれません。

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

[!DNL FreeWheel]は、書き出しごとに2種類のファイルを受け取ります。 両方のファイルタイプが自動的に生成され、配信されます。 お客様の側で操作は必要ありません。

**ID （データ）ファイル**&#x200B;には、オーディエンスメンバーシップデータが含まれています。 各行は、ユーザーIDを1つ以上のオーディエンス IDにマッピングします。 ファイルは、列ヘッダーなしでCSV形式で[!DNL FreeWheel]に配信されます。 書き出しに存在するID タイプごとに個別のファイルが生成されます（例えば、`aaid`には1つのファイル、`idfa`には1つのファイル）。

データファイル形式の例：

```csv
aebc1234-56f7-89ab-cdef-0123456789ab,segment_1,segment_2
f7c9a8b0-4d33-11ec-81d3-0242ac130003,segment_1,segment_3
123e4567-e89b-12d3-a456-426614174000,segment_2
```

**分類ファイル**&#x200B;は、書き出しに含まれるオーディエンスを記述します。 これらのファイルはデータファイルとともに配信され、オーディエンス ID、名前、TTL （有効期間）が数日で含まれます。 [!DNL FreeWheel]がサポートするTTLの最大値は90日です。 以下の例の値は実例です。

分類ファイル形式の例：

```csv
Segment ID,Segment Name,TTL
segment_1,my_first_segment,30
segment_2,my_second_segment,30
segment_3,my_third_segment,30
```

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

[!DNL FreeWheel]とその広告技術プラットフォームについて詳しくは、[FreeWheel web サイト ](https://www.freewheel.com){target="_blank"}を参照してください。
