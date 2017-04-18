<properties 
    pageTitle="Uzyskaj wprowadzenie z usługi Azure Active Directory i usługami programu Visual Studio połączenia (w przypadku projektów MVC) | Microsoft Azure" 
    description="Jak rozpocząć pracę przy użyciu usługi Azure Active Directory w projektach MVC po łączenia się lub tworzenie Azure AD przy użyciu programu Visual Studio połączenia usług" 
    services="active-directory" 
    documentationCenter="" 
    authors="TomArcher" 
    manager="douge" 
    editor=""/>
  
<tags 
    ms.service="active-directory" 
    ms.workload="web" 
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="tarcher"/>

# <a name="getting-started-with-azure-active-directory-and-visual-studio-connected-services-mvc-projects"></a>Wprowadzenie do usługi Azure Active Directory i programu Visual Studio połączenia usług (MVC projektów)

> [AZURE.SELECTOR]
> - [Wprowadzenie](vs-active-directory-dotnet-getting-started.md)
> - [Co się stało](vs-active-directory-dotnet-what-happened.md)
 
##<a name="requiring-authentication-to-access-controllers"></a>Wymaganie uwierzytelniania na kontrolerach programu access 

Wszystkie kontrolery w projekcie zostały adorned z atrybutem **Autoryzuj** . Ten atrybut wymaga użytkownikowi można uwierzytelnić przed uzyskaniem dostępu do tych kontrolerów. Aby umożliwić administratorowi można uzyskiwać dostęp anonimowo, Usuń ten atrybut od administratora. Jeśli chcesz ustawić uprawnienia na poziomie bardziej szczegółowego, zastosuj atrybut, każda z metod wymagającej autoryzacji zamiast dotyczących klasy kontrolerze.
 
##<a name="adding-signin--signout-controls"></a>Dodawanie logowania / Wyloguj się kontrolki 

Aby dodać kontrolki logowania i wyloguj się do widoku, można użyć częściowy widok **_LoginPartial.cshtml** dodać funkcję do jednej widoków. Oto przykład funkcjonalności dodany do widoku standardowego **_Layout.cshtml** . (Uwaga: ostatni element w dziel z zajęć pasek nawigacyjny — Zwiń):

<pre>
    &lt;! DOCTYPE html&gt; 
&lt;html&gt; 
&lt;head&gt; 
&lt;meta charset="utf-8" /&gt; 
&lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt; 
&lt;title&gt;@ViewBag.Title - My ASP.NET Application&lt;/title&gt; 
@Styles.Render("~/Content/css") @Scripts.Render("~/bundles/modernizr") &lt;/head&gt; 
&lt;body&gt; 
&lt;div class="navbar navbar-inverse navbar-fixed-top"&gt; 
&lt;div class="container"&gt; 
&lt;div class="navbar-header"&gt; 
&lt;button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse"&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;/button&gt; 
@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" }) &lt;/div&gt; 
&lt;div class="navbar-collapse collapse"&gt; 
&lt;ul class="nav navbar-nav"&gt; 
&lt;li&gt;@Html.ActionLink("Home", "Index", "Home")&lt;/li&gt; 
&lt;li&gt;@Html.ActionLink("About", "About", "Home")&lt;/li&gt; 
&lt;li&gt;@Html.ActionLink("Contact", "Contact", "Home")&lt;/li&gt; 
&lt;/ul&gt; 
                    <span style="background-color:yellow">@Html.Partial("_LoginPartial")</span> 
                &lt;/ div&gt; 
&lt;/div&gt; 
&lt;/div&gt; 
&lt;div class="container body-content"&gt; 
@RenderBody() &lt;hr /&gt; 
&lt;footer&gt; 
&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET Application&lt;/p&gt; 
&lt;/footer&gt; 
&lt;/div&gt; 
@Scripts.Render("~/bundles/jquery") @Scripts.Render("~/bundles/bootstrap") @RenderSection("scripts", required: false) &lt;/body&gt; 
&lt;/html                                                                                                                                                                                                                                                                                                                                                                                                                                                           &gt;
</pre>

[Dowiedz się więcej na temat usługi Azure Active Directory](https://azure.microsoft.com/services/active-directory/) 
