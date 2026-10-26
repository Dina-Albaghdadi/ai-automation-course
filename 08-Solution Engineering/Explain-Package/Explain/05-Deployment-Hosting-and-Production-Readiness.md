# 🚀 U8 — Deployment, Hosting and Production Readiness
## From a Tested Solution to a Production-Ready System

---

# 🎯 1. Learning Objectives

بعد الانتهاء من `Requirements → Design → Build → Integration → Testing`، لا يعني ذلك أن الحل أصبح جاهزًا للعميل.

هناك مرحلة أخرى مهمة:

> **كيف ننقل النظام من بيئة التطوير والاختبار إلى بيئة حقيقية يمكن للمستخدمين الاعتماد عليها؟**

في هذه المرحلة، سنتعلم كيف:

- نفهم الفرق بين Development وStaging وProduction.
- نختار Hosting مناسبًا لكل مكوّن.
- نجهز Environment Variables وSecrets.
- ننشر التطبيق والـ Workflows بصورة صحيحة.
- نجهز قاعدة البيانات للاستخدام الحقيقي.
- نتحقق من Production Configuration.
- نطبق أساسيات Security في بيئة الإنتاج.
- ننفذ Production Smoke Tests.
- نجهز Monitoring وLogging.
- نخطط للنسخ الاحتياطي والاستعادة.
- نوثق النظام قبل تسليمه للعميل.
- نحدد ما إذا كان الحل جاهزًا فعليًا للعمل.

---

# 🧠 LAYER 1 — Development Is Not Production

## 2. The Three Main Environments

من الأخطاء الشائعة التعامل مع البيئة التي نطوّر فيها النظام وكأنها البيئة التي سيستخدمها العميل.

يفضل، عندما يكون ذلك مناسبًا للمشروع، الفصل بين:

```text
Development
     |
     v
Staging
     |
     v
Production
```

## 2.1 Development

هي البيئة التي نستخدمها أثناء البناء والتجربة.

قد تتضمن:

- بيانات تجريبية.
- Workflows قيد التعديل.
- Prompts قيد التطوير.
- Debug Logs.
- تغييرات متكررة.
- إعدادات مؤقتة.

هذه البيئة ليست مناسبة للمستخدمين الحقيقيين.

---

## 2.2 Staging

هي بيئة قريبة قدر الإمكان من Production، نستخدمها لاختبار النسخة التي نريد نشرها.

يمكن استخدامها للتحقق من:

- إعدادات Production-like.
- Integration.
- Database migrations.
- Authentication.
- Environment Variables.
- Webhooks.
- AI workflows.
- Production-like user journeys.

ليست كل المشاريع الصغيرة تحتاج بيئة Staging منفصلة، لكن يجب معرفة فائدتها واتخاذ القرار بناءً على المخاطر وحجم النظام.

---

## 2.3 Production

هي البيئة التي يتعامل معها المستخدم الحقيقي.

يجب أن تكون:

- مستقرة.
- مؤمنة.
- موثقة.
- مراقبة.
- مرتبطة بالخدمات الصحيحة.
- منفصلة عن بيانات الاختبار.
- قابلة للصيانة.

### Engineering Principle

> Never treat Production as a testing playground.

لا نجرب تغييرات غير مدروسة مباشرة على بيئة العميل.

---

# 🏗️ LAYER 2 — Understand the Deployment Architecture

## 3. Deployment Is a System, Not a Button

عندما نقول:

> “نشرنا التطبيق”

فهذا لا يعني بالضرورة أن كل النظام أصبح جاهزًا.

قد يتكون الحل من:

```text
                    ┌─────────────────┐
                    │     User        │
                    └────────┬────────┘
                             |
                             v
                    ┌─────────────────┐
                    │   Web App       │
                    │   Lovable       │
                    └────────┬────────┘
                             |
                             v
                    ┌─────────────────┐
                    │ Server-Side     │
                    │ Function        │
                    └────────┬────────┘
                             |
                             v
                    ┌─────────────────┐
                    │   n8n Workflow  │
                    └───────┬─┬───────┘
                            | |
                  ┌─────────┘ └──────────┐
                  v                      v
          ┌──────────────┐       ┌──────────────┐
          │ AI Provider  │       │  Supabase    │
          └──────────────┘       └──────────────┘
```

كل مكوّن يحتاج إلى إعداد صحيح في Production.

لذلك، عند التخطيط للنشر، نسأل:

- أين يعمل التطبيق؟
- أين تعمل Server-Side Functions؟
- أين يعمل Workflow؟
- أين توجد قاعدة البيانات؟
- أين تحفظ Secrets؟
- كيف تتصل المكونات؟
- ما عنوان Production Webhook؟
- كيف نراقب الأخطاء؟
- كيف نحدث النظام لاحقًا؟

---

# ☁️ LAYER 3 — Hosting

## 4. What Is Hosting?

`Hosting` يعني توفير بيئة يمكن فيها تشغيل التطبيق أو الخدمة والوصول إليها عبر الشبكة.

لكن لا يوجد Hosting واحد لكل شيء.

قد يكون لدينا:

| Component | Possible Hosting |
|---|---|
| Frontend | Web Hosting Platform |
| Server-Side Functions | Serverless / Backend Platform |
| Database | Managed Database |
| n8n | n8n Cloud أو Self-hosted |
| AI Provider | External API |
| Static Assets | CDN / Web Hosting |
| Domain | Domain Provider |

اختيار الخدمة يعتمد على:

- التكلفة.
- سهولة الإدارة.
- متطلبات المشروع.
- Security.
- Scalability.
- Availability.
- خبرة الفريق.
- متطلبات العميل.

### Important Principle

> Choose infrastructure based on requirements, not popularity.

لا نختار أداة لمجرد أنها مشهورة.

---

# 🌐 LAYER 4 — Domain and HTTPS

## 5. Production URL

يحتاج التطبيق المنشور عادةً إلى عنوان يمكن للمستخدم الوصول إليه.

مثل:

```text
https://app.example.com
```

وجود Domain مخصص قد يكون مهمًا خصوصًا في الحلول التجارية.

لكن المهم أكثر هو أن يكون الاتصال آمنًا باستخدام:

```text
HTTPS
```

## 5.1 Why HTTPS Matters

HTTPS يساعد على حماية البيانات أثناء انتقالها بين المستخدم والخادم.

يصبح مهمًا خصوصًا عندما يتعامل النظام مع:

- حسابات المستخدمين.
- بيانات العملاء.
- Authentication.
- Forms.
- APIs.
- بيانات شخصية.

لا ينبغي تصميم Production System بحيث يعتمد على HTTP غير المشفر عندما تكون البيانات حساسة.

---

# 🔐 LAYER 5 — Environment Variables and Secrets

## 6. Configuration Should Not Be Hardcoded

قد يحتاج النظام إلى:

- API Keys.
- Database URLs.
- Webhook URLs.
- Authentication Secrets.
- Service Credentials.

لا يجب وضع القيم السرية مباشرة داخل كود التطبيق أو Workflow أو GitHub Repository.

بدلًا من ذلك، نستخدم:

```text
Environment Variables
```

أو Secret Management المناسب للمنصة.

### Example

بدلًا من:

```text
N8N_WEBHOOK_URL = "https://example.com/webhook/abc123..."
```

داخل الكود، نستخدم:

```text
N8N_WEBHOOK_URL = Environment Variable
```

ثم يقرأها التطبيق من بيئة التشغيل.

---

## 6.1 Public vs Secret Configuration

ليس كل Configuration سرًا.

مثال:

```text
APP_NAME
PUBLIC_API_URL
N8N_WEBHOOK_URL
```

قد تكون بعض هذه القيم غير حساسة حسب التصميم.

بينما:

```text
API_KEY
DATABASE_PASSWORD
SERVICE_ROLE_KEY
PRIVATE_TOKEN
```

هي أسرار ويجب حمايتها.

يجب تقييم كل قيمة وفق طبيعتها، وليس اعتبار كل Environment Variable سرًا تلقائيًا.

---

## 6.2 Frontend Security

هناك نقطة مهمة جدًا:

> Environment Variable الموجود داخل Frontend Bundle ليس سرًا.

إذا احتاج المتصفح إلى قيمة معينة، فمن المحتمل أن يتمكن المستخدم من رؤيتها.

لذلك:

```text
Secret
   ↓
Server Environment
   ↓
Server-Side Function
   ↓
External Service
```

أفضل من:

```text
Secret
   ↓
Frontend
   ↓
Browser
```

عند التعامل مع المفاتيح السرية.

---

# 🗃️ LAYER 6 — Production Database

## 7. Prepare the Database

قاعدة بيانات Production ليست مجرد نسخة من قاعدة بيانات Development.

يجب مراجعة:

- Schema.
- Constraints.
- Relationships.
- Indexes عند الحاجة.
- RLS Policies.
- Authentication.
- Data retention.
- Backup strategy.
- Test data.
- Initial production data.

## 7.1 Never Mix Test Data With Production Data

من الأخطاء الخطيرة أن تحتوي قاعدة بيانات العميل على:

```text
Test User
Demo User
Fake Customer
Verification test request
```

بجانب البيانات الحقيقية دون تمييز واضح.

يجب أن تكون بيانات الاختبار منفصلة، أو يتم تنظيفها قبل Production.

---

## 7.2 Database Migrations

عند تعديل Schema بعد النشر، لا نعدل Production بصورة عشوائية.

يجب أن يكون لدينا طريقة منظمة لتطبيق التغييرات.

مثال:

```text
Version 1
   ↓
Version 2
   ↓
Version 3
```

كل تغيير مهم يجب أن يكون معروفًا وقابلًا للتتبع.

### Example

إضافة:

```text
estimated_duration
```

إلى جدول `project_plans` يجب أن تكون عملية مقصودة وموثقة، وليس تعديلًا يدويًا غير معروف.

---

# 🔄 LAYER 7 — Production Workflow

## 8. Deploying n8n Workflows

إذا كان النظام يستخدم n8n، يجب التأكد من أن Workflow المستخدم في Production هو النسخة الصحيحة.

راجعي:

- Workflow مفعّل.
- Production Webhook URL صحيح.
- Credentials صحيحة.
- Environment Variables صحيحة.
- AI Model صحيح.
- Supabase connection صحيحة.
- Error handling موجود.
- Workflow لا يعتمد على بيانات اختبار.
- لا توجد Nodes مؤقتة أو Debug logic غير مطلوب.

### Test vs Production Webhook

قد يكون لدينا:

```text
Development / Test Webhook
```

و:

```text
Production Webhook
```

يجب ألا يستخدم التطبيق Production رابط الاختبار بالخطأ.

### Example

```text
Development
    |
    v
Test Webhook
    |
    v
Test Database
```

بينما:

```text
Production
    |
    v
Production Webhook
    |
    v
Production Database
```

الفصل بينهما يقلل خطر إرسال بيانات العميل إلى بيئة الاختبار.

---

# 🧩 LAYER 8 — Production Configuration

## 9. Configuration Review

قبل النشر، لا يكفي أن نقول:

> “Everything works.”

يجب مراجعة إعدادات Production.

### Configuration Checklist

#### Frontend

- [ ] Production API URL.
- [ ] Production Supabase configuration.
- [ ] No test URLs.
- [ ] No debug UI.
- [ ] No fake data.
- [ ] Correct application branding.
- [ ] Error pages configured.

#### Server-Side Functions

- [ ] Production secrets.
- [ ] Correct external endpoints.
- [ ] Authentication checks.
- [ ] Error handling.
- [ ] Logging.

#### n8n

- [ ] Production Webhook.
- [ ] Correct credentials.
- [ ] Correct AI provider/model.
- [ ] Correct database.
- [ ] Workflow active.
- [ ] Error handling.

#### Database

- [ ] Correct schema.
- [ ] RLS.
- [ ] Production data.
- [ ] Backup strategy.
- [ ] No unnecessary test records.

---

# 🛡️ LAYER 9 — Production Security

## 10. Security Review Before Launch

قبل إعطاء الرابط للعميل، يجب إجراء Security Review أساسي.

### 10.1 Authentication

إذا كان التطبيق يحتاج حسابات:

- هل يستطيع المستخدم تسجيل الدخول؟
- هل يستطيع الوصول فقط إلى بياناته؟
- ماذا يحدث عند تسجيل الخروج؟
- ماذا يحدث عند انتهاء الجلسة؟

### 10.2 Authorization

Authentication يجيب:

> Who are you?

Authorization يجيب:

> What are you allowed to do?

وجود Login لا يعني أن المستخدم يستطيع الوصول إلى كل البيانات.

### 10.3 Database Security

تحققي من:

- RLS Policies.
- Public access.
- Service Role usage.
- API exposure.
- Direct database access.

### 10.4 Secrets

تحققي من:

- عدم وجود API Keys في GitHub.
- عدم وجود Passwords في Screenshots.
- عدم وجود Tokens داخل Frontend code.
- عدم وجود Secrets داخل Logs.

---

# 🧹 LAYER 10 — Remove Development Artifacts

## 11. Production Cleanup

أثناء التطوير، قد نستخدم:

- Fake data.
- Temporary buttons.
- Debug logs.
- Test credentials.
- Test Webhooks.
- Placeholder text.
- Development-only pages.

قبل Production، يجب مراجعتها وإزالتها أو تعطيلها.

### Example

بدل أن تعرض الواجهة:

```text
AI plan generation will be available once n8n is connected.
```

يجب أن تعرض حالة حقيقية مرتبطة بحالة النظام الفعلية.

كذلك لا يجب أن يحتوي التطبيق على:

```text
Test Plan
Demo Customer
Fake Result
```

إذا كان المستخدم النهائي يراها على أنها بيانات حقيقية.

---

# 🧪 LAYER 11 — Production Smoke Testing

## 12. What Is a Smoke Test?

بعد النشر، نحتاج إلى اختبار سريع للتأكد من أن الوظائف الأساسية تعمل في Production.

لا يكون هذا اختبارًا شاملًا مثل مرحلة Testing، بل مجموعة اختبارات أساسية عالية القيمة.

### Example

بعد نشر PlanCraft AI:

1. افتحي Production URL.
2. أنشئي مستخدمًا تجريبيًا آمنًا إذا كان النظام يحتاج Authentication.
3. أرسلي طلبًا بسيطًا.
4. تحققي من حفظ الطلب.
5. تحققي من بدء Workflow.
6. تحققي من معالجة AI.
7. تحققي من حفظ الخطة.
8. تحققي من تحديث الحالة.
9. افتحي صفحة الخطة.
10. تحققي من ظهور البيانات الصحيحة.

### Smoke Test Principle

> Test the real production path, not a simulated version of it.

إذا كان التطبيق الحقيقي يستخدم:

```text
Production Frontend
→ Production Function
→ Production n8n
→ Production Supabase
```

فيجب أن يختبر Smoke Test هذا المسار الحقيقي.

---

# 📊 LAYER 12 — Monitoring and Observability

## 13. Production Needs Visibility

بعد النشر، لا ينتهي دورنا.

قد يفشل النظام بعد يوم من التشغيل بسبب:

- API rate limit.
- Expired credential.
- External service outage.
- Database issue.
- Unexpected user input.
- Workflow failure.
- Model behavior change.
- Resource limits.

لذلك نحتاج إلى معرفة:

> What is happening in production?

### Monitor Important Signals

يمكن متابعة:

- عدد الطلبات.
- عدد الطلبات الناجحة.
- عدد الطلبات الفاشلة.
- Workflow failures.
- Execution duration.
- Timeouts.
- API errors.
- Requests stuck in `processing`.
- Database errors.
- AI evaluation metrics عند الحاجة.

لا تحتاج كل المشاريع إلى نظام Monitoring معقد.

المطلوب هو مستوى من الرؤية يتناسب مع أهمية النظام.

---

# 🚨 LAYER 13 — Alerts

## 14. When Should We Be Notified?

ليس من المفيد تسجيل كل شيء وإرسال تنبيه لكل حدث.

نحتاج إلى تنبيهات للأحداث التي تتطلب تدخلًا.

أمثلة:

- Workflow يفشل عدة مرات متتالية.
- عدد كبير من الطلبات عالق في `processing`.
- خدمة خارجية لا تستجيب.
- Credential منتهية أو غير صالحة.
- معدل الأخطاء ارتفع بصورة غير طبيعية.
- استهلاك مورد تجاوز حدًا معينًا.

### Alert Principle

> An alert should lead to an action.

إذا لم يكن هناك إجراء مطلوب عند حدوث التنبيه، فقد لا يكون تنبيهًا مفيدًا.

---

# 💾 LAYER 14 — Backup and Recovery

## 15. Backup Is Not the Same as Recovery

وجود Backup لا يعني أن النظام قابل للاستعادة.

يجب أن نسأل:

- هل توجد نسخة احتياطية؟
- كم مرة يتم أخذها؟
- أين تحفظ؟
- كم مدة الاحتفاظ بها؟
- هل يمكن استعادتها؟
- كم يستغرق الاسترجاع؟
- من المسؤول عن تنفيذ Recovery؟

### Recovery Test

إذا كان النظام مهمًا، يجب اختبار الاستعادة بصورة دورية وفق سياسة مناسبة.

### Two Important Concepts

**RPO — Recovery Point Objective**

كم مقدار البيانات الذي يمكن أن نخسره زمنيًا؟

مثال:

إذا كان RPO = 1 hour، فهذا يعني أن التصميم يستهدف ألا تتجاوز خسارة البيانات ساعة واحدة في أسوأ حالة مقبولة.

**RTO — Recovery Time Objective**

كم من الوقت نستطيع أن نتحمل قبل إعادة النظام للعمل؟

مثال:

إذا كان RTO = 4 hours، فالمطلوب أن يكون النظام قابلًا للاستعادة خلال أربع ساعات وفق الخطة المحددة.

هذه الأرقام ليست افتراضات؛ يجب تحديدها بناءً على أهمية النظام ومتطلبات العميل.

---

# 🔄 LAYER 15 — Deployment Strategy

## 16. How Do We Release Changes?

عندما نريد تحديث Production، يمكن أن تكون لدينا استراتيجيات مختلفة.

### 16.1 Direct Deployment

ننشر التحديث مباشرة.

مناسب أحيانًا للمشاريع الصغيرة منخفضة المخاطر.

لكن يجب أن يسبقه اختبار مناسب ووجود طريقة للتراجع عند الحاجة.

---

## 16.2 Staged Release

نختبر النسخة في Staging أولًا:

```text
Development
    ↓
Testing
    ↓
Staging
    ↓
Production
```

هذا يقلل احتمال اكتشاف مشكلات كبيرة بعد النشر.

---

## 16.3 Rollback

يجب التفكير مسبقًا:

> What happens if the new version breaks Production?

يجب أن نعرف:

- كيف نكتشف المشكلة؟
- كيف نوقف التغيير؟
- كيف نرجع للنسخة السابقة؟
- ماذا يحدث للبيانات التي تم تعديلها؟
- هل يمكن عكس Database Migration؟
- هل يجب تعطيل Workflow الجديد؟

### Engineering Principle

> A deployment is not complete until you know how to recover from a bad deployment.

---

# 🧑‍💻 LAYER 16 — GitHub and Version Control

## 17. GitHub Is Part of the Engineering System

GitHub ليس فقط مكانًا لتخزين الملفات.

يمكن استخدامه لإدارة:

- Source Code.
- Prompts.
- Documentation.
- Configuration templates.
- Version history.
- Issues.
- Releases.
- Collaboration.

### Repository Structure Example

```text
project/
│
├── README.md
│
├── app/
│
├── database/
│   ├── schema.md
│   └── migrations/
│
├── workflows/
│   └── n8n-workflow.md
│
├── prompts/
│   └── system-prompt.md
│
├── docs/
│   ├── architecture.md
│   ├── data-contract.md
│   ├── testing.md
│   └── deployment.md
│
└── .env.example
```

لا نضع:

```text
.env
```

أو أي ملف يحتوي أسرارًا حقيقية داخل Repository عام.

بدلًا من ذلك يمكن استخدام:

```text
.env.example
```

يحتوي على أسماء المتغيرات المطلوبة دون قيمها السرية.

---

# 🏷️ LAYER 17 — Versioning

## 18. Know What Is Running

عندما يحدث خطأ في Production، يجب أن نعرف:

> Which version is running?

يمكن استخدام Versioning واضح، مثل:

```text
v1.0.0
v1.1.0
v1.1.1
```

أو نظام Versioning مناسب للمشروع.

يجب أن نعرف على الأقل:

- نسخة التطبيق.
- نسخة Workflow.
- نسخة Prompt المهمة.
- نسخة Schema عند الحاجة.
- Model المستخدم.
- تاريخ آخر Deployment.

هذا يجعل التشخيص والـ Rollback أكثر وضوحًا.

---

# 📖 LAYER 18 — Production Documentation

## 19. The System Must Be Understandable Without Its Builder

إذا كنتِ الشخص الوحيد الذي يعرف كيف يعمل النظام، فهذه مخاطرة.

يجب أن يستطيع شخص آخر فهم:

- ما الذي يفعله النظام؟
- كيف يعمل؟
- ما مكوناته؟
- أين توجد البيانات؟
- كيف يبدأ Workflow؟
- ما الخدمات الخارجية؟
- أين توجد Secrets؟
- كيف يتم النشر؟
- كيف يتم الاختبار؟
- ماذا نفعل إذا حدث خطأ؟

### Minimum Documentation

يجب أن يحتوي المشروع على:

```text
README.md
Architecture
Data Model
Integration Flow
Environment Variables
Deployment Steps
Testing Guide
Known Issues
Maintenance Notes
```

---

# 📘 LAYER 19 — README for the Client or Team

## 20. Recommended README Structure

يمكن استخدام:

```markdown id="0s6nko"
# Project Name

## Overview
What does this system do?

## Problem
What problem does it solve?

## Users
Who uses the system?

## Architecture
Describe the main components.

## Main Flow
Explain how data moves through the system.

## Technologies
- Frontend
- Database
- Workflow
- AI
- Hosting

## Environment Variables
List required variable names only.

## Local / Development Setup
How to run the project.

## Production
Describe the production environment.

## Testing
How to run or perform the main tests.

## Deployment
How the system is deployed.

## Monitoring
How to check system health.

## Troubleshooting
Common problems and solutions.

## Known Limitations
What the system does not currently support.

## Maintenance
What should be checked regularly.
```

---

# 🤝 LAYER 20 — Client Handover Readiness

## 21. The System Is Not Finished Until It Can Be Handed Over

`Handover` يعني نقل المعرفة والمسؤولية التشغيلية إلى العميل أو الفريق الذي سيستخدم النظام أو يديره.

يجب ألا يكون التسليم:

> “Here is the link. Good luck.”

بل يجب أن يتضمن:

- رابط Production.
- حسابات الوصول المناسبة.
- طريقة استخدام النظام.
- شرح الوظائف الأساسية.
- حدود النظام.
- طريقة التعامل مع الأخطاء.
- جهة التواصل للدعم.
- معلومات الصيانة.
- الوثائق.
- سياسة التحديثات.

---

# 👩‍🏫 LAYER 21 — Client Training

## 22. Train the User on the Product, Not the Technology

ليس مطلوبًا أن نشرح للعميل كل تفاصيل:

- n8n Nodes.
- Supabase Tables.
- API Requests.
- AI Prompt Architecture.

إلا إذا كان ذلك جزءًا من نطاق التسليم.

المستخدم النهائي يحتاج غالبًا إلى معرفة:

- كيف يستخدم النظام؟
- ماذا يفعل كل جزء؟
- ماذا يفعل إذا حدث خطأ؟
- ما الذي يستطيع النظام فعله؟
- ما الذي لا يستطيع فعله؟

### Example

بدل:

> “The system uses a Webhook Trigger connected to an AI Agent.”

نشرح للمستخدم:

> “بعد إرسال الطلب، سيبدأ النظام معالجته تلقائيًا، وستظهر النتيجة عند اكتمالها.”

نُبقي مستوى الشرح مناسبًا للشخص الذي أمامنا.

---

# 🛠️ LAYER 22 — Maintenance

## 23. Production Is the Beginning of Maintenance

بعد الإطلاق، يحتاج النظام إلى متابعة.

قد تشمل Maintenance:

### Technical Maintenance

- تحديث Dependencies.
- تحديث Workflow.
- تحديث API integrations.
- مراقبة الخدمات الخارجية.
- إصلاح Bugs.
- تحسين Performance.

### AI Maintenance

- مراجعة جودة المخرجات.
- تحديث Knowledge Base.
- إعادة تقييم Prompts.
- تقييم Model changes.
- مراجعة Regression Tests.
- تحديث Evaluation Dataset.

### Data Maintenance

- تنظيف البيانات حسب السياسة.
- Backup.
- Archiving.
- مراجعة حجم البيانات.
- مراجعة صلاحيات الوصول.

---

# 📅 LAYER 23 — Maintenance Plan

## 24. Define What Happens After Launch

يمكن إنشاء جدول بسيط:

| Frequency | Activity |
|---|---|
| Daily / As Needed | Check critical failures |
| Weekly | Review workflow errors |
| Monthly | Review usage and performance |
| Periodically | Review AI quality |
| Periodically | Review access permissions |
| Before major update | Run regression tests |
| After deployment | Run production smoke tests |

ليست هذه فترات إلزامية لكل مشروع.

يجب تحديدها بناءً على أهمية النظام وحجم الاستخدام ومخاطره.

---

# 💰 LAYER 24 — Production Cost Awareness

## 25. Production Has Ongoing Costs

عند بناء حل حقيقي، لا نحسب فقط تكلفة البناء.

يجب التفكير في:

- Hosting.
- Database.
- Workflow execution.
- AI API usage.
- Storage.
- Email/SMS/WhatsApp/Telegram services عند استخدامها.
- Domain.
- Monitoring.
- Maintenance.

### Example

قد يكون التطبيق مجانيًا أثناء التطوير، لكنه يصبح مكلفًا عند استخدامه من 1,000 مستخدم.

لذلك يجب التفكير في:

```text
Cost per User
Cost per Request
AI Token Cost
Workflow Execution Cost
Storage Cost
Infrastructure Cost
```

ثم تقدير التكلفة وفق الاستخدام المتوقع.

---

# ⚠️ LAYER 25 — Production Readiness Review

## 26. Final Readiness Questions

قبل أن نقول:

> “The system is ready.”

نسأل:

### Functionality

- [ ] هل المتطلبات الأساسية تعمل؟
- [ ] هل Acceptance Criteria تحققت؟
- [ ] هل المستخدم يستطيع إكمال المهمة الأساسية؟

### Integration

- [ ] هل كل المكونات متصلة؟
- [ ] هل Production URLs صحيحة؟
- [ ] هل Data Contracts متوافقة؟

### Security

- [ ] هل الأسرار محمية؟
- [ ] هل الصلاحيات صحيحة؟
- [ ] هل RLS أو Access Control مفعّل عند الحاجة؟
- [ ] هل يمكن الوصول إلى بيانات مستخدم آخر؟

### Reliability

- [ ] هل تم اختبار Failure Cases؟
- [ ] هل تم التعامل مع Duplicate Requests؟
- [ ] هل توجد Error Handling مناسبة؟
- [ ] هل توجد طريقة لمعرفة الطلبات العالقة؟

### AI

- [ ] هل تم تقييم مخرجات AI؟
- [ ] هل توجد Evaluation Dataset مناسبة؟
- [ ] هل تم اختبار المعلومات الناقصة؟
- [ ] هل تم اختبار عدم اختلاق المعلومات؟
- [ ] هل تم اختبار RAG أو Agent عند استخدامهما؟

### Deployment

- [ ] هل Production Environment منفصل عن Test Environment؟
- [ ] هل تم اختبار Production URL؟
- [ ] هل تم تنفيذ Smoke Test؟
- [ ] هل تم إزالة بيانات الاختبار؟

### Operations

- [ ] هل توجد Logs مناسبة؟
- [ ] هل توجد Monitoring مناسبة؟
- [ ] هل توجد Backup / Recovery Plan مناسبة؟
- [ ] هل توجد Documentation؟
- [ ] هل توجد Maintenance Plan؟

### Handover

- [ ] هل العميل يعرف كيف يستخدم النظام؟
- [ ] هل يعرف ما الذي يستطيع النظام فعله؟
- [ ] هل يعرف ما الذي لا يستطيع فعله؟
- [ ] هل يعرف كيف يطلب الدعم؟

---

# 🧪 LAYER 26 — Production Smoke Test Template

استخدمي القالب التالي بعد كل Deployment مهم:

```markdown id="x1j7pf"
# Production Smoke Test

## Deployment
- Version:
- Date:
- Environment: Production

## Test 1 — Application Access
Expected:
Application loads successfully.

Result:
PASS / FAIL

## Test 2 — Main User Action
Expected:
User can complete the primary action.

Result:
PASS / FAIL

## Test 3 — Database
Expected:
Data is saved correctly.

Result:
PASS / FAIL

## Test 4 — Workflow
Expected:
Production workflow executes successfully.

Result:
PASS / FAIL

## Test 5 — AI
Expected:
AI produces a valid result.

Result:
PASS / FAIL

## Test 6 — Result Retrieval
Expected:
The saved result can be retrieved.

Result:
PASS / FAIL

## Test 7 — Error Handling
Expected:
Invalid input produces a controlled error.

Result:
PASS / FAIL

## Test 8 — Security
Expected:
Unauthorized access is blocked.

Result:
PASS / FAIL

## Final Decision

- READY
- READY WITH LIMITATIONS
- NOT READY

## Notes
Document any issues found.
```

---

# 📦 LAYER 27 — Production Handover Package

## 28. What Should We Deliver?

عند تسليم حل للعميل، يمكن أن يحتوي Handover Package على:

```text
01 — Production URL
02 — User Guide
03 — Architecture Overview
04 — Data Model
05 — Integration Documentation
06 — Deployment Documentation
07 — Testing Report
08 — Known Limitations
09 — Maintenance Plan
10 — Support Information
```

ولا يجب أن تحتوي حزمة التسليم على Secrets أو كلمات مرور حساسة غير ضرورية.

إذا احتاج العميل إلى Credentials، يجب نقلها عبر طريقة آمنة مناسبة بدل وضعها داخل مستند عام أو GitHub.

---

# 🛠️ LAYER 28 — Practical Exercise

## Exercise: Make Your Solution Production-Ready

اختاري مشروعًا بنيته سابقًا، مثل:

- PlanCraft AI.
- Real Estate Lead Qualification Agent.
- TECHFIX RAG Assistant.
- AI Client Intake and Request Routing.

الهدف الآن ليس إضافة Features جديدة.

الهدف هو تحويل الحل الحالي إلى نظام يمكن التفكير في تسليمه إلى مستخدم حقيقي.

---

## Task 1 — Deployment Architecture

ارسمي Architecture توضح:

```text
User
 ↓
Frontend
 ↓
Server-Side Function
 ↓
Workflow
 ↓
AI / External Services
 ↓
Database
```

عدّلي الرسم وفق مشروعك الحقيقي.

---

## Task 2 — Environment Separation

حددي:

| Component | Development | Production |
|---|---|---|
| Frontend | ? | ? |
| Database | ? | ? |
| Workflow | ? | ? |
| AI Provider | ? | ? |
| Webhook | ? | ? |

ثم اكتبي ما الذي يجب ألا يختلط بين البيئتين.

---

## Task 3 — Secrets Audit

ابحثي في المشروع عن:

- API Keys.
- Tokens.
- Passwords.
- Service Role Keys.
- Webhook Secrets.
- Database Credentials.

تحققي من أنها ليست موجودة في:

- Frontend Code.
- GitHub.
- Screenshots.
- Public Documentation.
- Logs.

---

## Task 4 — Production Configuration Review

راجعي:

- Production URL.
- Production Webhook.
- Database.
- Credentials.
- Environment Variables.
- AI Model.
- Workflow Status.
- Authentication.
- Access Control.

---

## Task 5 — Production Smoke Test

نفّذي Smoke Test حقيقي على Production.

لا تكتفي بأن التطبيق يفتح.

اختبري:

```text
User Action
   ↓
Frontend
   ↓
Backend / Server Function
   ↓
Workflow
   ↓
AI
   ↓
Database
   ↓
Result
```

وسجلي النتيجة.

---

## Task 6 — Handover Documentation

أنشئي:

```text
README.md
deployment.md
testing.md
architecture.md
maintenance.md
user-guide.md
```

يجب أن يستطيع شخص آخر قراءة الملفات وفهم كيفية تشغيل النظام واستخدامه وصيانته.

---

# 📊 LAYER 29 — Production Readiness Report

استخدمي القالب التالي:

```markdown id="f0rj6n"
# Production Readiness Report

## 1. Project
- Project Name:
- Version:
- Date:

## 2. Production Environment
- Frontend:
- Database:
- Workflow:
- AI Provider:
- Hosting:

## 3. Deployment Status
- Application:
- Database:
- Workflow:
- Integrations:

## 4. Security Review
- Secrets:
- Authentication:
- Authorization:
- Database Access:
- Public Exposure:

## 5. Testing
- Functional:
- Integration:
- E2E:
- AI Evaluation:
- Security:
- Smoke Test:

## 6. Monitoring
- Logs:
- Error Tracking:
- Workflow Monitoring:
- Alerts:

## 7. Backup and Recovery
- Backup Strategy:
- Recovery Strategy:
- RPO:
- RTO:

## 8. Known Limitations
List limitations that are accepted or still unresolved.

## 9. Maintenance
Describe ongoing maintenance responsibilities.

## 10. Handover
- User Guide:
- Technical Documentation:
- Support Contact:
- Training:

## 11. Final Decision

- Production Ready
- Ready With Limitations
- Not Ready

## 12. Approval / Acceptance

Document who reviewed and accepted the solution.
```

---

# ✅ Definition of Done

تكون مرحلة `Deployment, Hosting and Production Readiness` مكتملة عندما:

- [ ] تم تحديد بيئة Production.
- [ ] تم فصل بيانات الاختبار عن بيانات الإنتاج.
- [ ] تم إعداد Hosting المناسب.
- [ ] تم إعداد Production URLs.
- [ ] تم تأمين الاتصال باستخدام HTTPS عند الحاجة.
- [ ] تم نقل Secrets إلى بيئة آمنة.
- [ ] تم مراجعة Database Security.
- [ ] تم نشر Workflow الصحيح.
- [ ] تم استخدام Production Webhooks الصحيحة.
- [ ] تم إزالة أو تعطيل مكونات التطوير غير المطلوبة.
- [ ] تم تنفيذ Production Smoke Test.
- [ ] توجد آلية مناسبة للمراقبة والتشخيص.
- [ ] توجد خطة Backup / Recovery مناسبة لأهمية النظام.
- [ ] توجد طريقة Rollback أو Recovery مناسبة.
- [ ] تم توثيق النظام.
- [ ] أصبح العميل قادرًا على استخدام الحل.
- [ ] تم تحديد مسؤوليات Maintenance.
- [ ] تم توثيق القيود المعروفة.
- [ ] تم اتخاذ قرار واضح: Ready / Ready With Limitations / Not Ready.

---

# 🧠 Final Engineering Mindset

في بداية الوحدة كنا نسأل:

> **What problem are we solving?**

ثم:

> **What should we build?**

ثم:

> **How do we build and integrate it?**

ثم:

> **How do we prove that it works?**

والآن:

> **Can someone rely on it in the real world?**

هذه هي النقلة من:

**AI Builder**

إلى:

**Solution Engineer.**

المهندس لا ينتهي دوره عندما ينجح الـ Demo.

بل يسأل:

- أين يعمل النظام؟
- من يستطيع الوصول إليه؟
- ماذا يحدث عندما يفشل؟
- كيف نعرف أنه فشل؟
- كيف نصلحه؟
- كيف نحدّثه؟
- كيف نحمي بيانات العميل؟
- كيف نسلّمه؟
- من سيصونه بعد ذلك؟
- وهل يمكن للعميل الاعتماد عليه فعلًا؟

> **A demo proves that an idea can work.  
> Production readiness proves that a system can be trusted.**

---

# ➡️ Next: U8 — Client Handover, Documentation and Maintenance

في الملف التالي سننتقل من **Production Readiness** إلى الجانب المهني والتجاري من Solution Engineering:

- كيف نسلّم الحل للعميل؟
- ماذا نوثّق؟
- كيف ندرّب المستخدم؟
- كيف نحدد Scope وLimitations؟
- كيف نحدد Support وMaintenance؟
- كيف نتعامل مع Change Requests؟
- كيف نحول الحل من مشروع تم بناؤه إلى خدمة يمكن إدارتها؟
- وكيف نجهز Proposal وHandover Package بصورة احترافية؟
