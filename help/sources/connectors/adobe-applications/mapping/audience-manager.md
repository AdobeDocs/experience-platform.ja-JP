---
keywords: Experience Platform；ホーム；人気のトピック；Audience Manager マッピング；audience manager マッピング
solution: Experience Platform
title: Adobe Audience Manager Source コネクタのマッピングフィールド
description: Audience Manager ソースコネクタの対応するAdobe Audience Manager データモデル（XDM）フィールドに、エクスペリエンスデータ（リアルタイムデータ、オンボーディングされたデータおよびプロファイルデータ）をマッピングする方法について説明します。
exl-id: b800ba43-c308-4334-adce-3d554d50cefb
source-git-commit: be2ad7a02d4bdf5a26a0847c8ee7a9a93746c2ad
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 70%

---

# Audience Manager フィールドのマッピング

次の表には、Adobe Audience Manager データのフィールド（リアルタイム、オンボード、プロファイルデータ）と対応する XDM フィールド間のマッピングが含まれています。

各 XDM フィールドについて詳しくは、『[XDM フィールドディクショナリ](../../../../xdm/schema/field-dictionary.md)』を参照してください。

## リアルタイムデータ

タイプ：リアルタイムデータ

| リアルタイムデータフィールド | XDM フィールド |
| --- | --- |
| `requestIds[]` | `ExperienceEvent.identityMap["ECID"]` |
| `requestIds[]` | `ExperienceEvent.endUserIds` - *endUserIds に存在する名前空間と最初の値のみ。* |
| `primaryDeviceId` | `ExperienceEvent.identityMap["CORE"]` |
| `primaryDeviceId` | ExperienceEvent.endUserIds - *endUserIds に存在する名前空間と最初の値のみ。* |
| `trait[]` | `ExperienceEvent.segmentMemberships["AAMTraits"]` |
| `segments[]` | `ExperienceEvent.segmentMemberships["AAMSegments"]` |
| `mergeRules[]` | `ExperienceEvent.profileStitch[]` |
| `timestamps` | `ExperienceEvent.timeStamp` |
| `deviceMetadata` | `ExperienceEvent.device` <ul><li>primaryHardwareType → type</li><li>manufacturer → manufacturer</li><li>marketingName → model</li><li>modelNumber → model</li></ul> |
| `location` | `ExperienceEvent.placeContext.geo` <ul><li>d_country → countryCode</li><li>d_state → stateProvince</li><li>d_city → city</li><li>d_postal → postalCode</li><li>d_lat → latitude</li><li>d_longitude → longitude</li></ul> |
| `request_user_agent` | `ExperienceEvent.environment.browserDetails` <ul><li>h_user-agent → userAgent</li><li>h_accept-language → acceptLanguage</li></ul> |
| `client_ip` | `ExperienceEvent.environment` <ul><li>d_os_name → os name </li><li>d_os_version → os_version</li></ul> |

{style="table-layout:auto"}

## プロファイルデータ

タイプ：プロファイル XDM

| プロファイルフィールド | XDM フィールド |
| --- | --- |
| `ids` | `identityMap` |
| `smem` | `ExperienceEvent.segmentMemberships["AAMSegments"]` |
| `tmem` | `ExperienceEvent.segmentMemberships["AAMTraits"]` |

{style="table-layout:auto"}
