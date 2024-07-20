---
title: IAB TCF 2.0 のサポート
description: IAB TCF へのAudience Managerプラグインと、Adobeのオプトインオブジェクトおよび Consent Management Provider （CMP）との連携方法について説明します。
feature: Data Governance & Privacy
activity: implement
doc-type: technical video
team: Technical Marketing
thumbnail: 26434.jpg
kt: 5027
role: Developer, Data Engineer, Architect
level: Experienced
exl-id: 04b4e786-0457-4dcc-bcf9-a79eda67bb2e
source-git-commit: 62b43b5627dabf754cf821f974a56c60989ef7ef
workflow-type: tm+mt
source-wordcount: '1059'
ht-degree: 0%

---

# Audience Managerでの IAB TCF 2.0 のサポート {#iab-tcf-support-in-audience-manager}

Adobeでは、オプトイン機能と、IAB Transparency and Consent Framework 2.0 （TCF 2.0）サポートへのAudience Managerプラグインを通じて、ユーザーのプライバシーの選択を管理および伝達する手段を提供します。 この記事は IAB TCF へのAudience Managerプラグインと、Adobeのオプトインオブジェクトおよび Consent Management Provider （CMP）との連携方法を理解するのに役立つドキュメントと連携します。 IAB について詳しくは、Web サイト （[https://www.iabeurope.eu/](https://www.iabeurope.eu/)）を参照してください。

## 最初の手順：Experience CloudID オプトインについて {#first-step-understand-ecid-s-opt-in}

IAB TCF の操作方法を理解するには、まずExperience CloudID サービス（ECID）ライブラリの一部である [!DNL Opt-in] 機能について理解する必要があります。 オプトインの仕組みに詳しくない場合は、最初に [ この役立つ記事 ](https://experienceleague.adobe.com/docs/core-services-learn/tutorials/id-service/use-opt-in-to-control-experience-cloud-activities-based-on-user-consent.html) を参照してください。 また、オプトイン [ ドキュメント ](https://experienceleague.adobe.com/docs/id-service/using/implementation/opt-in-service/optin-overview.html?lang=ja) も確認する必要があります。 これらのリソースを確認したら、このページに戻って続行します。

## IAB TCF 用のAudience Managerプラグイン {#the-audience-manager-plug-in-for-iab-tcf}

これで、オプトインサービスの仕組みについて少なくとも基本的に理解できたので、Audience Managerはサポートを [!DNL IAB Transparency and Consent Framework (TCF)] してオプトインサービスに追加できます。サポートは、オプトインオブジェクトへのプラグインを介して行われます。

IAB TCF のAudience Managerプラグインは、オプトインの機能を拡張し、AAMのお客様が IAB TCF に従ってユーザープライバシーの選択肢を評価、順守、ダウンストリームのパートナーに転送できるようにします。 データ管理者（Adobeのお客様）とベンダー（DMP、DSP、SSP、Ad Server など）の標準を提供します。 を使用して、同意全体の同意を把握できます。

## IAB TCF の有効化 {#enabling-iab-tcf}

IAB TCF 用のAudience Managerプラグインの有効化は、以下の短いビデオに示すように、Adobe Experience Platform Launchを使用している場合はシンプルなチェックボックスなので、簡単に実行できます。

>[!VIDEO](https://video.tv.adobe.com/v/26433/?quality=12)

または、Launch を使用していない場合は、`isIabContext=true` を使用して、Experience Cloud訪問者をインスタンス化する際に有効にできます。 これにより、IAB TCF フローが開始されます。つまり、同意収集に別の手順が追加され、IAB TCF を使用して IAB TC 文字列のクエリが実行され、オプトインに返されます。オプトインは、Experience Cloudソリューションと通信します。

## IAB TC 文字列 {#iab-tcf-consent-string}

IAB が提供する標準の 1 つは「同意文字列」（「DaisyBit」とも呼ばれます）で、これは実際には 2 つのリストが組み合わされています。

1. 目的：**何** に対して同意が与えられているか
1. ベンダー：**誰** に同意が与えられているか？

### 目的 {#purposes}

IAB TCF 2.0 では、同意を収集する 10 の「目的」（ベンダーが訪問者のデータで実行できること）があります。 Adobe Audience Managerでは、10 人全員の同意は必要なく、ベンダーの同意に加えて、次の目的でのみ同意が必要です。

* **目的 1:** デバイス上の情報の保存またはアクセス；
* **目的 10:** 製品の開発と改善；
* **特別な目的 1:** セキュリティの確保、詐欺の防止、デバッグ。

これは IAB TC 文字列の最初の部分であり、1 と 0 として記録されるだけで、その目的/アクティビティが承認されているかどうかを示します。

>[!NOTE]
>
>IAB 規制に従い、特別な目的 1 （セキュリティの確保、詐欺の防止、デバッグ）は常に同意され、ユーザーは異議を唱えることはできません。

### ベンダー {#vendors}

IAB TC 文字列のもう一つの部分は、数百のベンダーの長いリストです。これにより、訪問者は、サイトにタグを持ち、使用するベンダーを選択できる適用可能なベンダーのリストを提示できます。 ベンダーはリスト上で自分のスポットを維持します。 例えば、このリストのAdobe Audience Managerのベンダー番号は 565 です。 リスト内のその番号に「1」がある場合、Audience Managerはリストの先頭から承認された目的を実行できます。 AAM Spot に「0」が含まれる場合、データを使用して何も実行できません。

**お客様が IAB TCF を使用してこれらの目的およびベンダーを選択するための UI を提供する、またはすべてのアクティビティをAudience Manager/却下するには、IAB TCF に登録されている CMP パートナーを使用するか、IAB TCF をサポートし、IAB TCF に登録されている CMP パートナーをビルドする必要があります**。

## オプトイン：IAB アプリケーションとAdobeアプリケーション間の翻訳 {#opt-in-translating-between-iab-and-adobe-solutions}

IAB TCF を使用する利点の 1 つは、上記の標準的な目的により、エンドユーザーがAdobeソリューションのリストよりも、何を承認しているかをより深く把握できる可能性があることです。 エンドユーザーは、Audience Managerや [!DNL Target] ータを「承認」する意味がわからない場合もありますが、「デバイスに情報を保存したり、情報にアクセスしたり」、「製品を開発および改善する」方が、理解し同意する可能性が高くなります。

Audience Managerが承認されるために（例：IAB の目的をオプトインに変換してAAMに「はい」の票を与えるには、上記の目的 1 および 10 をエンドユーザーから同意する必要があります。 これらのいずれかが承認されていない場合、またはベンダーが承認されていない場合、AAMはピクセルファイアの実行や cookie の設定を行いません。 また、多くのお客様は、単に「すべて許可かすべて禁止」の UI をエンドユーザーに提供することを選んでおり、当然ながら、Audience Manager（およびその他のExperience Cloudソリューション）の使用を許可または禁止していることも、知っておくとよいでしょう。

IAB TCF フロー用Audience Managerプラグインがパブリッシャーと広告主の両方のユースケースにどのように適用されるかに関する優れた情報については、[ ドキュメント ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=en) を参照してください。

## IAB：同意のダウンストリーム送信 {#iab-sending-consent-downstream}

IAB TCF 用Audience Managerプラグインを使用すると、ユーザーの同意の選択肢は、グローバルベンダーリストに存在するパートナーのプラットフォームレベル（サードパーティ） ID 同期にも送信され、パートナーはユーザー同意情報を持ち、それを使用して行動できます。 この情報は、次の 2 つの変数で送信されます。

* gdpr = 1
* gdpr_consent = [ エンコードされた同意文字列 ]

ただし、ユーザーが IAB コンテキストにあり、同意を提供しない（または負の同意を提供する）場合、Audience Managerは IAB TC 文字列をまったく収集しないため、呼び出しがドロップされます。 そのため、その場合は…ダウンストリームでの同意の受け渡しはありません。

## Demo {#demo}

以下のビデオでは、ECID およびソリューションの Cookie とビーコンが、IAB ユーザー選択の影響を受ける仕組みについて説明します。

>[!VIDEO](https://video.tv.adobe.com/v/26434/?quality=12)

実装およびテスト方法、ユースケース、ワークフローなど、IAB TCF 2.0 用Audience Managerプラグインについて詳しくは、[ ドキュメント ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html) を参照してください。
