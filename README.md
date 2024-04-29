# Windows Server Firewall Yapilandirmasi

Windows Server 2019 güvenliğiniz için fiziksel bir firewall cihazı kullanmıyorsanız sunucunuzun güvenliği için firewall ayarlarını nasıl yapacağınızı sizinle paylaşmak isterim. 

Windows Server 2019, "Start" menüsünden

  "Windows Settings" menüsünü açınız.
    Windows Server 2016 ve 2019 da bulunan "Update & Security" menüsünü açınız..
    Sol menüden "Windows Security" seçiniz.
    Orta bölümde bulunan "Protection areas" altındaki "Firewall & network protection" açınız.
    Firewall & network protection menüsü altından Domain network, Private Network ve Public Network turn off yada turn on duruma getiriniz.  .
        Domain network menüsüne giriş yaparak "Helps protect your device while on a domain network" altından on veya off duruam getirebilirsiniz.
        Private Network menüsüne giriş yaparak "Helps protect your device while on a private network" altından on veya off duruam getirebilirsiniz.
        Public Network menüsüne giriş yaparak "Helps protect your device while on a public network" altından on veya off duruam getirebilirsiniz. 

  Güvenlik duvarı özellikleri penceresini kapatmak için sağ üstte bulunan "X" tıklayınız. 


Windows 2019 desktop experience ile firewall kural işlemlerini gerçekleştirmek için;


  Control Panel açınız
        "Windows Defender" Firewall tıklayınız.
        Sol bölümde bulunan "Advanced settings" tıklayınız
        Sol menüde bulunan inbound rules yada outbound rules tanımlaması yapabilirsiniz.
            Inbound rules tıklayınız.
                Sağ bölümde bulunan "New Rule…" tıklayınız.
                    Program tabanlı izin yönetimi yapmak için programı seçiniz. Program path belirtiniz. Allow yada Block olarak tanımlama yapınız. Kuralın geçerli olduğu kriterleri seçip ismini belirleyiniz.
                    Port tabanlı izin yönetimi yapmak için Port seçiniz. Port tipini belirleyiniz. Allow yada Block olarak tanımlama yapınız. Kuralın geçerli olduğu kriterleri seçip ismini belirleyiniz.
                    Custom olarak izin yönetimi yapmak için Protocol ve port belirleyiniz. IP tabanlı özelleştirme için scope ayarlarını yapılandırınız. Allow yada Block olarak tanımlama yapınız. Kuralın geçerli olduğu kriterleri seçip ismini belirleyiniz. 
            outbound rules tıklayınız.
                Sağ bölümde bulunan "New Rule…" tıklayınız.
                    Program tabanlı izin yönetimi yapmak için programı seçiniz. Program path belirtiniz. Allow yada Block olarak tanımlama yapınız. Kuralın geçerli olduğu kriterleri seçip ismini belirleyiniz.
                    Port tabanlı izin yönetimi yapmak için Port seçiniz. Port tipini belirleyiniz. Allow yada Block olarak tanımlama yapınız. Kuralın geçerli olduğu kriterleri seçip ismini belirleyiniz.
                    Custom olarak izin yönetimi yapmak için Protocol ve port belirleyiniz. IP tabanlı özelleştirme için scope ayarlarını yapılandırınız. Allow yada Block olarak tanımlama yapınız. Kuralın geçerli olduğu kriterleri seçip ismini belirleyiniz. 




Firewall işlemlerinizi PowerShell kullanarak yapmak için;

Windows PowerShell açınız.

`Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False` (firewall devre dışı) 
`Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled True` (firewall aktif)

PowerShell üzerinden detaylı firewall kuralları tanımlamak için aşağıdaki komutları kullanabilirsiniz.

 
```
New-NetFirewallRule
   [-PolicyStore <String>]
   [-GPOSession <String>]
   [-Name <String>]
   -DisplayName <String>
   [-Description <String>]
   [-Group <String>]
   [-Enabled <Enabled>]
   [-Profile <Profile>]
   [-Platform <String[]>]
   [-Direction <Direction>]
   [-Action <Action>]
   [-EdgeTraversalPolicy <EdgeTraversal>]
   [-LooseSourceMapping <Boolean>]
   [-LocalOnlyMapping <Boolean>]
   [-Owner <String>]
   [-LocalAddress <String[]>]
   [-RemoteAddress <String[]>]
   [-Protocol <String>]
   [-LocalPort <String[]>]
   [-RemotePort <String[]>]
   [-IcmpType <String[]>]
   [-DynamicTarget <DynamicTransport>]
   [-Program <String>]
   [-Package <String>]
   [-Service <String>]
   [-InterfaceAlias <WildcardPattern[]>]
   [-InterfaceType <InterfaceType>]
   [-LocalUser <String>]
   [-RemoteUser <String>]
   [-RemoteMachine <String>]
   [-Authentication <Authentication>]
   [-Encryption <Encryption>]
   [-OverrideBlockRules <Boolean>]
   [-CimSession <CimSession[]>]
   [-ThrottleLimit <Int32>]
   [-AsJob]
   [-WhatIf]
   [-Confirm]
   [<CommonParameters>]
```
Örneğin Sunucunuz üzerinde 80 portunu kapatmak için aşağıdaki kodu kullanabilirsiniz.

`PS C:\> New-NetFirewallRule -DisplayName "Block Outbound Port 80" -Direction Outbound -LocalPort 80 -Protocol TCP -Action Block`

veya sunucunuz üzerinde 80 portunu açmak için aşağıdaki kodu kullanabilirsiniz. 

`PS C:\> New-NetFirewallRule -DisplayName "Allow Outbound Port 80" -Direction Outbound -LocalPort 80 -Protocol TCP -Action Allow`


Güvenlik duvarını netsh komutları kullanılarak kapatmak  ve açmak için:

CMD komut satırını açınız. 

`netsh advfirewall set allprofiles state off` (firewall devre dışı)
`netsh advfirewall set allprofiles state on`  (firewall aktif)
