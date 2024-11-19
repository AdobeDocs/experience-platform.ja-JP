---
title: Magnite リアルタイム宛先接続
description: この宛先を使用すると、Adobe CDP オーディエンスを Magnite ストリーミングプラットフォームにリアルタイムで配信できます。
last-substantial-update: 2024-11-18T00:00:00Z
exl-id: 4e08a14b-6800-41e1-95a5-826a6241144d
source-git-commit: da05db9376893bdbe8f0aa291f19a507e4a73d4f
workflow-type: tm+mt
source-wordcount: '1317'
ht-degree: 31%

---

# Magnite：リアルタイム宛先接続

## 概要 {#overview}

Adobe Experience Platformの [!DNL Magnite: Real-Time] および [Magnite：バッチ ](/help/destinations/catalog/advertising/magnite-batch.md) 宛先は、Magnite ストリーミングプラットフォームでターゲティングとアクティブ化のためにオーディエンスをマッピングおよび書き出すのに役立ちます。

[!DNL Magnite Streaming] プラットフォームへのオーディエンスのアクティブ化は、Magnite:Real-Time 宛先と Magnite:Batch 宛先の両方を使用する必要がある 2 つの手順のプロセスです。

[!DNL Magnite Streaming] に対してオーディエンスをアクティブ化するには、次の手順を実行します。

* このページに示すように、[!DNL Magnite: Real-Time] 宛先のオーディエンスをアクティブ化します。
* Magnite：バッチ宛先で同じオーディエンスを有効化します。 [!DNL Magnite: Batch] の宛先は必須コンポーネントです。 [!DNL Magnite Streaming] バッチ宛先でオーディエンスをアクティブ化できないと、統合に失敗し、オーディエンスはアクティブ化されません。

メモ：リアルタイムの宛先を使用する場合、[!DNL Magnite Streaming] はリアルタイムでオーディエンスを受け取りますが、Magnite は一時的にプラットフォームにリアルタイムオーディエンスを保存することしかできず、数日以内にシステムから削除されます。 このため、Magnite:Real-Time 宛先を使用する場合は、*また* Magnite:Batch 宛先 – リアルタイム宛先に対して有効化する各オーディエンスを、バッチ宛先に対して有効化する必要があります。

>[!IMPORTANT]
>
>宛先コネクタとドキュメントページは、[!DNL Magnite] チームが作成および管理します。 お問い合わせや更新のリクエストについては、`adobe-tech@magnite.com` まで直接ご連絡ください。

## ユースケース {#use-cases}

[!DNL Magnite: Real-Time] 宛先を使用する方法とタイミングを理解しやすくするために、Adobe Experience Platform のお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

### アクティブ化とターゲティング {#activation-and-targeting}

この Magnite との統合により、お客様は、広告ターゲティングのために、CDP オーディエンスをAdobe Experience Platformから Magnite に渡すことができます。 オーディエンスは、ポジティブターゲティングとネガティブターゲティング（抑制）のために、Magnite 内で選択できます。

## 前提条件 {#prerequisites}

Adobe Experience Platformで [!DNL Magnite] の宛先を使用するには、まず [!DNL Magnite Streaming] アカウントが必要です。 [!DNL Magnite Streaming] アカウントをお持ちの場合は、[!DNL Magnite] アカウントマネージャーに連絡して、宛先にアクセスするための資格情報を取得し [!DNL Magnite's] ください。
[!DNL Magnite Streaming] アカウントをお持ちでない場合は、adobe-tech@magnite.comにお問い合わせください。

## サポートされている ID {#supported-identities}

[!DNL Magnite: Real-Time] の宛先では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|-------------------|--------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------|
| device_id | デバイスまたは ID の一意の ID。 タイプに関係なく、任意のデバイス ID とファーストパーティ ID を受け入れます。 | Magnite でサポートされている ID タイプには、PPUID、GAID、IDFA、TV デバイス ID などがありますが、これらに限定されません。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|-----------------------------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform[ セグメント化サービス ](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|------------------|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 書き出しタイプ | **[!UICONTROL セグメントの書き出し]** | [!DNL Magnite: Real-Time] 宛先で使用される識別子（名前、電話番号など）を使用して、セグメント（オーディエンス）のすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。セグメント評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

![ 宛先設定認証フィールドが未入力 ](../../assets/catalog/advertising/magnite/destination-realtime-config-auth-unfilled.png)

* **[!UICONTROL ユーザー名]**:[!DNL Magnite] から提供されたユーザー名。
* **[!UICONTROL パスワード]**:[!DNL Magnite] から提供されたパスワード。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL 会社名]**：顧客の名前または会社名。 サポートされている [!DNL Magnite Streaming] クライアントのみを選択できます。

>[!NOTE]
>
>会社名は、Magnite で設定し、[ 宛先への認証 ](#authenticate) ステップで設定したAmazon S3 配信バケットの名前と一致する文字列にする必要があります。 サポートされる文字には、「a ～ z」、「A ～ Z」、「0 ～ 9」、「–」（ダッシュ）、「_」（アンダースコア）があります。

![ 宛先設定認証フィールドに値が入力されています ](../../assets/catalog/advertising/magnite/destination-realtime-config-auth-filled.png)

完了したら、「**[!UICONTROL 作成]** ボタンを選択します。

![ オプションのガバナンスポリシーと実施アクション ](../../assets/catalog/advertising/magnite/destination-realtime-config-grouping-policy.png)

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
>
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスセグメントをアクティベートする手順は、[ストリーミングセグメントの書き出し宛先へのプロファイルとセグメントのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

宛先接続を作成したら、Audience Activation のフローに進むことができます。 次の節では、リアルタイム宛先を使用してオーディエンスをアクティブ化する方法について説明します。

### 属性と ID のマッピング {#map}

次の手順では、ソース識別子を Magnite device_id 識別子にマッピングします。

* 「**[!UICONTROL 新しいマッピングを追加]**」を選択して、必要な数のマッピングを追加できます。

リアルタイム宛先を使用したこの例では、Magnite device_id ターゲットフィールドにマッピングされた汎用の deviceId ソース識別子を含む行を表示しています。 マッピングを行ったら、「[!UICONTROL  次へ ]」を選択します。

![ 目的のデータフィールドを device_ID フィールドにマッピングする ](../../assets/catalog/advertising/magnite/destination-realtime-active-audience-field-mapping.png)

必ず、アクティブ化されたすべてのオーディエンスにマッピング ID を設定するか、マッピング ID が存在しない場合は「なし」を設定します。

![ 必ず、アクティブ化されたすべてのオーディエンスにマッピング ID を設定するか、マッピング ID が存在しない場合は「なし」を設定します ](../../assets/catalog/advertising/magnite/destination-realtime-active-audience-mappingid.png)。

各オーディエンスの開始日（必須）、終了日（オプション）、マッピング ID を設定する必要があります。

**マッピング ID**

* オーディエンスに Magnite と呼ばれる既存のセグメント ID がある場合は、**[!UICONTROL マッピング ID]** フィールドを使用します。

* **[!UICONTROL マッピング ID]** をオーディエンスに追加するには、各オーディエンス行を個別に選択し、右側の列にデータを入力します（上の画像を参照）。 マッピング ID を追加しない場合は、「マッピング ID」フィールドに「なし」を入力します。

「**[!UICONTROL 次へ]**」を選択し、アクティベーションフローを完了します。

![ 「次へ」を選択して、アクティベーションフローを完了します ](../../assets/catalog/advertising/magnite/destination-realtime-active-audience-review.png)。

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

オーディエンスがアップロードされたら、オーディエンスが正しく作成およびアップロードされたことを次の手順に従って検証できます。

<!--

* In 95% of cases, audiences will be delivered to Magnite Streaming in under 10 minutes. The actual receipt and processing of the events within Magnite Streaming depends on the shared data volume.

-->

* 取り込み後、オーディエンスは数分以内に [!DNL Magnite Streaming] に表示され、取引に適用できることが期待されます。 これを確認するには、Adobe Experience Platformのアクティベーション手順で共有されたセグメント ID を検索します。

## [!DNL Magnite: Batch] 宛先を介して同じオーディエンスをアクティブ化

リアルタイム宛先を使用して [!DNL Magnite Streaming] と共有されたオーディエンスも、Magnite：バッチ宛先を使用して共有する必要があります。 正しく設定されると、[!DNL Magnite Streaming] UI のセグメント名が更新され、毎日の更新後のAdobe Experience Platformで使用されるセグメント名が反映されます。

最後に、バッチ宛先が統合用に設定されていない場合は、Magnite：バッチ宛先ドキュメントを使用して今すぐ設定します。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

その他のヘルプドキュメントについては、[Magnite ヘルプセンター ](https://help.magnite.com/help) を参照してください。
