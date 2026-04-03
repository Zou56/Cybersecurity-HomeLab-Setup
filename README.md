# Cybersecurity-HomeLab-Setup

# 🛡️ Virtual Cybersecurity Lab on Apple Silicon (M1/M2/M3)

Repositori ini mendokumentasikan proses pembangunan lab *Penetration Testing* menggunakan arsitektur ARM pada MacBook. Tantangan utama dalam proyek ini adalah melakukan emulasi sistem x86 (Intel) di atas chip Apple Silicon.

## 🛠️ Tech Stack
- **Host OS:** macOS (Apple Silicon)
- **Virtualization:** [UTM](https://getutm.app/) (QEMU-based)
- **Attacker Machine:** Kali Linux 2024.x (ARM64 Native)
- **Victim Machine:** Metasploitable 2 (Linux x86 - Emulated)

## 🚀 Setup Highlights
### 1. Metasploitable 2 (x86 Emulation)

<img width="1035" height="768" alt="Screenshot 2026-04-03 at 10 28 09" src="https://github.com/user-attachments/assets/37a17d5b-958a-4e1b-bbe4-8f9f154739d3" />

Karena Metasploitable 2 hanya tersedia untuk arsitektur Intel, saya menggunakan fitur **Emulasi x86_64** di UTM dengan konfigurasi:
- **Architecture:** x86_64
- **System:** Standard PC (Q35 + ICH9)
- **Network:** Shared Network (agar dapat berkomunikasi dengan Kali Linux)
- **Fix:** Menonaktifkan `UEFI Boot` agar sistem *legacy* ini dapat berjalan.

### 2. Kali Linux (Native ARM64)

<img width="1036" height="765" alt="Screenshot 2026-04-03 at 10 28 57" src="https://github.com/user-attachments/assets/fa272554-6b5b-4ecf-9ec4-da0694983f56" />

Untuk performa maksimal, saya menginstal Kali Linux versi ARM64 secara native:
- **Mode:** Virtualize (bukan Emulate)
- **Display:** `virtio-ramfb` (untuk mengatasi masalah "Display output is not active" saat instalasi)

## ⚔️ Simulation: vsFTPd 2.3.4 Backdoor Exploit
Saya mensimulasikan serangan *backdoor* pada layanan FTP yang berjalan di target.

### Steps:
1. **Scanning:** Menggunakan `nmap -sV` untuk mengidentifikasi layanan yang berjalan.
   <img width="1680" height="1050" alt="Screenshot 2026-04-03 at 10 29 36" src="https://github.com/user-attachments/assets/bfab28d0-3644-4cf8-9edd-2b8f4870f02f" />
2. **Exploitation:** Menggunakan Metasploit Framework (`msfconsole`).
   <img width="1680" height="1050" alt="Screenshot 2026-04-03 at 10 33 19" src="https://github.com/user-attachments/assets/7c496526-b95a-499c-ad92-ad39a0f0ecda" />
3. **Troubleshooting:** Awalnya eksploitasi gagal membuat sesi (`no session was created`). Saya berhasil mengatasinya dengan mengganti payload secara manual ke `cmd/unix/interact`.
   <img width="1680" height="1050" alt="Screenshot 2026-04-03 at 10 32 17" src="https://github.com/user-attachments/assets/d43014ba-282a-4ac1-9a20-18b6ac742bd8" />

### Result:
```bash
msf6 exploit(unix/ftp/vsftpd_234_backdoor) > exploit
[*] 192.168.64.2:21 - Banner: 220 (vsFTPd 2.3.4)
[+] 192.168.64.2:21 - UID: uid=0(root) gid=0(root)
[*] Found shell.
[*] Command shell session 1 opened
```
<img width="1680" height="1050" alt="Screenshot 2026-04-03 at 10 35 49" src="https://github.com/user-attachments/assets/a5d5835a-be59-4cbb-9cd5-eefc87066514" />

## 🔧 Troubleshooting Log
- **Issue:** Kali Linux black screen ("Display output is not active") during installation.
- **Solution:** Switched emulated display card to `virtio-ramfb` and disabled OpenGL acceleration.
- **Issue:** Metasploit exploit failed to open session.
- **Solution:** Manually set payload to `cmd/unix/interact` to bypass network routing issues on Shared Network.

---
## 👨‍💻 Author
**Ibnu Restu Pamungkas**
*Informatics Student at Universitas Majalengka*
[LinkedIn](https://www.linkedin.com/in/ibnu-restu-pamungkas-989750299) | [Instagram](https://www.instagram.com/r.pamungkasss/)

> **⚠️ Disclaimer:** This project is for educational purposes only. I am not responsible for any misuse of the information provided in this repository.
