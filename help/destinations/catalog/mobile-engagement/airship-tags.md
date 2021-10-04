---
keywords: 航空船タグ；飛行船の宛先
title: 航空船タグ接続
description: Adobeの閲覧者データを Airship に Audience Tags としてシームレスに渡し、Airship 内でターゲティングをおこないます。
exl-id: 84cf5504-f0b5-48d8-8da1-ff91ee1dc171
source-git-commit: a765f6829f08f36010e0e12a7186bf5552dfe843
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 0%

---

# [!DNL Airship Tags] 接続 {#airship-tags-destination}

## 概要

[!DNL Airship] は、顧客ライフサイクルの各段階で、意味のあるパーソナライズされたチャネルメッセージをユーザーに届けるのを支援する、最先端の顧客エンゲージメントプラットフォームです。

この統合では、Adobe Experience Platformのセグメントデータを [!DNL Airship] に [Tags](https://docs.airship.com/guides/audience/tags/) として渡し、ターゲティングやトリガーをおこないます。

[!DNL Airship] の詳細については、[ 航空船のドキュメント ](https://docs.airship.com) を参照してください。


>[!TIP]
>
>このドキュメントページは [!DNL Airship] チームが作成しました。 お問い合わせや更新のご依頼は、[support.airship.com](https://support.airship.com/) まで直接お問い合わせください。

## 前提条件

Adobe Experience Platformセグメントを [!DNL Airship] に送信する前に、次の操作をおこなう必要があります。

* [!DNL Airship] プロジェクトにタググループを作成します。
* 認証用の bearer トークンを生成します。

>[!TIP]
> 
>[ このサインアップリンク ](https://go.airship.eu/accounts/register/plan/starter/) を介して [!DNL Airship] アカウントを作成します（まだ作成していない場合）。

## タググループ

Adobe Experience Platform のセグメントの概念は、Airship の [ タグ ](https://docs.airship.com/guides/audience/tags/) に似ていますが、実装にわずかな違いがあります。 この統合は、Experience Platformセグメント ](../../../xdm/field-groups/profile/segmentation.md) 内のユーザーの [ メンバーシップのステータスを、[!DNL Airship] タグの有無にマッピングします。 例えば、`xdm:status` が `realized` に変わった Platform セグメントでは、タグが [!DNL Airship] チャネルに追加されるか、このプロファイルのマッピング先のユーザーに名前を付けます。 `xdm:status` が `exited` に変わると、タグは削除されます。

この統合を有効にするには、`adobe-segments` という名前の *タググループ* を [!DNL Airship] に作成します。

>[!IMPORTANT]
>
>新しいタググループを作成する際には、**&quot;[!DNL Allow these tags to be set only from your server]&quot;と表示されるラジオボタンを** チェックしないでください。 これをおこなうと、Adobeタグの統合が失敗します。

タググループの作成手順については、[ タググループの管理 ](https://docs.airship.com/tutorials/manage-project/messaging/tag-groups) を参照してください。

## bearer トークンの生成

[Airship ダッシュボード ](https://go.airship.com) の **[!UICONTROL 設定]**&quot; **[!UICONTROL API と統合]** に移動し、左側のメニューで **[!UICONTROL トークン]** を選択します。

「**[!UICONTROL トークンを作成]**」をクリックします。

トークンのわかりやすい名前 ( 例：「Adobeタグの宛先」) を指定し、役割に「すべてのアクセス」を選択します。

「**[!UICONTROL トークンを作成]**」をクリックし、詳細を機密として保存します。

## ユースケース

[!DNL Airship Tags] の宛先をいつどのように使用するかをより深く理解できるように、Adobe Experience Platformのお客様がこの宛先を使用して解決できる使用例を以下に示します。

### 使用例#1

小売業者やエンターテイメントプラットフォームは、ロイヤルティ顧客のユーザープロファイルを作成し、それらのセグメントを [!DNL Airship] に渡してモバイルキャンペーンのメッセージターゲティングをおこなうことができます。

### 使用例#2

トリガーがAdobe Experience Platform内の特定のセグメントに含まれたり、特定のセグメントから除外されたりした場合に、1 対 1 のメッセージをリアルタイムで送信できます。

例えば、ある小売業者は、Platform でジーンズのブランド固有のセグメントを設定します。 その小売業者は、誰かがジーンズの好みを特定のブランドに設定したらすぐに、モバイルメッセージをトリガーできるようになりました。

## 宛先に接続 {#connect}

この宛先に接続するには、[ 宛先の設定に関するチュートリアル ](../../ui/connect-destination.md) で説明されている手順に従います。

### 接続パラメーター {#parameters}

[ この宛先を設定 ](../../ui/connect-destination.md) する際は、次の情報を指定する必要があります。

* **[!UICONTROL Bearer トークン]**:ダッシュボードから生成した bearer ト [!DNL Airship] ークン。
* **[!UICONTROL 名前]**:この宛先を識別するのに役立つ名前を入力します。
* **[!UICONTROL 説明]**:この宛先の説明を入力します。
* **[!UICONTROL ドメイン]**:この宛先に適用するデータセンターに応じて、米国または EU [!DNL Airship] のデータセンターを選択します。


## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、[ ストリーミングセグメントの書き出し先へのオーディエンスデータのアクティブ化 ](../../ui/activate-segment-streaming-destinations.md) を参照してください。

## マッピングの考慮事項 {#mapping-considerations}

[!DNL Airship] タグは、デバイスインスタンス（iPhone など）を表すチャネル上で設定できます。また、すべてのユーザーのデバイスを顧客 ID などの共通の識別子にマッピングする名前付きユーザー上でも設定できます。スキーマにプライマリ ID としてプレーンテキスト（ハッシュ化されていない）の電子メールアドレスがある場合は、**[!UICONTROL ソース属性]** の電子メールフィールドを選択し、以下に示すように、**[!UICONTROL ターゲット ID]** の下の右の列の [!DNL Airship] にマッピングします。

![名前付きユーザーマッピング](../../assets/catalog/mobile-engagement/airship-tags/mapping-option-2.png)

チャネルにマッピングする必要がある識別子（デバイスなど）の場合、ソースに基づく適切なチャネルにマッピングします。 次の画像は、Google 広告 ID を [!DNL Airship] Android チャネルにマッピングする方法を示しています。

![航空船タグに接続航](../../assets/catalog/mobile-engagement/airship-tags/select-source-identity.png)
![空船タグに接続チャ](../../assets/catalog/mobile-engagement/airship-tags/select-target-identity.png)
![ネルマッピング](../../assets/catalog/mobile-engagement/airship-tags/mapping-option.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] の宛先はすべて、データを処理する際のデータ使用ポリシーに準拠しています。 [!DNL Adobe Experience Platform] によるデータガバナンスの強制方法について詳しくは、[ データガバナンスの概要 ](../../../data-governance/home.md) を参照してください。
