---
title: インデックス交換
description: インデックス交換（インデックス）に接続し、データをアクティブ化して、インデックス UI で作成された取引でオーディエンスセグメントをターゲットにできるようにします。
source-git-commit: 4ecd3b60a6b45a94285785049fd4dee99d7c9bdf
workflow-type: tm+mt
source-wordcount: '1083'
ht-degree: 17%

---


# [!DNL Index Exchange] {#index-exchange}

## 概要 {#overview}

[!DNL Index] は、メディア所有者が全画面にわたってコンテンツの価値を最大化するのに役立つ、グローバル広告のサプライサイドのプラットフォームです。 20 年以上にわたる業界のリーダーシップを持つ [!DNL Index] は、世界最大のブランドとプレミアムなエクスペリエンスメーカーを結び付け、高品質の消費者体験を提供します。

この宛先コネクタを使用すると、オーディエンスセグメントをAdobe Experience Platformから [!DNL Index Exchange] のプログラム広告プラットフォームに直接書き出すことができます。

書き出すと、これらのオーディエンスセグメントを使用して、メディア所有者やマーケットプレイスのパートナーが取引をターゲットにしたり、マーケットプレイスのベンダーがパブリッシャーやキュレーターと共有したりできます。

>[!IMPORTANT]
>
>宛先コネクタとドキュメントページは、[!DNL Index] チームが作成および管理します。 ご質問や更新のリクエストについては、[technical_am_marketplace@indexexchange.com](mailto:technical_am_marketplace@indexexchange.com) まで直接お問い合わせください。

## ユースケース {#use-cases}

[!DNL Index Exchange] の宛先を使用する方法とタイミングをより深く理解するために、Experience Platformのお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

### モバイル、web および CTV プラットフォームでのユーザーのターゲティング {#targeting-users}

Experience Platformから [!DNL Index] にオーディエンスを送信し、モバイル、web、CTV プラットフォームのユーザーをターゲットにするのに、様々な識別情報を使用したいと考えているメディアオーナー、マーケットプレイスのパートナー、マーケットプレイスのベンダーです。

### モバイル、web および CTV プラットフォームでの特定のコンテンツのターゲティング {#targeting-content}

Experience Platformから [!DNL Index] にオーディエンスを送り、特定の URL、アプリバンドル、コンテンツ ID を使用して、モバイル、web、CTV プラットフォーム全体で特定のコンテンツを表示したいメディアオーナー、マーケットプレイスのパートナー、マーケットプレイスのベンダー。

## 前提条件 {#prerequisites}

オーディエンスセグメントをアカウントに表示するには、この宛先を使用する際に追加のプロセスを使用して、[!DNL Index] に登録する必要があります。 このプロセスに関するサポートについては、[!DNL Index Exchange] アカウント担当者にお問い合わせください。

## サポートされている ID {#supported-identities}

[!DNL Index] では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

[!DNL Index Exchange] の宛先では、1 つのアップロードで 1 つの ID タイプのみをサポートすることに注意してください。 宛先の詳細を設定する際に、適切な識別子タイプを指定する必要があります（以下の [&#x200B; 宛先の詳細の入力」 &#x200B;](#destination-details) 節を参照）。

複数の ID タイプをアップロードするには、[!DNL Index Exchange] の宛先のインスタンスを ID タイプごとに個別に作成します。

| ターゲット ID | 説明 | 注意点 |
| --- | --- | --- |
| GAID | GOOGLE ADVERTISING ID | ソース ID が GAID 名前空間の場合、ターゲット ID として GAID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、IDFA ターゲット ID を選択します。 |
| Windows AID | Windows Advertising ID | ソース ID が Windows AID 名前空間の場合は、Windows AID のターゲット ID を選択します。 |
| extern_id | カスタムユーザー ID | ソース ID がカスタム名前空間の場合は、このターゲット ID を選択します。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
| --------- | ---------- | ---------- |
| [!DNL Segmentation Service] | ✓ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/overview.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
| --------- | ---------- | --------- | 
| 書き出しタイプ | **[!UICONTROL Segment export]** | [!DNL Index Exchange] 宛先で使用される識別子（IDFA、GAID など）を使用して、セグメント（オーディエンス）のすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Batch]** | 3 時間、6 時間、8 時間、12 時間、24 時間の間隔でダウンストリームプラットフォームにファイルを書き出します。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Manage Destinations]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先の詳細の入力 {#destination-details}

宛先の詳細を設定するには、以下のフィールドを入力します。 UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![&#x200B; 宛先の詳細 &#x200B;](../../assets/catalog/advertising/index-exchange/destination-details.png)

* [!UICONTROL Name]：この宛先を後で認識できるように、名前を入力します。
* [!UICONTROL Description]：後でこの宛先を識別するのに役立つ説明を入力します。
* [!UICONTROL Identifier Type]: [!DNL Index] に送信する識別子に一致する、インデックス提供の識別子タイプを選択します。 以下のサポートされる識別子タイプの表を参照してください。 使用する ID タイプが不明な場合は、[!DNL Index] 担当者にお問い合わせください。 複数の識別子タイプを送信するには、この宛先のインスタンスを個別に作成します。
* [!UICONTROL Account ID]: [!DNL Index] アカウント ID を入力します。 これは、パブリッシャー ID とは異なります。 使用する ID が不明な場合は、[!DNL Index] 担当者にお問い合わせください。

#### サポートされる識別子タイプ

| 識別子タイプ | 説明 |
|------------------ | ------------- |
| [!DNL appbundle] | モバイルアプリバンドル |
| [!DNL contentid] | コンテンツ ID |
| [!DNL deviceid] | デバイス ID （例： IDFA、GAID、WAID など） |
| [!DNL ip] | IP アドレス |
| [!DNL postcode] | 郵便番号 |
| [!DNL url] | サイト URL |
| [!DNL ppid_xxx] | PPID の識別子については、[!DNL Index Exchange] アカウント担当者にお問い合わせください。 |

{style="table-layout:auto"}

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、この宛先へのデータフローのステータスに関する通知を受け取ることができます。 リストから 1 つ以上のアラートを選択して、データフローのステータス通知を登録します。 詳しくは、[UI を使用した宛先アラートの購読 &#x200B;](../../ui/alerts.md) についてのガイドを参照してください。
宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。<br> ![&#x200B; 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、[バッチプロファイル書き出し宛先に対するオーディエンスデータのアクティブ化](/help/destinations/ui/activate-batch-profile-destinations.md)を参照してください。

### 属性と ID のマッピング {#map}

ソースフィールドを選択中：

* 識別子（通常は IDFA やカスタム ID 名前空間などの名前空間）を選択します。 宛先の設定時に選択した識別子タイプに対応している必要があります。 例えば、IDFA 識別子をソースフィールドとして使用する場合、宛先は、「deviceid」識別子タイプで設定されている必要があります。

ターゲットフィールドを選択：

* ターゲットフィールドの名前は無視され、重要ではありません。 宛先は、送信される識別子のタイプにのみ関係します。これは、宛先を設定する際に選択した識別子タイプによって決まります。

![&#x200B; 属性と ID のマッピング &#x200B;](../../assets/catalog/advertising/index-exchange/identity-mapping.png)

### [!DNL Index] へのセグメントの登録 {#register-segments}

宛先に対してデータをアクティブ化する前または後に、[!DNL Index] 担当者に連絡して、アクティブ化するセグメントを登録してください。 名前、ID、説明、価格（該当する場合）など、追加のセグメントの詳細を登録する方法については、担当者から指示があります。

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

登録が完了すると、セグメントは [!DNL Index] アカウントでターゲティングに使用できるようになります。 データが正しく受信されていることを確認するには、受信したセグメントデータの量の詳細を提供できる [!DNL Index] 担当者にお問い合わせください。

## データの使用とガバナンス {#data-usage-governance}

Experience Platformのすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。 Experience Platformによるデータガバナンスの実施方法について詳しくは、[&#x200B; データガバナンスの概要 &#x200B;](/help/data-governance/home.md) を参照してください。
