# Operation Compass: Kali Linux Command Lab (Activity Workflow)

**By xen0byt3 | Black Ice WireHeading**

Type 30+ Linux and networking commands in Kali, in the order a real assessor would use them, audit each result, then submit your proof through your own GitHub repository shared with **xen0byt3**.

> **Ethics first.** Test only `scanme.nmap.org`, `example.com` and your own lab network. Never run these commands on a system you do not own or lack written permission to test.

## 1. Before you start

- **Option A:** a Kali Linux VM with internet access.
- **Option B (low-spec laptop or PC):** the browser-based **Practice Terminal**, `kali-terminal.html` (also the **Practice Terminal** button in the lab website). Nothing to install, it works offline, and `sudo` password is `kali`. Its network results (`nmap`, `dig`, `whois`, `traceroute` and others) are simulated samples, and your practice files are saved in your browser.
- A **terminal emulator open first**. Kali: Applications menu > Terminal Emulator, or `Ctrl+Alt+T`. Practice Terminal: the terminal is on the page (click **+** for the second tab in Phase 7). Type every command there, not in the lab website.
- The activity website `index.html` and a GitHub account. Git is needed only on Kali (`sudo apt install git` if missing); on a low-spec machine you can upload through the GitHub website (section 3).

## 2. How to work

1. Open `index.html`, wait for the 20 second splash, then do **Phase 0 to Phase 11** in order.
2. Type each command yourself. Use the **Audit** box (RUN and EXPECT) to confirm it worked.
3. Pass each phase quiz. **One wrong answer restarts the quiz.** The next phase unlocks only when all answers are correct.

## 4. Required screenshots (13 only)

Take **one screenshot per phase** of the whole terminal window (Practice Terminal screenshots are accepted). The prompt (`kali㉿kali`), the command you typed and its output must be visible and readable. Include the audit command if it proves the result. Save as PNG in `screenshots/` with the exact name below.

| Phase | File name                       | Must show                                                                                       |
| ----- | ------------------------------- | ----------------------------------------------------------------------------------------------- |
| 0     | `P00_workspace.png`             | `pwd` ending in `/Desktop/kali-lab` and `ls` showing the folders, `notes.txt` and `targets.txt` |
| 1     | `P01_ip-gateway.png`            | `ip a` (eth0 address) and `echo $GW`                                                            |
| 2     | `P02_arp-scan.png`              | `sudo arp-scan --localnet` result                                                               |
| 3     | `P03_dns-target-ip.png`         | `dig scanme.nmap.org +short` and `cat recon/target_ip.txt`                                      |
| 4     | `P04_traceroute.png`            | `cat recon/traceroute.txt`                                                                      |
| 5     | `P05_nmap-sv.png`               | `cat scans/nmap_sv.txt` with ports 22 and 80 and versions                                       |
| 6     | `P06_curl-headers.png`          | `curl -I http://scanme.nmap.org` with the Server header                                         |
| 7     | `P07_wireshark-tcp-port-80.png` | Wireshark with `captures/web.pcap` open and the filter `tcp.port == 80` applied                 |
| 8     | `P08_open-ports-sed.png`        | The `grep -E` and `sed` command with the clean open-port lines                                  |
| 9     | `P09_decoded-flag.png`          | Both `base64 --decode` results (the `FLAG{...}` and the bonus message)                          |
| 10    | `P10_final-report.png`          | `./build_report.sh` and `cat reports/final_report.txt`                                          |
| 11    | `P11_cleanup-verified.png`      | `ls captures`, `ls loot` and `ls -R` after `shred`                                              |
| Final | `FINAL_all-phases-passed.png`   | Website sidebar: **12 of 12 phases cleared**                                                    |

> Take each screenshot **as you finish the phase**. Phase 11 shreds `web.pcap` and `flag.b64`, so they cannot be recovered later.

## 7. SUBMISSION template

# Submission: Operation Compass

- Name:
- GitHub username:
- Environment used (Real Kali VM or Practice Terminal):
- Gateway IP:
- scanme.nmap.org IP:
- Open ports found:
- Decoded flag:
- Decoded bonus message:
- Problems met and fixes (1 to 2 lines):
- Practice Terminal users: paste final_report.txt and FINAL_REPORT_CLIENT.txt below
- Honor statement: I typed every command myself and used only the authorized targets.
- The Required screenshots

````
1. Create a  repository named `kali-command-lab-<yourcyberhandle>
2. **Settings > Collaborators > Add people > `xen0byt3`** (Write access).
````
