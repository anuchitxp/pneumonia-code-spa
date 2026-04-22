### **Pneumonia Coder Antigravity Prompt**

**Role:** Senior Full-Stack Developer & Clinical Informatics Specialist

**Objective:**
Create a single-file HTML application (`index.html`) named **"Pneumonia Coder Demystified"**. This tool serves as a clinical decision support and documentation guide for physicians to select accurate, evidence-based ICD-10 codes for pneumonia, aiming to maximize adjRW while ensuring audit compliance.

**Technical Constraints & Required Tech Stack:**
1.  **Single File:** All HTML, CSS, and JavaScript must be contained within one `.html` file.
2.  **No Dependencies:** Use only Vanilla HTML, Vanilla CSS, and Vanilla JavaScript. No external libraries (jQuery, Bootstrap, React, etc.) are allowed. No CDNs.
3.  **Cross-Browser Compatibility:** Must run in modern browsers (Chrome, Firefox, Safari, Edge).
4.  **Local Storage:** Use browser local storage to persist the user's checklist selections.

**UI/UX Guidelines:**
1.  **Design Philosophy:** Minimalist, clean, focused on speed and efficiency. Consider a dark theme suitable for Tiling Window Managers like Niri WM. Use CSS Grid/Flexbox for layout.
2.  **Layout:**
    * Header: Title "Pneumonia Coder Demystified" and subtitle "Clinical Documentation Wizard".
    * Main Content Area: Organized into four "Tiers" of coding optimization as sections.
    * Summary Box: A sticky section at the bottom to display the generated summary text and total adjRW.
3.  **Checklist Interface:** Use checkboxes as the primary method for user input. Each checklist item should represent a clinical evidence point or a specific ICD-10 code.
4.  **adjRW Tags:** Visual tags/labels near relevant items (e.g., `+0.5 adjRW`, `High RW`) to guide users toward higher-value coding opportunities.
5.  **Dynamic RW Calculation:** The total estimated adjRW should be calculated and displayed in the Summary Box in real-time as the user checks/unchecks items.

**Functional Logic (Crucial):**

**A. Tiered Coding Hierarchy & Mutual Exclusion:**
* Implement the following logic to enforce ICD-10Specificity and prevent dual coding of specific vs. unspecified codes:
    * **Tier 1: Pathogen-Specific Pneumonia** (J13-J16).
    * **Tier 2: Anatomical/Radiological Pneumonia** (J18.0, J18.1, J18.8).
    * **Interaction Logic:** If *any* checkbox in Tier 1 is selected, *all* checkboxes in Tier 2 must be automatically unchecked, disabled, and made visually distinct (e.g., lower opacity, greyed out). If Tier 1 becomes empty, Tier 2 must be re-enabled.

**B. Data Structure (Pre-load this JSON-like data):**
Create an internal JavaScript data structure (e.g., a constant array of objects) containing all relevant pneumonia codes. Each object must include:
* `id`: Unique string for the checklist item.
* `tier`: Number (1, 2, 3, or 4).
* `category`: String (e.g., "Pathogen", "Radiology").
* `code`: String (ICD-10 code, e.g., "J15.1").
* `description`: String (e.g., "Pseudomonas pneumonia").
* `evidenceNeeded`: String (Description of clinical evidence, e.g., "Positive Sputum Culture for Pseudomonas").
* `adjRWValue`: Number (Estimated adjRW contribution).
* `isModifier`: Boolean (True for Y-codes like Y95 or complications like A41.9).

**C. Summary Generation:**
* Based on the user's selections, dynamically generate a formatted, plain text "Discharge Summary" suitable for copy-pasting into a Hospital Information System (HIS). The summary must include:
    * Clear ICD-10 code and description for the Primary Diagnosis.
    * Clear ICD-10 codes and descriptions for all Secondary Diagnoses/Modifiers.
    * A dedicated section listing the "Required Clinical Documentation/Evidence" based on the selected codes.

**D. Copy & Reset Buttons:**
* **"Copy to Clipboard" Button:** Copies the generated summary text to the clipboard. Show a temporary "Copied!" notification.
* **"Reset Form" Button:** Clears all checkbox selections, resets disables, and clears the local storage.

**Example Tier Structure to Implement:**

**Tier 1: Pathogen-Specific (Highest adjRW)**
* [J13] Streptococcus pneumoniae (Positive Culture)
* [J15.0] Klebsiella pneumoniae (Positive Culture)
* [J15.1] Pseudomonas pneumonia (Positive Culture)

**Tier 2: Anatomical/Radiological (Use only if Tier 1 is blank)**
* [J18.1] Lobar Pneumonia (Consolidation on CXR)
* [J18.0] Bronchopneumonia (Patchy Infiltration on CXR)

**Tier 3: Clinical Context & Specifics**
* [J69.0] Aspiration Pneumonia (Witnessed Choking, Dysphagia, or Bedridden)

**Tier 4: Complications & Modifiers**
* [Y95] Hospital-acquired Condition (HAP/VAP) (Onset > 48hr after admission)
* [A41.9] Sepsis, unspecified (SOFA Score >= 2)
* [R57.2] Septic shock

---
