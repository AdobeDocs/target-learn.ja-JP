---
title: at.js 2.0の仕組み
description: at.js 2.0は、Adobe Targetのシングルページアプリ(SPA)に対するサポートを強化し、他のExperience Cloudソリューションと統合します。 このビデオと付属の図は、すべてがどのように連携するかを説明しています。
role: 開発者
level: 中間
topic: SPA，アーキテクチャ，開発
feature: 実装
doc-type: technical video
kt: null
thumbnail: null
author: Daniel Wright
translation-type: tm+mt
source-git-commit: b89732fcca0be8bffc6e580e4ae0e62df3c3655d
workflow-type: tm+mt
source-wordcount: '433'
ht-degree: 12%

---


# Adobe Targetのat.js 2.0の仕組み

`at.js` 2.0は、Adobe Targetのシングルページアプリケーション(SPA)のサポートを強化し、他のExperience Cloudソリューションと統合します。このビデオと付属の図は、すべてがどのように連携するかを説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/26250?quality=12)

## アーキテクチャ図

![at.js 2.0のページ読み込み時の動作](assets/pageload.png)

1. 呼び出しは、Experience CloudID(ECID)を返します。 ユーザーが認証されると、別の呼び出しによって顧客IDが同期されます。

1. `at.js` ライブラリの読み込みは、ドキュメント本体の同期的な読み込みと非表示の切り替えを行います(ページに実装されているオプションの非表示スニペットを使用して、非同期的に読み込むこ`at.js` ともできます)。

1. ページ読み込みリクエストは、設定済みのすべてのパラメーター、ECID、SDIDおよび顧客IDを含むように行われます。

1. プロファイルスクリプトを実行し、[!UICONTROL プロファイルストア]にフィードします。 ストアは、[!UICONTROL オーディエンスライブラリ]から正規オーディエンスを要求します(例：[!DNL Analytics]から共有されたオーディエンス、Audience Managerなど)。 [!UICONTROL 顧客] 属性は、バッチ処理で [!UICONTROL プロファイル] ストアに送信されます。
1. [!DNL Target]は、URL、リクエストパラメーター、プロファイルデータに基づいて、現在のページと今後の表示の訪問者に戻すアクティビティとエクスペリエンスを決定します

1. ターゲットコンテンツをページに戻します。オプションで、追加のパーソナライゼーションのプロファイル値も含まれます。

   デフォルトコンテンツがちらつくことなく、可能な限り迅速に現在のページ上のターゲットコンテンツが表示されます。

   将来の表示向けにターゲット設定されるシングルページアプリのコンテンツはブラウザーにキャッシュされるので、表示がトリガーされたときに追加のサーバー呼び出しを行うことなく、即座に適用できます。 （次の`triggerView()`動作の図を参照）。

1. [!DNL Analytics] ページから [!UICONTROL Data ] CollectionServersに送信されるデータ
1. [!DNL Target] データは、SDID を使用して データに適合され、Analytics レポートストレージへと処理されます。[!DNL Analytics][!DNL Analytics] データは、A4Tレポート [!DNL Analytics] とA4Tレポートの両方で表示 [!DNL Target] できます。

![at.js 2.0のtriggerView()関数が使用される場合の動作](assets/triggerview.png)

1. `adobe.target.triggerView()` がシングルページアプリケーションで呼び出される
1. 表示の対象コンテンツがキャッシュから読み取られます

1. デフォルトコンテンツがちらつくことなく、可能な限り迅速にターゲットコンテンツが表示されます

1. 通知リクエストが[!DNL Target] [!UICONTROL プロファイルストア]に送信され、アクティビティ内の訪問者がカウントされ、指標が増分されます
1. [!DNL Analytics] データがSPAから [!UICONTROL Data ] CollectionServersに送信される

1. [!DNL Target] データが [!DNL Target] バックエンドから [!UICONTROL Data ] CollectionServersに送信されます。[!DNL Target] データは、SDID を使用して [!DNL Analytics] データに適合され、[!DNL Analytics] レポートストレージへと処理されます。[!DNL Analytics] データは、A4Tレポート [!DNL Analytics] とA4Tレポートの両方で表示 [!DNL Target] できます。

## その他のリソース

* [at.js 2.0のシングルページアプリケーションへの実装](implement-atjs-20-in-a-single-page-application.md)
* [Adobe TargetのVisual Experience Composerを使用したシングルページアプリ(SPA VEC)](../experiences/use-the-visual-experience-composer-for-single-page-applications.md)
* [at.jsの仕組みに関するドキュメント](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/at-js/how-atjs-works.html)