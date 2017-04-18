<properties
   pageTitle="Przykładowe aplikacji do użytku w środowiskach granicę zabezpieczeń | Microsoft Azure"
   description="Wdrażanie tej aplikacji prostej sieci web po utworzeniu strefy Zdemilitaryzowanej, aby przetestować scenariuszy przepływu ruchu"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/01/2016"
   ms.author="jonor"/>

# <a name="sample-application-for-use-with-security-boundary-environments"></a>Przykładowa aplikacja do użytku z środowiskach granicę zabezpieczeń

[Wróć do strony zabezpieczeń obramowanie najważniejsze wskazówki][HOME]

Tych skryptów programu PowerShell można uruchamiać lokalnie na serwerach IIS01 i AppVM01, aby zainstalować i konfigurowanie aplikacji sieci web bardzo proste, na której są wyświetlane strony html na serwerze IIS01 fronton zawartości z serwera AppVM01 wewnętrznej bazy danych.

Ta aplikacja będzie przewiduje prosty środowiska testowania w wielu przykładach strefy Zdemilitaryzowanej i w jaki sposób zmiany na punkty końcowe, NSGs, UDR i zapory reguł może mieć wpływ ruchu.

## <a name="firewall-rule-to-allow-icmp"></a>Reguły zapory, aby umożliwić ICMP
Ten prosty instrukcji programu PowerShell można uruchamiać na dowolnego maszyn wirtualnych systemu Windows, aby zezwolić na ruch ICMP (Ping). Dzięki temu będzie dla łatwiejsze testowym i rozwiązywania problemów, umożliwiając protokołu ping w celu przekazania przez zaporę systemu Windows (w przypadku większości distros Linux, które ICMP jest domyślnie włączone).

    # Turn On ICMPv4
    New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" `
        -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow

**Uwaga:** Jeśli używasz skryptów, to dodawanie reguły zapory Oto pierwszej instrukcji.

## <a name="iis01---web-application-installation-script"></a>IIS01 - skrypt instalacji aplikacji sieci Web
Ten skrypt będzie:

1.  Otwórz IMCPv4 (Ping) na lokalnego serwera zapory systemu windows do testowania łatwiejsze
2.  Instalowanie programu IIS i programu .net Framework v4.5
3.  Tworzenie pliku Web.config i strona sieci web programu ASP.NET
4.  Zmienianie domyślnej puli aplikacji w celu ułatwienia dostępu do plików
5.  Ustawianie użytkownikom anonimowym do konta administratora i hasło

Tego skryptu programu PowerShell powinny być uruchamiane lokalnie, gdy RDP sprzedał w IIS01.

    # IIS Server Post Build Config Script
    # Get Admin Account and Password
        Write-Host "Please enter the admin account information used to create this VM:" -ForegroundColor Cyan
        $theAdmin = Read-Host -Prompt "The Admin Account Name (no domain or machine name)"
        $thePassword = Read-Host -Prompt "The Admin Password"
        
    # Turn On ICMPv4
        Write-Host "Creating ICMP Rule in Windows Firewall" -ForegroundColor Cyan
        New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow
        
    # Install IIS
        Write-Host "Installing IIS and .Net 4.5, this can take some time, like 15+ minutes..." -ForegroundColor Cyan
        add-windowsfeature Web-Server, Web-WebServer, Web-Common-Http, Web-Default-Doc, Web-Dir-Browsing, Web-Http-Errors, Web-Static-Content, Web-Health, Web-Http-Logging, Web-Performance, Web-Stat-Compression, Web-Security, Web-Filtering, Web-App-Dev, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Net-Ext, Web-Net-Ext45, Web-Asp-Net45, Web-Mgmt-Tools, Web-Mgmt-Console
        
    # Create Web App Pages
        Write-Host "Creating Web page and Web.Config file" -ForegroundColor Cyan
        $MainPage = '<%@ Page Language="vb" AutoEventWireup="false" %>
        <%@ Import Namespace="System.IO" %>
        <script language="vb" runat="server">
            Protected Sub Page_Load(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Load
                Dim FILENAME As String = "\\10.0.2.5\WebShare\Rand.txt"
                Dim objStreamReader As StreamReader
                objStreamReader = File.OpenText(FILENAME)
                Dim contents As String = objStreamReader.ReadToEnd()
                lblOutput.Text = contents
                objStreamReader.Close()
                lblTime.Text = Now()
            End Sub
        </script>
            
        <!DOCTYPE html>
        <html xmlns="http://www.w3.org/1999/xhtml">
        <head runat="server">
            <title>DMZ Example App</title>
        </head>
        <body style="font-family: Optima,Segoe,Segoe UI,Candara,Calibri,Arial,sans-serif;">
          <form id="frmMain" runat="server">
            <div>
              <h1>Looks like you made it!</h1>
              This is a page from the inside (a web server on a private network),<br />
              and it is making its way to the outside! (If you are viewing this from the internet)<br />
              <br />
              The following sections show:
              <ul style="margin-top: 0px;">
                <li> Local Server Time - Shows if this page is or isnt cached anywhere</li>
                <li> File Output - Shows that the web server is reaching AppVM01 on the backend subnet and successfully returning content</li>
                <li> Image from the Internet - Doesnt really show anything, but it made me happy to see this when the app worked</li>
              </ul>
              <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
                <b>Local Web Server Time</b>: <asp:Label runat="server" ID="lblTime" /></div>
              <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
                <b>File Output from AppVM01</b>: <asp:Label runat="server" ID="lblOutput" /></div>
              <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
                <b>Image File Linked from the Internet</b>:<br />
                <br />
                <img src="http://sd.keepcalm-o-matic.co.uk/i/keep-calm-you-made-it-7.png" alt="You made it!" width="150" length="175"/></div>
            </div>
          </form>
        </body>
        </html>'
        
        $WebConfig ='<?xml version="1.0" encoding="utf-8"?>
        <configuration>
          <system.web>
            <compilation debug="true" strict="false" explicit="true" targetFramework="4.5" />
            <httpRuntime targetFramework="4.5" />
            <identity impersonate="true" />
            <customErrors mode="Off"/>
          </system.web>
          <system.webServer>
            <defaultDocument>
              <files>
                <add value="Home.aspx" />
              </files>
            </defaultDocument>
          </system.webServer>
        </configuration>'
            
        $MainPage | Out-File -FilePath "C:\inetpub\wwwroot\Home.aspx" -Encoding ascii
        $WebConfig | Out-File -FilePath "C:\inetpub\wwwroot\Web.config" -Encoding ascii
    
    # Set App Pool to Clasic Pipeline to remote file access will work easier
        Write-Host "Updaing IIS Settings" -ForegroundColor Cyan
        c:\windows\system32\inetsrv\appcmd.exe set app "Default Web Site/" /applicationPool:".NET v4.5 Classic"
        c:\windows\system32\inetsrv\appcmd.exe set config "Default Web Site/" /section:system.webServer/security/authentication/anonymousAuthentication /userName:$theAdmin /password:$thePassword /commit:apphost
        
    # Make sure the IIS settings take
        Write-Host "Restarting the W3SVC" -ForegroundColor Cyan
        Restart-Service -Name W3SVC
        
        Write-Host
        Write-Host "Web App Creation Successfull!" -ForegroundColor Green
        Write-Host


## <a name="appvm01---file-server-installation-script"></a>AppVM01 - plik skryptu instalacji serwera
Ten skrypt automatycznie konfiguruje wewnętrznej dla tej prostej aplikacji. Ten skrypt będzie:

1.  Otwórz IMCPv4 (Ping) w zaporze łatwiejsze testowania
2.  Utwórz nowy katalog
3.  Tworzenie pliku tekstowego jako zdalnego dostępu strony sieci web
4.  Ustawianie uprawnień dla katalogów i plików do anonimowy, aby umożliwić dostęp
5.  Wyłączanie rozszerzonych zabezpieczeń programu Internet Explorer umożliwia przeglądanie łatwiejsze z tego serwera 

>[AZURE.IMPORTANT] **Najważniejsze wskazówki**: nigdy nie zostanie wyłączone rozszerzonych zabezpieczeń programu Internet Explorer na serwerze produkcyjnym, a także zazwyczaj jest dobrym pomysłem można przeglądać w sieci web na serwerze produkcyjnym. Ponadto otwarcia udziały plików dla dostępu anonimowego jest dobrym pomysłem, ale gotowe tutaj dla uproszczenia.

Tego skryptu programu PowerShell powinny być uruchamiane lokalnie, gdy RDP sprzedał w AppVM01. PowerShell jest wymagane do się polecenie Uruchom jako Administrator, aby zapewnić pomyślne wykonanie.
    
    # AppVM01 Server Post Build Config Script
    # PowerShell must be run as Administrator for Net Share commands to work
    
    # Turn On ICMPv4
        New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow
    
    # Create Directory
        New-Item "C:\WebShare" -ItemType Directory
    
    # Write out Rand.txt
        $FileContent = "Hello, I'm the contents of a remote file on AppVM01."
        $FileContent | Out-File -FilePath "C:\WebShare\Rand.txt" -Encoding ascii
    
    # Set Permissions on share
        $Acl = Get-Acl "C:\WebShare"
        $AccessRule = New-Object system.security.accesscontrol.filesystemaccessrule("Everyone","ReadAndExecute, Synchronize","ContainerInherit, ObjectInherit","InheritOnly","Allow")
        $Acl.SetAccessRule($AccessRule)
        Set-Acl "C:\WebShare" $Acl
    
    # Create network share
        Net Share WebShare=C:\WebShare "/grant:Everyone,READ"
    
    # Turn Off IE Enhanced Security Configuration for Admins
        Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0
    
        Write-Host
        Write-Host "File Server Setup Successfull!" -ForegroundColor Green
        Write-Host
    

## <a name="dns01---dns-server-installation-script"></a>DNS01 - skryptu instalacji serwera DNS
Istnieje skryptu zawarte w tej aplikacji przykładowej konfiguracji serwera DNS. Jeśli testujesz reguły zapory, musi zawierać ruchu DNS NSG lub UDR, trzeba ręcznie będzie konfiguracji serwera DNS01. Plik xml konfiguracji sieci dla obu przykładach zawiera DNS01 jako podstawowy serwer DNS i publicznej serwera DNS obsługiwanego przez 3 poziom jako kopii zapasowych serwera DNS. Serwer DNS 3 poziom być rzeczywiste serwerów DNS na potrzeby ruchu nie lokalnego i z DNS01 nie konfiguracji, nie lokalnych DNS mogą się pojawić.

<!--Link References-->
[HOME]: ../best-practices-network-security.md
