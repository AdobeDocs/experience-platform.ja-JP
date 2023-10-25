---
title: イベント転送でのシークレットの設定
description: イベント転送のプロパティで使用されるエンドポイントを認証するために UI でシークレットを設定する方法について説明します。
exl-id: eefd87d7-457f-422a-b159-5b428da54189
source-git-commit: a863d65c3e6e330254a58aa822383c0847b0e5f5
workflow-type: tm+mt
source-wordcount: '2182'
ht-degree: 85%

---

# イベント転送でのシークレットの設定

イベント転送では、秘密鍵は別のシステムの資格情報を表すリソースであり、データの安全な交換を可能にします。秘密鍵は、イベント転送プロパティ内でのみ作成できます。

現在、次のシークレットタイプがサポートされています。

| 秘密鍵タイプ | 説明 |
| --- | --- |
| [!UICONTROL Google OAuth 2] | [Google Ads API](https://developers.google.com/google-ads/api/docs/oauth/overview) および [Pub/Sub API](https://cloud.google.com/pubsub/docs/reference/service_apis_overview) で使用する [OAuth 2.0](https://datatracker.ietf.org/doc/html/rfc6749) 認証仕様をサポートするためのいくつかの属性が含まれています。システムの指示に従って必要な情報を入力すると、指定した間隔でトークンの更新が行われます。 |
| [!UICONTROL HTTP] | ユーザー名とパスワードの 2 つの文字列属性がそれぞれ含まれます。 |
| [!UICONTROL OAuth2] | [OAuth 2.0](https://datatracker.ietf.org/doc/html/rfc6749) 認証仕様の[クライアント資格情報付与タイプ](https://datatracker.ietf.org/doc/html/rfc6749#section-1.3.4)をサポートするためのいくつかの属性が含まれています。システムの指示に従って必要な情報を入力すると、指定した間隔でトークンの更新が行われます。 |
| [!UICONTROL OAuth 2 JWT] | 次の JSON Web トークン (JWT) プロファイルをサポートする属性が複数含まれます。 [OAuth 2.0 の認証](https://datatracker.ietf.org/doc/html/rfc7523#section-2.1) 交付金 システムの指示に従って必要な情報を入力すると、指定した間隔でトークンの更新が行われます。 |
| [!UICONTROL トークン] | 両方のシステムで認識および理解されている認証トークン値を表す単一の文字列。 |

{style="table-layout:auto"}

このガイドでは、Experience Platform UI またはデータ収集 UI でイベント転送（[!UICONTROL Edge]）プロパティのシークレットを設定する方法の概要を説明します。

>[!NOTE]
>
>秘密鍵の構造の JSON の例など、Reactor API で秘密鍵を管理する方法に関するガイダンスについて詳しくは、[秘密鍵 API ガイド](../../api/guides/secrets.md)を参照してください。

## 前提条件

このガイドは、UI でタグやイベント転送のリソースを管理する方法（データ要素やイベント転送ルールの作成方法など）を既に熟知していることを前提としています。概要については、[リソースの管理](../managing-resources/overview.md)に関するガイドを参照してください。

また、タグとイベント転送の公開フロー（ライブラリにリソースを追加する方法や、テスト用に web サイトにビルドをインストールする方法など）についての実用的な理解が必要です。詳しくは、[公開の概要](../publishing/overview.md)を参照してください。

## 秘密鍵の作成 {#create}

>[!CONTEXTUALHELP]
>id="platform_eventforwarding_secrets_environments"
>title="シークレットの環境"
>abstract="シークレットをイベント転送で使用できるようにするには、シークレットを既存の環境に割り当てる必要があります。イベント転送プロパティ用の環境が作成されていない場合は、先に進む前に環境を設定する必要があります。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=ja" text="環境の概要"

シークレットを作成するには、左側のナビゲーションで「**[!UICONTROL イベント転送]**」を選択し、シークレットを追加するイベント転送プロパティを開きます。 次に、左側のナビゲーションで「**[!UICONTROL シークレット]**」 を選択し、「**[!UICONTROL 新しいシークレットの作成]**」を選択します。

![新しいシークレットの作成](../../images/ui/event-forwarding/secrets/create-new-secret.png)

次の画面では、シークレットの詳細を設定できます。シークレットをイベント転送で使用できるようにするには、シークレットを既存の環境に割り当てる必要があります。イベント転送プロパティ用に環境が作成されていない場合は、続行する前に[環境](../publishing/environments.md)に関するガイドを参照して、環境の設定方法を確認してください。

>[!NOTE]
>
>秘密鍵を環境に追加する前に作成して保存する場合は、残りの情報を入力する前に、「**[!UICONTROL 秘密鍵を環境に添付]**」トグルを無効にします。シークレットを使用する場合は、後で環境に割り当てる必要があります。
>
>![環境の無効化](../../images/ui/event-forwarding/secrets/env-disabled.png)

「**[!UICONTROL ターゲット環境]**」で、ドロップダウンメニューを使用して、シークレットを割り当てる環境を選択します。「**[!UICONTROL シークレット名]**」では、環境のコンテキストでシークレットの名前を指定します。この名前は、イベント転送プロパティの下にあるすべての秘密鍵に対して一意である必要があります。

![環境と名前](../../images/ui/event-forwarding/secrets/env-and-name.png)

シークレットは、一度に 1 つの環境にのみ割り当てることができますが、必要に応じて、異なる環境の複数のシークレットに同じ資格情報を割り当てることができます。「**[!UICONTROL 環境の追加]**」を選択して、別の行をリストに追加します。

![環境の追加](../../images/ui/event-forwarding/secrets/add-env.png)

追加する環境ごとに、関連付けられたシークレットに別の一意の名前を指定する必要があります。使用可能なすべての環境を使い切ってしまうと、「**[!UICONTROL 環境の追加]**」ボタンは使用できなくなります。

![環境を追加できません](../../images/ui/event-forwarding/secrets/add-env-greyed.png)

ここからシークレットを作成する手順は、作成するシークレットのタイプによって異なります。詳しくは、以下のサブセクションを参照してください。

* [[!UICONTROL トークン]](#token)
* [[!UICONTROL HTTP]](#http)
* [[!UICONTROL OAuth2]](#oauth2)
* [[!UICONTROL OAuth 2 JWT]](#oauth2jwt)
* [[!UICONTROL Google OAuth 2]](#google-oauth2)

### [!UICONTROL トークン] {#token}

トークンシークレットを作成するには、「**[!UICONTROL タイプ]**」ドロップダウンから「**[!UICONTROL トークン]**」を選択します。表示される「**[!UICONTROL トークン]**」フィールドに、認証先のシステムで認識される資格情報文字列を入力します。「**[!UICONTROL シークレットの作成]**」を選択して、シークレットを保存します。

![トークンシークレット](../../images/ui/event-forwarding/secrets/token-secret.png)

### [!UICONTROL HTTP] {#http}

HTTP シークレットを作成するには、「**[!UICONTROL タイプ]**」ドロップダウンから「**[!UICONTROL シンプルな HTTP]**」を選択します。下に表示されるフィールドに、資格情報のユーザー名とパスワードを入力した後、「**[!UICONTROL シークレットの作成]**」を選択してシークレットを保存します。

>[!NOTE]
>
>資格情報は、保存時に、[基本「HTTP 認証スキーム」](https://www.rfc-editor.org/rfc/rfc7617.html)を使用してエンコードされます。

![HTTP シークレット](../../images/ui/event-forwarding/secrets/http-secret.png)

### [!UICONTROL OAuth2] {#oauth2}

OAuth2 シークレットを作成するには、「**[!UICONTROL タイプ]**」ドロップダウンから「**[!UICONTROL OAuth2]**」を選択します。 以下に表示されるフィールドに、[[!UICONTROL クライアント ID] と[!UICONTROL クライアントシークレット]](https://www.oauth.com/oauth2-servers/client-registration/client-id-secret/)のほか、OAuth 統合の[[!UICONTROL トークン URL]](https://www.oauth.com/oauth2-servers/access-tokens/client-credentials/) を入力します。UI の「[!UICONTROL トークン URL]」フィールドは、認証サーバーホストとトークンパスを連結したものです。

![OAuth2 シークレット](../../images/ui/event-forwarding/secrets/oauth-secret-1.png)

「**[!UICONTROL 資格情報オプション]**」で、`scope` や `audience` などの他の資格情報オプションをキーと値のペアの形式で提供できます。キーと値のペアを追加するには、「**[!UICONTROL さらに追加]**」を選択します。

![資格情報オプション](../../images/ui/event-forwarding/secrets/oauth-secret-2.png)

最後に、シークレットの&#x200B;**[!UICONTROL 更新オフセット]**&#x200B;値を設定できます。これは、トークンの有効期限が切れる前に、システムが自動更新を実行する秒数を表します。同等の時間（時間と分）がフィールドの右側に表示され、入力中に自動で更新されます。

![オフセットの更新](../../images/ui/event-forwarding/secrets/oauth-secret-3.png)

例えば、更新オフセットがデフォルト値の `14400`（4 時間）に設定されていて、アクセストークンの `expires_in` 値が `86400`（24 時間）の場合、システムは 20 時間で自動的に秘密鍵を更新します。

>[!IMPORTANT]
>
>OAuth 秘密鍵は、4 時間以上の更新間隔、8 時間以上の有効期限が必要です。この制限により、生成されたトークンで問題が発生した場合に介入するための時間が 4 時間以上確保されます。
>
>例えば、オフセットが `28800`（8 時間）に設定されていて、アクセストークンの `expires_in` が `36000`（10 時間）の場合、差分が 4 時間未満となるため、交換は失敗します。

終了したら、「**[!UICONTROL 秘密鍵の作秘]**」を選択し、秘密鍵を保存します。

![OAuth2 オフセットの保存](../../images/ui/event-forwarding/secrets/oauth-secret-4.png)

### [!UICONTROL OAuth 2 JWT] {#oauth2jwt}

OAuth 2 JWT シークレットを作成するには、「 」を選択します。 **[!UICONTROL OAuth 2 JWT]** から **[!UICONTROL タイプ]** ドロップダウン。

![The [!UICONTROL 秘密鍵を作成] 」タブに、 [!UICONTROL タイプ] ドロップダウン。](../../images/ui/event-forwarding/secrets/oauth-jwt-secret.png)

>[!NOTE]
>
>唯一の [!UICONTROL アルゴリズム] JWT への署名が現在サポートされている RS256。

以下に表示されるフィールドで、 [!UICONTROL 発行者], [!UICONTROL 件名], [!UICONTROL 対象ユーザ], [!UICONTROL カスタム要求], [!UICONTROL TTL]を選択し、 [!UICONTROL アルゴリズム] をドロップダウンから選択します。 次に、 [!UICONTROL 秘密鍵 ID]、 、および [[!UICONTROL トークン URL]](https://www.oauth.com/oauth2-servers/access-tokens/client-credentials/) OAuth 統合用。 The [!UICONTROL トークン URL] フィールドは必須フィールドではありません。 値を指定した場合、JWT はアクセストークンと交換されます。 この秘密は `expires_in` 属性を返します。 [!UICONTROL オフセットを更新] の値です。 値を指定しない場合、エッジにプッシュされる秘密鍵は JWT になります。 JWT は、 [!UICONTROL TTL] および [!UICONTROL オフセットを更新] 値。

![The [!UICONTROL 秘密鍵を作成] タブ内で選択した入力フィールドがハイライト表示されています。](../../images/ui/event-forwarding/secrets/oauth-jwt-information.png)

の下 **[!UICONTROL 秘密鍵証明書オプション]**&#x200B;を使用する場合、 `jwt_param` キーと値のペアの形式で書き込まれます。 キーと値のペアを追加するには、「**[!UICONTROL さらに追加]**」を選択します。

![The [!UICONTROL 秘密鍵を作成] タブハイライト [!UICONTROL 秘密鍵証明書オプション] フィールド。](../../images/ui/event-forwarding/secrets/oauth-jwt-credential-options.png)

最後に、シークレットの&#x200B;**[!UICONTROL 更新オフセット]**&#x200B;値を設定できます。これは、トークンの有効期限が切れる前に、システムが自動更新を実行する秒数を表します。同等の時間（時間と分）がフィールドの右側に表示され、入力中に自動で更新されます。

![The [!UICONTROL 秘密鍵を作成] タブハイライト [!UICONTROL オフセットを更新] フィールドに入力します。](../../images/ui/event-forwarding/secrets/oauth-jwt-refresh-offset.png)

例えば、更新オフセットが `1800` （30 分）に設定され、アクセストークンに `expires_in` の値 `3600` （1 時間）の場合、1 時間でシークレットが自動的に更新されます。

>[!IMPORTANT]
>
>OAuth 2 JWT シークレットは、更新の間隔を 30 分以上にする必要があり、1 時間以上有効である必要があります。 この制限により、生成されたトークンに問題が発生した場合に介入するまでに最低 30 分かかります。
>
>例えば、オフセットが `1800` （30 分）に設定され、アクセストークンに `expires_in` / `2700` （45 分）、結果として 30 分未満になる差異が原因で、交換が失敗する場合があります。

終了したら、「**[!UICONTROL 秘密鍵の作秘]**」を選択し、秘密鍵を保存します。

![The [!UICONTROL 秘密鍵を作成] タブのハイライト [!UICONTROL 秘密鍵を作成]](../../images/ui/event-forwarding/secrets/oauth-jwt-create-secret.png)

### [!UICONTROL Google OAuth2] {#google-oauth2}

Google OAuth2 シークレットを作成するには、**[!UICONTROL タイプ]**&#x200B;ドロップダウンから「**[!UICONTROL Google OAuth2]**」を選択します。「**[!UICONTROL スコープ]**」で、この秘密鍵を使用してアクセスを許可する Google API を選択します。現在、次の製品がサポートされています。

* [Google 広告 API](https://developers.google.com/google-ads/api/docs/oauth/overview)
* [Pub/Sub API](https://cloud.google.com/pubsub/docs/reference/service_apis_overview)

完了したら、「**[!UICONTROL 秘密鍵を作成]**」を選択します。

![Google OAuth2 秘密鍵](../../images/ui/event-forwarding/secrets/google-oauth.png)

秘密鍵を Google で手動で認証する必要があることを示すポップオーバーが表示されます。「**[!UICONTROL 作成して認証]**」を選択して続行します。

![Google 認証ポップオーバー](../../images/ui/event-forwarding/secrets/google-authorization.png)

Google アカウントの資格情報を入力するためのダイアログが表示されます。画面の指示に従い、選択したスコープでイベント転送にデータへのアクセスを許可します。認証プロセスが完了すると、シークレットが作成されます。

>[!IMPORTANT]
>
>組織が Google Cloud アプリケーション用に再認証ポリシーを設定している場合、認証の有効期限が切れた後（ポリシー設定に応じて 1～24 時間）、作成されたシークレットは正常に更新されません。
>
>この問題を解決するには、Google 管理コンソールにログインし、イベント転送アプリ（Adobe Real-Time CDP イベント転送）を [!DNL Trusted] としてマークできるように **[!DNL App access control]** ページに移動します。詳しくは、Google ドキュメントで [Google Cloud サービスのセッション継続時間を設定する](https://support.google.com/a/answer/9368756)を参照してください。

## 秘密鍵の編集

プロパティの秘密鍵を作成したら、**[!UICONTROL 秘密鍵]**&#x200B;ワークスペースに一覧表示されます。既存の秘密鍵の詳細を編集するには、リストから名前を選択します。

![編集する秘密鍵を選択](../../images/ui/event-forwarding/secrets/edit-secret.png)

次の画面では、秘密鍵の名前と資格情報を変更できます。

![秘密鍵の編集](../../images/ui/event-forwarding/secrets/edit-secret-config.png)

>[!NOTE]
>
>秘密鍵が既存の環境に関連付けられている場合、その秘密鍵を別の環境に再割り当てすることはできません。異なる環境で同じ資格情報を使用する場合は、代わりに[新しい秘密鍵を作成する](#create)必要があります。この画面から環境を再割り当てできるのは、以前に秘密鍵を環境に割り当てたことがない場合、または秘密鍵が添付されていた環境を削除した場合のみです。

### 秘密鍵の交換の再試行

編集画面から秘密鍵の交換を再試行または更新できます。このプロセスは、編集する秘密鍵のタイプによって異なります。

| 秘密鍵タイプ | 再試行プロトコル |
| --- | --- |
| [!UICONTROL トークン] | 「**[!UICONTROL 秘密鍵の交換]**」を選択し、秘密鍵の交換を再試行します。このコントロールは、秘密鍵に接続された環境がある場合にのみ使用できます。 |
| [!UICONTROL HTTP] | 秘密鍵に接続された環境がない場合は、「**[!UICONTROL 秘密鍵の交換]**」を選択し、資格情報を base64 に交換します。環境が接続されている場合は、「**[!UICONTROL シークレットの交換とデプロイ]**」を選択して、シークレットを base64 に交換しデプロイします。 |
| [!UICONTROL OAuth2] | 「**[!UICONTROL トークンの生成]**」を選択して資格情報を交換し、認証プロバイダーからアクセストークンを返します。 |

## 秘密鍵の削除

**[!UICONTROL 秘密鍵]**&#x200B;ワークスペースで既存の秘密鍵を削除するには、名前の横にあるチェックボックスを選択してから、「**[!UICONTROL 削除]**」を選択します。

![秘密鍵の削除](../../images/ui/event-forwarding/secrets/delete.png)

## イベント転送での秘密鍵の使用

イベント転送で秘密鍵を使用するには、まず秘密鍵自体を参照する[データ要素](../managing-resources/data-elements.md)を作成する必要があります。データ要素を保存した後、それをイベント転送[ルール](../managing-resources/rules.md)に含め、それらのルールを[ライブラリ](../publishing/libraries.md)に追加できます。ライブラリは、[ビルド](../publishing/builds.md)としてアドビのサーバーにデプロイできます。

データ要素を作成するときは、**[!UICONTROL Core]** 拡張機能を選択してから、データ要素タイプとして「**[!UICONTROL 秘密鍵]**」を選択します。右側のパネルが更新され、データ要素に最大 3 つの秘密鍵を割り当てるためのドロップダウンコントロールが提供されます。それぞれ[!UICONTROL 開発用]、[!UICONTROL ステージング用]および[!UICONTROL 実稼動用]です。

![データ要素](../../images/ui/event-forwarding/secrets/data-element.png)

>[!NOTE]
>
>開発環境、ステージング環境、実稼動環境に関連付けられた秘密鍵のみが、それぞれのドロップダウンに表示されます。

複数の秘密鍵を 1 つのデータ要素に割り当ててルールに含めることで、含まれるライブラリが[公開フロー](../publishing/publishing-flow.md)のどこにあるかに応じて、データ要素の値を変更できます。

![複数の秘密鍵を持つ秘密鍵データ要素](../../images/ui/event-forwarding/secrets/multi-secret-data-element.png)

>[!NOTE]
>
>データ要素を作成する場合は、開発環境を割り当てる必要があります。ステージング環境と実稼動環境の秘密鍵は必須ではありませんが、これらの環境に移行しようとするビルドは、その秘密鍵タイプのデータ要素秘密鍵する環境用に選択された秘密鍵がない場合、失敗します。

## 次の手順

このガイドでは、UI でのシークレットの管理方法について説明しました。Reactor API を使用してシークレットを操作する方法については、[シークレットエンドポイントガイド](../../api/endpoints/secrets.md)を参照してください。
