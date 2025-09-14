# Set sub domain record

要為現有域名設定子網域，您需要在同一個 DNS Zone 中添加一個新的 DNS 記錄。這是一個簡單的過程，不需要建立新的 Zone。

您可以使用以下步驟添加子網域記錄：

```bash
# 假設您的主域名是 example.com，子網域是 subproject.example.com
# 1. 開始交易
gcloud dns record-sets transaction start --zone="您的zone名稱"

# 2. 添加子網域記錄
gcloud dns record-sets transaction add "subproject.example.com." \
  --zone="您的zone名稱" \
  --type="A" \
  --ttl="300" \
  --rrdatas="子專案的IP地址"

# 3. 執行交易
gcloud dns record-sets transaction execute --zone="您的zone名稱"
```

關鍵說明：
- 您需要在同一個 DNS Zone 中添加新的 A 記錄
- 子網域格式為 `subproject.example.com.`（注意結尾有一個點）
- `--rrdatas` 參數需要設定為您子專案的 IP 地址
- 如果子專案使用的是 CNAME 記錄而非 IP 地址，則需要將 `--type` 改為 "CNAME"，並將 `--rrdatas` 設為目標主機名

這樣設定後，訪問 subproject.example.com 的請求將被導向到您子專案的 IP 地址。

