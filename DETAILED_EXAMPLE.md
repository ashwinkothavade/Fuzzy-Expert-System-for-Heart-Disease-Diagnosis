# Complete Fuzzy Logic Example: Step-by-Step Walkthrough

This document provides a detailed trace of the fuzzy logic diagnosis system for a specific patient case.

---

## Patient Profile

**Case:** 55-year-old male with multiple cardiovascular risk factors

### Input Parameters:
```
Age:              55 years
Sex:              0 (Male)
Chest Pain:       2 (Atypical Angina)
Blood Pressure:   165 mmHg
Cholesterol:      280 mg/dL
Blood Sugar:      130 mg/dL
ECG:              0.5
Heart Rate:       180 bpm
Exercise Angina:  1 (Yes)
Old Peak:         2.5 mm
Thallium Scan:    6 (Medium/Fixed Defect)
```

---

# STAGE 1: FUZZIFICATION

Converting crisp numerical inputs into fuzzy membership degrees.

## 1.1 Age = 55 years

### Membership Functions:

**Young (0 ≤ x ≤ 29):**
```
μ_young(x) = 1.0                if 0 ≤ x ≤ 29
μ_young(x) = (-1/9)x + 38/9     if 29 < x ≤ 38
μ_young(x) = 0.0                if x > 38
```

**Mild (33 ≤ x ≤ 45):**
```
μ_mild(x) = 0.0                 if x < 33
μ_mild(x) = (1/5)x - 33/5       if 33 ≤ x ≤ 38
μ_mild(x) = (-1/7)x + 45/7      if 38 < x ≤ 45
μ_mild(x) = 0.0                 if x > 45
```

**Old (40 ≤ x ≤ 58):**
```
μ_old(x) = 0.0                  if x < 40
μ_old(x) = (1/8)x - 40/8        if 40 ≤ x ≤ 48
μ_old(x) = (-1/10)x + 58/10     if 48 < x ≤ 58
μ_old(x) = 0.0                  if x > 58
```

**Very Old (52 < x):**
```
μ_very_old(x) = 0.0             if x ≤ 52
μ_very_old(x) = (1/8)x - 52/8   if 52 < x ≤ 60
μ_very_old(x) = 1.0             if x > 60
```

### Calculation for Age = 55:

```python
μ_young(55) = 0.0  (55 > 38, outside range)

μ_mild(55) = 0.0  (55 > 45, outside range)

μ_old(55) = (-1/10) × 55 + 58/10
          = -5.5 + 5.8
          = 0.3

μ_very_old(55) = (1/8) × 55 - 52/8
               = 6.875 - 6.5
               = 0.375
```

### Result:
```python
age = {
    'young': 0.0,
    'mild': 0.0,
    'old': 0.3,
    'very_old': 0.375
}
```

**Interpretation:** At 55 years, the patient is 30% "old" and 37.5% "very old"

---

## 1.2 Blood Pressure = 165 mmHg

### Membership Functions:

**Low:**
```
μ_low(x) = 1.0                  if x ≤ 111
μ_low(x) = (-1/23)x + 134/23    if 111 < x ≤ 134
μ_low(x) = 0.0                  if x > 134
```

**Medium:**
```
μ_medium(x) = 0.0               if x < 127
μ_medium(x) = (1/12)x - 127/12  if 127 ≤ x ≤ 139
μ_medium(x) = (-1/14)x + 153/14 if 139 < x ≤ 153
μ_medium(x) = 0.0               if x > 153
```

**High:**
```
μ_high(x) = 0.0                 if x < 142
μ_high(x) = (1/15)x - 142/15    if 142 ≤ x ≤ 157
μ_high(x) = (-1/15)x + 172/15   if 157 < x ≤ 172
μ_high(x) = 0.0                 if x > 172
```

**Very High:**
```
μ_very_high(x) = 0.0            if x ≤ 154
μ_very_high(x) = (1/17)x - 154/17  if 154 < x ≤ 171
μ_very_high(x) = 1.0            if x > 171
```

### Calculation for BP = 165:

```python
μ_low(165) = 0.0  (165 > 134)

μ_medium(165) = 0.0  (165 > 153)

μ_high(165) = (-1/15) × 165 + 172/15
            = -11.0 + 11.467
            = 0.467

μ_very_high(165) = (1/17) × 165 - 154/17
                 = 9.706 - 9.059
                 = 0.647
```

### Result:
```python
blood_pressure = {
    'low': 0.0,
    'medium': 0.0,
    'high': 0.467,
    'very_high': 0.647
}
```

**Interpretation:** BP of 165 is 46.7% "high" and 64.7% "very high"

---

## 1.3 Cholesterol = 280 mg/dL

### Calculation:

```python
μ_low(280) = 0.0  (280 > 197)

μ_medium(280) = 0.0  (280 > 250)

μ_high(280) = (-1/44) × 280 + 307/44
            = -6.364 + 6.977
            = 0.613

μ_very_high(280) = 0.0  (280 < 281, just below threshold)
```

### Result:
```python
cholesterol = {
    'low': 0.0,
    'medium': 0.0,
    'high': 0.613,
    'very_high': 0.0
}
```

---

## 1.4 Blood Sugar = 130 mg/dL

### Membership Function:

```
μ_true(x) = 0.0         if x < 105
μ_true(x) = 0.067x - 7  if 105 ≤ x < 120
μ_true(x) = 1.0         if x ≥ 120
```

### Calculation:

```python
μ_true(130) = 1.0  (130 ≥ 120)
μ_false(130) = 1 - 1.0 = 0.0
```

### Result:
```python
blood_sugar = {
    'true': 1.0,
    'false': 0.0
}
```

---

## 1.5 Heart Rate = 180 bpm

### Calculation:

```python
μ_low(180) = 0.0  (180 > 141)

μ_medium(180) = (-1/42) × 180 + 194/42
              = -4.286 + 4.619
              = 0.333

μ_high(180) = (1/58) × 180 - 152/58
            = 3.103 - 2.621
            = 0.483
```

### Result:
```python
heart_rate = {
    'low': 0.0,
    'medium': 0.333,
    'high': 0.483
}
```

---

## 1.6 ECG = 0.5

### Calculation:

```python
μ_normal(0.5) = 0.0  (0.5 > 0.4)

μ_abnormal(0.5) = (-1/0.8) × 0.5 + 1.8/0.8
                = -0.625 + 2.25
                = 1.625 (capped at 1.0 in range check)
                
Actually: 0.5 is in range [0.2, 1.0]
μ_abnormal(0.5) = (1/0.8) × 0.5 - 0.2/0.8
                = 0.625 - 0.25
                = 0.375

μ_hypertrophy(0.5) = 0.0  (0.5 < 1.4)
```

### Result:
```python
ECG = {
    'normal': 0.0,
    'abnormal': 0.375,
    'hypertrophy': 0.0
}
```

---

## 1.7 Old Peak = 2.5 mm

### Calculation:

```python
μ_low(2.5) = 0.0  (2.5 > 2)

μ_risk(2.5) = (1/1.3) × 2.5 - 1.5/1.3
            = 1.923 - 1.154
            = 0.769

μ_terrible(2.5) = 0.0  (2.5 = threshold, just at boundary)
```

### Result:
```python
old_peak = {
    'low': 0.0,
    'risk': 0.769,
    'terrible': 0.0
}
```

---

## 1.8 Singleton Parameters

### Chest Pain = 2 (Atypical Angina):
```python
chest_pain = {
    'typical_anginal': 0.0,
    'atypical_anginal': 1.0,
    'non_anginal_pain': 0.0,
    'asymptomatic': 0.0
}
```

### Exercise = 1 (Yes):
```python
exercise = {
    'true': 1.0,
    'false': 0.0
}
```

### Thallium = 6 (Medium):
```python
thallium = {
    'normal': 0.0,
    'medium': 1.0,
    'high': 0.0
}
```

### Sex = 0 (Male):
```python
sex = {
    'male': 1.0,
    'female': 0.0
}
```

---

# STAGE 2: INFERENCE ENGINE

Applying 54 fuzzy rules using MIN (AND) and MAX (OR) operators.

## Rule Application Examples:

### Rule 1: IF (age IS very_old) AND (chest_pain IS atypical_anginal) THEN sick_4

```python
activation = min(age['very_old'], chest_pain['atypical_anginal'])
           = min(0.375, 1.0)
           = 0.375

output['sick4'].append(0.375)
```

**Explanation:** Both conditions partially true, take minimum (AND operation)

---

### Rule 2: IF (heart_rate IS high) AND (age IS old) THEN sick_4

```python
activation = min(heart_rate['high'], age['old'])
           = min(0.483, 0.3)
           = 0.3

output['sick4'].append(0.3)
```

---

### Rule 9: IF (chest_pain IS asymptomatic) OR (age IS very_old) THEN sick_1

```python
activation = max(chest_pain['asymptomatic'], age['very_old'])
           = max(0.0, 0.375)
           = 0.375

output['sick1'].append(0.375)
```

**Explanation:** At least one condition true, take maximum (OR operation)

---

### Rule 12: IF chest_pain IS atypical_anginal THEN sick_1

```python
activation = chest_pain['atypical_anginal']
           = 1.0

output['sick1'].append(1.0)
```

---

### Rule 17: IF sex IS male THEN sick_2

```python
activation = sex['male']
           = 1.0

output['sick2'].append(1.0)
```

---

### Rule 20: IF blood_pressure IS high THEN sick_2

```python
activation = blood_pressure['high']
           = 0.467

output['sick2'].append(0.467)
```

---

### Rule 22: IF blood_pressure IS very_high THEN sick_4

```python
activation = blood_pressure['very_high']
           = 0.647

output['sick4'].append(0.647)
```

---

### Rule 25: IF cholesterol IS high THEN sick_2

```python
activation = cholesterol['high']
           = 0.613

output['sick2'].append(0.613)
```

---

### Rule 38: IF exercise IS true THEN sick_3

```python
activation = exercise['true']
           = 1.0

output['sick3'].append(1.0)
```

---

### Rule 44: IF old_peak IS risk THEN sick_2

```python
activation = old_peak['risk']
           = 0.769

output['sick2'].append(0.769)
```

---

### Rule 51: IF thallium IS medium THEN sick_2

```python
activation = thallium['medium']
           = 1.0

output['sick2'].append(1.0)
```

---

## Aggregation: Taking Maximum for Each Category

After all 54 rules fire, collect activations:

```python
output = {
    'sick1': [0.0, 0.375, 1.0, 0.0, ...],
    'sick2': [1.0, 0.467, 0.613, 0.769, 1.0, ...],
    'sick3': [1.0, 0.3, 0.0, ...],
    'sick4': [0.375, 0.3, 0.647, 0.0, ...],
    'healthy': [0.0, 0.0, 0.0, ...]
}
```

**Aggregate by taking maximum:**

```python
inference_output = {
    'outputsick_sick1': max([0.0, 0.375, 1.0, ...]) = 1.0,
    'outputsick_sick2': max([1.0, 0.467, 0.613, 0.769, 1.0, ...]) = 1.0,
    'outputsick_sick3': max([1.0, 0.3, ...]) = 1.0,
    'outputsick_sick4': max([0.375, 0.3, 0.647, ...]) = 0.647,
    'outputsick_healthy': max([0.0, 0.0, ...]) = 0.0
}
```

**Interpretation:** Multiple rules strongly indicate sick1, sick2, and sick3 (all at 1.0), with moderate indication of sick4 (0.647)

---

# STAGE 3: DEFUZZIFICATION

Converting fuzzy output to crisp diagnosis using Center of Gravity (CoG) method.

## Output Membership Functions:

### Healthy:
```
μ_healthy(x) = 1.0                  if 0 ≤ x ≤ 0.25
μ_healthy(x) = (-1/0.75)x + 1/0.75  if 0.25 < x ≤ 1.0
μ_healthy(x) = 0.0                  if x > 1.0
```

### Sick 1:
```
μ_sick1(x) = x          if 0 ≤ x ≤ 1.0
μ_sick1(x) = -x + 2.0   if 1.0 < x ≤ 2.0
μ_sick1(x) = 0.0        if x > 2.0
```

### Sick 2:
```
μ_sick2(x) = x - 1.0    if 1.0 ≤ x ≤ 2.0
μ_sick2(x) = -x + 3.0   if 2.0 < x ≤ 3.0
μ_sick2(x) = 0.0        otherwise
```

### Sick 3:
```
μ_sick3(x) = x - 2.0    if 2.0 ≤ x ≤ 3.0
μ_sick3(x) = -x + 4.0   if 3.0 < x ≤ 4.0
μ_sick3(x) = 0.0        otherwise
```

### Sick 4:
```
μ_sick4(x) = 0.0                    if x ≤ 3.0
μ_sick4(x) = (1/0.75)x - 3/0.75     if 3.0 < x ≤ 3.75
μ_sick4(x) = 1.0                    if x > 3.75
```

---

## CoG Calculation Process:

### Step 1: Clip membership functions at activation levels

For each point x in domain [0, 5]:

```python
s_healthy = min(μ_healthy(x), 0.0) = 0.0  (always 0)
s1 = min(μ_sick1(x), 1.0)
s2 = min(μ_sick2(x), 1.0)
s3 = min(μ_sick3(x), 1.0)
s4 = min(μ_sick4(x), 0.647)
```

### Step 2: Take union (maximum)

```python
result(x) = max(s_healthy, s1, s2, s3, s4)
```

### Step 3: Calculate weighted average

```python
numberOfPoints = 1000
delta = 5.0 / 1000 = 0.005

numerator = 0.0
denominator = 0.0

for i in range(1001):
    x = i * delta
    
    # Calculate clipped values
    s1 = min(μ_sick1(x), 1.0)
    s2 = min(μ_sick2(x), 1.0)
    s3 = min(μ_sick3(x), 1.0)
    s4 = min(μ_sick4(x), 0.647)
    s_healthy = 0.0
    
    # Union
    res = max(s1, s2, s3, s4, s_healthy)
    
    # Accumulate
    numerator += res * x * delta
    denominator += res * delta

CoG = numerator / denominator
```

### Example calculations at specific points:

**At x = 1.5:**
```python
s1 = min((-1.5 + 2.0), 1.0) = min(0.5, 1.0) = 0.5
s2 = min((1.5 - 1.0), 1.0) = min(0.5, 1.0) = 0.5
s3 = 0.0  (1.5 < 2.0)
s4 = 0.0  (1.5 < 3.0)
result = max(0.5, 0.5, 0.0, 0.0) = 0.5
```

**At x = 2.5:**
```python
s1 = 0.0  (2.5 > 2.0)
s2 = min((-2.5 + 3.0), 1.0) = min(0.5, 1.0) = 0.5
s3 = min((2.5 - 2.0), 1.0) = min(0.5, 1.0) = 0.5
s4 = 0.0  (2.5 < 3.0)
result = max(0.0, 0.5, 0.5, 0.0) = 0.5
```

**At x = 3.2:**
```python
s1 = 0.0
s2 = 0.0  (3.2 > 3.0)
s3 = min((-3.2 + 4.0), 1.0) = min(0.8, 1.0) = 0.8
s4 = min((1/0.75)*3.2 - 3/0.75, 0.647) = min(0.267, 0.647) = 0.267
result = max(0.0, 0.0, 0.8, 0.267) = 0.8
```

### Final CoG Result:

```python
CoG = 2.6641300197005533
```

---

## Diagnosis Mapping:

```python
result = ''

if CoG < 1.78:
    result += 'healthy, '
# 2.664 < 1.78? NO

if 1.0 <= CoG <= 2.51:
    result += 'sick1, '
# 1.0 <= 2.664 <= 2.51? NO (2.664 > 2.51)

if 1.78 <= CoG < 3.25:
    result += 'sick2, '
# 1.78 <= 2.664 < 3.25? YES ✓

if 1.5 <= CoG <= 4.5:
    result += 'sick3, '
# 1.5 <= 2.664 <= 4.5? YES ✓

if CoG >= 3.25:
    result += 'sick4, '
# 2.664 >= 3.25? NO

final_output = result + str(CoG)
```

---

# FINAL RESULT

```
sick2, sick3, 2.6641300197005533
```

## Clinical Interpretation:

**Diagnosis:** Moderate to Significant Cardiovascular Risk

**Risk Level:** The patient falls into overlapping categories:
- **Sick 2 (Moderate Risk):** CoG in range [1.78, 3.25)
- **Sick 3 (Significant Risk):** CoG in range [1.5, 4.5]

**Key Risk Factors Identified:**
1. Age 55 (old/very old category)
2. Blood pressure 165 mmHg (high/very high)
3. Cholesterol 280 mg/dL (high)
4. Blood sugar 130 mg/dL (elevated)
5. Heart rate 180 bpm (high)
6. Exercise-induced angina (present)
7. Old peak 2.5 mm (risk level)
8. Thallium scan shows fixed defect
9. Male gender (higher risk)

**Recommendation:**
- Medical consultation strongly advised
- Lifestyle modifications recommended
- Follow-up cardiovascular testing
- Consider medication for BP and cholesterol
- Stress test evaluation

---

# Summary of Fuzzy Logic Process

## Why Fuzzy Logic?

This example demonstrates the power of fuzzy logic in medical diagnosis:

1. **Handles Uncertainty:** Age 55 is neither definitively "old" nor "very old" - it's 30% one and 37.5% the other

2. **Smooth Transitions:** Blood pressure 165 is partially "high" (46.7%) and partially "very high" (64.7%)

3. **Multiple Diagnoses:** Patient exhibits characteristics of both Sick 2 and Sick 3, reflecting medical reality

4. **Expert Knowledge:** 54 rules encode medical expertise about risk factor combinations

5. **Gradual Risk Assessment:** CoG of 2.66 indicates moderate-to-significant risk, not a binary yes/no

## Advantages Over Crisp Logic:

**Crisp Logic Would Say:**
- Age 55 → "Old" (binary decision)
- BP 165 → "High" (threshold-based)
- Result → Single category

**Fuzzy Logic Says:**
- Age 55 → 30% old, 37.5% very old (gradual)
- BP 165 → 46.7% high, 64.7% very high (overlapping)
- Result → Multiple categories with confidence level

This better reflects the continuous nature of cardiovascular disease progression!

---

**Document Created:** November 17, 2025  
**System:** MediFuzzy v1.0  
**Example Patient:** 55-year-old male, moderate-to-significant risk
