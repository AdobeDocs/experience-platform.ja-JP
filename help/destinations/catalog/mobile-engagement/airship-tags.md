---
keywords: 飛行船タグ；飛行船の宛先
title: Airship Tags 接続
description: Airship 内でターゲティングするために、Adobeのオーディエンスデータをオーディエンスタグとして Airship にシームレスに渡します。
exl-id: 84cf5504-f0b5-48d8-8da1-ff91ee1dc171
source-git-commit: c35b43654d31f0f112258e577a1bb95e72f0a971
workflow-type: tm+mt
source-wordcount: '970'
ht-degree: 32%

---

# [!DNL Airship Tags] 接続 {#airship-tags-destination}

## 概要

[!DNL Airship] は、優れたカスタマーエンゲージメントプラットフォームです。カスタマーライフサイクルのあらゆる段階において、ユーザーに対して有意義でパーソナライズされたオムニチャネルメッセージを提供するのを支援します。

この統合により、Adobe Experience Platform オーディエンスデータがに渡されます。 [!DNL Airship] as [タグ](https://docs.airship.com/guides/audience/tags/) ターゲティングまたはトリガー用。

について詳しくは、 [!DNL Airship]を参照してください。 [Airship ドキュメント](https://docs.airship.com).


>[!TIP]
>
>この宛先コネクタとドキュメントページは、で作成および管理されます。 [!DNL Airship] チーム。 お問い合わせや更新のリクエストについては、 [support.airship.com](https://support.airship.com/).

## 前提条件

Adobe Experience Platform オーディエンスをに送信する前に [!DNL Airship]は、以下を行う必要があります。

* でタググループを作成する [!DNL Airship] プロジェクト。
* 認証用のベアラートークンを生成します。

>[!TIP]
> 
>を作成 [!DNL Airship] 次を使用してアカウント [このサインアップリンク](https://go.airship.eu/accounts/register/plan/starter/) まだの場合は、

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platformを通じて生成されたオーディエンス [セグメント化サービス](../../../segmentation/home.md). |
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

AdobeExperience Platform のオーディエンスの概念は、次のようになります [タグ](https://docs.airship.com/guides/audience/tags/) 飛行船では、実装にわずかな違いがあります。 この統合では、ユーザーのステータスを [Experience Platformセグメントのメンバーシップ](../../../xdm/field-groups/profile/segmentation.md) ～の有無に関して [!DNL Airship] タグ。 例えば、Platform オーディエンスでは、 `xdm:status` 変更先 `realized`を選択すると、タグが [!DNL Airship] このプロファイルがマッピングされるチャネルまたは名前付きユーザー。 次の場合 `xdm:status` 変更先 `exited`が削除されると、タグも削除されます。

この統合を有効にするには、 *タググループ* 。対象： [!DNL Airship] 名前付き `adobe-segments`.

>[!IMPORTANT]
>
>タググループを新規作成する場合 **確認しない** というラジオボタン[!DNL Allow these tags to be set only from your server]」と入力します。 この操作を行うと、Adobeタグの統合が失敗します。

参照： [タググループの管理](https://docs.airship.com/tutorials/manage-project/messaging/tag-groups) タググループの作成手順について説明します。

## ベアラートークンの生成

に移動 **[!UICONTROL 設定]** » **[!UICONTROL API と統合]** が含まれる [飛行船ダッシュボード](https://go.airship.com) を選択して、 **[!UICONTROL トークン]** 左側のメニューの

クリック **[!UICONTROL トークンを作成]**.

トークンのわかりやすい名前（例：「Adobeタグの宛先」）を指定し、ロールで「すべてのアクセス」を選択します。

クリック **[!UICONTROL トークンを作成]** 詳細を機密情報として保存します。

## ユースケース

を使用する方法とタイミングをより深く理解するために、 [!DNL Airship Tags] 宛先の場合、Adobe Experience Platformのお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

### のユースケース#1

小売業者やエンターテインメントプラットフォームは、ロイヤルティ顧客に関するユーザープロファイルを作成し、それらのオーディエンスをに渡すことができます。 [!DNL Airship] モバイルキャンペーンでのメッセージターゲティング用。

### のユースケース#2

ユーザーがAdobe Experience Platform内の特定のオーディエンスに含まれる、または特定のオーディエンスから除外される場合に、1 対 1 のメッセージをリアルタイムでトリガーにします。

例えば、小売業者が、Platform でジーンズのブランド固有のオーディエンスを設定するとします。 この小売業者は、ユーザーが特定のブランドをジーンズに好むように設定するとすぐに、モバイルメッセージをトリガーに設定できるようになりました。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

* **[!UICONTROL ベアラートークン]**：から生成したベアラートークン [!DNL Airship] ダッシュボード。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：この宛先を識別するのに役立つ名前を入力します。
* **[!UICONTROL 説明]**：この宛先の説明を入力します。
* **[!UICONTROL ドメイン]**：米国または欧州のデータセンターを、次に応じて選択します [!DNL Airship] データセンターはこの宛先に適用されます。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先にオーディエンスをアクティブ化する手順については、[ストリーミングオーディエンス書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

## マッピングに関する考慮事項 {#mapping-considerations}

[!DNL Airship] タグは、デバイスインスタンスを表すチャネル（例：iPhone）または、ユーザーのすべてのデバイスを共通の識別情報（例：カスタマー ID）にマッピングする名前付きユーザーで設定できます。 スキーマのプライマリ ID としてプレーンテキスト（ハッシュ化されていない）メールアドレスがある場合は、のメールフィールドを選択します **[!UICONTROL Source属性]** およびをにマッピングします [!DNL Airship] の右列の名前付きユーザー **[!UICONTROL ターゲット Id]**&#x200B;を参照してください。

![名前付きユーザーマッピング](../../assets/catalog/mobile-engagement/airship-tags/mapping-option-2.png)

チャネルにマッピングする必要がある識別子（デバイス）については、ソースに基づいて適切なチャネルにマッピングします。 次の画像は、Google Advertising ID をにマッピングする方法を示しています [!DNL Airship] Android チャンネル。

![Airship Tags への接続](../../assets/catalog/mobile-engagement/airship-tags/select-source-identity.png)
![Airship Tags への接続](../../assets/catalog/mobile-engagement/airship-tags/select-target-identity.png)
![チャネルマッピング](../../assets/catalog/mobile-engagement/airship-tags/mapping-option.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。方法について詳しくは、 [!DNL Adobe Experience Platform] データガバナンスを実施します。を参照してください。 [データガバナンスの概要](../../../data-governance/home.md).
