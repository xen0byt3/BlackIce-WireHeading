# Operation Compass: Kali Linux Command Lab (Activity Workflow)

**By xen0byt3 | Black Ice WireHeading**

Type 30+ Linux and networking commands in Kali, in the order a real assessor would use them, audit each result, then submit your proof through your own GitHub repository shared with **xen0byt3**.

> **Ethics first.** Test only `scanme.nmap.org`, `example.com` and your own lab network. Never run these commands on a system you do not own or lack written permission to test.

## 1. Before you start

- Kali Linux VM with internet access
- A **terminal emulator open first** (Applications menu > Terminal Emulator, or `Ctrl+Alt+T`). Type every command there, not in the website.
- The activity website `blackiceWIREHEADING.html`, a GitHub account, and Git (`sudo apt install git` if missing)

## 2. How to work

1. Open `blackiceWIREHEADING.html`, wait for the 20 second splash, then do **Phase 0 to Phase 11** in order.
2. Type each command yourself. Use the **Audit** box (RUN and EXPECT) to confirm it worked.
3. Pass each phase quiz. **One wrong answer restarts the quiz.** The next phase unlocks only when all answers are correct.

## 3. GitHub setup (once, before Phase 0)

1. Create a **private** repository named `kali-command-lab-<Lastname>-<Firstname>` with **Add a README file** ticked.
2. **Settings > Collaborators > Add people > `xen0byt3`** (Write access).
3. Clone it and create your branch:

```bash
cd ~/Desktop
git clone https://github.com/<your-username>/kali-command-lab-<Lastname>-<Firstname>.git
cd kali-command-lab-<Lastname>-<Firstname>
git config user.name "Your Full Name"
git config user.email "your-github-email@example.com"
git checkout -b submission
mkdir -p screenshots evidence
```

Use a **Personal Access Token** (or `gh auth login`) when Git asks for a password. Never upload the token.

## 4. Required screenshots (13 only)

Take **one screenshot per phase** of the whole terminal window. The prompt (`kali@kali`), the command you typed and its output must be visible and readable. Include the audit command if it proves the result. Save as PNG in `screenshots/` with the exact name below.

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

## 5. Commit after every phase

```bash
cd ~/Desktop/kali-command-lab-<Lastname>-<Firstname>
mv ~/Pictures/<your-screenshot>.png screenshots/P03_dns-target-ip.png
git add screenshots
git commit -m "P03: add DNS and target IP screenshot"
git push -u origin submission
```

- One commit per phase, message starts with `P00:` to `P11:`.
- After your first push, open a **Draft Pull Request** (`submission` to `main`), title `Submission: <Lastname>, <Firstname>`, and request a review from **xen0byt3**.

## 6. Final upload (after Phase 11)

Copy the files that remain in your lab folder. Do not upload the shredded `web.pcap` or `flag.b64`.

```bash
cd ~/Desktop/kali-command-lab-<Lastname>-<Firstname>
cp -r ~/Desktop/kali-lab/recon ~/Desktop/kali-lab/scans ~/Desktop/kali-lab/reports evidence/
cp ~/Desktop/kali-lab/notes.txt ~/Desktop/kali-lab/targets.txt ~/Desktop/kali-lab/build_report.sh evidence/
```

Add `SUBMISSION.md` (template below), then:

```bash
git add .
git commit -m "FINAL: add evidence files and SUBMISSION.md"
git push
git tag v1.0-final && git push origin v1.0-final
```

Mark the Pull Request **Ready for review** and answer xen0byt3's comments by pushing fixes to the same branch.

Final repository layout:

```text
kali-command-lab-<Lastname>-<Firstname>/
├── SUBMISSION.md
├── screenshots/   (13 PNG files named as in section 4)
└── evidence/      (notes.txt, targets.txt, build_report.sh, recon/, scans/, reports/)
```

## 7. `SUBMISSION.md` template

```markdown
# Submission: Operation Compass

- Name / Section:
- GitHub username:
- Gateway IP:
- scanme.nmap.org IP:
- Open ports found:
- Decoded flag:
- Decoded bonus message:
- Problems met and fixes (1 to 2 lines):
- Honor statement: I typed every command myself and used only the authorized targets.
```

## 8. Grading (100 points)

| Criteria                                                                             | Points |
| ------------------------------------------------------------------------------------ | ------ |
| 13 screenshots present, correctly named, full terminal readable                      | 40     |
| Evidence files present and consistent with the screenshots                           | 20     |
| Final report and client report correct (IPs shown as `x.x.x.x`)                      | 10     |
| All 12 quizzes passed (`FINAL_all-phases-passed.png`)                                | 10     |
| GitHub workflow: private repo, collaborator, branch, per-phase commits, Pull Request | 15     |
| `SUBMISSION.md` complete                                                             | 5      |

Wrong file name: -1 each. Cropped or edited screenshot: -2 each. Missing collaborator access: -5.

## 9. Quick fixes

- `Permission denied`: add `sudo`.
- `traceroute` or `netstat` not found: `sudo apt install traceroute net-tools`.
- No packets in tcpdump: use your real interface name from `ip a` instead of `eth0`.
- `git push` rejected: `git pull --rebase origin submission`, then push again.
