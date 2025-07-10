> [!SUMMARY]+ Table of Contents
<%*
    // Get input from user - specify maximum header level to be displayed (default = 3)
    let header_limit = await tp.system.prompt("Show Contents Down to Which Header Level (1-6)?", "3");
    
    let headers = await tp.file.content
        .split('\n') // split file into lines
        .filter(t => t.match(/^[#]+\s+/gi)) // only get headers
        .map(h => {
            let header_level = h.split(' ')[0].match(/#/g).length;
            // get header text without special characters like '[' and ']'
            //let header_text = h.substring(h.indexOf(' ') + 1).replace(/[\[\]]+/g, '');
            // get header text without removing any special characters
            let header_text = h.substring(h.indexOf(' ') + 1);
	    // get header text URL as it is in the file (DON'T REMOVE SPECIAL CHARS OR LINK TO HEADER WON'T WORK)
            let header_url = h.substring(h.indexOf(' ') + 1);
            
            // Wikilinks style output:
            //let header_link = `[[${tp.file.title}#${header_url}|${header_text}]]`
		
            // Non-wikilinks style output:
            let file_title= tp.file.title.replace(/ /g, '%20');   // Replace spaces in file names with '%20'
            header_url = header_url.replace(/ /g, '%20');         // Replace spaces in urls with '%20'
            let header_link = `[${header_text}](${file_title}.md#${header_url})`


            // Output ALL header levels
            // prepend block-quote (>), indentation and bullet-point (-)
            //return `>${'    '.repeat(header_level - 1) + '- ' + header_link}`;

            // Output headers up to specified level
            if ( header_level <= header_limit) {
                return `>${'    '.repeat(header_level - 1) + '- ' + header_link}`;
            }

        })
        .join('\n')
	    
        // If not using all headers, empty lines are inserted where non-displayed headers should be.
        // This removes any blank lines
        while (headers.includes("\n\n")) { headers= headers.replace(/\n\n/g,'\n'); }
%><% headers %>
