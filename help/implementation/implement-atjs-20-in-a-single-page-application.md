---
title: シングルページアプリケーション(SPA)でat.js 2.0を実装する方法
description: Adobe Targetのat.js 2.0は、次世代のクライアント側テクノロジーでパーソナライゼーションを実行するための機能セットを提供します。 シングルページアプリケーション(SPA)にat.js 2.0を実装するには、次の手順に従います。
role: Developer
level: Intermediate
topic: SPA, Architecture, Development
feature: Implementation
doc-type: technical video
kt: null
thumbnail: null
author: Daniel Wright
exl-id: 955f0571-5791-4dbb-9931-e6d5c8bb42a7
source-git-commit: a6b645b6d9693a4c8882fd47ee0d61698c0b834d
workflow-type: tm+mt
source-wordcount: '419'
ht-degree: 0%

---

# シングルページアプリケーション(SPA)でのat.js 2.0を使用したAdobe Targetの実装

Adobe Targetの`at.js` 2.0は、次世代のクライアント側テクノロジーでパーソナライゼーションを実行するための機能セットを提供します。 このバージョンでは、単一ページアプリケーション(SPA)と調和したインタラクションを実現するための`at.js`のアップグレードに焦点を当てています。

>[!VIDEO](https://video.tv.adobe.com/v/26248?quality=12)

## SPAでat.js 2.0を実装する方法

* シングルページアプリケーションの&lt;head>に`at.js` 2.0を実装します。
* SPAでビューが変更されるたびに、 `adobe.target.triggerView()`関数を実装します。 これをおこなうには、URLハッシュの変更のリッスン、SPAで実行されたカスタムイベントのリッスン、アプリケーションに直接`triggerView()`コードを埋め込むなど、様々な手法を使用できます。 特定の単一ページアプリケーションに最適なオプションを選択する必要があります。
* ビュー名は、`triggerView()`関数の最初のパラメーターです。 シンプルで明確な一意の名前を使用して、TargetのVisual Experience Composerで簡単に選択できるようにします。
* ビューのトリガーは、小さなビューの変更に加えて、SPA以外のコンテキスト（無限にスクロールするページの半分下など）でも変更できます。
* `at.js` 2.0および `triggerView()` は、Adobe Experience Platform Launchなどのタグ管理ソリューションを介して実装できます。

## at.js 2.0の制限

アップグレードする前に、`at.js` 2.0に関する次の制限事項に注意してください。

* `at.js` 2.0では、クロスドメイントラッキングはサポートされていません
* mboxOverride.browserIpおよびmboxSession URLパラメーターは、`at.js` 2.0ではサポートされていません
* `at.js` 2.0では、従来の関数mboxCreate、mboxDefine、mboxUpdateが廃止されました。デフォルトのコンテンツが表示され、ネットワークリクエストはおこなわれません。

## ビデオで使用されるライブラリのフッターコード

以下のコードは、ビデオの実行中に`at.js`ライブラリの「ライブラリフッター」セクションに追加されました。 アプリが最初に読み込まれ、次にアプリ内のハッシュの変更時に実行されます。 ビュー名にはハッシュのクリーンアップバージョンが使用され、ハッシュが空の場合は「home」が使用されます。 SPAを識別するために、コードはURL内で「react/」というテキストを探します。このテキストは、サイト上で更新する必要がある可能性が高くなります。 また、SPAでカスタムイベントの`triggerView()`を実行したり、コードを直接アプリに埋め込んだりする方が適している場合もあることに注意してください。

```javascript
function sanitizeViewName(viewName) {
  if (viewName.startsWith('#')) {
    viewName = viewName.substr(1);
  }
  if (viewName.startsWith('/')) {
    viewName = viewName.substr(1);
  }
  return viewName;
}
function triggerView(viewName) {
  viewName = sanitizeViewName(viewName) || 'home';
  // Validate if the Target Libraries are available on your website
  if (typeof adobe != 'undefined' && adobe.target && typeof adobe.target.triggerView === 'function') {
    adobe.target.triggerView(viewName);
    console.log('AT: View triggered on page load: '+viewName)
  }
}
//fire triggerView when the SPA loads and when the hash changes in the SPA
if(window.location.pathname.indexOf('react/') >-1){
    triggerView(location.hash);
}
window.onhashchange = function() {
    if(window.location.pathname.indexOf('react/') >-1){
        triggerView(location.hash);
    }
}
```

## その他のリソース

* [at.js 2.0の仕組みについて（アーキテクチャ図）](understanding-how-atjs-20-works.md)
* [シングルページアプリケーション(SPA VEC)でのAdobe TargetのVisual Experience Composerの使用](../experiences/use-the-visual-experience-composer-for-single-page-applications.md)
* [at.js 1.xからat.js 2.0ドキュメントへのアップグレード](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/upgrading-from-atjs-1x-to-atjs-20.html?lang=en)
