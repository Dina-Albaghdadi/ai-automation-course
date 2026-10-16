# 05 — تنفيذ SkillBridge Lite باستخدام Lovable
# SPECIFY & BUILD WITH AI

## 1. هدف التنفيذ

في هذا الملف، سنحوّل التصميم المعتمد في الملفات السابقة إلى نسخة أولية قابلة للتشغيل من **SkillBridge Lite** باستخدام Lovable.

سننفّذ الصفحات والوظائف التالية:

- Home
- My Profile
- Edit Profile
- Opportunities
- Opportunity Details
- Analysis Results

سنلتزم بنطاق المشروع وتصميمه السابقين، ولن نضيف وظائف جديدة غير مطلوبة.

## 2. تجهيز المشروع

قبل البدء:

1. افتح Lovable وأنشئ مشروعًا باسم `SkillBridge Lite`.
2. ارجع إلى ملفات المشروع السابقة في GitHub:
   - `01-Business-Analysis.md`
   - `02-Requirements.md`
   - `03-Design-the-Experience.md`
   - `04-Design-the-System.md`
3. استخدم قرارات هذه الملفات مرجعًا للبناء.

إذا لم تكن ملفات GitHub متصلة بـ Lovable، أرسل تعليمات البناء الموجودة في هذا الملف مباشرة.

## 3. بناء التطبيق

أرسل التعليمات التالية إلى Lovable:

### Build Prompt

```text
Build SkillBridge Lite, a responsive web application that helps users understand how their professional profile relates to career opportunities.

Follow the existing project requirements and design decisions. Build only the functionality specified below.

PAGES AND FUNCTIONALITY

1. HOME
Create a professional landing page explaining the purpose of SkillBridge Lite.
Provide navigation to My Profile and Opportunities.

2. MY PROFILE
Display the user's:
- Education
- Professional Background
- Skills
- Projects and Experience

Include an Edit Profile button.

Use editable sample profile data that the user can replace with their own information.

3. EDIT PROFILE
Allow the user to edit all profile fields and save changes.

Persist the saved profile using a simple, suitable storage mechanism for a single-user prototype. The saved information must remain available after navigation and page refresh.

4. OPPORTUNITIES
Display multiple sample opportunities:
- Junior AI Automation Engineer
- Junior Web Developer
- Junior Data Analyst

Each opportunity must have its own title, description, and required skills.

5. OPPORTUNITY DETAILS
Display the selected opportunity's details and requirements.

Include an "Analyze My Fit" button that starts an analysis using the latest saved profile and the selected opportunity.

6. ANALYSIS RESULTS
Display three sections:

- Relevant Skills: profile skills and experience related to the opportunity.
- Potential Gaps: requirements that are not sufficiently supported by the available profile information.
- Recommended Next Steps: practical recommendations relevant to the selected opportunity and profile.

The results must correspond to the selected opportunity and the profile data used.

Do not assume that an unmentioned skill is definitely missing. Distinguish between a confirmed gap and insufficient profile information.

FUNCTIONAL REQUIREMENTS

- Navigation between all required pages must work.
- Profile changes must be saved and reflected throughout the application.
- Each opportunity must display its own details and requirements.
- The selected opportunity and latest saved profile must be used for analysis.
- Handle empty profile fields and missing information gracefully.
- Provide clear feedback for saving and analysis states.
- Do not leave buttons that appear functional but do nothing.

IMPLEMENTATION CONSTRAINTS

This is a single-user prototype.

Use structured sample data for the initial profile and opportunities.

Do not add multi-user authentication, external job scraping, payments, or unrelated features.

Do not assume that a database or AI service is already connected.

If no AI service is configured, implement a clearly identified prototype analysis based on the available data. Do not present mock results as real AI-generated results.

Keep the implementation simple, maintainable, and responsive.

After building, report which requested features work and which integrations, if any, still require configuration.
```

## 4. التحقق من التنفيذ

بعد انتهاء Lovable من البناء، اختبر المسار التالي:

1. افتح Home وانتقل إلى My Profile.
2. افتح Edit Profile، وعدّل إحدى المهارات، ثم احفظ التغيير.
3. ارجع إلى My Profile وتأكد من ظهور البيانات الجديدة.
4. حدّث الصفحة وتأكد من استمرار التغييرات.
5. انتقل إلى Opportunities واختر إحدى الفرص.
6. افتح Opportunity Details وتأكد من ظهور المتطلبات الصحيحة.
7. اضغط Analyze My Fit.
8. راجع Relevant Skills وPotential Gaps وRecommended Next Steps.
9. اختر فرصة ثانية وأعد التحليل.
10. تأكد من ارتباط النتائج بالفرصة الجديدة وبالملف المهني المحفوظ.

## 5. إصلاح المشكلات

إذا لم تعمل إحدى الوظائف، أرسل إلى Lovable وصفًا محددًا للمشكلة بدلًا من إعادة بناء التطبيق بالكامل.

استخدم القالب التالي وعدّله بحسب المشكلة التي ظهرت:

```text
Fix the following issue in SkillBridge Lite:

Problem:
[Describe the actual behavior that is not working.]

Expected behavior:
[Describe what should happen.]

Requirements:
- Preserve the existing project scope and page design.
- Fix the underlying issue rather than adding unrelated features.
- Do not modify working functionality unnecessarily.
- Verify the fix using the relevant user flow.

After implementing the fix, report what changed and what still needs verification.
```

بعد كل إصلاح، أعد اختبار الوظيفة المتأثرة.

## 6. مخرجات الملف

قبل الانتقال إلى الملف التالي، تأكد من إنجاز ما يلي:

- [ ] بناء الصفحات الست المطلوبة.
- [ ] تشغيل التنقل بين الصفحات.
- [ ] تعديل الملف المهني وحفظه.
- [ ] عرض عدة فرص مهنية وتفاصيل كل فرصة.
- [ ] تشغيل مسار تحليل الملاءمة.
- [ ] عرض أقسام نتائج التحليل الثلاثة.
- [ ] التأكد من استخدام الملف المهني والفرصة المختارة في التحليل.
- [ ] اختبار المسار الأساسي وإصلاح المشكلات المكتشفة.
- [ ] تحديد أي وظيفة أو تكامل لم يكتمل تنفيذه.

**الناتج النهائي:** نسخة أولية من SkillBridge Lite مبنية باستخدام Lovable، مع وظائف أساسية تم اختبارها، وقائمة واضحة بأي نواقص متبقية.
