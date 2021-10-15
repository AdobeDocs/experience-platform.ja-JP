---
keywords: 飛行船の属性；飛行船の宛先
title: 航空船属性の接続
description: Adobeのオーディエンスデータをオーディエンス属性として Airship にシームレスに渡し、Airship 内でターゲティングをおこないます。
exl-id: bfc1b52f-2d68-40d6-9052-c2ee1e877961
source-git-commit: a765f6829f08f36010e0e12a7186bf5552dfe843
workflow-type: tm+mt
source-wordcount: '706'
ht-degree: 0%

---

# [!DNL Airship Attributes] 接続 {#airship-attributes-destination}

## 概要 {#overview}

[!DNL Airship] は、顧客ライフサイクルの各段階で、意味のあるパーソナライズされたチャネルメッセージをユーザーに届けるのを支援する、最先端の顧客エンゲージメントプラットフォームです。

この統合では、Adobeプロファイルデータを [!DNL Airship] に [ 属性 ](https://docs.airship.com/guides/audience/attributes/) として渡し、ターゲティングまたはトリガーします。

[!DNL Airship] の詳細については、[ 航空船のドキュメント ](https://docs.airship.com) を参照してください。

>[!TIP]
>
>このドキュメントページは [!DNL Airship] チームが作成しました。 お問い合わせや更新のご依頼は、[support.airship.com](https://support.airship.com/) まで直接お問い合わせください。

## 前提条件 {#prerequisites}

オーディエンスセグメントを [!DNL Airship] に送信する前に、次の操作をおこなう必要があります。

* [!DNL Airship] プロジェクトで属性を有効にします。
* 認証用の bearer トークンを生成します。

>[!TIP]
>
>[ このサインアップリンク ](https://go.airship.eu/accounts/register/plan/starter/) を介して [!DNL Airship] アカウントを作成します（まだ作成していない場合）。

## 属性の有効化 {#enable-attributes}

Adobe Experience Platformプロファイル属性は [!DNL Airship] 属性に似ており、このページで後述するマッピングツールを使用して、Platform で互いに簡単にマッピングできます。

[!DNL Airship] プロジェクトには、定義済みの属性とデフォルトの属性がいくつかあります。カスタム属性がある場合は、まず [!DNL Airship] で定義する必要があります。 詳しくは、[ 属性の設定と管理 ](https://docs.airship.com/tutorials/audience/attributes/) を参照してください。

## bearer トークンの生成 {#bearer-token}

[Airship ダッシュボード ](https://go.airship.com) の **[!UICONTROL 設定]**&quot; **[!UICONTROL API と統合]** に移動し、左側のメニューで **[!UICONTROL トークン]** を選択します。

「**[!UICONTROL トークンを作成]**」をクリックします。

トークンのわかりやすい名前 ( 例：「Adobe属性の宛先」) を指定し、役割に「すべてのアクセス」を選択します。

「**[!UICONTROL トークンを作成]**」をクリックし、詳細を機密として保存します。

## ユースケース {#use-cases}

[!DNL Airship Attributes] の宛先をいつどのように使用するかをより深く理解できるように、Adobe Experience Platformのお客様がこの宛先を使用して解決できる使用例を以下に示します。

### 使用例#1

Adobe Experience Platform内で収集したプロファイルデータを活用して、[!DNL Airship] のチャネル内のメッセージとリッチコンテンツをパーソナライズします。 例えば、[!DNL Experience Platform] プロファイルデータを活用して、[!DNL Airship] 内で場所属性を設定します。 これにより、ホテルのブランドは各ユーザーに最も近いホテルの場所の画像を表示できます。

### 使用例#2

Adobe Experience Platformの属性を活用して、[!DNL Airship] プロファイルをさらに強化し、SDK または [!DNL Airship] 予測データと組み合わせます。 例えば、ある小売業者は、忠誠度ステータスと場所データ（プラットフォームの属性）と [!DNL Airship] チャーンデータが予測されるセグメントを作成して、ラスベガス (NV) に住み、チャーンの確率が高い金忠誠度ステータスのユーザーに高いメッセージを送信できます。

## 宛先に接続 {#connect}

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、[ ストリーミングセグメントの書き出し先へのオーディエンスデータのアクティブ化 ](../../ui/activate-segment-streaming-destinations.md) を参照してください。

### 接続パラメーター {#parameters}

[ この宛先を設定 ](../../ui/connect-destination.md) する際は、次の情報を指定する必要があります。

* **[!UICONTROL Bearer トークン]**:ダッシュボードから生成した bearer ト [!DNL Airship] ークン。
* **[!UICONTROL 名前]**:この宛先を識別するのに役立つ名前を入力します。
* **[!UICONTROL 説明]**:この宛先の説明を入力します。
* **[!UICONTROL ドメイン]**:この宛先に適用するデータセンターに応じて、米国または EU [!DNL Airship] のデータセンターを選択します。

## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、[ ストリーミングセグメントの書き出し先へのオーディエンスデータのアクティブ化 ](../../ui/activate-segment-streaming-destinations.md) を参照してください。

## マッピングの考慮事項 {#mapping-considerations}

[!DNL Airship] 属性は、デバイスインスタンス (iPhoneなど ) を表すチャネルで設定できます。また、すべてのユーザーのデバイスを顧客 ID などの共通の識別子にマッピングする名前付きユーザーでも設定できます。スキーマにプライマリ ID としてプレーンテキスト（ハッシュ化されていない）の電子メールアドレスがある場合は、**[!UICONTROL ソース属性]** の電子メールフィールドを選択し、以下に示すように、**[!UICONTROL ターゲット ID]** の下の右の列の [!DNL Airship] にマッピングします。

![名前付きユーザーマッピング](../../assets/catalog/mobile-engagement/airship/mapping.png)

チャネルにマッピングする必要がある識別子（デバイスなど）の場合、ソースに基づく適切なチャネルにマッピングします。 次の図は、2 つのマッピングの作成方法を示しています。

* [!DNL Airship] iOSチャネルに対する IDFA iOS広告 ID
* Adobe`fullName` 属性を [!DNL Airship] &quot;Full Name&quot;属性に設定

>[!NOTE]
>
>属性マッピングのターゲットフィールドを選択する際に [!DNL Airship] ダッシュボードに表示されるわかりやすい名前を使用します。

**ID のマッピング**

ソースフィールドを選択：

![飛行船属性に接続](../../assets/catalog/mobile-engagement/airship/select-source-identity.png)

ターゲットフィールドを選択：

![飛行船属性に接続](../../assets/catalog/mobile-engagement/airship/select-target-identity.png)

**Map 属性**

ソース属性を選択：

![ソースフィールドの選択](../../assets/catalog/mobile-engagement/airship/select-source-attributes.png)

ターゲット属性を選択：

![ターゲットフィールドの選択](../../assets/catalog/mobile-engagement/airship/select-target-attribute.png)

マッピングの検証：

![チャネルマッピング](../../assets/catalog/mobile-engagement/airship/mapping.png)


## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] の宛先はすべて、データを処理する際のデータ使用ポリシーに準拠しています。 [!DNL Adobe Experience Platform] によるデータガバナンスの強制方法について詳しくは、「[ データガバナンスの概要 ](../../../data-governance/home.md)」を参照してください。
