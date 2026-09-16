# Linux 系統與網路組態備份 (linuxconf)

本專案用於備份與紀錄 Linux 伺服器的核心網路設定檔及主機名稱對應表，以便於系統重建、異常排查或日常維運管理。

## 📁 檔案清單與說明

* **`ens160.nmconnection`**：伺服器主要網路介面 `ens160` 的 NetworkManager 設定檔，內含 IP 位址、網閘（Gateway）及 DNS 等靜態網路配置。
* **`hosts`**：系統的靜態主機名稱解析對應檔（`/etc/hosts`），定義了本機名稱與局部區域網路內其他主機的 IP 映射。

## 🚀 部署與還原說明

> ⚠️ **注意**：還原或覆蓋系統設定檔前，請務必先備份原有的舊檔案。

### 1. 網路設定檔還原
將 `ens160.nmconnection` 複製到系統的 NetworkManager 設定目錄下，並重新載入網路設定：

```bash
# 複製檔案（請確保網卡名稱與實際硬體一致）
cp ens160.nmconnection /etc/NetworkManager/system-connections/

# 修改正確的檔案權限（安全規範要求此設定檔權限必須為 600）
chmod 600 /etc/NetworkManager/system-connections/ens160.nmconnection

# 重新載入並啟用網路設定
nmcli connection load /etc/NetworkManager/system-connections/ens160.nmconnection
nmcli connection up ens160
```

### 2. 主機名稱設定檔還原
將備份的 `hosts` 檔案覆蓋至系統的 `/etc/` 目錄：

```bash
cp hosts /etc/hosts
```

## 🛠️ 維護日誌
* 專案初始化，並成功上傳主機檔與網路組態設定。
