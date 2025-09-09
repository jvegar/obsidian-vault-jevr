<%*
// ==============================================================================
// TABLE OF CONTENTS GENERATOR FOR TEMPLATER
// ==============================================================================
// This template generates a Table of Contents based on headers in your note
// Supports styled headers, special characters, and customizable depth levels
// FIXED: Properly handles bold and other markdown formatting in display text
// ==============================================================================

// CONFIGURATION PARAMETERS
// You can modify these values to customize the TOC generation
const MAX_LEVELS = 5; // Maximum header levels to include (1-6)
const INCLUDE_EMPTY_SECTIONS = false; // Include headers with no content below them
const ADD_HORIZONTAL_RULE = true; // Add horizontal rule after TOC
const CUSTOM_TOC_TITLE = "📋 Table of Contents"; // Title for the TOC section
const INDENT_STYLE = "  "; // Indentation style (spaces or tabs)
const LINK_STYLE = "section"; // "block" for [[Header]] or "section" for [[#Header]]

// ==============================================================================
// MAIN TOC GENERATION FUNCTION
// ==============================================================================

function generateTableOfContents() {
    // Get the current note content
    const noteContent = tp.file.content;
    
    if (!noteContent) {
        return "<!-- No content found in note -->";
    }
    
    // Extract headers from the note content
    const headers = extractHeaders(noteContent);
    
    if (headers.length === 0) {
        return "<!-- No headers found in note -->";
    }
    
    // Filter headers based on max levels
    const filteredHeaders = headers.filter(header => header.level <= MAX_LEVELS);
    
    if (filteredHeaders.length === 0) {
        return `<!-- No headers found within ${MAX_LEVELS} levels -->`;
    }
    
    // Generate the TOC structure
    const tocLines = generateTocLines(filteredHeaders);
    
    // Build the complete TOC
    let toc = "";
    
    // Add custom title if specified
    if (CUSTOM_TOC_TITLE) {
        toc += `## ${CUSTOM_TOC_TITLE}\n\n`;
    }
    
    // Add the TOC content
    toc += tocLines.join("\n");
    
    // Add horizontal rule if specified
    if (ADD_HORIZONTAL_RULE) {
        toc += "\n\n---\n";
    }
    
    return toc;
}

// ==============================================================================
// HEADER EXTRACTION FUNCTION
// ==============================================================================

function extractHeaders(content) {
    const headers = [];
    const lines = content.split('\n');
    
    // State tracking for context-aware parsing
    let inCodeBlock = false;           // Are we inside a fenced code block?
    let codeBlockDelimiter = null;     // What delimiter started the current code block?
    let inIndentedCodeBlock = false;   // Are we in an indented code block?
    
    for (let i = 0; i < lines.length; i++) {
        const line = lines[i];
        const trimmedLine = line.trim();
        
        // Check for fenced code block boundaries (``` or ~~~)
        const fencedCodeMatch = trimmedLine.match(/^(`{3,}|~{3,})/);
        if (fencedCodeMatch) {
            if (!inCodeBlock) {
                // Starting a new fenced code block
                inCodeBlock = true;
                codeBlockDelimiter = fencedCodeMatch[1][0]; // Either ` or ~
                continue;
            } else if (codeBlockDelimiter && trimmedLine.startsWith(codeBlockDelimiter.repeat(3))) {
                // Ending the current fenced code block
                inCodeBlock = false;
                codeBlockDelimiter = null;
                continue;
            }
        }
        
        // Check for indented code blocks (4+ spaces at start of line)
        const isIndentedCode = line.match(/^    /);
        if (isIndentedCode && !inCodeBlock) {
            inIndentedCodeBlock = true;
        } else if (!isIndentedCode && inIndentedCodeBlock && trimmedLine !== '') {
            // Non-empty line that's not indented ends the indented code block
            inIndentedCodeBlock = false;
        }
        
        // Skip header detection if we're in any type of code block
        if (inCodeBlock || inIndentedCodeBlock) {
            continue;
        }
        
        // Now we can safely check for headers in non-code contexts
        const headerMatch = trimmedLine.match(/^(#{1,6})\s+(.+)$/);
        
        if (headerMatch) {
            const level = headerMatch[1].length;
            const text = headerMatch[2];
            
            // Skip if this would be the TOC itself
            if (CUSTOM_TOC_TITLE && text.includes(CUSTOM_TOC_TITLE.replace(/[^\w\s]/g, ''))) {
                continue;
            }
            
            headers.push({
                level: level,
                text: text,
                rawText: text,
                lineNumber: i + 1
            });
        }
    }
    
    return headers;
}

// ==============================================================================
// TOC LINE GENERATION FUNCTION
// ==============================================================================

function generateTocLines(headers) {
    const tocLines = [];
    
    // Find the minimum header level to establish the baseline for indentation
    // This ensures proper hierarchy regardless of the document's starting level
    const minLevel = Math.min(...headers.map(h => h.level));
    
    headers.forEach(header => {
        // Calculate relative indentation based on the document's actual structure
        // The first level headers (minLevel) will have zero indentation
        // Each subsequent level will be indented relative to the baseline
        const relativeLevel = header.level - minLevel;
        const indent = INDENT_STYLE.repeat(relativeLevel);
        
        // FIXED: Create clean display text by removing markdown formatting
        // This ensures that **Bold Header** displays as "Bold Header" in the TOC
        const displayText = cleanMarkdownFormatting(header.text);
        
        // Create the link target - clean version for linking (already handled properly)
        const linkTarget = createLinkTarget(header.text);
        
        // Generate the TOC line based on link style
        let tocLine;
        if (LINK_STYLE === "section") {
            // Use section linking with hash symbol for same-document navigation
            // Now uses cleaned display text while maintaining proper link targeting
            tocLine = `${indent}- [[#${linkTarget}|${displayText}]]`;
        } else {
            // Block style creates links to separate notes (not recommended for TOC)
            tocLine = `${indent}- [[${linkTarget}]]`;
        }
        
        tocLines.push(tocLine);
    });
    
    return tocLines;
}

// ==============================================================================
// MARKDOWN FORMATTING CLEANUP FUNCTIONS
// ==============================================================================

function cleanMarkdownFormatting(text) {
    // This function removes markdown formatting for display purposes
    // while preserving the actual content for readability in the TOC
    
    return text
        // Remove bold formatting: **text** and __text__ -> text
        .replace(/\*\*(.+?)\*\*/g, '$1')      // **bold** -> bold
        .replace(/__(.+?)__/g, '$1')          // __bold__ -> bold
        
        // Remove italic formatting: *text* and _text_ -> text
        .replace(/\*(.+?)\*/g, '$1')          // *italic* -> italic
        .replace(/_(.+?)_/g, '$1')            // _italic_ -> italic
        
        // Remove other common markdown formatting
        .replace(/`(.+?)`/g, '$1')            // `code` -> code
        .replace(/~~(.+?)~~/g, '$1')          // ~~strikethrough~~ -> strikethrough
        .replace(/\[([^\]]+)\]\([^)]+\)/g, '$1') // [text](url) -> text
        
        // Clean up any remaining HTML entities
        .replace(/&amp;/g, '&')               // &amp; -> &
        .replace(/&lt;/g, '<')                // &lt; -> <
        .replace(/&gt;/g, '>')                // &gt; -> >
        .replace(/&quot;/g, '"')              // &quot; -> "
        
        // Clean up whitespace
        .replace(/\s+/g, ' ')                 // Multiple spaces -> single space
        .trim();                              // Remove leading/trailing spaces
}

function createLinkTarget(headerText) {
    // Create Obsidian-compatible header IDs that precisely match internal linking system
    // This function replicates Obsidian's header ID generation algorithm
    
    // Use the same cleaning logic as display text for consistency
    let cleanText = cleanMarkdownFormatting(headerText);
    
    // Return the cleaned text exactly as Obsidian would process it
    // Note: We don't modify special characters like &, periods, or numbers
    // because Obsidian preserves these in header IDs
    
    return cleanText;
}

// ==============================================================================
// UTILITY FUNCTIONS
// ==============================================================================

function debugHeaders(headers) {
    console.log("=== HEADER DEBUG INFO ===");
    console.log("Total headers found:", headers.length);
    
    // Calculate minimum level for relative indentation analysis
    const minLevel = Math.min(...headers.map(h => h.level));
    console.log("Minimum header level in document:", minLevel);
    console.log(""); // Empty line for readability
    
    headers.forEach((header, index) => {
        const displayText = cleanMarkdownFormatting(header.text);
        const linkTarget = createLinkTarget(header.text);
        const oldIndentLevel = header.level - 1; // Old calculation method
        const newIndentLevel = header.level - minLevel; // New relative calculation
        
        console.log(`${index + 1}. Header Analysis:`);
        console.log(`   Level: ${header.level}`);
        console.log(`   Line: ${header.lineNumber}`);
        console.log(`   Original text: "${header.text}"`);
        console.log(`   Display text: "${displayText}"`);
        console.log(`   Link target: "${linkTarget}"`);
        console.log(`   Old indent level: ${oldIndentLevel} (${INDENT_STYLE.repeat(oldIndentLevel)})`);
        console.log(`   New indent level: ${newIndentLevel} (${INDENT_STYLE.repeat(newIndentLevel)})`);
        console.log(`   Generated link: [[#${linkTarget}|${displayText}]]`);
        console.log(""); // Empty line between headers
    });
    console.log("========================");
}

// Test function to verify specific header processing
function testHeaderProcessing(headerText) {
    console.log("=== TESTING SPECIFIC HEADER ===");
    console.log(`Input: "${headerText}"`);
    console.log(`Display text: "${cleanMarkdownFormatting(headerText)}"`);
    console.log(`Link target: "${createLinkTarget(headerText)}"`);
    console.log(`Final link: [[#${createLinkTarget(headerText)}|${cleanMarkdownFormatting(headerText)}]]`);
    console.log("==============================");
}

// Function to demonstrate formatting cleanup
function demonstrateFormattingCleanup() {
    console.log("=== FORMATTING CLEANUP EXAMPLES ===");
    
    const testCases = [
        "**Bold Header**",
        "__Another Bold Header__",
        "*Italic Header*",
        "_Another Italic Header_",
        "`Code Header`",
        "~~Strikethrough Header~~",
        "**Bold** and *Italic* Combined",
        "[Link Text](https://example.com)",
        "Mixed **Bold** `Code` _Italic_ Text"
    ];
    
    testCases.forEach(test => {
        console.log(`Original: "${test}"`);
        console.log(`Cleaned:  "${cleanMarkdownFormatting(test)}"`);
        console.log(""); // Empty line for readability
    });
    
    console.log("==============================");
}

// Function to demonstrate indentation differences
function demonstrateIndentationLogic(headers) {
    console.log("=== INDENTATION COMPARISON ===");
    const minLevel = Math.min(...headers.map(h => h.level));
    
    console.log("Document structure analysis:");
    console.log(`Minimum header level: ${minLevel}`);
    console.log("Headers with old vs new indentation:");
    
    headers.forEach((header, index) => {
        const oldIndent = header.level - 1;
        const newIndent = header.level - minLevel;
        console.log(`${index + 1}. Level ${header.level}: "${header.text}"`);
        console.log(`   Old method: ${oldIndent} indents -> "${INDENT_STYLE.repeat(oldIndent)}- Link"`);
        console.log(`   New method: ${newIndent} indents -> "${INDENT_STYLE.repeat(newIndent)}- Link"`);
    });
    console.log("==============================");
}

// Function to analyze code block detection in document
function analyzeCodeBlockContext(content) {
    console.log("=== CODE BLOCK ANALYSIS ===");
    const lines = content.split('\n');
    let inCodeBlock = false;
    let codeBlockDelimiter = null;
    let inIndentedCodeBlock = false;
    let potentialHeadersInCode = [];
    
    for (let i = 0; i < lines.length; i++) {
        const line = lines[i];
        const trimmedLine = line.trim();
        
        // Check for fenced code block boundaries
        const fencedCodeMatch = trimmedLine.match(/^(`{3,}|~{3,})/);
        if (fencedCodeMatch) {
            if (!inCodeBlock) {
                inCodeBlock = true;
                codeBlockDelimiter = fencedCodeMatch[1][0];
                console.log(`Line ${i + 1}: Starting fenced code block with '${codeBlockDelimiter}'`);
            } else if (codeBlockDelimiter && trimmedLine.startsWith(codeBlockDelimiter.repeat(3))) {
                inCodeBlock = false;
                codeBlockDelimiter = null;
                console.log(`Line ${i + 1}: Ending fenced code block`);
            }
        }
        
        // Check for indented code blocks
        const isIndentedCode = line.match(/^    /);
        if (isIndentedCode && !inCodeBlock) {
            if (!inIndentedCodeBlock) {
                console.log(`Line ${i + 1}: Starting indented code block`);
                inIndentedCodeBlock = true;
            }
        } else if (!isIndentedCode && inIndentedCodeBlock && trimmedLine !== '') {
            console.log(`Line ${i + 1}: Ending indented code block`);
            inIndentedCodeBlock = false;
        }
        
        // Look for potential headers that would be ignored due to code context
        const headerMatch = trimmedLine.match(/^(#{1,6})\s+(.+)$/);
        if (headerMatch && (inCodeBlock || inIndentedCodeBlock)) {
            potentialHeadersInCode.push({
                line: i + 1,
                text: headerMatch[2],
                level: headerMatch[1].length,
                context: inCodeBlock ? 'fenced code block' : 'indented code block'
            });
        }
    }
    
    if (potentialHeadersInCode.length > 0) {
        console.log("\nPotential headers found inside code blocks (correctly ignored):");
        potentialHeadersInCode.forEach(item => {
            console.log(`   Line ${item.line}: "${item.text}" (Level ${item.level}) - in ${item.context}`);
        });
    } else {
        console.log("\nNo potential headers found inside code blocks.");
    }
    
    console.log("==============================");
}

// ==============================================================================
// TEMPLATE EXECUTION
// ==============================================================================

// Generate and return the Table of Contents
const tableOfContents = generateTableOfContents();

// DEBUGGING: Uncomment these lines to analyze your document structure
// testHeaderProcessing("**1. Features & Compatibility**");
// demonstrateFormattingCleanup();
// debugHeaders(extractHeaders(tp.file.content));
// demonstrateIndentationLogic(extractHeaders(tp.file.content));
// analyzeCodeBlockContext(tp.file.content);

// Return the generated TOC
-%>
<%* return tableOfContents; -%>