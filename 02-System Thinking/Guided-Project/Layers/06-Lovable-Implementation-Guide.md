# 06 — Lovable Implementation Guide

## SkillBridge Lite — AI-Powered Career Fit Assistant

---

# 1. Purpose of This Guide

هذا الملف هو دليل التنفيذ العملي لبناء SkillBridge Lite باستخدام Lovable.

سنستخدم ما حددناه في الملفات السابقة لبناء تطبيق يساعد المستخدم على:

- عرض ملفه المهني وتعديله.
- استكشاف ثلاث فرص مهنية تجريبية.
- الاطلاع على تفاصيل الفرصة.
- طلب تحليل مدى ارتباط ملفه المهني بمتطلبات الفرصة.
- استعراض نقاط الارتباط والفجوات المحتملة والخطوات المقترحة.

لن نطلب من Lovable بناء كل شيء دفعة واحدة.

سنبني المشروع على مراحل صغيرة، ونتحقق من النتيجة بعد كل مرحلة قبل الانتقال إلى التالية.

## Implementation Principle

**Build the smallest useful version first. Improve it only when necessary.**

هذا المبدأ مهم لأن هدفنا هو بناء تطبيق عملي ضمن وقت التدريب والرصيد المتاح، وليس إنشاء منتج ضخم.

---

# 2. Before You Start

قبل فتح Lovable، تأكدي من توفر العناصر التالية:

- [ ] فهم المشكلة والمستخدم المستهدف.
- [ ] تحديد المتطلبات الأساسية.
- [ ] تحديد الشاشات الرئيسية.
- [ ] تحديد بنية البيانات.
- [ ] تحديد منطق التطبيق.
- [ ] تحديد مهمة AI ومخرجاتها.
- [ ] معرفة الوظائف التي لن نبنيها في هذه النسخة.

هذه المعلومات موجودة في ملفات المشروع السابقة.

لا نحتاج إلى إعادة شرح المشروع من البداية داخل كل Prompt.

---

# 3. Implementation Scope

سننفذ المشروع وفق الأولويات التالية.

## P0 — Core Prototype

الوظائف الأساسية:

1. Home Page.
2. Professional Profile.
3. Opportunities List.
4. Opportunity Details.
5. AI Fit Analysis Interface.
6. Navigation بين الصفحات.
7. حالات التحميل والخطأ والبيانات غير الكافية.

## P1 — Useful Enhancements

نضيفها فقط إذا انتهينا من الوظائف الأساسية وكان الوقت والرصيد يسمحان:

- الاحتفاظ بالملف المهني باستخدام `localStorage`.
- تحسينات بصرية إضافية.
- تحسينات بسيطة على تجربة الهاتف.

## Outside Scope

لن نبني في هذه الجلسة:

- Authentication.
- Database.
- Admin Dashboard.
- Analysis History.
- Saved Opportunities.
- External Job Search.
- Resume Generator.
- Job Application System.
- AI Agents.
- Integrations with external business systems.

لن نضيف ميزة لمجرد أن Lovable قادر على بنائها؛ يجب أن تخدم هدف المشروع.

---

# 4. Step 1 — Create the Project

افتحي Lovable وأنشئي مشروعًا جديدًا.

استخدمي الاسم:

**SkillBridge Lite**

ثم أرسلي Prompt البداية التالي.

## Prompt 1 — Build the Core Application

```text
Build a polished, responsive web application called SkillBridge Lite.

PRODUCT PURPOSE

SkillBridge Lite helps students, recent graduates, and early-career professionals understand how their documented skills, projects, and experience relate to a selected professional opportunity.

The core value proposition is:

"Understand your fit. Discover what to improve. Take your next step."

IMPORTANT SCOPE RULES

Build a small, functional prototype.

Do not build authentication, a database, an admin dashboard, external job search, saved opportunities, analysis history, or job application functionality.

Do not add features that are not requested.

APPLICATION STRUCTURE

Create five main views:

1. Home
2. My Profile
3. Opportunities
4. Opportunity Details
5. AI Fit Analysis

Use a simple, consistent navigation system.

The user must be able to move between the views without a full page reload when appropriate.

HOME PAGE

Create a professional landing page with:

- Product name: SkillBridge Lite
- Headline: Understand Your Fit. Discover Your Next Step.
- A short description explaining the product.
- A primary button: Explore Opportunities
- A secondary button: Complete My Profile

MY PROFILE

Create a profile form with these fields:

- name
- headline
- about
- skills
- projects
- experience

Use suitable text inputs and text areas.

Allow the user to edit the profile and save changes in application state.

Use a clearly labeled sample profile if no profile has been entered.

Do not invent professional information about the current user.

OPPORTUNITIES

Display exactly three sample opportunities:

1. AI Automation Intern
Company: TechFlow
Description: Support development of AI-powered workflows and automation solutions for business processes.
Required skills: Python, AI, Automation, APIs.

2. Junior Frontend Developer
Company: PixelWorks
Description: Help build responsive web interfaces and interactive user experiences.
Required skills: HTML, CSS, JavaScript, React.

3. IoT Solutions Engineer
Company: Connected Systems Lab
Description: Support development of connected systems using sensors, microcontrollers, and software.
Required skills: IoT, Python, ESP32, APIs.

Each opportunity must have a clear action to view its details.

OPPORTUNITY DETAILS

Display the selected opportunity's:

- Title
- Company
- Description
- Required skills
- Responsibilities

Include a button labeled Analyze My Fit.

The analysis must use the current profile and the selected opportunity.

AI FIT ANALYSIS

Create the result interface with three sections:

1. Relevant Skills
2. Potential Gaps
3. Recommended Next Steps

Also support loading, insufficient-data, unavailable, and error states.

AI HONESTY

Do not pretend that a real AI analysis has occurred unless a real AI service has actually been configured and called successfully.

If no AI service is configured, show a clear message explaining that real AI analysis is currently unavailable.

If a sample result preview is provided, label it clearly as a demonstration example. Do not present it as a real analysis.

DESIGN DIRECTION

Use a modern, polished, professional design suitable for an early-career technology product.

Use:

- Clear visual hierarchy
- Readable typography
- Generous spacing
- Consistent cards and buttons
- Subtle borders and shadows
- A restrained color palette
- Responsive layouts for desktop and mobile

Avoid excessive animations, unnecessary decorative sections, and complicated UI components.

IMPLEMENTATION

Use the simplest suitable frontend structure.

Keep the three opportunities as fixed sample data.

Use application state for profile edits and the selected opportunity.

Do not add a database or external integrations.

Build the core views and navigation first.

Do not claim that the AI integration is complete unless it has actually been implemented and verified.
```

### ماذا نفعل بعد إرسال Prompt؟

لا ترسلي Prompt آخر مباشرة.

أولًا، افحصي ما بناه Lovable:

- هل ظهرت الصفحات الأساسية؟
- هل التنقل يعمل؟
- هل الفرص الثلاث موجودة؟
- هل نموذج الملف المهني قابل للتعديل؟
- هل تصميم التطبيق متناسق؟

إذا كانت هذه العناصر موجودة، انتقلي إلى الخطوة التالية.

إذا كان هناك خلل أساسي، أصلحيه باستخدام Prompt قصير ومحدد بدلًا من إعادة بناء التطبيق بالكامل.

---

# 5. Step 2 — Verify the User Flow

قبل إضافة أي وظيفة جديدة، اختبري مسار الاستخدام الأساسي.

```text
Home
  ↓
My Profile
  ↓
Opportunities
  ↓
Opportunity Details
  ↓
Analyze My Fit
  ↓
AI Fit Analysis
```

يجب أن يتمكن المستخدم أيضًا من الانتقال مباشرة إلى الفرص من الصفحة الرئيسية، ثم فتح ملفه المهني عند الحاجة.

## Verification Checklist

- [ ] Home Page تظهر بصورة سليمة.
- [ ] زر Explore Opportunities يفتح قائمة الفرص.
- [ ] زر Complete My Profile يفتح نموذج الملف المهني.
- [ ] يمكن تعديل الملف المهني.
- [ ] يمكن حفظ التعديلات في حالة التطبيق.
- [ ] تظهر الفرص الثلاث.
- [ ] تفتح كل فرصة صفحة التفاصيل الصحيحة.
- [ ] زر Analyze My Fit يعمل.
- [ ] تصل الواجهة إلى صفحة تحليل الملاءمة.
- [ ] لا توجد أزرار رئيسية لا تفعل شيئًا.

**مهم:** الانتقال إلى صفحة التحليل لا يعني أن AI يعمل بالفعل. سنتحقق من ذلك بصورة منفصلة.

---

# 6. Step 3 — Refine Only What Is Necessary

بعد بناء النسخة الأولى، لا تطلبي من Lovable إعادة تصميم التطبيق بالكامل لمجرد وجود تفاصيل صغيرة لا تعجبك.

اختاري أهم مشكلة تؤثر في الاستخدام، ثم عالجيها.

### Example Prompt — Improve the Existing Interface

```text
Improve the existing SkillBridge Lite interface without rebuilding the application.

Keep all existing pages, data structures, navigation, and functionality.

Focus only on:

- Clearer visual hierarchy
- Consistent spacing
- Readable typography
- Consistent button and card styles
- Better mobile responsiveness

Do not add features, new pages, databases, integrations, or complex animations.

Preserve the existing working behavior.
```

استخدمي هذا الطلب فقط إذا كانت الواجهة تحتاج فعلًا إلى تحسين عام.

إذا كانت المشكلة في عنصر واحد، فالأفضل تحديد العنصر نفسه بدلًا من طلب تحسين شامل.

---

# 7. Step 4 — Make the Profile Functional

نريد أن يتمكن المستخدم من إدخال معلوماته المهنية وتعديلها.

## Required Behavior

1. يفتح المستخدم صفحة My Profile.
2. تظهر البيانات الحالية.
3. يعدل المستخدم أحد الحقول.
4. يضغط Save Profile.
5. يحتفظ التطبيق بالتعديلات في حالة التطبيق.
6. تستخدم عملية التحليل البيانات الحالية، لا بيانات تجريبية قديمة.

إذا كان نموذج الملف المهني يعمل بالفعل، فلا تعيدي تنفيذ هذه الخطوة.

إذا لم يعمل الحفظ، استخدمي Prompt التالي.

## Prompt 2 — Fix Profile Editing

```text
Update only the My Profile functionality in the existing SkillBridge Lite application.

Requirements:

- Keep the existing profile fields:
  name, headline, about, skills, projects, experience.
- Allow the user to edit all fields.
- Make Save Profile update the current application state.
- Show a clear confirmation when the profile is saved.
- Ensure the opportunity analysis uses the latest saved profile data.

Do not rebuild the page.

Do not add a database, authentication, new profile fields, or unrelated features.

Preserve the existing design and navigation.
```

### Optional: Browser Persistence

إذا بقي الوقت والرصيد، يمكن طلب الاحتفاظ بالملف المهني بعد إعادة تحميل الصفحة باستخدام `localStorage`.

هذا تحسين اختياري، وليس شرطًا لتكملة الوظائف الأساسية.

```text
Add localStorage persistence only for the existing professional profile.

Load the saved profile when the application starts.

If no saved profile exists, use the current sample profile.

Update localStorage when the user saves profile changes.

Do not store API keys, secrets, or unnecessary sensitive information.

Do not change the existing page design or add unrelated features.
```

إذا كان الحفظ الحالي كافيًا للتدريب، فتجاوزي هذا التحسين.

---

# 8. Step 5 — Verify Opportunity Selection

يجب أن يستخدم التطبيق الفرصة التي اختارها المستخدم، وليس فرصة ثابتة داخل الكود.

## Expected Behavior

إذا اختار المستخدم:

**AI Automation Intern**

يجب أن يعرض التطبيق تفاصيل هذه الفرصة ويستخدم متطلباتها عند طلب التحليل.

وإذا اختار:

**IoT Solutions Engineer**

فيجب أن يستخدم متطلبات فرصة IoT.

### Checklist

- [ ] تظهر الفرص الثلاث فقط.
- [ ] لكل فرصة معرّف مميز.
- [ ] تعرض صفحة التفاصيل البيانات الصحيحة.
- [ ] يتغير التحليل المطلوب بحسب الفرصة المختارة.
- [ ] لا تظهر بيانات فرصة مختلفة عن الفرصة التي فتحها المستخدم.

إذا كانت هذه الوظائف تعمل، لا حاجة إلى إرسال Prompt إضافي.

---

# 9. Step 6 — Prepare the AI Analysis Interface

قبل توصيل خدمة AI، نتأكد من أن الواجهة جاهزة لاستقبال النتيجة.

يجب أن تتضمن صفحة AI Fit Analysis:

## Section 1 — Relevant Skills

عرض المهارات والمشاريع والخبرات ذات الصلة بالفرصة المختارة.

## Section 2 — Potential Gaps

عرض المتطلبات التي لا تتوفر عنها أدلة كافية في الملف المهني.

## Section 3 — Recommended Next Steps

عرض خطوات عملية تساعد المستخدم على تطوير ملفه أو اكتساب خبرة إضافية.

كما يجب أن تتعامل الواجهة مع الحالات التالية:

- `idle`
- `analyzing`
- `success`
- `insufficient_data`
- `ai_unavailable`
- `error`

إذا كانت الواجهة موجودة بالفعل، لا تعيدي إنشاءها.

## Prompt 3 — Complete the Analysis Interface

```text
Update only the existing AI Fit Analysis interface in SkillBridge Lite.

Create or refine these three result sections:

1. Relevant Skills
2. Potential Gaps
3. Recommended Next Steps

Support these states:

- Idle
- Analyzing
- Success
- Insufficient Data
- AI Unavailable
- Error

Show a loading indicator while a real analysis request is in progress.

Display clear, user-friendly messages for unavailable services and errors.

Do not present static example data as a real AI-generated result.

If the AI service has not been configured, clearly state that real AI analysis is unavailable.

Keep the existing application structure and avoid adding unrelated features.
```

---

# 10. Step 7 — Configure Real AI Analysis

هذه الخطوة مشروطة بتوفر خدمة AI مناسبة وإمكانية تهيئتها ضمن وقت التدريب والرصيد المتاح.

**لا تفترضي أن Lovable سينفذ تحليلًا حقيقيًا لمجرد إضافة زر أو كتابة Prompt.**

قبل التنفيذ، تحققي مما إذا كانت بيئة المشروع توفر آلية AI مناسبة، وما الإعدادات أو الصلاحيات المطلوبة لاستخدامها.

إذا كان التكامل يحتاج إلى مفتاح API، فيجب التعامل معه بطريقة آمنة وعدم وضعه في كود الواجهة الأمامية أو مستودع GitHub.

إذا كانت هناك آلية AI مهيأة بالفعل، استخدمي المواصفات الموجودة في:

`05-AI-Design-and-Specification.md`

## Prompt 4 — Implement the AI Analysis

استخدمي هذا الطلب فقط بعد التأكد من وجود آلية AI مناسبة وقابلة للتهيئة.

```text
Implement the real AI Fit Analysis functionality in the existing SkillBridge Lite application using the AI service already configured for this project.

Follow the AI specification defined for SkillBridge Lite.

INPUT

Use the latest saved professional profile and the currently selected opportunity.

TASK

Analyze how the documented skills, projects, and experience relate to the selected opportunity.

OUTPUT

Return structured data containing exactly these top-level fields:

- relevantSkills
- potentialGaps
- recommendedNextSteps

Each relevantSkills item must contain:

- item
- evidence
- relevance

Each potentialGaps item must contain:

- requirement
- reason

Each recommendedNextSteps item must contain:

- action
- reason

BEHAVIOR

- Use only information supported by the supplied profile and opportunity.
- Do not invent skills, qualifications, projects, or experience.
- Distinguish missing evidence from confirmed lack of skill.
- Do not guarantee hiring outcomes.
- Validate the returned data before presenting it as a successful analysis.
- Handle loading, insufficient input, invalid output, service unavailability, and errors.

SECURITY

Do not expose API keys or secrets in client-side code.

Use the appropriate secure mechanism for the configured AI service.

IMPORTANT

Do not pretend that an AI request succeeded if it failed.

Do not replace failed requests with static results presented as real AI output.

Do not add a database, authentication, analysis history, or unrelated features.

Preserve the existing application and implement only the required AI functionality.
```

### بعد تنفيذ التكامل

لا تعتمدي على رسالة Lovable التي تقول إنه أكمل العمل فقط.

تحققي من أن:

- الطلب يصل فعلًا إلى خدمة AI.
- المدخلات تحتوي على الملف المهني الحالي.
- المدخلات تحتوي على الفرصة المختارة.
- النتيجة تتغير عند تغيير البيانات.
- النتيجة تتوافق مع البنية المطلوبة.
- الأخطاء لا تؤدي إلى عرض نتيجة وهمية.
- مفاتيح API ليست مكشوفة في الواجهة الأمامية.

إذا لم تتمكني من إعداد الاتصال الحقيقي ضمن الجلسة، انتقلي إلى الخطوة التالية دون الادعاء بأن AI يعمل.

---

# 11. Step 8 — Handle the Unavailable AI State

إذا لم تتوفر خدمة AI مهيأة، يجب أن يظل التطبيق واضحًا وصادقًا.

يجب ألا يؤدي الضغط على Analyze My Fit إلى عرض نتيجة ثابتة باعتبارها تحليلًا حقيقيًا.

يمكن عرض رسالة مثل:

> AI analysis is not configured in this prototype yet. You can still explore opportunities and manage your professional profile.

وإذا أردتِ إظهار شكل النتيجة للتدريب على تصميم الواجهة، يمكن إضافة خيار منفصل:

**Preview Example Result**

يجب أن توضح الواجهة أن النتيجة مثال تجريبي، لا تحليل حقيقي لبيانات المستخدم.

لا حاجة إلى هذه الميزة إذا كانت ستستهلك وقتًا أو Credits دون فائدة تعليمية واضحة.

---

# 12. Step 9 — Final Verification

قبل اعتبار النسخة الأولية جاهزة للعرض، نفذي الفحوصات التالية.

## A. Navigation

- [ ] الصفحة الرئيسية تفتح بصورة سليمة.
- [ ] التنقل بين الصفحات يعمل.
- [ ] الأزرار الرئيسية تؤدي إلى الوجهات الصحيحة.
- [ ] لا توجد روابط داخلية مكسورة.

## B. Professional Profile

- [ ] تظهر جميع الحقول المطلوبة.
- [ ] يمكن تعديل الحقول.
- [ ] يعمل الحفظ.
- [ ] يستخدم التحليل أحدث بيانات الملف المهني.

## C. Opportunities

- [ ] تظهر الفرص الثلاث.
- [ ] تعرض كل فرصة اسمها وشركتها ووصفها.
- [ ] تعرض صفحة التفاصيل المتطلبات الصحيحة.
- [ ] يستخدم التحليل الفرصة التي اختارها المستخدم.

## D. AI Analysis

إذا كانت خدمة AI مهيأة:

- [ ] يتم تنفيذ طلب حقيقي.
- [ ] تستخدم المدخلات الصحيحة.
- [ ] تظهر الأقسام الثلاثة.
- [ ] لا تُعرض معلومات غير مدعومة بوصفها حقائق.
- [ ] تظهر رسالة واضحة عند حدوث خطأ.

إذا لم تكن خدمة AI مهيأة:

- [ ] يوضح التطبيق أن التحليل الحقيقي غير متاح.
- [ ] لا يعرض بيانات ثابتة على أنها نتيجة حقيقية.
- [ ] يظل بإمكان المستخدم استكشاف بقية التطبيق.

## E. User Experience

- [ ] النصوص مقروءة.
- [ ] التصميم متناسق.
- [ ] التطبيق مناسب للشاشات الصغيرة.
- [ ] لا توجد عناصر أساسية مقطوعة.
- [ ] لا توجد وظائف شكلية توحي بعمل ميزة غير منفذة.

---

# 13. Common Implementation Problems

## Problem 1 — Lovable Changes Too Much

**المشكلة:** طلب تعديل بسيط يؤدي إلى تغييرات غير مطلوبة في صفحات أخرى.

**الحل:** حددي الصفحة أو الوظيفة المطلوبة، واطلبي الحفاظ على بقية التطبيق.

مثال:

```text
Fix only the profile save behavior.

Do not change other pages, styles, navigation, or functionality.
```

## Problem 2 — A Button Looks Functional but Does Nothing

**المشكلة:** يظهر الزر في الواجهة، لكنه لا ينفذ الإجراء المطلوب.

**الحل:** حددي السلوك المتوقع بوضوح، ثم اطلبي تنفيذ هذا السلوك دون إعادة بناء الصفحة.

## Problem 3 — AI Analysis Shows the Same Result Every Time

**المشكلة:** قد يكون التطبيق يعرض بيانات ثابتة بدلًا من استدعاء خدمة AI حقيقية.

**الحل:** تحققي من آلية الاتصال، والمدخلات المرسلة، والنتيجة المستلمة. لا تعتبري تغير التصميم دليلًا على وجود تحليل حقيقي.

## Problem 4 — Too Many Changes Consume Credits

**المشكلة:** تكرار الطلبات الكبيرة وإعادة التصميم يؤدي إلى استهلاك غير ضروري للرصيد.

**الحل:**

1. اكتبي المشكلة المحددة.
2. حددي السلوك المطلوب.
3. اطلبي تعديلًا محدودًا.
4. اختبري النتيجة.
5. لا تعيدي تنفيذ ما يعمل بالفعل.

## Problem 5 — The Application Becomes Too Complex

**المشكلة:** إضافة ميزات لم تُطلب تجعل المشروع أصعب في التنفيذ والفهم.

**الحل:** عودي إلى نطاق المشروع الأساسي واحذفي أي إضافة لا تخدم الهدف الرئيسي، إذا كان حذفها آمنًا ولا يكسر وظائف أخرى.

---

# 14. Credit-Saving Strategy

لا يوجد عدد ثابت مضمون من Credits لبناء المشروع؛ فالاستعمال الفعلي يتأثر بحالة المشروع، وطبيعة التعديلات، وآلية التسعير المتاحة في Lovable.

لذلك، سنستخدم استراتيجية تقلل احتمالية الاستهلاك غير الضروري.

### Rule 1 — One Clear Prompt at a Time

لا ترسلي عدة طلبات كبيرة ومتداخلة قبل مراجعة النتائج.

### Rule 2 — Inspect Before Fixing

تحققي مما بناه Lovable قبل طلب إضافة وظيفة موجودة بالفعل.

### Rule 3 — Fix, Don't Rebuild

اطلبي إصلاح الجزء المتعطل بدلًا من إعادة بناء التطبيق كاملًا.

### Rule 4 — Prioritize the Core Flow

الأولوية لمسار الاستخدام الأساسي، وليس للتفاصيل الزخرفية.

### Rule 5 — Make Persistence Optional

لا تضيفي `localStorage` إذا كانت حالة التطبيق كافية لتحقيق هدف التدريب.

### Rule 6 — Treat AI Integration Separately

ابدئي بالواجهة والتدفق الأساسي، ثم تحققي من إمكانية إعداد AI الحقيقي.

لا تنفقي الرصيد على محاولات تكامل غير واضحة قبل التأكد من المتطلبات.

### Rule 7 — Stop When the MVP Works

عندما تحقق النسخة الأولية متطلبات المشروع، لا تستمري في إضافة ميزات لمجرد تحسينها بصورة غير محدودة.

---

# 15. Definition of Done

نعتبر SkillBridge Lite جاهزًا للعرض عندما يتحقق ما يلي:

- [ ] تطبيق ويب متجاوب.
- [ ] صفحة رئيسية واضحة.
- [ ] ملف مهني قابل للتعديل.
- [ ] ثلاث فرص مهنية تجريبية.
- [ ] صفحة تفاصيل لكل فرصة.
- [ ] اختيار الفرصة الصحيحة.
- [ ] صفحة تحليل تتضمن الأقسام الثلاثة المطلوبة.
- [ ] حالات مناسبة للتحميل والخطأ والبيانات غير الكافية.
- [ ] استخدام البيانات الحالية عند تنفيذ التحليل.
- [ ] وضوح حالة تكامل AI الحقيقية.
- [ ] عدم تقديم بيانات تجريبية على أنها تحليل حقيقي.
- [ ] عدم وجود وظائف إضافية تعقّد المشروع دون حاجة.

**ملاحظة:** إذا تعذر إعداد AI الحقيقي، يمكن اعتبار الواجهة والتدفق الأساسيين مكتملين كنموذج أولي، لكن لا يجوز اعتبار وظيفة التحليل بالذكاء الاصطناعي مكتملة.

---

# 16. Final Outcome

بعد تنفيذ هذا الدليل، يكون الهدف هو امتلاك نموذج أولي يوضح كيف تتحول متطلبات منتج إلى تطبيق ويب باستخدام Lovable.

يجب أن نستطيع شرح:

- من هو المستخدم؟
- ما المشكلة التي يعالجها المنتج؟
- ما الوظائف الأساسية؟
- كيف تنتقل البيانات داخل التطبيق؟
- ما المهمة التي ينفذها AI؟
- ما الضوابط التي تحكم مخرجاته؟
- ما الذي يعمل بالفعل؟
- ما الذي لا يزال يحتاج إلى إعداد أو تطوير؟

**The goal is not to generate a website. The goal is to engineer a working solution.**
