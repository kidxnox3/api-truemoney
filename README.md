# TrueMoney Voucher API — วิธีการใช้งาน & วิธีส่งคำขอ (Request)

API สำหรับตรวจสอบ (verify) และกดรับ (redeem) **TrueMoney Gift Voucher**  
รองรับการเรียกแบบ **HTTP GET / POST (form & JSON)** และเปิด **CORS** ให้เรียกจากเว็บได้

---

## 📌 Endpoints

GET/POST /verify?voucher=<VOUCHER_ID_OR_LINK>&mobile=<MOBILE>
GET/POST /redeem?voucher=<VOUCHER_ID_OR_LINK>&mobile=<MOBILE>
GET/POST /flow ?voucher=<VOUCHER_ID_OR_LINK>&mobile=<MOBILE> # verify แล้ว redeem ต่อ

markdown
คัดลอกโค้ด

**พารามิเตอร์หลัก**
- `voucher` : โค้ดตรง หรือทั้งลิงก์ `https://gift.truemoney.com/campaign/?v=...`
- `mobile`  : เบอร์ TrueMoney Wallet ที่จะ “รับเงิน”
  - ถ้าไม่ส่ง ระบบจะพยายามอ่านจากไฟล์ config (`data.json`) ของเซิร์ฟเวอร์

**Headers ที่รองรับ**
- `Content-Type: application/json` (เมื่อส่งแบบ JSON)
- `Authorization: Bearer <YOUR_SERVER_KEY>` (ถ้าเจ้าของ API เปิดตรวจคีย์)

---

## 🧪 Quick Test (กดรับทันที)

### Curl (GET)
```bash
curl -s "https://api.kiddy.wtf/redeem?voucher=VOUCHER_ID&mobile=0812345678"
Curl (POST JSON)
bash
คัดลอกโค้ด
curl -s -X POST "https://api.kiddy.wtf/redeem" \
  -H "Content-Type: application/json" \
  -d '{"voucher":"VOUCHER_ID","mobile":"0812345678"}'
🔧 วิธีการ Request (ตัวอย่างหลายภาษา)
1) JavaScript (fetch)
js
คัดลอกโค้ด
// Redeem
fetch("https://api.kiddy.wtf/redeem?voucher=VOUCHER_ID&mobile=0812345678")
  .then(res => res.json())
  .then(data => {
    console.log("Status:", data?.redeemResponse?.status?.code, data);
  })
  .catch(console.error);

// Verify (ส่งเป็น JSON POST ก็ได้)
fetch("https://api.kiddy.wtf/verify", {
  method: "POST",
  headers: {"Content-Type": "application/json"},
  body: JSON.stringify({ voucher: "VOUCHER_ID", mobile: "0812345678" })
})
  .then(r => r.json())
  .then(console.log)
  .catch(console.error);
2) PHP (cURL client)
php
คัดลอกโค้ด
<?php
// Redeem แบบ GET
$url = "https://api.kiddy.wtf/redeem?voucher=VOUCHER_ID&mobile=0812345678";
$ch  = curl_init($url);
curl_setopt_array($ch, [
  CURLOPT_RETURNTRANSFER => true,
]);
$res = curl_exec($ch);
curl_close($ch);
$data = json_decode($res, true);
print_r($data);

// Verify แบบ POST JSON
$ch = curl_init("https://api.kiddy.wtf/verify");
curl_setopt_array($ch, [
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_POST => true,
  CURLOPT_HTTPHEADER => ["Content-Type: application/json"],
  CURLOPT_POSTFIELDS => json_encode(["voucher"=>"VOUCHER_ID","mobile"=>"0812345678"], JSON_UNESCAPED_UNICODE)
]);
$res = curl_exec($ch);
curl_close($ch);
echo $res;
3) Postman (แนวทาง)
Method: GET หรือ POST

URL: https://api.kiddy.wtf/redeem (หรือ /verify, /flow)

Headers (ถ้ามี): Content-Type: application/json, Authorization: Bearer <YOUR_SERVER_KEY>

Body (raw, JSON):

json
คัดลอกโค้ด
{ "voucher": "VOUCHER_ID", "mobile": "0812345678" }
📤 รูปแบบการตอบกลับ (Response)
สำเร็จ (Redeem)
json
คัดลอกโค้ด
{
  "redeemResponse": {
    "status": { "message": "สำเร็จ", "code": "SUCCESS" },
    "data": {
      "voucher": {
        "voucher_id": "xxxxxxxxxxx",
        "amount_baht": "10.00",
        "redeemed_amount_baht": "10.00",
        "status": "redeemed",
        "expire_date": 1735251234,
        "type": "R",
        "redeemed": 20,
        "available": 0
      },
      "owner_profile": { "full_name": "xxxxxxx ***" },
      "redeemer_profile": null,
      "my_ticket": null,
      "tickets": []
    }
  }
}
ข้อผิดพลาดที่พบบ่อย
json
คัดลอกโค้ด
{ "redeemResponse": { "status": { "message": "curl error", "code": "CURL_ERROR" }, "data": null } }
json
คัดลอกโค้ด
{
  "status": { "message": "bad request", "code": "BAD_REQUEST" },
  "error": { "missing": { "mobile": "required", "voucher": "ok" } }
}
🛡️ แนวทางความปลอดภัย (แนะนำ)
เปิดตรวจ Authorization: Bearer <SERVER_KEY> สำหรับทุกคำขอภายนอก

เปิด HTTPS เท่านั้น (ปกติแพลตฟอร์มเช่น Render/Railway จะมีให้อยู่แล้ว)

ตั้ง Rate Limit ที่หน้า Web Server/WAF เพิ่มเติม (เช่น Cloudflare Rules)

ปิด error บน production และอย่าใส่ข้อมูลลับไว้ในซอร์ส

🆘 Troubleshooting
CURL_ERROR : โฮสต์ปลายทางบล็อก outbound หรือ PHP ไม่มี cURL/OpenSSL → แนะนำรันบน Render/Railway/VPS

SSL handshake error : บังคับ TLS 1.2 ฝั่งเซิร์ฟเวอร์ (ผู้พัฒนา API จะตั้งค่าไว้ให้แล้ว)

404 : ตรวจว่า endpoint ถูกต้อง และโครง rewrite (ถ้ามี) ทำงานปกติ

JSON parse error ฝั่ง client : ตรวจ Content-Type: application/json และรูปแบบ JSON

👤 ผู้พัฒนา
Kid – Ai (Kiddy)

makefile
คัดลอกโค้ด
::contentReference[oaicite:0]{index=0}
