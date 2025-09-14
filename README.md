# 🎉 TrueMoney Voucher API (Free to Use)

API สำหรับ **ตรวจสอบ (Verify)** และ **กดรับ (Redeem)** TrueMoney Gift Voucher  
รองรับ 3 โหมดการใช้งาน:

1. **Redeem** → กดรับเงินเข้ากระเป๋าเลย  
2. **Verify** → ตรวจสอบว่าวอเชอร์ใช้ได้หรือไม่, มูลค่าเท่าไร, เหลือหรือหมด  
3. **Flow** → ทำ Verify ก่อน แล้วกด Redeem ต่ออัตโนมัติ  

**แจกให้ใช้ฟรี** ✅  
เรียกผ่าน HTTP (GET/POST) ได้ทั้งจากเว็บ, เกม, หรือสคริปต์  

---

## 📌 Endpoints

```
GET/POST /redeem?voucher=<VOUCHER_ID_OR_LINK>&mobile=<MOBILE>
GET/POST /verify?voucher=<VOUCHER_ID_OR_LINK>&mobile=<MOBILE>
GET/POST /flow  ?voucher=<VOUCHER_ID_OR_LINK>&mobile=<MOBILE>
```

- `voucher` → โค้ดวอเชอร์ หรือทั้งลิงก์ เช่น `https://gift.truemoney.com/campaign/?v=XXXXXXXX`  
- `mobile`  → เบอร์ TrueMoney Wallet ที่จะใช้รับเงิน  
  - ถ้าไม่ส่ง → ระบบจะพยายามอ่านจากไฟล์ config (`data.json`) ฝั่งเซิร์ฟเวอร์  

---

## 🧪 วิธีใช้งาน (Examples)

### 🔹 Redeem (กดรับเงิน)

#### Curl
```bash
curl -s "https://api.kiddy.wtf/redeem?voucher=VOUCHER_ID&mobile=0812345678"
```

#### JavaScript
```js
fetch("https://api.kiddy.wtf/redeem?voucher=VOUCHER_ID&mobile=0812345678")
  .then(r => r.json())
  .then(d => console.log("Redeem:", d))
  .catch(console.error);
```

#### PHP
```php
<?php
$url = "https://api.kiddy.wtf/redeem?voucher=VOUCHER_ID&mobile=0812345678";
$ch  = curl_init($url);
curl_setopt_array($ch, [ CURLOPT_RETURNTRANSFER => true ]);
$res = curl_exec($ch);
curl_close($ch);
print_r(json_decode($res, true));
```

#### Python
```python
import requests

url = "https://api.kiddy.wtf/redeem"
params = {"voucher": "VOUCHER_ID", "mobile": "0812345678"}
res = requests.get(url, params=params)
print("Redeem:", res.json())
```

---

### 🔹 Verify (ตรวจสอบวอเชอร์)

#### Curl
```bash
curl -s "https://api.kiddy.wtf/verify?voucher=VOUCHER_ID&mobile=0812345678"
```

#### JavaScript
```js
fetch("https://api.kiddy.wtf/verify?voucher=VOUCHER_ID&mobile=0812345678")
  .then(r => r.json())
  .then(d => console.log("Verify:", d))
  .catch(console.error);
```

#### PHP
```php
<?php
$url = "https://api.kiddy.wtf/verify?voucher=VOUCHER_ID&mobile=0812345678";
$ch  = curl_init($url);
curl_setopt_array($ch, [ CURLOPT_RETURNTRANSFER => true ]);
$res = curl_exec($ch);
curl_close($ch);
print_r(json_decode($res, true));
```

#### Python
```python
import requests

url = "https://api.kiddy.wtf/verify"
params = {"voucher": "VOUCHER_ID", "mobile": "0812345678"}
res = requests.get(url, params=params)
print("Verify:", res.json())
```

---

### 🔹 Flow (Verify → Redeem)

#### Curl
```bash
curl -s "https://api.kiddy.wtf/flow?voucher=VOUCHER_ID&mobile=0812345678"
```

#### JavaScript
```js
fetch("https://api.kiddy.wtf/flow?voucher=VOUCHER_ID&mobile=0812345678")
  .then(r => r.json())
  .then(d => console.log("Flow:", d))
  .catch(console.error);
```

#### PHP
```php
<?php
$url = "https://api.kiddy.wtf/flow?voucher=VOUCHER_ID&mobile=0812345678";
$ch  = curl_init($url);
curl_setopt_array($ch, [ CURLOPT_RETURNTRANSFER => true ]);
$res = curl_exec($ch);
curl_close($ch);
print_r(json_decode($res, true));
```

#### Python
```python
import requests

url = "https://api.kiddy.wtf/flow"
params = {"voucher": "VOUCHER_ID", "mobile": "0812345678"}
res = requests.get(url, params=params)
print("Flow:", res.json())
```

---

## 📤 ตัวอย่าง Response (Redeem สำเร็จ)

```json
{
  "redeemResponse": {
    "status": {
      "message": "success",
      "code": "SUCCESS"
    },
    "data": {
      "voucher": {
        "voucher_id": "429518470949487422",
        "amount_baht": "10.00",
        "redeemed_amount_baht": "10.00",
        "member": 1,
        "status": "active",
        "link": "0199489266a279c199b4f5822ca4620f35h",
        "detail": "",
        "expire_date": 1757945578215,
        "type": "R",
        "redeemed": 1,
        "available": 0
      },
      "owner_profile": {
        "full_name": "สมชาย ผู้ส่ง"
      },
      "redeemer_profile": {
        "mobile_number": "093xxxxxxx"
      },
      "my_ticket": {
        "mobile": "093xxxxxxx",
        "update_date": 1757859242273,
        "amount_baht": "10.00",
        "full_name": "สมหญิง ผู้รับ",
        "profile_pic": null
      },
      "tickets": [
        {
          "mobile": "093-xxx-1882",
          "update_date": 1757859242273,
          "amount_baht": "10.00",
          "full_name": "สมหญิง ผู้รับ",
          "profile_pic": null
        }
      ]
    }
  }
}
```

---

## 👤 Developer
Kiddy
