---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: Adobe Experience Platformでの同意処理
topic-legacy: getting started
description: Adobe2.0標準を使用して、Adobe Experience Platformで顧客の同意シグナルを処理する方法を説明します。
exl-id: cd76a3f6-ae55-4d75-9b30-900fadb4664f
source-git-commit: 1c398cdac45141b4886d984db32fbac7ca60265c
workflow-type: tm+mt
source-wordcount: '1572'
ht-degree: 0%

---

# Adobe Experience Platformでの同意処理

Adobe Experience Platformでは、顧客から収集した同意データを処理し、保存した顧客プロファイルに統合できます。 その後、このデータをダウンストリームプロセスで使用して、特定の顧客に対してデータ収集を行うか、またはそのプロファイルを特定の目的に使用するかを決定できます。 例えば、特定のプロファイルの同意データは、書き出されたオーディエンスセグメントに含めることができるか、電子メール、テキストメッセージ、プッシュ通知などの特定のマーケティングチャネルに参加できるかを決定できます。

このドキュメントでは、同意管理プラットフォーム(CMP)によって生成された顧客の同意データを取り込み、そのデータをダウンストリームの使用例のために顧客プロファイルに統合するためのPlatformデータ操作の設定方法の概要を説明します。

>[!NOTE]
>
>このドキュメントでは、Adobe標準を使用した同意データの処理に焦点を当てます。 IAB Transparency and Consent Framework(TCF)2.0に準拠して同意データを処理する場合は、リアルタイム顧客データプラットフォームの[TCF 2.0サポートに関するガイド](../iab/overview.md)を参照してください。

## 前提条件

このガイドでは、同意データの処理に関わる様々なExperience Platformサービスに関する十分な知識が必要です。

* [エクスペリエンスデータモデル(XDM)](../../../../xdm/home.md):顧客体験データを整理する際に使用する標準化されたExperience Platformフレームワーク。
* [Adobe Experience Platform Identity Service](../../../../identity-service/home.md):デバイスやシステム間でIDを結び付けることで、顧客体験データの断片化によって生じる基本的な課題を解決します。
* [リアルタイム顧客プロファイル](../../../../profile/home.md):機能を使 [!DNL Identity Service] 用して、データセットから詳細な顧客プロファイルをリアルタイムで作成します。リアルタイム顧客プロファイルは、データレイクからデータを取り込み、顧客プロファイルを独自の別々のデータストアに保持します。
* [Adobe Experience Platform Web SDK](../../../../edge/home.md):様々なPlatformサービスを顧客向けWebサイトに統合できる、クライアントサイドJavaScriptライブラリ。
   * [SDKの同意コマンド](../../../../edge/consent/supporting-consent.md):このガイドに示す、同意関連のSDKコマンドの使用例の概要です。
* [Adobe Experience Platform Segmentation Service](../../../../segmentation/home.md):リアルタイム顧客プロファイルデータを、類似した特性を共有し、マーケティング戦略と同様に対応する個人のグループに分割できます。

## 同意処理フローの概要 {#summary}

システムが正しく設定された後に同意データを処理する方法を次に示します。

1. 顧客がWebサイト上のダイアログを通じて、データ収集に関する同意設定を提供します。
1. 各ページの読み込み時（またはCMPが同意設定の変更を検出した場合）、サイトのカスタムスクリプトは現在の環境設定を標準XDMオブジェクトにマッピングします。 次に、このオブジェクトがPlatform Web SDK `setConsent`コマンドに渡されます。
1. `setConsent`が呼び出されると、Platform Web SDKは、同意の値が最後に受け取った値と異なるかどうかを確認します。 値が異なる（または以前の値がない）場合、構造化された同意/環境設定データがAdobe Experience Platformに送信されます。
1. 同意/環境設定データは、[!DNL Profile]が有効なデータセットに取り込まれます。このデータセットのスキーマに、同意/環境設定フィールドが含まれています。

CMPの同意変更フックでトリガーされるSDKコマンドに加えて、同意データは、[!DNL Profile]対応のデータセットに直接アップロードされた、顧客が生成したXDMデータを通じてExperience Platformに送ることもできます。

### 同意の適用

Platformでの同意処理のサポートの現在のリリースでは、Platform Web SDKによってデータ収集権限(`collect.val`)のみが自動的に適用されます。 顧客プロファイルでより詳細な同意と環境設定を収集して保持できますが、これらの追加のシグナルは、独自のダウンストリームプロセスで手動で適用する必要があります。

>[!NOTE]
>
>上記のXDM同意フィールドの構造について詳しくは、[[!UICONTROL 同意と環境設定]データタイプ](../../../../xdm/data-types/consents.md)のガイドを参照してください。

システムが設定されると、Platform Web SDKは、現在のユーザーのデータ収集の同意値を解釈し、データをAdobe Experience Platform Edgeネットワークに送信するか、クライアントから削除するか、データ収集権限が「はい」または「いいえ」に設定されるまで保持するかを決定します。

## CMP内で顧客の同意データを生成する方法の決定 {#consent-data}

各CMPシステムは固有なので、顧客がサービスとやり取りする際に同意を得る最適な方法を決定する必要があります。 これを実現する一般的な方法は、次の例のようなCookieの同意ダイアログを使用することです。

![](../../../images/governance-privacy-security/consent/adobe/overview/consent-dialog.png)

このダイアログでは、顧客がデータの特定のマーケティングおよびパーソナライゼーションの使用例をオプトインまたはオプトアウトできるはずです。 これらの同意と環境設定は、次の手順で[!DNL Profile]が有効になったデータセットに対して定義するデータモデルに準拠している必要があります。

## [!DNL Profile]が有効なデータセットへの標準化された同意フィールドの追加 {#dataset}

顧客の同意データは、スキーマに同意フィールドが含まれている[!DNL Profile]対応のデータセットに送信する必要があります。 これらのフィールドは、個々の顧客に関する属性情報を取り込むために使用するのと同じスキーマとデータセットに含める必要があります。

このガイドに進む前に、[同意データを取得するためのデータセット](./dataset.md)の設定に関するチュートリアルを参照して、これらの必須フィールドを[!DNL Profile]対応のデータセットに追加する方法の詳細を確認してください。

## [!DNL Profile]結合ポリシーを更新して同意データを含める {#merge-policies}

同意データを処理するための[!DNL Profile]対応のデータセットを作成したら、結合ポリシーが各顧客プロファイルに常に同意フィールドを含むように設定されていることを確認する必要があります。 これには、競合する可能性がある他のデータセットよりも同意データセットの優先順位を優先するように、データセットの優先順位を設定する必要があります。

>[!NOTE]
>
>競合するデータセットがない場合は、結合ポリシーのタイムスタンプの優先順位を代わりに設定する必要があります。 これにより、顧客が指定した最新の同意が、使用される同意設定になるようになります。

結合ポリシーの操作方法の詳細については、まず[結合ポリシーの概要](../../../../profile/merge-policies/overview.md)をお読みください。 結合ポリシーを設定する場合、[データセットの準備](./dataset.md)のガイドで説明されているように、[!UICONTROL 同意と環境設定]スキーマフィールドグループが提供する必要な同意属性がすべてプロファイルに含まれていることを確認する必要があります。

## 同意データをPlatformに取り込む

顧客プロファイルに必要な同意フィールドを表すデータセットと結合ポリシーを取得したら、次の手順は、同意データ自体をPlatformに取り込むことです。

主に、Adobe Experience Platform Web SDKを使用して、CMPで同意変更イベントが検出されるたびに、同意データをPlatformに送信する必要があります。 モバイルプラットフォームで同意データを収集する場合は、Adobe Experience Platform Mobile SDKを使用する必要があります。 また、収集した同意データを同意データセットのXDMスキーマにマッピングし、バッチ取り込みを通じてPlatformに送信することで、収集した同意データを直接取り込むこともできます。

これらの各メソッドの詳細については、以下のサブセクションで説明します。

### 同意データを処理するExperience PlatformWeb SDKの設定 {#web-sdk}

Webサイト上の同意変更イベントをリッスンするようにCMPを設定したら、Experience PlatformWeb SDKを統合して、更新された同意設定を受け取り、ページの読み込みのたびに、同意変更イベントが発生したときにPlatformに送信できます。 詳しくは、[顧客の同意データを処理するためのWeb SDKの設定](../sdk.md)に関するガイドを参照してください。

### 同意データを処理するExperience PlatformMobile SDKの設定 {#mobile-sdk}

モバイルアプリケーションで顧客の同意の設定が必要な場合は、Experience PlatformMobile SDKを統合して同意設定を取得および更新し、同意APIが呼び出されるたびにそれらの設定をPlatformに送信できます。

[同意API](https://aep-sdks.gitbook.io/docs/v/AEP-Edge-Docs/using-mobile-extensions/adobe-edge-consent/edge-consent-api-reference)を使用した同意モバイル拡張機能](https://aep-sdks.gitbook.io/docs/v/AEP-Edge-Docs/using-mobile-extensions/adobe-edge-consent)および[の設定については、Mobile SDKのドキュメントを参照してください。 モバイルSDKを使用してプライバシーに関する問題を処理する方法の詳細については、「[プライバシーとGDPR](https://aep-sdks.gitbook.io/docs/v/AEP-Edge-Docs/resources/privacy-and-gdpr)」の節を参照してください。

### XDM準拠の同意データの直接取得 {#batch}

バッチ取得を使用して、XDM準拠の同意データをCSVファイルから取得できます。 これは、以前に収集された同意データのバックログがまだ顧客プロファイルに統合されていない場合に役立ちます。

[CSVファイルのXDM](../../../../ingestion/tutorials/map-a-csv-file.md)へのマッピングのチュートリアルに従って、データフィールドをXDMに変換し、Platformに取り込む方法を学びます。 マッピングに[!UICONTROL 宛先]を選択する場合は、必ず「**[!UICONTROL 既存のデータセット]**&#x200B;を使用」オプションを選択し、前に作成した[!DNL Profile]対応の同意データセットを選択します。

## 実装のテスト {#test-implementation}

顧客の同意データを[!DNL Profile]対応データセットに取り込んだら、更新したプロファイルに同意の属性が含まれているかどうかを確認できます。

>[!IMPORTANT]
>
>UIに既存のプロファイルの属性を表示するには、そのプロファイルに関連付けられているID値（および対応する名前空間）が少なくとも1つ必要です。
>
>この情報にアクセスできない場合は、独自のテスト同意データを取り込み、代わりに既知のID値/名前空間に関連付けることができます。

プロファイルの詳細を検索する手順について詳しくは、『[!DNL Profile] UIガイド』の「IDによるプロファイルの参照](../../../../profile/ui/user-guide.md#browse) 」の節を参照してください。[

新しい同意属性は、デフォルトではプロファイルのダッシュボードに表示されません。 したがって、プロファイルの詳細ページの「**[!UICONTROL 属性]**」タブに移動して、期待どおりに取り込まれたことを確認する必要があります。 必要に応じてダッシュボードをカスタマイズする方法については、[プロファイルダッシュボード](../../../../profile/ui/profile-dashboard.md)のガイドを参照してください。

<!-- (To be included once CJM is GA)
## Handling consent in Customer Journey Management

If you are using Customer Journey Management, after confirming that your profiles and segments contain consent data, you can start honoring customer [marketing preferences](../../../../xdm/data-types/consents.md#marketing) when pulling segments from Platform. Specifically, profiles who have opted out of the email marketing preference should not be included in segments that are targeted for email campaigns.

Customer Journey Management can also send consent-change signals back to Platform. When a customer selects an "unsubscribe" link in an email message, the updated consent preference is sent to Platform and the appropriate profile attributes are updated accordingly.
-->

## 次の手順

このガイドでは、Adobe標準を使用して顧客の同意データを処理するようにPlatformの操作を設定し、これらの属性を顧客プロファイルに表示する方法について説明しました。 セグメント認定やその他のダウンストリームの使用例で、顧客の同意設定を決定要因として統合できるようになりました。

Experience Platformのプライバシー関連機能について詳しくは、Platform](../../overview.md)の[ガバナンス、プライバシー、セキュリティの概要を参照してください。
