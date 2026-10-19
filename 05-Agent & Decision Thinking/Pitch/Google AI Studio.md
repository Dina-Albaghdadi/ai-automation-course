# 🔎 Research Pair — U5: Control Its Agency

## Topic: Creating a Gemini API Key from Google AI Studio
### (API Integration for AI Agents — Authentication)

| | |
|---|---|
| **المتدربة** | دينا البغدادي |
| **المدربة** | نادية الخطيب |
| **المؤسسة** | حاضنة العمل الحر — مركز شؤون المرأة |
| **الوحدة** | U5: Control Its Agency |
| **المشروع المرتبط** | Real Estate Lead Qualification Agent |

---

## 🎯 فكرة البحث

كيف ننتقل من تجربة نموذج Gemini داخل واجهة Google AI Studio إلى استخدامه داخل نظام حقيقي (مثل n8n)؟ الجواب: عن طريق **API Key** يعرّف الخدمة بهويتنا (Authentication) ويسمح لنظامنا بإرسال Request واستقبال Response.

---

## 1️⃣ What is it? — ما هو؟

**Google AI Studio** بيئة ويب مجانية من Google لتجربة نماذج Gemini مباشرة من المتصفح، واختبار البرومبتات، وطلب مخرجات منظمة (JSON)، ثم تصدير الكود.

**API Key** هو مفتاح سري يربط طلبات برنامجنا بحسابنا ومشروعنا في Google، ويستخدمه Gemini API للتحقق من هوية الطالب (Authentication).

**المشكلة التي يحلها:** بدل أن نبقى داخل واجهة التجربة، يمكن لأي نظام (n8n، Laravel، Python...) أن يستدعي النموذج برمجيًا بشكل آمن ومنظم.

### خطوات إنشاء المفتاح

1. فتح [aistudio.google.com](https://aistudio.google.com) وتسجيل الدخول بحساب Google.
2. من القائمة الجانبية: **Get API key**.
3. الضغط على **Create API key** وكتابة اسم للمفتاح.
4. اختيار مشروع (Project): إنشاء مشروع جديد (**Create project**) أو استخدام **Default Gemini Project**.
5. نسخ المفتاح وحفظه في مكان آمن.

### مفهوم المشروع (Project)

- كل مفتاح يتبع مشروعًا واحدًا.
- **حدود الاستخدام (Rate Limits) تُحسب على مستوى المشروع وليس على مستوى المفتاح.**
- الأفضل إنشاء مشروع منفصل لكل مشروع عمل (مثل `real-estate-agent`) حتى لا يتشارك الحدود والاستخدام مع مشاريع أخرى.

### دورة الطلب والاستجابة (Request / Response)

```bash
curl "https://generativelanguage.googleapis.com/v1beta/models/MODEL_NAME:generateContent" \
  -H "x-goog-api-key: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -X POST \
  -d '{
    "contents": [{
      "parts": [{"text": "Extract the budget from: I want an apartment, budget 60000 USD"}]
    }]
  }'
```

- `POST`: لأننا نرسل بيانات (البرومبت) إلى الخدمة.
- `x-goog-api-key`: المفتاح، وهو الـ **Authentication**.
- **Response:** بيانات JSON فيها رد النموذج.
- اسم النموذج `MODEL_NAME` يؤخذ من زر **Get code** في المنصة لأن الأسماء تتغير.

> ⚠️ المفتاح في المثال وهمي (`YOUR_API_KEY`). لا يتم وضع مفتاح حقيقي في أي ملف أو لقطة شاشة.

---

## 2️⃣ When would you use it? — متى نستخدمه؟

- عندما ننتقل من **التجربة** في AI Studio إلى **التنفيذ** داخل نظام حقيقي.
- عندما يحتاج Workflow في n8n أو تطبيق Laravel إلى استدعاء نموذج Gemini تلقائيًا.
- عندما نريد أن يعمل الوكيل دون تدخل يدوي في كل مرة.

### حدود يجب معرفتها

| النقطة | التوضيح |
|---|---|
| **Rate Limits** | تُقاس بعدد الطلبات بالدقيقة (RPM) والتوكنز بالدقيقة (TPM) والطلبات باليوم (RPD)، وتختلف حسب النموذج. تجاوزها يعطي خطأ **429**. |
| **أين تُحسب؟** | على مستوى **المشروع** وليس المفتاح. |
| **Tiers** | Free ثم Tier 1 عند ربط حساب فوترة بالمشروع، وهكذا. |
| **الأرقام الفعلية** | تُراجع من صفحة Rate limits / Usage في AI Studio لأنها تتغير. |
| **الخصوصية** | في الخطة المجانية قد تُستخدم البيانات لتحسين منتجات Google، لذلك نستخدم بيانات تجريبية فقط. |

### أمان المفتاح

- لا يُرفع على GitHub ولا يوضع في كود الواجهة (Frontend).
- يُخزَّن في `.env` أو في **Credentials** داخل n8n.
- إذا انكشف: يُحذف وينشأ مفتاح جديد من نفس صفحة المفاتيح.

---

## 3️⃣ How can it help our agent? — كيف يفيد وكيل العقارات؟

في مشروع **Real Estate Lead Qualification Agent**:

1. نجرّب **System Prompt** و**JSON Schema** في AI Studio أولًا (الحقول: `full_name`, `contact_number`, `property_type`, `budget`, `preferred_location`).
2. ننشئ API Key من مشروع مخصص للوكيل.
3. نضع المفتاح في **Credentials** داخل n8n.
4. يستدعي الـ Workflow نموذج Gemini عبر الـ API (Request)، فيرجع بيانات العميل منظمة (Response).
5. بعد تأكيد العميل لبياناته، يحفظها الـ Workflow في Google Sheets.

**قبل الاتصال، يحتاج الوكيل إلى معرفة:** عنوان الخدمة (Endpoint)، طريقة الطلب (POST)، المفتاح (Authentication)، شكل البيانات المرسلة، وشكل الرد المتوقع.

**ملاحظة تصميمية:** عند الوصول إلى حد الاستخدام (429)، نستخدم إعادة المحاولة (Retry) مع تأخير بسيط في n8n.

---

## 🎤 Share — ملخص العرض الشفهي (3 دقائق)

- **أهم فكرة جديدة:** المفتاح مرتبط بمشروع، والحدود والفوترة تُحسب على المشروع، لذلك ننشئ مشروعًا منفصلًا لكل مشروع عمل.
- **المشكلة التي يحلها:** يوصل نظامنا بنموذج Gemini بشكل آمن وموثّق بدل الاكتفاء بالتجربة اليدوية.
- **أين نستخدمه:** في وكيل العقارات عبر n8n، وفي أي مشروع AI Automation مستقبلي.

---

## ✅ Research Pair Checklist

- [x] فهمتُ الفكرة الأساسية.
- [x] استكشفتُ الأداة (Google AI Studio).
- [x] استطعتُ شرح استخدام عملي واحد (استدعاء Gemini من Workflow).
- [x] ربطتُ الفكرة بمشروع وكيل العقارات.
- [x] استعددتُ لشرح ما تعلمته في 3 دقائق.

---

## 📚 Sources

- [Rate limits | Gemini API](https://ai.google.dev/gemini-api/docs/rate-limits)
- [Build apps in Google AI Studio](https://ai.google.dev/gemini-api/docs/aistudio-build-mode)
- [Google AI Studio: Build and Test Gemini AI Models (Codecademy)](https://www.codecademy.com/article/google-ai-studio-build-and-test-gemini-ai-models)
