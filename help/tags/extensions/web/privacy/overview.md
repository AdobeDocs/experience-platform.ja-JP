---
title: Adobe Privacy 拡張機能の概要
description: Adobe Experience Platform の Adobe Privacy タグ拡張機能について説明します。
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 96%

---

# Adobe Privacy 拡張機能の概要

>[!NOTE]
>
>Adobe Experience Platform Launchは、Adobe Experience Platformのデータ収集テクノロジーのスイートとしてリブランドされました。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。用語の変更点の一覧については、次の[ドキュメント](../../../term-updates.md)を参照してください。

Adobe Privacy 拡張機能は、アドビソリューションによってエンドユーザーに割り当てられているユーザー ID を収集および削除する機能を提供します。

## インストール時のソリューションの設定

拡張機能カタログから Adobe Privacy 拡張機能をインストールすると、更新するソリューションを選択するよう求められます。現在、以下のソリューションを更新できます。

* Analytics（AA）
* Audience Manager（AAM）
* Target
* 訪問者サービス
* AdCloud
* 1 つまたは複数のソリューションを選択し、「更新」を選択します。
* ソリューションを選択して設定したら、「保存」をクリックします。Adobe Privacy 拡張機能 が、インストール済み拡張機能に追加されます。

   各ソリューションのオプションは以下のとおりです。

### Analytics

![](../../../images/ext-privacy-aa.jpg)

デフォルトでは、文字列を入力またはデータ要素を選択して、レポートスイートを提供する必要があります。

他の項目を設定するには、「**[!UICONTROL 項目を選択]**」を選択し、設定する項目を選択してから、「**[!UICONTROL 追加]**」を選択してリクエストされたパラメーターまたはデータ要素を入力します。

### Audience Manager

![](../../../images/ext-privacy-aam.jpg)

「**[!UICONTROL 項目を選択]**」、設定する項目、「**[!UICONTROL 追加]**」の順に選択してリクエストされたパラメーターまたはデータ要素を入力します。現時点では、`aamUUIDCookieName` のみを設定できます。

### Target

![](../../../images/ext-privacy-target.jpg)

Target クライアントコードを入力します。

### 訪問者サービス

![](../../../images/ext-privacy-visitor.jpg)

IMS 組織 ID を入力します。

### AdCloud

![](../../../images/ext-privacy-adcloud.jpg)

AdCloud 用に設定が必要な特定のパラメーターはありません。

## Adobe Privacy 拡張機能の設定

拡張機能をインストールした後、その拡張機能を無効化または削除することができます。インストールした拡張機能の Adobe Privacy カードで「**[!UICONTROL 設定]**」を選択し、「**[!UICONTROL 無効化]**」または「**[!UICONTROL アンインストール]**」を選択します。

## アクション

Adobe Privacy 拡張機能を使用してルールを設定する際には、次のアクションを使用できます。

### Retrieve identities

イベントと条件が満たされたら、その訪問者用に保存されている ID 情報を取得します。

データを渡す JavaScript 関数の名前を入力します。この関数またはメソッドは、取得した ID を処理します。それらを保存、表示、または Adobe GDPR API に送信するかどうかは、お客様がコントロールできます。

### Remove identities

イベントと条件が満たされたら、その訪問者用に保存されている ID 情報を削除します。

データを渡す JavaScript 関数の名前を入力します。この関数またはメソッドは、取得した ID を処理します。それらを保存、表示、または Adobe GDPR API に送信するかどうかは、お客様がコントロールできます。

### Retrieve then remove identies

イベントと条件が満たされたら、その訪問者用に保存されている ID 情報を取得してから削除します。

## チュートリアル：プライバシー拡張機能の設定

次に、データ要素を設定し、プライバシー拡張機能と共に使用する方法のスタブ化された例を示します。

1. `privacyFunc` という名前のデータ要素を作成します。

   ```JavaScript
   window.privacyFunc = function(a,b){
       console.log(a,b);
   }
   return window.privacyFunc
   ```

1. Adobe Privacy 拡張機能のアクションを指定して、ライブラリの読み込み（ページの上部）時に実行するルールを作成します。データ要素に `privacyFunc` を選択します。

   * **拡張機能**：アドビプライバシー
   * **アクションタイプ**：ID の取得
このアクションタイプは、作成、削除または削除されていない ID を表示します。
   * **名前**：ID の取得

1. 開発ライブラリを更新してから、公開およびテストします。
