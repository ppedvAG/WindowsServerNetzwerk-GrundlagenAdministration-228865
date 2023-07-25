# Hilfreiche Powershell cmdlets und CMD Kommandows

---

Mit dem folgenden Befehl lässt sich der Server oder auch Client via PowerShell umbenennen und automatisch neustarten

```powershell
Rename-Computer -NewNAme Server1 -Restart 
```
---

Mit dem folgenden Befehl lässt sich in einer cmd oder einer PowerShell die DNS Registration manuell anstoßen.

```cmd
ipconfig /registerdns
```

---
Mit dem folgenden PowerShell cmdlet ist es möglich das Gerät der Domäne zu joinen und gleichzeitig mit umzubennen der Neustart muss manuell ausgelöst werden.

```powershell
Add-Computer -DomainName ppedv.test -NewName Server2
```
---
Mit dem folgenden PowerShell cmdlet lässt sich eine Remote PowerShell Verbindung herstellen

```powershell
Enter-PSSession -ComputerName Server2.ppedv.test
```
---
Mit dem folgenden PowerShell cmdlet lässt sich der Datendedpluzierungsjob "Optimization" für das Volume F manuell starten. 
```powershell
Start-DedupJob -Volume F: -Type Optimization
```
--- 
Mit dem folgenden cmdlet lässt sich der Status aller eventuell vorhanden Dedupjobs abfragen
```powershell
Get-Dedupjob
```
---
Mit dem folgenden Powershell cmdlet lässt sich die Rolle des DateiServers und des Speicherreplikat sowie die benötigten Verwaltungstools automatisch auf mehreren angegeben Servern installieren. Sollte ein Neustart notwendig sein wird dieser automatisch ausgelöst. 
```powershell
Invoke-Command -ComputerName Server2,Server3 {Install-WindowsFeature -Name FS-FileServer,Storage-Replica -IncludeManagementTools -Restart}
```
---
Konfigurieren der IP Adresse , SubnetzMaske und DNS Server über die PowerShell
Als erstes muss man den Index zu konfigurierenden Interfaces festellen über den Befehl
```powershell
Get-NetAdapter
```
Dort sollte man dann das richtige Interface auswählen da die Interface Index Nr im folgenden Befehel benötigt wird. In diesem Beispiel wird für das Interface mit der Index Nummer 4 die IP Adresse **192.168.10.3** mit einer Präfix von **24** gesetzt was einer Subnetzmaske von **255.255.255.0** entspricht. Es wäre hier auch möglich mit einem weiteren Parameter das Standardgateway anzugeben was in diesem Falle hier aber entfällt.
```powershell
New-NetIPAddress -InterfaceIndex 4 -IPAddress 192.168.10.3 -PrefixLength 24 
```
Anschließend muss lediglich noch dem DNS Client die Adresse des bevorzugten DNS Servers mitgeteilt werden. Auch wird wieder die InterfaceIndex Nummer verwendet. Ein zweiter DNS würde hier mit einem Komma angegeben hinter der ersten Adresse.
```powershell
Set-DnsClientServerAddress -InterfaceIndex 4 -ServerAddresses 192.168.10.1
```
---

---
Mit dem folgenden PowerShell cmdlet lassen sich alle über SMB geöffneten Dateien anzeigen
```powershell
Get-SmbOpenFile
```
Mit dem folgenden PowerShell cmdlet lassen sich alle aktiven SMB Verbindungen darstellen
```powershell
Get-SmbSession
```
---
Mit diesem Befehehl wird Sysprep im Modus verallgemeinern ausgeführt und aktivieren des OOBE. In der Unattend Datei kann hinterlegt sein das das Administrator Profil zum Defaul Profil kopiert wird.
```cmd
C:\Windows\System32\Sysprep\sysprep.exe /generalize /oobe /shutdown /unattend:F:\Copyprofile.xml
```
---
```powershell
Resolve-DNSName -Name server1
# zum Abfragen von DNS Records vom Standard Server, der RecordType und Server könnten über Parameter angegeben werden
Clear-DnsClientCache
# zum löschen des DNS CAch
Get-DnsClientCache
# zum anzeigen des aktuellen Inhalts des DNS Caches
```



