---
title: Experience Platform ダッシュボード用のPower BI レポートテンプレート
description: Power BI を使用した Experience Platform データを、レポートテンプレートを使用して調べます。
exl-id: fb98a79f-3d82-4e11-b08a-b7cb06414462
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1482'
ht-degree: 75%

---

# ダッシュボードの Power BI レポートテンプレート

Power BI レポートテンプレート機能を使用すると、Adobe Experience Platform のデータが入った魅力的なレポートを作成できます。 合理化されたインストールプロセスにより、リアルタイム顧客プロファイル、セグメント化および宛先の標準ウィジェットが自動的にインストールされます。 また、インストールの際には Power BI をデータモデルに接続するので、レポートテンプレートを簡単にカスタマイズして拡張できます。これらのレポートは、Experience Platform上の組織の認証情報を必要とする受信者なしで、組織全体で共有できます。

このドキュメントでは、Adobe Experience PlatformをPower BI アプリケーションと接続し、レポートテンプレートを使用して主要なExperience Platform データインサイトを外部ユーザーと共有する方法について説明します。

## はじめに

このチュートリアルを続行する前に、Experience Platformの [ スキーマ構成 ](../../xdm/schema/composition.md)、および [ 和集合スキーマ ](../../xdm/schema/composition.md#union) を通じてリアルタイム顧客プロファイルに属性が含まれる方法を確認することをお勧めします。

Power BI アプリケーション統合をインストールするには、ユーザーはまず次のExperience Platform権限を取得している必要があります。

- クエリの管理
- サンドボックスの管理

これらの権限の割り当て方法については、[アクセス制御](../../access-control/home.md)ドキュメントを参照してください。

また、このチュートリアルに従うには、Power BI アカウントが必要です。アカウントを作成するには、[Power BI ホームページ](https://powerbi.microsoft.com/ja-jp/)に移動し、登録プロセスに従います。この Power BI アカウントのユーザーも、Power BI 設定内の「**ワークスペースを作成**」設定を有効にします。この設定は、Power BI 管理ポータルのテナント設定内にあります。アカウントがテナントまたは雇用主によって提供されている場合は、担当の管理者に連絡してこの設定を有効にしてもらってください。

![Power BI 管理ポータルワークスペース設定の作成。](../images/power-bi/create-workspace-settings.png)

>[!NOTE]
>
>Experience Platform UI の左側のナビゲーションに「ダッシュボード」タブを表示し、ダッシュボードインベントリビューを確認できるようにするには、Experience Platform ライセンスの一部として、プロファイル、セグメント化、宛先のいずれかのダッシュボードにアクセスできる必要があります。

## Power BI アプリケーション統合のインストール

Experience Platform UI 内で、左側のナビゲーションの **[!UICONTROL ダッシュボード]** を選択して、[!UICONTROL &#x200B; ダッシュボード &#x200B;] ワークスペースを開きます。 この「[!UICONTROL 参照]」タブには、現在使用可能なダッシュボードビューのリストが表示されます。使用可能なダッシュボードの表示について詳しくは、[インベントリドキュメント](../inventory.md)を参照してください。

次に、「**[!UICONTROL 統合]**」タブをクリックします。Power BI アプリケーション統合ページが表示されます。 ここから&#x200B;**[!UICONTROL インストール]**&#x200B;をクリックして、インストールを開始します。

>[!NOTE]
>
>この「[!UICONTROL インストール]」ボタンは、クエリサービスの管理とサンドボックスの管理権限の両方を持っていない限り、無効になります。

![Power BI の詳細画面の「インストール」ボタンがハイライト表示されています。](../images/power-bi/details-screen.png)

### 資格情報を入力

インストールプロセスの最初の手順は、Power BI アプリケーション統合に有効期限のない資格情報を入力することです。これらを入力するオプションは 2 つあります。[[!UICONTROL 新しい資格情報を作成]](#create-new-credentials)または[[!UICONTROL 既存の資格情報を使用]](#use-existing-credentials)です。適切な切り替えを選択して続行します。

#### 新しい資格情報を作成 {#create-new-credentials}

新しい資格情報を生成する際には、次の 2 つの必須フィールドがあります。[!UICONTROL 名前]および[!UICONTROL 割り当て先]です。この[!UICONTROL 割り当て先]フィールドは、Power BI に関連付けられた電子メールアドレスに関連しています。

![Power BI が生成する新しい資格情報画面。](../images/power-bi/generate-new-credentials.png)

>[!IMPORTANT]
>
>有効期限のない資格情報を作成するには、自身に特定の権限と役割が割り当てられている必要があります。 必要な権限は、サンドボックスの管理とクエリサービス統合の管理です。 Adobe Experience Platform 管理者と開発者の役割が必要です。これらの権限の割り当て方法については、[アクセス制御](../../access-control/home.md)ドキュメントをお読みください。

有効期限のないクエリサービス資格情報の生成について詳しくは、[有効期限のない資格情報ガイド](../../query-service/ui/credentials.md#non-expiring-credentials)を参照してください。

有効期限のない資格情報が初めて生成された後、JSON ファイルがそのマシンにダウンロードされます。 その後、この JSON ファイルを資格情報として他のユーザーと共有して、インストールプロセスを完了できます。

#### 既存の資格情報を使用 {#use-existing-credentials}

JSON 資格情報ファイルをアップロードして検証にパスすることもできます。有効期限のない資格情報の値を含むこれらの JSON ファイルは、有効期限のない資格情報の作成時に使用されるローカルマシンにダウンロードされます。

>[!IMPORTANT]
>
>有効期限のない既存の資格情報を使用するには、ユーザーに既に資格情報が割り当てられている必要があります。 ユーザーに資格情報が割り当てられておらず、Adobe Admin Console を使用して新しい資格情報を作成できない場合、ユーザーはインストールプロセスを続行できません。

**[!UICONTROL 資格情報ファイルのアップロード]**&#x200B;を選択し、表示されるダイアログでアップロードする適切な JSON ファイルを選択します。

![「資格情報ファイルをアップロード」ボタンをハイライト表示した Power BI 資格情報画面。](../images/power-bi/upload-credential-file.png)

有効期限のない認証情報を指定すると、Experience Platformによって自動検証されます。 検証が成功すると、確認メッセージが表示されます。 「**[!UICONTROL 次へ]**」を選択し、Power BI アプリケーションの同意契約を確認します。

![有効期限のない資格情報は、「次へ」ボタンがハイライト表示された画面で正常に検証されました。](../images/power-bi/successfully-uploaded-credential-file.png)

### 同意の入力

同意画面が表示されます。「**[!UICONTROL 同意を確認]**」をクリックすると、利用規約およびプライバシーに関する声明に従って、Power BI がデータにアクセスして使用するために必要な権限の詳細を説明する新しいウィンドウが開きます。

![「同意の確認」ボタンがハイライト表示された状態で、同意の入力画面が表示されます。](../images/power-bi/provide-consent-display.png)

「**[!UICONTROL 同意する]**」を選択して、Power BI データへのアクセス権を付与し、Experience Platform データを使用します。

![Power BI アプリケーションの権限リクエスト。](../images/power-bi/permissions.png)

>[!NOTE]
>
>同意を得る前にインストールプロセスを終了した場合、Power BI アプリケーション統合はダッシュボードインベントリにインストールされません。

同意を得た後、レポートテンプレートはインストールプロセスの一環として、Power BI 環境に自動的にインストールされます。次に、Power BIは、有効期限のない資格情報を使用してExperience Platformにアクセスし、連続してすべての SQL クエリを実行し、レポートテンプレートに返されたデータを入力します。

「**[!UICONTROL 完了]**」をクリックして、ダッシュボードインベントリに戻ります。

![「完了」ボタンがハイライト表示された状態で、同意を入力する画面が表示されます。](../images/power-bi/finish-consent-review.png)

これで Power BI レポートテンプレートがインストールされ、「[!UICONTROL 参照]」タブの下の使用可能なダッシュボードのリストに表示されます。 **[!UICONTROL Power BI]** をリストから選択し、Power BI 環境に移動します。

![Power BI の一覧がダッシュボードインベントリに表示されます。](../images/power-bi/power-bi-dashboard-inventory.png)

>[!IMPORTANT]
>
>Power BI 管理者は、ユーザーが Power BI 環境でこれらのダッシュボードを表示するための適切なアクセス権を持っていることを確認する必要があります。

## Power BI ワークスペース

[Power BI ワークスペース](https://dxt.powerbi.com)にログインした後、自分がアクセス権を持っている各サービスでレポートテンプレートを利用できます。対応する表示権限を持っている場合に&#x200B;**のみ**、レポートテンプレートにはプロファイル、セグメント、宛先ダッシュボードが含まれます。

プロファイル、セグメントおよび宛先の標準ウィジェットは、デフォルトで Power BI テンプレートレポート内で使用できます。

>[!NOTE]
>
>ダッシュボードを Power BI 環境にインストールするには、特定のダッシュボードに対する編集権限を有効にしておく必要があります。

![ 標準のPower BI プロファイルウィジェットを使用したExperience Platform プロファイルテンプレートレポート。](../images/power-bi/profile-report-template.png)

ダッシュボードを Power BI にインストールすると、デフォルトではレポートテンプレートがすべてのユーザーに表示されます。任意のレポートテンプレートへのアクセスを制限する場合は、該当するユーザーのアクセスを Power BI 環境内で無効にしてください。

## Power BI レポートテンプレートのカスタマイズ

カスタムウィジェットを使用すると、カスタム属性をデータモデルに追加して、Power BI が提供するレポートテンプレートを強化できます。

>[!NOTE]
>
>カスタムウィジェットに使用できる属性は、和集合スキーマで使用できる属性によって異なります。 カスタムウィジェットの利点を活かすための和集合スキーマの表示および参照方法については、[和集合スキーマ UI ガイド](../../profile/ui/union-schema.md)を参照してください。

### カスタムウィジェットの作成

カスタムウィジェットはウィジェットライブラリを通じて作成されます。 機能の紹介については[ウィジェットライブラリの概要](../customize/widget-library.md)を参照してください。また、特定の手順については[カスタムウィジェットを作成するためのチュートリアル](../customize/custom-widgets.md)を参照してください。

>[!IMPORTANT]
>
>新しく作成されたカスタムウィジェットは、Adobe Experience Platform ダッシュボードと Power BI レポートテンプレート間で自動的に同期&#x200B;**されません**。Experience Platform UI で作成したカスタムウィジェットは、Power BI環境内で手動で再作成する必要があります。

### カスタムウィジェットを Power BI 環境で再作成

ダッシュボードにカスタムウィジェット内の適切な指標と属性が含まれたら、Power BI 環境内で表示されるレポートテンプレートを変更する準備が整います。 ユーザーインターフェイスを通してレポートを編集する情報については、[Power BI ドキュメント](https://docs.microsoft.com/ja-JP/power-bi/)を参照してください。

## Power BI アプリケーション統合の削除

ダッシュボードを削除するには、ダッシュボードインベントリに移動し、ダッシュボード名の横にある削除アイコン（![ 削除アイコン ](/help/images/icons/delete.png)）を選択します。

>[!NOTE]
>
>Experience Platform UI から統合を削除できるのは、Power BI ダッシュボードをインストールしたユーザーのみです。

![「参照」ボタンと削除アイコンがハイライトされた状態で表示されるダッシュボードインベントリ画面の「参照」タブ。](../images/power-bi/delete-power-bi-dashboard.png)

確認ポップオーバーが表示されます。「**[!UICONTROL 削除]**」をクリックしてプロセスを確定します。

>[!IMPORTANT]
>
>Experience Platform UI からPower BI ダッシュボードを削除することによって、お使いのPower BI環境で使用可能なレポートテンプレートは **削除されません**。 Power BI レポートテンプレートに含まれる情報を完全に削除する場合は、Power BI アカウントにログインして、その環境からレポートテンプレートを削除する必要があります。削除したら、ユーザーは前述と同じインストール手順に従って、Power BI ダッシュボードを再インストールできます。

## 次の手順

このドキュメントでは、Power BIのレポートテンプレートをExperience Platformに統合して、プロファイル、セグメント、宛先ダッシュボードから魅力的なデータインサイトを共有する方法について、より深く理解します。 詳しくは、[ダッシュボードのカスタマイズの概要](../customize/overview.md)を参照し、ダッシュボードのカスタマイズを確認してください。
