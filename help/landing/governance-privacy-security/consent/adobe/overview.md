---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: Adobe Experience Platformでの同意処理
description: Adobe 2.0 標準を使用して、Adobe Experience Platformで顧客の同意シグナルを処理する方法を説明します。
role: Developer
feature: Consent
exl-id: cd76a3f6-ae55-4d75-9b30-900fadb4664f
source-git-commit: f988d7665a40b589ca281d439b6fca508f23cd03
workflow-type: tm+mt
source-wordcount: '1562'
ht-degree: 1%

---

# Adobe Experience Platformでの同意処理

Adobe Experience Platformを使用すると、顧客から収集した同意データを処理し、保存されている顧客プロファイルに統合できます。 その後、このデータをダウンストリームプロセスで使用して、特定の顧客についてデータ収集が行われるのか、特定の目的のために顧客のプロファイルが使用されるのかを判断できます。 例えば、特定のプロファイルの同意データによって、書き出されたオーディエンスセグメントにデータを含めることができるかどうか、または特定のマーケティングチャネル（メール、テキストメッセージ、プッシュ通知など）にデータを参加できるかどうかを決定できます。

このドキュメントでは、Experience Platform データ操作を設定して、同意管理プラットフォーム（CMP）によって生成された顧客の同意データを取り込み、ダウンストリームのユースケース用に顧客プロファイルに統合する方法の概要を説明します。

>[!NOTE]
>
>このドキュメントでは、Adobe標準を使用した同意データの処理に焦点を当てています。 IAB Transparency and Consent Framework （TCF） 2.0 に従って同意データを処理する場合は、[Adobe Real-Time Customer Data Platformでの TCF 2.0 のサポート &#x200B;](../iab/overview.md) に関するガイドを参照してください。

## 前提条件

このガイドでは、同意データの処理に関連する様々なExperience Platform サービスについて、実際に理解している必要があります。

* [Experience Data Model（XDM）](/help/xdm/home.md)：Adobe Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
* [Adobe Experience Platform ID サービス &#x200B;](/help/identity-service/home.md): デバイスやシステムをまたいで ID を結び付けることで、カスタマーエクスペリエンスのフラグメント化によって発生する基本的な課題を解決します。
* [&#x200B; リアルタイム顧客プロファイル &#x200B;](/help/profile/home.md):[!DNL Identity Service] の機能を使用して、データセットから詳細な顧客プロファイルをリアルタイムで作成します。 リアルタイム顧客プロファイルは、データレイクからデータを取り込み、顧客プロファイルを独立したデータストアに保持します。
* [Adobe Experience Platform web SDK](/help/collection/js/js-overview.md)：様々なExperience Platform サービスをお客様に向けた web サイトに統合できるクライアントサイド JavaScript ライブラリです。
   * [SDK同意コマンド &#x200B;](/help/collection/js/commands/setconsent.md)：このガイドに記載されている同意関連のSDK コマンドのユースケースの概要です。
* [Adobe Experience Platform セグメント化サービス &#x200B;](/help/segmentation/home.md): リアルタイム顧客プロファイルデータを、類似の特性を持ち、マーケティング戦略に対して同様の反応を示す個人のグループに分割できます。

## 同意処理フローの概要 {#summary}

次に、システムが適切に設定された後に同意データが処理される方法について説明します。

1. 顧客は、web サイト上のダイアログを通じて、データ収集に対する同意環境設定を指定します。
1. ページの読み込み（または CMP が同意環境設定の変更を検出した場合）ごとに、サイト上のカスタムスクリプトが現在の環境設定を標準 XDM オブジェクトにマッピングします。 次に、このオブジェクトはExperience Platform web SDK `setConsent` コマンドに渡されます。
1. `setConsent` が呼び出されると、Experience Platform Web SDKは同意値が最後に受信した値と異なるかどうかを確認します。 値が異なる（または以前の値がない）場合は、構造化された同意/環境設定データがAdobe Experience Platformに送信されます。
1. 同意/環境設定データは、同意/環境設定フィールドを含んだスキーマを持つ [!DNL Profile] 対応データセットに取り込まれます。

CMP の同意変更フックによってトリガーされるSDK コマンドに加えて、同意データは、[!DNL Profile] 対応データセットに直接アップロードされる顧客生成の XDM データを介してExperience Platformにも送ることができます。

### 同意の適用

Experience Platformでの同意処理のサポートの現在のリリースでは、データ収集権限（`collect.val`）のみがExperience Platform web SDKによって自動的に適用されます。 顧客プロファイルで同意および環境設定をよりきめ細かく収集して保持できますが、これらの追加のシグナルは、独自のダウンストリームプロセスで手動で適用する必要があります。

>[!NOTE]
>
>上記の XDM 同意フィールドの構造について詳しくは、[[!UICONTROL Consents and Preferences] データタイプのガイドを参照してください &#x200B;](/help/xdm/data-types/consents.md)

システムが設定されると、Experience Platform Web SDKが現在のユーザーのデータ収集の同意値を解釈し、データがAdobe Experience Platform Edge Networkに送信されるか、クライアントからドロップされるか、データ収集権限が yes または no に設定されるまで保持される必要があるかどうかを判断します。

## CMP 内で顧客同意データを生成する方法の決定 {#consent-data}

各 CMP システムは一意なので、顧客がサービスとやり取りする際に同意を提供できる最適な方法を決定する必要があります。 これを行う一般的な方法は、次の例のような cookie 同意ダイアログを使用することです。

![](../../../images/governance-privacy-security/consent/adobe/overview/consent-dialog.png)

このダイアログでは、顧客がデータの特定のマーケティングおよびパーソナライゼーションのユースケースをオプトインまたはオプトアウトできるようにする必要があります。 これらの同意と環境設定は、次の手順で [!DNL Profile] 対応データセット用に定義するデータモデルに準拠している必要があります。

## [!DNL Profile] 対応データセットへの標準化された同意フィールドの追加 {#dataset}

顧客の同意データは、スキーマに同意フィールドが含まれている [!DNL Profile] 対応データセットに送信する必要があります。 これらのフィールドは、個々の顧客に関する属性情報の取得に使用するのと同じスキーマおよびデータセットに含める必要があります。

このガイドを続行する前に、これらの必須フィールドを [&#x200B; 対応データセットに追加する方法の詳細な手順については、](./dataset.md) 同意データを取得するためのデータセットの設定 [!DNL Profile] に関するチュートリアルを参照してください。

## 同意データ [!DNL Profile] 含めるように結合ポリシーを更新する {#merge-policies}

同意データを処理するための [!DNL Profile] 対応データセットを作成したら、各顧客プロファイルに常に同意フィールドを含めるように結合ポリシーが設定されていることを確認する必要があります。 これには、競合する可能性がある他のデータセットよりも同意データセットが優先されるように、データセットの優先順位を設定することが含まれます。

>[!NOTE]
>
>競合するデータセットがない場合は、代わりに結合ポリシーのタイムスタンプの優先順位を設定する必要があります。 これにより、顧客から指定された最新の同意が使用される同意設定になります。

結合ポリシーの使用方法について詳しくは、まず [&#x200B; 結合ポリシーの概要 &#x200B;](../../../../profile/merge-policies/overview.md) をお読みください。 結合ポリシーを設定する場合は、[!UICONTROL Consents and Preferences] データセットの準備 [&#x200B; に関するガイドに記載されているように、](./dataset.md) スキーマフィールドグループによって提供される必要なすべての同意属性がプロファイルに含まれていることを確認する必要があります。

## 同意データをExperience Platformに取り込む

データセットと結合ポリシーを用意して、顧客プロファイルで必要な同意フィールドを表したら、次の手順で、同意データ自体をExperience Platformに取り込みます。

第一に、CMP が同意変更イベントを検出した場合は常に、Adobe Experience Platform Web SDKを使用して同意データをExperience Platformに送信する必要があります。 モバイルプラットフォームで同意データを収集する場合は、Adobe Experience Platform Mobile SDKを使用する必要があります。 また、収集した同意データを、同意データセットの XDM スキーマにマッピングし、バッチ取り込みを通じてExperience Platformに送信することで、直接取り込むことを選択することもできます。

これらの各方法の詳細については、以下のサブセクションで説明します。

### 同意データを処理するためのExperience Platform Web SDKの設定 {#web-sdk}

Web サイトで同意変更イベントをリッスンするように CMP を設定したら、Experience Platform Web SDKを統合して、更新された同意設定を受け取り、ページの読み込みごとに、また同意変更イベントが発生するたびにExperience Platformに送信することができます。 詳しくは、[&#x200B; 顧客同意データを処理するための Web SDKの設定 &#x200B;](../sdk.md) に関するガイドを参照してください。

### 同意データを処理するためのExperience Platform Mobile SDKの設定 {#mobile-sdk}

モバイルアプリケーションで顧客の同意環境設定が必要な場合は、Experience Platform Mobile SDKを統合して、同意設定を取得および更新し、同意 API が呼び出されるたびに同意をExperience Platformに送信できます。

[&#x200B; 同意モバイル拡張機能の設定 &#x200B;](https://developer.adobe.com/client-sdks/documentation/consent-for-edge-network/) および [&#x200B; 同意 API の使用 &#x200B;](https://developer.adobe.com/client-sdks/documentation/consent-for-edge-network/api-reference/) については、Mobile SDKのドキュメントを参照してください。 Mobile SDKを使用してプライバシーに関する懸念を処理する方法について詳しくは、「[&#x200B; プライバシーと GDPR](https://developer.adobe.com/client-sdks/resources/privacy-and-gdpr/)」の節を参照してください。

### XDM 準拠の同意データの直接取り込み {#batch}

バッチ取り込みを使用して、XDM 準拠の同意データを CSV ファイルから取り込むことができます。 これは、まだ顧客プロファイルに統合されていない、以前に収集した同意データのバックログがある場合に役立ちます。

データフィールドを XDM に変換し、Experience Platformに取り込む方法については、[CSV ファイルを XDM にマッピングする &#x200B;](../../../../ingestion/tutorials/map-csv/overview.md) に関するチュートリアルに従ってください。 マッピングの [!UICONTROL Destination] を選択する場合は、「**[!UICONTROL Use existing dataset]**」オプションを選択し、前に作成した [!DNL Profile] 対応の同意データセットを選択します。

## 実装のテスト {#test-implementation}

顧客の同意データを [!DNL Profile] 対応データセットに取り込んだら、更新されたプロファイルに同意属性が含まれているかどうかを確認できます。

>[!IMPORTANT]
>
>UI で既存のプロファイルの属性を表示するには、そのプロファイルに関連付けられた少なくとも 1 つの ID 値（および対応する名前空間）を把握している必要があります。
>
>この情報にアクセスできない場合は、独自のテスト同意データを取り込み、代わりに、既知の ID 値/名前空間に関連付けることができます。

プロファイルの詳細を検索する方法に関する具体的な手順については、[&#x200B; UI ガイドの &#x200B;](../../../../profile/ui/user-guide.md#browse)ID によるプロファイルの参照 [!DNL Profile] に関する節を参照してください。

新しい同意属性は、デフォルトではプロファイルのダッシュボードには表示されません。 したがって、プロファイルが期待どおりに取り込まれていることを確認するには、プロファイルの詳細ページの「**[!UICONTROL Attributes]**」タブに移動する必要があります。 ニーズに合わせてダッシュボードをカスタマイズする方法については、[&#x200B; プロファイルダッシュボード &#x200B;](../../../../profile/ui/profile-dashboard.md) に関するガイドを参照してください。

<!-- (To be included once CJM is GA)
## Handling consent in Customer Journey Management

If you are using Customer Journey Management, after confirming that your profiles and segments contain consent data, you can start honoring customer [marketing preferences](../../../../xdm/data-types/consents.md#marketing) when pulling segments from Experience Platform. Specifically, profiles who have opted out of the email marketing preference should not be included in segments that are targeted for email campaigns.

Customer Journey Management can also send consent-change signals back to Experience Platform. When a customer selects an "unsubscribe" link in an email message, the updated consent preference is sent to Experience Platform and the appropriate profile attributes are updated accordingly.
-->

## 次の手順

このガイドでは、Adobe標準を使用して顧客の同意データを処理し、それらの属性を顧客プロファイルで表すようにExperience Platformを設定する方法について説明しました。 セグメントの選定やその他のダウンストリームのユースケースの決定要因として、顧客同意環境設定を統合できるようになりました。

Experience Platformのプライバシー関連の機能について詳しくは、[Experience Platformでのガバナンス、プライバシー、セキュリティ &#x200B;](../../overview.md) の概要を参照してください。
