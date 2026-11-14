# BANGSAEN42 Medical Operations Dashboard

Static web app (ไฟล์เดียว `index.html`, ไม่ต้อง build) ดึงข้อมูลสดจาก Google Sheet ทุก 15 วินาที

## Deploy ด้วย GitHub Pages
1. สร้าง repo ใหม่ แล้วอัปโหลด `index.html` และ `README.md` ไว้ที่ root
2. Settings → Pages → Source: **Deploy from a branch** → Branch: `main` / `(root)` → Save
3. รอ 1–2 นาที เปิด `https://<username>.github.io/<repo>/`

## ตั้งค่า
แก้ `CFG` ในไฟล์ `index.html`:

| ค่า | ความหมาย |
|---|---|
| `SHEET_ID` | ID ของ Google Sheet |
| `TAB` | ชื่อแท็บข้อมูลดิบ (`Form Responses 1`) |
| `LOG_TAB` | แท็บ Patient log (ใช้คอลัมน์ G เป็น Total stay) |
| `REFRESH_SEC` | ความถี่รีเฟรช (วินาที) |

ลำดับคอลัมน์ของ `Form Responses 1` ตั้งไว้ที่ตัวแปร `C` (เช่น `LOC:3` = คอลัมน์ D จุดเกิดเหตุ, `TIN:11` = Time in, `TRI:15` = Triage, `DX:30` = Diagnosis, `DISP:36` = Disposition)

## ข้อควรรู้
- Google Sheet ต้องแชร์เป็น **Anyone with the link → Viewer** เพราะหน้าเว็บอ่านชีตโดยตรงจากเบราว์เซอร์
- ข้อมูลผู้ป่วย (Bib, เพศ/อายุ, Diagnosis) จะเปิดดูได้ด้วยลิงก์ชีตและ URL ของเว็บ ควรจำกัดผู้รู้ลิงก์ หรือวางหลัง login (Cloudflare Access, Netlify password) แทน GitHub Pages สาธารณะ
- Total stay: ถ้าหาแถวที่ตรงกันในแท็บ Patient log ได้ (จับคู่ด้วย Bib + Time in) จะใช้ค่าจากคอลัมน์ G ถ้าไม่พบจะคำนวณเองจาก Time in ถึง Disposition time
- ปุ่ม PDF ใช้ html2canvas + jsPDF จาก CDN (cdnjs)
