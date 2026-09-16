# REPORT.md

## Assignment 03 — Docker: Build, Publish, Pull, Compose

### ข้อมูลผู้จัดทำ

- รหัสนักศึกษา: 671540005008-1
- Docker Hub: phongsakorn05/ceksu-badge
- Image ของเพื่อน: kittiphat26/ceksu-badge:1.0

---

## 1. Tag `1.0` และ `1.1` ต่างกันอย่างไร

Tag ใช้สำหรับระบุเวอร์ชันของ Docker Image  
ในงานนี้ `1.0` เป็นเวอร์ชันแรก และ `1.1` เป็นเวอร์ชันที่แก้ไขหน้าเว็บ  
โดย Version 1.1 มีการเปลี่ยนสีพื้นหลังและเพิ่มข้อความให้แตกต่างจาก 1.0

---

## 2. ถ้า `docker pull` จะได้ Image จากที่ไหน

คำสั่ง `docker pull` จะดึง Image จาก Docker Registry  
ในงานนี้ดึง Image จาก Docker Hub เช่น `phongsakorn05/ceksu-badge:1.0`  
เมื่อ Pull สำเร็จ สามารถนำ Image มาสร้าง Container และเปิดใช้งานได้

---

## 3. Image ของเพื่อนมี Port และ Cmd อะไร

จากคำสั่ง `docker image inspect` พบว่า Image ของเพื่อนเปิดใช้ Port `80/tcp`  
Cmd ที่พบคือ `nginx -g daemon off;`  
จาก `docker history` พบว่า Image ใช้ Nginx และ Alpine เป็นพื้นฐาน

---

## 4. Docker Hub กับ GitHub ต่างกันอย่างไร

Docker Hub ใช้สำหรับเก็บและเผยแพร่ Docker Image  
GitHub ใช้สำหรับเก็บ Source Code และไฟล์ของโปรเจกต์  
ดังนั้น Docker Hub เหมาะกับการแจกจ่าย Image ส่วน GitHub เหมาะกับการเก็บโค้ดและเอกสาร

---

## 5. ถ้าเพื่อนแก้ Image แล้ว Push ทับ Tag `1.0` จะเกิดอะไรขึ้น

ถ้า Push Image ใหม่ทับ Tag `1.0` ผู้ที่ Pull `1.0` ในภายหลังอาจได้รับ Image รุ่นใหม่  
ทำให้ Tag เดิมไม่ได้รับประกันว่าจะชี้ไปยัง Image เดิมตลอดเวลา  
วิธีป้องกันคือไม่ควรเขียนทับ Tag เดิม และควรสร้าง Tag ใหม่ เช่น `1.1` หรือ `2.0`

---

## Docker Compose

ใช้ Docker Compose เพื่อรัน 2 Services พร้อมกัน

### Service 1 — mybadge

- Build จาก Dockerfile ในโปรเจกต์
- Host Port: `8080`
- Container Port: `80`

### Service 2 — friend

- Image: `kittiphat26/ceksu-badge:1.0`
- Host Port: `8081`
- Container Port: `80`

หลังจากแก้ไข `index.html` ต้องใช้คำสั่ง:

```bash
docker compose up -d --build