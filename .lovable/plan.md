

# Plan: Auto-fill DOCX Template on Proceed & Navigate Back to Results

## What Changes

### 1. Copy DOCX template into project
- Copy `Funding_Application_Template_v2.docx` to `public/Funding_Application_Template_v2.docx` so it's accessible at runtime.

### 2. Install `docx` npm package
- Add `docx` (and `file-saver`) to generate filled DOCX files client-side.

### 3. Create `src/services/documentGenerator.ts`
- New utility that builds a DOCX matching the template structure, replacing placeholders (`{{full_name}}`, `{{email}}`, `{{funding_purpose}}`, etc.) with user data from `localStorage` (`fn-user-data`) and the selected foundation's details.
- Uses the `docx` library to create the document programmatically (tables, application letter, declaration section).
- Triggers browser download via `file-saver`.

### 4. Update `OrganizationDetail.tsx`
- On "Proceed" / "Download Application Document" click: call the document generator with user data + foundation info, then navigate to `/download-confirmation` passing the foundation ID and questionnaire state via route state.

### 5. Update `Results.tsx` — pass questionnaire data via route state
- The Results page already receives `QuestionnaireData` via `location.state`. Pass this data through to OrganizationDetail when navigating, so it's available for document generation.

### 6. Update `DownloadConfirmation.tsx`
- Change "Back to Home" button to navigate back to `/results` with the original questionnaire data in route state, so the user returns to their list of matched organizations and can apply to another one.

## Technical Details
- User data source: `localStorage.getItem("fn-user-data")` (set during questionnaire completion)
- Foundation data: passed via route state or looked up by ID from `mockFoundations`
- Template fields mapped: `full_name`, `email`, `phone`, `address`, `applicant_type`, `funding_purpose`, `amount`, `deadline`, `duration`, `organization_name`, `organization_category`, `user_situation`, `benefit_description`
- The DOCX is generated entirely client-side — no backend needed

