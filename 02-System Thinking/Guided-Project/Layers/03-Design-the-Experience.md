# 03 — Design the Experience


# 1. User Journey

يمر المستخدم في SkillBridge Lite بالرحلة التالية:

| Step | User Action | Screen | Expected Experience |
|---|---|---|---|
| 1 | يفتح الموقع | Home | يتعرف على SkillBridge Lite ووظيفته |
| 2 | يضغط على My Profile | My Profile | يستعرض معلوماته المهنية |
| 3 | يضغط على Edit Profile | Edit Profile | يعدّل معلوماته ومهاراته وخبراته |
| 4 | يحفظ التعديلات | My Profile | يرى ملفه المهني بعد التحديث |
| 5 | يضغط على Opportunities | Opportunities | يستعرض الفرص المهنية المتاحة |
| 6 | يختار فرصة | Opportunity Details | يطّلع على تفاصيل الفرصة ومتطلباتها |
| 7 | يضغط على Analyze My Fit | Analysis Results | يستعرض مدى ارتباط ملفه بالفرصة |
| 8 | يراجع النتائج | Analysis Results | يتعرف على المهارات المرتبطة والفجوات المحتملة والخطوات المقترحة |
| 9 | يعود إلى الفرص | Opportunities | يستكشف فرصة أخرى |

## Main User Flow

```text
                    HOME
                   /    \
                  /      \
                 v        v
          MY PROFILE   OPPORTUNITIES
                |           |
                v           v
          EDIT PROFILE   OPPORTUNITY
                |         DETAILS
                v           |
          MY PROFILE        | Analyze My Fit
                            v
                     ANALYSIS RESULTS
                            |
                            v
                      OPPORTUNITIES
