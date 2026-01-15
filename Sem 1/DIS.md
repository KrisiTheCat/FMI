# Математически Анализ II - Теоретичен Справочник (К2)

---

### **1. Граница на функция ($x \to a$)**
* **Дефиниция на Коши ($\varepsilon - \delta$):**
    - $\forall \varepsilon > 0, \exists \delta > 0 : \forall x \in D_f, 0 < |x - a| < \delta \implies |f(x) - b| < \varepsilon$
* **Дефиниция на Хайне (Редици):**
    - $\forall \{x_n\}_{n=1}^{\infty} \subset D_f \setminus \{a\}, \lim_{n \to \infty} x_n = a \implies \lim_{n \to \infty} f(x_n) = b$
* **Граница клоняща към $+\infty$ (Твоят стил):**
    - $\forall \varepsilon > 0, \exists \delta > 0 : \forall x, 0 < |x - a| < \delta \implies f(x) > \frac{1}{\varepsilon}$
* **Граница клоняща към $-\infty$:**
    - $\forall \varepsilon > 0, \exists \delta > 0 : \forall x, 0 < |x - a| < \delta \implies f(x) < -\frac{1}{\varepsilon}$

---

### **2. Непрекъснатост и Равномерна непрекъснатост**
* **Непрекъснатост в точка ($a$):**
    - $\forall \varepsilon > 0, \exists \delta > 0 : \forall x \in D_f, |x - a| < \delta \implies |f(x) - f(a)| < \varepsilon$
* **Равномерна непрекъснатост:**
    - $\delta$ зависи само от $\varepsilon$ за целия интервал.
    - $\forall \varepsilon > 0, \exists \delta > 0 : \forall x_1, x_2 \in D_f, |x_1 - x_2| < \delta \implies |f(x_1) - f(x_2)| < \varepsilon$

---

### **3. Диференцируемост в точка ($a$)**
* **Дефиниция:** Наличие на крайна граница на диференциалното частно.
* $\exists L \in \mathbb{R} : \forall \varepsilon > 0, \exists \delta > 0 : \forall h, 0 < |h| < \delta \implies \left| \frac{f(a+h) - f(a)}{h} - L \right| < \varepsilon$

---

### **4. Интегруемост по Риман в $[a, b]$**
* **Определен интеграл ($I$):** Точната стойност на лицето под кривата.
* **Дефиниция:** - $\exists I \in \mathbb{R} : \forall \varepsilon > 0, \exists \delta > 0, \forall \sigma : d(\sigma) < \delta \implies \left| \sum_{i=1}^{n} f(\xi_i)\Delta x_i - I \right| < \varepsilon$
* **Елементи:** $\sigma$ (разделяне), $d(\sigma)$ (диаметър), $\xi_i$ (произволна точка), $\sum f(\xi_i)\Delta x_i$ (Риманова сума).

---

### **5. Основна теорема на интегралното смятане**
* **Формулировка:** Нека $f$ е непрекъсната в $[a, b]$. Нека $F$ е дефинирана като:
    - $F(x) = \int_{a}^{x} f(t) dt, \quad x \in [a, b]$
* **Резултат 1:** $F$ е диференцируема във всяка точка и $F'(x) = f(x)$.
* **Резултат 2 (Обратното):** Ако $G$ е примитивна на $f$ ($G' = f$), то:
    - $\int_{a}^{x} f(t) dt = G(x) - G(a)$

---

### **6. Важни зависимости**
* **Йерархия:** Диференцируемост $\implies$ Непрекъснатост $\implies$ Интегруемост.
* **Т-ма на Кантор:** Непрекъснатост в затворен интервал $[a, b] \implies$ Равномерна непрекъснатост.

---

### **Програмен модел (SymPy)**

**config.py**
```python
from sympy import Symbol, S
VAR_X = Symbol('x')
VAR_N = Symbol('n', integer=True)
