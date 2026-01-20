# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This repository contains custom clipper templates for [Obsidian Web Clipper](https://github.com/obsidianmd/obsidian-clipper), a browser extension that captures web content into Obsidian notes. These templates define how content from various websites should be extracted and formatted when clipping to Obsidian.

## Template Architecture

### Template Structure

All templates are JSON files located in the `templates/` directory. Each template follows a consistent schema with these key components:

1. **Metadata**
   - `schemaVersion`: Always "0.1.0"
   - `name`: Display name for the template
   - `behavior`: Usually "create" to create new notes
   - `triggers`: Array of URL patterns or schema types that activate this template

2. **Output Configuration**
   - `noteNameFormat`: Template for the note filename
   - `path`: Destination folder in Obsidian vault (e.g., "Clippings", "References")
   - `noteContentFormat`: Template for the note body content

3. **Properties Array**
   - Frontmatter properties that will be added to the Obsidian note
   - Each property has: `name`, `value` (using template syntax), and `type`
   - Types include: `text`, `number`, `date`, `multitext`

### Template Categories

1. **Website-Specific Templates**
   - Target specific sites with URL-based triggers (e.g., `https://www.goodreads.com/book/`)
   - Use CSS selectors to extract specific elements (e.g., `selector:.BookPageTitleSection__title h1`)
   - Examples: arxiv-clipper.json, goodreads-clipper.json, imdb-clipper.json, wikipedia-clipper.json

2. **Schema-Based Templates**
   - Trigger on structured data types (e.g., `schema:@Recipe`, `schema:@Product`)
   - Extract data from Schema.org markup
   - Work across multiple sites that share the same schema type
   - Examples: recipes-clipper.json, product-clipper.json

### Template Syntax Patterns

Templates use a custom syntax with double braces `{{...}}` that supports:

- **Selectors**: `{{selector:CSS_SELECTOR}}` - Extract content using CSS selectors
- **Schema data**: `{{schema:propertyName}}` or `{{schema:@Type:property}}`
- **Metadata**: `{{meta:property:name}}`, `{{title}}`, `{{url}}`, `{{date}}`, `{{description}}`, `{{image}}`
- **Filters** (chained with `|`):
  - `first`, `last`, `unique` - Array operations
  - `split:"delimiter"|slice:start,end` - String manipulation
  - `date:"YYYY-MM-DD"` - Date formatting
  - `wikilink` - Convert to Obsidian wikilink format `[[text]]`
  - `join` - Join array elements
  - `replace:"old","new"` - Text replacement
  - `round:decimals` - Number rounding
  - `list:task` - Convert to checklist
  - `table` - Format as table
  - `uncamel`, `capitalize` - Text transformations
  - `remove_html:("selectors")` - Remove HTML elements
  - `markdown` - Convert HTML to markdown

### Trigger Patterns

1. **URL triggers**: Exact strings or regex patterns
   - Exact: `"https://arxiv.org/html/"`
   - Regex: `"/^https:\\/\\/[a-z-.]+\\.wikipedia\\.org\\/wiki\\/.*$/"` (regex wrapped in forward slashes)

2. **Schema triggers**: Match structured data types
   - Format: `"schema:@TypeName"` (e.g., `"schema:@Recipe"`, `"schema:@Product"`)

## Common Development Tasks

When creating or modifying templates:

1. **Test the CSS selectors** on the target website first to ensure they capture the correct elements
2. **Use appropriate data types** for properties:
   - `number` for numeric values (ratings, years, pages, ISBN)
   - `date` for temporal data
   - `multitext` for arrays/lists (tags, authors, genres)
   - `text` for single-line strings
3. **Add proper trigger patterns**:
   - For website-specific templates, include the base URL
   - Use regex to match URL patterns with variations
   - Ensure regex patterns are properly escaped for JSON
4. **Set reasonable default paths**:
   - "Clippings" for general web content
   - "References" for structured reference material (books, movies, products)
5. **Include common properties** where applicable:
   - `categories` - For organizing in Obsidian (usually a wikilink)
   - `created` - Set to `{{date}}` for creation timestamp
   - `tags` - For tagging/filtering
   - `source` or `url` - To preserve the original URL

## Repository Structure

```
clipper-templates/
├── templates/           # All template JSON files
│   ├── arxiv-clipper.json
│   ├── goodreads-clipper.json
│   ├── imdb-clipper.json
│   ├── recipes-clipper.json
│   └── ...
├── README.md           # Template documentation
└── LICENSE
```

## Notes

- This is a collection of templates, not a software project - there are no build, test, or deployment commands
- Templates are meant to be imported into Obsidian Web Clipper (see [official documentation](https://help.obsidian.md/web-clipper/templates))
- Templates work with the author's specific Obsidian vault structure available at https://github.com/kepano/kepano-obsidian
