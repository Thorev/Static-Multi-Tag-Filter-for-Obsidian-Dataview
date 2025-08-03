# Static-Multi-Tag-Filter-for-Obsidian-Dataview
Static Multi-Tag Filter for Obsidian Dataview

````js
```dataviewjs
/* ===== TAG FILTER CONFIGURATION ===== */
const targetTags = [
    "#Эссе", 
    "#В_работе", 
    // Add more tags as needed
    // Format: "#tag_name"
    // Max tags: unlimited
];

/* ===== DISPLAY SETTINGS ===== */
const displaySettings = {
    header: "Pages tagged:",          // Main header text
    showCount: true,                  // Show results counter
    sortAlphabetically: true,         // Sort results A-Z
    showTagList: true,                // Show searched tags
    showCreationDates: false,         // Show note creation dates
    maxResults: 0                     // 0 = no limit, or set number
};

/* ===== MAIN CODE ===== */
// Header section with tags
if (displaySettings.header) {
    dv.header(2, displaySettings.header);
    
    if (displaySettings.showTagList && targetTags.length > 0) {
        const tagString = targetTags.map(t => `"${t}"`).join(" + ");
        dv.paragraph(`**Filter:** ${tagString}`);
    }
}

// Filter pages containing ALL target tags
let filteredPages = dv.pages()
    .where(p => 
        targetTags.every(tag => 
            p.file.tags && 
            p.file.tags.includes(tag)
        )
    );

// Apply sorting
if (displaySettings.sortAlphabetically) {
    filteredPages = filteredPages.sort(p => p.file.name);
}

// Apply results limit
if (displaySettings.maxResults > 0) {
    filteredPages = filteredPages.slice(0, displaySettings.maxResults);
}

// Results counter
if (displaySettings.showCount) {
    const total = filteredPages.length;
    const limitNote = displaySettings.maxResults > 0 && 
                      total >= displaySettings.maxResults ?
        ` (showing first ${displaySettings.maxResults} of ${total})` : "";
    
    dv.paragraph(`**${total} notes found**${limitNote}`);
}

// Display results
if (filteredPages.length > 0) {
    if (displaySettings.showCreationDates) {
        dv.table(
            ["Note", "Created"],
            filteredPages.map(p => [
                p.file.link,
                p.file.ctime?.toFormat("yyyy-MM-dd") || "N/A"
            ])
        );
    } else {
        dv.list(filteredPages.file.link);
    }
} else {
    dv.paragraph("No matching notes found");
}

// Optional: Add search tips
dv.el("hr", "");
dv.paragraph("> *To modify search: Edit `targetTags` in the code block*");
```
````


### Usage Tips:

Edit Tags: Modify the targetTags array
Adjust Settings: Toggle features in displaySettings
Save Note
Copy Block: Duplicate for different searches

