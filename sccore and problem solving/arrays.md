# Problem Solving with Arrays (Dart) - Curriculum

هذه الخطة الشاملة لتعلم وإتقان حل المشكلات باستخدام المصفوفات (Arrays/Lists) في لغة Dart.

## Lectures Overview

- **Lecture 1**: Array Fundamentals & Memory
- **Lecture 2**: Iteration & Dart Built-in Functions
- **Lecture 3**: Sorting & Binary Search
- **Lecture 4**: Two Pointers Technique
- **Lecture 5**: Sliding Window Technique
- **Lecture 6**: Prefix Sum & Kadane's Algorithm
- **Lecture 7**: Hashing & Frequency Counting
- **Lecture 8**: 2D Arrays & Matrices
- **Lecture 9**: Final Milestone (Mini-Project)

---

## Detailed Notes

### Lecture 1

- **Array Fundamentals & Memory**
   - كيفية تخزين المصفوفات وتوزيع العناوين داخل الـ RAM.
   - الفرق بين Static Arrays و Dynamic Arrays.
   - فهم ديناميكية الـ List في Dart وكيفية تمددها في الذاكرة.
   - تكلفة العمليات الأساسية ($O(1)$ للوصول والتعديل).
   
   > **Summary**
   > - المصفوفات تحجز مساحات متجاورة في الذاكرة العشوائية.
   > - الوصول لأي عنصر عن طريق الفهرس يكلف وقت $O(1)$ بسبب العناوين المتجاورة.
   > - إضافة عنصر في بداية المصفوفة مكلف ويأخذ وقت $O(N)$ بسبب الحاجة لإزاحة العناصر.
   > 
   > **Key rule to remember:** العناوين المتجاورة في الذاكرة هي السبب الرئيسي لسرعة المصفوفات في الوصول والتعديل.
   > 
   > **Common mistake to avoid:** استخدام عملية الإضافة في بداية مصفوفة تحتوي على عدد كبير من العناصر مما يسبب بطء في الأداء.
