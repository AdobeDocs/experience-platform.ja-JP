---
title: Magnite ストリーミングバッチ宛先
description: この宛先を使用して、Adobe CDP オーディエンスを Magnite ストリーミングプラットフォームにバッチで配信します。
badgeBeta: label="ベータ版" type="Informative"
hide: true
hidefromtoc: true
source-git-commit: c35b43654d31f0f112258e577a1bb95e72f0a971
workflow-type: tm+mt
source-wordcount: '1685'
ht-degree: 14%

---


# Magnite ストリーミング：バッチ接続 {#magnite-streaming-batch}

## 概要 {#overview}

このドキュメントでは、Magnite ストリーミング：バッチ宛先について説明し、オーディエンスをアクティベートしてそれに書き出す方法をより深く理解するのに役立つ、サンプルのユースケースを提供します。

Adobe Real-Time CDPのオーディエンスは、Magnite:Streaming Platform に次の 2 つの方法で配信できます。1 日に 1 回配信することも、リアルタイムで配信することもできます。

1. 1 日に 1 回だけオーディエンスを配信する必要がある場合や、配信する必要がある場合は、Magnite：ストリーミングバッチ宛先を使用できます。この宛先は、毎日の S3 バッチファイル配信を介してオーディエンスを Magnite：ストリーミングに配信します。 これらのバッチオーディエンスは、リアルタイムオーディエンスとは異なり、プラットフォームに無期限に保存されます。リアルタイムオーディエンスは数日のみ保存されます。

2. ただし、オーディエンスをリアルタイムで配信する必要がある場合は、Magnite：ストリーミングリアルタイム宛先を使用する必要があります。 リアルタイムの宛先を使用する場合、Magnite：ストリーミングはリアルタイムでオーディエンスを受け取りますが、リアルタイムのオーディエンスはプラットフォームに一時的に保存することしかできず、数日以内にシステムから削除されます。 このため、Magnite:Streaming Real-Time 宛先を使用する場合は、Magnite:Streaming Batch 宛先 – リアルタイム宛先に対して有効化する各オーディエンス、また、バッチ宛先に対して有効化する必要があります。

まとめると、Adobe Real-Time CDP オーディエンスを 1 日 1 回だけ配信する場合は、Magnite：ストリーミングバッチ宛先のみを使用し、オーディエンスは 1 日に 1 回配信されます。 Adobe Real-Time CDP オーディエンスをリアルタイムで配信する場合は、Magnite：ストリーミングバッチ宛先と Magnite：ストリーミングリアルタイム宛先の両方を使用します。 詳しくは、Magnite：ストリーミングをご覧ください。


Magnite：ストリーミングバッチの宛先、その宛先への接続方法およびバッチの宛先に対してAdobe Real-Time CDP オーディエンスをアクティブ化する方法について詳しくは、以下のリンクを参照してください。
リアルタイムの宛先について詳しくは、代わりに [ このドキュメント ](magnite-streaming.md) を参照してください。

>[!IMPORTANT]
>
>この宛先コネクタはベータ版で、一部のお客様のみご利用いただけます。 アクセス権をリクエストするには、Adobe担当者にお問い合わせください。
>
>宛先コネクタとドキュメントページは、[!DNL Magnite] チームが作成および管理します。 お問い合わせや更新のリクエストについては、`adobe-tech@magnite.com` まで直接ご連絡ください。

## ユースケース {#use-cases}

Magnite ストリーミング：バッチ宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platformのお客様がこの宛先を使用して解決できるサンプルユースケースを以下に示します。

### のユースケース#1 {#use-case-1}

Magnite ストリーミング：リアルタイム宛先のオーディエンスをアクティブ化しました。

バッチ配信のデータは、Magnite ストリーミングプラットフォーム内のリアルタイム配信のデータを置き換えて保持する必要があるので、Magnite Streaming: Real-Time 宛先を介してアクティブ化されたオーディエンスも、Magnite Streaming: Batch 宛先を使用する必要があります。

### のユースケース#2 {#use-case-2}

Magnite ストリーミングプラットフォームに対してバッチ/毎日のアクティブ化でのみオーディエンスをアクティブ化する場合。

Magnite Streaming: Batch 宛先を介してアクティブ化されたオーディエンスは、バッチ/毎日の頻度で配信され、Magnite ストリーミングプラットフォームでターゲティング可能になります。

## 前提条件 {#prerequisites}

Adobe Experience Platformで Magnite の宛先を使用するには、まず Magnite ストリーミングアカウントが必要です。 [!DNL Magnite Streaming] アカウントをお持ちの場合は、[!DNL Magnite] アカウントマネージャーに連絡して、宛先にアクセスするための資格情報を取得し [!DNL Magnite's] ください。 [!DNL Magnite Streaming] アカウントをお持ちでない場合は、adobe-tech@magnite.comにお問い合わせください。

## サポートされている ID {#supported-identities}

Magnite ストリーミング：バッチ宛先は、Adobe CDP から *任意* ID ソースを受け取ることができます。 現在、この宛先には、マッピング先の 3 つのターゲット ID フィールドがあります。

>[!NOTE]
>
>*任意* ID ソースは、任意の magnite_deviceId ターゲット ID にマッピングできます。

| ターゲット ID | 説明 | 注意点 |
|:--------------------------- |:------------------------------------------------------------------------------------------------ |:------------------------------------------------------------------------------------- |
| magnite_deviceId_GAID | GOOGLE ADVERTISING ID | ソース ID が GAID の場合は、このターゲット ID を選択します |
| magnite_deviceId_IDFA | Apple の広告主 ID | ソース ID が IDFA の場合は、このターゲット ID を選択します |
| magnite_deviceId_CUSTOM | カスタム/ユーザー定義 ID | ソース ID が GAID や IDFA でない場合や、カスタム ID やユーザー定義 ID である場合は、このターゲット ID を選択します |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

| オーディエンスオリジン | サポートあり | 説明 |
|-----------------------------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform[ セグメント化サービス ](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

| 項目 | タイプ | メモ |
|-----------------------------|----------|----------|
| 書き出しタイプ | オーディエンスのエクスポート | Magnite ストリーミング：バッチ宛先で使用される識別子（名前、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | バッチ | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、バッチ [ ファイルベースの宛先 ](/help/destinations/destination-types.md) を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

宛先の使用が承認され、Magnite ストリーミングが資格情報を共有したら、次の手順に従ってデータの認証、マッピングおよび共有を行ってください。

### 宛先に対する認証 {#authenticate}

AdobeExperience カタログで、Magnite Streaming: Batch の宛先を見つけます。 「その他のオプション」ボタン（\...）をクリックし、宛先接続/インスタンスを設定します。

既存のアカウントがある場合は、アカウントタイプオプションを「既存のアカウント」に変更することで、そのアカウントを見つけることができます。 それ以外の場合は、以下にアカウントを作成します。

新しいアカウントを作成し、初めて宛先に認証するには、必要な「S3 アクセスキー」および「S3 シークレットキー」フィールド（アカウントマネージャーを通じて提供）に入力し、「宛先に接続 **[!UICONTROL を選択し]** す。

![ 宛先設定認証フィールドが未入力 ](../../assets/catalog/advertising/magnite/destination-batch-config-auth-unfilled.png)

>[!NOTE]
>
>Magnite ストリーミングのセキュリティポリシーでは、S3 キーを定期的にローテーションする必要があります。 今後、新しい S3 アクセスと S3 秘密鍵でアカウントを更新する予定です。 アカウント自体を更新するだけで、そのアカウントを使用している宛先は、更新されたキーを自動的に使用します。 新しいキーをアップロードしないと、データはこの宛先に送信されません。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：この宛先接続/インスタンスを
未来。
* **[!UICONTROL 説明]**：識別に役立つ説明
今後の宛先接続/ インスタンス。
* **[!UICONTROL ソースパートナーの名前]**:Magnite Streaming のプラットフォームでソースとして使用したい名前

![ 宛先設定認証フィールドに値が入力されています ](../../assets/catalog/advertising/magnite/destination-batch-config-auth-filled.png)

>[!NOTE]
>
>複数の ID タイプ（GAID、IDFA など）を送信する予定がある場合 バッチ宛先を使用する場合、それぞれに新しい宛先接続/インスタンスが必要です。 詳しくは、Magnite アカウント担当者にお問い合わせください。

「**[!UICONTROL 次へ]**」を選択して続行できます

次の画面、「ガバナンスポリシーと実施アクション （オプション）」では、関連するデータガバナンスポリシーをオプションで選択できます。 「データの書き出し」は、通常、Magnite ストリーミングバッチ宛先に対して選択されます。

![ オプションのガバナンスポリシーと実施アクション ](../../assets/catalog/advertising/magnite/destination-batch-config-grouping-policy.png)

選択したら、「**[!UICONTROL 作成]**」を選択します。このオプションの画面をスキップする場合は、

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

### この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、[バッチプロファイル書き出し宛先に対するオーディエンスデータのアクティブ化](/help/destinations/ui/activate-batch-profile-destinations.md)を参照してください。

### 属性と ID のマッピング {#map}

「**[!UICONTROL Source]**」フィールドでは、デバイスの任意の属性や ID を選択することができます。 この例では、「DeviceId」というカスタム IdentityMap を選択しました
![ 目的のデータフィールドを device_id フィールドにマッピングする ](../../assets/catalog/advertising/magnite/destination-batch-active-audience-field-mapping.png)

**[!UICONTROL ターゲットフィールド]** で以下を実行します。
![ 適切なデバイスタイプのターゲット ID を選択し ](../../assets/catalog/advertising/magnite/destination-batch-active-audience-select-device-type.png) す。詳しくは、[ サポートされる ID](#supported-identities) を参照してください。
この例では、{2**[!UICONTROL Target フィールド]** がカスタム IdentityMap:DeviceID として定義されているので、{Source フィールド ]**:magnite_deviceId_CUSTOM を選択しています。**[!UICONTROL 

>[!NOTE]
>
>複数の ID タイプ（GAID、IDFA など）の送信やマッピングを計画している場合 バッチ宛先を使用する場合、それぞれに新しい宛先接続/インスタンスが必要です。 詳しくは、Magnite アカウント担当者にお問い合わせください。


「各オーディエンスのファイル名と書き出しスケジュールを設定」画面で、各オーディエンスの開始日（必須）、終了日（オプション）、マッピング ID （必須）を設定する必要があります。

>[!IMPORTANT]
>
> この宛先にはマッピング ID または「なし」が必要です。
>
> マッピング ID は、オーディエンスに Magnite ストリーミングと呼ばれる既存のセグメント ID がある場合に指定する必要があります。 それ以外の場合は、マッピング ID として「NONE」を使用する必要があります。
>
> 各オーディエンスのファイル名を設定する場合は、「カスタムテキスト」フィールドを介したマッピング ID を追加してください。 マッピング ID は次のように追加されます。`{previous_filename}\_\[MAPPING_ID\].` このオーディエンスが Magnite ストリーミングを初めて使用するもので、マッピング ID を指定しない場合は、「カスタムテキスト」フィールドに「なし」を入力する必要があります。 この場合、新しいファイル名は `{previous_filename}\_\[NONE\]` になります。

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

オーディエンスがアップロードされたら、オーディエンスが正しく作成およびアップロードされたことを検証できます。

* Magnite ストリーミングバッチ宛先は、S3 ファイルを毎日 Magnite ストリーミングに配信します。 配信および取り込み後、オーディエンス/セグメントは Magnite ストリーミングに表示され、取引に適用できることが期待されます。 これを確認するには、Adobe Experience Platformのアクティベーション手順で共有されたセグメント ID またはセグメント名を参照します。

>[!NOTE]
>
>Magnite ストリーミングバッチ宛先にアクティブ化/配信されたオーディエンスは、Magnite ストリーミングリアルタイム宛先を介してアクティブ化/配信されたオーディエンスと同じオーディエンスを *置き換え* ます。 セグメント名を使用してセグメントを検索する場合、バッチが Magnite ストリーミングプラットフォームで取り込まれて処理されるまで、リアルタイムでセグメントが見つからない場合があります。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

その他のヘルプドキュメントについては、[Magnite ヘルプセンター ](https://help.magnite.com/help) を参照してください。
