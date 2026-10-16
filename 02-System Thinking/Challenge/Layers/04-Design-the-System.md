# Layer 04 — DESIGN THE SYSTEM
## تصميم النظام

**السؤال المركزي:**

> كيف سيعمل الحل من الداخل؟

في الطبقات السابقة، ركزنا على:

- فهم المستخدم واحتياجاته.
- تحديد المشكلة والمتطلبات.
- تصميم تجربة الاستخدام والتفاعل.

الآن ننتقل إلى ما يحدث **خلف الواجهة**.

عندما يصمم المستخدم Portfolio ويضغط على مشروع، أو يرسل رسالة، أو يشاهد معلومات مهنية، هناك نظام في الخلفية يجعل هذه التجربة ممكنة.

لذلك نحتاج إلى **System Design**.

> **System Design هو تحديد ما يحتاجه الحل من الداخل حتى يعمل وفق المتطلبات وتجربة الاستخدام التي صممناها.**

في هذه الطبقة سننظر إلى أربعة مجالات رئيسية:

1. **Data Design**
2. **AI Design**
3. **Automation Design**
4. **Access Rules**

---

# 1. A — Data Design

## تصميم البيانات

أول سؤال نسأله عند تصميم النظام هو:

> **What information does the system need?**

ما المعلومات التي يحتاج إليها النظام؟

في مشروع **My Professional Portfolio**، نريد أن يستطيع الزائر معرفة:

- من صاحب الموقع؟
- ما تخصصه؟
- ما مهاراته؟
- ما مشاريعه؟
- ما خبراته؟
- كيف يمكن التواصل معه؟

إذن النظام يحتاج إلى بيانات تمثل هذه المعلومات.

لكننا لا نريد تخزين المعلومات بطريقة عشوائية.

نحتاج إلى تنظيمها.

وهنا تظهر أربعة مفاهيم:

- **Entity**
- **Attribute**
- **Relationship**
- **State**

---

## 1.1 Entity

**Entity** هو شيء مهم في النظام نحتاج إلى تخزين معلومات عنه.

في الـPortfolio يمكن أن تكون لدينا:

```text
Profile
Project
Skill
Experience
```

مثلًا:

```text
Project
```

هو **Entity** لأن النظام يحتاج إلى الاحتفاظ بمعلومات عن كل مشروع.

قد يكون لدينا:

```text
Project 01
Project 02
Project 03
```

كل واحد منها يمثل مشروعًا داخل النظام.

### في مشروعنا

يمكننا التفكير في:

```text
Portfolio
├── Profile
├── Skills
├── Projects
└── Experience
```

إذن:

> **Entity = الشيء الذي نحتاج إلى الاحتفاظ بمعلومات عنه.**

---

# 1.2 Attribute

إذا كان **Entity** يمثل الشيء، فما المعلومات التي تصفه؟

هنا يأتي:

**Attribute**

وهو معلومة تصف الـEntity.

مثلًا، إذا كان لدينا:

```text
Project
```

يمكن أن تكون الـAttributes:

```text
Title
Description
Problem
Solution
Role
Technologies
```

أي:

```text
Project
├── Title
├── Description
├── Problem
├── Solution
├── Role
└── Technologies
```

إذن:

> **Entity = الشيء**

> **Attribute = المعلومات التي تصف الشيء**

---

# 1.3 Relationship

الـEntities لا تكون دائمًا منفصلة.

قد توجد بينها علاقات.

في الـPortfolio مثلًا:

صاحب الـProfile لديه عدة Projects.

يمكن تمثيل ذلك:

```text
Profile
   |
   └── Projects
          |
          ├── Project 01
          ├── Project 02
          └── Project 03
```

إذن لدينا علاقة بين:

```text
Profile → Projects
```

وقد يكون لدينا أيضًا:

```text
Project → Skills
```

لأن المشروع يستخدم مجموعة من المهارات أو التقنيات.

إذن:

> **Relationship = العلاقة بين الـEntities داخل النظام.**

---

# 1.4 State

أحيانًا لا تكون البيانات مجرد معلومات ثابتة.

قد يكون للشيء حالة تتغير مع الوقت.

وهنا نستخدم:

**State**

مثلًا، المشروع قد يكون:

```text
Draft
Published
Archived
```

في مشروع Portfolio، قد نحتاج إلى معرفة:

> هل هذا المشروع منشور للزوار أم لا؟

فنستطيع مثلًا أن نستخدم:

```text
Project State

Draft
Published
Archived
```

ليس معنى ذلك أننا يجب أن نضيف هذه الحالات للمشروع فعلًا.

بل نسأل:

> **هل توجد معلومات تمثل حالة متغيرة داخل النظام؟**

إذا نعم، نحددها كـ **State**.

---

# 2. B — AI Design

## تصميم مهام الذكاء الاصطناعي

بعد تحديد البيانات، نسأل:

> **Where does the system actually need AI?**

وهنا نقطة مهمة جدًا:

لا نضع AI في النظام لمجرد أننا نستطيع ذلك.

بل نحدد:

> **ما المهمة التي تحتاج فعلًا إلى قدرات الذكاء الاصطناعي؟**

لدينا في Portfolio مثلًا معلومات عن مشروع:

```text
Project Name
Problem
Solution
Role
Technologies
```

يمكننا أن نضيف مهمة مثل:

> Generate a professional summary for this project.

هذه هي:

**AI Task**

---

# 2.1 AI Task

**AI Task** هي مهمة نريد من AI تنفيذها.

في مثالنا:

```text
Project Information
        ↓
      AI Task
        ↓
Professional Project Summary
```

فالـAI لا "يبني الموقع" بشكل عام.

لدينا مهمة محددة:

> تحويل معلومات المشروع إلى ملخص مهني.

وهذه طريقة التفكير الصحيحة:

بدل:

> "نريد استخدام AI في المشروع."

نقول:

> "نحتاج AI لتنفيذ هذه المهمة المحددة."

---

# 2.2 AI vs Rule

الآن نحتاج إلى التمييز بين:

**AI**

و

**Rule**

ليس كل قرار يحتاج إلى AI.

مثلًا، إذا أردنا أن نعرض المشاريع المميزة فقط:

```text
IF project.featured = true
THEN show project
```

هذه **Rule**.

لا نحتاج AI.

لأن القرار واضح ويمكن تحديده مسبقًا.

أما إذا أردنا:

> كتابة ملخص مهني لمشروع اعتمادًا على معلوماته.

فهذه مهمة يمكن أن تستفيد من AI لأنها تتطلب فهمًا وتوليدًا للنص.

إذن نسأل دائمًا:

> **Does this task need AI, or is a Rule enough?**

### المبدأ

> **Don't use AI when a deterministic Rule is enough.**

لا نستخدم AI عندما تكون القاعدة الثابتة كافية.

---

# 2.3 Structured Output

عندما نستخدم AI، لا يكفي أحيانًا أن نقول:

> Analyze this project.

نحتاج إلى تحديد شكل النتيجة التي نريدها.

مثلًا:

```text
Summary
Key Value
Technologies
Professional Relevance
```

أو:

```json
{
  "summary": "...",
  "keyValue": "...",
  "technologies": [],
  "professionalRelevance": "..."
}
```

هذا يسمى:

**Structured Output**

أي أننا نحدد شكل المخرجات التي يجب أن يعيدها AI.

وهذا مهم عندما يحتاج النظام إلى استخدام نتيجة AI داخل واجهته أو داخل جزء آخر من النظام.

---

# 3. C — Automation Design

## تصميم الأتمتة

بعد ذلك نسأل:

> **What should the system do automatically?**

في الـPortfolio، لدينا مثلًا **Contact Form**.

الزائر يكتب رسالة ويرسلها.

بدل أن يتطلب الأمر تدخلًا يدويًا في كل مرة، يمكن للنظام أن ينفذ بعض الإجراءات تلقائيًا.

مثلًا:

```text
Contact Form Submitted
        ↓
Store Message
        ↓
Send Notification
```

هنا بدأنا نفكر في:

**Automation**

---

# 3.1 Automation

**Automation** هي جعل النظام ينفذ إجراءً أو مجموعة إجراءات تلقائيًا بدل تنفيذها يدويًا.

في مثالنا:

```text
Visitor submits message
        ↓
System handles the message
        ↓
Message is stored
        ↓
Notification is sent
```

الفكرة ليست أداة معينة.

نحن الآن نصمم:

> **What should happen automatically?**

وليس:

> كيف ننفذها في أداة معينة؟

---

# 3.2 Action

داخل الـAutomation لدينا إجراءات.

هذه الإجراءات تسمى:

**Actions**

في مثالنا:

```text
Store Message
Send Notification
```

كل واحدة منها تمثل **Action** ينفذها النظام.

إذن:

> **Automation** تصف ما الذي يحدث تلقائيًا.

> **Action** تصف الإجراء الذي ينفذه النظام.

---

# 3.3 Human-in-the-Loop

ليس معنى Automation أن النظام يجب أن يعمل بشكل مستقل دائمًا.

أحيانًا نحتاج إلى الإنسان.

وهذا يسمى:

**Human-in-the-Loop**

مثلًا، يمكن أن نتصور نظامًا يستقبل رسالة من الزائر، ثم يحدد إن كانت تحتاج إلى مراجعة صاحب الموقع.

```text
Message Received
       ↓
System Analysis
       ↓
Needs Review?
    ↙       ↘
  Yes        No
   ↓          ↓
Human       Automatic
Review       Handling
```

في الـPortfolio البسيط قد لا نحتاج إلى هذا المستوى.

وهنا نتعلم نقطة مهمة:

> **ليس مطلوبًا أن نستخدم Human-in-the-Loop في كل نظام.**

نحن فقط نحدد هل نحتاجه أم لا.

---

# 4. D — Access Rules

## قواعد الوصول والصلاحيات

الآن لدينا سؤال آخر:

> **Who can access what?**

في بعض الأنظمة، لا يستطيع كل مستخدم فعل كل شيء.

في الـPortfolio يمكن أن يكون لدينا مثلًا:

```text
Visitor
Owner
```

الزائر يستطيع مشاهدة المحتوى.

أما صاحب الـPortfolio فيحتاج إلى إمكانية إدارة محتواه.

---

# 4.1 Role

**Role** يحدد نوع المستخدم داخل النظام.

في مثالنا:

```text
Visitor
Owner
```

هذه هي الـRoles.

---

# 4.2 Permission

**Permission** تحدد ما الذي يستطيع المستخدم فعله.

مثلًا:

| Role | Permission |
|---|---|
| Visitor | View Projects |
| Visitor | View Experience |
| Visitor | Send Message |
| Owner | Edit Projects |
| Owner | Edit Experience |

هنا بدأنا نحدد:

> من يستطيع فعل ماذا؟

---

# 4.3 Access Control

**Access Control** هو تطبيق قواعد الوصول التي تحدد من يستطيع الوصول إلى المعلومات أو تنفيذ الإجراءات.

مثلًا:

```text
Visitor
   ↓
View Project
   ✓

Edit Project
   ✗
```

بينما:

```text
Owner
   ↓
View Project
   ✓

Edit Project
   ✓
```

لكن إذا كان الـPortfolio مجرد موقع عام بدون Login أو Dashboard، فقد لا نحتاج إلى نظام صلاحيات معقد.

وهذا جزء مهم من **System Design**:

> ليس الهدف إضافة المزيد من التكنولوجيا.

بل تحديد ما يحتاجه النظام فعلًا.

---

# 5. تجميع تصميم النظام

بعد فهم المجالات الأربعة، نستطيع النظر إلى الـPortfolio كحل متكامل.

## Data

نحتاج إلى:

```text
Profile
Projects
Skills
Experience
Contact Information
```

ونحدد داخلها الـEntities والـAttributes والعلاقات والحالات عند الحاجة.

---

## AI

لدينا مثلًا:

```text
AI Task:
Generate Professional Project Summary
```

ثم نحدد:

- هل تحتاج المهمة فعلًا إلى AI؟
- ما شكل الـStructured Output المطلوب؟

---

## Automation

لدينا مثلًا:

```text
Contact Form Submitted
        ↓
Store Message
        ↓
Send Notification
```

ونحدد الـActions المطلوبة، وهل نحتاج إلى Human-in-the-Loop أم لا.

---

## Access

لدينا:

```text
Visitor
   → View Public Content

Owner
   → Manage Portfolio Content
```

ونحدد الـPermissions المطلوبة لكل Role.

---

# 6. System Design Decision

الآن لا نريد إضافة كل شيء إلى المشروع.

نريد اتخاذ قرارات.

يمكننا تلخيص تفكيرنا هكذا:

| المجال | السؤال | مثال في Portfolio |
|---|---|---|
| **Data Design** | ما المعلومات التي يحتاجها النظام؟ | Profile, Projects, Skills, Experience |
| **AI Design** | أين نحتاج AI فعلًا؟ | Generate Project Summary |
| **Automation Design** | ماذا يمكن أن يحدث تلقائيًا؟ | Handle Contact Submission |
| **Access Rules** | من يستطيع فعل ماذا؟ | Visitor / Owner |

الهدف هو الوصول إلى:

> **Logical System Design**

تصميم منطقي يوضح ما يحتاجه الحل من الداخل قبل أن نبدأ بناءه.

---

# 7. كيف يرتبط ذلك بالطبقات السابقة؟

نحن لا نبدأ System Design من الصفر.

لدينا ما توصلنا إليه في الطبقات السابقة.

```text
Layer 01
Understand the User
        ↓
Layer 02
Define the Solution
        ↓
Layer 03
Design the Experience
        ↓
Layer 04
Design the System
        ↓
Layer 05
Specify & Build with AI
```

كل Layer تجيب عن سؤال مختلف.

### Layer 03 — Experience Design

> كيف سيستخدم الشخص الحل؟

### Layer 04 — System Design

> ماذا يحتاج الحل من الداخل حتى يجعل هذه التجربة تعمل؟

### Layer 05 — Specify & Build with AI

> كيف نعبّر عن هذا التصميم بشكل واضح ونوجّه AI لبناء الحل؟

---

# 8. خلاصة Layer 04

في هذه الطبقة تعلمنا أن **System Design** لا يعني إضافة أكبر عدد ممكن من التقنيات.

بل يعني اتخاذ قرارات واضحة حول ما يحتاجه الحل.

ركزنا على أربعة مجالات:

### A. Data Design

```text
Entity
Attribute
Relationship
State
```

### B. AI Design

```text
AI Task
AI vs Rule
Structured Output
```

### C. Automation Design

```text
Automation
Action
Human-in-the-Loop
```

### D. Access Rules

```text
Role
Permission
Access Control
```

والسؤال الذي نعود إليه دائمًا هو:

> **What does this solution actually need?**

وليس:

> **What technologies can we add?**

---

## المبدأ الأساسي

> **Design the system around the requirements, not around the tools.**

صمّم النظام وفق احتياجات الحل، وليس وفق الأدوات المتاحة.

---

## Layer 04 Completion Check

قبل الانتقال إلى Layer 05، يجب أن تكون قادرًا على:

- تحديد الـ**Entities** الأساسية في الحل.
- تحديد الـ**Attributes** التي تصفها.
- فهم الـ**Relationships** بين البيانات.
- تحديد الـ**States** عندما تكون موجودة.
- تحديد مهام الـ**AI** المناسبة.
- التمييز بين **AI** و **Rule**.
- تحديد **Structured Output** عند الحاجة.
- تحديد ما يمكن تنفيذه كـ**Automation**.
- تحديد الـ**Actions** المطلوبة.
- معرفة متى يمكن أن نحتاج إلى **Human-in-the-Loop**.
- تحديد الـ**Roles** والـ**Permissions** الأساسية.
- اتخاذ قرارات تصميم مبنية على احتياجات الحل.

> **You don't design the system by adding technology.  
> You design it by making the right decisions.**
