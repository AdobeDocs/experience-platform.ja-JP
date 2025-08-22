---
keywords: 飛行船タグ；飛行船の宛先
title: Airship Tags 接続
description: Airship 内でターゲティングするために、Adobeのオーディエンスデータをオーディエンスタグとして Airship にシームレスに渡します。
exl-id: 84cf5504-f0b5-48d8-8da1-ff91ee1dc171
source-git-commit: 5619424024eff81fca21408288494402e2a4d4ff
workflow-type: tm+mt
source-wordcount: '1082'
ht-degree: 29%

---

# [!DNL Airship Tags] 接続 {#airship-tags-destination}

## 概要

>[!IMPORTANT]
>
>* 2025 年 8 月 21 日（PT）以降、宛先カタログに 2 つの **[!DNL Airship Tags]** カードが並んで表示されるようになります。 これは、宛先サービスの内部アップグレードが原因です。 既存の **[!DNL Airship Tags]** 宛先コネクタの名前は、**[!UICONTROL （非推奨） Airship Tags に変更され]** 名前が **[!UICONTROL Airship Tags]** の新しいカードが使用できるようになりました。
>* 新しいアクティベーションデータフローについては、カタログの新しい **[!UICONTROL Airship Tags]** 接続を使用します。 **[!UICONTROL （非推奨） Airship Tags]** の宛先へのアクティブなデータフローがある場合は、自動的に更新されるので、アクションは必要ありません。
>* [Flow Service API](https://developer.adobe.com/experience-platform-apis/references/destinations/) を使用してデータフローを作成する場合は、[!DNL flow spec ID] を更新し、次の値に [!DNL connection spec ID] す必要があります。
>   * フロー仕様 ID: `0c7e71c8-4d60-4685-a216-77f57e37b04a`
>   * 接続仕様 ID: `aec13e22-8226-4b5d-9961-6baa35b251d2`

[!DNL Airship] は、カスタマーエンゲージメントプラットフォームのリーダーであり、カスタマーライフサイクルのあらゆる段階で、ユーザーに対して有意義でパーソナライズされたオムニチャネルメッセージを提供するのを支援します。

この統合では、ターゲティングやトリガーのために、Adobe Experience Platform オーディエンスデータを [!DNL Airship] as [Tags](https://docs.airship.com/guides/audience/tags/) に渡します。

[!DNL Airship] について詳しくは、[Airship のドキュメント ](https://docs.airship.com) を参照してください。


>[!TIP]
>
>この宛先コネクタとドキュメントページは、[!DNL Airship] チームが作成および管理します。 お問い合わせや更新のリクエストについては、[support.airship.com](https://support.airship.com/) まで直接ご連絡ください。

## 前提条件

Adobe Experience Platform オーディエンスを [!DNL Airship] に送信する前に、次の操作を行う必要があります。

* [!DNL Airship] プロジェクトにタググループを作成します。
* 認証用のベアラートークンを生成します。

>[!TIP]
> 
>[!DNL Airship] このサインアップリンク [ から ](https://go.airship.eu/accounts/register/plan/starter/) アカウントをまだ作成していない場合は、作成します。

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
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | Airship タグの宛先で使用される識別子を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## タググループ

Adobe Experience Platform のオーディエンスの概念は、Airship の [Tags](https://docs.airship.com/guides/audience/tags/) に似ていますが、実装にわずかな違いがあります。 この統合は、ユーザーの [Experience Platform セグメントのメンバーシップ ](../../../xdm/field-groups/profile/segmentation.md) のステータスを、[!DNL Airship] タグの有無にマップします。 例えば、`xdm:status` が `realized` に変わるExperience Platform オーディエンスの場合、タグは [!DNL Airship] チャネルまたはこのプロファイルのマッピング先である名前付きユーザーに追加されます。 `xdm:status` が `exited` に変わると、タグは削除されます。

この統合を有効にするには、という名前の *に* タググループ [!DNL Airship]`adobe-segments` 作成します。

>[!IMPORTANT]
>
>新しいタググループを作成する場合 **オンにしない**、「[!DNL Allow these tags to be set only from your server]」というラジオボタンを使用します。 この操作を行うと、Adobe タグの統合が失敗します。

タググループの作成手順については、[ タググループの管理 ](https://docs.airship.com/tutorials/manage-project/messaging/tag-groups) を参照してください。

## ベアラートークンの生成

**[!UICONTROL Airship ダッシュボード]** 設定 **[!UICONTROL /]** API と統合 [ に移動し ](https://go.airship.com) 左側のメニューで **[!UICONTROL トークン]** を選択します。

**[!UICONTROL トークンを作成]** をクリックします。

トークンのわかりやすい名前（例：「Adobe Tags Destination」）を指定し、ロールに「All Access」を選択します。

**[!UICONTROL トークンを作成]** をクリックし、詳細を機密として保存します。

## ユースケース

[!DNL Airship Tags] の宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platformのお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

### のユースケース#1

小売業者やエンターテインメントプラットフォームは、ロイヤルティ顧客に関するユーザープロファイルを作成し、モバイルキャンペーンでメッセージをターゲティングするためにこれらのオーディエンスを [!DNL Airship] に渡すことができます。

### のユースケース#2

ユーザーがAdobe Experience Platform内の特定のオーディエンスに含まれる、または特定のオーディエンスから除外される場合に、1 対 1 のメッセージをリアルタイムでトリガーにします。

例えば、retailerがExperience Platformでジーンズのブランド固有のオーディエンスを設定したとします。 このretailerでは、特定のブランドをジーンズが好むように設定するとすぐに、モバイルメッセージをトリガーできるようになりました。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

* **[!UICONTROL ベアラートークン]**:[!DNL Airship] ダッシュボードから生成したベアラートークンです。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：この宛先を識別するのに役立つ名前を入力します。
* **[!UICONTROL 説明]**：この宛先の説明を入力します。
* **[!UICONTROL ドメイン]**：この宛先に適用される [!DNL Airship] データセンターに応じて、米国または欧州のデータセンターを選択します。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先にオーディエンスをアクティブ化する手順については、[ストリーミングオーディエンス書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

## マッピングに関する考慮事項 {#mapping-considerations}

タグ [!DNL Airship]、デバイスインスタンスを表すチャネル（例：iPhone）または、ユーザーのすべてのデバイスを共通の識別情報（例：カスタマー ID）にマッピングする名前付きユーザーに対して設定できます。 スキーマにプレーンテキスト（ハッシュ化されていない）メールアドレスがプライマリ ID として存在する場合は、**[!UICONTROL Source属性のメールフィールドを選択し]** 以下に示すように、[!DNL Airship] ターゲット ID **[!UICONTROL の下の右側の列で]** 名のユーザーにマッピングします。

![ 名前付きユーザーマッピング ](../../assets/catalog/mobile-engagement/airship-tags/mapping-option-2.png)

チャネルにマッピングする必要がある識別子（デバイス）については、ソースに基づいて適切なチャネルにマッピングします。 次の画像は、Google Advertising ID を [!DNL Airship] Android チャネルにマッピングする方法を示しています。

![Airship タグへの接続 ](../../assets/catalog/mobile-engagement/airship-tags/select-source-identity.png)
![Airship タグへの接続 ](../../assets/catalog/mobile-engagement/airship-tags/select-target-identity.png)
![ チャネルマッピング ](../../assets/catalog/mobile-engagement/airship-tags/mapping-option.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[ データガバナンスの概要 ](../../../data-governance/home.md) を参照してください。
