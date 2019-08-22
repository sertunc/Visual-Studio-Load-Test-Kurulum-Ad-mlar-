# Yük Testi Kurulumu
- vs_enterprise__2033924517.1488196047.exe (ana bilgisayar)
- vs_testcontroller__1058008090.1542709831.exe  (ana bilgisayar)
- vs_testagent__1058008090.1542709831.exe (yük yapacak olan bilgisayarlar)

# Kurulumlar tamamlandiktan sonra
  - **Visual Studio Test Controller (VSTTController)** servisinin çalıştığından emin olun. Duruma göre "Log On" kısmından login olan kullanıcı bilgileri girilir.
  - Ana bilgisayarda **"Computer Management"** -> **"Local Users and Groups"** açılır ve **"Group"** klasöründe **"TeamTestAgentService"**, **"TeamTestControllerAdmins"** ve **"TeamTestControllerUsers"** gruplarına test agent olarak bağlanacak bilgisayarların kullanıcıları eklenir. *Örneğin Sertunc pcsinden selen kullanıcısı ile bağlanılacaksa selen kullanıcısı eklenir.*
  - **"Test Controller Configuration Tool"** açılır ve **"This Account"** kısmına login olunan kullanıcı bilgileri girilir.
  - Daha sonra Visual Studio'dan test projesi açılır ve sol üst kısımdaki butonlardan **"Manage Test Controller"** (çekiç simgesi) tıklanır. Controller kısımından Bilgisayar adı seçilir, yoksa elle girilir.
  - Ardindan yük yapacak olan bilgisayarlara test agent kurulur ve **"Test Agent Configuration Tool"** açılır. **"Run test agent as interactive process"** kısmına login olan kullanıcı bilgileri girilir. **"Register with Test Controller"** kısmı checklenir ve **Controller pc** adi girilir. Örneğin SERTUNCSERVER:6901 ve "Apply Settings"e tıklanır. 
  - Tekrar Visual Studio'dan "Solution Items" altında bulunan kısımdan **.testsettings** dosyası açılır. **"Roles"** tabından **"Test Execution Method"** dropdowndan **"Remote Execution"** seçilir ve karşısında test controller seçilir. Aşağıda bulunan **Roles** kısımında ise **<All agents on the controller ve yes** olmasına dikkat edin ve Apply tıklayın.
  - Ve testi çalıştırın.

# Dikkat Edilecekler
- Testler localde çalıştırılmak istenirse **"Solution Items"** altında **"Local.testsettings"** dosyasında **"Roles"** tabından **"Local Execution"** seçilmelidir. Aksi halde testler Skipped olarak kalır. Testler agentlarda çalıştırılmak istenirse **"Remote Execution"** seçilir.
- Host dosyasında herhangi bir kayıt olmamalı
- Firewall kapalı olabilir
- PC'ler birbirlerine erişebilir olmalı
- Kullanıcılar local admin olmalı
- Sonuçlar veri tabanına yazılmak istenirse sql sunucu olarak **"(localdb)\MsSqlLocalDb"** tablo olarak ise **"LoadTest2010"** kullanılır.
- Agent bilgisayarlardan **"Performance Counter"** gelmezse yapılacaklar
  - **"Remote Procedure Call (RPC)"** ve **"RemoteRegistry"** servisi çalışıyor olmalı.
  - **"WMI Performance Adapter"**, **"Performance Counter DLL Host"**, **"Performance Logs and Alerts"**, **"Remote Procedure Call (RPC) Locator"** opsiyonel
  - **"Performance Log Users"** ve **"Performance Monitor Users"** kullanıcı gruplarına bağlanacak olan kullanıcı eklenmeli.
  - Firewall kapalı olsa bile **"Inbound Rules"** kısmına **"Performance Logs and Alerts (DCOM-In)"**, **"Performance Logs and Alerts (TCP-In)"** seçenekleri enable edilmeli (toplamda 4 adet).
  - Test için komut satırında **"TypePerf -q -s {BilgisayarAdı}"** kullanılır.
