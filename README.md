<div align="center">

# 🔌 UnPlugged
**Bash script to help build and run an offline installer in recovery.**

[![GitHub Stars](https://img.shields.io/github/stars/corpnewt/UnPlugged?style=for-the-badge&color=ffd700)](https://github.com/corpnewt/UnPlugged/stargazers)
[![License](https://img.shields.io/github/license/corpnewt/UnPlugged?style=for-the-badge&color=007bff)](LICENSE)
[![Platform](https://img.shields.io/badge/Platform-macOS%20Recovery-white?style=for-the-badge&logo=apple)](https://github.com/corpnewt/UnPlugged)

---

Công cụ hỗ trợ tạo bộ cài offline ngay trong môi trường Recovery, giúp cài đặt macOS mà không cần kết nối internet ổn định hoặc khi bộ cài online gặp lỗi.

[📋 Prerequisites](#-prerequisites) • [🛠️ Pre-Install](#-pre-install-steps) • [💻 Recovery Steps](#-recovery-environment-steps) • [📝 Notes](#-notes-for-sonoma-and-later)

</div>

---

## 📋 Prerequisites

Để sử dụng công cụ này, bạn cần chuẩn bị sẵn các thành phần sau:

* **Bộ công cụ:** [UnPlugged](https://github.com/corpnewt/UnPlugged) & [gibMacOS](https://github.com/corpnewt/gibMacOS).
* **Dữ liệu macOS:**
    * Sử dụng `.dmg/.pkg` từ các bản cũ (10.7-10.12, trừ 10.9) tại [đây](https://support.apple.com/en-us/102662).
    * Riêng Mavericks (10.9), xem tại [thread này](https://forums.macrumors.com/threads/can-we-download-mavericks-directly-from-apples-servers.2444350/).
* **Recovery:** [macrecovery.py](https://github.com/acidanthera/OpenCorePkg/tree/master/Utilities/macrecovery) (hoặc gibMacRecovery).
* **Phần cứng:** USB dung lượng 16GB trở lên & EFI đã cấu hình theo [Dortania guide](https://dortania.github.io/OpenCore-Install-Guide/).

---

## 🛠️ Pre-Install Steps

1.  **Download macOS:** Tải **cùng một phiên bản** macOS qua cả `gibMacOS` và `macrecovery.py`.
    * *Lưu ý:* Chỉ cần cùng phiên bản lớn (Ví dụ: cùng là Ventura), không nhất thiết phải trùng hoàn toàn build số (point release). Tránh các bản Beta hoặc Apple Silicon từ gibMacOS.
2.  **Format USB:** Chia USB thành 2 phân vùng:
    * **Phân vùng 1 (FAT32):** Khoảng 750MB - 1GB (Chứa EFI và thư mục recovery).
    * **Phân vùng 2 (ExFAT):** Toàn bộ dung lượng còn lại (Chứa file từ gibMacOS). 
    * *Quan trọng:* Đảm bảo **Allocation Unit Size** của ExFAT là **1024 KB hoặc nhỏ hơn**.
3.  **Copy dữ liệu:** * Copy thư mục `EFI` và `com.apple.recovery.boot` vào phân vùng FAT32.
    * Copy các file tải từ gibMacOS và file `UnPlugged.command` vào phân vùng ExFAT.
4.  **Eject USB:** Rút USB an toàn để tránh lỗi ghi dữ liệu trước khi boot vào Recovery.

### 📂 Example USB Structure
```text
USB Drive
├── 📁 FAT32 Partition (OPENCORE)
│   ├── 📁 EFI
│   └── 📁 com.apple.recovery.boot
│       ├── 📄 BaseSystem.dmg
│       └── 📄 BaseSystem.chunklist
└── 📁 ExFAT Partition (UnPlugged)
    ├── 📦 InstallAssistant.pkg / InstallESDDmg.pkg
    └── 📄 UnPlugged.command
