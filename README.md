# 🐧 Linux 系統與網路組態備份庫 (linuxconf)

[![Git Version](https://shields.io)](https://git-scm.com)
[![Platform](https://shields.io)](https://redhat.com)

本專案用於集中備份與管理 Linux 伺服器的核心網路設定檔及主機名稱對應表，確保系統在遭遇異常、重建或進行叢集擴充時，能夠快速還原至正確的網路組態。

---

## 📂 檔案架構與核心功能

| 檔案名稱 | 建議存放路徑 | 說明描述 |
| :--- | :--- | :--- |
| **`ens160.nmconnection`** | `/etc/NetworkManager/system-connections/` | 主要網路介面 `ens160` 的 NetworkManager 設定檔，包含靜態 IP、網閘（Gateway）與 DNS。 |
| **`hosts`** | `/etc/hosts` | 系統靜態主機名稱解析對應檔，用於定義本機名稱與局部區域網路（LAN）內的 IP 映射。 |

---

## 🚀 部署與快速還原指南

> ⚠️ **安全宣告**：還原或覆蓋系統設定檔屬高風險操作，執行前請務必先將系統現有的舊檔案進行備份。

### 1. 網路設定檔（NetworkManager）還原

將備份的連接組態複製回對應目錄。**請特別注意：NetworkManager 要求設定檔權限必須極其嚴格，否則系統將拒絕讀取該網卡設定。**

```bash
# 1. 複製設定檔至系統目錄
sudo cp ens160.nmconnection /etc/NetworkManager/system-connections/

# 2. 修改檔案權限為 600（僅 root 可讀寫，極重要！）
sudo chmod 600 /etc/NetworkManager/system-connections/ens160.nmconnection

# 3. 重新載入 NetworkManager 組態
sudo nmcli connection load /etc/NetworkManager/system-connections/ens160.nmconnection

# 4. 啟用並重啟網路介面
sudo nmcli connection up ens160
```

### 2. 主機名稱解析檔（Hosts）還原

直接將備份的檔案覆蓋至系統的網卡控制層級：

```bash
# 覆蓋系統預設的 hosts 檔案
sudo cp hosts /etc/hosts
```

---

## 🛠️ 維護日誌與異動紀錄

* **2026-09-16**：優化專案文件結構，補全 RHEL 10 規格之部署與權限修正說明。
* **項目初始化**：成功完成 `git init` 並完成首批網路基礎設定檔之上傳。
