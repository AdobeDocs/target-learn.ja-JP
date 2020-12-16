---
title: Android向けAdobeMobile Services SDK v4搭載のAdobe Target
description: Android向けAdobeMobile Services SDK v4を搭載したAdobe Targetは、既にAdobeMobile Services SDK v4を使用し、Adobe Targetでのアプリエクスペリエンスのパーソナライズを開始したいAndroid開発者にとって、最適なスタートポイントです。
feature: mobile
kt: 3040
audience: developer
doc-type: tutorial
activity-type: implement
translation-type: tm+mt
source-git-commit: d6cedd55dcc9c08d2d2ca5709e15fe5ea9fab8b8
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 2%

---


# Android向けAdobeMobile Services SDK v4を使用したAdobe Target — 概要

_AdobeMobile Services SDK v4 for_ Androidを使用するAdobe Targetは、既にAdobeMobile Services SDK v4を使用しており、Adobe Targetでのアプリエクスペリエンスのパーソナライズを開始したいAndroid開発者にとって、最適なスタートポイントです。

レッスンを完了するためのデモAndroidアプリが用意されています。 このチュートリアルを完了したら、お使いのAndroidアプリで[!DNL Target]を実装する開始の準備が整います。

このチュートリアルでは、以下の内容について学習します。

* [AdobeMobile Services SDK](https://docs.adobe.com/content/help/en/mobile-services/android/getting-started-android/requirements.html)のセットアップを検証
* 次のタイプの[!DNL Target]リクエストを実装します。
   * [!DNL Target]コンテンツのプリフェッチ
   * 1つのリクエストで複数の[!DNL Target]場所(mbox)をバッチ処理
   * 要求のブロック（アプリの表示前に実行）
   * 非ブロック要求（バックグラウンドで実行）
   * リアルタイム（非キャッシュ）
   * キャッシュバスティングの再フェッチ
* パーソナライゼーションの追加強化を要求するパラメーター
* オーディエンスとオファーの作成
* レイアウトのパーソナライズ
* 機能に関するフラグを付けた新機能を公開する

## 前提条件

このレッスンでは、次の操作を行うと想定します。

* AdobeIDと承認者レベルでAdobe Targetインターフェイスにアクセスできる（以下の検証手順を参照）
* 自分のアカウントにリクエストできるように、Adobe Targetのクライアントコードを知っておいてください。 クライアントコードは、   設定/実装/at.js設定を編集画面
* [Mobile Servicesユーザーインターフェイス](https://mobilemarketing.adobe.com)にアクセスでき、使い慣れている
* Androidモバイルアプリの開発用IDEを持っている。 このチュートリアルでは、様々な手順とスクリーンショットで[Android Studio](https://developer.android.com/studio/install)を使用しています

必要なExperience Cloudソリューションへのアクセス権がない場合は、Experience Cloud管理者に問い合わせてください。

また、JavaのAndroid開発に精通していることを前提としています。 レッスンを完了するのにJavaのエキスパートである必要はありませんが、コードを問題なく読み、理解できれば、レッスンの内容をさらに詳しく理解できるはずです。

### Adobe Targetへのアクセスの確認

このレッスンでは、Adobe Targetへのアクセスが必要です。 次の手順に進む前に、次の手順を実行して、Adobe Targetにアクセスできることを確認します。

1. [Adobe Experience Cloud](https://experience.adobe.com/)にログインします。
1. Experience Cloudのホーム画面で、[!DNL Target]をクリックします。
   ![Experience Cloudホーム画面](assets/aec_homeScreen_clickTarget.png)
1. 下の図に示すように、Adobe Targetのアクティビティリストに移動する必要があります。ユーザーに承認者レベルのアクセス権があることを確認してください。 [!DNL Target]にアクセスできない場合、または承認者レベルのアクセスを確認できない場合は、会社のExperience Cloud管理者に連絡し、このアクセスを要求して、許可された後でこのチュートリアルを再開してください。

   ![AdobeUI](assets/targetUI_approver.png)

## レッスンについて

このレッスンでは、お客様のAdobe Targetアカウントを使用して、「We.Travel」と呼ばれるデモ旅行アプリにAdobe Targetを導入します。 チュートリアルを終えるまでに、アプリの使用状況に基づいて、パーソナライズされたメッセージをユーザーに配信します。 最終的なパーソナライゼーションエクスペリエンスは次のようになります。

![We.Travelアプリfinal](assets/overview_final_result.jpg)

Web.Travelアプリ内の実装を順を追って実行すると、独自のモバイルアプリ内で[!DNL Target]を使用して開始できるようになります。

始めよう！

**[次へ：「サンプルアプリのダウンロードと更新」>](download-and-update-the-sample-app.md)**
