---
title: Microsoft Passport 経由でのパスワードを使用しない ID の認証 | Microsoft Docs
description: Microsoft Passport の概要と、Microsoft Passport のデプロイに関する詳細情報について説明します。
services: active-directory
documentationcenter: ''
author: femila
manager: swadhwa
editor: ''
tags: azure-classic-portal

ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: femila

---
# <a name="authenticating-identities-without-passwords-through-microsoft-passport"></a>Microsoft Passport 経由でのパスワードを使用しない ID の認証
パスワードだけで認証する現在の方法は、ユーザーの安全を保つうえで十分ではありません。 ユーザーは、パスワードを再利用したり、忘れたりすることがあります。 パスワードに対しては、攻撃やフィッシング、解読、推測が可能です。 また、パスワードは、覚えるのが難しく、“[pass the hash](https://technet.microsoft.com/dn785092.aspx)” のような攻撃の対象にもなります。

## <a name="about-microsoft-passport"></a>Microsoft Passport について
Microsoft Passport は、組織とコンシューマー向けの、パスワードを超える秘密/公開キーまたは証明書ベースの認証方式です。 この認証方法で使用されるキー ペア資格情報は、パスワードの代わりに使用でき、侵害、盗難、フィッシングに対する耐性があります。

 Microsoft Passport を使用すると、ユーザーは Microsoft アカウント、Windows Server Active Directory アカウント、Microsoft Azure Active Directory (Azure AD) アカウント、Fast IDentity Online (FIDO) 認証をサポートする Microsoft 以外のサービスに対する認証を行うことができます。 Microsoft Passport の登録時の 2 段階の初期検証の後、Microsoft Passport がユーザーのデバイスに設定されます。次に、ユーザーがジェスチャ (Windows Hello または PIN) を設定します。 ユーザーは、身元を確認するジェスチャを提供します。 その後、Windows が Microsoft Passport を使ってユーザーを認証し、保護されたリソースとサービスにアクセスできるようにします。

秘密キーは、PIN、生体認証、リモート デバイス (ユーザーがデバイスへのサインインに使用したスマート カードなど) などの "ユーザー ジェスチャ" を通じてのみ使用可能になります。 この情報は、証明書または非対称のキー ペアにリンクされます。 デバイスにトラステッド プラットフォーム モジュール (TPM) チップが搭載されている場合、秘密キーはハードウェアで証明されます。 秘密キーはデバイスの外部に移動されることはありません。

公開キーは、Azure Active Directory と Windows Server Active Directory (オンプレミスの場合) に登録されます。 ID プロバイダー (IDP) は、ユーザーの公開キーを秘密キーにマップしてユーザーを検証し、One Time Password (OTP)、PhoneFactor、その他の通知メカニズムを使ってサインイン情報を提供します。

## <a name="why-enterprises-should-adopt-microsoft-passport"></a>Microsoft Passport を導入する理由
Microsoft Passport を有効にすると、次の操作によって会社のリソースのセキュリティを高めることができます。

* ハードウェア推奨オプションで Microsoft Passport をセットアップします。 キーは TPM 1.2 または TPM 2.0 で生成されます (使用できる場合)。 TPM が使用できない場合は、ソフトウェアがキーを生成します。
* PIN の複雑さと長さ、および組織で Hello の使用が有効であるかどうかを定義します。
* 証明書ベースの信頼を使って、スマート カードのようなシナリオをサポートするように Microsoft Passport を構成します。

## <a name="how-microsoft-passport-works"></a>Microsoft Passport の動作
1. キーは、TPM によってハードウェアで、またはソフトウェアで生成されます。 多くのデバイスには、暗号化キーをデバイスに統合してハードウェアをセキュリティで保護する、組み込みの TPM チップが搭載されています。 TPM 1.2 または TPM 2.0 は、キーまたは生成されたキーから作成さる証明書を生成します。
2. TPM は、これらのハードウェア バインド キーを保証します。
3. 1 つのロック解除ジェスチャによって、デバイスのロックが解除されます。 デバイスがドメインまたは Azure AD に参加している場合、このジェスチャを使用して複数のリソースにアクセスできます。

## <a name="how-the-microsoft-passport-lifecycle-works"></a>Microsoft Passport のライフサイクル
![Microsoft Passport のライフサイクル](./media/active-directory-azureadjoin/active-directory-azureadjoin-microsoft-passport.png)

前の図は、秘密/公開キーのペアと、ID プロバイダーによる検証を示しています。 これらの各手順について、以下で詳しく説明します。

1. ユーザーが組み込みの複数の証明方法 (ジェスチャ、物理スマート カード、Multi-Factor Authentication) を通じて自分の身元を証明し、Azure Active Directory、オンプレミスの Active Directory などの ID プロバイダー (IDP) にこの情報を送信します。
2. デバイスが、キーを作成し、キーを証明した後、このキーの公開部分を取り出してステーションのステートメントをアタッチします。署名後 IDP に送信して、このキーを登録します。
3. キーの公開部分が IDP に登録されるとすぐに、IDP はデバイスにチャレンジを返信して、キーの秘密部分に署名するよう要求します。
4. 次に、IDP は、ユーザーとデバイスが保護されたリソースにアクセスできるようにする認証トークンを検証し、発行します。 IDP は、クロスプラットフォーム アプリを作成するか、または (JavaScript/Webcrypto API を使用して) ブラウザーのサポートを使用して、ユーザーの Microsoft Passport の資格情報を作成および使用します。

## <a name="the-deployment-requirements-for-microsoft-passport"></a>Microsoft Passport のデプロイ要件
### <a name="at-the-enterprise-level"></a>企業レベル
* 企業が、Azure サブスクリプションを持っていること。

### <a name="at-the-user-level"></a>ユーザー レベル
* ユーザーのコンピューターで、Windows 10 Professional または Enterprise が実行されていること。

詳細なデプロイメントの手順については、「 [組織での Microsoft Passport for Work の有効化](active-directory-azureadjoin-passport-deployment.md)」を参照してください。

## <a name="additional-information"></a>追加情報
* [エンタープライズ向け Windows 10: デバイスを仕事に使用する方法](active-directory-azureadjoin-windows10-devices-overview.md)
* [Azure Active Directory 参加を使用したクラウド機能の Windows 10 デバイスへの拡張](active-directory-azureadjoin-user-upgrade.md)
* [Azure AD 参加の使用シナリオについて](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Windows 10 エクスペリエンスのためのドメイン参加済みデバイスの Azure AD への接続](active-directory-azureadjoin-devices-group-policy.md)
* [Azure AD Join の設定](active-directory-azureadjoin-setup.md)

<!--HONumber=Oct16_HO2-->


