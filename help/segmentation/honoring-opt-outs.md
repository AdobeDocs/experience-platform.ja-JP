---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: オプトアウトの実行
topic: overview
translation-type: tm+mt
source-git-commit: f156679601c2ed0bb933a66a56661c29c1b9c778
workflow-type: tm+mt
source-wordcount: '954'
ht-degree: 0%

---


# セグメントでのオプトアウトリクエストの実行

[!DNL Experience Platform] 顧客が内部でのデータの使用状況およびストレージに関するオプトアウトリクエストを送信でき [!DNL Real-time Customer Profile]ます。 これらのオプトアウト要求は [!DNL California Consumer Privacy Act] (CCPA)の一部であり、カリフォルニア州の住民は、自分の個人データにアクセスして削除する権利を持ち、自分の個人データが販売されているか、また誰に開示されているかを知ることができます。

顧客がオプトアウトしたら、マーケティングアクティビティ用のオーディエンスを生成する際に、組織がそれらのオプトアウトに従うことが重要です。 このドキュメントでは、オプトアウトリクエストの実行に関する重要な詳細について説明します。

## はじめに

オプトアウト要求を実行するには、関連する様々な [!DNL Adobe Experience Platform] サービスについての理解が必要です。 オプトアウト要求を処理する前に、次のサービスに関するドキュメントを確認してください。

- [!DNL Real-time Customer Profile](../profile/home.md): 複数のソースからの集計データに基づいて、リアルタイムで統合された顧客プロファイルを提供します。
- [!DNL Adobe Experience Platform Segmentation Service](./home.md): データからオーディエンスセグメントを作成でき [!DNL Real-time Customer Profile] ます。
- [!DNL Experience Data Model (XDM)](../xdm/home.md): Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
- [!DNL Adobe Experience Platform Privacy Service](../privacy-service/home.md): 組織が、内部の顧客データに関するデータのプライバシー規制へのコンプライアンスを自動化できるよう支援 [!DNL Platform]します。

## オプトアウトミックスイン

CCPAオプトアウト要求を受け入れるために、和集合スキーマの一部であるスキーマの1つに、必要な [!DNL Experience Data Model] (XDM)オプトアウトフィールドが含まれている必要があります。 スキーマにオプトアウトフィールドを追加するために使用できるミックスインが2つあります。それぞれのミックスインについて詳しくは、以下の節で説明します。

- [プロファイルのプライバシー](#profile-privacy): 様々なオプトアウトタイプ（一般または販売/共有）を取り込むために使用します。
- [プロファイル環境設定の詳細](#profile-preferences-details): 特定のXDMチャネルのオプトアウト要求を取り込むために使用します。

スキーマにmixinを追加する手順については、次のXDMドキュメントの「mixin」追加の節を参照してください。
- [スキーマレジストリAPIのチュートリアル](../xdm/api/getting-started.md)。: スキーマレジストリAPIを使用したスキーマの構築
- [スキーマエディタのチュートリアル](../xdm/tutorials/create-schema-ui.md): Platformユーザーインターフェイスを使用したスキーマの作成。

ユーザーインターフェイスのスキーマに追加されたオプトアウトミックスインの例を次に示します。

![](images/opt-outs/opt-out-mixins-user-interface.png)

各ミックスインの構造と、各ミックスインがスキーマに貢献するフィールドの説明を、以下の節で詳しく説明します。

### [!DNL Profile Privacy]

Mixinを使用すると、次の2種類のCCPAオプトアウトリクエストを顧客から取得できます。 [!DNL Profile Privacy]

1. 一般的なオプトアウト
2. 販売/共有のオプトアウト

![](images/opt-outs/profile-privacy.png)

この [!DNL Profile Privacy] ミックスインには次のフィールドが含まれています。

- プライバシーオプトアウト(`privacyOptOuts`): オプトアウトオブジェクトのリストを含む配列。
- オプトアウトのタイプ(`optOutType`): オプトアウトのタイプ。 このフィールドは列挙型で、2つの値を指定できます。
   - 一般的なオプトアウト(`general_opt_out`)
   - 販売共有オプトアウト(`sales_sharing_opt_out`)
- オプトアウト値(`optOutValue`): 指定したオプトアウトタイプに基づくオプトアウトのアクティブ状態（オプトアウト信号の値とも呼ばれます）。 このフィールドは、4つの値を持つ列挙です。
   - 提供なし(`not_provided`): オプトアウト要求が提供されていません。
   - 保留中の検証(`pending`): オプトアウト要求は、検証待ちです。
   - オプトアウト(`out`): 顧客がオプトアウトしました。
   - オプトイン(`in`): 顧客がオプトインしました。
- オプトアウトタイムスタンプ(`timestamp`): 受信したオプトアウト信号のタイムスタンプ。

Mixinの完全な構造を表示するには、 [!DNL Profile Privacy] XDMのパブリックGitHubリポジトリ [](https://github.com/adobe/xdm/blob/master/schemas/context/profile-privacy.schema.json) 、またはPlatformUIを使用してMixinをプレビューする方法を参照してください。

### プロファイルの環境設定の詳細

プロファイル環境設定の詳細ミックスインには、顧客プロファイルに対する環境設定を表すいくつかのフィールドが用意されています(E メールフォーマット、好みの言語、タイムゾーンなど)。 このミックスインに含まれるフィールドの1つであるOptInOut (`optInOut`)を使用すると、個々のチャネルに対してオプトアウト値を設定できます。

![](images/opt-outs/profile-preferences-details.png)

「プロファイル環境設定の詳細」ミックスインには、オプトアウトに関連する次のフィールドが含まれています。

- OptInOut (`optInOut`): 各キーが通信チャネルの有効で既知のURIを表すオブジェクトで、各チャネルのオプトアウトのアクティブ状態が表します。 各チャネルには、次の4つの値のいずれかを指定できます。
   - 提供なし(`not_provided`): このチャネルのオプトアウト要求は提供されていません。
   - 保留中の検証(`pending`): このチャネルのオプトアウト要求は、検証待ちです。
   - オプトアウト(`out`): お客様はこのチャネルをオプトアウトしました。
   - オプトイン(`in`): お客様はこのチャネルにオプトインしました。
- グローバルオプトアウト(`globalOptout`): boolean値です。trueに設定した場合、プロファイルのグローバルオプトアウトの上書きを設定します。 このフィールドのデフォルト値はfalseです。

次の例のJSONは、OptInOutオブジェクトが様々な通信チャネル用に複数のオプトアウト信号を取り込む方法を示しています。

```json
{
  "xdm:optInOut": {
    "https://ns.adobe.com/xdm/channels/email": "pending",
    "https://ns.adobe.com/xdm/channels/phone": "out",
    "https://ns.adobe.com/xdm/channels/sms": "in",
    "https://ns.adobe.com/xdm/channels/fax": "not_provided",
    "https://ns.adobe.com/xdm/channels/direct-mail": "not_provided",
    "https://ns.adobe.com/xdm/channels/apns": "not_provided",
    "xdm:globalOptout": false
  }
}
```

プロファイル環境設定の詳細Mixinの完全な構造を表示するには、 [XDMパブリックGitHubリポジトリ](https://github.com/adobe/xdm/blob/master/schemas/context/profile-preferences-details.schema.json) 、または [!DNL Platform] UIを使用してMixinをプレビューしてください。

## セグメントでのオプトアウトの処理

CCPAオプトアウトフラグが付いたプロファイルをセグメントに含めないようにするには、セグメントの作成時に、既存のセグメントに特別なフィールドを追加するか、セグメントの作成時に含める必要があります。

以下の節では、2種類のオプトアウトフラグに対して適切なフィールドを追加する方法を示します。
1. 一般的なオプトアウト
2. 販売/共有のオプトアウト

### 一般的なオプトアウト

[!DNL Segmentation] は、「[!UICONTROL 一般的なオプトアウト]」フラグを含むすべてのプロファイルに対して自動的に優先します。つまり、これらのプロファイルは、デフォルトでオーディエンスやエクスポートに含まれません。 ただし、オプトアウトプロファイルがオーディエンスやマーケティングアクティビティに含まれないように、適切なフィールドを追加することをお勧めします。

これは、ユーザーインターフェイスを使用して、 **[!UICONTROL プライバシーオプトアウト]** 属性を追加することで行うことができます。 この場合、セグメントには、オプトインしたユーザーのみが含まれるように設定されます(つまり、プロファイルに一般的なオプトアウトフラグが設定されていません)。 これは、「[!UICONTROL オプトアウトタイプ]」が「[!UICONTROL 一般的なオプトアウト」、「]オプトアウト値[!UICONTROL 」が「]オプトイン宣言」に等しいことで行われます。

![](images/opt-outs/segment-general-opt-out.png)

### 販売/共有のオプトアウト

ユーザーのプロファイルに販売/共有のオプトアウトフラグが設定されている場合、このプロファイルはセグメントの作成やマーケティングのアクティビティには使用できません。 このフラグが確実に適用されるように、「[!UICONTROL Opt-Out Type]」が「[!UICONTROL Sales Sharing Opt-Out]」、「[!UICONTROL Opt-Out Value]」が「Opt-in Opt-in Pont-in Type」と等しい必要があります。

![](images/opt-outs/segment-sales-sharing-opt-out.png)

<!-- ### Overriding default exclusions

In some instances, such as building a segment of people who have opted out, it may be necessary to override the default exclusion of opted-out profiles. This override can be done via the API or in the Segment Builder user interface. -->

## 次の手順

APIとユーザーインターフェイスを使用したセグメント定義やオーディエンスの操作を含む、セグメント化について詳しくは、 [セグメント化の概要を読んでください](./home.md)。

プライバシーに関する法的規制や組織のプライバシー規制への自動コンプライアンスを容易にする方法など、内部のデータのプライバシーに関する詳細は、に関するドキュメントを参照して [!DNL Platform]ください [!DNL Privacy Service][!DNL Privacy Service](../privacy-service/home.md)。
