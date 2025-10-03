# Dosing Verification Against AAP/FDA Guidelines

## Purpose
This document verifies the CloseDose calculator's dosing recommendations against established AAP (American Academy of Pediatrics) and FDA dosing guidelines.

## Reference Guidelines

### Acetaminophen (Tylenol)
**Source**: AAP, FDA, Pediatric Dosage Handbook

- **Standard dose**: 10-15 mg/kg per dose
- **Dosing interval**: Every 4-6 hours as needed
- **Maximum single dose**: 
  - Infants 0-3 months: Consult physician
  - Infants 4-11 months: 80-160 mg (based on weight)
  - Children 2-11 years: Up to 480 mg
  - Children 12+ years: Up to 1000 mg (or 15 mg/kg, whichever is less)
- **Maximum daily dose**: 75 mg/kg/day or 4000 mg/day (whichever is less), divided into 4-6 doses
- **Concentration**: Most common is 160 mg/5 mL for children's suspension

### Ibuprofen (Motrin/Advil)
**Source**: AAP, FDA, Pediatric Dosage Handbook

- **Standard dose**: 5-10 mg/kg per dose
- **Dosing interval**: Every 6-8 hours as needed
- **Age restriction**: Not recommended for infants under 6 months
- **Maximum single dose**:
  - Children 6 months - 11 years: 400 mg (or 10 mg/kg, whichever is less)
  - Children 12+ years: 400-800 mg
- **Maximum daily dose**: 40 mg/kg/day or 2400 mg/day (whichever is less)
- **Concentrations**: 
  - Infant drops: 50 mg/1.25 mL
  - Children's suspension: 100 mg/5 mL

## Current Calculator Implementation Review

### Age Group: 2-6 months (Infants)

**Current Implementation:**
```javascript
const acetaMgCalculated = 12.5 * weightKg;
const ACETA_MAX_MG_INFANT = 160;
const acetaMg = Math.min(acetaMgCalculated, ACETA_MAX_MG_INFANT);
```
- Dose: 12.5 mg/kg
- Max single dose: 160 mg
- Interval: Every 4 hours

**Analysis:**
- ⚠️ **ISSUE**: 12.5 mg/kg is at the LOWER end of the 10-15 mg/kg range
- ✅ Max dose cap of 160 mg is appropriate for infants
- ✅ 4-hour interval is appropriate for this age group
- ✅ Correctly warns that ibuprofen is not recommended

**Recommendation:**
- Consider adjusting to 15 mg/kg to be consistent with standard pediatric dosing
- Current conservative approach (12.5 mg/kg) may be intentional for safety

### Age Group: 6+ months

**Current Implementation:**
```javascript
const acetaMgCalculated = 15 * weightKg;
const ACETA_MAX_SINGLE_DOSE_MG = 1000;
const ibuMgCalculated = 10 * weightKg;
const IBU_MAX_SINGLE_DOSE_MG = 800;
```
- Acetaminophen: 15 mg/kg, max 1000 mg, every 6 hours
- Ibuprofen: 10 mg/kg, max 800 mg, every 6 hours

**Analysis:**
- ✅ Acetaminophen 15 mg/kg is at the upper end of the 10-15 mg/kg range (appropriate)
- ✅ Ibuprofen 10 mg/kg is at the upper end of the 5-10 mg/kg range (appropriate)
- ⚠️ **ISSUE**: Maximum single dose caps may be too high for younger children:
  - 1000 mg acetaminophen is appropriate for adolescents 12+ years, but may be excessive for younger children (6 months - 11 years)
  - 800 mg ibuprofen is appropriate for adolescents 12+ years, but exceeds the 400 mg max for children under 12
- ⚠️ **ISSUE**: The "6+ months" age group is too broad - it should be split into:
  - 6 months - 2 years
  - 2-11 years  
  - 12+ years
- ✅ 6-hour interval is appropriate

**Recommendation:**
- Split the "6+ months" category into more specific age ranges with appropriate max doses:
  - 6 months - 11 years: Acetaminophen max 480 mg, Ibuprofen max 400 mg
  - 12+ years: Acetaminophen max 1000 mg, Ibuprofen max 800 mg

## Test Cases

### Test Case 1: 4-month-old, 6 kg infant
**Expected:**
- Acetaminophen: 75-90 mg (12.5-15 mg/kg)
- Volume: 2.3-2.8 mL of 160 mg/5 mL suspension
- Interval: Every 4-6 hours
- Ibuprofen: Not recommended

**Current Calculator:**
- Acetaminophen: 75 mg (12.5 mg/kg) = 2.3 mL
- Interval: Every 4 hours
- Ibuprofen: Warning displayed ✅

### Test Case 2: 10-month-old, 9 kg infant
**Expected:**
- Acetaminophen: 90-135 mg (10-15 mg/kg), capped at 160 mg
- Volume: 2.8-4.2 mL of 160 mg/5 mL suspension
- Ibuprofen: 45-90 mg (5-10 mg/kg)
- Volume: 1.1-2.3 mL of 50 mg/1.25 mL OR 2.3-4.5 mL of 100 mg/5 mL

**Current Calculator:**
- Acetaminophen: 135 mg (15 mg/kg) = 4.2 mL ✅
- Ibuprofen: 90 mg (10 mg/kg) = 2.3 mL (infant) OR 4.5 mL (children's) ✅

### Test Case 3: 5-year-old, 18 kg child
**Expected:**
- Acetaminophen: 180-270 mg (10-15 mg/kg), max 480 mg for this age
- Ibuprofen: 90-180 mg (5-10 mg/kg), max 400 mg for this age

**Current Calculator:**
- Acetaminophen: 270 mg (15 mg/kg), no issue since under 480 mg ✅
- Ibuprofen: 180 mg (10 mg/kg), no issue since under 400 mg ✅

### Test Case 4: 12-year-old, 50 kg adolescent
**Expected:**
- Acetaminophen: 500-750 mg (10-15 mg/kg), max 1000 mg
- Ibuprofen: 250-500 mg (5-10 mg/kg), max 800 mg

**Current Calculator:**
- Acetaminophen: 750 mg (15 mg/kg), under 1000 mg cap ✅
- Ibuprofen: 500 mg (10 mg/kg), under 800 mg cap ✅

### Test Case 5: Large adolescent, 80 kg
**Expected:**
- Acetaminophen: Weight-based would be 1200 mg, but should cap at 1000 mg
- Ibuprofen: Weight-based would be 800 mg (exactly at cap)

**Current Calculator:**
- Acetaminophen: Capped at 1000 mg ✅
- Ibuprofen: Capped at 800 mg ✅

## Key Findings

1. ✅ **Acetaminophen dosing for 6+ months** (15 mg/kg) is correct
2. ✅ **Ibuprofen dosing for 6+ months** (10 mg/kg) is correct
3. ⚠️ **Acetaminophen for 2-6 months** (12.5 mg/kg) is slightly conservative but safe
4. ⚠️ **Age group "6+ months" is too broad** - should be split to provide appropriate max dose caps
5. ⚠️ **Maximum single dose caps** may be too high for children under 12

## Recommendations

### Priority 1: Critical Safety Issue
The "6+ months" age group should be split into:
1. **6-11 months** or **6 months - 2 years**: More conservative max doses
2. **2-11 years**: Moderate max doses (Acetaminophen 480 mg, Ibuprofen 400 mg)
3. **12+ years**: Adult max doses (Acetaminophen 1000 mg, Ibuprofen 800 mg)

### Priority 2: Consistency
- Consider increasing infant acetaminophen dose from 12.5 mg/kg to 15 mg/kg to align with standard guidelines
- Update dosing intervals in UI to show "every 4-6 hours" for acetaminophen to reflect the guideline range

### Priority 3: Documentation
- Add source references in the code comments
- Display last reviewed date prominently
- Add disclaimer about consulting pediatrician for specific cases

## Conclusion

The calculator's core dosing formulas (mg/kg) are generally correct and align with AAP/FDA guidelines. However, the age groupings are too broad, which can lead to inappropriately high maximum dose caps being applied to younger children. The main recommendation is to split the "6+ months" category into more specific age ranges with appropriate maximum dose limits.
