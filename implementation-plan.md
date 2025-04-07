# Full Implementation Plan for Unified Calculator with Contract Template

Here's a comprehensive plan broken down into small, manageable steps that could be implemented in a new chat with access to the current codebase:

## Phase 1: Basic Structure and Setup

### Step 1: Create the basic HTML structure
- Create a new file `contract-calculator.html`
- Include basic HTML structure with sections for:
  - Calculator interface
  - Contract template
- Add p5.js library reference
- Include password protection code

### Step 2: Set up CSS styling
- Create styles for calculator section
- Create styles for contract section
- Add print-specific CSS for PDF export
- Ensure responsive design

### Step 3: Implement basic JavaScript functionality
- Set up event listeners
- Create basic calculator functions
- Add contract update functions
- Implement password protection

## Phase 2: Calculator Implementation

### Step 4: Implement hours and priority visualization
- Create p5.js canvas for hours visualization
- Implement priority breakdown (high, normal, as-available)
- Add interactive elements for adjusting hours

### Step 5: Implement compensation visualization
- Create p5.js canvas for compensation breakdown
- Visualize direct cash, deferred cash, and equity
- Add interactive elements for adjusting compensation

### Step 6: Implement updated equity calculation
- Base equity: 0.25% per month at 10 hrs/week
- Additional equity for deferred cash: 0.25% per $2,000 deferred
- Additional equity for hours beyond 10: 0.25% per additional 10 hrs/week
- Calculate projected equity after 3 months

### Step 7: Add configurable parameters
- Add company valuation input (default: $1.5M)
- Add sliders for adjustable equity parameters:
  - Base equity percentage (default: 0.25%, range: 0.1% - 0.5%)
  - Hours-based equity increment (default: 0.25% per 10 hrs, range: 0.1% - 0.5%)
  - Deferred cash equity increment (default: 0.25% per $2k, range: 0.1% - 0.5%)

### Step 8: Implement results display
- Show detailed breakdown of hours by priority
- Display compensation details
- Show monthly equity and projected equity
- Calculate equity value based on company valuation

## Phase 3: Contract Template Integration

### Step 9: Create dynamic contract template
- Use template.md as the basis
- Create HTML structure for contract
- Add placeholders for dynamic content

### Step 10: Link calculator to contract
- Update contract fields based on calculator inputs
- Implement two-way binding for editable fields
- Ensure non-editable fields remain protected

### Step 11: Implement editable fields
- Make certain fields editable by the client:
  - Company name
  - Effective date
  - Other negotiable terms

### Step 12: Implement signature functionality
- Create canvas-based signature input
- Convert signature to data URL
- Store signature in localStorage
- Display signature in contract

## Phase 4: Export and Finalization

### Step 13: Implement PDF export
- Add "Print to PDF" button
- Configure print-specific CSS
- Ensure proper formatting for printing

### Step 14: Add data persistence
- Save contract state to localStorage
- Generate unique URL with parameters
- Allow loading saved state

### Step 15: Add final touches
- Add instructions and help text
- Implement error handling
- Add validation for required fields
- Ensure accessibility

## Phase 5: Testing and Deployment

### Step 16: Test thoroughly
- Test with different scenarios
- Verify calculations
- Test PDF export
- Test on different browsers

### Step 17: Deploy to GitHub Pages
- Commit changes to repository
- Push to GitHub
- Verify deployment

## Detailed Implementation Notes

### Updated Equity Calculation
```javascript
function calculateMonthlyEquity(hours, deferredCash, baseEquityRate, hoursEquityRate, deferredEquityRate) {
  // Base equity for minimum commitment
  let equity = baseEquityRate;
  
  // Additional equity for hours beyond minimum
  equity += Math.max(0, hours - 10) / 10 * hoursEquityRate;
  
  // Additional equity for deferred cash
  equity += (deferredCash / 2000) * deferredEquityRate;
  
  return equity;
}

function calculateEquityValue(equityPercent, valuation) {
  return (equityPercent / 100) * valuation;
}
```

### Contract Template Structure
The contract will follow the structure in template.md, with an Addendum section that includes:

1. **Responsibilities**
   - Hours commitment with priority breakdown
   - Description of services

2. **Compensation**
   - Direct cash amount
   - Deferred cash terms
   - Equity terms with vesting schedule
   - Company valuation assumption

3. **Signatures**
   - Client signature field (canvas-based)
   - Date fields
   - Consultant signature (pre-filled)

### Print-to-PDF Implementation
```javascript
function printToPDF() {
  // Hide calculator section
  document.getElementById('calculator-section').style.display = 'none';
  
  // Show only contract section
  document.getElementById('contract-section').style.display = 'block';
  
  // Trigger browser print
  window.print();
  
  // Restore original display after printing
  setTimeout(function() {
    document.getElementById('calculator-section').style.display = 'block';
    document.getElementById('contract-section').style.display = 'block';
  }, 1000);
}
```

### Signature Implementation
```javascript
// Initialize signature canvas
function initSignatureCanvas() {
  const canvas = document.getElementById('signature-canvas');
  const ctx = canvas.getContext('2d');
  let isDrawing = false;
  
  canvas.addEventListener('mousedown', startDrawing);
  canvas.addEventListener('mousemove', draw);
  canvas.addEventListener('mouseup', stopDrawing);
  canvas.addEventListener('mouseout', stopDrawing);
  
  function startDrawing(e) {
    isDrawing = true;
    draw(e);
  }
  
  function draw(e) {
    if (!isDrawing) return;
    
    ctx.lineWidth = 2;
    ctx.lineCap = 'round';
    ctx.strokeStyle = '#000';
    
    ctx.lineTo(e.clientX - canvas.offsetLeft, e.clientY - canvas.offsetTop);
    ctx.stroke();
    ctx.beginPath();
    ctx.moveTo(e.clientX - canvas.offsetLeft, e.clientY - canvas.offsetTop);
  }
  
  function stopDrawing() {
    isDrawing = false;
    ctx.beginPath();
    
    // Save signature to localStorage
    localStorage.setItem('signature', canvas.toDataURL());
    
    // Update signature in contract
    document.getElementById('signature-image').src = canvas.toDataURL();
  }
}
```

This comprehensive plan outlines all the steps needed to implement the unified calculator with contract template functionality. Each step is designed to be manageable and can be implemented incrementally.
