# 🧪 Lab 5 – Java, apt vs apt-get, snap, GUI, Vim on Ubuntu Server

**Estimated Duration:** 3 hours

**Instructions:** Complete all practical tasks in a repository named `Lab5`. When finished, push your work to a repository named `CC_<student_Name>_<student_roll_number>`.

## 🎯 Objective

This lab demonstrates package discovery and management on an Ubuntu Server (CLI) and installing/configuring a lightweight GUI for remote access. You will:

- Discover how the system suggests packages when a command is not installed (example: `java`).
- Install and remove Java using both `apt` and `apt-get`.
- Learn the difference between `apt update` and `apt upgrade`.
- Install an application with snap on a headless server and verify installation.
- Install and configure a lightweight GUI (XFCE) and XRDP for remote desktop access, and control GUI login behavior at boot.
- Launch and visually verify Visual Studio Code in the GUI (Task 5 & Task 6).
- Add a third-party apt repository and signing key to install Google Chrome (Task 7).
- Add PPAs and install GUI apps (Task 8).
- Create a simple Kubernetes Pod manifest using vim (Task 9).
- Edit the Kubernetes manifest to add an annotation and practice discarding edits (Task 10).
- Practice basic vim editing commands (Task 11).
- Practice vim search, navigation, substitution and undo (Task 12).

## 🧩 Prerequisites

- An Ubuntu Server VM (the same VM used in Lab 4 is fine).
- SSH access to the VM from your host terminal.
- Sudo privileges on the VM.

> Note: Installing a GUI and third-party packages increases disk and memory usage. Ensure the VM has sufficient resources.

## 📋 Task List

1. Task 1: Discover missing command & install Java using apt suggestion
2. Task 2: Install & remove Java using apt-get (explicitly)
3. Task 3: apt update vs apt upgrade — run & explain
4. Task 4: Install Visual Studio Code via snap on CLI and verify (DO NOT remove Code)
5. Task 5: Install XFCE GUI + XRDP — minimal desktop and remote access (GUI) and launch VS Code
6. Task 6: Install lightdm-gtk-greeter and GUI verification — start GUI, open VS Code, take snapshot, then end (GUI)
7. Task 7: Install Google Chrome by adding its apt source & key (Chrome)
8. Task 8: Install applications via PPA (Audacity & OBS) and launch
9. Task 9: Create a Kubernetes sample YAML using vim
10. Task 10: Edit the Kubernetes YAML — add annotation, verify, then discard temporary change
11. Task 11: Vim editing practice — delete, undo, numeric deletes, navigation
12. Task 12: Vim search, add matches, substitute, undo

---

## Task 1 – Discover missing command & install Java using apt suggestion

Run `java` when it is not installed, follow the system suggestion to install a Java package using apt, verify, then remove and clear the shell cache.

```bash
java
sudo apt install <suggested-package> -y
java --version
sudo apt remove <suggested-package> -y
java
hash -r
java
```

📸 Screenshots: `task1_java_suggestion.png`, `task1_java_install.png`, `task1_java_version.png`, `task1_java_remove.png`, `task1_java_not_found.png`, `task1_hash_clear.png`

## Task 2 – Install & remove Java using apt-get

```bash
sudo apt-get update
sudo apt-get install default-jre -y
java --version
sudo apt-get remove default-jre -y
hash -r
java
```

📸 Screenshots: `task2_aptget_install.png`, `task2_java_version_after_aptget.png`, `task2_aptget_remove.png`, `task2_hash_after_remove.png`

## Task 3 – apt update vs apt upgrade

```bash
sudo apt update
sudo apt upgrade
nano ~/apt_update_vs_upgrade.md   # write 3-5 sentences
```

📸 Screenshots: `task3_apt_update.png`, `task3_apt_upgrade.png`, `task3_explanation.png`

## Task 4 – Install Visual Studio Code via snap on CLI and verify (DO NOT remove Code)

```bash
sudo snap install --classic code
snap list code
code --version
ls -l /snap/bin | grep code
```

📸 Screenshots: `task4_snap_install.png`, `task4_snap_list.png`, `task4_code_version_or_info.png`, `task4_snap_bin_location.png`

## Task 5 – Install XFCE GUI + XRDP and launch VS Code

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install xfce4 xfce4-goodies -y
sudo apt install xrdp -y
sudo systemctl enable --now xrdp
sudo systemctl status xrdp
echo xfce4-session > ~/.xsession
```

Connect with Remote Desktop (mstsc) to the server IP, log in with your Ubuntu user, and launch `code` from a terminal in the GUI.

📸 Screenshots: `task5_update.png`, `task5_xfce_install.png`, `task5_xrdp_enable.png`, `task5_xrdp_status.png`, `task5_xsession.png`, `task5_rdp_connect.png`, `task5_vscode_launch.png`

## Task 6 – Install lightdm-gtk-greeter and GUI verification

```bash
sudo apt install lightdm lightdm-gtk-greeter -y
sudo mkdir -p /etc/lightdm/lightdm.conf.d
echo -e "[Seat:*]\ngreeter-session=lightdm-gtk-greeter\nuser-session=xfce\nautologin-user-timeout=0" | sudo tee /etc/lightdm/lightdm.conf.d/99-xfce.conf
sudo rm -f /var/lib/lightdm/.Xauthority
sudo rm -f ~/.Xauthority
sudo rm -rf ~/.cache/sessions
sudo chown -R $USER:$USER /home/$USER
sudo systemctl restart lightdm
```

Enable the GUI at boot, reboot and observe, then disable it, reboot and observe:

```bash
sudo systemctl enable lightdm
sudo systemctl set-default graphical.target
sudo reboot

sudo systemctl set-default multi-user.target
sudo systemctl disable lightdm
sudo reboot
```

Start and stop the GUI manually (no reboot), then launch VS Code in the GUI and close it:

```bash
sudo systemctl start lightdm
sudo systemctl stop lightdm
sudo systemctl start lightdm
code
```

📸 Screenshots: `task6_lightdm_install.png`, `task6_lightdm_config.png`, `task6_lightdm_cleanup.png`, `task6_lightdm_restart.png`, `task6_gui_enable_boot.png`, `task6_after_reboot_gui.png`, `task6_gui_disable_boot.png`, `task6_after_reboot_cli.png`, `task6_gui_start.png`, `task6_gui_stop.png`, `task6_gui_start_command.png`, `task6_vscode_launch.png`

## Task 7 – Install Google Chrome by adding its apt source & key

1. Try `sudo apt install google-chrome-stable -y` first (it fails). Screenshot the error.
2. Inspect `/etc/apt`, `/etc/apt/sources.list`, `/etc/apt/sources.list.d/` and `ubuntu.sources`.
3. Append the Chrome stanza to `/etc/apt/sources.list.d/ubuntu.sources`:

```
Types: deb
URIs:  http://dl.google.com/linux/chrome/deb/
Suites: stable
Components: main
Architectures: amd64
Signed-By: /etc/apt/keyrings/google.gpg
```

4. Add the signing key, update and install:

```bash
curl -fsSL https://dl.google.com/linux/linux_signing_key.pub | sudo gpg --dearmor -o /etc/apt/keyrings/google.gpg
sudo apt update
sudo apt install google-chrome-stable -y
```

5. Alternate (preferred) method: remove Chrome, remove the stanza and key, then:

```bash
echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/google.gpg] http://dl.google.com/linux/chrome/deb/ stable main" | sudo tee /etc/apt/sources.list.d/google-chrome.list > /dev/null
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://dl.google.com/linux/linux_signing_key.pub | sudo gpg --dearmor -o /etc/apt/keyrings/google.gpg
sudo apt update
sudo apt install google-chrome-stable -y
```

📸 Screenshots: `task7_install_chrome_error.png`, `task7_ls_etc_apt.png`, `task7_cat_sources_list.png`, `task7_ls_sources_list_d.png`, `task7_cat_ubuntu_sources.png`, `task7_edit_ubuntu_sources.png`, `task7_add_key.png`, `task7_apt_update.png`, `task7_install_chrome.png`, `task7_alternate_remove.png`, `task7_alternate_edit.png`, `task7_remove_key.png`, `task7_create_google_chrome_list.png`, `task7_list_sources_after_create.png`, `task7_add_key_alt.png`, `task7_apt_update_alt.png`, `task7_install_chrome_alt.png`

## Task 8 – Install applications via PPA (Audacity & OBS) and launch

```bash
sudo add-apt-repository ppa:ubuntuhandbook1/audacity -y
sudo apt update
sudo apt install audacity -y
audacity --version

sudo add-apt-repository ppa:obsproject/obs-studio -y
sudo apt update
sudo apt install obs-studio -y
obs --version
```

📸 Screenshots: `task8_add_ppa_audacity.png`, `task8_apt_update_audacity.png`, `task8_install_audacity.png`, `task8_audacity_launch.png` or `task8_audacity_version.png`, `task8_add_ppa_obs.png`, `task8_apt_update_obs.png`, `task8_install_obs.png`, `task8_obs_launch.png` or `task8_obs_version.png`

## Task 9 – Create a Kubernetes sample YAML using vim

```bash
vim
sudo apt update && sudo apt install vim -y   # only if vim is missing
mkdir -p ~/Lab5
cd ~/Lab5
vim k8s-sample.yaml
```

File contents:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx:1.19
    ports:
    - containerPort: 80
  restartPolicy: Always
```

Save with `:wq`.

📸 Screenshots: `task9_vim_check.png`, `task9_vim_install.png` (only if installed), `task9_mkdir_cd.png`, `task9_vim_edit.png`, `task9_k8s_saved.png`

## Task 10 – Edit the Kubernetes YAML: add annotation, verify, then discard temporary change

Add under `metadata:`:

```yaml
  annotations:
    lab: lesson11
```

Save with `:wq`, verify with `cat k8s-sample.yaml`. Then reopen, add a temporary comment `# temp: do-not-keep`, quit with `:q!`, and verify with `cat` that it is not present.

📸 Screenshots: `task10_verify_annotation.png`, `task10_verify_entering_temp_data.png`, `task10_verify_no_temp_comment.png`

## Task 11 – Vim editing practice

In `~/Lab5/k8s-sample.yaml`: `dd` then `u`; `d3d` (or `3dd`) then `u`; `1G`; `G`; `$` and `0` on the `containerPort: 80` line; quit with `:q`.

📸 Screenshots: `task11_dd_delete_and_undo.png`, `task11_delete3_and_undo.png`, `task11_line1.png`, `task11_navigation.png`

## Task 12 – Vim search, add matches, substitute, undo

Search `/nginx`, move with `n` and `N`, add two more occurrences of `nginx`, cycle through matches, run `:%s/nginx/webapp`, undo with `u`, then quit (`:q!` to discard).

📸 Screenshots: `task12_search_nginx.png`, `task12_n_and_N_navigation.png`, `task12_added_occurrences.png`, `task12_cycle_matches.png`, `task12_substitute_result.png`, `task12_undo_and_quit.png`

---

## Exam Evaluation Question

Install Docker Desktop on your VMware Workstation Ubuntu Server. No commands or solutions are provided; research the steps yourself. Verify Docker Desktop is installed and running, and save a screenshot as `exam_evaluation_docker_desktop.png`.

## Troubleshooting & notes

- If any apt commands fail due to networking, capture the error output as screenshots and include them in your submission.
- vim: `i` insert mode, `Esc` command mode, `:wq` save and quit, `:q!` quit without saving, `u` undo, `Ctrl-R` redo, `3dd` / `d3d` numeric delete, `/pattern` search, `n` / `N` next/previous, `:%s/pattern/replacement` substitute.

## Submission

Create a repository named `CC_<YourName>_<YourRollNumber>/Lab5` containing:

```
Lab5/
  workspace/                    # any sample files you used (optional for this lab)
  screenshots/                  # all screenshots listed above
  Word file and PDF or .md file # student generated report
  Lab5-instructions.md          # this lab file
```

The report (Word and PDF, or .md) must contain the step outputs or terminal screenshots for each task and your answers to the hands-on practical exam questions.

## Checklist (for students)

- [ ] Ran `java` and followed apt suggestion — saved screenshots.
- [ ] Installed Java via apt, verified version, removed it, ran `hash -r` — saved screenshots.
- [ ] Installed Java via apt-get, verified, removed, cleared cache — saved screenshots.
- [ ] Ran `sudo apt update` and `sudo apt upgrade` and wrote explanation — saved screenshot.
- [ ] Installed VS Code using snap and did NOT remove it — saved screenshots.
- [ ] Installed XFCE + XRDP, launched VS Code in GUI — saved screenshots.
- [ ] Enabled GUI boot target, rebooted and captured GUI screenshot, then disabled it, rebooted and captured CLI screenshot.
- [ ] Performed Task 6 GUI verification and closed VS Code — saved screenshots.
- [ ] Performed Task 7 (Chrome) — saved screenshots.
- [ ] Performed Task 8 (PPAs: Audacity and OBS) — saved screenshots.
- [ ] Performed Task 9 (vim, `k8s-sample.yaml`) — saved screenshots.
- [ ] Performed Task 10 (annotation, discard edit) — saved screenshots.
- [ ] Performed Task 11 (dd/u, numeric deletes, navigation) — saved screenshots.
- [ ] Performed Task 12 (search, substitute, undo) — saved screenshots.
- [ ] Performed the Exam Evaluation (Docker Desktop) — saved screenshot.
- [ ] Pushed repository `CC_<YourName>_<YourRollNumber>` with Lab5 content and screenshots.
