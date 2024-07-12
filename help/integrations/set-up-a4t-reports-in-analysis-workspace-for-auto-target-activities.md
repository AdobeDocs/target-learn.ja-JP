---
title: ' [!DNL Analysis Workspace] for [!DNL Auto-Target] Activities で A4T レポートを設定する方法'
description: '[!UICONTROL Auto-Target] のアクティビティを実行した際に予期した結果が得られるように  [!DNL Analysis Workspace]  で A4T レポートを設定する方法を教えてください。'
badgePremium: label="Premium" type="Positive" url="https://experienceleague.adobe.com/docs/target/using/introduction/intro.html#premium newtab=true" tooltip="Target Premium に含まれる機能を確認してください。"
role: User
level: Intermediate
topic: Personalization, Integrations
feature: Analytics for Target (A4T), Auto-Target, Integrations
doc-type: tutorial
thumbnail: null
kt: null
exl-id: 58006a25-851e-43c8-b103-f143f72ee58d
source-git-commit: 78e5b5f7fa8f4c1a08c06c6d2b0e1a5242cd464c
workflow-type: tm+mt
source-wordcount: '2430'
ht-degree: 1%

---

# [!DNL Analysis Workspace] での [!DNL Auto-Target] アクティビティ用 A4T レポートの設定

>[!IMPORTANT]
>
>[!UICONTROL Auto-Target] アクティビティの場合は、[!DNL Analytics Workspace] でレポートを確認し、A4T パネルを手動で作成する必要があります。

[!DNL Auto-Target] アクティビティの [!UICONTROL Analytics for Target] （A4T）統合は、[!DNL Adobe Analytics] の目標指標を使用しながら、[!DNL Adobe Target] アンサンブル機械学習（ML）アルゴリズムを使用して、プロファイル、動作、コンテキストに基づいて、各訪問者に最適なエクスペリエンスを選択します。

[!DNL Adobe Analytics] [!DNL Analysis Workspace] には豊富な分析機能が備わっていますが、実験アクティビティ（手動の [!UICONTROL A/B Test] と [!UICONTROL Auto-Allocate]）とパーソナライゼーションアクティビティ（[!UICONTROL [!UICONTROL Auto-Target]]）には違いがあるので、[!DNL Auto-Target] アクティビティを正しく解釈するには、デフォルトの **[!UICONTROL Analytics for Target]** パネルにいくつかの変更を加える必要があります。

このチュートリアルでは、次の主要な概念に基づいて、[!DNL Analysis Workspace] での [!UICONTROL Auto-Target] アクティビティ分析に推奨される変更について説明します。

* **[!UICONTROL Control vs Targeted]** ディメンションを使用して、[!UICONTROL Control] エクスペリエンスと [!UICONTROL Auto-Target] アンサンブル ML アルゴリズムで提供されるエクスペリエンスを区別できます。
* 訪問回数は、エクスペリエンスレベルのパフォーマンスの分類を表示する際の正規化指標として使用する必要があります。 また、[Adobe Analyticsのデフォルトのカウント方法には、ユーザーが実際にはアクティビティコンテンツを表示しない訪問が含まれる場合がありますが ](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-viewing-reports.html#metrics){target=_blank} このデフォルトの動作は、適切にスコープが設定されたセグメント（詳細は以下）を使用することで変更できます。
* 訪問ルックバックスコープ型アトリビューションは、所定のアトリビューションモデルの「訪問ルックバックウィンドウ」とも呼ばれ、[!DNL Adobe Target] ML モデルによってトレーニングフェーズで使用され、目標指標を分割する場合は、同じ（デフォルト以外の）アトリビューションモデルを使用する必要があります。

## [!DNL Analysis Workspace] での [!UICONTROL Auto-Target] パネル用の A4T の作成

[!UICONTROL Auto-Target] のレポートの A4T を作成するには、以下に示すように、[!DNL Analysis Workspace] の **[!UICONTROL Analytics for Target]** パネルから開始するか、フリーフォームテーブルから開始します。 次に、以下の選択を行います。

1. **[!UICONTROL Control Experience]**：任意のエクスペリエンスを選択できます。ただし、この選択は後で上書きされます。 [!UICONTROL Auto-Target] のアクティビティの場合、制御エクスペリエンスは実際には制御戦略であることに注意してください。これは、a）すべてのエクスペリエンスの中からランダムに提供されるか、b）単一のエクスペリエンスを提供します（この選択は、[!DNL Adobe Target] のアクティビティ作成時におこないます）。 選択肢（b）を選択した場合でも、[!UICONTROL Auto-Target] アクティビティは特定のエクスペリエンスをコントロールとして指定しました。 [!UICONTROL Auto-Target] アクティビティに関する A4T の分析については、このチュートリアルで概要を説明しているアプローチに従う必要があります。
2. **[!UICONTROL Normalizing Metric]**: 「[!UICONTROL Visits]」を選択します。
3. **[!UICONTROL Success Metrics]**: レポート対象の指標を選択できますが、通常は [!DNL Target] でのアクティビティ作成時に最適化対象として選択した指標と同じ指標に関するレポートを表示する必要があります。

   [!UICONTROL Auto-Target] アクティビティの ![[!UICONTROL Analytics for Target] パネルの設定。](assets/Figure1.png)

   *図 1:[!UICONTROL Auto-Target] アクティビティの [!UICONTROL Analytics for Target] パネルの設定*

>[!TIP]
>
>[!UICONTROL Auto-Target] のアクティビティに対して [!UICONTROL Analytics for Target] ントロールパネルを設定するには、任意のコントロールエクスペリエンスを選択し、正規化指標として [!UICONTROL Visits] を選択し、アクティビティの作成時に最適化のために選択した同じ目標指標 [!DNL Target] 選択します。

## [!UICONTROL Control vs.Targeted] ディメンションを使用して、[!DNL Target] アンサンブル ML モデルをコントロールと比較します

デフォルトの A4T パネルは、個々のエクスペリエンスのパフォーマンスをコントロー [!UICONTROL A/B Test] エクスペリエンスと比較することを目的とした、クラシック（手動）のエクスペリエンスまたは [!UICONTROL Auto-Allocate] アクティビティ用に設計されています。 しかし、[!UICONTROL Auto-Target] のアクティビティでは、コントロール *戦略* とターゲット *戦略* の間の一次比較が必要です。 つまり、[!UICONTROL Auto-Target] ンサンブル ML モデルの制御戦略に対する全体的なパフォーマンスの上昇率を決定します。

この比較を実行するには、**[!UICONTROL Control vs Targeted (Analytics for Target)]** ディメンションを使用します。 ドラッグ&amp;ドロップして、デフォルトの A4T レポートの **[!UICONTROL Target Experiences]** ディメンションを置き換えます。

この置換により、A4T パネルのデフォルトの [!UICONTROL Lift and Confidence] 計算が無効になることに注意してください。 混乱を避けるために、次のレポートを残して、これらの指標をデフォルトパネルから削除できます。

[!DNL Analysis Workspace]](assets/Figure2.png) の ![[!UICONTROL Experiences by Activity Conversions] パネル

*図 2:[!DNL Auto-Target] アクティビティに対して推奨されるベースラインレポート。 このレポートは、（アンサンブル ML モデルによって提供される）ターゲットトラフィックを制御トラフィックと比較するように設定されています。*

>[!NOTE]
>
>現在、[!UICONTROL Auto-Target] の A4T レポートでは、[!UICONTROL Control vs Targeted] ディメンションで [!UICONTROL Lift and Confidence] 数を使用できません。 サポートが追加されるまでは、[ 信頼性計算ツール ](https://experienceleague.adobe.com/docs/target/assets/complete_confidence_calculator.xlsx) をダウンロードして手動で計算で [!UICONTROL Lift and Confidence] ます。

## 指標のエクスペリエンスレベルの分類の追加

アンサンブル ML モデルのパフォーマンスをさらに詳しく理解するには、**[!UICONTROL Control vs Targeted]** ディメンションのエクスペリエンスレベルの分類を調べることができます。 [!DNL Analysis Workspace] では、**[!UICONTROL Target Experiences]** ディメンションをレポートにドラッグしてから、コントロールおよびターゲットの各ディメンションを別々に分類します。

[!DNL Analysis Workspace]](assets/Figure3.png) の ![[!UICONTROL Experiences by Activity Conversions] パネル

*図 3：ターゲットエクスペリエンスによるターゲットディメンションの分類*

結果のレポートの例を次に示します。

[!DNL Analysis Workspace]](assets/Figure4.png) の ![[!UICONTROL Experiences by Activity Conversions] パネル

*図 4：エクスペリエンスレベルの分類を含む標準 [!UICONTROL Auto-Target] レポート。 目標指標は異なる場合があり、制御戦略は 1 つのエクスペリエンスである可能性があることに注意してください。*

>[!TIP]
>
>[!DNL Analysis Workspace] では、歯車アイコンをクリックして「[!UICONTROL Conversion Rate]」列でパーセンテージを非表示にし、エクスペリエンスのコンバージョン率に集中できるようにします。 その後、コンバージョン率は小数として書式設定されますが、それに応じてパーセンテージとして解釈されます。

## 「[!UICONTROL Visits]」が [!UICONTROL Auto-Target] アクティビティの正しい正規化指標である理由

[!UICONTROL Auto-Target] アクティビティを分析する場合、デフォルトの正規化指標として常に [!UICONTROL Visits] を選択します。 [!UICONTROL Auto-Target] のパーソナライゼーションでは、訪問者のエクスペリエンスが訪問ごとに 1 回（公式には、[!DNL Target] セッションごとに 1 回）選択されます。つまり、訪問者に表示されるエクスペリエンスは、1 回の訪問ごとに変化します。 したがって、[!UICONTROL Unique Visitors] を正規化指標として使用すると、1 人のユーザーに（異なる訪問をまたいで）複数のエクスペリエンスが表示される可能性があるという事実により、コンバージョン率が混乱する可能性があります。

この点を簡単な例で示します。2 人の訪問者が、エクスペリエンスが 2 つしかないキャンペーンにエントリするシナリオを考えてみましょう。 最初の訪問者が 2 回訪問する。 訪問者は最初の訪問ではエクスペリエンス A に割り当てられ、2 回目の訪問ではエクスペリエンス B に割り当てられます（2 回目の訪問でプロファイルの状態が変更されるため）。 2 回目の訪問の後、訪問者は注文を行うことでコンバージョンを行います。 コンバージョンは、最近表示されたエクスペリエンス（エクスペリエンス B）に関連付けられます。 2 人目の訪問者も 2 回訪問し、エクスペリエンス B の両方が表示されますが、コンバージョンは行われません。

訪問者レベルのレポートと訪問レベルのレポートを比較しましょう。

| エクスペリエンス | ユニーク訪問者 | 訪問 | コンバージョン数 | 訪問者が正規化したコンバージョン率 | 訪問で正規化されたコンバージョン率 |
| --- | --- | --- | --- | --- | --- |
| A | 1 | 1 | - | 0% | 0% |
| B | 2 | 3 | 1 | 50% | 33.3% |
| 合計 | 2 | 4 | 1 | 50% | 25％ |

*表 1：通常の A/B テストのように訪問に関する決定がスティッキー（訪問者ではない）となるシナリオの訪問者の正規化されたレポートと訪問の正規化されたレポートの比較例。 このシナリオでは、訪問者を正規化した指標は混乱を招きます。*

表に示すように、訪問者レベルの数に明確な不一致があります。 合計 2 人のユニーク訪問者がいるにもかかわらず、これは、各エクスペリエンスへの個別のユニーク訪問者の合計ではありません。 訪問者レベルのコンバージョン率は必ずしも間違いではありませんが、個々のエクスペリエンスを比較する場合、訪問レベルのコンバージョン率の方がはるかに理にかなっていると言えます。 正式には、分析単位（「訪問回数」）は決定のスティッキネスの単位と同じです。つまり、指標のエクスペリエンスレベルの分類を追加して比較できます。

## アクティビティへの実際の訪問をフィルタリングする

[!DNL Target] アクティビティへの訪問に対する [!DNL Adobe Analytics] のデフォルトのカウント方法には、ユーザーが [!DNL Target] アクティビティとやり取りしなかった訪問が含まれる場合があります。 これは、アクティビティ [!DNL Target] 割り当てが [!DNL Analytics] の訪問者コンテキスト内に保持されるためです。 その結果、[!DNL Target] アクティビティへの訪問回数が増え、コンバージョン率が低下する場合があります。

ユーザーが実際に [!UICONTROL Auto-Target] のアクティビティを操作した訪問をレポートする場合（アクティビティのエントリ、ディスプレイまたは訪問イベント、コンバージョンのいずれか）、次のことができます。

1. 該当する [!DNL Target] アクティビティからのヒットを含む特定のセグメントを作成し、次に
1. このセグメントを使用して [!UICONTROL Visits] 指標をフィルタリングします。

**セグメントを作成するには：**

1. [!DNL Analysis Workspace] ツールバーの「**[!UICONTROL Components > Create Segment]**」オプションを選択します。
2. セグメントの **[!UICONTROL Title]** を指定します。 次の例では、セグメントの名前は [!DNL "Hit with specific Auto-Target activity"] です。
3. **[!UICONTROL Target Activities]** のディメンションをセグメント **[!UICONTROL Definition]** セクションにドラッグします。
4. **[!UICONTROL equals]** 演算子を使用します。
5. 特定の [!DNL Target] アクティビティを検索します。
6. 歯車アイコンをクリックし、「**[!UICONTROL Attribution model > Instance]**」を選択します（下図を参照）。
7. **[!UICONTROL Save]** をクリックします。

![[!DNL Analysis Workspace]](assets/Figure5.png) のセグメント

*図 5：ここに示すようなセグメントを使用して、レポート用に A4T の [!UICONTROL Visits] 指標をフィルタリング [!UICONTROL Auto-Target] ます*

セグメントを作成したら、そのセグメントを使用して [!UICONTROL Visits] 指標をフィルタリングし、ユーザーが [!DNL Target] アクティビティとインタラクションを行った訪問のみを [!UICONTROL Visits] 指標に含めるようにします。

**このセグメントを使用して [!UICONTROL Visits] をフィルタリングするには：**

1. コンポーネントツールバーから新しく作成したセグメントをドラッグし、青い **[!UICONTROL Filter by]** プロンプトが表示されるまで **[!UICONTROL Visits]** 指標ラベルのベース上にマウスポインターを置きます。
2. セグメントを解放します。 その指標にフィルターが適用されます。

最終的なパネルは次のように表示されます。

[!DNL Analysis Workspace]](assets/Figure6.png) の ![[!UICONTROL Experiences by Activity Conversions] パネル

*図 6: [!UICONTROL Visits] 指標に適用された「特定の自動ターゲットアクティビティでヒット」セグメントを含むレポートパネル。 このセグメントにより、ユーザーが実際に当該の [!DNL Target] アクティビティを操作した訪問のみが、レポートに含まれます。*

## 目標指標とアトリビューションが最適化条件に関連付けられていることを確認します

A4T 統合により、[!DNL Adobe Analytics] が *パフォーマンスレポートの生成* に使用するのと同じコンバージョンイベントデータを使用して *[!UICONTROL Auto-Target] ML モデルをトレーニング* できます。 ただし、ML モデルのトレーニング時にこのデータを解釈する際に採用する必要がある前提があります。これは、[!DNL Adobe Analytics] のレポートフェーズで行われたデフォルトの前提とは異なります。

特に、[!DNL Adobe Target] ML モデルでは、訪問スコープのアトリビューションモデルを使用します。 つまり、ML モデルでは、ML モデルによる決定にコンバージョンが「属性」とされるためには、アクティビティのコンテンツが表示されるのと同じ訪問でコンバージョンが行われる必要があると想定しています。 これは、[!DNL Target] がそのモデルのタイムリーなトレーニングを保証するために必要 [!DNL Target] す。コンバージョン（[!DNL Adobe Analytics] のレポートのデフォルトのアトリビューションウィンドウ）の最大 30 日を待ってから、そのモデルのトレーニングデータに含めることはできません。

したがって、[!DNL Target] モデルで（トレーニング中に）使用されるアトリビューションと（レポート生成中に）データのクエリに使用されるデフォルトアトリビューションの違いにより、不一致が生じる可能性があります。 実際には問題がアトリビューションにある場合は、ML モデルのパフォーマンスが低下しているように見えることさえあります。

>[!TIP]
>
>ML モデルが、レポートで表示している指標とは異なる属性を持つ指標を最適化している場合、モデルは期待どおりに動作しない可能性があります。 これを回避するには、レポートの目標指標で、[!DNL Target] ML モデルで使用されるのと同じ指標定義およびアトリビューションを使用していることを確認してください。

正確な指標の定義とアトリビューションの設定は、アクティビティの作成時に指定した [ 最適化条件 ](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-at-aa.html?lang=ja#supported){target=_blank} によって異なります。

### Target で定義されたコンバージョン、または *訪問あたりの指標値の最大化* を使用した指標の [!DNL Analytics] 定

指標が [!DNL Target] コンバージョンの場合、または **訪問あたりの指標値の最大化** を持つ [!DNL Analytics] 指標の場合、目標指標定義を使用すると、同じ訪問で複数のコンバージョンイベントを発生させることができます。

[!DNL Target] ML モデルで使用されるのと同じアトリビューション手法を持つ目標指標を表示するには、次の手順に従います。

1. 目標指標の歯車アイコンの上にマウスポインターを置きます。

   ![gearicon.png](assets/gearicon.png)

1. 表示されたメニューから、**[!UICONTROL Data settings]** までスクロールします。
1. 「**[!UICONTROL Use non-default  attribution model]**」を選択します（選択されていない場合）。

   ![non-defaultattributionmodel.png](assets/non-defaultattributionmodel.png)

1. **[!UICONTROL Edit]** をクリックします。
1. 「**[!UICONTROL Model]**: **[!UICONTROL Participation]**」と「**[!UICONTROL Lookback window]**: **[!UICONTROL Visit]**」を選択します。

   ![ParticipationbyVisit.png](assets/ParticipationbyVisit.png)

1. **[!UICONTROL Apply]** をクリックします。

これらの手順により、エクスペリエンスが表示されたのと同じ訪問で目標指標イベントが *いつでも* （「パーティシペーション」）発生した場合、レポートで目標指標がエクスペリエンスの表示に関連付けられることが保証されます。

### *ユニーク訪問のコンバージョン率* を使用した指標のカスタマイ [!DNL Analytics]

**ポジティブ指標セグメントを使用して訪問を定義**

最適化条件として *ユニーク訪問コンバージョン率の最大化* を選択したシナリオでは、コンバージョン率の正しい定義は指標値が肯定的となる訪問回数の割合です。 これを実現するには、指標の正の値を持つ訪問をフィルタリングするセグメントを作成し、次に訪問回数指標をフィルタリングします。

1. 前と同様に、[!DNL Analysis Workspace] のツールバーの「**[!UICONTROL Components > Create Segment]**」オプションを選択します。
2. セグメントの **[!UICONTROL Title]** を指定します。

   次の例では、セグメントの名前は [!DNL "Visits with an order"] です。

3. 最適化目標で使用したベース指標をセグメントにドラッグします。

   次の例では、**orders** 指標を使用し、コンバージョン率で注文が記録された訪問の割合を測定します。

4. セグメント定義コンテナの左上で、「**[!UICONTROL Include]** クセス **を選択** ます。
5. **[!UICONTROL is greater than]** 演算子を使用して、値を 0 に設定します。

   値を 0 に設定すると、このセグメントには注文指標が肯定的な訪問が含まれます。

6. **[!UICONTROL Save]** をクリックします。

![Figure7.png](assets/Figure7.png)

*図 7：正の順序を持つ訪問に対するセグメント定義フィルタリング アクティビティの最適化指標に応じて、注文を適切な指標に置き換える必要があります*

**これをアクティビティフィルター適用指標の訪問回数に適用**

このセグメントを使用して、注文数が正の訪問と、[!DNL Auto-Target] アクティビティのヒットがあった訪問をフィルタリングできるようになりました。 指標のフィルタリング手順は以前と同様で、既にフィルタリングされた訪問指標に新しいセグメントを適用すると、レポートパネルは図 8 のようになります

![Figure8.png](assets/Figure8.png)

*図 8：正しいユニーク訪問のコンバージョン指標を含むレポートパネル：アクティビティからのヒットが記録された訪問回数と、コンバージョン指標（この例では注文数）が 0 以外の訪問数。*

## 最後の手順：上記の魔法をキャプチャするコンバージョン率を作成します

前の節で [!UICONTROL Visit] と目標の指標に対して行った変更を踏まえ、レポートパネルのデフォルト A4T に対して行う必要のあ [!DNL Auto-Target] 最終変更は、適切にフィルタリングされた「訪問回数」指標に対する正しい比率（修正された目標指標のコンバージョン率）であるコンバージョン率を作成することです。

これを行うには、次の手順を使用して [!UICONTROL Calculated Metric] を作成します。

1. [!DNL Analysis Workspace] ツールバーの「**[!UICONTROL Components > Create Metric]**」オプションを選択します。
1. 指標の **[!UICONTROL Title]** を指定します。 例えば、「アクティビティ XXX の訪問補正コンバージョン率」などです。
1. **[!UICONTROL Format]** = Percent および **[!UICONTROL Decimal Places]** = 2 を選択します。
1. アクティビティに関連する目標指標（例：[!UICONTROL Activity Conversions]）を定義にドラッグし、前述のように、この目標指標の歯車アイコンを使用して、アトリビューションモデルを（パーティシペーション|訪問）に調整します。
1. **[!UICONTROL Definition]** セクションの右上から「**[!UICONTROL Add > Container]**」を選択します。
1. 2 つのコンテナ間の除算（÷）演算子を選択します。
1. 以前に作成したセグメント（このチュートリアルの「特定の [!UICONTROL Auto-Target] アクティビティを使用したヒット」という名前）を、この特定の [!DNL Auto-Target] アクティビティにドラッグします。
1. **[!UICONTROL Visits]** 指標をセグメントコンテナにドラッグします。
1. **[!UICONTROL Save]** をクリックします。

>[!TIP]
>
> また、[ クイック計算指標機能 ](https://experienceleague.adobe.com/docs/analytics-learn/tutorials/components/calculated-metrics/quick-calculated-metrics-in-analysis-workspace.html?lang=ja) を使用して、この指標を作成することもできます。

完全な計算指標の定義はここに表示されます。

![Figure9.png](assets/Figure9.png)

*図 7：訪問訂正済みおよびアトリビューション訂正済みのモデルコンバージョン率指標の定義。 （この指標は、目標指標とアクティビティに依存することに注意してください。 つまり、この指標の定義は、アクティビティをまたいで再利用することはできません）*

>[!IMPORTANT]
>
>A4T パネルのコンバージ [!UICONTROL Conversion] ンレート指標が、テーブルのコンバージョンイベントまたは標準化指標にリンクされていない。 このチュートリアルで提案された変更を行っても、[!UICONTROL Conversion] 率は変更に自動的に適応しません。 したがって、コンバージョンイベントのアトリビューションまたは正規化指標（またはその両方）を変更する場合は、最後の手順として、上記のようにコンバージ [!UICONTROL Conversion] ンレートも変更する必要があります。

## 概要：[!UICONTROL Auto-Target] レポートの最終的なサンプル [!DNL Analysis Workspace] パネル

上記のすべての手順を 1 つのパネルにまとめたもので、次の図は、[!UICONTROL Auto-Target] の A4T アクティビティに推奨されるレポートの全体像を示しています。 このレポートは、[!DNL Target] ML モデルで目標指標を最適化するために使用されるレポートと同じです。 このレポートには、このチュートリアルで説明したすべてのニュアンスと推奨事項が含まれています。 また、このレポートは、従来のレポート作成主導の [!UICONTROL Auto-Target] アクティビティで使用され [!DNL Target] カウント方法にも最も近い結果を示しています。

クリックして画像を展開します。

![Analysis Workspaceでの [!DNL Analysis Workspace]](assets/Figure10.png "A4T レポートの最終的な A4T レポート "){width="600" zoomable="yes"}

*図 10：このチュートリアルの前の節で説明した指標の定義に対するすべての調整を組み合わせた、[!DNL Adobe Analytics] [!DNL Workspace] の最終的な A4T [!UICONTROL Auto-Target] レポート*
