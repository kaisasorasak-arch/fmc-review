# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**FMC Software** — ระบบประเมินผลงานพนักงานบน browser เพื่อปรับเงินเดือนและเลื่อนตำแหน่ง
ข้อมูลทั้งหมดเก็บใน Google Sheets ผ่าน Google Apps Script Web App (fetch API) ไม่มี backend server

> เดิมชื่อ APEX REVIEW v2 — เปลี่ยนเป็น FMC Software (2026-05-30)

## Roles & Groups

**4 Roles:** พนักงาน (employee) · หัวหน้า (manager) · ผู้บริหาร (executive) · HR (hr)

**5 Groups:** FMC · ISEC · PPP · BNT · NSP

**Flow:** พนักงาน → Self-eval → หัวหน้า (ดู self + กรอก manager eval + recommendation) → Executive/Director (ดู Report + ตัดสินใจขั้นสุดท้าย) → HR (เห็นผล Director Decision เพื่อดำเนินการต่อ)

## Company → Group Mapping (ข้อมูลจริง)

| Group | บริษัท | รหัสพนักงาน |
|-------|--------|------------|
| FMC | แฟคซิลิตี้ แมนเนจเมนท์ (= เซมเพิล ค็อกเครน เดิม) | FMC-xxx, SCA-xxx |
| ISEC | ไอ ซิคิวริตี้ เอ็นเตอร์ | ISEC-xxx |
| PPP | ทริปเปิ้ล พี เทคโนโลยี | PPP-xxx + OUTS บางส่วน |
| BNT | บ้านนลินทิพย์ | BNT-xxx + OUTS/FMC บางส่วน |
| NSP | เน็ตเซอร์พลัส | NSP-xxx + OUTS บางส่วน |

**OUTS prefix** = พนักงานร่วม/shared resources — จัดสังกัด Group ตาม Org Chart ที่ปรากฏ

## ระดับตำแหน่ง → position_type

| ระดับ | position_type | Form | หมายเหตุ |
|-------|--------------|------|---------|
| ระดับบริหาร | — | **ไม่ประเมิน** | MD, Director สูงสุด |
| ระดับฝ่าย | manager | Manager Form | PDF: แบบประเมิน (หน.) _Manager_AsstManager.pdf |
| ระดับแผนก | manager | Manager Form | ใช้ PDF เดียวกับระดับฝ่าย |
| ระดับปฏิบัติการ (senior) | senior | Staff/Senior Form | PDF: แบบประเมิน_Staff_Senior.pdf |
| ระดับปฏิบัติการ (staff) | staff | Staff/Senior Form | ใช้ PDF เดียวกับ senior |
| Director / BU Director | director | Manager Form | ใช้ isManagerType() check — เหมือน manager |

## Form Structure (Staff & Senior) — ตาม PDF จริง

ไฟล์ PDF: `แบบประเมินผลการปฎิบัติงาน_Staff_Senior.pdf` (อยู่ในโฟลเดอร์โปรเจกต์)

**4 ส่วน:**

| ส่วน | เนื้อหา | กรอกโดย |
|-----|--------|---------|
| ส่วนที่ 1 | ภารกิจหลัก 3 ข้อ + checkbox (เกินเป้า/บรรลุ/ต่ำกว่า) | พนักงาน |
| ส่วนที่ 2 | พฤติกรรม 10 หัวข้อ คะแนน 1–5 | พนักงาน |
| ส่วนที่ 3 | ทบทวนตนเอง 4 ช่อง (จุดแข็ง/พัฒนา/สนับสนุน/เป้าหมาย 6 เดือน) | พนักงาน |
| ส่วนที่ 4 | สรุปผลโดยรวม + ข้อเสนอแนะ | หัวหน้า |

**พฤติกรรม 10 หัวข้อ (BEHAVIOR_LIST ใน app.js):**
1. ความมีระเบียบวินัย (Discipline)
2. การเป็นคำพูดและให้เกียรติคำพูด (Integrity)
3. การเลือกตอบสนอง (Be Proactive)
4. การกำหนดเป้าหมาย (Goal Setting)
5. การวางแผนและจัดลำดับความสำคัญ (Planning)
6. การทำงานร่วมกับผู้อื่น (Collaboration)
7. ทักษะการแก้ปัญหา (Problem Solving)
8. ทักษะการรับฟังผู้อื่นอย่างตั้งใจ (Active Listening)
9. ทักษะการนำเสนอ (Presentation Skill)
10. ความรู้เกี่ยวกับภารกิจของงาน (Knowledge)

## Grade System

**Staff / Senior** — หัวหน้าเลือก (ไม่ auto-calculate):
- ดีเยี่ยม · ดี · ปานกลาง · พอใช้ · ต้องปรับปรุง

**Manager / Director** — หัวหน้าเลือก (ไม่ auto-calculate):
- ดีเยี่ยม · ดี · ปานกลาง · พอใช้ · ต้องปรับปรุง

## Design

- **ชื่อระบบ:** FMC Software (เดิม APEX REVIEW)
- **Logo:** FM (ตัวอักษรในกล่องสีแดง)
- **สีหลัก:** `#E02020` (แดง) — เปลี่ยนจาก `#6B4EFF` (ม่วง) เมื่อ 2026-05-30
- **Sidebar:** `#1A1A2E` (dark navy) — ไม่เปลี่ยน
- **Font:** Sarabun (Thai) + Inter (EN)

## Files

| ไฟล์ | หน้าที่ |
|------|--------|
| `index.html` | Single-page app — views ทั้งหมด |
| `style.css` | Styles + @media print สำหรับ PDF |
| `app.js` | Auth, routing, API calls, export (~2,500 บรรทัด) |
| `apps-script.gs` | Google Apps Script backend (reference) |
| `แบบประเมินผลการปฎิบัติงาน_Staff_Senior.pdf` | แบบฟอร์ม Staff/Senior |
| `แบบประเมินผลการปฎิบัติงาน  (หน.) _Manager_AsstManager.pdf` | แบบฟอร์ม Manager (⚠️ ชื่อมีช่องว่าง 2 ช่องก่อน "(หน.)") |
| `รายชื่อพนักงาน.xlsx` | ข้อมูลพนักงานจริง 112 คน |
| `_ไฟล์ไม่เกี่ยวข้อง/` | โฟลเดอร์เก็บ temp + test files |

## How to Run

เปิดไฟล์ `index.html` ตรงๆ ใน browser — ไม่ต้องใช้ server

**Mock accounts (ทดสอบโดยไม่ต้อง Google Sheets):**

| Username | Password | Role | หมายเหตุ |
|----------|----------|------|---------|
| `admin` | `admin` | Executive | **Super account** — เข้าได้ทุกฟีเจอร์ |
| `hr` | `1234` | HR | จัดการพนักงาน |
| `fmc-001` | `1234` | Executive | นาย พิชัย สีห์โสภณ (MD FMC) |
| `fmc-266` | `1234` | Manager | นาย วีระ พานิชกุล (Section Eng. Mgr FMC) |
| `fmc-187` | `1234` | Employee | นาย ชาญชัย อ่องเทศ (Staff, FMC) |
| `isec-121` | `1234` | Executive | นาย กฤช อมรวรเดโช (MD ISEC) |
| `ppp-015` | `1234` | Executive | นาย สุริยะ รัตตากร (Deputy MD PPP) |

## Tech Stack

- Vanilla HTML / CSS / JavaScript (ไม่มี framework)
- Google Sheets เป็น database ผ่าน Google Apps Script Web App URL
- **SheetJS** via CDN — Export Excel (ได้รับอนุญาตแล้ว)
- PDF — `window.print()` + CSS @media print (ไม่มี library)

## Architecture

การไหลของข้อมูล:
1. `app.js` เรียก `apiGet({ action, ...params })` → GET request ไป Apps Script
2. `app.js` เรียก `apiPost(payload)` → POST request ไป Apps Script
3. Apps Script อ่าน/เขียน Google Sheets (4 sheets: Users, SelfEval, ManagerEval, ExecDecision) แล้วตอบกลับเป็น JSON

ค่า `APPS_SCRIPT_URL` เก็บใน `app.js` บรรทัดแรก — ต้องเปลี่ยนหลัง deploy จริง

**Mock mode:** เมื่อ URL ยังเป็น `'YOUR_APPS_SCRIPT_URL_HERE'` ระบบใช้ `mockApi()` + `localStorage` แทน

## Design Guidelines

- สไตล์: Utilitarian, dense, scannable — ใช้งานทุกวัน
- สีหลัก: `#6B4EFF` (purple) สีรอง: `#F0EDFF`
- Sidebar background: `#1A1A2E` (dark navy)
- Font: Sarabun (Thai) + Inter (EN) จาก Google Fonts
- Desktop-first — ไม่ต้อง responsive mobile

## Coding Guidelines

- comment โค้ดเป็นภาษาไทย
- ถ้าไม่แน่ใจอะไรให้ถามก่อน อย่าเพิ่งทำเอง
- **เรียก Skills ที่เกี่ยวข้องก่อน implement ทุกครั้ง** (frontend-design-direction, performance-review, ฯลฯ)
- หลังแก้โค้ดเสร็จให้บอกว่าต้องเปิด browser เช็คที่ส่วนไหน และคาดหวังว่าจะเห็นอะไร
- **⚠️ อ่านและเรียนรู้โค้ดที่มีอยู่ก่อนเสมอ — ห้ามเปลี่ยนหรือลบของเดิมเด็ดขาด**
- **แก้เฉพาะจุดที่จำเป็นเท่านั้น ไม่ refactor หรือ restructure โดยไม่ได้รับอนุญาต**

## Constraints

- ห้ามเพิ่ม library หรือ framework ภายนอกโดยไม่ได้รับอนุญาต (SheetJS ได้รับอนุญาตแล้ว)
- ห้ามแก้ไขไฟล์มากกว่า 1 ไฟล์พร้อมกัน เว้นแต่จะบอกล่วงหน้า
- **ห้ามเปลี่ยนหรือลบโค้ดเดิมเด็ดขาด — อ่านและเรียนรู้ก่อน แล้วค่อยแก้เฉพาะจุด**
- ห้ามสร้างไฟล์ใหม่โดยไม่บอกก่อน

## Development Phases (อัปเดต 2026-05-29)

### Phase 1 — Foundation ✅ เสร็จแล้ว
- Auth / Login / Logout / Session
- Sidebar + routing (showView, goBack, goDashboard)
- Employee Dashboard (สถานะ self-eval, grade, upload ไฟล์)
- Manager Dashboard (ตารางทีม, progress bar)
- Executive Dashboard (ภาพรวม 5 กลุ่ม, ตารางพนักงานทั้งหมด)
- Self-Eval Form — Staff/Senior ครบ 4 ส่วน (leave, missions, behavior 10, self-review)
- Manager Eval Form — Staff/Senior (ดู self read-only, grade picker, recommendation, comment)
- Admin CRUD พนักงาน + Import Excel + Export Template
- Upload ไฟล์แบบฟอร์ม (JPG/PNG/PDF, localStorage)
- Mock API + localStorage ทุก action
- Period selector Q1/Q2 + year

### Phase 2 — Complete Core Flow ✅ เสร็จแล้ว (2026-05-29)

- **Import พนักงานจริง 112 คน** จาก `รายชื่อพนักงาน.xlsx`
  - `const REAL_EMPLOYEES` embed ใน `app.js` บรรทัด ~2200
  - username = รหัสพนักงาน lowercase (`fmc-266`) / password = `1234`
  - PPP-038 manager_id ว่าง (หัวหน้าไม่อยู่ในไฟล์)
- **CRUD พนักงานบันทึกถาวร** ผ่าน localStorage — `persistEmployees()` บันทึกเฉพาะพนักงานจริง
- **getMockUsers()** merge testAccounts เสมอ ไม่ขึ้นกับ localStorage (แก้ bug admin login)
- **Exec Dashboard** แสดง employee + manager / ปุ่มประเมินตรงในตาราง / grade ใช้ GRADE_LIST
- **Executive nav** เพิ่ม Self-Evaluation (admin เข้าได้ทุกฟีเจอร์)

**✅ ครบแล้ว (2026-05-30):**
- **Report รายบุคคล** — เขียนใหม่ทั้งหมด: header + ภารกิจ + behavior bar chart + ทบทวนตนเอง + ความเห็นหัวหน้า + print PDF
- **Exec Decision** — บันทึกใน Report view + แสดงสถานะ ✓ ในตาราง Exec Dashboard

### Phase 3 — Manager Form ✅ เสร็จแล้ว (2026-05-29)
- **MANAGER_BEHAVIOR_LIST** — 10 หัวข้อ Manager (7 Habits + Delegation + Coaching)
- **Manager Self-Eval Form** 5 ส่วน ตาม PDF: ภารกิจ+ผลลัพธ์, พฤติกรรม, ทบทวนผู้นำ, วิสัยทัศน์
  - ฟังก์ชัน: `renderOldSelfEvalLayout()` + `submitManagerSelfEval()`
- **Manager Mgr-Eval Form** ส่วนที่ 5: `renderOldMgrEvalLayout()`
- **Manager Dashboard** เพิ่ม form download + upload (`mgr-self-form-section` ใน index.html)
- **downloadForm('manager')** → เปิด `แบบประเมินผลการปฎิบัติงาน  (หน.) _Manager_AsstManager.pdf`
  - ⚠️ ชื่อไฟล์มีช่องว่าง 2 ช่องก่อน "(หน.)"

### Phase 4 — Google Sheets Integration ✅ เสร็จแล้ว (2026-05-31)

**apps-script.gs รองรับทุก action:**
- GET: `login`, `getAllData`
- POST: `submitSelfEval`, `submitManagerEval`, `submitExecDecision`, `addEmployee`, `updateEmployee`, `deleteEmployee`

**Schema (columns ใน Sheets):**
- Users: `id, username, password, role, group, name, position, position_type, department, manager_id, nickname`
- SelfEval: `id, employee_id, form_type, missions, leave, behavior, self_review, vision, kpi_scores, comment, year, quarter, submitted_at`
- ManagerEval: `id, manager_id, employee_id, form_type, overall_grade, mgr_behavior, manager_comment, recommendation, kpi_scores, year, quarter, submitted_at`
- ExecDecision: `id, exec_id, employee_id, decision, note, year, quarter, decided_at`

**ขั้นตอน Deploy (ทำ 1 ครั้ง):**
1. เปิด [script.google.com](https://script.google.com) → สร้าง Project ใหม่
2. วางโค้ดจาก `apps-script.gs` ทั้งหมด
3. รัน `setupSheets()` 1 ครั้ง เพื่อสร้าง header ใน Google Sheets
4. กด Deploy → New Deployment → Web App
   - Execute as: **Me**
   - Who has access: **Anyone**
5. Copy URL ที่ได้ → วางแทน `'YOUR_APPS_SCRIPT_URL_HERE'` ใน `app.js` บรรทัดแรก
6. Import พนักงาน 112 คน ผ่านหน้า Admin → Import Excel (`รายชื่อพนักงาน.xlsx`)
7. Test login ทุก role

### Phase 5 — Polish & Export ✅ บางส่วนเสร็จแล้ว (2026-06-02)

**Export Excel HR ✅ เสร็จแล้ว**
- ฟังก์ชัน `exportExcel()` ใน app.js — แก้ใหม่ทั้งหมด
- คอลัมน์: ชื่อ, กลุ่ม, แผนก, ตำแหน่ง, ประเภท, Self-Eval, Grade, Recommendation, Director Decision, หมายเหตุ
- ชื่อไฟล์: `fmc-review-HR-[กลุ่ม]-[วันที่].xlsx`
- รวม employee + manager
- ⚠️ ถ้า browser download ชื่อ `apex-review-...` = cache เก่า → Ctrl+Shift+R

**PDF Print ✅ แก้แล้ว**
- print Report ไม่ติดคู่มือมาด้วยแล้ว
- คู่มือต้องกดปุ่ม print จากหน้าคู่มือเท่านั้น (`printManual()` เพิ่ม class `printing-manual` บน body)
- CSS: `body.printing-manual #view-manual { display: block }` — แยก print context ชัดเจน
- ⏳ PDF ยังพิมพ์ได้แค่ทีละ 1 คน — รอตัดสินใจว่าจะพิมพ์ทีละคน หรือทั้งกลุ่มพร้อมกัน

**ฟอร์ม Admin ✅ เสร็จแล้ว (2026-06-03)**
- `position_type = 'admin'` — เพิ่มใน dropdown + posTypeBadge (สีส้ม #F59E0B) + `isAdminType()`
- Self-Eval 4 ส่วน (multi-step): Competencies / Behaviors / ภารกิจ / ทบทวนตนเอง
- Manager Eval 4 ส่วน: เห็น Self-Eval พนักงาน + ให้คะแนน + สรุป + Recommendation + Career + DevPlan
- คะแนน 4 ส่วน × 25 = 100 คะแนน
- Data: `ADMIN_COMPETENCY_LIST` (10) + `ADMIN_BEHAVIOR_LIST` (10) ใน app.js
- State: `adminFormData`, `adminFormStep`, `adminMgrFormData`, `adminMgrFormStep`
- ฟังก์ชัน: `renderAdminSelfEvalLayout`, `renderAdminMgrStep`, `submitAdminSelfEval`, `submitAdminMgrEval`

**ฟอร์มตำแหน่งอื่น ⏳ รอแบบฟอร์ม**
- แต่ละ position_type มี Competency ต่างกัน — รอ PDF จากเจ้านาย
- ตำแหน่งที่รอ: staff, senior, manager, director
- โครงสร้างเหมือน Admin — 4 ส่วน / multi-step / 25 คะแนนต่อส่วน

**ยังค้างอยู่:**
- UI/UX polish จากการใช้งานจริง

### Phase 6 — Hosting / Deploy ✅ เสร็จแล้ว (2026-06-04)
- **URL:** https://kaisasorasak-arch.github.io/fmc-review/
- **Platform:** GitHub Pages
- **GitHub account:** kaisasorasak-arch / Repo: fmc-review
- พนักงานเข้าใช้งานได้จากทุกที่ผ่าน browser

## UI Changes Log (2026-05-31) — Google Sheets Integration

- **APPS_SCRIPT_URL** อัปเดตเป็น URL ล่าสุด (deploy ครั้งที่ 4)
  - URL ปัจจุบัน: `AKfycbz4UWRy7EKgDUFht-4aLHNioePl0sMCX-1VYd29lGtNfk_ssX0WxznLO6A8X-1cDFy4Fw`
  - Spreadsheet ID: `1NA7GvcT83I1b2u09rkHHPo2DJsVy5Et_Q92srbF6cqM`
- **Bug: case-insensitive id** — Sheet เก็บ `fmc-187` (lowercase), REAL_EMPLOYEES ใช้ `FMC-187` (uppercase)
  - แก้ใน `apps-script.gs`: `handleLogin` และ `findRow` ใช้ `.toLowerCase()` เปรียบเทียบ
  - แก้ใน `app.js`: สร้าง helper `sameId(a,b)` — ใช้แทน `===` ทุกจุดที่เปรียบ employee id
- **Bug: UI ไม่อัปเดตหลัง submit** — แก้โดยเรียก `loadAllData()` หลัง `apiPost` ทุกจุด
  - `submitStaffSeniorSelfEval` → `loadAllData()` → `renderEmpDashboard()`
  - `submitStaffSeniorMgrEval` → `loadAllData()` → `renderMgrDashboard()`
  - `submitExecDecision` → `loadAllData()` → `renderReport()`

---

## UI Changes Log (2026-05-30)

- **ชื่อระบบ:** APEX REVIEW → **FMC Software**
- **Logo:** ◈ → **FM** (กล่องสีแดง)
- **Primary color:** `#6B4EFF` (ม่วง) → `#E02020` (แดง) ทั้งระบบ
- **position_type:** เพิ่ม `director` (ใช้ Manager Form เหมือน `manager`)
  - ฟังก์ชัน `isManagerType(posType)` → returns true สำหรับ 'manager' และ 'director'
- **Employee Dashboard:** ลบ section "แบบฟอร์มกรอกมือ" + "อัปโหลด" ออกแล้ว
- **โฟลเดอร์:** สร้าง `_ไฟล์ไม่เกี่ยวข้อง/` ย้าย temp files + node_modules + test files

## UI Changes Log (2026-05-31)

- **Department field** — เพิ่มแสดงแผนก (Column K จาก Excel) ใน:
  - Employee Dashboard: แสดงใต้ชื่อ (`emp-group-label`, `emp-dept-label`)
  - Manager Dashboard: คอลัมน์ "แผนก" ในตารางทีม
  - Exec Dashboard: คอลัมน์ "แผนก" + filter dropdown กรองตาม group ที่เลือก
  - Report รายบุคคล: แสดงใน meta line ของ header
- **Modal แก้ไขพนักงาน:**
  - เพิ่ม field "แผนก" (dropdown `modal-dept`) กรองตาม group อัตโนมัติ (`onModalGroupChange()`)
  - หัวหน้า dropdown รวม executive ด้วย (ไม่จำกัดแค่ manager)
- **Exec Dashboard filter:** dept dropdown rebuild ตาม group ที่เลือก (ไม่แสดงทุกแผนกเมื่อเลือก group)
- **HR Dashboard:** ซ่อนปุ่ม "ประเมิน" และ Exec Decision — HR ดูได้อย่างเดียว
- **Exec Dashboard:** ปุ่ม "ประเมิน" → เปลี่ยนเป็น **"ดู / ตัดสินใจ"** → เปิด Report พร้อม Director Decision form
- **Director Decision:** เปลี่ยน label จาก "ผลการตัดสินใจของผู้บริหาร" → **"◈ Director — ผลการตัดสินใจขั้นสุดท้าย"**
  - Executive กรอกได้เสมอแม้ Manager ยังไม่ประเมิน
  - HR และ Manager เห็นแบบ read-only เมื่อ Executive บันทึกแล้ว
- **Exec Dashboard คอลัมน์:** เปลี่ยน "ตัดสินใจ" → **"Director"**
- **คู่มือการใช้งาน** — เพิ่ม view `manual` ในระบบ
  - sidebar: เมนู "? คู่มือการใช้งาน" ทุก role ยกเว้น employee
  - `renderManual()` render content ตาม `currentUser.role` อัตโนมัติ
  - functions: `manualManager()` / `manualExecutive()` / `manualHR()` ใน app.js
  - ปุ่ม "🖨 พิมพ์ / บันทึก PDF" → `printManual()` → `window.print()`
  - CSS class helpers: `.print-only` (ซ่อนปกติ/แสดงตอน print), `.no-print` (ซ่อนตอน print)
  - หน้าปก `.manual-cover` แสดงเฉพาะตอน print พร้อม page-break-after
