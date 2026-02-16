# Meal Plan Generator Web Application

## Project Overview
A professional web application for generating personalized meal plans with PDF download functionality. Designed for Gayathri Dissanayake - Nutritionist & Dietitian.

---

## 🎯 Project Objectives
- Create personalized meal plans based on patient data
- Generate professional PDF documents with custom letterhead
- Provide mobile-friendly, responsive interface
- Follow Facebook color theme for modern, trustworthy appearance
- Enable easy data entry and instant PDF generation

---

## 🛠️ Technology Stack

### Frontend Technologies
- **HTML5** - Semantic markup and structure
- **CSS3** - Styling and animations
- **Tailwind CSS** - Utility-first CSS framework for rapid UI development
- **JavaScript (ES6+)** - Application logic and interactivity

### PDF Generation
- **jsPDF** - Client-side PDF generation
- **jsPDF-AutoTable** - Table generation for meal plans
- **html2canvas** (Optional) - For complex layouts

### Additional Libraries
- **Font Awesome** - Icons
- **Google Fonts** - Professional typography (Roboto, Inter, or Poppins)
- **SweetAlert2** - Beautiful alert/confirmation dialogs

---

## 🎨 Design Specifications

### Color Theme (Facebook Inspired)
```css
Primary Blue: #1877F2
Dark Blue: #166FE5
Light Blue: #E7F3FF
Background: #F0F2F5
White: #FFFFFF
Text Dark: #050505
Text Secondary: #65676B
Success Green: #42B72A
Border: #CCD0D5
```

### Typography
- **Primary Font:** Inter or Roboto
- **Headings:** Bold, 600-700 weight
- **Body:** Regular, 400 weight
- **Font Sizes:** Responsive (mobile-first)

### Layout Principles
- Mobile-first responsive design
- Minimum touch target: 44x44px
- Consistent spacing (8px grid system)
- Clean, professional aesthetics
- Clear visual hierarchy

---

## ✨ Must-Have Features

### 1. Patient Information Form
- **Personal Details**
  - Full Name
  - Age
  - Gender (Male/Female)
  - Date
  - Contact Information (Phone, Email)
  
- **Physical Measurements**
  - Height (cm)
  - Weight (kg)
  - BMI (auto-calculated)
  - Target Weight
  - IBW - Ideal Body Weight (auto-calculated)

### 2. Meal Plan Configuration
- **Plan Type Selection**
  - General Meal Plan (template-based)
  - Weekly Meal Plan (7-day structured)
  - Custom Meal Plan

- **Dietary Preferences**
  - Vegetarian options
  - Non-vegetarian options
  - Calorie targets (auto-suggest based on BMI)
  
- **Meal Time Slots**
  - Early Morning (Detox/Warm Water)
  - Bed Tea (optional)
  - Breakfast
  - Mid-Morning Snack
  - Lunch
  - Evening Snack
  - Before Dinner
  - Dinner

### 3. Meal Options Input
- Dynamic form for each meal time
- Multiple option selection (A, B, C, D)
- Portion size specifications
- Ingredient list with quantities
- Cooking method preferences

### 4. General Guidelines Section
- Things to Consider (checkboxes)
- Foods to Avoid (customizable list)
- Water intake recommendation
- Exercise recommendations
- Sleep and stress management tips

### 5. PDF Generation Features
- **Professional Letterhead**
  - Nutritionist name and credentials
  - Contact information placeholder
  - Professional logo area
  - Date and plan reference number

- **PDF Content**
  - Patient information summary
  - Complete meal plan with formatting
  - Nutritional guidelines
  - Professional footer
  - Page numbers

### 6. Form Validation
- Required field validation
- Format validation (email, phone)
- Range validation (height, weight)
- Real-time feedback
- Error messaging

### 7. Data Management
- Save form data to localStorage
- Load previously saved data
- Clear/reset form functionality
- Export/import meal plan templates

---

## 🌟 Nice-to-Have Features

### 1. Advanced Functionality
- **Template Library**
  - Pre-built meal plan templates
  - Quick-select common plans
  - Template customization

- **Meal Database**
  - Searchable food items
  - Nutritional information
  - Common Sri Lankan dishes
  - Portion size calculator

- **BMI Calculator Widget**
  - Visual BMI chart
  - Health category indicators
  - Weight loss timeline estimator

### 2. User Experience Enhancements
- **Progressive Form**
  - Multi-step wizard interface
  - Progress indicator
  - Previous/Next navigation
  - Save & continue later

- **Live Preview**
  - Real-time meal plan preview
  - PDF preview before download
  - Print-friendly view

- **Dark Mode**
  - Toggle light/dark theme
  - Comfortable viewing option
  - Save preference

### 3. Professional Features
- **Multiple PDF Formats**
  - Detailed version (full plan)
  - Summary version (quick reference)
  - Patient copy vs. Nutritionist copy

- **Branding Customization**
  - Upload custom logo
  - Custom color schemes
  - Personalized footer text

- **Email Integration**
  - Send PDF directly to patient email
  - Email template with instructions
  - Automated follow-up reminders

### 4. Analytics & Tracking
- **Patient History**
  - Previous meal plans
  - Weight progress tracking
  - BMI trend charts

- **Plan Statistics**
  - Total calories per day
  - Macronutrient breakdown
  - Meal timing analysis

### 5. Accessibility Features
- **ARIA Labels**
- **Keyboard Navigation**
- **Screen Reader Support**
- **High Contrast Mode**
- **Font Size Adjustments**

### 6. Additional Tools
- **Shopping List Generator**
  - Auto-generate from meal plan
  - Categorized by food groups
  - Printable/downloadable

- **Recipe Suggestions**
  - Cooking instructions
  - Healthy recipe alternatives
  - Video links

- **Hydration Tracker**
- **Exercise Logger**
- **Progress Photos Upload**

---

## 📱 Responsive Design Breakpoints

```css
/* Mobile First Approach */
Mobile: 320px - 767px
Tablet: 768px - 1023px
Desktop: 1024px - 1439px
Large Desktop: 1440px+
```

---

## 🗂️ File Structure

```
meal-plan-generator/
│
├── index.html                 # Main HTML file
├── README.md                  # This file
│
├── css/
│   ├── tailwind.min.css      # Tailwind CSS framework
│   └── custom.css            # Custom styles
│
├── js/
│   ├── app.js                # Main application logic
│   ├── formValidation.js     # Form validation functions
│   ├── calculations.js       # BMI and other calculations
│   ├── pdfGenerator.js       # PDF generation logic
│   └── dataManager.js        # localStorage management
│
├── assets/
│   ├── images/
│   │   ├── logo.png          # Nutritionist logo
│   │   └── letterhead.png    # PDF letterhead
│   │
│   └── fonts/                # Custom fonts (if needed)
│
├── data/
│   ├── mealTemplates.json    # Pre-defined meal plan templates
│   ├── foodDatabase.json     # Food items and nutritional info
│   └── guidelines.json       # Standard nutritional guidelines
│
└── docs/
    └── user-guide.pdf        # User documentation
```

---

## 🚀 Development Phases

### Phase 1: Foundation (Week 1)
- [x] Set up project structure
- [x] Implement basic HTML structure
- [x] Add Tailwind CSS
- [x] Create patient information form
- [x] Implement form validation
- [x] Add BMI auto-calculation

### Phase 2: Core Features (Week 2)
- [x] Develop meal plan form sections
- [x] Implement dynamic meal options
- [x] Add guidelines section
- [x] Integrate localStorage
- [x] Create responsive layout

### Phase 3: PDF Generation (Week 3)
- [x] Set up jsPDF library
- [x] Design PDF template
- [x] Create letterhead
- [x] Implement PDF generation logic
- [x] Add download functionality

### Phase 4: Enhancement (Week 4)
- [x] Add template library
- [x] Implement live preview
- [x] Create print view
- [x] Add dark mode
- [x] Optimize performance

### Phase 5: Testing & Deployment (Week 5)
- [x] Cross-browser testing
- [x] Mobile device testing
- [x] Accessibility testing
- [x] Performance optimization
- [x] Deploy to hosting

---

## 💻 Key JavaScript Functions

### 1. BMI Calculation
```javascript
function calculateBMI(weight, height) {
    const heightInMeters = height / 100;
    return (weight / (heightInMeters * heightInMeters)).toFixed(1);
}
```

### 2. Form Data Collection
```javascript
function collectFormData() {
    return {
        patientInfo: {...},
        measurements: {...},
        mealPlan: {...},
        guidelines: {...}
    };
}
```

### 3. PDF Generation
```javascript
function generatePDF(data) {
    const doc = new jsPDF();
    // Add letterhead
    // Add patient info
    // Add meal plan tables
    // Add guidelines
    doc.save(`MealPlan_${data.name}_${data.date}.pdf`);
}
```

### 4. Data Persistence
```javascript
function saveToLocalStorage(key, data) {
    localStorage.setItem(key, JSON.stringify(data));
}

function loadFromLocalStorage(key) {
    return JSON.parse(localStorage.getItem(key));
}
```

---

## 🎯 Form Sections Structure

### Section 1: Patient Information
- Name, Age, Gender, Date
- Contact: Phone, Email, Address

### Section 2: Physical Measurements
- Height, Weight
- BMI (auto-calculated, read-only)
- Target Weight, IBW

### Section 3: Meal Plan Type
- Radio buttons: General / Weekly / Custom
- Calorie target input
- Duration (days)

### Section 4: Daily Meal Schedule
For each meal time:
- Meal time name
- Option A, B, C, D (expandable sections)
- Ingredients with portions
- Cooking instructions

### Section 5: Nutritional Guidelines
- Water intake
- Coconut oil consumption
- Exercise recommendations
- Sleep and stress management
- Foods to avoid (checkboxes)

### Section 6: Professional Details
- Nutritionist name (pre-filled)
- Credentials
- Contact information
- License number (optional)
- Signature area

---

## 📋 Sample PDF Layout

```
┌─────────────────────────────────────────────┐
│  [LOGO]    Gayathri Dissanayake             │
│            Dietitian & Nutritionist         │
│            Contact: [Phone] | [Email]       │
├─────────────────────────────────────────────┤
│                                             │
│  PERSONALIZED MEAL PLAN                     │
│                                             │
│  Patient: [Name]        Date: [Date]        │
│  Age: [XX] | Gender: [X] | BMI: [XX.X]      │
│  Current Weight: [XX] kg | Target: [XX] kg  │
│                                             │
├─────────────────────────────────────────────┤
│                                             │
│  EARLY MORNING (6:00 - 7:00 AM)             │
│  • Detox water 1 glass                      │
│  • Warm water 1 glass                       │
│                                             │
│  BREAKFAST (8:00 - 9:00 AM)                 │
│  ┌──────────────────────────────────┐       │
│  │ Option A:                        │       │
│  │ • Rice - 1 cup                   │       │
│  │ • Dhal curry - 3 tbsp            │       │
│  │ • Chicken - 30g                  │       │
│  └──────────────────────────────────┘       │
│                                             │
│  [... more meals ...]                       │
│                                             │
├─────────────────────────────────────────────┤
│  IMPORTANT GUIDELINES                       │
│  • Water intake: 2-3 liters/day             │
│  • Exercise: 45 min daily                   │
│  • Avoid: Fried foods, processed snacks     │
├─────────────────────────────────────────────┤
│ Generated on: [Date]        Page 1 of 2     │
│ © Gayathri Dissanayake - All Rights Reserved│
└─────────────────────────────────────────────┘
```

---

## 🔒 Security & Privacy

### Data Protection
- No server-side storage (client-side only)
- Option to clear all data
- No third-party data sharing
- GDPR-compliant data handling

### Best Practices
- Input sanitization
- XSS prevention
- CSRF protection (if backend added)
- Secure PDF generation

---

## 🧪 Testing Checklist

### Functionality Testing
- [ ] Form validation works correctly
- [ ] BMI calculates accurately
- [ ] All meal options can be added/removed
- [ ] PDF generates with correct data
- [ ] PDF downloads successfully
- [ ] localStorage saves/loads data
- [ ] Form reset clears all fields

### Responsive Testing
- [ ] Mobile (320px - 767px)
- [ ] Tablet (768px - 1023px)
- [ ] Desktop (1024px+)
- [ ] Landscape/Portrait orientations

### Browser Compatibility
- [ ] Chrome (latest)
- [ ] Firefox (latest)
- [ ] Safari (latest)
- [ ] Edge (latest)
- [ ] Mobile browsers

### Accessibility Testing
- [ ] Keyboard navigation
- [ ] Screen reader compatibility
- [ ] Color contrast ratio
- [ ] Focus indicators
- [ ] ARIA labels

### Performance Testing
- [ ] Page load time < 3 seconds
- [ ] PDF generation < 5 seconds
- [ ] Smooth animations (60fps)
- [ ] No memory leaks

---

## 📦 Required CDN Links

```html
<!-- Tailwind CSS -->
<script src="https://cdn.tailwindcss.com"></script>

<!-- jsPDF -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<!-- jsPDF-AutoTable -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.31/jspdf.plugin.autotable.min.js"></script>

<!-- Font Awesome -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

<!-- Google Fonts -->
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">

<!-- SweetAlert2 -->
<script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
```

---

## 🎓 Learning Resources

### Tailwind CSS
- Official Docs: https://tailwindcss.com/docs
- Tutorial: https://www.youtube.com/tailwindlabs

### jsPDF
- Documentation: https://artskydj.github.io/jsPDF/docs/
- Examples: https://parall.ax/products/jspdf

### Responsive Design
- MDN Web Docs: https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Responsive_Design

---

## 🚀 Deployment Options

### Free Hosting Platforms
1. **GitHub Pages** (Recommended)
   - Free static hosting
   - Custom domain support
   - HTTPS by default

2. **Netlify**
   - Easy deployment
   - Continuous deployment
   - Form handling

3. **Vercel**
   - Fast deployment
   - Great performance
   - Analytics

4. **Firebase Hosting**
   - Google infrastructure
   - SSL certificate
   - CDN

---

## 📝 Usage Instructions

### For Developers
1. Clone/download the project
2. Open `index.html` in a browser
3. No build process required
4. Edit files as needed
5. Test locally before deployment

### For Nutritionist (End User)
1. Open the web application
2. Fill in patient information
3. Enter physical measurements
4. Select meal plan type
5. Customize meal options
6. Add guidelines and restrictions
7. Preview the meal plan
8. Click "Generate PDF"
9. Download and share with patient

---

## 🐛 Troubleshooting

### PDF Not Generating
- Check browser console for errors
- Ensure jsPDF library is loaded
- Verify all required fields are filled
- Try different browser

### Form Data Not Saving
- Check if localStorage is enabled
- Clear browser cache
- Check browser console

### Responsive Issues
- Clear cache and hard reload
- Check viewport meta tag
- Test on actual devices

---

## 🔄 Future Enhancements

### Version 2.0 Features
- [ ] Backend integration (optional)
- [ ] User authentication
- [ ] Cloud storage for meal plans
- [ ] Mobile app (React Native)
- [ ] WhatsApp integration
- [ ] Payment gateway (for premium features)
- [ ] Multi-language support (Sinhala, Tamil, English)
- [ ] AI-powered meal suggestions
- [ ] Nutritional analysis API integration
- [ ] Calendar integration
- [ ] Appointment booking system

---

## 👥 Contributors
- **Developer:** [Your Name]
- **Nutritionist:** Gayathri Dissanayake
- **UI/UX Designer:** [Designer Name]

---

## 📄 License
This project is proprietary software developed for Gayathri Dissanayake - Nutritionist & Dietitian.

---

## 📞 Support
For technical support or questions:
- Email: support@mealplangenerator.com
- Phone: [Contact Number]
- Website: [Website URL]

---

## ✅ Final Checklist Before Launch

- [ ] All features tested and working
- [ ] Responsive on all devices
- [ ] Cross-browser compatible
- [ ] Accessibility standards met
- [ ] Performance optimized
- [ ] SEO meta tags added
- [ ] Favicon added
- [ ] User documentation created
- [ ] Privacy policy added
- [ ] Terms of service added
- [ ] Analytics setup (Google Analytics)
- [ ] Backup and recovery plan
- [ ] Domain registered
- [ ] Hosting configured
- [ ] SSL certificate installed
- [ ] Launch announcement prepared

---

**Version:** 1.0.0  
**Last Updated:** February 2026  
**Status:** Ready for Development

---

*Built with ❤️ for better nutrition and healthier lives*
