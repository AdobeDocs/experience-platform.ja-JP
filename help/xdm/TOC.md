---
audience: user
user-guide-title: エクスペリエンスデータモデル（XDM）システムヘルプ
breadcrumb-title: Experience Data Model（XDM）ガイド
user-guide-description: エクスペリエンスデータモデル(XDM)クラスとスキーマフィールドグループを使用して、エクスペリエンスデータを標準化します。
feature: Schemas
source-git-commit: 8133804076b1c0adf2eae5b748e86a35f3186d14
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 41%

---


# エクスペリエンスデータモデル（XDM）システム {#xdm}

* [XDM システムの概要](home.md)
* スキーマ {#schema}
   * [スキーマ合成の基本](schema/composition.md)
   * [データモデリングのベストプラクティス](schema/best-practices.md)
   * [XDMフィールドタイプの制約](schema/field-constraints.md)
   * [XDMでの名前空間](./schema/namespaces.md)
   * [XDM フィールド辞書](schema/field-dictionary.md)
   * 業界データモデル{#industries}
      * [概要](./schema/industries/overview.md)
      * [小売](./schema/industries/retail.md)
      * [金融サービス](./schema/industries/financial.md)
      * [通信業](./schema/industries/telecom.md)
      * [旅行と接客業](./schema/industries/travel-hospitality.md)
* クラス {#classes}
   * [XDM 個人プロファイル](./classes/individual-profile.md)
   * [XDM ExperienceEvent](./classes/experienceevent.md)
   * [セグメント定義](./classes/segment-definition.md)
* スキーマフィールドグループ {#field-groups}
   * プロファイルフィールドグループ{#profile}
      * [人口統計の詳細](./field-groups/profile/demographic-details.md)
      * [IAB TCF 2.0の同意](./field-groups/profile/iab.md)
      * [IdentityMap](./field-groups/profile/identitymap.md)
      * [ロイヤルティの詳細](./field-groups/profile/loyalty-details.md)
      * [個人の連絡先の詳細](./field-groups/profile/personal-contact-details.md)
      * [同意と環境設定](./field-groups/profile/consents.md)
      * [セグメントメンバーシップの詳細](./field-groups/profile/segmentation.md)
      * [通信購読](./field-groups/profile/telecom-subscription.md)
      * [勤務先の詳細](./field-groups/profile/work-contact-details.md)
   * イベントフィールドグループ{#event}
      * [キャンペーンマーケティングの詳細](./field-groups/event/campaign-marketing-details.md)
      * [チャネルの詳細](./field-groups/event/channel-details.md)
      * [コマースの詳細](./field-groups/event/commerce-details.md)
      * [デバイスのトレードインの詳細](./field-groups/event/device-trade-in-details.md)
      * [エンドユーザーIDの詳細](./field-groups/event/enduserids.md)
      * [環境の詳細](./field-groups/event/environment-details.md)
      * [IAB TCF 2.0の同意](./field-groups/event/iab.md)
      * [Webの詳細](./field-groups/event/web-details.md)
   * [フィールドグループ名の更新](./field-groups/name-updates.md)
* データタイプ {#data-types}
   * [アプリケーション](./data-types/application.md)
   * [ビーコン](./data-types/beacon.md)
   * [ブラウザーの詳細](./data-types/browser-details.md)
   * [コマース](./data-types/commerce.md)
   * [Consent String](./data-types/consent-string.md)
   * [同意と環境設定](./data-types/consents.md)
   * [通貨](./data-types/currency.md)
   * [デバイス](./data-types/device.md)
   * [電子メールアドレス](./data-types/email-address.md)
   * [環境](./data-types/environment.md)
   * [エクスペリエンスチャネル](./data-types/experience-channel.md)
   * [一般的な同意フィールド](./data-types/consent-field.md)
   * [汎用マーケティング環境設定フィールド](./data-types/marketing-field.md)
   * [購読付きの汎用マーケティング環境設定フィールド](./data-types/marketing-field-subscriptions.md)
   * [汎用パーソナライゼーション設定フィールド](./data-types/personalization-field.md)
   * [地域](./data-types/geo.md)
   * [ジオサークル](./data-types/geo-circle.md)
   * [地理座標](./data-types/geo-coordinates.md)
   * [地域インタラクションの詳細](./data-types/geo-interaction-details.md)
   * [ジオシェイプ](./data-types/geo-shape.md)
   * [ID](./data-types/identity.md)
   * [マーケティング](./data-types/marketing.md)
   * [測定](./data-types/measure.md)
   * [Order](./data-types/order.md)
   * [支払品目](./data-types/payment-item.md)
   * [ユーザー](./data-types/person.md)
   * [ユーザー名](./data-types/person-name.md)
   * [電話番号](./data-types/phone-number.md)
   * [場所コンテキスト](./data-types/place-context.md)
   * [POIの詳細](./data-types/poi-details.md)
   * [POIインタラクション](./data-types/poi-interaction.md)
   * [住所](./data-types/postal-address.md)
   * [製品リスト項目](./data-types/product-list-item.md)
   * [検索](./data-types/search.md)
   * [購読](./data-types/subscription.md)
   * [通信購読](./data-types/telecom-subscription.md)
   * [Web情報](./data-types/web-information.md)
   * [Webインタラクション](./data-types/web-interaction.md)
   * [Webページの詳細](./data-types/webpage-details.md)
*  スキーマUI  {#ui}
   * [概要](./ui/overview.md)
   * [XDM リソースの参照](./ui/explore.md)
   * リソースの作成と編集{#resources}
      * [スキーマ](./ui/resources/schemas.md)
      * [クラス](./ui/resources/classes.md)
      * [フィールドグループ](./ui/resources/field-groups.md)
      * [データタイプ](./ui/resources/data-types.md)
   * フィールド{#fields}を定義します。
      * [概要](./ui/fields/overview.md)
      * [必須フィールド](./ui/fields/required.md)
      * [オブジェクトフィールド](./ui/fields/object.md)
      * [配列フィールド](./ui/fields/array.md)
      * [列挙フィールド](./ui/fields/enum.md)
      * [ID フィールド](./ui/fields/identity.md)
      * [関係フィールド](./ui/fields/relationship.md)
   * [サンプルXDMデータの生成](./ui/sample.md)
   * [XDMスキーマの書き出し](./ui/export.md)
* スキーマレジストリ API {#api}
   * [概要](api/overview.md)
   * [はじめに](api/getting-started.md)
   * [スキーマ](api/schemas.md)
   * [動作](api/behaviors.md)
   * [クラス](api/classes.md)
   * [スキーマフィールドグループ](api/field-groups.md)
   * [データタイプ](api/data-types.md)
   * [記述子](api/descriptors.md)
   * [和集合](api/unions.md)
   * [書き出し/読み込み](api/export-import.md)
   * [サンプルデータ](api/sample-data.md)
   * [監査ログ](api/audit-log.md)
   * [アドホックスキーマ](api/ad-hoc.md)
   * [Mixin（非推奨）](api/mixins.md)
   * [付録](api/appendix.md)
* チュートリアル {#tutorials}
   * [スキーマの作成（UI）](tutorials/create-schema-ui.md)
   * [スキーマの作成（API）](tutorials/create-schema-api.md)
   * [2 つのスキーマ間の関係の定義（UI）](tutorials/relationship-ui.md)
   * [2 つのスキーマ間の関係の定義（API）](tutorials/relationship-api.md)
   * [アドホックスキーマの作成（API）](tutorials/ad-hoc.md)
* [トラブルシューティングガイド](troubleshooting-guide.md)
* [API リファレンス](https://www.adobe.io/experience-platform-apis/references/schema-registry/)
* [Platform リリースノート](https://docs.adobe.com/content/help/ja-JP/experience-platform/release-notes/latest.html)