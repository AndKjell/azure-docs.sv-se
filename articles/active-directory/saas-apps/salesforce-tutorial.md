---
title: 'Självstudier: Azure Active Directory-integrering med Salesforce | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Salesforce.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2018
ms.author: jeedes
ms.openlocfilehash: 36f1bd9c11c8932968a3501ef22fdb7153411256
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/08/2018
ms.locfileid: "48867569"
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce"></a>Självstudier: Azure Active Directory-integrering med Salesforce

I den här självstudien får du lära dig hur du integrerar Salesforce med Azure Active Directory (AD Azure).

Integrera Salesforce med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till Salesforce.
- Du kan aktivera användarna att automatiskt få loggat in på Salesforce (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton på en central plats – Azure portal.

Om du vill veta mer om integrering av SaaS-app med Azure AD finns i [vad är programåtkomst och enkel inloggning med Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Förutsättningar

Om du vill konfigurera Azure AD-integrering med Salesforce, behöver du följande objekt:

- En Azure AD-prenumeration
- En Salesforce enkel inloggning aktiverat prenumeration

> [!NOTE]
> Om du vill testa stegen i den här självstudien rekommenderar vi inte med hjälp av en produktionsmiljö.

Om du vill testa stegen i den här självstudien bör du följa dessa rekommendationer:

- Använd inte din produktionsmiljö, om det inte behövs.
- Om du inte har en Azure AD-utvärderingsmiljö, kan du [få en månads utvärdering](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning

I den här självstudien kan du testa Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här självstudien består av två viktigaste byggstenarna:

1. Att lägga till Salesforce från galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-salesforce-from-the-gallery"></a>Att lägga till Salesforce från galleriet

Om du vill konfigurera integreringen av Salesforce till Azure AD, som du behöver lägga till Salesforce från galleriet i din lista över hanterade SaaS-appar.

**Utför följande steg för att lägga till Salesforce från galleriet:**

1. I den **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.

    ![Azure Active Directory-knappen][1]

2. Gå till **företagsprogram**. Gå till **alla program**.

    ![Bladet för Enterprise-program][2]

3. Lägg till nytt program, klicka på **nytt program** knappen överst i dialogrutan.

    ![Knappen Nytt program][3]

4. I sökrutan skriver **Salesforce**väljer **Salesforce** resultatet panelen klickar **Lägg till** för att lägga till programmet.

    ![Salesforce i resultatlistan](./media/salesforce-tutorial/tutorial_salesforce_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med Salesforce baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning att fungera, behöver Azure AD du veta vad du motsvarighet i Salesforce är till en användare i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och relaterade användaren i Salesforce upprättas.

I Salesforce, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** att upprätta länken-relation.

Om du vill konfigurera och testa Azure AD enkel inloggning med Salesforce, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  – om du vill ge användarna använda den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  – om du vill testa Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare i Salesforce](#create-a-salesforce-test-user)**  – du har en motsvarighet för Britta Simon i Salesforce som är länkad till en Azure AD-representation av användaren.
4. **[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  – om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  – om du vill kontrollera om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet ska du aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Salesforce-program.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med Salesforce:**

1. I Azure-portalen på den **Salesforce** program integration-sidan klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning för länken][4]

2. Klicka på **ändra enkel inloggningsläge** på skärmen för att välja den **SAML** läge.

    ![Konfigurera enkel inloggning för länken](./media/salesforce-tutorial/tutorial_general_300.png)

3. På den **väljer du en metod för enkel inloggning** dialogrutan klickar du på **Välj** för **SAML** läge för att aktivera enkel inloggning.

    ![Konfigurera enkel inloggning för länken](./media/salesforce-tutorial/tutorial_general_301.png)

4. På den **ange in enkel inloggning med SAML** klickar du på **redigera** knappen för att öppna **SAML grundkonfiguration** dialogrutan.
   
    ![Konfigurera enkel inloggning för länken](./media/salesforce-tutorial/tutorial_general_302.png)

5. På den **SAML grundkonfiguration** avsnittet, utför följande steg:

    ![Salesforce-domän och URL: er med enkel inloggning för information](./media/salesforce-tutorial/tutorial_salesforce_url.png)

    a. I den **inloggnings-URL** textrutan skriver du värdet med följande mönster:

    Enterprise-konto: `https://<subdomain>.my.salesforce.com`

    Developer-konto: `https://<subdomain>-dev-ed.my.salesforce.com`

    b. I den **identifierare** textrutan skriver du värdet med följande mönster:

    Enterprise-konto: `https://<subdomain>.my.salesforce.com`

    Developer-konto: `https://<subdomain>-dev-ed.my.salesforce.com`

    > [!NOTE]
    > Dessa värden är inte verkliga. Uppdatera dessa värden med faktiska inloggnings-URL och identifierare. Kontakta [Salesforce-klienten supportteamet](https://help.salesforce.com/support) att hämta dessa värden.

6. På den **SAML-signeringscertifikat** avsnittet, klicka på **hämta** att ladda ned **Federation Metadata XML** och spara xml-filen på datorn.

    ![Länk för hämtning av certifikat](./media/salesforce-tutorial/tutorial_salesforce_certificate.png) 

7. Öppna en ny flik i webbläsaren och logga in på ditt Salesforce-administratörskonto.

8. Klicka på den **installationsprogrammet** under **inställningsikonen** i det övre högra hörnet på sidan.

    ![Konfigurera enkel inloggning](./media/salesforce-tutorial/configure1.png)

9. Rulla ned till den **inställningar** i navigeringsfönstret klickar du på **identitet** att expandera avsnittet relaterade. Klicka sedan på **inställningar för enkel inloggning**.

    ![Konfigurera enkel inloggning](./media/salesforce-tutorial/sf-admin-sso.png)

10. På den **inställningar för enkel inloggning** klickar du på den **redigera** knappen.

    ![Konfigurera enkel inloggning](./media/salesforce-tutorial/sf-admin-sso-edit.png)

    > [!NOTE]
    > Om det inte går att aktivera enkel inloggning för inställningar för ditt Salesforce-konto kan du behöva kontakta [Salesforce-klienten supportteamet](https://help.salesforce.com/support).

11. Välj **SAML aktiverat**, och klicka sedan på **spara**.

      ![Konfigurera enkel inloggning](./media/salesforce-tutorial/sf-enable-saml.png)

12. Om du vill konfigurera SAML enkel inloggning för, klickar du på **nytt från metadatafilen**.

    ![Konfigurera enkel inloggning](./media/salesforce-tutorial/sf-admin-sso-new.png)

13. Klicka på **Välj fil** att ladda upp den XML-fil som du har hämtat i Azure-portalen och klicka på **skapa**.

    ![Konfigurera enkel inloggning](./media/salesforce-tutorial/xmlchoose.png)

14. På den **SAML enkel inloggning inställningar** sidan fält fylla i automatiskt och klicka på Spara.

    ![Konfigurera enkel inloggning](./media/salesforce-tutorial/salesforcexml.png)

15. I det vänstra navigeringsfönstret i Salesforce, klickar du på **Företagsinställningar** Expandera avsnittet relaterade och klicka sedan på **min domän**.

    ![Konfigurera enkel inloggning](./media/salesforce-tutorial/sf-my-domain.png)

16. Rulla ned till den **Autentiseringskonfiguration** , och klicka på **redigera** knappen.

    ![Konfigurera enkel inloggning](./media/salesforce-tutorial/sf-edit-auth-config.png)

17. I den **Autentiseringskonfiguration** avsnittet, kontrollera den **AzureSSO** som **autentisering Servie** SAML SSO-konfiguration och klicka sedan på **spara** .

    ![Konfigurera enkel inloggning](./media/salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > Om mer än en authentication-tjänsten är markerat uppmanas användarna att välja vilka autentiseringstjänst som de vill logga in med när du initierar enkel inloggning till ditt Salesforce-miljö. Om du inte vill det. inträffa så bör du **lämnar du alla andra autentiseringstjänster avmarkerad**.

### <a name="create-an-azure-ad-test-user"></a>Skapa en Azure AD-testanvändare

Målet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.

1. I Azure-portalen, i den vänstra rutan väljer **Azure Active Directory**väljer **användare**, och välj sedan **alla användare**.

    ![Skapa en Azure AD-användare][100]

2. Välj **ny användare** överst på skärmen.

    ![Skapa en Azure AD-användare för testning](./media/salesforce-tutorial/create_aaduser_01.png) 

3. Utför följande steg i egenskaperna för användaren.

    ![Skapa en Azure AD-användare för testning](./media/salesforce-tutorial/create_aaduser_02.png)

    a. I den **namn** ange **BrittaSimon**.
  
    b. I den **användarnamn** fälttyp **brittasimon@yourcompanydomain.extension**  
    Till exempel, BrittaSimon@contoso.com

    c. Välj **egenskaper**väljer den **Show lösenord** kryssrutan och sedan skriva ned det värde som visas i rutan lösenord.

    d. Välj **Skapa**.

### <a name="create-a-salesforce-test-user"></a>Skapa en testanvändare i Salesforce

I det här avsnittet skapas en användare som kallas Britta Simon i Salesforce. Salesforce har stöd för just-in-time-etablering, som är aktiverat som standard. Det finns inga uppgift åt dig i det här avsnittet. Om en användare inte redan finns i Salesforce, skapas en ny när du försöker få åtkomst till Salesforce. Salesforce stöder även automatisk användaretablering, kan du hitta mer information om [här](salesforce-provisioning-tutorial.md) om hur du konfigurerar automatisk användaretablering.

### <a name="assign-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet ska aktivera du Britta Simon att använda Azure enkel inloggning om du beviljar åtkomst till Salesforce.

![Tilldela rollen][200]

**Om du vill tilldela Britta Simon Salesforce, utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** klickar **alla program**.

    ![Tilldela användare][201]

2. I listan med program väljer **Salesforce**.

    ![Salesforce-länk i listan med program](./media/salesforce-tutorial/tutorial_salesforce_app.png)

3. I menyn till vänster, klickar du på **användare och grupper**.

    ![Länken ”användare och grupper”][202]

4. Klicka på **Lägg till användare** knappen. Välj sedan **användare och grupper** på **Lägg till tilldelning** dialogrutan.

    ![Fönstret Lägg till tilldelning][203]

5. På **användare och grupper** dialogrutan **Britta Simon** på listan användare.

6. Klicka på **Välj** knappen **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen **Lägg till tilldelning** dialogrutan.

### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet ska testa du Azure AD enkel inloggning för konfigurationen med hjälp av åtkomstpanelen.

När du klickar på panelen Salesforce i åtkomstpanelen du bör få automatiskt loggat in på ditt Salesforce-program.
Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över guider om hur du integrerar SaaS-appar med Azure Active Directory](tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Konfigurera Användaretablering](salesforce-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/salesforce-tutorial/tutorial_general_01.png
[2]: ./media/salesforce-tutorial/tutorial_general_02.png
[3]: ./media/salesforce-tutorial/tutorial_general_03.png
[4]: ./media/salesforce-tutorial/tutorial_general_04.png

[100]: ./media/salesforce-tutorial/tutorial_general_100.png

[200]: ./media/salesforce-tutorial/tutorial_general_200.png
[201]: ./media/salesforce-tutorial/tutorial_general_201.png
[202]: ./media/salesforce-tutorial/tutorial_general_202.png
[203]: ./media/salesforce-tutorial/tutorial_general_203.png