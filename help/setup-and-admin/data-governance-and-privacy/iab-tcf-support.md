---
title: IAB TCF 2.2 のサポート
description: IAB TCF へのAudience Manager プラグインと、Adobeのオプトインオブジェクトおよび Consent Management Provider （CMP）との連携方法について説明します。
feature: Data Governance & Privacy
thumbnail: 26434.jpg
kt: 5027
role: Developer
level: Experienced
exl-id: 04b4e786-0457-4dcc-bcf9-a79eda67bb2e
source-git-commit: d47848370e7bf7617f2b706041c911161a6479cd
workflow-type: tm+mt
source-wordcount: '1059'
ht-degree: 0%

---

# Audience Managerでの IAB TCF 2.2 のサポート {#iab-tcf-support-in-audience-manager}

Adobeでは、オプトイン機能と、Audience Manager プラグインの IAB Transparency and Consent Framework 2.2 （TCF 2.2）サポートにより、ユーザーのプライバシーの選択を管理および伝達する手段を提供します。 この記事は IAB TCF へのAudience Manager プラグインと、Adobeのオプトインオブジェクトおよび Consent Management Provider （CMP）との連携方法を理解するのに役立つドキュメントと連携します。 IAB について詳しくは、Web サイト （[https://www.iabeurope.eu/](https://www.iabeurope.eu/)）を参照してください。

## 最初の手順：Experience Cloud ID オプトインについて {#first-step-understand-ecid-s-opt-in}

IAB TCF の操作方法を理解するには、まずExperience Cloud ID サービス（ECID）ライブラリの一部である [!DNL Opt-in] 機能について理解する必要があります。 オプトインの仕組みに詳しくない場合は、最初に [&#x200B; この役立つ記事 &#x200B;](https://experienceleague.adobe.com/docs/core-services-learn/tutorials/id-service/use-opt-in-to-control-experience-cloud-activities-based-on-user-consent.html?lang=ja) を参照してください。 また、オプトイン [&#x200B; ドキュメント &#x200B;](https://experienceleague.adobe.com/docs/id-service/using/implementation/opt-in-service/optin-overview.html?lang=ja) も確認する必要があります。 これらのリソースを確認したら、このページに戻って続行します。

## IAB TCF 用Audience Manager プラグイン {#the-audience-manager-plug-in-for-iab-tcf}

これで、オプトインサービスの仕組みについて少なくとも基本的に理解できたので、Audience Managerはサポートを [!DNL IAB Transparency and Consent Framework (TCF)] してオプトインサービスに追加できます。サポートは、オプトインオブジェクトにプラグインを介して行われます。

IAB TCF 用Audience Manager プラグインは、オプトインの機能を拡張し、AAMのお客様が IAB TCF に従ってユーザープライバシーの選択を評価、順守およびダウンストリームパートナーに転送できるようにします。 データ管理者（Adobeのお客様に該当）とベンダー（DMP、DSP、SSP、広告サーバーなど）が同意ランドスケープ全体での同意を理解するために使用できる標準を提供します。

## IAB TCF の有効化 {#enabling-iab-tcf}

IAB TCF 用のAudience Manager プラグインの有効化は、以下の短いビデオに示すように、Adobe Experience Platform Launchを使用している場合は簡単なチェックボックスなので、簡単に実行できます。

>[!VIDEO](https://video.tv.adobe.com/v/38261/?captions=jpn&quality=12)

または、Experience Cloudを使用していない場合は、Launch 訪問者をインスタンス化する際に、`isIabContext=true` を使用して有効にできます。 これにより、IAB TCF フローが開始されます。つまり、同意収集に別の手順が追加され、IAB TCF を使用して IAB TC 文字列のクエリが実行され、オプトインに返されます。オプトインは、その後Experience Cloud ソリューションと通信します。

## IAB TC 文字列 {#iab-tcf-consent-string}

IAB が提供する標準の 1 つは「同意文字列」（「DaisyBit」とも呼ばれます）で、これは実際には 2 つのリストが組み合わされています。

1. 目的：**何** に対して同意が与えられているか
1. ベンダー：**誰** に同意が与えられているか？

### 目的 {#purposes}

IAB TCF 2.2 では、同意を収集する 10 の「目的」（ベンダーが訪問者のデータで実行できること）があります。 Adobe Audience Managerでは、10 人全員の同意は必要なく、ベンダーの同意に加えて、次の目的でのみ同意が必要です。

* **目的 1:** デバイス上の情報の保存またはアクセス；
* **目的 10:** 製品の開発と改善；
* **特別な目的 1:** セキュリティの確保、詐欺の防止、デバッグ。

これは IAB TC 文字列の最初の部分であり、1 と 0 として記録されるだけで、その目的/アクティビティが承認されているかどうかを示します。

>[!NOTE]
>
>IAB 規制に従い、特別な目的 1 （セキュリティの確保、詐欺の防止、デバッグ）は常に同意され、ユーザーは異議を唱えることはできません。

### ベンダー {#vendors}

IAB TC 文字列のもう一つの部分は、数百のベンダーの長いリストです。これにより、訪問者は、サイトにタグを持ち、使用するベンダーを選択できる適用可能なベンダーのリストを提示できます。 ベンダーはリスト上で自分のスポットを維持します。 例えば、このリストのAdobe Audience Managerのベンダー番号は 565 です。 リスト内の該当する番号に「1」が含まれる場合、Audience Managerは、リストの先頭から承認された目的を実行できます。 AAMのスポットに「0」が含まれる場合、データを使用して何も実行できません。

**Audience Managerで、お客様が IAB TCF を使用してこれらの目的およびベンダーを選択するための UI を提供する、またはすべてのアクティビティを承認/却下するには、IAB TCF に登録されている CMP パートナーを使用するか、IAB TCF をサポートし、IAB TCF に登録されている CMP パートナーをビルドする必要があります**。

## オプトイン：IAB アプリケーションとAdobe アプリケーション間の翻訳 {#opt-in-translating-between-iab-and-adobe-solutions}

IAB TCF を使用する利点の 1 つは、上記の標準的な目的により、エンドユーザーがAdobe ソリューションのリストよりも、何を承認しているかをより深く把握できる可能性があることです。 エンドユーザーは、Audience Managerや [!DNL Target] を「承認」する意味がわからない場合もありますが、「デバイスに情報を保存またはアクセス」したり、「製品を開発および改善」する方が、理解し同意する可能性が高くなります。

Audience Managerを承認するため（例：オプトインの IAB 目的を翻訳してAAMに「はい」の票を与えるには、上記の目的 1 および 10 をエンドユーザーから同意する必要があります。 これらのいずれかが承認されていない場合、またはベンダーが承認されていない場合、AAMは pixel fires を実行したり、cookie を設定したりしません。 また、多くのお客様は、単純に「すべて許可かすべて禁止」の UI をエンドユーザーに提供することを選んでいます。これは、もちろん、Audience Manager（およびその他のExperience Cloud ソリューション）の使用を許可または禁止します。

[&#x200B; ドキュメント &#x200B;](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=ja) には、IAB TCF 用Audience Manager プラグイン フローがパブリッシャーと広告主の両方のユースケースにどのように適用されるかに関する優れた情報が記載されています。

## IAB：同意のダウンストリーム送信 {#iab-sending-consent-downstream}

IAB TCF 用Audience Manager プラグインを使用すると、ユーザーの同意の選択肢は、グローバルベンダーリストに存在するパートナーのプラットフォームレベル（サードパーティ）の ID 同期にも送信されます。これにより、パートナーはユーザーの同意情報を持ち、それを使用して行動できます。 この情報は、次の 2 つの変数で送信されます。

* gdpr = 1
* gdpr_consent = [ エンコードされた同意文字列 ]

ただし、ユーザーが IAB コンテキストにあり、同意を提供しない（または負の同意を提供する）場合、Audience Managerは IAB TC 文字列をまったく収集しないため、呼び出しはドロップされます。 そのため、その場合は…ダウンストリームでの同意の受け渡しはありません。

## Demo {#demo}

以下のビデオでは、ECID およびソリューションの Cookie とビーコンが、IAB ユーザー選択の影響を受ける仕組みについて説明します。

>[!VIDEO](https://video.tv.adobe.com/v/38245/?captions=jpn&quality=12)

実装およびテスト方法、ユースケース、ワークフローなど、IAB TCF 2.2 用Audience Manager プラグインについて詳しくは、[&#x200B; ドキュメント &#x200B;](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=ja) を参照してください。
