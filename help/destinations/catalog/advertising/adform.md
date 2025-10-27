---
title: Adform
description: Adform は、プログラムによるメディア売買ソリューションの大手プロバイダーです。 Adform をAdobe Experience Platformに接続すると、Experience Cloud ID （ECID）に基づいて Adform を通じてファーストパーティオーディエンスをアクティブ化できます。
last-substantial-update: 2025-10-23T00:00:00Z
source-git-commit: c429ee227bd93455f541a32266bfbef9ddeaae06
workflow-type: tm+mt
source-wordcount: '972'
ht-degree: 32%

---


# Adform 接続 {#adform}

## 概要 {#overview}

Adform は、プログラムによるメディア売買ソリューションの大手プロバイダーです。 Adform をAdobe Experience Platformに接続すると、Experience Cloud ID （ECID）に基づいて Adform を通じてファーストパーティオーディエンスをアクティブ化できます。

>[!IMPORTANT]
>
>宛先コネクタとドキュメントページは、Adform チームが作成および管理します。 お問い合わせや更新のリクエストについては、`support@adform.com` まで直接ご連絡ください。

## ユースケース {#use-cases}

Adform 宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platformのお客様がこの宛先を使用して解決できるサンプルユースケースを以下に示します。

### Adobe Real-Time CDP audience activation {#use-case-1}

この宛先を使用して、Adobe Real-Time CDP オーディエンスを Adform に送信し、Experience Cloud ID （ECID）と Adform の ID Fusion に基づいてアクティブ化します。 Adform の ID Fusion は、Experience Cloud ID （ECID）に基づいてファーストパーティオーディエンスをアクティブ化できる Adform の ID 解決サービスです。

一般的なケースは、Experience Cloud ID （ECID）に基づいて、web サイトやアプリへの web サイト訪問者をリターゲティングすることです。 すぐに利用できる [ イベントストリーミング ](https://exchange.adobe.com/apps/ec/600102/adform-s2s-site-tracking) または [ クライアントサイド ](https://experienceleague.adobe.com/en/docs/experience-platform/destinations/catalog/analytics/adform) Adform 拡張機能を使用して、Experience Cloud ID （ECID）を Adform に送信するだけです。 その後、Experience Cloud ID （ECID）のみに基づいて、アクティベーション用に Adform 宛先を介してオーディエンスを Adform と共有できます。

## 前提条件 {#prerequisites}

* この宛先を使用するには、既存の Adform 顧客である必要があります。
* Adobe Audience Base のデータ接続資格情報が必要です。
   * Adform Audience Base データ接続資格情報がない場合は、Adform 担当者にお問い合わせください。
* 適切に同期させるには、エンティティから Adform サイトトラッキングへの [ イベントストリーミング ](https://exchange.adobe.com/apps/ec/600102/adform-s2s-site-tracking) または [ クライアントサイド ](https://experienceleague.adobe.com/en/docs/experience-platform/destinations/catalog/analytics/adform) 接続が必要です。
   * エンティティから Adform サイトトラッキングへのイベントストリーミングまたはクライアントサイド接続がない場合は、Adform 担当者にお問い合わせください。
   * Adform は、[ イベントストリーミング ](https://exchange.adobe.com/apps/ec/600102/adform-s2s-site-tracking) と [ クライアントサイド ](https://experienceleague.adobe.com/en/docs/experience-platform/destinations/catalog/analytics/adform) の両方に対してAdobe Experience Cloud拡張機能を提供します。


## サポートされている ID {#supported-identities}

Adform では、以下の表に示す ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| ECID | Experience Cloud ID | ECID を表す名前空間。 この名前空間は、「Adobe Marketing Cloud ID」、「Adobe Experience Cloud ID」、「Adobe Experience Platform ID」という別名で呼ばれることもあります。詳しくは、[ECID](/help/identity-service/features/ecid.md) に関する次のドキュメントを参照してください。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/overview.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Segment export]** | *宛先* 宛先で使用される識別子（名前、電話番号など）を使用して、セグメント（オーディエンス）のすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Batch]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Manage Destinations]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対する認証を行うには、必須フィールドに入力し、「**[!UICONTROL Connect to destination]**」を選択します。

![ 宛先に対する認証 ](../../assets/catalog/advertising/adform/authenticate-destination.png)

* **[!UICONTROL Account name]**：今後この宛先接続を識別するためのアカウント名を入力してください…
* **[!UICONTROL S3 Access Key ID]**:Adform から提供された S3 アクセスキーを入力します。
* **[!UICONTROL S3 Secret Access Key]**:Adform から提供された S3 シークレット アクセスキーを入力します。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![ 宛先の詳細の入力 ](../../assets/catalog/advertising/adform/configure-destination-details.png)

* **[!UICONTROL Name]**：今後この宛先を認識するための名前。
* **[!UICONTROL Description]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL Provider Name]**:Adform から提供された Adform アカウント名。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、[バッチプロファイル書き出し宛先に対するオーディエンスデータのアクティブ化](/help/destinations/ui/activate-batch-profile-destinations.md)を参照してください。

### 属性と ID のマッピング {#map}

* **ECID** （Experience Cloud ID）

マッピング手順では、[!DNL ECID] ターゲット ID マッピングのみを使用します。 他の ID フィールドを含めないでください。含めると、アクティベーションが正常に完了しなくなります。

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

宛先コネクタは、ECID ID のみを宛先に書き出します。 その他の ID は書き出されません。 データの書き出しが成功したかどうかを確認するには、Adobe Audience Base アカウントにログインし、オーディエンスが使用可能かどうかを確認してください。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

Adform Audience Base について詳しくは、[Adform Audience Base ドキュメント ](https://www.adformhelp.com/hc/en-us/categories/9738365991697-Data-Management-Platform) を参照してください。