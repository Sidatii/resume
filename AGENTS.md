# AGENTS.md - Developer Guidelines for Resume Template

## Overview
This repository contains LaTeX resume templates for software developers. The project uses Docker for consistent builds and supports multiple resume versions.

## Build Commands

### Full Build (Docker)
```bash
# Build Docker image and compile resume
docker build -t latex-resume .
docker run --rm -i -v "$PWD":/data latex-resume pdflatex <filename>.tex

# Using the build script
./build.sh
```

### Direct LaTeX Compilation (requires local LaTeX installation)
```bash
# Compile a specific resume file
pdflatex v1/sidati_nouhi_resume.tex
lualatex v1/sidati_nouhi_resume_lualatex.tex
lualatex v2/sidati_nouhi_resume_1.tex

# Clean auxiliary files after build
rm -f *.aux *.log *.out *.fls *.fdb_latexmk *.synctex.gz
```

### Build Validation
```bash
# Check for compilation errors
pdflatex <filename>.tex 2>&1 | grep -i error

# Verify PDF generation
ls -la *.pdf
```

## Testing Commands

Since this is a LaTeX document project, "testing" involves validation rather than unit tests:

### PDF Generation Test
```bash
# Test compilation of all resume versions
for file in v*/sidati_nouhi_resume*.tex; do
    echo "Testing $file..."
    if [[ "$file" == *"lualatex"* ]]; then
        lualatex "$file" && echo "✓ $file compiled successfully" || echo "✗ $file failed"
    else
        pdflatex "$file" && echo "✓ $file compiled successfully" || echo "✗ $file failed"
    fi
done
```

### Single File Validation
```bash
# Validate specific resume file
pdflatex v1/sidati_nouhi_resume.tex && echo "Resume compiled successfully"
lualatex v1/sidati_nouhi_resume_lualatex.tex && echo "Resume compiled successfully"
```

### Font and Package Check
```bash
# Check for missing packages in logs (use appropriate engine)
lualatex <filename>.tex 2>&1 | grep -i "undefined\|missing\|font.*not.*found"
pdflatex <filename>.tex 2>&1 | grep -i "undefined\|missing"
```

## Code Style Guidelines

### LaTeX Structure and Formatting

#### Document Class and Packages
- Use `article` class with appropriate paper size: `\documentclass[letterpaper,11pt]{article}`
- Import packages in logical groups with comments:
```latex
% Core LaTeX packages
\usepackage{latexsym}
\usepackage[empty]{fullpage}

% Font and color packages
\usepackage{titlesec}
\usepackage[usenames,dvipsnames]{color}
```

#### Font Selection
- **Main font**: Roboto (or Noto Sans fallback) - Clean, modern sans-serif perfect for software engineers
- **Monospace font**: Source Code Pro - Excellent for technical content and code snippets
- For lualatex: Use `\setmainfont{Roboto}` with Noto Sans fallback for systems without Roboto
- For pdflatex: Use system fonts or LaTeX defaults

#### Document Structure
- Follow standard LaTeX document structure: `\begin{document}` ... `\end{document}`
- Use clear section separation with comments:
```latex
%----------HEADING-----------------
\begin{tabular*}...

%-----------EDUCATION-----------------
\section{Education}
```

### Custom Commands and Macros

#### Naming Conventions
- Use descriptive names starting with `\resume`: `\resumeHeading`, `\resumeSubheading`
- Follow camelCase for multi-word commands: `\resumeSubItem`
- Document custom commands with comments:
```latex
% Custom commands
\newcommand{\resumeItem}[2]{
  \item\small{
    \textbf{#1}{: #2 \vspace{-2pt}}
  }
}
```

#### Command Parameters
- Use numbered parameters `#1`, `#2` for required arguments
- Document parameter meanings in comments above command definitions

### Formatting and Typography

#### Text Formatting
- Use semantic markup: `\textbf{}` for bold, `\textit{}` for italic
- Consistent spacing: `\vspace{-5pt}` for fine-tuning vertical space
- Use `\small` for secondary information

#### Lists and Items
- Use `\begin{itemize}` for unordered lists
- Customize list appearance with `enumitem` package:
```latex
\begin{itemize}[leftmargin=*]
```

#### Tables and Tabular
- Use `tabularx` for flexible table layouts
- Align columns appropriately: `l@{\extracolsep{\fill}}r` for left-right alignment
- Use `\extracolsep{\fill}` for automatic spacing

### Content Organization

#### Section Organization
- Follow standard resume structure: Education, Experience, Technologies, Projects
- Use `\section{}` for main headings
- Use `\resumeSubheading` for position/company entries
- Technologies section should be organized by categories:
  ```latex
  \textbf{Backend:} Java, Spring Boot, REST APIs...
  \textbf{Frontend:} Angular, React, TypeScript...
  \textbf{Database:} PostgreSQL, MongoDB, Redis...
  \textbf{CI/CD:} Docker, Jenkins, GitHub Actions...
  ```

#### Date Formatting
- Use consistent date format: `Sep 2022 -- Juil 2024`
- Use `--` for date ranges (en-dash)
- Include location for remote/in-person positions

### Code Quality

#### Comments and Documentation
- Use `%` for single-line comments
- Document complex formatting logic:
```latex
% Adjust margins
\addtolength{\oddsidemargin}{-0.5in}
```
- Comment out unused code rather than deleting:
```latex
% Custom fonts (uncomment if needed)
% \usepackage{fontspec}
```

#### File Organization
- Keep auxiliary files separate (handled by .gitignore)
- Name files descriptively: `sidati_nouhi_resume.tex`
- Use version directories: `v1/`, `v2/` for different iterations

### Error Handling and Debugging

#### Compilation Errors
- Check `.log` files for detailed error messages
- Common issues:
  - Missing packages: Install `texlive-latex-extra`
  - Font issues: Check fontspec usage
  - Path issues: Use absolute paths for included files

#### ATS (Applicant Tracking System) Compatibility
- Ensure machine-readable PDF: `\pdfgentounicode=1`
- Use standard fonts and avoid complex formatting
- Test with PDF readers for text extraction

### Version Control

#### Git Practices
- Commit auxiliary files (.gitignore handles cleanup)
- Use descriptive commit messages: "Update experience section"
- Branch for major changes: `feature/new-template-version`

#### File Naming
- Use consistent naming: `{name}_resume.tex`
- Version suffixes: `resume_1.tex`, `resume_2.tex`
- Avoid spaces in filenames

### Internationalization

#### Language Support
- Load babel for language support: `\usepackage[english]{babel}`
- Use appropriate date formats for locale
- Ensure proper encoding: `\input{glyphtounicode}`

#### Character Encoding
- Use UTF-8 for source files
- Handle special characters properly
- Test compilation with non-ASCII names

### Performance and Optimization

#### Build Optimization
- Use Docker for consistent environments
- Cache Docker layers appropriately
- Minimize package imports to reduce compilation time

#### File Size
- Keep PDF size reasonable (< 1MB)
- Optimize images if included
- Use efficient LaTeX constructs

### Maintenance

#### Template Updates
- Test changes across all resume versions
- Update build scripts when adding new features
- Document breaking changes

#### Dependency Management
- Pin Docker image versions
- Document required LaTeX packages
- Test with different TeX distributions

### Security Considerations

#### Content Security
- Avoid including sensitive information
- Use placeholder data for templates
- Sanitize personal information before sharing

#### Build Security
- Use trusted Docker images
- Validate PDF output for embedded content
- Avoid external file inclusions

### Collaboration Guidelines

#### Code Reviews
- Review LaTeX syntax and formatting
- Test compilation in different environments
- Check for consistent styling

#### Issue Tracking
- Use descriptive issue titles: "Compilation fails on Ubuntu"
- Include LaTeX version and error logs
- Provide minimal reproducible examples

### Development Environment

#### Required Tools
- Docker for consistent builds
- LaTeX distribution (TeX Live) for local development
- PDF viewer for validation

#### Editor Configuration
- Use LaTeX-aware editors (VS Code with LaTeX extensions)
- Enable syntax highlighting
- Configure build shortcuts

### Deployment and Distribution

#### GitHub Actions
- Automatic PDF generation on push/PR
- Use standard LaTeX Docker images
- Commit generated PDFs for easy access

#### Template Usage
- Provide clear setup instructions
- Include customization guidelines
- Document required modifications

This guide ensures consistent, professional LaTeX resume templates that compile reliably across different environments and maintain high standards for both code quality and visual presentation.</content>
<parameter name="filePath">/home/human/upscale/resume/AGENTS.md