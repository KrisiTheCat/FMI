# Математически Анализ II - Теоретичен Справочник (К2)

---

### **1. Граница на функция ($x \to a$)**
* **Дефиниция на Коши ($\varepsilon-\delta$):**
    - Описва поведението чрез околности.
    - $\forall \varepsilon > 0, \exists \delta > 0 : \forall x \in D_f, 0 < |x - a| < \delta \implies |f(x) - b| < \varepsilon$
* **Дефиниция на Хайне (Редици):**
    - Свежда границата на функция до граница на редица.
    - $\forall \{x_n\}_{n=1}^{\infty} \subset D_f \setminus \{a\}, \lim_{n \to \infty} x_n = a \implies \lim_{n \to \infty} f(x_n) = b$
* **Граница към $-\infty$:**
    - $\forall M > 0, \exists \delta > 0 : \forall x \in D_f, 0 < |x - a| < \delta \implies f(x) < -M$

---

### **2. Непрекъснатост в точка (а)**
* Функцията е дефинирана в $a$ и границата съвпада със стойността $f(a)$.
* **Коши:** $\forall \varepsilon > 0, \exists \delta > 0 : \forall x \in D_f, |x - a| < \delta \implies |f(x) - f(a)| < \varepsilon$
* **Хайне:** $\forall \{x_n\}_{n=1}^{\infty} \subset D_f, \lim_{n \to \infty} x_n = a \implies \lim_{n \to \infty} f(x_n) = f(a)$

---

### **3. Диференцируемост в точка (а)**
* Съществува крайна граница на диференциалното частно.
* $f'(a) = \lim_{h \to 0} \frac{f(a+h) - f(a)}{h}$
* **С квантори:** $\exists L \in \mathbb{R} : \forall \varepsilon > 0, \exists \delta > 0 : \forall h, 0 < |h| < \delta \implies \left| \frac{f(a+h) - f(a)}{h} - L \right| < \varepsilon$

---

### **4. Интегруемост по Риман в [a, b]**
* Лицето под кривата се дефинира чрез граница на Риманови суми.
* $\exists I \in \mathbb{R} : \forall \varepsilon > 0, \exists \delta > 0, \forall \sigma : d(\sigma) < \delta \implies \left| \sum_{i=1}^{n} f(\xi_i)\Delta x_i - I \right| < \varepsilon$
* **Условия:** Всяка непрекъсната функция в $[a, b]$ е интегруема.

---

### **5. Основни връзки и теореми**
* **Йерархия:** Диференцируемост $\implies$ Непрекъснатост $\implies$ Интегруемост.
* **Т-ма на Нютон-Лайбниц:** $\int_a^b f(x) dx = F(b) - F(a)$, където $F'(x) = f(x)$.
* **Функция на горната граница:** $\Phi(x) = \int_a^x f(t) dt \implies \Phi'(x) = f(x)$.

---

### **Програмен модел (SymPy)**

**config.py**
```python
from sympy import Symbol, S
VAR_X = Symbol('x')
VAR_N = Symbol('n', integer=True)
