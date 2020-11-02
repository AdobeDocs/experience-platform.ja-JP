---
product: experience-platform
audience: user
user-guide-title: エクスペリエンスデータモデル（XDM）システムヘルプ
breadcrumb-title: Data Model（XDM）ガイド
user-guide-description: エクスペリエンスデータモデル（XDM）クラスと Mixin を使用して、エクスペリエンスデータを標準化します。
translation-type: tm+mt
source-git-commit: 0a5b6bab6a0a11a572a4cd5de95b33f8d61d34bc
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 69%

---


# エクスペリエンスデータモデル（XDM）システム {#xdm}

* [XDM システムの概要](home.md)
* スキーマ {#schema}
   * [スキーマ構成の基本](schema/composition.md)
   * [データモデリングのベストプラクティス](schema/best-practices.md)
   * [XDMフィールド型の制約](schema/field-constraints.md)
   * [XDM フィールド辞書](schema/field-dictionary.md)
   * スキーマの使用例 {#use-cases}
      * [プライバシーの同意ミックスイン](schema/privacy-consent.md)
* クラス {#classes}
   * [XDM 個人プロファイル](./classes/individual-profile.md)
   * [XDM ExperienceEvent](./classes/experienceevent.md)
* ミックスイン {#mixins}
   * プロファイルミックスイン {#profile}
      * [IdentityMap](./mixins/profile/identitymap.md)
      * [人口統計の詳細](./mixins/profile/person-details.md)
      * [個人の連絡先の詳細](./mixins/profile/personal-details.md)
      * [セグメントのメンバーシップの詳細](./mixins/profile/segmentation.md)
      * [勤務先担当者の詳細](./mixins/profile/work-details.md)
   * イベントミックスイン {#event}
      * [エンドユーザーIDの詳細](./mixins/event/enduserids.md)
      * [環境の詳細](./mixins/event/environment-details.md)
   * [Mixin名の更新](./mixins/name-updates.md)
* データタイプ {#data-types}
   * [Beacon](./data-types/beacon.md)
   * [ブラウザーの詳細](./data-types/browser-details.md)
   * [デバイス](./data-types/device.md)
   * [電子メールアドレス](./data-types/email-address.md)
   * [環境](./data-types/environment.md)
   * [地域](./data-types/geo.md)
   * [地域サークル](./data-types/geo-circle.md)
   * [地域座標](./data-types/geo-coordinates.md)
   * [地域とのやり取りの詳細](./data-types/geo-interaction-details.md)
   * [ジオシェイプ](./data-types/geo-shape.md)
   * [ID](./data-types/identity.md)
   * [個人名](./data-types/person-name.md)
   * [電話番号](./data-types/phone-number.md)
   * [配置コンテキスト](./data-types/place-context.md)
   * [POIの詳細](./data-types/poi-details.md)
   * [POI相互作用](./data-types/poi-interaction.md)
   * [住所](./data-types/postal-address.md)
* スキーマレジストリ API {#api}
   * [はじめに](api/getting-started.md)
   * [リソースの一覧](api/list-resources.md)
   * [リソースの検索](api/look-up-resource.md)
   * [リソースの更新](api/update-resource.md)
   * [リソースの置換](api/replace-resource.md)
   * [リソースの削除](api/delete-resource.md)
   * [クラスの作成](api/create-class.md)
   * [Mixin の作成](api/create-mixin.md)
   * [データタイプの作成](api/create-data-type.md)
   * [スキーマ](api/create-schema.md)
   * [和集合](api/unions.md)
   * [記述子](api/descriptors.md)
   * [アドホックスキーマ](api/ad-hoc.md)
   * [付録](api/appendix.md)
* チュートリアル {#tutorials}
   * [スキーマの作成（API）](tutorials/create-schema-api.md)
   * [スキーマの作成（UI）](tutorials/create-schema-ui.md)
   * [2 つのスキーマ間の関係の定義（API）](tutorials/relationship-api.md)
   * [2 つのスキーマ間の関係の定義（UI）](tutorials/relationship-ui.md)
   * [アドホックスキーマの作成（API）](tutorials/ad-hoc.md)
* [トラブルシューティングガイド](troubleshooting-guide.md)
* [API リファレンス](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml)
* [プラットフォームのリリースノート](https://docs.adobe.com/content/help/ja-JP/experience-platform/release-notes/latest.html)