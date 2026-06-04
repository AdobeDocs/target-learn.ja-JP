---
title: シングルページアプリケーション（SPA）でのat.js 2.0の実装方法
description: Adobe Target at.js 2.0は、次世代のクライアントサイド技術でパーソナライゼーションを実行するための豊富な機能セットを提供します。 シングルページアプリケーション（SPA）にat.js 2.0を実装するには、次の手順に従います。
role: Developer
level: Intermediate
topic: SPA, Architecture, Development
feature: Implementation
doc-type: technical video
kt: null
author: Daniel Wright
exl-id: 955f0571-5791-4dbb-9931-e6d5c8bb42a7
TQID: https://experienceleague.adobe.com/eGA92lV-FAhNnjeKc-Vceh1DrDgWBUKqDkVHzzTQ5Nk
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: c0b4abf2d4ead4d58a8db6e8970857b7b50dbe5c
workflow-type: tm+mt
source-wordcount: 420
ht-degree: 0%

---

# Adobe Target at.js 2.0をシングルページアプリケーション（SPA）に実装する

Adobe Targetの`at.js` 2.0には、次世代のクライアントサイドのテクノロジーでパーソナライゼーションを実行するための豊富な機能セットが用意されています。 このバージョンは、`at.js`をアップグレードして、シングルページアプリケーション（SPA）との調和のとれたインタラクションを実現することに重点を置いています。

>[!VIDEO](https://video.tv.adobe.com/v/26248?quality=12)

## SPAでのat.js 2.0の実装方法

* シングル ページ アプリケーションの&lt;head>に`at.js` 2.0を実装します。
* SPAでビューが変更されるたびに`adobe.target.triggerView()`関数を実装します。 これを行うには、URL ハッシュの変更をリッスンする、SPAによって実行されるカスタムイベントをリッスンする、`triggerView()` コードをアプリケーションに直接埋め込むなど、さまざまなテクニックを使用できます。 特定のシングルページアプリケーションに最適なオプションを選択する必要があります。
* ビュー名は、`triggerView()`関数の最初のパラメーターです。 シンプルで明確、かつ一意の名前を使用することで、TargetのVisual Experience Composerで簡単に選択できます。
* ビューは、小さなビューの変更時だけでなく、SPA以外のコンテキスト（半方向下や無限スクロール ページなど）でもトリガーできます。
* `at.js` 2.0と`triggerView()`は、Adobe Experience Platform Launchなどのタグ管理ソリューションを介して実装できます。

## at.js 2.0の制限

アップグレードする前に、`at.js` 2.0の次の制限に注意してください。

* `at.js` 2.0では、クロスドメインの追跡はサポートされていません
* mboxOverride.browserIpおよびmboxSession URL パラメーターは、`at.js` 2.0ではサポートされていません
* レガシー関数mboxCreate、mboxDefine、mboxUpdateは`at.js` 2.0で非推奨（廃止予定）です。 デフォルトのコンテンツが表示され、ネットワークリクエストは行われません。

## ビデオで使用されるライブラリフッターコード

以下のコードは、ビデオ中に`at.js` ライブラリの「ライブラリフッター」セクションに追加されました。 アプリが最初に読み込まれてから、アプリ内のハッシュ変更で実行されます。 ビュー名としてハッシュのクリーンアップ版を使用し、ハッシュが空の場合は「home」を使用します。 SPAを識別するために、コードはURL内のテキスト「react/」を探します。これは、サイトで更新する必要がある可能性が最も高くなります。 また、SPAでカスタムイベントから`triggerView()`を削除するか、コードをアプリに直接埋め込む方が適切な場合があることに注意してください。

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

* [at.js 2.0の仕組みを理解する（アーキテクチャ図） ](understanding-how-atjs-20-works.md)
* [Adobe TargetのVisual Experience Composer for Single Page Applications （SPA VEC）の使用](../experiences/use-the-visual-experience-composer-for-single-page-applications.md)
