---
title: Adobe Audience Manager 拡張機能の概要
description: Adobe Experience Platform の Adobe Audience Manager タグ拡張機能について説明します。
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: ht
source-wordcount: '496'
ht-degree: 100%

---

# Adobe Audience Manager 拡張機能の概要

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。用語の変更点の一覧については、次の[ドキュメント](../../../term-updates.md)を参照してください。

Audience Manager タグ拡張機能を使用すれば、Audience Manager が使用する DIL コードを Adobe Experience Platform のプロパティと統合できます。

このリファレンスは、この拡張機能を使用してルールを作成するときに使用できるオプションに関する情報です。

>[!NOTE]
>
>この拡張機能は、Adobe Analytics データのイベント転送には使用されません。イベント転送の場合は、[Adobe Analytics 拡張機能](../analytics/overview.md)を使用します。

## Adobe Audience Manager 拡張機能を設定する

Adobe Audience Manager 拡張機能がまだインストールされていない場合は、プロパティを開いて、**[!UICONTROL エクステンション／カタログ]**&#x200B;を選択し、Adobe Audience Manager 拡張機能にカーソルを置いて「**[!UICONTROL インストール]**」を選択します。

拡張機能を設定するには、「[!UICONTROL エクステンション]」タブを開き、拡張機能にカーソルを置いて「**[!UICONTROL 設定]**」を選択します。

### DIL 設定

DIL 設定をおこないます。次の設定オプションを使用できます。

![](../../../images/ext-aam-config.png)

#### DIL Version

Data Integration Library（DIL）のバージョンを表示します。

この設定は変更できません。

#### Exclude Specific Paths

除外されたパスのいずれかと URL 一致する場合、拡張機能は読み込まれません。

「 **[!UICONTROL パスを追加]**」を選択して、除外された URL を指定します。

URL が正規表現の場合は、正規表現を有効にします。

#### Use DIL Site Catalyst Module

[SiteCatalyst モジュール](https://experiencecloud.adobe.com/resources/help/ja_JP/aam/r_dil_sc_init.html)は、DIL と連携して Analytic タグ要素を Audience Manager に送信します。

コードエディターを使用して siteCatalyst.init ファイルを設定します。

メモを作成し、この設定に関する情報を含めることもできます。

#### Use DIL Google Analytics Module

[Google Analytics モジュール](https://experiencecloud.adobe.com/resources/help/ja_JP/aam/dil-google-universal-analytics.html)を有効にします。

#### DIL.create Initialization Properties

[DIL.create](https://experiencecloud.adobe.com/resources/help/ja_JP/aam/r_dil_create.html) が使用している初期化プロパティと、[visitorService object](https://experiencecloud.adobe.com/resources/help/ja_JP/aam/r_dil_visitor_service.html) の名前空間サブプロパティを追加します。コードエディターでは、コードコメントに 2 つの使用例が含まれています。

「**[!UICONTROL 項目を選択]**」を選択して、プロパティを追加します。

「i」アイコンの上にマウスポインターを置くと、各プロパティの動作がわかります。プロパティの詳細については、[Audience Manager DIL ドキュメント](https://experiencecloud.adobe.com/resources/help/ja_JP/aam/r_dil_create.html)を参照してください。

拡張機能の設定が完了したら、「**[!UICONTROL 保存]**」を選択します。

## Adobe Audience Manager 拡張機能のアクションタイプ

このトピックでは、Audience Manager 拡張機能で使用できるアクションタイプについて説明します。

Adobe Audience Manager 拡張機能は、ルールの「Then」部分に次のアクションを提供します。

### カスタムコードの実行

コードエディターで設定したカスタムコードを実行します。

コードエディターに目的のコードを入力してから、コードの名前を指定します。このコードは、ルールビルダーの「Then」部分で利用できるようになります。

![](../../../images/ext-aam-then.png)

また、メモを作成し、この設定に関する情報を含めることもできます。
