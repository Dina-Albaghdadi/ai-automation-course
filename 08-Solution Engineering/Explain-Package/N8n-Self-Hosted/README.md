# 🚀 n8n Self-Hosted — Learning Roadmap
## From Windows to Production

### 🎯 الهدف
تعلّم تثبيت n8n وإدارته باستخدام Docker، ثم نشره على VPS ليعمل باستمرار.

---

## Phase 1 — Install Docker on Windows

**الهدف:** تجهيز بيئة التشغيل.

**تعلّمي:**
- Docker Desktop
- WSL 2
- Docker Engine

**المصدر:**  
[Docker Desktop Installation on Windows](https://docs.docker.com/desktop/setup/install/windows-install/)

[Docker with WSL 2](https://docs.docker.com/desktop/features/wsl/)

**التطبيق:** ثبّتي Docker Desktop وتأكدي من عمله.

---

## Phase 2 — Run n8n Locally

**الهدف:** تشغيل n8n على جهازك.

**الفيديو التطبيقي:**  
[Install n8n Locally with Docker Desktop](https://www.youtube.com/watch?v=6ET3G7GiqZA)

**تعلّمي:**
- Docker Image
- Container
- Port Mapping
- Volume

**التطبيق:** شغّلي n8n وافتحيه من المتصفح عبر `localhost:5678`.

---

## Phase 3 — Manage n8n with Docker Compose

**الهدف:** إدارة n8n من ملف إعدادات واحد.

**تعلّمي:**
- `compose.yaml`
- Services
- Environment Variables
- Volumes
- تشغيل الحاويات وإيقافها ومراجعة Logs

**المصدر:**  
[n8n — Docker Compose Installation](https://github.com/n8n-io/n8n-docs/blob/main/docs/deploy/host-n8n/install-options/install-using-docker-compose.md)

**التطبيق:** أنشئي ملف `compose.yaml` وشغّلي n8n من خلاله، ثم أوقفيه وأعيدي تشغيله وتحققي من بقاء بياناتك.

---

## Phase 4 — Data and Backups

**الهدف:** حماية بيانات n8n.

**تعلّمي:**
- Persistent Volumes
- SQLite
- Credentials Encryption
- Backup and Restore

**المصدر:**  
[n8n — Self-Hosting Documentation](https://github.com/n8n-io/n8n-docs/blob/main/docs/deploy/host-n8n/README.md)

**التطبيق:** أنشئي نسخة احتياطية واختبري استعادة البيانات.

---

## Phase 5 — Deploy n8n on a VPS

**الهدف:** تشغيل n8n على خادم متاح عبر الإنترنت.

**تعلّمي:**
- Linux وSSH
- VPS
- Docker Compose on a Server
- Domain وDNS
- HTTPS وReverse Proxy
- Security and Updates

**المصدر:**  
[n8n — Docker Compose on a Server](https://github.com/n8n-io/n8n-docs/blob/main/docs/deploy/host-n8n/install-options/use-a-cloud-provider/use-docker-compose.md)

**التطبيق:** انشري n8n على VPS، واضبطي الوصول الآمن، واختبري Production Webhooks.

---

## ✅ Final Project

أنشئي بيئة n8n Self-Hosted تحقق المتطلبات التالية:

- [ ] تشغيل n8n باستخدام Docker Desktop.
- [ ] إدارة n8n باستخدام Docker Compose.
- [ ] حفظ البيانات واستعادتها.
- [ ] إنشاء Webhook واختباره.
- [ ] نشر n8n على VPS باستخدام HTTPS.
- [ ] توثيق خطوات التثبيت والتشغيل والصيانة.

### مسار التعلم

**Windows → Docker Desktop → n8n → Docker Compose → Backup & Restore → VPS → HTTPS & Security**

**مهم:** أتقني المراحل من 1 إلى 4 أولًا، ثم انتقلي إلى النشر على VPS. لا تحتاجين إلى PostgreSQL في البداية؛ تعلّميه لاحقًا عند الحاجة إلى إعداد أكثر ملاءمة للإنتاج.
