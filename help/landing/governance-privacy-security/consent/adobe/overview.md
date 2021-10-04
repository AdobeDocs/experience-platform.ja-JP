---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: Adobe Experience Platformでの同意処理
topic-legacy: getting started
description: Adobe2.0 標準を使用して、Adobe Experience Platformで顧客の同意のシグナルを処理する方法を説明します。
exl-id: cd76a3f6-ae55-4d75-9b30-900fadb4664f
source-git-commit: 1c398cdac45141b4886d984db32fbac7ca60265c
workflow-type: tm+mt
source-wordcount: '1572'
ht-degree: 0%

---

# Adobe Experience Platformでの同意処理

Adobe Experience Platformを使用すると、顧客から収集した同意データを処理し、保存した顧客プロファイルに統合できます。 その後、このデータをダウンストリームプロセスで使用して、特定の顧客に対してデータ収集を行うか、またはそのプロファイルを特定の目的に使用するかを決定できます。 例えば、特定のプロファイルの同意データは、書き出されたオーディエンスセグメントに含めることができるか、電子メール、テキストメッセージ、プッシュ通知などの特定のマーケティングチャネルに参加できるかを決定できます。

このドキュメントでは、同意管理プラットフォーム (CMP) によって生成された顧客の同意データを取り込み、そのデータをダウンストリームの使用例のために顧客プロファイルに統合するように Platform データ操作を設定する方法の概要を説明します。

>[!NOTE]
>
>このドキュメントでは、Adobe標準を使用した同意データの処理に焦点を当てています。 IAB Transparency and Consent Framework(TCF)2.0 に準拠して同意データを処理する場合は、リアルタイム顧客データプラットフォームの [TCF 2.0 サポートのガイド ](../iab/overview.md) を参照してください。

## 前提条件

このガイドでは、同意データの処理に関わる様々なExperience Platformサービスに関する十分な知識が必要です。

* [エクスペリエンスデータモデル (XDM)](../../../../xdm/home.md):顧客体験データを整理する際に使用する標準化されたExperience Platformフレームワーク。
* [Adobe Experience Platform Identity Service](../../../../identity-service/home.md):デバイスやシステム間で ID を結び付けることで、顧客体験データの断片化によって生じる基本的な課題を解決します。
* [リアルタイム顧客プロファイル](../../../../profile/home.md):機能を使 [!DNL Identity Service] 用して、データセットから詳細な顧客プロファイルをリアルタイムで作成します。リアルタイム顧客プロファイルは、データレイクからデータを取り込み、顧客プロファイルを独立したデータストアに保持します。
* [Adobe Experience Platform Web SDK](../../../../edge/home.md):様々な Platform サービスを顧客向け Web サイトに統合できる、クライアントサイド JavaScript ライブラリ。
   * [SDK の同意コマンド](../../../../edge/consent/supporting-consent.md):このガイドに示す、同意関連の SDK コマンドの使用例の概要です。
* [Adobe Experience Platform Segmentation Service](../../../../segmentation/home.md):リアルタイム顧客プロファイルデータを、類似した特性を共有し、マーケティング戦略と同様に対応する個人のグループに分割できます。

## 同意処理フローの概要 {#summary}

次に、システムが適切に設定された後に同意データが処理される方法を示します。

1. 顧客が Web サイト上のダイアログを通じて、データ収集に関する同意設定を提供します。
1. ページの読み込み（または CMP が同意設定の変更を検出した場合）ごとに、サイトのカスタムスクリプトが現在の設定を標準 XDM オブジェクトにマッピングします。 次に、このオブジェクトが Platform Web SDK `setConsent` コマンドに渡されます。
1. `setConsent` が呼び出されると、Platform Web SDK は、同意の値が最後に受け取った値と異なるかどうかを確認します。 値が異なる（または以前の値がない）場合、構造化された同意/環境設定データがAdobe Experience Platformに送信されます。
1. 同意/環境設定データは、[!DNL Profile] 対応のデータセットに取り込まれます。このデータセットのスキーマには、同意/環境設定フィールドが含まれています。

CMP の同意変更フックによってトリガーされる SDK コマンドに加えて、同意データは、[!DNL Profile] 対応のデータセットに直接アップロードされた、顧客が生成した XDM データを通じてExperience Platformに送ることもできます。

### 同意の適用

Platform での同意処理のサポートの現在のリリースでは、データ収集権限 (`collect.val`) のみが Platform Web SDK によって自動的に適用されます。 より詳細な同意と環境設定を顧客プロファイルで収集して保持できますが、これらの追加のシグナルは、独自のダウンストリームプロセスで手動で適用する必要があります。

>[!NOTE]
>
>上記の XDM 同意フィールドの構造について詳しくは、[[!UICONTROL  同意と環境設定 ] データタイプ ](../../../../xdm/data-types/consents.md) のガイドを参照してください。

システムが設定されると、Platform Web SDK は、現在のユーザーのデータ収集の同意値を解釈して、データをAdobe Experience Platform Edge ネットワークに送信するか、クライアントから削除するか、データ収集権限が「はい」または「いいえ」に設定されるまで保持するかを決定します。

## CMP 内で顧客の同意データを生成する方法の決定 {#consent-data}

各 CMP システムは固有なので、お客様がサービスとやり取りする際に同意を提供できる最適な方法を決定する必要があります。 これを実現する一般的な方法は、次の例のような Cookie の同意ダイアログを使用することです。

![](../../../images/governance-privacy-security/consent/adobe/overview/consent-dialog.png)

このダイアログでは、顧客が特定のマーケティングおよびパーソナライゼーションの使用例をデータに対してオプトインまたはオプトアウトできる必要があります。 これらの同意と環境設定は、次の手順で [!DNL Profile] 有効にしたデータセットに対して定義するデータモデルに従う必要があります。

## [!DNL Profile] が有効なデータセットへの標準化された同意フィールドの追加 {#dataset}

顧客の同意データは、スキーマに同意フィールドが含まれている [!DNL Profile] 対応のデータセットに送信する必要があります。 これらのフィールドは、個々の顧客に関する属性情報を取り込むために使用するのと同じスキーマとデータセットに含める必要があります。

このガイドに進む前に、[!DNL Profile] 有効なデータセットにこれらの必須フィールドを追加する方法の詳細については、[ 同意データを取得するデータセットの設定 ](./dataset.md) に関するチュートリアルを参照してください。

## [!DNL Profile] 結合ポリシーを更新して、同意データを含めます {#merge-policies}

同意データを処理するための [!DNL Profile] 対応のデータセットを作成したら、結合ポリシーが各顧客プロファイルに常に同意フィールドを含むように設定されていることを確認する必要があります。 これには、競合する可能性がある他のデータセットよりも同意データセットの方が優先されるように、データセットの優先順位を設定する必要があります。

>[!NOTE]
>
>競合するデータセットがない場合は、結合ポリシーのタイムスタンプの優先順位を代わりに設定する必要があります。 これにより、顧客が指定した最新の同意が、使用される同意設定になることを保証できます。

結合ポリシーの操作方法の詳細については、まず [ 結合ポリシーの概要 ](../../../../profile/merge-policies/overview.md) を読んでください。 結合ポリシーを設定する場合、[ データセットの準備 ](./dataset.md) のガイドで説明されているように、[!UICONTROL  同意と環境設定 ] スキーマフィールドグループで提供される必要な同意属性がすべてプロファイルに含まれていることを確認します。

## 同意データを Platform に取り込む

顧客プロファイルに必要な同意フィールドを表すデータセットと結合ポリシーを取得したら、次の手順は、同意データ自体を Platform に取り込むことです。

主に、Adobe Experience Platform Web SDK を使用して、CMP で同意変更イベントが検出された場合は常に、同意データを Platform に送信する必要があります。 モバイルプラットフォームで同意データを収集する場合は、Adobe Experience Platform Mobile SDK を使用する必要があります。 また、収集した同意データを同意データセットの XDM スキーマにマッピングし、バッチ取得を通じて Platform に送信することで、直接取得した同意データを取り込むこともできます。

これらの各メソッドの詳細については、以下のサブセクションで説明します。

### 同意データを処理するExperience PlatformWeb SDK の設定 {#web-sdk}

Web サイト上の同意変更イベントをリッスンするように CMP を設定したら、Experience PlatformWeb SDK を統合して、更新された同意設定を受け取り、ページの読み込みのたびに、同意変更イベントが発生したときに Platform に送信できます。 詳しくは、[ 顧客の同意データを処理するための Web SDK の設定 ](../sdk.md) のガイドを参照してください。

### 同意データを処理するExperience PlatformMobile SDK の設定 {#mobile-sdk}

お客様の同意の設定がモバイルアプリケーションで必要な場合は、Experience PlatformMobile SDK を統合して、同意の設定を取得および更新し、同意 API が呼び出されるたびにそれらの設定を Platform に送信できます。

同意 API](https://aep-sdks.gitbook.io/docs/v/AEP-Edge-Docs/using-mobile-extensions/adobe-edge-consent/edge-consent-api-reference) を使用した [ 同意モバイル拡張機能 ](https://aep-sdks.gitbook.io/docs/v/AEP-Edge-Docs/using-mobile-extensions/adobe-edge-consent) および [ の設定については、Mobile SDK のドキュメントを参照してください。 Mobile SDK を使用してプライバシーに関する問題を処理する方法の詳細については、「[ プライバシーと GDPR](https://aep-sdks.gitbook.io/docs/v/AEP-Edge-Docs/resources/privacy-and-gdpr)」の節を参照してください。

### XDM 準拠の同意データの直接取り込み {#batch}

バッチ取得を使用して、XDM 準拠の同意データを CSV ファイルから取り込むことができます。 これは、以前に収集された同意データのバックログがまだ顧客プロファイルに統合されていない場合に役立ちます。

[CSV ファイルの XDM](../../../../ingestion/tutorials/map-a-csv-file.md) へのマッピングのチュートリアルに従って、データフィールドを XDM に変換し、Platform に取り込む方法を学びます。 マッピングに [!UICONTROL  宛先 ] を選択する場合は、必ず「**[!UICONTROL 既存のデータセット]** を使用」オプションを選択し、先ほど作成した [!DNL Profile] 対応の同意データセットを選択します。

## 実装のテスト {#test-implementation}

顧客の同意データを [!DNL Profile] 対応のデータセットに取り込んだら、更新したプロファイルに同意の属性が含まれているかどうかを確認できます。

>[!IMPORTANT]
>
>UI で既存のプロファイルの属性を表示するには、そのプロファイルに関連付けられている ID 値（および対応する名前空間）が少なくとも 1 つ必要です。
>
>この情報にアクセスできない場合は、独自のテストの同意データを取り込み、代わりに既知の ID 値/名前空間に関連付けることができます。

プロファイルの詳細を検索する手順については、『[!DNL Profile] UI ガイド』の「ID によるプロファイルの参照 ](../../../../profile/ui/user-guide.md#browse)」の節を参照してください。[

新しい同意属性は、デフォルトではプロファイルのダッシュボードに表示されません。 したがって、プロファイルの詳細ページで「**[!UICONTROL 属性]**」タブに移動して、期待どおりに取り込まれたことを確認する必要があります。 必要に応じてダッシュボードをカスタマイズする方法については、[ プロファイルダッシュボード ](../../../../profile/ui/profile-dashboard.md) のガイドを参照してください。

<!-- (To be included once CJM is GA)
## Handling consent in Customer Journey Management

If you are using Customer Journey Management, after confirming that your profiles and segments contain consent data, you can start honoring customer [marketing preferences](../../../../xdm/data-types/consents.md#marketing) when pulling segments from Platform. Specifically, profiles who have opted out of the email marketing preference should not be included in segments that are targeted for email campaigns.

Customer Journey Management can also send consent-change signals back to Platform. When a customer selects an "unsubscribe" link in an email message, the updated consent preference is sent to Platform and the appropriate profile attributes are updated accordingly.
-->

## 次の手順

このガイドでは、Adobe標準を使用して顧客の同意データを処理するように Platform の操作を設定し、これらの属性を顧客プロファイルで表す方法について説明しました。 顧客の同意設定を、セグメント認定やその他のダウンストリームの使用例の決定要因として統合できるようになりました。

Experience Platformのプライバシー関連機能について詳しくは、[Platform](../../overview.md) のガバナンス、プライバシー、セキュリティの概要を参照してください。
