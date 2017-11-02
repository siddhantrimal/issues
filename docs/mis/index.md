standard documentation<a name="#top"></a>

# Configuring MIS on a 32-bit Windows 7 system

## Pre-requisites
You'll need the following resources before beginning

* Native Requirements
    * IMS Backend Host Files
    * IMS Frontend Host Files

* Software Requirements
    * Hard Requirements
        * .NET 4.5 Framework Standalone Installer
        * SQL Server 2012 Enterprise 32-bit ([fallback to SQL Server 2012 Express](https://www.microsoft.com/en-us/download/details.aspx?id=35579&751be11f-ede8-5a0c-058c-2ee190a24fa6=True))
        * SQL Server Management Studio 2012 32-bit (as `SQLEXPRWT_x86_ENU.exe` in [here](https://www.microsoft.com/en-us/download/details.aspx?id=35579&751be11f-ede8-5a0c-058c-2ee190a24fa6=True))
    * Soft Requirements
        * Google Chrome Browser Standalone (32-bit) ([fallback URL for latest standalone](https://www.google.com/chrome/browser/desktop/index.html?system=true&standalone=1))
        * Sublime Text Editor ([download](https://www.sublimetext.com/3))


## Preparing the device
Please follow the steps, in order, for successful installation

1. Before doing _anything_, Install all the [pre-requisites](#pre-requisites) properly
1. Press `Win+R` and type `inetmgr`; then hit `ENTER`
    * If this does __not__ open IIS Manager, press `Win+R`, type `optionalfeatures`, hit `ENTER` and, Enable IIS along with associated .NET Services. It is __very important__ to check if all the features in `Internet Information Services > World Wide Web Services > Application Development Features > *` are enabled. These are expected to be enabled.
1. On the IIS Manager, create a frontend and a backend, in each case.
    * Ensure that the binding is taking place properly.
     * Bind with all IP Addresses i.e.`*` and, specific ports (say) `:88` and `:99`.
     * __The host files for both are associated with `.NET 4.5 Framework`.__ _Since it is a_ ___just in place___ _replacement for .NET 4.0, it is highly compatible and, must not be confused._
     * The default .NET framework for Windows 7 is `.NET 3.5` and it __will NOT__ show any newer .NET framework in the `optionalfeatures` list even if you install it in your device. However, `.NET 4.5` is vital to your Application Pool.
        * To enable `.NET 4.5` manually, run this command in `cmd.exe` with Administrative priviledge.
        ```
        %windir%\Microsoft.NET\Framework\v4.0.30319\aspnet_regiis.exe -ir
        ```
        * ___NOTE:___ _The above command runs successfully only when you install .NET 4.5 Framework first_
        * The `.NET 4.5` will still not show up in your Application pool. But you can now manually make an entry like follows:
        ```
        Name: .NET 4.5
        .NET Framework Version: .NET Framework v4.0.30319
        Managed pipeline mode: Integrated
        [✓] : Start application pool immediately
        ```
        * __Then on the host file Basic setting(i.e.`IIS Manager > IMS-UI > Basic Setting`) you will be finally able to select `.NET 4.5`__
4. Finally, double check your preparation by hitting the server.
    * If you browse the server, it should display a similar kind of footer information to this: `Version Information: Microsoft .NET Framework Version:4.0.30319; ASP.NET Version:4.0.30319.34209` along with a standard `404 Error`.
    * If you're getting `Error 500` or `503`, something has gone terribly wrong in the previous steps.
    * Also, browse your fronend. It should be able to open but should not be able to access the server.

## Setting up

1. __Host File Configurations__
Even though the server may be behaving _seemingly correctly_, it may really not be the case. In this step, we need to first change the following files

    * `Web.Config`
        * Please adjust the Server connections strings properly
        * To double check this, please visit the `IIS Manager > IIS_Backend > Connection Settings` and read the `AbDataContext` entry carefully.
            * Most errors, such as `GET http://192.168.92.130:88/api/school/getAll 500 (Internal Server Error)` are due to this.
    * `common.services`
        * Properly set the IP Address. `localhost:88` is an example. Or, you may bind to static IP(_not recommended_)

## Basic Troubleshoot
* __Binding Settings:__ If these settings are improper, IP will only be available locally, for access
    * Can be configured in `inetmgr`
* __Enable Services:__ If the server is giving you trouble, chances are that it(`MSSQLSERVER`) hasn't been enabled. 
    * If you've renamed `MSSQLSERVER` as something else, chances are that your `AbDataContext` is still searching for the wrong server name. Fix in `Web.Config` or `IIS Manager > Your Host > Connection Settings`
* __Allow Ports through Firewall:__ If everything is done to the letter, and your system still doesn't work, the firewall is probably blocking it. Please configure the `Inbound` and `Outbound` ports as the two required ports in ___Advanced Firewall Settings___


---

# KEYWORDS
|ABBR./Keyword|FullForm/Info|
|---|---
|__MIS__|Management Information System
|__IMS__|Information Management System
|__SSMS__|SQL Server Management Studio
|__Standalone__|Self-suffifient ; does not need internet

[^Back to top](#top)
