# Technical Specification Document
## Meal Plan Generator Web Application

---

## 1. SYSTEM ARCHITECTURE

### 1.1 Client-Side Architecture
```
┌─────────────────────────────────────────┐
│         User Interface Layer            │
│  (HTML5 + Tailwind CSS + JavaScript)    │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│      Application Logic Layer            │
│   - Form Management                     │
│   - Data Validation                     │
│   - Calculations                        │
│   - State Management                    │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│         Data Layer                      │
│   - LocalStorage API                    │
│   - JSON Data Models                    │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│         Output Layer                    │
│   - PDF Generation (jsPDF)              │
│   - Download Management                 │
└─────────────────────────────────────────┘
```

### 1.2 Technology Choices Justification

**HTML5**
- Semantic markup for better SEO
- Native form validation
- Accessibility features
- Cross-browser compatibility

**Tailwind CSS**
- Rapid development
- Consistent design system
- Small bundle size (with purge)
- Mobile-first approach
- Easy customization

**Vanilla JavaScript**
- No framework overhead
- Fast performance
- Better control
- Easy to maintain
- No build tools required

**jsPDF**
- Client-side PDF generation
- No server required
- Privacy-friendly
- Customizable output
- Good documentation

---

## 2. DATA MODELS

### 2.1 Patient Information Model
```javascript
const PatientInfo = {
    personalDetails: {
        name: String,           // Required, max 100 chars
        age: Number,            // Required, 1-120
        gender: String,         // Required, 'Male' | 'Female'
        date: Date,             // Required, ISO format
        contactPhone: String,   // Optional, format: +94XXXXXXXXX
        contactEmail: String,   // Optional, email format
        address: String         // Optional, max 200 chars
    },
    physicalMeasurements: {
        height: Number,         // Required, cm, 100-250
        weight: Number,         // Required, kg, 30-300
        bmi: Number,            // Calculated, read-only
        targetWeight: Number,   // Required, kg, 30-300
        ibw: Number,            // Calculated, read-only
        bodyFat: Number         // Optional, %, 5-60
    },
    metadata: {
        planId: String,         // UUID
        createdAt: Date,
        updatedAt: Date,
        version: String         // e.g., "1.0.0"
    }
};
```

### 2.2 Meal Plan Model
```javascript
const MealPlan = {
    planConfig: {
        planType: String,       // 'general' | 'weekly' | 'custom'
        duration: Number,       // Days, 1-30
        calorieTarget: Number,  // kcal/day, 1000-5000
        startDate: Date
    },
    dailySchedule: {
        earlyMorning: MealSlot,
        bedTea: MealSlot,
        breakfast: MealSlot,
        midMorning: MealSlot,
        lunch: MealSlot,
        eveningSnack: MealSlot,
        beforeDinner: MealSlot,
        dinner: MealSlot
    },
    weeklyPlan: {
        monday: DailyMeals,
        tuesday: DailyMeals,
        wednesday: DailyMeals,
        thursday: DailyMeals,
        friday: DailyMeals,
        saturday: DailyMeals,
        sunday: DailyMeals
    }
};
```

### 2.3 Meal Slot Model
```javascript
const MealSlot = {
    timeSlot: String,           // e.g., "8:00 - 9:00 AM"
    options: [
        {
            optionLabel: String, // 'A', 'B', 'C', 'D'
            items: [
                {
                    food: String,
                    quantity: String,
                    unit: String,
                    cookingMethod: String
                }
            ],
            notes: String
        }
    ],
    selected: String            // Currently selected option
};
```

### 2.4 Guidelines Model
```javascript
const Guidelines = {
    waterIntake: String,        // e.g., "2-3 liters/day"
    coconutOil: String,         // e.g., "3 tsp/day"
    exercise: {
        type: String,
        duration: String,
        frequency: String,
        intensity: String
    },
    thingsToConsider: [String],
    foodsToAvoid: [String],
    sleepRecommendation: String,
    stressManagement: String,
    specialInstructions: String
};
```

### 2.5 Nutritionist Profile Model
```javascript
const NutritionistProfile = {
    name: String,               // "Gayathri Dissanayake"
    credentials: String,        // "Dietitian & Nutritionist"
    contactPhone: String,
    contactEmail: String,
    address: String,
    licenseNumber: String,
    logo: String,               // Base64 or URL
    signature: String           // Base64 or URL
};
```

---

## 3. FORM VALIDATION RULES

### 3.1 Patient Information Validation
```javascript
const validationRules = {
    name: {
        required: true,
        minLength: 2,
        maxLength: 100,
        pattern: /^[a-zA-Z\s.]+$/,
        message: "Please enter a valid name (letters only)"
    },
    age: {
        required: true,
        min: 1,
        max: 120,
        type: 'number',
        message: "Age must be between 1 and 120"
    },
    gender: {
        required: true,
        options: ['Male', 'Female'],
        message: "Please select gender"
    },
    email: {
        required: false,
        pattern: /^[^\s@]+@[^\s@]+\.[^\s@]+$/,
        message: "Please enter a valid email address"
    },
    phone: {
        required: false,
        pattern: /^[+]?[0-9]{10,15}$/,
        message: "Please enter a valid phone number"
    },
    height: {
        required: true,
        min: 100,
        max: 250,
        type: 'number',
        message: "Height must be between 100-250 cm"
    },
    weight: {
        required: true,
        min: 30,
        max: 300,
        type: 'number',
        message: "Weight must be between 30-300 kg"
    },
    targetWeight: {
        required: true,
        min: 30,
        max: 300,
        type: 'number',
        message: "Target weight must be between 30-300 kg"
    }
};
```

### 3.2 Real-time Validation Implementation
```javascript
function validateField(fieldName, value) {
    const rule = validationRules[fieldName];
    const errors = [];
    
    if (rule.required && !value) {
        errors.push(`${fieldName} is required`);
    }
    
    if (rule.pattern && !rule.pattern.test(value)) {
        errors.push(rule.message);
    }
    
    if (rule.min && value < rule.min) {
        errors.push(rule.message);
    }
    
    if (rule.max && value > rule.max) {
        errors.push(rule.message);
    }
    
    return {
        isValid: errors.length === 0,
        errors: errors
    };
}
```

---

## 4. CALCULATION FORMULAS

### 4.1 BMI Calculation
```javascript
/**
 * Calculate Body Mass Index
 * @param {number} weight - Weight in kilograms
 * @param {number} height - Height in centimeters
 * @returns {number} BMI value with 1 decimal place
 */
function calculateBMI(weight, height) {
    const heightInMeters = height / 100;
    const bmi = weight / (heightInMeters * heightInMeters);
    return Math.round(bmi * 10) / 10;
}

/**
 * Get BMI category
 * @param {number} bmi - BMI value
 * @returns {object} Category and color code
 */
function getBMICategory(bmi) {
    if (bmi < 18.5) {
        return { category: 'Underweight', color: '#3B82F6' };
    } else if (bmi >= 18.5 && bmi < 25) {
        return { category: 'Normal', color: '#10B981' };
    } else if (bmi >= 25 && bmi < 30) {
        return { category: 'Overweight', color: '#F59E0B' };
    } else {
        return { category: 'Obese', color: '#EF4444' };
    }
}
```

### 4.2 Ideal Body Weight (IBW) Calculation
```javascript
/**
 * Calculate Ideal Body Weight using Devine formula
 * @param {number} height - Height in centimeters
 * @param {string} gender - 'Male' or 'Female'
 * @returns {number} IBW in kilograms
 */
function calculateIBW(height, gender) {
    const heightInInches = height / 2.54;
    let ibw;
    
    if (gender === 'Male') {
        // IBW (kg) = 50 + 2.3 × (height in inches - 60)
        ibw = 50 + 2.3 * (heightInInches - 60);
    } else {
        // IBW (kg) = 45.5 + 2.3 × (height in inches - 60)
        ibw = 45.5 + 2.3 * (heightInInches - 60);
    }
    
    return Math.round(ibw * 10) / 10;
}
```

### 4.3 Calorie Requirement Estimation
```javascript
/**
 * Calculate daily calorie requirement using Mifflin-St Jeor Equation
 * @param {object} params - Patient parameters
 * @returns {number} Daily calorie requirement
 */
function calculateCalorieRequirement({ weight, height, age, gender, activityLevel }) {
    let bmr;
    
    if (gender === 'Male') {
        bmr = (10 * weight) + (6.25 * height) - (5 * age) + 5;
    } else {
        bmr = (10 * weight) + (6.25 * height) - (5 * age) - 161;
    }
    
    const activityMultipliers = {
        sedentary: 1.2,      // Little or no exercise
        light: 1.375,        // Exercise 1-3 days/week
        moderate: 1.55,      // Exercise 3-5 days/week
        active: 1.725,       // Exercise 6-7 days/week
        veryActive: 1.9      // Very hard exercise
    };
    
    return Math.round(bmr * activityMultipliers[activityLevel]);
}
```

### 4.4 Weight Loss Timeline
```javascript
/**
 * Calculate estimated time to reach target weight
 * @param {number} currentWeight - Current weight in kg
 * @param {number} targetWeight - Target weight in kg
 * @param {number} weeklyLoss - Weekly weight loss in kg (default: 0.5)
 * @returns {object} Timeline details
 */
function calculateWeightLossTimeline(currentWeight, targetWeight, weeklyLoss = 0.5) {
    const totalLoss = currentWeight - targetWeight;
    const weeks = Math.ceil(totalLoss / weeklyLoss);
    const months = Math.ceil(weeks / 4);
    
    return {
        totalWeightLoss: totalLoss,
        estimatedWeeks: weeks,
        estimatedMonths: months,
        weeklyTarget: weeklyLoss,
        isHealthy: weeklyLoss <= 1 // Healthy loss is 0.5-1 kg/week
    };
}
```

---

## 5. PDF GENERATION SPECIFICATIONS

### 5.1 PDF Layout Configuration
```javascript
const pdfConfig = {
    format: 'a4',
    orientation: 'portrait',
    unit: 'mm',
    margins: {
        top: 20,
        right: 15,
        bottom: 20,
        left: 15
    },
    fonts: {
        primary: 'helvetica',
        headings: 'helvetica-bold',
        body: 'helvetica'
    },
    colors: {
        primary: '#1877F2',
        secondary: '#65676B',
        accent: '#42B72A',
        border: '#CCD0D5',
        background: '#F0F2F5'
    },
    fontSize: {
        title: 18,
        heading: 14,
        subheading: 12,
        body: 10,
        small: 8
    }
};
```

### 5.2 PDF Structure
```javascript
function generatePDF(data) {
    const doc = new jsPDF(pdfConfig);
    
    // Page 1: Header & Patient Info
    addLetterhead(doc, data.nutritionist);
    addPatientInfo(doc, data.patient);
    addPhysicalMeasurements(doc, data.measurements);
    
    // Page 2-3: Meal Plan
    addMealPlanTable(doc, data.mealPlan);
    
    // Page 4: Guidelines
    addGuidelines(doc, data.guidelines);
    
    // Footer on all pages
    addFooter(doc, data.nutritionist);
    
    // Save PDF
    const fileName = `MealPlan_${data.patient.name}_${data.date}.pdf`;
    doc.save(fileName);
}
```

### 5.3 Letterhead Design
```javascript
function addLetterhead(doc, nutritionist) {
    // Logo (if available)
    if (nutritionist.logo) {
        doc.addImage(nutritionist.logo, 'PNG', 15, 10, 30, 30);
    }
    
    // Nutritionist Name
    doc.setFontSize(16);
    doc.setTextColor(5, 5, 5);
    doc.setFont('helvetica', 'bold');
    doc.text(nutritionist.name, 50, 20);
    
    // Credentials
    doc.setFontSize(11);
    doc.setFont('helvetica', 'normal');
    doc.setTextColor(101, 103, 107);
    doc.text(nutritionist.credentials, 50, 26);
    
    // Contact Information
    doc.setFontSize(9);
    doc.text(`Phone: ${nutritionist.phone} | Email: ${nutritionist.email}`, 50, 32);
    
    // Horizontal line
    doc.setDrawColor(204, 208, 213);
    doc.setLineWidth(0.5);
    doc.line(15, 45, 195, 45);
}
```

### 5.4 Table Styling
```javascript
const tableStyles = {
    headStyles: {
        fillColor: [24, 119, 242],  // Facebook blue
        textColor: [255, 255, 255],
        fontSize: 11,
        fontStyle: 'bold',
        halign: 'center'
    },
    bodyStyles: {
        fontSize: 9,
        cellPadding: 3,
        textColor: [5, 5, 5],
        lineColor: [204, 208, 213],
        lineWidth: 0.1
    },
    alternateRowStyles: {
        fillColor: [240, 242, 245]
    },
    columnStyles: {
        0: { cellWidth: 40, fontStyle: 'bold' },
        1: { cellWidth: 140 }
    }
};
```

---

## 6. RESPONSIVE DESIGN IMPLEMENTATION

### 6.1 Tailwind Breakpoints
```javascript
// Mobile First Approach
const breakpoints = {
    sm: '640px',   // Small devices
    md: '768px',   // Medium devices (tablets)
    lg: '1024px',  // Large devices (desktops)
    xl: '1280px',  // Extra large devices
    '2xl': '1536px' // 2X Extra large devices
};
```

### 6.2 Responsive Form Layout
```html
<!-- Example responsive grid -->
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
    <!-- Form fields adapt based on screen size -->
</div>
```

### 6.3 Mobile Touch Optimization
```css
/* Minimum touch target size: 44x44px */
.btn {
    min-height: 44px;
    min-width: 44px;
    padding: 12px 24px;
}

/* Larger inputs on mobile */
@media (max-width: 768px) {
    input, select, textarea {
        font-size: 16px; /* Prevents zoom on iOS */
        padding: 12px;
    }
}
```

---

## 7. LOCALSTORAGE MANAGEMENT

### 7.1 Storage Structure
```javascript
const storageKeys = {
    PATIENT_DATA: 'mealplan_patient_data',
    MEAL_PLAN: 'mealplan_meal_data',
    GUIDELINES: 'mealplan_guidelines',
    TEMPLATES: 'mealplan_templates',
    SETTINGS: 'mealplan_settings',
    HISTORY: 'mealplan_history'
};
```

### 7.2 Save/Load Functions
```javascript
class StorageManager {
    static save(key, data) {
        try {
            const serialized = JSON.stringify(data);
            localStorage.setItem(key, serialized);
            return true;
        } catch (error) {
            console.error('Storage error:', error);
            return false;
        }
    }
    
    static load(key) {
        try {
            const serialized = localStorage.getItem(key);
            return serialized ? JSON.parse(serialized) : null;
        } catch (error) {
            console.error('Load error:', error);
            return null;
        }
    }
    
    static remove(key) {
        localStorage.removeItem(key);
    }
    
    static clear() {
        Object.values(storageKeys).forEach(key => {
            localStorage.removeItem(key);
        });
    }
    
    static exportData() {
        const allData = {};
        Object.entries(storageKeys).forEach(([name, key]) => {
            allData[name] = this.load(key);
        });
        return allData;
    }
    
    static importData(data) {
        Object.entries(data).forEach(([name, value]) => {
            const key = storageKeys[name];
            if (key) {
                this.save(key, value);
            }
        });
    }
}
```

---

## 8. PERFORMANCE OPTIMIZATION

### 8.1 Code Splitting Strategy
```javascript
// Lazy load PDF library only when needed
function loadPDFLibrary() {
    return new Promise((resolve, reject) => {
        if (window.jsPDF) {
            resolve(window.jsPDF);
        } else {
            const script = document.createElement('script');
            script.src = 'path/to/jspdf.js';
            script.onload = () => resolve(window.jsPDF);
            script.onerror = reject;
            document.head.appendChild(script);
        }
    });
}
```

### 8.2 Debouncing for Real-time Validation
```javascript
function debounce(func, delay = 300) {
    let timeoutId;
    return function (...args) {
        clearTimeout(timeoutId);
        timeoutId = setTimeout(() => func.apply(this, args), delay);
    };
}

// Usage
const validateEmail = debounce((email) => {
    // Validation logic
}, 500);
```

### 8.3 Image Optimization
```javascript
// Compress and resize logo/images before storing
async function compressImage(file, maxWidth = 800, quality = 0.8) {
    return new Promise((resolve) => {
        const reader = new FileReader();
        reader.onload = (e) => {
            const img = new Image();
            img.onload = () => {
                const canvas = document.createElement('canvas');
                let width = img.width;
                let height = img.height;
                
                if (width > maxWidth) {
                    height *= maxWidth / width;
                    width = maxWidth;
                }
                
                canvas.width = width;
                canvas.height = height;
                
                const ctx = canvas.getContext('2d');
                ctx.drawImage(img, 0, 0, width, height);
                
                resolve(canvas.toDataURL('image/jpeg', quality));
            };
            img.src = e.target.result;
        };
        reader.readAsDataURL(file);
    });
}
```

---

## 9. ERROR HANDLING

### 9.1 Global Error Handler
```javascript
class ErrorHandler {
    static handle(error, context = '') {
        console.error(`Error in ${context}:`, error);
        
        // User-friendly message
        let message = 'An error occurred. Please try again.';
        
        if (error.name === 'ValidationError') {
            message = error.message;
        } else if (error.name === 'StorageError') {
            message = 'Unable to save data. Please check storage availability.';
        }
        
        this.showError(message);
    }
    
    static showError(message) {
        Swal.fire({
            icon: 'error',
            title: 'Oops...',
            text: message,
            confirmButtonColor: '#1877F2'
        });
    }
    
    static showSuccess(message) {
        Swal.fire({
            icon: 'success',
            title: 'Success!',
            text: message,
            confirmButtonColor: '#42B72A',
            timer: 3000
        });
    }
}
```

---

## 10. ACCESSIBILITY FEATURES

### 10.1 ARIA Labels
```html
<!-- Form field with ARIA -->
<div class="form-group">
    <label for="patientName" class="form-label">
        Patient Name
        <span class="text-red-500" aria-label="required">*</span>
    </label>
    <input 
        type="text" 
        id="patientName"
        name="patientName"
        class="form-input"
        aria-required="true"
        aria-describedby="nameHelp"
        aria-invalid="false"
    />
    <span id="nameHelp" class="help-text">
        Enter the patient's full name
    </span>
    <span class="error-message" role="alert" aria-live="polite"></span>
</div>
```

### 10.2 Keyboard Navigation
```javascript
// Trap focus in modal
function trapFocus(element) {
    const focusableElements = element.querySelectorAll(
        'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
    );
    
    const firstElement = focusableElements[0];
    const lastElement = focusableElements[focusableElements.length - 1];
    
    element.addEventListener('keydown', (e) => {
        if (e.key === 'Tab') {
            if (e.shiftKey && document.activeElement === firstElement) {
                e.preventDefault();
                lastElement.focus();
            } else if (!e.shiftKey && document.activeElement === lastElement) {
                e.preventDefault();
                firstElement.focus();
            }
        }
    });
}
```

---

## 11. SECURITY BEST PRACTICES

### 11.1 Input Sanitization
```javascript
function sanitizeInput(input) {
    const map = {
        '&': '&amp;',
        '<': '&lt;',
        '>': '&gt;',
        '"': '&quot;',
        "'": '&#x27;',
        "/": '&#x2F;',
    };
    return String(input).replace(/[&<>"'/]/g, (s) => map[s]);
}
```

### 11.2 XSS Prevention
```javascript
function escapeHTML(str) {
    const div = document.createElement('div');
    div.textContent = str;
    return div.innerHTML;
}

// Use when inserting user content
element.textContent = userInput; // Safe
// element.innerHTML = userInput; // Unsafe!
```

---

## 12. TESTING STRATEGY

### 12.1 Unit Tests (Example)
```javascript
// Test BMI calculation
function testBMICalculation() {
    const testCases = [
        { weight: 70, height: 175, expected: 22.9 },
        { weight: 82, height: 151, expected: 35.9 },
        { weight: 116, height: 172, expected: 39.2 }
    ];
    
    testCases.forEach(test => {
        const result = calculateBMI(test.weight, test.height);
        console.assert(
            result === test.expected,
            `BMI calculation failed: expected ${test.expected}, got ${result}`
        );
    });
}
```

### 12.2 Integration Tests
```javascript
// Test form submission workflow
async function testFormSubmission() {
    // 1. Fill form
    fillForm(mockPatientData);
    
    // 2. Validate
    const isValid = validateForm();
    console.assert(isValid, 'Form validation failed');
    
    // 3. Save to storage
    const saved = saveToLocalStorage();
    console.assert(saved, 'Failed to save data');
    
    // 4. Generate PDF
    const pdf = await generatePDF();
    console.assert(pdf !== null, 'PDF generation failed');
}
```

---

## 13. DEPLOYMENT CHECKLIST

### 13.1 Pre-deployment
- [ ] All features tested
- [ ] Code minified
- [ ] Images optimized
- [ ] Meta tags added
- [ ] Favicon configured
- [ ] Analytics setup
- [ ] Error tracking configured
- [ ] Performance benchmarked

### 13.2 Deployment Steps
1. Build production files
2. Test on staging environment
3. Run lighthouse audit
4. Deploy to hosting
5. Configure SSL
6. Test on live URL
7. Submit to search engines
8. Monitor for errors

---

## 14. MAINTENANCE PLAN

### 14.1 Regular Updates
- Weekly: Security patches
- Monthly: Feature updates
- Quarterly: Major version updates
- Yearly: Technology stack review

### 14.2 Backup Strategy
- Daily: LocalStorage export
- Weekly: Code repository backup
- Monthly: Full system backup

---

**Document Version:** 1.0  
**Last Updated:** February 2026  
**Status:** Ready for Implementation
