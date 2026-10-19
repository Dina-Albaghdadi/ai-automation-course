# U5 — CONTROL ITS AGENCY
## هندسة استقلالية الوكلاء الذكيين

---

# 1. السؤال المركزي للوحدة

> **كيف نصمم AI Agent يمتلك الاستقلالية المناسبة لاتخاذ القرارات وتنفيذ المهام، مع إبقاء سلوكه داخل حدود واضحة وقابلة للتحكم والتقييم؟**

في الوحدات السابقة تعلّمنا كيف نفهم العمل، ونبني الحلول، ونختبرها، ونجعل الذكاء الاصطناعي يعتمد على المعرفة المناسبة.

في هذه الوحدة ننتقل إلى سؤال جديد:

**متى نسمح للـ AI أن يقرر بنفسه؟**

الـ Workflow التقليدي ينفذ مسارًا نحدده مسبقًا، بينما يستطيع الـ AI Agent اتخاذ قرارات حول الخطوة التالية، واختيار الأدوات المناسبة، والاستفادة من نتائج التنفيذ لتحديد ما يفعله بعد ذلك.

لكن زيادة الاستقلالية تعني أيضًا زيادة المسؤولية والمخاطر.

لذلك، ليس هدفنا بناء أكثر Agent استقلالية، بل بناء Agent يمتلك **القدر المناسب من الاستقلالية لتحقيق هدف محدد ضمن حدود يمكن التحكم بها**.

> **The goal is not maximum autonomy. The goal is controlled, purposeful, and verifiable autonomy.**

---

# 2. أهداف التعلم

بنهاية الوحدة، ستكون المتدربة قادرة على:

1. التمييز بين Workflow وAI-Powered Workflow وAI Agent.
2. تحديد متى نحتاج إلى Agent، ومتى يكون استخدامه غير ضروري.
3. تحديد مسؤولية الـ Agent ونطاق عمله والقرارات المسموح بها.
4. فهم وتصميم Decision Loop.
5. تحديد كيفية اختيار الـ Agent للأدوات واستخدام نتائجها.
6. التمييز بين Memory وState.
7. تصميم حدود الاستقلالية والصلاحيات.
8. تحديد الحالات التي تتطلب Human Approval أو Escalation.
9. تصميم Stop Conditions واضحة.
10. تقييم سلوك الـ Agent ونتائجه، واستخدام التقييم لتحسين تصميمه.

---

# 3. Agent Engineering Framework

سنعتمد خلال الوحدة على ست طبقات مترابطة:

```text
CHOOSE
هل نحتاج إلى Agent؟
        ↓
DEFINE
ما مسؤوليته وحدوده؟
        ↓
DECIDE
كيف يتخذ القرار؟
        ↓
ACT
كيف ينفذ القرار؟
        ↓
CONTROL
كيف نتحكم في استقلاليته؟
        ↓
EVALUATE
هل حقق الهدف وتصرف بشكل صحيح؟
        ↓
IMPROVE
كيف نحسّن التصميم؟
```

| Layer | السؤال المركزي | المخرج |
|---|---|---|
| 01 — CHOOSE | هل نحتاج إلى Agent أصلًا؟ | Architecture Decision |
| 02 — DEFINE | ما مسؤوليته وحدود عمله؟ | Agent Contract |
| 03 — DECIDE | كيف يحدد الخطوة التالية؟ | Decision Design |
| 04 — ACT | كيف يحول القرار إلى تنفيذ؟ | Action & Tool Design |
| 05 — CONTROL | ما الذي يُسمح له بفعله؟ | Control Design |
| 06 — EVALUATE | هل حقق الهدف وتصرف ضمن الحدود؟ | Evaluation Design |

تبدأ عملية التحسين عندما تكشف نتائج التقييم عن مشكلة في التصميم.

---

# LAYER 01 — CHOOSE
## هل نحتاج إلى Agent أصلًا؟

يبدأ التصميم الصحيح من المشكلة، وليس من الأداة التي نريد استخدامها.

ليس كل نظام يحتاج إلى Agent. أحيانًا يكون Workflow ثابت كافيًا، وأحيانًا نحتاج إلى AI في خطوة محددة فقط، بينما تتطلب حالات أخرى Agent يستطيع تحديد الخطوة التالية بناءً على الحالة الحالية.

## 1. Workflow

نظام ينفذ خطوات ومسارات محددة مسبقًا.

```text
Trigger
   ↓
Step 1
   ↓
Step 2
   ↓
Condition
   ↓
Action
```

**متى يناسبنا؟**

عندما تكون الخطوات والشروط والمسارات معروفة ويمكن تحديدها مسبقًا.

## 2. AI-Powered Workflow

يستخدم AI في خطوة أو أكثر، لكن الـ Workflow يظل مسؤولًا عن التحكم في مسار التنفيذ.

```text
Input
   ↓
AI interprets input
   ↓
Workflow evaluates result
   ↓
Action
```

قد يتولى AI مهام مثل:

- Classification
- Information Extraction
- Text Generation
- Interpretation

لكن وجود AI داخل Workflow لا يعني بالضرورة أن النظام أصبح Agent.

## 3. AI Agent

نظام يستخدم الذكاء الاصطناعي لاتخاذ قرارات حول ما يجب فعله بعد ذلك لتحقيق هدف معين، مع إمكانية تكرار دورة القرار والتنفيذ بناءً على النتائج.

```text
Observe
   ↓
Interpret
   ↓
Decide
   ↓
Act
   ↓
Observe Result
   ↓
Decide Again
```

الفرق الأساسي ليس مجرد استخدام LLM، بل وجود مساحة لاتخاذ قرارات متتابعة حول الخطوة التالية.

## 4. Agency

تشير **Agency** هنا إلى مساحة القرارات والأفعال التي يستطيع النظام تحديدها أو تنفيذها بشكل مستقل.

كلما زادت القرارات التي يستطيع الـ Agent اتخاذها دون تدخل خارجي، زادت مساحة استقلاليته.

مثلاً:

- Agent يختار بين خيارين محددين.
- Agent يختار الأداة المناسبة من مجموعة أدوات.
- Agent يحدد ترتيب الإجراءات والخطوة التالية بناءً على النتائج.

## 5. Minimum Necessary Agency

المبدأ الهندسي:

> **Give the Agent only the autonomy the problem requires.**

نختار أبسط بنية تحقق الهدف:

```text
Fixed, predictable process
        ↓
Workflow

AI needed for specific tasks
        ↓
AI-Powered Workflow

Dynamic decisions and next-step selection
        ↓
AI Agent
```

### مخرج الـ Layer

**Architecture Decision**

يحدد نوع النظام المناسب، وسبب اختياره، ومقدار الاستقلالية التي تحتاجها المشكلة.

---

# LAYER 02 — DEFINE
## ما مسؤولية الـ Agent وحدود عمله؟

بعد اختيار استخدام Agent، يجب تحديد ما الذي نريد منه تحقيقه، وما القرارات التي تدخل ضمن مسؤوليته.

الوكيل الذي لا يمتلك مسؤولية وحدودًا واضحتين يصعب التحكم في سلوكه أو تقييمه.

## 1. Agent Responsibility

تحدد المهمة التي يتحمل الـ Agent مسؤوليتها.

بدلًا من تعليمات عامة مثل:

> Help the user.

نحدد مسؤولية واضحة يمكن تقييمها، مثل:

> Analyze incoming requests and determine the appropriate next action within the permitted scope.

## 2. Success Criteria

تحدد الشروط التي تجعل المهمة ناجحة.

يجب أن تكون هذه الشروط واضحة وقابلة للملاحظة.

على سبيل المثال:

- تحديد الإجراء المناسب.
- تنفيذ الإجراء المطلوب عندما يكون مسموحًا.
- عدم تجاوز الصلاحيات.
- إيقاف المهمة عند تحقق الهدف.

## 3. Scope

يحدد نطاق عمل الـ Agent.

**In Scope:** المهام والقرارات التي تدخل ضمن مسؤوليته.

**Out of Scope:** المهام والقرارات التي لا تدخل ضمن مسؤوليته.

مثال:

```text
In Scope:
Classifying incoming requests.

Out of Scope:
Making final financial decisions.
```

## 4. Decision Boundaries

نحدد القرارات التي يستطيع الـ Agent اتخاذها، والقرارات التي تتطلب شروطًا إضافية أو تدخلًا بشريًا.

يمكن تقسيمها إلى:

- **Allowed:** قرارات مسموح بها ضمن نطاقه.
- **Conditional:** قرارات مسموح بها عند تحقق شروط محددة.
- **Restricted:** قرارات لا يستطيع تنفيذها دون موافقة أو صلاحية إضافية.

## 5. Missing Information

يجب أن نحدد ما الذي يفعله الـ Agent عندما لا يمتلك معلومات كافية لاتخاذ قرار مناسب.

قد يحتاج إلى:

- Ask: طلب معلومات إضافية.
- Wait: انتظار معلومة أو نتيجة.
- Escalate: إحالة الحالة إلى إنسان.
- Stop: إيقاف المهمة عندما يتعذر الاستمرار.

> عدم كفاية المعلومات حالة يجب تصميم سلوكها، وليس سببًا للتخمين.

### مخرج الـ Layer

**Agent Contract**

وثيقة مختصرة تحدد:

- Responsibility
- Success Criteria
- Scope
- Decision Boundaries
- Missing Information Behavior

---

# LAYER 03 — DECIDE
## كيف يحدد الـ Agent الخطوة التالية؟

هذه الطبقة تركز على آلية اتخاذ القرار، وكيف ينتقل الـ Agent من فهم الحالة إلى تحديد الإجراء المناسب.

## 1. Decision Loop

```text
Observe
   ↓
Interpret
   ↓
Decide
   ↓
Validate
   ↓
Act
   ↓
Observe Result
   ↓
Decide Again
```

### Observe

ملاحظة الحالة الحالية من خلال المعلومات المتاحة، أو نتائج الأدوات، أو التغيرات التي حدثت أثناء تنفيذ المهمة.

السؤال:

**What is happening now?**

### Interpret

تفسير المعلومات وفهم علاقتها بهدف المهمة.

السؤال:

**What does this information mean for the task?**

### Decide

اختيار الخطوة التالية بناءً على الحالة والهدف والحدود المحددة.

قد يقرر الـ Agent:

- Continue
- Ask
- Use Tool
- Wait
- Escalate
- Stop
- Complete

### Validate

التحقق من أن القرار مناسب قبل تنفيذه.

نسأل:

- هل المعلومات كافية؟
- هل القرار ضمن نطاق المسؤولية؟
- هل الإجراء مسموح؟
- هل يحتاج إلى موافقة؟

## 2. Decision Conditions

يجب أن تكون الحالات المهمة واضحة في تصميم الـ Agent.

مثال:

```text
If required information is missing
→ Ask

If information is sufficient
→ Determine the next action

If approval is required
→ Escalate for approval

If the goal is achieved
→ Complete
```

## 3. Decision Is Not Authorization

قد يقرر الـ Agent أن Action معينة مناسبة، لكن هذا لا يعني أن النظام يسمح له بتنفيذها.

```text
Agent Decision
      ↓
Authorization Check
      ↓
Execute if permitted
```

> **The model can recommend an action, but the system must enforce permission boundaries.**

لا ينبغي الاعتماد على الـ Prompt وحده لمنع تنفيذ الإجراءات الحساسة.

### مخرج الـ Layer

**Decision Design**

يوضح كيف يلاحظ الـ Agent الحالة، ويفسرها، ويختار الخطوة التالية، ويتعامل مع نقص المعلومات.

---

# LAYER 04 — ACT
## كيف يحول الـ Agent قراره إلى فعل؟

بعد اتخاذ القرار، يحتاج الـ Agent إلى وسيلة لتنفيذ الإجراء المناسب.

هنا يأتي دور الـ Tools.

## 1. Tools

الـ Tool هي واجهة يستطيع النظام استخدامها لتنفيذ Action محددة.

أمثلة:

- Search
- Retrieve
- Calculate
- Create
- Update
- Send

الـ Tool تربط قرار الـ Agent بإجراء يمكن للنظام تنفيذه.

## 2. Tool Selection

عندما يمتلك الـ Agent أكثر من Tool، يجب أن يعرف متى يستخدم كل واحدة منها.

يجب أن يحدد التصميم:

- ما وظيفة كل Tool؟
- متى تكون مناسبة؟
- ما شروط استخدامها؟
- ما البيانات التي تحتاجها؟

ليس الهدف توفير أكبر عدد من الأدوات، بل توفير الأدوات الضرورية للمهمة.

> **Give the Agent only the tools it needs.**

## 3. Tool Inputs and Results

كل Tool تحتاج إلى مدخلات مناسبة، وتعيد نتيجة يجب على الـ Agent فهمها.

```text
Decision
   ↓
Tool Selection
   ↓
Tool Inputs
   ↓
Tool Execution
   ↓
Tool Result
   ↓
Interpret Result
   ↓
Next Decision
```

نتيجة الـ Tool ليست بالضرورة نهاية العملية؛ فقد تتطلب قرارًا جديدًا.

## 4. Context

الـ **Context** هو المعلومات المتاحة للـ Agent عند اتخاذ القرار.

ليس من الضروري تضمين جميع المعلومات المتاحة، بل المعلومات التي يحتاج إليها لفهم الحالة وتحديد الإجراء المناسب.

السؤال:

**What information does the Agent need for this decision?**

## 5. Memory vs. State

رغم ارتباط المفهومين، فإنهما يؤديان وظيفتين مختلفتين.

**Memory**

المعلومات التي يحتفظ بها النظام ليستفيد منها لاحقًا.

مثال: معلومات سابقة عن المستخدم أو المهمة.

**State**

الحالة الحالية للمهمة، والتي توضح المرحلة التي وصلت إليها.

مثال:

```text
collecting_information
        ↓
awaiting_confirmation
        ↓
saved
```

الفرق الأساسي:

```text
Memory → What information do we retain?

State → What is the current status of the task?
```

تحديد الحالة مهم عندما تعتمد الخطوة التالية على المرحلة التي وصل إليها النظام.

### مخرج الـ Layer

**Action & Tool Design**

يحدد الأدوات المتاحة، ومتى تستخدم، ومدخلاتها ونتائجها، والمعلومات والحالة التي يحتاج إليها الـ Agent لمواصلة المهمة.

---

# LAYER 05 — CONTROL
## كيف نتحكم في استقلالية الـ Agent؟

وجود القدرة على اتخاذ القرار والتنفيذ لا يعني أن كل قرار أو إجراء مسموح.

هذه الطبقة تحدد كيف نحافظ على استقلالية مفيدة دون منح الـ Agent صلاحيات غير ضرورية.

## 1. Autonomy Boundaries

نحدد أين يستطيع الـ Agent العمل باستقلالية، وأين يحتاج إلى شروط إضافية أو موافقة أو تدخل بشري.

## 2. Permissions & Least Privilege

تحدد الـ **Permissions** ما الذي يستطيع الـ Agent الوصول إليه أو تنفيذه.

أما **Least Privilege** فهو مبدأ منح النظام أقل قدر من الصلاحيات اللازمة لأداء مسؤوليته.

أمثلة:

- إذا كان يحتاج إلى قراءة البيانات، فلا نمنحه صلاحية تعديلها دون حاجة.
- إذا كان يحتاج إلى تحديث سجل محدد، فلا نمنحه صلاحية تعديل جميع السجلات دون مبرر.

> **Grant only the permissions required for the task.**

## 3. Guardrails

الـ **Guardrails** هي الضوابط التي تمنع أو تقلل القرارات أو الأفعال غير المسموح بها.

قد تشمل:

- التحقق من المدخلات.
- تقييد الأدوات المتاحة.
- التحقق من الصلاحيات قبل التنفيذ.
- منع الإجراءات التي تتجاوز النطاق المحدد.
- فرض شروط قبل تنفيذ الإجراءات الحساسة.

الـ Prompt يستطيع توجيه السلوك، لكن القيود المهمة يجب فرضها على مستوى النظام أو الأداة أو الصلاحيات عندما يكون ذلك ممكنًا.

> **Prompt guides behavior. System controls execution.**

## 4. Human Approval

بعض الإجراءات تحتاج إلى موافقة بشرية قبل تنفيذها.

```text
Agent proposes action
        ↓
Approval required
        ↓
Human reviews
        ↓
Approve / Reject
        ↓
Execute only if approved
```

يجب أن يتناسب التدخل البشري مع مستوى الخطر وأثر الإجراء.

## 5. Escalation

تحدث الـ **Escalation** عندما يحتاج الموقف إلى تدخل جهة بشرية أو قرار خارج صلاحيات الـ Agent.

أمثلة:

- المعلومات غير كافية.
- القرار خارج نطاق المسؤولية.
- الإجراء يحتاج إلى موافقة.
- الحالة غير واضحة أو غير متوقعة.

## 6. Stop Conditions

يجب أن يعرف الـ Agent متى يتوقف عن العمل.

أمثلة:

```text
Goal achieved
        ↓
Complete

Required information unavailable
        ↓
Ask or Stop

Action requires human intervention
        ↓
Escalate

Task outside scope
        ↓
Stop
```

القدرة على التوقف جزء أساسي من التصميم، وليست مجرد حالة استثنائية.

### مخرج الـ Layer

**Control Design**

يحدد حدود الاستقلالية، والصلاحيات، والضوابط، ومتى يحتاج الـ Agent إلى موافقة أو إحالة أو توقف.

---

# LAYER 06 — EVALUATE
## كيف نعرف أن الـ Agent حقق الهدف وتصرف بشكل صحيح؟

لا يكفي أن نحكم على الـ Agent من خلال النتيجة النهائية فقط.

قد يصل إلى نتيجة صحيحة بعد استخدام أداة غير مسموح بها، أو تجاوز موافقة مطلوبة، أو اتخاذ قرار دون معلومات كافية.

لذلك نحتاج إلى تقييم النتيجة والسلوك ومسار التنفيذ.

## 1. Outcome Evaluation

هل حقق الـ Agent الهدف المطلوب؟

نقارن النتيجة النهائية بمعايير النجاح المحددة في Agent Contract.

## 2. Behavioral Evaluation

هل التزم الـ Agent بنطاق مسؤوليته وحدود قراراته؟

نراجع مثلًا:

- هل اتخذ قرارات ضمن Scope؟
- هل استخدم الأدوات المسموح بها؟
- هل طلب المعلومات عند الحاجة؟
- هل احترم شروط الموافقة؟

## 3. Trajectory Evaluation

الـ **Trajectory** هي تسلسل القرارات والإجراءات التي اتخذها الـ Agent للوصول إلى النتيجة.

نراجع هذا التسلسل لمعرفة ما إذا كانت الخطوات مناسبة، وليس فقط ما إذا كانت النتيجة النهائية صحيحة.

مثال:

```text
Observe
   ↓
Decision
   ↓
Tool
   ↓
Result
   ↓
Next Decision
   ↓
Complete
```

## 4. Improvement Loop

نستخدم نتائج التقييم لتحديد ما يحتاج إلى تحسين في التصميم.

```text
Evaluate
   ↓
Identify the Problem
   ↓
Find the Design Cause
   ↓
Improve
   ↓
Evaluate Again
```

قد تكشف المشكلة عن:

- مسؤولية غير واضحة.
- معلومات غير كافية.
- أداة غير مناسبة.
- حدود قرار غير محددة.
- صلاحيات أوسع من المطلوب.
- شرط توقف غير واضح.

الهدف هو تحديد الجزء الذي يحتاج إلى تعديل بدلًا من افتراض أن كل مشكلة سببها نموذج الذكاء الاصطناعي نفسه.

### مخرج الـ Layer

**Evaluation Design**

يحدد كيف نقيم النتيجة، والسلوك، ومسار التنفيذ، وكيف نستخدم النتائج لتحسين تصميم الـ Agent.

---

# 4. Agent Design Canvas

استخدم  هذا الـ Canvas لتنظيم تصميم أي Agent قبل بنائه.
## A. CHOOSE — Architecture

**Problem:**  
ما المشكلة التي نحاول حلها؟

**Architecture:**  
Workflow / AI-Powered Workflow / Agent

**Why this architecture?**  
لماذا اخترنا هذا النوع من الأنظمة؟

**Required Agency:**  
ما مقدار الاستقلالية المطلوبة؟

## B. DEFINE — Responsibility

**Responsibility:**  
ما مسؤولية الـ Agent؟

**Success Criteria:**  
متى نعتبر المهمة ناجحة؟

**Scope:**  
ما الذي يدخل ضمن مسؤوليته؟

**Out of Scope:**  
ما الذي لا يدخل ضمن مسؤوليته؟

**Decision Boundaries:**  
ما القرارات المسموح بها، وما الذي يحتاج إلى شروط أو موافقة؟

**Missing Information:**  
ماذا يفعل عندما تكون المعلومات غير كافية؟

## C. DECIDE — Decision Design

**Observation:**  
ما المعلومات أو النتائج التي يحتاج إلى ملاحظتها؟

**Decision Options:**  
ما الخطوات التي يمكنه الاختيار بينها؟

**Decision Conditions:**  
ما الظروف التي تؤثر في اختياره؟

**Validation:**  
ما الذي يجب التحقق منه قبل التنفيذ؟

## D. ACT — Action Design

**Tools:**  
ما الأدوات التي يحتاج إليها؟

**Tool Conditions:**  
متى يستخدم كل Tool؟

**Tool Inputs:**  
ما البيانات التي يحتاج إليها؟

**Tool Results:**  
كيف يستخدم نتائج التنفيذ في اتخاذ القرار التالي؟

**Context:**  
ما المعلومات الضرورية للقرار؟

**Memory & State:**  
ما المعلومات التي يجب الاحتفاظ بها، وما الحالة الحالية للمهمة؟

## E. CONTROL — Control Design

**Autonomy Boundaries:**  
أين يستطيع العمل باستقلالية؟

**Permissions:**  
ما الصلاحيات المطلوبة؟

**Guardrails:**  
ما الضوابط التي تمنع الإجراءات غير المسموح بها؟

**Human Approval:**  
ما الإجراءات التي تحتاج إلى موافقة؟

**Escalation:**  
متى يحتاج إلى تدخل بشري؟

**Stop Conditions:**  
متى يجب أن يتوقف؟

## F. EVALUATE — Evaluation Design

**Outcome:**  
هل حقق الهدف؟

**Behavior:**  
هل التزم بالحدود؟

**Trajectory:**  
هل كان مسار القرارات والإجراءات مناسبًا؟

**Improvement:**  
ما الذي يحتاج إلى تعديل؟

---

# 5. المبادئ الهندسية الأساسية

## 01 — Problem Before Agent

ابدأ بالمشكلة، لا بالأداة.

## 02 — Simplest Suitable Architecture

اختر أبسط بنية تحقق الهدف، ولا تستخدم Agent عندما يكون Workflow كافيًا.

## 03 — Purposeful Autonomy

كل مساحة قرار تمنحها للـ Agent يجب أن تكون لها حاجة واضحة.

## 04 — Define the Boundaries

كل Agent يحتاج إلى مسؤولية ونطاق وحدود قرار واضحة.

## 05 — Decision Is Not Authorization

قد يكون القرار مناسبًا، لكن التنفيذ يظل خاضعًا لصلاحيات النظام.

## 06 — Least Privilege

لا تمنح الـ Agent صلاحيات تتجاوز ما يحتاج إليه.

## 07 — Tools Must Serve the Goal

أضف الأدوات التي يحتاج إليها لتحقيق مسؤوليته، لا الأدوات التي يمكنه استخدامها فحسب.

## 08 — Memory Is Not State

الذاكرة تحتفظ بالمعلومات، بينما الحالة توضح المرحلة الحالية للمهمة.

## 09 — Validate Before Acting

تحقق من المعلومات والقرار والصلاحيات قبل تنفيذ الإجراءات المهمة.

## 10 — Know When to Stop

يجب أن يعرف الـ Agent متى يكمل، أو يسأل، أو ينتظر، أو يصعّد، أو يتوقف.

## 11 — Human Oversight Should Match Risk

كلما زادت خطورة الإجراء وأثره، زادت الحاجة إلى ضوابط ومراجعة مناسبة.

## 12 — Evaluate Behavior, Not Only Results

النتيجة الصحيحة لا تعني بالضرورة أن الـ Agent تصرف بطريقة صحيحة.

## 13 — Improve the Design

عند ظهور مشكلة، ابحث عن السبب في تصميم النظام، وليس في النموذج وحده.

---

# 6. الخلاصة

هندسة الـ Agent ليست مجرد إعطاء نموذج الذكاء الاصطناعي أدوات والسماح له بالعمل باستقلالية.

إنها عملية تصميم متكاملة:

```text
CHOOSE
هل نحتاج Agent؟
        ↓
DEFINE
ما مسؤوليته وحدوده؟
        ↓
DECIDE
كيف يختار الخطوة التالية؟
        ↓
ACT
كيف ينفذ القرار؟
        ↓
CONTROL
ما الذي يُسمح له بفعله؟
        ↓
EVALUATE
هل حقق الهدف وتصرف بشكل صحيح؟
        ↓
IMPROVE
كيف نحسن التصميم؟
```

الهدف ليس بناء أكثر Agent استقلالية.

بل بناء Agent يمتلك القدر المناسب من الاستقلالية، ويستطيع اتخاذ قرارات مفيدة، وتنفيذ إجراءات مسموح بها، والتوقف عندما ينبغي له التوقف، مع إمكانية تقييم سلوكه وتحسينه.

> **Engineer the autonomy — don't just enable it.**

