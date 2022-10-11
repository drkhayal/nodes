# Realio  Node qurma rəhbəri
## Sistem istəyi
 İstənilən bir Cosmos-SDK chain kimi, sistem istəyi olduqca uyğundur. 
## Minimum  Sistem istəyi
* 3x CPU; 
* 4GB RAM
* 80GB Disk
* Sabit internet bağlantısı  (minimum 10Mbps olacaq)
Donanım Gereksinimleri

## Məsləhət edilən sistem 
* 4x CPU;
* 8GB RAM
* 200 GB  (SSD və ya NVME)
## Realio Full Node qurmaq üçün addımlar
 Bu scripti istifadə edərək,  Realio fullnode-unu bir neçə dəqiqə ərzində avtomatik quracaqsınız.Scriptlə qurduğunuzda sizdən node adı istəyəcək, özünüzə aid bir ad qoyun.

```bash
wget -O RLO.sh https://raw.githubusercontent.com/drkhayal/nodes/main/realio/RLO.SH && chmod +x RLO.sh && ./RLO.sh
```
## Qurduqdan sonrakı addımlar 
Təsdiqləyicinin blokları sinxronizasiya etdiyindən əmin olmalısınız. Sinxronizasiya olub-olmadığını bilmək üçün bu kodu istifadə edin. 
```bash
realio-networkd status 2>&1 | jq .SyncInfo
```
## Cüzdan yaratma
Yeni cüzdan yaratmaq üçün bu koddan istifadə edin. Mnemoniclərinizi (12 və ya 24 sözünüzü yaddaşda saxlamağı unutmayın.
```bash
realio-networkd keys add $RLO_WALLET
```
(Opsional) Əgər artıq cüzdanınız varsa, onun mnemoniclərini istifadə edərək öz cüzdanınızı əlavə etmək istəyirsinizsə bu koddan istifadə edin. (bu ikinci dəfə quranlar və ya artıq özündə olan cüzdanı qoşmaq istəyənlər üçündür)
```bash
realio-networkd keys add $RLO_WALLET --recover
```
Aktiv cüzdanlara baxmaq üçün 
```bash
realio-networkd keys list
```
## Validator yaratmaq
Validator yaratmadan öncə balansınızda ən az 1 ədəd token olduğundan əmin olun.(1 rio 1000000 ario'edir) və nodun sinxron olduğundan əmin olun.

Cüzdan  balansını yoxlamaq üçün (sondakını silib öz adresinizi yazın): 
```bash
realio-networkd query bank balances $RLO_WALLET_ADDRESS
```
  Balansda token görmürsünüzsə, çox güman ki , node hələ sinxron olmayıb. Sinxron olmağını gözləyin və sonra davam edin.
Validator yaratmaq. 
```bash
realio-networkd tx staking create-validator \
  --amount 1999000ario \
  --from $RLO_WALLET \
  --commission-max-change-rate "0.01" \
  --commission-max-rate "0.2" \
  --commission-rate "0.07" \
  --min-self-delegation "1" \
  --pubkey  $(realio-networkd tendermint show-validator) \
  --moniker $RLO_NODENAME \
  --chain-id $RLO_ID \
  --fees 250ario
  ```


## Lazım ola biləcək kodlar:
Logları Kontrol Et:
```bash
journalctl -fu realio-networkd -o cat
```
Nodu başlad:
```bash
systemctl start realio-networkd
```
Nodu saxla:
```bash
systemctl stop realio-networkd
```
Nodu restart et: 
```bash
systemctl restart realio-networkd
```
