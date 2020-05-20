---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: オプトアウトの実行
topic: overview
translation-type: tm+mt
source-git-commit: 902ba5efbb5f18a2de826fffd023195d804309cc
workflow-type: tm+mt
source-wordcount: '1066'
ht-degree: 0%

---


# セグメントでのオプトアウトリクエストの実行

エクスペリエンスプラットフォームを使用すると、顧客は、データの使用状況やストレージに関するオプトアウトリクエストをリアルタイムの顧客プロファイル内で送信できます。 これらのオプトアウト要求は、カリフォルニア州消費者プライバシー法(CCPA)の一部で、カリフォルニア州の住民に対して、個人データにアクセスして削除する権利と、個人データが販売されているか（そして誰に対して）開示されるかを知る権利を提供します。

顧客がオプトアウトしたら、マーケティングアクティビティ用のオーディエンスを生成する際に、組織がそれらのオプトアウトに従うことが重要です。 このドキュメントでは、オプトアウトリクエストの実行に関する重要な詳細について説明します。

## はじめに

オプトアウトリクエストを実行するには、関連する様々なAdobe Experience Platformサービスについて理解している必要があります。 オプトアウト要求を処理する前に、次のサービスに関するドキュメントを確認してください。

- [リアルタイム顧客プロファイル](../profile/home.md): 複数のソースからの集計データに基づいて、リアルタイムで統合された顧客プロファイルを提供します。
- [Adobe Experience Platform Segmentation Service](./home.md): リアルタイム顧客プロファイルデータからオーディエンスセグメントを作成できます。
- [Experience Data Model(XDM)](../xdm/home.md): プラットフォームが顧客体験データを編成する際に使用する標準化されたフレームワーク。
- [Adobe Experience Platform Privacy Service](../privacy-service/home.md): 組織は、プラットフォーム内の顧客データに関するデータのプライバシーに関する規制へのコンプライアンスを自動化できます。 次の規則が含まれます。
   - カリフォルニア消費者プライバシー法(CCPA): カリフォルニア在住者に対するデータのプライバシー権。個人データにアクセスして削除する権利や、個人データが販売されているか、または誰に開示されているかを知る権利など。
   - GDPR(General Data Protection Regulation): 「アクセス権」、「忘れられる権利」を含む、欧州和集合のメンバーに対するデータプライバシー権。

## オプトアウトミックスイン

CCPAオプトアウト要求に従うために、和集合スキーマの一部であるスキーマの1つに、必要なExperience Data Model(XDM)オプトアウトフィールドが含まれている必要があります。 スキーマにオプトアウトフィールドを追加するために使用できるミックスインが2つあります。それぞれのミックスインについて詳しくは、以下の節で説明します。

- [プロファイルのプライバシー](#profile-privacy): 様々なオプトアウトタイプ（一般または販売/共有）を取り込むために使用します。
- [プロファイル環境設定の詳細](#profile-preferences-details): 特定のXDMチャネルのオプトアウト要求を取り込むために使用します。

スキーマにmixinを追加する手順については、次のXDMドキュメントの「mixin」追加の節を参照してください。
- [スキーマレジストリAPIのチュートリアル](../xdm/api/getting-started.md)。: スキーマレジストリAPIを使用したスキーマの構築
- [スキーマエディタのチュートリアル](../xdm/tutorials/create-schema-ui.md): Platformユーザーインターフェイスを使用したスキーマの作成。

ユーザーインターフェイスのスキーマに追加されたオプトアウトミックスインの例を次に示します。

![](images/opt-outs/opt-out-mixins-user-interface.png)

各ミックスインの構造と、各ミックスインがスキーマに貢献するフィールドの説明を、以下の節で詳しく説明します。

### プロファイルのプライバシー

プロファイルプライバシーミックスインを使用すると、顧客から2種類のCCPAオプトアウトリクエストを取り込むことができます。

1. 一般的なオプトアウト
2. 販売/共有のオプトアウト

![](images/opt-outs/profile-privacy.png)

プロファイルプライバシーミックスインには次のフィールドが含まれています。

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

プロファイルプライバシーミックスインの完全な構造を表示するには、 [XDMパブリックGitHubリポジトリ](https://github.com/adobe/xdm/blob/master/schemas/context/profile-privacy.schema.json) 、またはプラットフォームUIを使用したミックスインのプレビューを参照してください。

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

プロファイルの環境設定の詳細Mixinの完全な構造を表示するには、 [XDMパブリックGitHubリポジトリ](https://github.com/adobe/xdm/blob/master/schemas/context/profile-preferences-details.schema.json) 、またはPlatform UIを使用してMixinをプレビューしてください。

## セグメントでのオプトアウトの処理

CCPAオプトアウトフラグが付いたプロファイルをセグメントに含めないようにするには、セグメントの作成時に、既存のセグメントに特別なフィールドを追加するか、セグメントの作成時に含める必要があります。

以下の節では、2種類のオプトアウトフラグに対して適切なフィールドを追加する方法を示します。
1. 一般的なオプトアウト
2. 販売/共有のオプトアウト

### 一般的なオプトアウト

セグメントは、「一般的なオプトアウト」フラグを含むすべてのプロファイルに自動的に従います。つまり、これらのプロファイルは、デフォルトでオーディエンスやエクスポートに含まれません。 ただし、オプトアウトプロファイルがオーディエンスやマーケティングアクティビティに含まれないように、適切なフィールドを追加することをお勧めします。

これは、セグメントビルダーのユーザーインターフェイスで「 **プライバシーオプトアウト** 」属性を追加することで行うことができます。 この場合、セグメントには、オプトインを許可したユーザーのみが含まれるように設定されます(つまり、プロファイルに一般的なオプトアウトフラグが設定されていません)。 これは、「オプトアウトタイプ」が「一般的なオプトアウト」、「オプトアウト値」が「オプトイン」に等しいと宣言することで行われます。

![](images/opt-outs/segment-general-opt-out.png)

### 販売/共有のオプトアウト

ユーザーのプロファイルに販売/共有のオプトアウトフラグが設定されている場合、このプロファイルはセグメントの作成やマーケティングのアクティビティには使用できません。 このフラグが確実に適用されるように、「オプトアウトタイプ」が「販売共有オプトアウト」、「オプトアウト値」が「オプトイン」に等しい必要があります。

![](images/opt-outs/segment-sales-sharing-opt-out.png)

<!-- ### Overriding default exclusions

In some instances, such as building a segment of people who have opted out, it may be necessary to override the default exclusion of opted-out profiles. This override can be done via the API or in the Segment Builder user interface. -->

## 次の手順

APIとユーザーインターフェイスを使用したセグメント定義やオーディエンスの操作を含む、セグメント化について詳しくは、 [セグメント化の概要を読んでください](./home.md)。

プラットフォーム内のデータのプライバシーに関する詳細は、プライバシーサービスが法的および組織のプライバシー規制への自動コンプライアンスを支援する方法など、 [プライバシーサービスのドキュメントを参照してください](../privacy-service/home.md)。
