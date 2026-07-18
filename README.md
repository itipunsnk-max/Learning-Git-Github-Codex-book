# Git, GitHub, VS Code & Codex — หลักสูตรใช้งานจริง

เอกสารบทเรียนภาษาไทยสำหรับผู้เริ่มต้นที่ต้องการใช้ Git ในโปรเจกต์จริง ตั้งแต่การสร้าง repository, ตรวจ diff, ทำงานกับ branch, แก้ conflict, กู้คืนงาน และตรวจสอบการเปลี่ยนแปลงที่ Codex ช่วยแก้ไข

## เปิดใช้งาน

เปิด [`docs/index.html`](docs/index.html) ใน browser ได้โดยตรง หรือใช้ GitHub Pages ที่ชี้ไปยังโฟลเดอร์รากของ repository แล้วกำหนดไฟล์เริ่มต้นเป็น `docs/index.html` หากโฮสต์รองรับการเลือกโฟลเดอร์ ให้เลือก `/docs` เป็น source

ไม่มี framework หรือ build tool และไม่มี dependency ภายนอก จึงทำงานแบบ offline ได้

## นำโปรเจกต์ขึ้น GitHub

ตัวอย่างนี้ใช้ branch หลักชื่อ `main` และหน้าเว็บอยู่ในโฟลเดอร์ `docs/`

### 1. สร้าง repository บน GitHub

1. เข้า GitHub แล้วเลือก **New repository**
2. ตั้งชื่อ เช่น `git-github-codex-book`
3. ถ้ามีไฟล์โปรเจกต์อยู่ในเครื่องแล้ว แนะนำให้สร้าง repository แบบว่างก่อน โดยไม่ต้องเพิ่ม README หรือ `.gitignore` ซ้ำ
4. คัดลอก URL ของ repository เช่น `https://github.com/<username>/<repository>.git`

### 2. เชื่อมโฟลเดอร์นี้กับ GitHub

เปิด PowerShell หรือ Terminal ที่โฟลเดอร์โปรเจกต์ แล้วรัน:

```bash
git init -b main
git config user.name "Your Name"
git config user.email "you@example.com"
git add .
git status
git diff --cached
git commit -m "docs: publish git field guide"
git remote add origin https://github.com/<username>/<repository>.git
git remote -v
git push -u origin main
```

ถ้าโฟลเดอร์นี้เป็น Git repository อยู่แล้ว ให้ข้าม `git init` และตรวจ `git remote -v` ก่อนเพิ่ม `origin` หากมี `origin` อยู่แล้ว ใช้:

```bash
git remote set-url origin https://github.com/<username>/<repository>.git
git push -u origin main
```

เมื่อ GitHub ขอสิทธิ์ ให้ใช้ Git Credential Manager, SSH หรือ token ตามนโยบายของบัญชี/องค์กร อย่าใส่ token หรือ password ลงใน URL, README หรือไฟล์ source

### 3. เปิดเว็บด้วย GitHub Pages

เพราะหน้าเริ่มต้นอยู่ที่ `docs/index.html` ให้ตั้งค่าใน repository ดังนี้:

1. เปิด repository บน GitHub แล้วไปที่ **Settings → Pages**
2. ใน **Build and deployment** เลือก **Deploy from a branch**
3. เลือก branch `main` และ folder `/docs`
4. กด **Save** แล้วรอ deployment เสร็จ
5. เปิด URL ที่ GitHub แสดงในหน้า Pages

หลังจากแก้บทเรียน ให้ตรวจและส่งขึ้นใหม่:

```bash
git status
git add docs README.md
git diff --cached
git commit -m "docs: update lesson"
git push
```

> GitHub Pages จะเผยแพร่ไฟล์จาก branch/folder ที่เลือกไว้ ดังนั้นอย่าลบโฟลเดอร์ `/docs` หลังตั้งค่า source เป็น `/docs`

## นำเว็บขึ้น Netlify

Netlify ใช้ได้ 2 วิธี: เชื่อมกับ GitHub เพื่อ deploy อัตโนมัติ หรืออัปโหลดโฟลเดอร์ด้วย Drag and drop

### วิธี A: เชื่อม Netlify กับ GitHub (แนะนำ)

1. Push โปรเจกต์ขึ้น GitHub ตามขั้นตอนด้านบน
2. เข้า Netlify แล้วเลือก **Add new project → Import an existing project**
3. เลือก **Deploy with GitHub** และอนุญาตให้ Netlify เข้าถึง repository
4. เลือก repository นี้
5. ตั้งค่า:

   - **Branch to deploy:** `main`
   - **Base directory:** เว้นว่าง
   - **Build command:** เว้นว่าง เพราะเป็นเว็บ static
   - **Publish directory:** `docs`

6. กด **Deploy**

ทุกครั้งที่ push ไปยัง `main` Netlify จะสร้าง deployment ใหม่จากโฟลเดอร์ `docs` ให้โดยอัตโนมัติ

### วิธี B: Drag and drop

ใช้เมื่อไม่ต้องการเชื่อม GitHub:

1. เข้า Netlify แล้วเปิดหน้า **Sites**
2. ลากโฟลเดอร์ `docs` ไปวางในพื้นที่ deploy
3. Netlify จะสร้าง URL ชั่วคราวให้
4. หากต้องการแก้ชื่อเว็บ ให้เข้า **Site configuration → General → Change site name**

วิธีนี้เหมาะกับการทดลองหรือส่งเว็บครั้งเดียว แต่การแก้ไขครั้งต่อไปต้องลากโฟลเดอร์ `docs` ขึ้นใหม่เอง

### ตรวจสอบก่อน deploy

```bash
git status
git diff --check
git log --oneline -5
```

ตรวจด้วยว่า:

- เปิด `docs/index.html` ได้
- ลิงก์บทเรียน 01–07 ใช้งานได้
- ไม่มี token, password, `.env` หรือข้อมูลส่วนตัว
- ไฟล์ `.gitignore` ไม่ปล่อย `node_modules/`, log หรือไฟล์ build ที่ไม่จำเป็น

## โครงสร้าง

```text
README.md
docs/
├── index.html
├── assets/
│   ├── style.css
│   └── script.js
├── images/
├── examples/
│   └── sample-workflow.md
├── git-basics.html
├── diff-and-review.html
├── undo-and-recovery.html
├── branching.html
├── vscode-workflow.html
├── codex-workflow.html
└── troubleshooting.html
.gitignore
git-github-cheatsheet.html  # ไฟล์ต้นฉบับที่เก็บไว้เพื่ออ้างอิง
```

## เส้นทางการเรียน

1. เริ่มจาก [Git คืออะไร](docs/git-basics.html)
2. ฝึกตรวจงานด้วย [diff และ review](docs/diff-and-review.html)
3. เรียน [branch และ merge conflict](docs/branching.html)
4. รู้จัก [การย้อนและกู้คืนงาน](docs/undo-and-recovery.html)
5. ต่อ workflow ใน [VS Code](docs/vscode-workflow.html)
6. ปิดท้ายด้วย [Codex กับ repository](docs/codex-workflow.html)

ทุกคำสั่งใช้ placeholder เช่น `<username>` และ `<repository>` ห้ามแทนด้วย token, password หรือข้อมูลส่วนตัวจริงในไฟล์ตัวอย่าง

## แหล่งอ้างอิงการเผยแพร่

- [GitHub Pages: Configuring a publishing source](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)
- [Netlify: Production deploy](https://docs.netlify.com/deploy/deploy-types/production-deploy/)
