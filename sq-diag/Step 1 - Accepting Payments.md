# Ghost Theme Payment Button Integration - Sequence Diagram

```mermaid
sequenceDiagram
    participant User as User/Admin
    participant Ghost as Ghost Admin Panel
    participant FS as File System
    participant Editor as Text Editor
    participant Website as Ghost Website
    participant Razorpay as Razorpay Service

    Note over User, Razorpay: Step 1: Accessing Ghost Theme Files
    User->>Ghost: Login to Ghost Admin Panel (/ghost)
    Ghost->>User: Display admin dashboard
    User->>Ghost: Navigate to Settings > Design
    Ghost->>User: Show Design section with themes
    User->>Ghost: Click "..." on active theme > Download
    Ghost->>FS: Generate theme .zip file
    FS->>User: Download theme .zip file

    Note over User, Razorpay: Step 2: Unzipping and Editing Theme Files
    User->>FS: Extract/unzip theme file
    FS->>User: Create unzipped theme folder
    User->>FS: Open theme folder structure
    FS->>User: Display files (default.hbs, partials/, etc.)
    User->>Editor: Open .hbs file (e.g., default.hbs)
    Editor->>User: Display file contents for editing

    Note over User, Razorpay: Step 3: Inserting Payment Button Code
    User->>Editor: Locate insertion position (before </body> or in footer)
    User->>Editor: Paste Razorpay payment button code
    Note right of Editor: Insert HTML form with Razorpay script and payment button ID
    Editor->>FS: Save modified .hbs file

    Note over User, Razorpay: Step 4: Re-uploading Modified Theme
    User->>FS: Select all theme files and folders
    User->>FS: Create new .zip file from modified theme
    FS->>User: Generate new theme .zip file
    User->>Ghost: Navigate back to Settings > Design
    User->>Ghost: Click "Upload a theme"
    User->>Ghost: Select and upload new .zip file
    Ghost->>Ghost: Process and install modified theme
    Ghost->>User: Show upload success message
    User->>Ghost: Click "Activate now"
    Ghost->>Ghost: Activate modified theme

    Note over User, Razorpay: Step 5: Verification
    User->>Website: Visit live Ghost website
    Website->>User: Load page with modified theme
    Website->>Razorpay: Load payment button script
    Razorpay->>Website: Render payment button
    Website->>User: Display page with Razorpay payment button
    User->>User: Verify button appears correctly

    Note over User, Razorpay: Integration Complete ✅
```

## Key Interactions Summary

1. **Theme Download**: User accesses Ghost admin panel and downloads the current active theme as a .zip file
2. **Local Modification**: User extracts the theme, opens .hbs files in a text editor, and inserts the Razorpay payment button code
3. **Theme Upload**: User re-zips the modified theme and uploads it back to Ghost admin panel
4. **Activation**: User activates the new modified theme
5. **Verification**: The live website now displays the integrated Razorpay payment button

## Critical Points
- The payment button ID (`pl_XXxXX6XxXXXXxx`) must be replaced with the actual Razorpay payment button ID
- Proper .zip file structure is crucial - theme files should be at the root level of the .zip, not in a nested folder
- The button placement location determines where it appears on the website (footer, header, or specific pages)
