---
title: Adobe Targetのat.js 2.0の仕組み
seo-title: Adobe Targetのat.js 2.0の仕組み
description: at.js 2.0は、シングルページアプリ(SPA)に対するAdobe Targetのサポートを強化し、他のExperience Cloudソリューションと統合します。 このビデオと付属の図は、すべてがどのように連携するかを説明しています。
audience: developer
difficulty: 3
author: Daniel Wright
doc-type: implement
activity-type: technical-video
translation-type: tm+mt
source-git-commit: 37443ae4c1cdda387c8db0053201d520fa1ec224
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 11%

---


# Adobe Targetのat.js 2.0の仕組み

`at.js` 2.0は、シングルページアプリ(SPA)に対するAdobe Targetのサポートを強化し、他のExperience Cloudソリューションと統合します。 このビデオと付属の図は、すべてがどのように連携するかを説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/26250?quality=12)

## アーキテクチャ図

![at.js 2.0のページ読み込み時の動作](assets/pageload.png)

1. 呼び出しは、Experience CloudID(ECID)を返します。 ユーザーが認証されると、別の呼び出しによって顧客IDが同期されます。

1. `at.js` ライブラリの読み込みは、ドキュメント本文を同期的に行うか、非同期的に行うかを指定します（ページに実装されているオプションの非表示前のスニペットとは異なる方法で読み込むこともできます）。`at.js`

1. ページ読み込みリクエストは、設定済みのすべてのパラメーター、ECID、SDIDおよび顧客IDを含むように行われます。

1. Profile scripts execute and feed into the [!UICONTROL Profile Store]. The Store requests qualified audiences from the [!UICONTROL Audience Library] (e.g. audiences shared from [!DNL Analytics], Audience Manager, etc). [!UICONTROL 顧客属性] は、バッチ処理で [!UICONTROL プロファイルストア] に送信されます。
1. Based on URL, request parameters, and profile data, [!DNL Target] decides which Activities and Experiences to return to the visitor for the current page and future views

1. ターゲットコンテンツをページに戻します。オプションで、追加のパーソナライゼーションのプロファイル値も含まれます。

   デフォルトコンテンツがちらつくことなく、可能な限り迅速に現在のページ上のターゲットコンテンツが表示されます。

   将来の表示向けにターゲット設定されるシングルページアプリのコンテンツはブラウザーにキャッシュされるので、表示がトリガーされたときに追加のサーバー呼び出しを行うことなく、即座に適用できます。 (動 `triggerView()` 作については、次の図を参照)。

1. [!DNL Analytics] ページから [!UICONTROL データ収集] サーバーに送信されるデータ
1. [!DNL Target] データは、SDID を使用して データに適合され、Analytics レポートストレージへと処理されます。[!DNL Analytics][!DNL Analytics] データは、A4TレポートとA4Tレポートの両方で表示 [!DNL Analytics] で [!DNL Target] きます。

![at.js 2.0のtriggerView()関数が使用される場合の動作](assets/triggerview.png)

1. `adobe.target.triggerView()` がシングルページアプリケーションで呼び出される
1. 表示の対象コンテンツがキャッシュから読み取られます

1. デフォルトコンテンツがちらつくことなく、可能な限り迅速にターゲットコンテンツが表示されます

1. Notification request is sent to the [!DNL Target] [!UICONTROL Profile Store] to count the visitor in the activity and increment metrics
1. [!DNL Analytics] データがSPAから [!UICONTROL データ収集] サーバーに送信される

1. [!DNL Target] データがバックエンド [!DNL Target] から [!UICONTROL データ収集] サーバーに送信されます。 [!DNL Target] データは、SDID を使用して [!DNL Analytics] データに適合され、[!DNL Analytics] レポートストレージへと処理されます。[!DNL Analytics] データは、A4TレポートとA4Tレポートの両方で表示 [!DNL Analytics] で [!DNL Target] きます。

## その他のリソース

* [at.js 2.0のシングルページアプリケーションへの実装](implement-atjs-20-in-a-single-page-application.md)
* [Adobe TargetのVisual Experience Composerをシングルページアプリケーションで使用(SPA VEC)](../experiences/use-the-visual-experience-composer-for-single-page-applications.md)
* [at.jsの仕組みに関するドキュメント](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/at-js/how-atjs-works.html)