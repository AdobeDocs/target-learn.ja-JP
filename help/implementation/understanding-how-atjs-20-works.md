---
title: at.js 2.0 の仕組み
description: at.js 2.0 は、Adobe Targetのシングルページアプリケーション (SPA) のサポートを強化し、他のExperience Cloudソリューションと統合します。 このビデオと付属の図は、すべてがどのように組み合わされるかを説明しています。
role: Developer
level: Intermediate
topic: SPA, Architecture, Development
feature: Implementation
doc-type: technical video
kt: null
author: Daniel Wright
exl-id: 7f037665-88a7-469c-8df5-c82cb0f65382
source-git-commit: 80208b3ecbc0d627d2afe72f882e91c9800d2726
workflow-type: tm+mt
source-wordcount: '412'
ht-degree: 12%

---

# Adobe Targetの at.js 2.0 の仕組みについて

`at.js` 2.0 は、Adobe Targetのシングルページアプリケーション (SPA) のサポートを強化し、他のExperience Cloudソリューションと統合します。 このビデオと付属の図は、すべてがどのように組み合わされるかを説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/26250?quality=12)

## アーキテクチャ図

![ページ読み込み時の at.js 2.0 の動作](assets/pageload.png)

1. 呼び出しによってExperience CloudID(ECID) が返されます。 ユーザーが認証されると、別の呼び出しが顧客 ID を同期します。

1. `at.js` ライブラリがドキュメント本文 (`at.js` は、ページに実装されているオプションの事前非表示スニペットを使用して非同期で読み込むこともできます )。

1. ページ読み込みリクエストは、設定済みのすべてのパラメーター、ECID、SDID および顧客 ID を含めておこなわれます。

1. プロファイルスクリプトを実行し、 [!UICONTROL プロファイルストア]. ストアは、 [!UICONTROL オーディエンスライブラリ] ( 例： [!DNL Analytics]、Audience Managerなど )。 [!UICONTROL 顧客属性] 送信先 [!UICONTROL プロファイルストア] バッチ・プロセスで使用します。
1. URL、リクエストパラメーターおよびプロファイルデータに基づいて、 [!DNL Target] は、現在のページおよび将来のビューでどのアクティビティおよびエクスペリエンスを訪問者に返すかを決定します

1. ターゲットコンテンツが（オプションで、追加のパーソナライゼーションに関するプロファイル値を含めて）ページに送り返されました。

   デフォルトコンテンツがちらつくことなく、可能な限り迅速に現在のページ上のターゲットコンテンツが表示されます。

   将来の単一ページアプリケーションのビューに向けたターゲットコンテンツはブラウザーにキャッシュされるので、ビューがトリガーされる際に追加のサーバー呼び出しを必要とせずに、即座に適用できます。 ( 次の図： `triggerView()` 動作 )。

1. [!DNL Analytics] ページから [!UICONTROL データ収集] サーバー
1. [!DNL Target] データは、SDID を使用して データに適合され、Analytics レポートストレージへと処理されます。[!DNL Analytics][!DNL Analytics] データは、 [!DNL Analytics] および [!DNL Target] A4T レポートを使用する。

![at.js 2.0 の動作 (triggerView() 関数が使用された場合 )](assets/triggerview.png)

1. `adobe.target.triggerView()` は、単一ページアプリケーションで呼び出されます。
1. ビューのターゲットコンテンツがキャッシュから読み取られます

1. デフォルトコンテンツがちらつくことなく、可能な限り迅速にターゲットコンテンツが表示されます

1. 通知リクエストが [!DNL Target] [!UICONTROL プロファイルストア] ：アクティビティで訪問者をカウントし、指標を増分します
1. [!DNL Analytics] データがSPAから [!UICONTROL データ収集] サーバー

1. [!DNL Target] データは [!DNL Target] ～のバックエンド [!UICONTROL データ収集] サーバー。 [!DNL Target] データは、SDID を使用して [!DNL Analytics] データに適合され、[!DNL Analytics] レポートストレージへと処理されます。[!DNL Analytics] データは、 [!DNL Analytics] および [!DNL Target] A4T レポートを使用する。

## その他のリソース

* [シングルページアプリケーションでの at.js 2.0 の実装](implement-atjs-20-in-a-single-page-application.md)
* [シングルページアプリケーション (SPA VEC) でのAdobe Targetの Visual Experience Composer の使用](../experiences/use-the-visual-experience-composer-for-single-page-applications.md)
