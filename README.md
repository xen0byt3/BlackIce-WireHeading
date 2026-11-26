# Operation Compass: Kali Linux Command Lab (Activity Workflow)

**Course material by xen0byt3 | Black Ice WireHeading**

A hands-on, white-hat reconnaissance activity. You will type 30+ essential Linux and networking commands in Kali, in the order a real assessor would use them, prove each result with an audit check, capture screenshots, and submit everything through your own GitHub repository that you share with **xen0byt3**.

> **Ethics first.** Only test the authorized targets listed below. Never run these commands against a system you do not own or lack written permission to test.

| Authorized target | Notes |
|---|---|
| `scanme.nmap.org` | Provided by the Nmap Project for scan practice. Light scans only. |
| `example.com` | Fallback web target. |
| Your own lab network | Host-only or NAT network you control. `arp-scan` runs here only. |

---

## 1. What you need

- A Kali Linux VM with internet access (NAT or Bridged)
- A **terminal emulator**: Applications menu > Terminal Emulator, or `Ctrl+Alt+T`. Open it **before** you start any task. Every command is typed in the terminal, not in the website.
- The activity website: `index.html` (open it in a browser)
- A GitHub account
- Git (preinstalled on Kali. If not: `sudo apt install git`)
- The screenshot tool (`PrintScreen`, or the Screenshot app). Images are saved in `~/Pictures`.

## 2. How the activity website works

1. Open `index.html`. Wait for the 20 second Black Ice WireHeading splash and progress bar.
2. Work through **Phase 0 to Phase 11** in order. Each step shows the command, an explanation, and an **Audit** box that tells you which command to **RUN** to check your work and what to **EXPECT**.
3. Type every command yourself. Commands are locked from copying, except decode strings and some sed and regex patterns.
4. At the end of each phase, pass the **phase quiz**. **One wrong answer restarts the quiz from question 1.** The next phase unlocks only after every question is correct.
5. After the last quiz the Final output page unlocks.

## 3. Phase overview

| Phase | Goal | Main commands |
|---|---|---|
| 0 | Build the workspace on the Desktop | `pwd` `ls` `cd` `mkdir` `touch` `echo` |
| 1 | Know your own machine | `ip a` `hostname -I` `ip route` |
| 2 | Local network recon | `ping` `arp-scan --localnet` `ss` / `netstat` `sudo` |
| 3 | Public intelligence | `whois` `nslookup` `dig` |
| 4 | Reachability and path | `ping` `traceroute` |
| 5 | Port and service scanning | `nmap` `nmap -sV` `nmap -A` |
| 6 | Talking to the services | `curl` `wget` `nc` |
| 7 | Capture your own traffic | `tcpdump -i eth0` + Wireshark |
| 8 | Extract the findings | `cat` `head` `tail` `grep` `sed` regex `cp` `mv` |
| 9 | Decode the instructor messages | `base64 --decode` |
| 10 | Build the final report | `chmod` script, `cp` `sed -i` `mv` |
| 11 | Clean-up | `rm` `shred` |

---

## 4. Setup before Phase 0 (GitHub repository)

### 4.1 Create your repository

1. On GitHub click **New repository**.
2. **Repository name** (exact format): `kali-command-lab-<Lastname>-<Firstname>`
   Example: `kali-command-lab-Dela-Cruz-Juan`
3. Set visibility to **Private** (your lab IP addresses and report are inside).
4. Tick **Add a README file**, then **Create repository**.

### 4.2 Add xen0byt3 as a collaborator

1. Open your repository > **Settings** > **Collaborators** (or **Collaborators and teams**).
2. Click **Add people**, type **`xen0byt3`**, select the account and choose **Write** access.
3. Click **Add**. Reviewing starts when xen0byt3 accepts the invitation, so send the repo link to the instructor too.

### 4.3 Clone and create your working branch

Open the terminal (this is the only part done before Phase 0):

```bash
cd ~/Desktop
git clone https://github.com/<your-username>/kali-command-lab-<Lastname>-<Firstname>.git
cd kali-command-lab-<Lastname>-<Firstname>
git config user.name "Your Full Name"
git config user.email "your-github-email@example.com"
git checkout -b submission
mkdir -p screenshots evidence
```

> GitHub no longer accepts account passwords for `git push`. When asked for a password, paste a **Personal Access Token** (GitHub > Settings > Developer settings > Personal access tokens, with `repo` scope), or sign in with `gh auth login`. Never upload your token to the repository.

Your lab folder (`~/Desktop/kali-lab`) is created later, in Phase 0, and stays separate from this repository folder.

### 4.4 Open a draft Pull Request

After your first push (section 6) open a **Draft Pull Request** from `submission` to `main`, set the title to `Submission: <Lastname>, <Firstname>`, and request a review from **xen0byt3**. Keep pushing to the same branch. xen0byt3 will review and comment on the Pull Request.

---

## 5. Required screenshots and file naming

### 5.1 Screenshot rules

- Format **PNG**. Save into the repository's `screenshots/` folder.
- Capture the **whole terminal window**, never a crop of only the output.
- The **prompt (`kali@kali`), the command you typed, and its output** must all be visible and readable.
- If a command's result is shown by its **Audit** check, include the audit command in the same screenshot.
- Blur nothing. Screenshots with edited or missing content are not accepted.
- Name format (exact): `P<phase>-S<step>_<short-description>.png`
  Phase and step use two digits. Words in the description are separated by hyphens, lowercase.

### 5.2 Screenshot checklist (46 files)

| # | File name | What the screenshot must show |
|---|---|---|
| 1 | `P00-S01_pwd-desktop.png` | `cd ~/Desktop` then `pwd` ending in `/Desktop` |
| 2 | `P00-S02_kali-lab-created.png` | `mkdir kali-lab`, `cd kali-lab`, `pwd`, and `ls ..` listing `kali-lab` |
| 3 | `P00-S03_ls-structure.png` | `ls` showing the five folders and `ls -l` showing `notes.txt` and `targets.txt` |
| 4 | `P00-S04_cat-targets.png` | `cat targets.txt` printing `scanme.nmap.org` |
| 5 | `P00-QUIZ_passed.png` | Website: Phase 0 quiz showing **Quiz passed** |
| 6 | `P01-S01_ip-a.png` | `ip a` with `eth0` and its inet address |
| 7 | `P01-S02_ip-route-gateway.png` | `ip route`, then `echo $GW` printing the gateway |
| 8 | `P01-S03_ls-recon-route.png` | `ls recon` and `cat recon/route.txt` |
| 9 | `P01-QUIZ_passed.png` | Website: Phase 1 **Quiz passed** |
| 10 | `P02-S01_ping-gateway.png` | `ping -c 4 $GW` with the 0% packet loss line |
| 11 | `P02-S02_arp-scan.png` | `sudo arp-scan --localnet` result and `ls -l recon` |
| 12 | `P02-S03_ss-listening.png` | `ss -tulnp` output and `ls -l recon` showing `listening_ports.txt` |
| 13 | `P02-QUIZ_passed.png` | Website: Phase 2 **Quiz passed** |
| 14 | `P03-S01_whois.png` | `whois nmap.org` filtered output (registrar, dates, name servers) |
| 15 | `P03-S02_nslookup-dig.png` | `nslookup scanme.nmap.org` and `dig scanme.nmap.org +short` |
| 16 | `P03-S03_target-ip.png` | `echo $TARGET`, `cat recon/target_ip.txt`, `ls recon` |
| 17 | `P03-QUIZ_passed.png` | Website: Phase 3 **Quiz passed** |
| 18 | `P04-S01_ping-target.png` | `ping -c 4 scanme.nmap.org` |
| 19 | `P04-S02_traceroute.png` | `cat recon/traceroute.txt` and `ls -l recon` |
| 20 | `P04-QUIZ_passed.png` | Website: Phase 4 **Quiz passed** |
| 21 | `P05-S01_nmap-basic.png` | `nmap scanme.nmap.org` result and `ls -l scans` |
| 22 | `P05-S02_nmap-sv.png` | `cat scans/nmap_sv.txt` with ports 22 and 80 and versions |
| 23 | `P05-S03_nmap-aggressive.png` | `sudo nmap -A -p 22,80 ...` and `ls -l scans` |
| 24 | `P05-QUIZ_passed.png` | Website: Phase 5 **Quiz passed** |
| 25 | `P06-S01_curl-headers.png` | `curl -I http://scanme.nmap.org` with the Server header |
| 26 | `P06-S02_wget-ls.png` | `wget` command and `ls -l recon` showing `scanme_index.html` |
| 27 | `P06-S03_nc-banner.png` | `nc -v -w 3 scanme.nmap.org 22` banner and the hand-typed HTTP request |
| 28 | `P06-QUIZ_passed.png` | Website: Phase 6 **Quiz passed** |
| 29 | `P07-S01_tcpdump-capture.png` | Both terminals: tcpdump capturing and the `curl -sI` run, plus `ls -l captures` |
| 30 | `P07-S02_pcap-read.png` | `tcpdump -nn -r captures/web.pcap` with `head` and `tail`, handshake flags visible |
| 31 | `P07-S03_wireshark-tcp-port-80.png` | Wireshark with `captures/web.pcap` open and the filter `tcp.port == 80` applied (green bar) |
| 32 | `P07-QUIZ_passed.png` | Website: Phase 7 **Quiz passed** |
| 33 | `P08-S01_grep-sed-open-ports.png` | The `grep -E` and `sed` command with the clean open-port lines |
| 34 | `P08-S02_ip-regex.png` | `grep -oE` IPv4 regex output and `grep -c` count |
| 35 | `P08-S03_cp-mv-audit.png` | `cp`, `mv`, then `ls reports` and `ls scans` |
| 36 | `P08-QUIZ_passed.png` | Website: Phase 8 **Quiz passed** |
| 37 | `P09-S01_flag-decoded.png` | `base64 --decode` of the flag showing `FLAG{...}` |
| 38 | `P09-S02_bonus-decoded.png` | The bonus decode showing the readable message |
| 39 | `P09-QUIZ_passed.png` | Website: Phase 9 **Quiz passed** |
| 40 | `P10-S01_script-chmod.png` | `ls -l build_report.sh` before and after `chmod +x` |
| 41 | `P10-S02_final-report.png` | `./build_report.sh` and `cat reports/final_report.txt` |
| 42 | `P10-S03_client-report.png` | `cat reports/FINAL_REPORT_CLIENT.txt` with IPs shown as `x.x.x.x` and `ls -l reports` |
| 43 | `P10-QUIZ_passed.png` | Website: Phase 10 **Quiz passed** |
| 44 | `P11-S01_cleanup-verified.png` | `shred` output, then `ls captures`, `ls loot` and `ls -R` showing the report remains |
| 45 | `P11-QUIZ_passed.png` | Website: Phase 11 **Quiz passed** |
| 46 | `FINAL_all-phases-passed.png` | Website: **Mission progress 12 of 12 phases cleared** (sidebar ring) |

> **Take screenshots as you go.** Phase 11 shreds `captures/web.pcap` and `loot/flag.b64`, so their content cannot be recovered afterwards.

### 5.3 Evidence files to upload

After Phase 11, copy the files that remain in your lab folder into the repository. Do **not** try to upload the shredded `web.pcap` or `flag.b64` (they no longer exist).

```bash
cd ~/Desktop/kali-command-lab-<Lastname>-<Firstname>
cp -r ~/Desktop/kali-lab/recon ~/Desktop/kali-lab/scans ~/Desktop/kali-lab/reports evidence/
cp ~/Desktop/kali-lab/notes.txt ~/Desktop/kali-lab/targets.txt ~/Desktop/kali-lab/build_report.sh evidence/
```

Expected evidence after clean-up (file names are produced by the lab, do not rename them):

| File | Comes from |
|---|---|
| `evidence/recon/my_ip.txt`, `route.txt`, `arp_scan.txt`, `listening_ports.txt`, `target_ip.txt`, `traceroute.txt` | Phases 1 to 4 |
| `evidence/scans/nmap_sv.txt`, `nmap_aggressive.txt` | Phase 5 |
| `evidence/reports/nmap_sv_evidence.txt` | Phase 8 |
| `evidence/reports/final_report.txt` | Phase 10 |
| `evidence/reports/FINAL_REPORT_CLIENT.txt` | Phase 10 |
| `evidence/build_report.sh`, `notes.txt`, `targets.txt` | Phases 0 and 10 |

### 5.4 Final repository structure

```text
kali-command-lab-<Lastname>-<Firstname>/
├── README.md              (created by GitHub, you may leave it)
├── SUBMISSION.md          (you fill this in, template in section 7)
├── screenshots/
│   ├── P00-S01_pwd-desktop.png
│   ├── ... (46 files in total)
│   └── FINAL_all-phases-passed.png
└── evidence/
    ├── notes.txt
    ├── targets.txt
    ├── build_report.sh
    ├── recon/
    ├── scans/
    └── reports/
```

---

## 6. Upload and collaboration workflow (every phase)

Commit and push **after every phase** so xen0byt3 can follow your progress on the Pull Request.

```bash
cd ~/Desktop/kali-command-lab-<Lastname>-<Firstname>
mv ~/Pictures/<your-screenshot>.png screenshots/P03-S01_whois.png   # rename to the exact name
git add screenshots
git commit -m "P03: add whois, dig and target IP screenshots"
git push -u origin submission
```

| Rule | Example |
|---|---|
| One commit per finished phase | `P05: add nmap basic, version and aggressive scans` |
| Commit message starts with the phase code | `P00:` ... `P11:` |
| Final commit for the evidence files | `FINAL: add evidence files and SUBMISSION.md` |
| Push to the `submission` branch only | `git push origin submission` |

When all phases are done:

1. Add the evidence files (section 5.3) and your `SUBMISSION.md` (section 7).
2. Commit and push: `git add . && git commit -m "FINAL: add evidence files and SUBMISSION.md" && git push`
3. On GitHub, change the Pull Request from **Draft** to **Ready for review**.
4. Reply to any review comment from **xen0byt3** by pushing fixes to the same branch. Do not delete or recreate the repository.
5. Create the tag: `git tag v1.0-final && git push origin v1.0-final`

**Do not upload:** your Personal Access Token, passwords, or any file outside the lab (browser data, other courses).

---

## 7. `SUBMISSION.md` template

Create `SUBMISSION.md` in the repository root with this content:

```markdown
# Submission: Operation Compass

- Name:
- Section / Class:
- GitHub username:
- Repository: kali-command-lab-<Lastname>-<Firstname>
- Kali version (cat /etc/os-release):
- Interface used (eth0 or other):
- Date completed:

## Result summary
- Gateway IP:
- Target IP of scanme.nmap.org:
- Open ports found:
- Web server and version (curl Server header):
- Decoded flag:
- Decoded bonus message:
- Packets captured in web.pcap:

## Problems I met and how I fixed them
1.
2.

## Reflection (3 to 5 sentences)
What was the quietest command, what was the noisiest, and what would a defender see?

## Honor statement
I typed every command myself, used only the authorized targets, and these files are my own work.
Signed: ____________
```

---

## 8. Grading rubric (100 points)

| Criteria | Points |
|---|---|
| Screenshots: all 46 present, correctly named, full terminal visible | 40 |
| Evidence files present and consistent with the screenshots | 15 |
| Audit proof: audit command and expected output visible in the screenshots | 10 |
| Quizzes: all 12 phases passed (screenshots plus the Mission progress image) | 10 |
| Final report and client report correct (IPs redacted in the client copy) | 10 |
| GitHub workflow: private repo, collaborator, branch, per-phase commits, Pull Request | 10 |
| `SUBMISSION.md` complete and honest | 5 |

Deductions: wrong file names (-1 each), cropped or unreadable screenshots (-2 each), missing collaborator access (-5), copied or edited screenshots (**zero for the activity**).

---

## 9. Troubleshooting

| Problem | Fix |
|---|---|
| `Permission denied` on `arp-scan`, `nmap -A`, `tcpdump` | Add `sudo` |
| `traceroute: command not found` | `sudo apt install traceroute` |
| `netstat: command not found` | `sudo apt install net-tools` or use `ss -tulnp` |
| tcpdump shows no packets | Use your real interface name (check `ip a`) instead of `eth0` |
| `echo $GW` prints an empty line | Re-run `ip route` and retype the `GW=$(...)` command carefully |
| `git push` asks for a password | Use a Personal Access Token or `gh auth login` |
| `git push` rejected | `git pull --rebase origin submission`, then push again |
| Quiz restarted | One wrong answer resets the quiz. Re-read the step and answer again |
| `scanme.nmap.org` unreachable | Use `example.com` for DNS, `curl` and `wget`, and tell your instructor |

---

## 10. Quick reference: final checklist

- [ ] Terminal emulator opened before starting
- [ ] Private repo `kali-command-lab-<Lastname>-<Firstname>` created
- [ ] **xen0byt3** added as collaborator (Write)
- [ ] Working on branch `submission`, Draft Pull Request opened
- [ ] 12 phases completed, 12 quizzes passed
- [ ] 46 screenshots named exactly as in section 5.2
- [ ] Evidence files uploaded, `SUBMISSION.md` complete
- [ ] Pull Request marked Ready for review and tag `v1.0-final` pushed

*Black Ice WireHeading | developed by xen0byt3 | Authorized, ethical, white-hat practice only.*
