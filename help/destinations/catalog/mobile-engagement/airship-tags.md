---
keywords: 飛行船タグ、飛行船の宛先
title: 航空船タグ接続
description: AdobeのオーディエンスデータをAirshipにAudience Tagsとしてシームレスに渡し、Airship内でターゲティングをおこないます。
exl-id: 84cf5504-f0b5-48d8-8da1-ff91ee1dc171
source-git-commit: a765f6829f08f36010e0e12a7186bf5552dfe843
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 0%

---

# [!DNL Airship Tags] 接続 {#airship-tags-destination}

## 概要

[!DNL Airship] は、顧客ライフサイクルの各段階で、ユーザーに対して、意味のあるパーソナライズされたチャネルメッセージを提供するのを支援する、最先端の顧客エンゲージメントプラットフォームです。

この統合により、Adobe Experience Platformのセグメントデータが[!DNL Airship]に[Tags](https://docs.airship.com/guides/audience/tags/)として渡され、ターゲティングまたはトリガーされます。

[!DNL Airship]について詳しくは、[飛行船に関するドキュメント](https://docs.airship.com)を参照してください。


>[!TIP]
>
>このドキュメントページは[!DNL Airship]チームが作成しました。 お問い合わせや更新のご依頼は、[support.airship.com](https://support.airship.com/)まで直接お問い合わせください。

## 前提条件

Adobe Experience Platformセグメントを[!DNL Airship]に送信する前に、次の操作を行う必要があります。

* [!DNL Airship]プロジェクトにタググループを作成します。
* 認証用のbearerトークンを生成します。

>[!TIP]
> 
>[!DNL Airship]アカウントを[このサインアップリンク](https://go.airship.eu/accounts/register/plan/starter/)を介して作成します（まだ作成していない場合）。

## タググループ

Adobe Experience Platformのセグメントの概念は、Airshipの[タグ](https://docs.airship.com/guides/audience/tags/)に似ていますが、実装にわずかな違いがあります。 この統合は、Experience Platformセグメント](../../../xdm/field-groups/profile/segmentation.md)内のユーザーの[メンバーシップのステータスを、[!DNL Airship]タグの有無にマッピングします。 例えば、`xdm:status`が`realized`に変わるPlatformセグメントでは、タグが[!DNL Airship]チャネルに追加されるか、このプロファイルがマッピングされる名前の付いたユーザーに追加されます。 `xdm:status`が`exited`に変わると、タグは削除されます。

この統合を有効にするには、*タググループ*&#x200B;を`adobe-segments`という名前で[!DNL Airship]に作成します。

>[!IMPORTANT]
>
>新しいタググループ&#x200B;**を作成する際に、「[!DNL Allow these tags to be set only from your server]」というラジオボタンを**&#x200B;チェックしないでください。 そうすると、Adobeタグの統合が失敗します。

タググループの作成手順については、[タググループの管理](https://docs.airship.com/tutorials/manage-project/messaging/tag-groups)を参照してください。

## bearerトークンの生成

[Airshipダッシュボード](https://go.airship.com)で&#x200B;**[!UICONTROL 設定]**&quot; **[!UICONTROL APIと統合]**&#x200B;に移動し、左側のメニューで&#x200B;**[!UICONTROL トークン]**&#x200B;を選択します。

「**[!UICONTROL トークンを作成]**」をクリックします。

トークンのわかりやすい名前(例：「Adobeタグの宛先」)を指定し、役割に「すべてのアクセス」を選択します。

「**[!UICONTROL トークンを作成]**」をクリックし、詳細を機密として保存します。

## ユースケース

[!DNL Airship Tags]宛先をいつどのように使用するかを理解しやすくするために、Adobe Experience Platformのお客様がこの宛先を使用して解決できる使用例を以下に示します。

### 使用例#1

小売業者やエンターテイメントプラットフォームは、ロイヤルティ顧客のユーザープロファイルを作成し、それらのセグメントを[!DNL Airship]に渡してモバイルキャンペーンのメッセージターゲティングをおこなうことができます。

### 使用例#2

トリガーがAdobe Experience Platform内の特定のセグメントに該当する、または特定のセグメントから離脱する場合に、1対1のメッセージをリアルタイムで送信する。

例えば、ある小売業者は、Platformでジーンズのブランド固有のセグメントを設定します。 あの小売業者は、誰かがジーンズを特定のブランドに好むように設定するとすぐに、モバイルメッセージをトリガーできるようになりました。

## 宛先に接続 {#connect}

この宛先に接続するには、[宛先の設定に関するチュートリアル](../../ui/connect-destination.md)で説明されている手順に従います。

### 接続パラメーター {#parameters}

[この宛先を設定](../../ui/connect-destination.md)する際に、次の情報を指定する必要があります。

* **[!UICONTROL Bearerトークン]**:ダッシュボードから生成したbearerト [!DNL Airship] ークン。
* **[!UICONTROL 名前]**:この宛先を識別するのに役立つ名前を入力します。
* **[!UICONTROL 説明]**:この宛先の説明を入力します。
* **[!UICONTROL ドメイン]**:この宛先に適用するデータセンターに応じて、米国またはEUの [!DNL Airship] データセンターを選択します。


## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、[ストリーミングセグメントの書き出し先へのオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

## マッピングに関する考慮事項 {#mapping-considerations}

[!DNL Airship] タグは、デバイスインスタンス（iPhoneなど）を表すチャネルに設定できます。また、すべてのユーザーのデバイスを顧客IDなどの共通の識別子にマッピングする名前付きユーザーに設定することもできます。スキーマにプライマリIDとしてのプレーンテキスト（ハッシュ化されていない）電子メールアドレスがある場合は、**[!UICONTROL ソース属性]**&#x200B;の電子メールフィールドを選択し、以下に示すように、**[!UICONTROL ターゲットID]**&#x200B;の下の右の列の[!DNL Airship]名前付きユーザーにマッピングします。

![名前付きユーザーマッピング](../../assets/catalog/mobile-engagement/airship-tags/mapping-option-2.png)

チャネルにマッピングする必要がある識別子（デバイスなど）の場合、ソースに基づく適切なチャネルにマッピングします。 次の画像は、Google広告IDを[!DNL Airship] Androidチャネルにマッピングする方法を示しています。

![Airship Tagsに接続Airship TagsChannel](../../assets/catalog/mobile-engagement/airship-tags/select-source-identity.png)
![マッピングに](../../assets/catalog/mobile-engagement/airship-tags/select-target-identity.png)
![接続](../../assets/catalog/mobile-engagement/airship-tags/mapping-option.png)

## データの使用とガバナンス {#data-usage-governance}

すべての[!DNL Adobe Experience Platform]宛先は、データを処理する際のデータ使用ポリシーに準拠しています。 [!DNL Adobe Experience Platform]によるデータガバナンスの強制方法について詳しくは、「[データガバナンスの概要](../../../data-governance/home.md)」を参照してください。
