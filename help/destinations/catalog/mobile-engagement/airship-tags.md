---
keywords: 飛行船タグ；飛行船の目的地
title: 航空船タグの接続
description: AdobeオーディエンスデータをAirship内でのターゲティング用のオーディエンスタグとしてAirshipにシームレスに渡します。
exl-id: 84cf5504-f0b5-48d8-8da1-ff91ee1dc171
translation-type: tm+mt
source-git-commit: ab0798851e5f2b174d9f4241ad64ac8afa20a938
workflow-type: tm+mt
source-wordcount: '1183'
ht-degree: 13%

---

# （ベータ版） [!DNL Airship Tags]接続{#airship-tags-destination}

>[!IMPORTANT]
>
>Adobe Experience Platformの[!DNL Airship Tags]宛先は現在ベータ版です。 ドキュメントと機能は変更される場合があります。

## 概要

[!DNL Airship] は、顧客関与プラットフォームをリードし、顧客のライフサイクルのあらゆる段階で、意味のあるパーソナライズされたチャネルのメッセージをユーザに届けるのに役立ちます。

この統合は、ターゲティングまたはトリガーのために、Adobe Experience Platformセグメントデータを[!DNL Airship][タグ](https://docs.airship.com/guides/audience/tags/)としてに渡します。

[!DNL Airship]の詳細については、[航空船ドキュメント](https://docs.airship.com)を参照してください。


>[!TIP]
>
>このドキュメントページは[!DNL Airship]チームが作成しました。 お問い合わせや更新のご依頼は、[support.airship.com](https://support.airship.com/)まで直接お問い合わせください。

## 前提条件

Adobe Experience Platformセグメントを[!DNL Airship]に送信する前に、次の操作を行う必要があります。

* [!DNL Airship]プロジェクトにタググループを作成します。
* 認証用のベアラートークンを生成します。

>[!TIP]
> 
>まだ[!DNL Airship]アカウントを作成していない場合は、[このサインアップリンク](https://go.airship.eu/accounts/register/plan/starter/)を経由してアカウントを作成します。

## タググループ

Adobe Experience Platformのセグメントの概念は、Airshipの[タグ](https://docs.airship.com/guides/audience/tags/)と似ていますが、実装にわずかな違いがあります。 この統合は、Experience Platformセグメント](../../../xdm/field-groups/profile/segmentation.md)のユーザーの[メンバーシップのステータスを、[!DNL Airship]タグの存在または存在しない状態にマッピングします。 例えば、`xdm:status`が`realized`に変わるプラットフォームセグメントでは、タグは[!DNL Airship]チャネルに追加されるか、このプロファイルがマッピングされる名前を付けます。 `xdm:status`が`exited`に変更されると、タグは削除されます。

この統合を有効にするには、*タググループ*&#x200B;を[!DNL Airship]内に`adobe-segments`という名前で作成します。

>[!IMPORTANT]
>
>新しいタググループ&#x200B;**を作成する場合、「[!DNL Allow these tags to be set only from your server]」と表示されるラジオボタンを**&#x200B;チェックしないでください。 これを行うと、Adobeタグの統合が失敗します。

タググループの作成手順については、[タググループの管理](https://docs.airship.com/tutorials/manage-project/messaging/tag-groups)を参照してください。

## ベアラートークンの生成

[Airshipダッシュボード](https://go.airship.com)の&#x200B;**[!UICONTROL 設定]**&quot; **[!UICONTROL APIs &amp; Integrations]**&#x200B;に移動し、左側のメニューで&#x200B;**[!UICONTROL トークン]**&#x200B;を選択します。

「**[!UICONTROL トークンを作成]**」をクリックします。

トークンにわかりやすい名前(「Adobeタグの宛先」など)を指定し、ロールに「すべてのアクセス」を選択します。

「**[!UICONTROL トークン]**&#x200B;を作成」をクリックし、詳細を機密情報として保存します。

## 使用例

[!DNL Airship Tags]の行き先の使い方と使い方を理解するために、Adobe Experience Platformのお客様がこの行き先を使って解決できる使用例を以下に示します。

### 使用例1

小売業者やエンターテイメントプラットフォームは、忠誠度の高い顧客に対してユーザープロファイルを作成し、それらのセグメントを[!DNL Airship]に渡して、モバイルキャンペーンに対するメッセージのターゲット設定を行うことができます。

### 使用例2

トリガーがAdobe Experience Platform内の特定のセグメントに入る、または離れるときに、1対1のメッセージをリアルタイムで表示します。

例えば、ある小売業者がプラットフォームでジーンズブランド固有のセグメントを設定するとします。 あの小売店では、誰かがジーンズの好みを特定のブランドにしたらすぐにモバイルメッセージをトリガーできるようになりました。

## [!DNL Airship Tags] {#connect-airship-tags}に接続

**[!UICONTROL 宛先]**/**[!UICONTROL カタログ]**&#x200B;で、**[!UICONTROL モバイルエンゲージメント]**&#x200B;カテゴリまでスクロールします。 「**[!DNL Airship Tags]**」を選択し、「**[!UICONTROL 設定]**」を選択します。

>[!NOTE]
>
>この宛先との接続が既に存在する場合は、宛先カードに「**[!UICONTROL Activate]**」ボタンが表示されます。 「**[!UICONTROL アクティブ化]**」と「**[!UICONTROL 設定]**」の違いについて詳しくは、保存先のワークスペースドキュメントの「[カタログ](../../ui/destinations-workspace.md#catalog)」の節を参照してください。

![航空船タグに接続](../../assets/catalog/mobile-engagement/airship-tags/catalog.png)

**アカウント**&#x200B;の手順で、[!DNL Airship Tags]宛先への接続を事前に設定している場合は、「**[!UICONTROL 既存のアカウント]**」を選択し、既存の接続を選択します。 または、「**[!UICONTROL 新しいアカウント]**」を選択して、[!DNL Airship Tags]への新しい接続を設定できます。 **[!UICONTROL 宛先]**&#x200B;に接続を選択し、[!DNL Airship]ダッシュボードから生成したベアラトークンを使用して、[!DNL Airship]プロジェクトにAdobe Experience Platformを接続します。

>[!NOTE]
>
>Adobe Experience Platformは、認証プロセスで資格情報の検証をサポートし、[!DNL Airship]アカウントに正しくない資格情報を入力するとエラーメッセージを表示します。 このため、間違った資格情報を使用すると、ワークフローを完了することができません。

![航空船タグに接続](../../assets/catalog/mobile-engagement/airship-tags/connect-account.png)

資格情報が確認され、Adobe Experience Platformが[!DNL Airship]プロジェクトに接続されたら、**[!UICONTROL 次へ]**&#x200B;を選択して&#x200B;**[!UICONTROL セットアップ]**&#x200B;の手順に進むことができます。

**[!UICONTROL 認証]**&#x200B;手順で、アクティベーションフローの&#x200B;**[!UICONTROL 名前]**&#x200B;と&#x200B;**[!UICONTROL 説明]**&#x200B;を入力します。

また、この手順では、この宛先に適用する[!DNL Airship]データセンターに応じて、米国またはEUデータセンターを選択できます。 最後に、データをエクスポート先にエクスポートする1つ以上の&#x200B;**[!UICONTROL マーケティングアクション]**&#x200B;を選択します。 Adobe定義のマーケティングアクションから選択するか、独自のマーケティングアクションを作成することができます。 マーケティングアクションについて詳しくは、[データ使用ポリシーの概要](../../../data-governance/policies/overview.md)を参照してください。

上記のフィールドに入力したら、「**[!UICONTROL 宛先を作成]**」を選択します。

![航空船タグに接続](../../assets/catalog/mobile-engagement/airship-tags/select-domain.png)

これで宛先が作成されました。後でセグメントをアクティブにする場合は、「**[!UICONTROL 保存して終了]**」を選択します。また、「**[!UICONTROL 次へ]**」を選択してワークフローを続行し、アクティブ化するセグメントを選択することもできます。どちらの場合も、残りのワークフローについては、次の[セグメントをアクティブにする](#activate-segments)の節を参照してください。

## セグメントのアクティブ化 {#activate-segments}

[!DNL Airship Tags]にセグメントをアクティブ化するには、次の手順に従います。

**[!UICONTROL 宛先／参照]**&#x200B;で、セグメントをアクティブ化する宛先を選択します。[!DNL Airship Tags]

![活性化流](../../assets/catalog/mobile-engagement/airship-tags/browse.png)

宛先の名前をクリックします。これにより、「アクティブ化」のフローに移動します。


宛先に対するアクティベーションフローが既に存在する場合は、その宛先に現在送信されているセグメントを確認できます。右側のパネルで「**[!UICONTROL アクティベーションの編集]**」を選択し、以下の手順に従ってアクティベーションの詳細を変更します。

![活性化流](../../assets/catalog/mobile-engagement/airship-tags/activate.png)

「**[!UICONTROL アクティブ化]**」を選択します。 **[!UICONTROL 宛先]**&#x200B;をアクティブにするワークフローの&#x200B;**[!UICONTROL セグメントを選択]**&#x200B;ページで、[!DNL Airship Tags]に送信するセグメントを選択します。

![segments-to-destination](../../assets/catalog/mobile-engagement/airship-tags/select-segments.png)

**[!UICONTROL マッピング]**&#x200B;手順で、[XDM](../../../xdm/home.md)スキーマーから宛先スキーマーにマッピングする属性とIDを選択します。 **[!UICONTROL 追加新しいマッピング]**&#x200B;を選択して、スキーマを参照し、対応するターゲットIDにマッピングします。

![IDマッピング初期画面](../../assets/catalog/mobile-engagement/airship-tags/identity-mapping.png)

[!DNL Airship] タグは、デバイスインスタンスを表すチャネル（iPhoneなど）またはすべてのユーザーのデバイスを顧客IDなどの共通の識別子にマップする名前付きユーザーに設定できます。スキーマの主なIDとしてテキスト形式の（ハッシュ化されていない）電子メールアドレスがある場合は、**[!UICONTROL ソース属性]**&#x200B;で電子メールフィールドを選択し、**[!UICONTROL ターゲットID]**&#x200B;の右の列の[!DNL Airship]にマップします。

![名前付きユーザーマッピング](../../assets/catalog/mobile-engagement/airship-tags/mapping-option-2.png)

チャネルにマップする必要がある識別子（デバイスなど）の場合、ソースに基づいて適切なチャネルにマップします。 次の画像は、Google広告IDを[!DNL Airship] Androidチャネルにマップする方法を示しています。

![Airship](../../assets/catalog/mobile-engagement/airship-tags/select-source-identity.png)
![タグに接続Airshipタグに接続](../../assets/catalog/mobile-engagement/airship-tags/select-target-identity.png)
![チャネルマッピング](../../assets/catalog/mobile-engagement/airship-tags/mapping-option.png)

**[!UICONTROL セグメントスケジュール]**&#x200B;ページで、現在、スケジュールは無効になっています。 「**[!UICONTROL 次へ]**」をクリックしてレビュー手順に進みます。

「**[!UICONTROL 確認]**」ページには、選択の概要が表示されます。「**[!UICONTROL キャンセル]**」を選択してフローを分割するか、「**[!UICONTROL 戻る]**」を選択して設定を変更する、または、「**[!UICONTROL 完了]**」を選択して確定し、宛先へのデータの送信を開始します。

>[!IMPORTANT]
>
>この手順では、Adobe Experience Platformがデータ使用ポリシーの違反を確認します。 次に、ポリシー違反の例を示します。 セグメントアクティベーションのワークフローは、違反を解決するまで完了できません。 ポリシー違反の解決方法について詳しくは、「データ管理ドキュメント」の「[ポリシーの適用](../../../data-governance/enforcement/auto-enforcement.md)」を参照してください。

![confirm-selection](../../assets/common/data-policy-violation.png)

ポリシー違反が検出されなかった場合は、[**[!UICONTROL 完了]**]を選択して、選択と開始が宛先にデータを送信することを確認します。

![confirm-selection](../../assets/catalog/mobile-engagement/airship-tags/review.png)


## データの使用とガバナンス{#data-usage-governance}

すべての[!DNL Adobe Experience Platform]宛先は、データ処理時のデータ使用ポリシーに準拠しています。 [!DNL Adobe Experience Platform]によるデータガバナンスの適用方法について詳しくは、[データガバナンスの概要](../../../data-governance/home.md)を参照してください。
