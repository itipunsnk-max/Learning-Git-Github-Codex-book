# Sample workflow: เปลี่ยนแปลงเล็ก ๆ อย่างตรวจสอบได้

```bash
git switch main
git pull --rebase
git switch -c feature/customer-filter
git status
git diff
git add src/customer-filter.js
git diff --staged
git commit -m "feat: add customer filter"
git push -u origin feature/customer-filter
```

ก่อนเปิด Pull Request ให้ตรวจ `git diff main...HEAD`, ทดสอบ และอธิบายสิ่งที่เปลี่ยน/สิ่งที่ยังไม่ได้เปลี่ยน
