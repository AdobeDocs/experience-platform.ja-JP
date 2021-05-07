---
keywords: Experience Platform；ホーム；人気の高いトピック
solution: Experience Platform
title: Adobe Experience Platformでの同意処理
topic-legacy: getting started
description: Adobe2.0標準を使用して、Adobe Experience Platformで顧客の同意シグナルを処理する方法を学びます。
exl-id: cd76a3f6-ae55-4d75-9b30-900fadb4664f
translation-type: tm+mt
source-git-commit: ab0798851e5f2b174d9f4241ad64ac8afa20a938
workflow-type: tm+mt
source-wordcount: '1570'
ht-degree: 0%

---

# Adobe Experience Platformでの同意処理

Adobe Experience Platformでは、顧客から収集した同意データを処理し、保存されている顧客プロファイルに統合できます。 その後、このデータをダウンストリームプロセスで使用して、特定の顧客に対してデータ収集を行うか、そのプロファイルを特定の目的に使用するかを判断できます。 例えば、特定のプロファイルの同意データによって、その署名をエクスポートしたオーディエンスセグメントに含めることができるか、電子メール、テキストメッセージ、プッシュ通知などの特定のマーケティングチャネルに参加できるかを指定できます。

このドキュメントでは、同意管理プラットフォーム(CMP)によって生成された顧客の同意データを取り込み、そのデータを顧客プロファイルに統合して、下流の用途に使用するためのプラットフォームデータ操作の構成方法について概要を説明します。

>[!NOTE]
>
>このドキュメントは、Adobe標準を使用した同意データの処理に重点を置いています。 IAB Transparency and Consent Framework (TCF) 2.0に準拠して同意データを処理する場合は、[リアルタイム顧客データプラットフォーム](../iab/overview.md)でのTCF 2.0のサポートに関するガイドを参照してください。

## 前提条件

このガイドでは、同意データの処理に関連する様々なExperience Platformサービスについて、十分な理解を得る必要があります。

* [Experience Data Model(XDM)](../../../../xdm/home.md):Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
* [Adobe Experience Platform・アイデンティティ・サービス](../../../../identity-service/home.md):デバイスやシステム間でIDをブリッジ化することによって顧客体験データを断片化することによって生じる基本的な課題を解決します。
* [リアルタイム顧客プロファイル](../../../../profile/home.md):機能を使用して、データセットから詳細な顧客プロファイルをリアルタイムで作成します。 [!DNL Identity Service] リアルタイム顧客プロファイルは、Data Lakeからデータを取り込み、顧客プロファイルを独立したData Storeに維持します。
* [Adobe Experience PlatformウェブSDK](../../../../edge/home.md):様々なプラットフォームサービスを顧客と直接やり取りするWebサイトに統合できる、クライアント側のJavaScriptライブラリ。
   * [SDKの同意コマンド](../../../../edge/consent/supporting-consent.md):このガイドに示す同意関連のSDKコマンドの使用例の概要です。
* [Adobe Experience Platform分類サービス](../../../../segmentation/home.md):リアルタイム顧客プロファイルデータを、同じ特性を共有し、マーケティング戦略と同様に対応する個人のグループに分割できます。

## 同意処理フローの概要{#summary}

システムが正しく設定された後に同意データを処理する方法を次に示します。

1. 顧客は、Webサイト上のダイアログを通じて、データ収集に関する同意の環境設定を行います。
1. 各ページ読み込み時（またはCMPが同意の環境設定の変更を検出した場合）、サイト上のカスタムスクリプトは現在の環境設定を標準のXDMオブジェクトにマップします。 次に、このオブジェクトをPlatform Web SDK `setConsent`コマンドに渡します。
1. `setConsent`を呼び出すと、プラットフォームWeb SDKは、同意値が最後に受け取った値と異なるかどうかを確認します。 値が異なる（または以前の値がない）場合は、構造化された同意/環境設定データがAdobe Experience Platformに送信されます。
1. 同意/優先度データは、[!DNL Profile]対応のデータセットに取り込まれ、そのスキーマに同意/優先度フィールドが含まれます。

CMPの同意変更フックによってトリガーされるSDKコマンドに加えて、[!DNL Profile]対応のデータセットに直接アップロードされる、お客様が生成するXDMデータを介してExperience Platformに同意データを送ることもできます。

### 同意の実施

プラットフォームの同意処理サポートの現在のリリースでは、データ収集権限(`collect.val`)のみがプラットフォームWeb SDKによって自動的に適用されます。 顧客のプロファイル内でより詳細な同意と環境設定を収集して持続させることはできますが、これらの追加シグナルは、独自のダウンストリームプロセス内で手動で適用する必要があります。

>[!NOTE]
>
>上述のXDM同意フィールドの構造について詳しくは、[Consents &amp; Preferences data type](../../../../xdm/data-types/consents.md)のガイドを参照してください。

システムが設定されると、Platform Web SDKは、現在のユーザーのデータ収集の同意値を解釈して、データをAdobe Experience Platformエッジネットワークに送信するか、クライアントから削除するか、データ収集権限が「はい」または「いいえ」に設定されるまで保持するかを決定します。

## CMP {#consent-data}内で顧客の同意データを生成する方法を決定する

各CMPシステムは独自のものなので、顧客がサービスとやり取りする際に、顧客が同意を与える最善の方法を決定する必要があります。 これを行う一般的な方法は、次の例のように、cookieの同意ダイアログを使用することです。

![](../../../images/governance-privacy-security/consent/adobe/overview/consent-dialog.png)

このダイアログでは、顧客がデータの特定のマーケティングおオプトインよびパーソナライゼーションの使用例を、またはその使用例から除外できるようにする必要があります。 これらのコンセントと環境設定は、次の手順で[!DNL Profile]対応データセット用に定義するデータモデルに準拠する必要があります。

## 追加[!DNL Profile]対応データセット{#dataset}に対する同意フィールドを標準化

顧客の同意データは、スキーマに同意フィールドが含まれている[!DNL Profile]対応のデータセットに送信する必要があります。 これらのフィールドは、個々の顧客に関する属性情報を取り込むのに使用するのと同じスキーマーおよびデータセットに含める必要があります。

このガイドを続ける前に、[!DNL Profile]が有効なデータセットにこれらの必須フィールドを追加する方法の詳細については、[同意データを取り込むデータセットの設定](./dataset.md)のチュートリアルを参照してください。

## [!DNL Profile]結合ポリシーを更新して、同意データを含める{#merge-policies}

同意データを処理するための[!DNL Profile]対応のデータセットを作成したら、結合ポリシーが各顧客プロファイルに常に同意フィールドを含むように設定されていることを確認する必要があります。 これには、競合する可能性のある他のデータセットよりも同意データセットの優先順位を優先するように、データセットの優先順位を設定する必要があります。

>[!NOTE]
>
>競合するデータセットがない場合は、マージポリシーのタイムスタンプの優先順位を設定する必要があります。 これにより、顧客が指定した最新の同意が、使用される同意設定であることを確認できます。

結合ポリシーの使用方法の詳細については、[merge policies user guide](../../../../profile/ui/merge-policies.md)を参照してください。 マージポリシーを設定する場合は、[データセットの準備](./dataset.md)のガイドで説明されているように、プロファイルに、「同意と環境設定」スキーマフィールドグループが提供する必要な同意属性がすべて含まれていることを確認する必要があります。

## 同意データをプラットフォームに取り込む

データセットとマージポリシーを取得し、お客様のプロファイルで必要な同意フィールドを表すようにしたら、次の手順は、同意データ自体をプラットフォームに取り込むことです。

主に、CMPで同意変更イベントが検出された場合は、プラットフォームに同意データを送信するために、Adobe Experience PlatformWeb SDKを使用する必要があります。 モバイルプラットフォームで同意データを収集する場合は、Adobe Experience PlatformモバイルSDKを使用する必要があります。 また、収集した同意データを同意データセットのXDMスキーマにマッピングし、バッチ取り込みを使用してPlatformに送信することで、収集した同意データを直接取り込むこともできます。

各メソッドの詳細については、以下のサブセクションを参照してください。

### 同意データを処理するようにExperience PlatformWeb SDKを設定{#web-sdk}

Webサイトの同意変更イベントをリッスンするようにCMPを設定したら、Experience PlatformWeb SDKを統合して、更新された同意設定を受け取り、各ページの読み込み時および同意変更イベントが発生した場合にプラットフォームに送信できます。 詳しくは、[顧客の同意データを処理するようにWeb SDKを設定する](./sdk.md)のガイドを参照してください。

### 同意データを処理するようにExperience PlatformモバイルSDKを設定{#mobile-sdk}

モバイルアプリケーションで顧客の同意が必要な場合、Experience PlatformMobile SDKを統合して、同意設定を取得および更新し、同意APIが呼び出されるたびにPlatformに送信できます。

[同意API](https://aep-sdks.gitbook.io/docs/v/AEP-Edge-Docs/using-mobile-extensions/adobe-edge-consent/edge-consent-api-reference)を使用した](https://aep-sdks.gitbook.io/docs/v/AEP-Edge-Docs/using-mobile-extensions/adobe-edge-consent)および[同意モバイル拡張の設定については、モバイルSDKのドキュメントを参照してください。 モバイルSDKを使用してプライバシーに関する問題を解決する方法について詳しくは、[プライバシーとGDPR](https://aep-sdks.gitbook.io/docs/v/AEP-Edge-Docs/resources/privacy-and-gdpr)を参照してください。

### XDM準拠の同意データを直接取り込む{#batch}

バッチインジェストを使用して、CSVファイルからXDM準拠の同意データを取り込むことができます。 これは、顧客プロファイルにまだ統合されていない、以前に収集された同意データがバックログされている場合に役立ちます。

[CSVファイルのXDM](../../../../ingestion/tutorials/map-a-csv-file.md)へのマッピングのチュートリアルに従って、データフィールドをXDMに変換し、プラットフォームに取り込む方法を学びます。 マッピングに対して[!UICONTROL 宛先]を選択する場合は、「**[!UICONTROL 既存のデータセット]**&#x200B;を使用」オプションを選択し、前に作成した[!DNL Profile]対応の同意データセットを選択します。

## 導入をテスト{#test-implementation}

[!DNL Profile]対応データセットに顧客の同意データを取り込んだ後、更新したプロファイルに同意の属性が含まれているかどうかを確認できます。

>[!IMPORTANT]
>
>UI内の既存のプロファイルの属性を表示するには、そのプロファイルに関連付けられた1つ以上のID値(および対応する名前空間)を把握しておく必要があります。
>
>この情報にアクセスできない場合は、独自のテスト同意データを取り込み、代わりにユーザーに知られているID値/名前空間に関連付けることができます。

プロファイルの詳細を調べる手順については、『[!DNL Profile] UIガイド』の「ID](../../../../profile/ui/user-guide.md#browse)によるプロファイルの参照」の節を参照してください。[

新しい同意属性は、デフォルトではプロファイルのダッシュボードに表示されません。 したがって、プロファイルの詳細ページの「**[!UICONTROL 属性]**」タブに移動して、期待どおりに取り込まれたことを確認する必要があります。 必要に応じてダッシュボードをカスタマイズする方法については、[プロファイルダッシュボード](../../../../profile/ui/profile-dashboard.md)のガイドを参照してください。

<!-- (To be included once CJM is GA)
## Handling consent in Customer Journey Management

If you are using Customer Journey Management, after confirming that your profiles and segments contain consent data, you can start honoring customer [marketing preferences](../../../../xdm/data-types/consents.md#marketing) when pulling segments from Platform. Specifically, profiles who have opted out of the email marketing preference should not be included in segments that are targeted for email campaigns.

Customer Journey Management can also send consent-change signals back to Platform. When a customer selects an "unsubscribe" link in an email message, the updated consent preference is sent to Platform and the appropriate profile attributes are updated accordingly.
-->

## 次の手順

このガイドでは、Adobe標準を使用して顧客の同意データを処理し、顧客プロファイルにこれらの属性を表示するように、Platformオペレーションを設定する方法について説明します。 セグメントクオリフィケーションや他の下流の使用例の決定要因として、顧客の同意を好む設定を組み込むことができるようになりました。

Experience Platformのプライバシー関連機能について詳しくは、プラットフォーム](../../overview.md)の[ガバナンス、プライバシー、セキュリティの概要を参照してください。
