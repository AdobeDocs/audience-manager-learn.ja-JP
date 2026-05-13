---
title: 類似モデルを活用して、1st パーティデータから完了済みの在庫を拡張します
description: このチュートリアルでは、類似モデルを設定して使用する手順を説明します。これにより、類似オーディエンスを新規作成し、コンバージョンセグメントの拡張機能として販売できます。
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 23523.jpg
kt: 1688
role: User, Developer, Admin, Leader
level: Intermediate
exl-id: 6820528e-3211-4a1d-be05-50f1292179d2
TQID: https://experienceleague.adobe.com/xFz82Q0MZ-ZyErTuOZPm66xUSe3uLbAGJ3xMiUBun8A
product_v2:
  - id: df80eeb1-8d72-467e-b0df-9d51c7d3a0a1
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: f8a45b24-4be7-4f1b-909b-60d06b483a20
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
source-git-commit: 3152e8fc51e0e06c90c17dce0aa203a27995e88d
workflow-type: tm+mt
source-wordcount: 891
ht-degree: 0%

---

# 類似モデルを活用して、1st パーティデータから完了済みの在庫を拡張します {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

このチュートリアルでは、類似オーディエンス [!UICONTROL Models]を設定して使用するための手順を説明します。これにより、類似オーディエンスを新しく作成し、コンバージョンセグメントの拡張機能として販売できます。

## ユースケースの詳細 {#use-case-details}

あなたはコンテンツパブリッシャーです。 サイトのコンバーターの在庫が既に売り切れている場合、商談はそこで終わると思うかもしれません。 AAMの類似[!UICONTROL Models]を入力します。 この機能を利用することで、売り切れた在庫をさらに拡大し、まだコンバージョンしていない可能性があるものの、コンバージョンした人のように見えたり、行動したりする人のオーディエンスも販売することができます。 このオーディエンスセグメントは、通常、実際のコンバーターよりも低い価格で販売されますが、サイトに広告を掲載したい広告主に追加のオーディエンスオプションを提供することで、売上を高めることができます。 このユースケースのさらに大きな利点は、1st パーティデータに対してこのモデルを実行するのに費用がかからないことです。

このチュートリアルの手順は次のとおりです。

1. 理想的なユーザー（コンバージョン）特性またはセグメントの特定/作成
1. このコンバージョン特性/セグメントをベース項目として使用してモデルを作成する
1. モデルで[!UICONTROL First party]個のデータソースを選択し、モデルを実行します
1. モデル結果から[!UICONTROL Algorithmic Trait]を作成し、特性をセグメントに追加します
1. 関心のある広告主にセグメントを提供し、コンバージョンセグメントの売上を拡大します

## 理想的なユーザー（コンバージョン）特性またはセグメントの特定または作成 {#identify-create-an-ideal-user-conversion-trait-or-segment}

サイトでのオーディエンスの行動を促進していますか？ コンバージョンイベントとは？ もちろん、サイトの種類や業種、組織の目標に応じて、この質問に対する答えはたくさんあります。 いずれにせよ、AAMでは、これらの条件を満たした訪問者の特性を作成するのが一般的です。

このユースケースでは、コンバーターである人の在庫を完売したので、これは既に想定されています。 ただし、このチュートリアルの目的では、このチュートリアルを他のユースケースのリファレンスとして説明することをお勧めします。

また、イベントを使用して特性を作成する場合は、特性に追加するユーザーを収集しないようにする必要がある大きな注意点があります。 次のビデオで大きな明らかをご覧ください。 :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**メモ：**&#x200B;上記のビデオでは、Adobe Analyticsを使用していることを前提としています。 明らかに、そんなことはありません。 Google Analytics（GA）をお持ちの場合は、AAMにデータを送信するために使用できるモジュールがあります（[&#x200B; ドキュメント &#x200B;](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=ja)を参照）。サイト上のコンバージョンアクティビティがGAまでにAAMに送信される場合は、そのモジュールからコンバージョン特性を作成できます。 別の分析ソリューションがある場合（または分析ソリューションがない場合）でも、DIL コードや`submit`関数などを介してAAMにデータを送信できます（[&#x200B; ドキュメント &#x200B;](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html?lang=ja)を参照）。 次に、コンバージョンアクティビティがサイトで実行されたときに送信されたデータに基づいて、コンバージョン特性を作成します。

## 1st パーティデータから類似モデルを作成する {#creating-a-look-alike-model-from-first-party-data}

このステップでは、[!UICONTROL First Party]の類似モデルを作成します。 つまり、ベースとなる特性やセグメントに1st パーティのコンバージョン特性やセグメントを使用するだけでなく（これはほとんどのモデルでは普通のことですが）、コンバーターのように見える多くの人のために、1st パーティデータのプールを調査することになります。 2nd パーティデータやサードパーティデータは扱いません。

このユースケースでは、これは重要です。なぜなら、アドビのサイトで、コンバーターのように見えても、まだコンバージョンに至っていないユーザーのセグメントを作成し、この類似セグメントを興味を持った広告主に販売しようとしているからです。

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## アルゴリズム特性の作成 {#creating-an-algorithmic-trait}

次に、[!UICONTROL Algorithmic Trait]を作成して、モデルの結果を使用できるようにする必要があります。 特性を作成しなければ、モデルは役に立ちません。 モデルの実行後、特性ダイアログに移動して[!UICONTROL Algorithmic Trait]を作成してください。 次のビデオでは、いくつかのヒントを紹介します。

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## 広告主に[!UICONTROL Algorithmic Segment]を提供する {#offering-the-algorithmic-segment-to-advertisers}

[!UICONTROL Algorithmic Trait]を作成したら、データをアクティブ化するために新しいセグメントを作成できます（特性をアクティブ化することはできませんが、[!UICONTROL Algorithmic Trait]を含む新しい1つの特性セグメントを作成して、セグメントをアクティブ化（使用）できます）。

類似モデルで高いスコアを獲得したファーストパーティ訪問者のセグメント（コンバーターのように見えますが、まだコンバージョンに至っていない訪問者）を作成したら、サイトで実際のコンバーターの在庫をすべて売り切れた後でも、このセグメントをサイトの広告主に提供できます。 これは、このオーディエンスを拡張し、Audience Managerで類似[!UICONTROL Models]を使用して引き続き売上を増加させる優れた方法です。
