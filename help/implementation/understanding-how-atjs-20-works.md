---
title: at.js 2.0 の仕組み
description: at.js 2.0 は、Adobe Targetのシングルページアプリケーション (SPA) のサポートを強化し、他のExperience Cloudソリューションと統合します。 このビデオと付属の図は、すべてがどのように連携しているかを説明しています。
role: Developer
level: Intermediate
topic: SPA, Architecture, Development
feature: Implementation
doc-type: technical video
kt: null
author: Daniel Wright
exl-id: 7f037665-88a7-469c-8df5-c82cb0f65382
source-git-commit: 342e02562b5296871638c1120114214df6115809
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 11%

---

# Adobe Targetの at.js 2.0 の仕組みについて

`at.js` 2.0 は、Adobe Targetのシングルページアプリケーション (SPA) のサポートを強化し、他のExperience Cloudソリューションと統合します。このビデオと付属の図は、すべてがどのように連携しているかを説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/26250?quality=12)

## アーキテクチャ図

![ページ読み込み時の at.js 2.0 の動作](assets/pageload.png)

1. 呼び出しはExperience CloudID(ECID) を返します。 ユーザーが認証されると、別の呼び出しが顧客 ID を同期します。

1. `at.js` ライブラリは、ドキュメント本文を同期的に読み込み、非表示にします (`at.js` ページに実装されているオプションの非表示スニペットを使用して非同期的に読み込むこともできます )。

1. ページ読み込みリクエストは、設定済みのすべてのパラメーター、ECID、SDID および顧客 ID を含めておこなわれます。

1. プロファイルスクリプトが実行され、[!UICONTROL  プロファイルストア ] に送られます。 ストアは、[!UICONTROL  オーディエンスライブラリ ] から正規のオーディエンスをリクエストします ( 例：[!DNL Analytics] から共有されたオーディエンス、Audience Managerなど )。 [!UICONTROL 顧客属] 性は、バッチ処理 [!UICONTROL でプ] ロファイルストアに送信されます。
1. [!DNL Target] は、URL、リクエストパラメーターおよびプロファイルデータに基づいて、現在のページおよび将来のビューで、どのアクティビティおよびエクスペリエンスを訪問者に返すかを決定します

1. （オプションで、追加のパーソナライゼーションに関するプロファイル値を含む）ページに送り返されたターゲットコンテンツ。

   デフォルトコンテンツがちらつくことなく、可能な限り迅速に現在のページ上のターゲットコンテンツが表示されます。

   単一ページアプリケーションの将来のビューに向けたターゲットコンテンツはブラウザーにキャッシュされるので、ビューがトリガーされる際に追加のサーバー呼び出しを必要とせずに、即座に適用できます。 （`triggerView()` の動作については、次の図を参照）。

1. [!DNL Analytics] ページからデータ収集サーバーに送信さ [!UICONTROL れる] データ
1. [!DNL Target] データは、SDID を使用して データに適合され、Analytics レポートストレージへと処理されます。[!DNL Analytics][!DNL Analytics] A4T レポートを使用して、との両 [!DNL Analytics] 方で [!DNL Target] データを表示できます。

![at.js 2.0 の動作 (triggerView() 関数が使用された場合 )](assets/triggerview.png)

1. `adobe.target.triggerView()` が単一ページアプリケーションで呼び出される
1. ビューのターゲットコンテンツがキャッシュから読み取られます

1. デフォルトコンテンツがちらつくことなく、可能な限り迅速にターゲットコンテンツが表示されます

1. 通知リクエストが [!DNL Target] [!UICONTROL  プロファイルストア ] に送信され、アクティビティで訪問者がカウントされ、指標が増分されます
1. [!DNL Analytics] データがSPAからデータ収集サーバーに送 [!UICONTROL 信さ] れる

1. [!DNL Target] データがバックエンドからデ [!DNL Target] ータ収集サーバー [!UICONTROL に送] 信されます。[!DNL Target] データは、SDID を使用して [!DNL Analytics] データに適合され、[!DNL Analytics] レポートストレージへと処理されます。[!DNL Analytics] A4T レポートを使用して、との両 [!DNL Analytics] 方で [!DNL Target] データを表示できます。

## その他のリソース

* [シングルページアプリケーションでの at.js 2.0 の実装](implement-atjs-20-in-a-single-page-application.md)
* [シングルページアプリケーション (SPA VEC) でのAdobe Targetの Visual Experience Composer の使用](../experiences/use-the-visual-experience-composer-for-single-page-applications.md)
* [at.js の仕組みドキュメント](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/at-js/how-atjs-works.html?lang=en)
