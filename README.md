🚀 NEXUS CSS Plugin - Complete Documentation
📋 Table of Contents
Overview

Architecture

Installation

Configuration

Usage

Supported Preprocessors

Technical Reference

Development Guide

Roadmap

Troubleshooting

Contributing

License

🎯 Overview
What is NEXUS Plugin?
NEXUS is a smart CSS workflow plugin that enhances your existing editor and browser with visual CSS insights, impact analysis, and live editing for multiple preprocessors.

Core Value Proposition
"Click any webpage element, edit its source CSS/SCSS, and instantly see every other element that will be affected by your change."

Key Features
🔍 Visual Inspection - Click elements to see original source files

🎯 Impact Analysis - See cascade effects before saving

⚡ Live Editing - Edit SCSS/CSS with instant browser updates

🔌 Multi-Preprocessor - SASS, LESS, Stylus support

🛠️ Editor Agnostic - Works with VS Code, WebStorm, Sublime, etc.

🏗️ Architecture
System Overview
text
┌─────────────────┐    WebSocket    ┌──────────────────┐
│   Your Editor   │ ◄─────────────► │  NEXUS Browser   │
│   (VS Code,     │                 │     Helper       │
│   WebStorm,     │                 │                  │
│   Sublime, etc) │                 └──────────────────┘
└─────────────────┘                         │
         │                                  │ CDP
         │ File System                      │
         ▼                                  ▼
┌─────────────────┐                 ┌──────────────────┐
│ NEXUS Language  │                 │   Target Website │
│    Server       │                 │                  │
└─────────────────┘                 └──────────────────┘
Core Components
1. Language Server (LSP)
Preprocessor detection and compilation

Source map resolution

Impact analysis engine

File watching and change detection

2. Editor Extensions
VS Code, WebStorm, Sublime integration

UI for impact visualization

Command palette integration

Status indicators

3. Browser Helper
Enhanced DevTools overlay

Element inspection and highlighting

Live CSS reloading

WebSocket communication bridge

4. Preprocessor Adapters
typescript
interface IPreprocessor {
  name: string;
  extensions: string[];
  compile(filePath: string): Promise<CompileResult>;
  parseDependencies(content: string): Dependency[];
  analyzeImpact(change: Change): ImpactResult;
}
Data Flow
User inspects element in browser

Browser helper resolves source map to original file

Language server analyzes dependencies and impact

Editor extension displays insights and allows editing

Changes compile and reload live in browser

📥 Installation
Quick Start
bash
# Method 1: VS Code Extension
code --install-extension nexus-css

# Method 2: NPM Package (for other editors)
npm install -g @nexus-css/plugin

# Method 3: Browser Helper Only
npx @nexus-css/browser-helper
Editor-Specific Setup
VS Code
json
// settings.json
{
  "nexus.autoStart": true,
  "nexus.port": 3001,
  "nexus.features.impactAnalysis": true,
  "nexus.preprocessors": ["sass", "less"]
}
WebStorm
Install "NEXUS CSS" plugin from marketplace

Enable in Settings → Tools → NEXUS CSS

Configure preprocessor paths

Sublime Text
Install via Package Control: NEXUS CSS

Configure via Preferences → Package Settings → NEXUS CSS

Browser Setup
javascript
// Add to your site for enhanced features
<script src="http://localhost:3001/nexus-client.js"></script>
Or use the browser extension:

Chrome Web Store: "NEXUS CSS Helper"

Firefox Add-ons: "NEXUS CSS Helper"

⚙️ Configuration
Project Configuration (.nexus/config.json)
json
{
  "version": "1.0",
  "preprocessors": [
    {
      "name": "sass",
      "entryPoints": ["src/styles/main.scss"],
      "configFile": "sass.config.js",
      "outputDir": "dist/css",
      "watch": true
    },
    {
      "name": "less",
      "entryPoints": ["src/legacy/styles.less"],
      "outputDir": "dist/css"
    }
  ],
  "mappings": [
    {
      "url": "http://localhost:3000/src/",
      "fs": "./src/",
      "type": "exact"
    },
    {
      "url": "http://localhost:3000/dist/",
      "fs": "./dist/",
      "type": "exact"
    }
  ],
  "features": {
    "impactAnalysis": true,
    "liveReload": true,
    "sourceMaps": true,
    "dependencyGraph": true,
    "autoFixSourceMaps": true
  },
  "exclude": [
    "node_modules/**",
    "dist/**",
    ".git/**",
    "**/*.min.css"
  ],
  "compilerOptions": {
    "sourceMap": true,
    "sourceMapContents": true,
    "outputStyle": "expanded"
  }
}
Global Configuration
json
// ~/.nexus/global.json
{
  "editor": "vscode",
  "defaultPort": 3001,
  "browser": "chrome",
  "analytics": false,
  "updateChannel": "stable"
}
🚀 Usage
Basic Workflow
1. Start NEXUS
bash
# In your project root
nexus start

# Or from editor command palette
NEXUS: Start Inspector
2. Inspect Elements
Open your site in browser

Click "Enable NEXUS" in the overlay

Click any element to see source information

3. Edit with Confidence
scss
// Editing variables.scss:12
$primary-color: #007bff;

// NEXUS shows impact:
// 🔍 This change affects 47 elements across 12 files
// • buttons.scss (8 elements)
// • headers.scss (5 elements) 
// • cards.scss (12 elements)
// • forms.scss (22 elements)
4. Live Updates
Changes save to source files

CSS recompiles automatically

Browser updates without refresh

Impact analysis updates in real-time

Editor Commands
bash
# VS Code Command Palette (Ctrl+Shift+P)
NEXUS: Start Inspector           # Launch browser connection
NEXUS: Analyze Dependencies      # Show project CSS graph
NEXUS: Find CSS References       # Find where style is used
NEXUS: Impact Preview            # Show what changes affect
NEXUS: Debug Source Maps         # Validate source map chain
NEXUS: Validate Setup            # Check configuration
NEXUS: Restart Compiler          # Restart preprocessor watchers
Browser Interface
Enhanced DevTools: SCSS file links in styles pane

Impact Sidebar: Inheritance chain and dependencies

Live Reload Indicator: Shows compilation status

Source Map Debugger: Validates map resolution

🎨 Supported Preprocessors
SASS/SCSS ✅ Full Support
Feature	Support Level	Notes
Variables	✅ Complete	Full impact analysis
Mixins	✅ Complete	Dependency tracking
Extends	✅ Complete	Inheritance graphs
Placeholders	✅ Complete	Usage analysis
Modules	✅ Complete	@use/@forward
Source Maps	✅ Native	Best support
Configuration:

json
{
  "name": "sass",
  "entryPoints": ["src/styles/main.scss"],
  "configFile": "sass.config.js",
  "includePaths": ["src", "node_modules"]
}
LESS ✅ Basic Support
Feature	Support Level	Notes
Variables	✅ Complete	Impact analysis
Mixins	✅ Basic	Usage tracking
Imports	✅ Complete	Dependency resolution
Functions	⚠️ Limited	Basic detection
Source Maps	✅ Supported	
Configuration:

json
{
  "name": "less", 
  "entryPoints": ["src/styles/main.less"],
  "plugins": ["less-plugin-autoprefix"]
}
Stylus ✅ Basic Support
Feature	Support Level	Notes
Variables	✅ Complete	Impact analysis
Mixins	✅ Basic	Usage tracking
Functions	⚠️ Limited	Basic detection
Source Maps	✅ Supported	
PostCSS 🔄 Pipeline Support
Chains with other preprocessors

Source map merging

Plugin-aware analysis

🔧 Technical Reference
API Reference
Language Server Protocol
typescript
// LSP Methods
interface NexusLanguageServer {
  // File operations
  'nexus/fileChanged': (params: FileChangeParams) => FileChangeResult;
  'nexus/compileFile': (params: CompileParams) => CompileResult;
  
  // Analysis
  'nexus/analyzeImpact': (params: ImpactParams) => ImpactResult;
  'nexus/findReferences': (params: ReferenceParams) => ReferenceResult;
  'nexus/getDependencyGraph': (params: GraphParams) => GraphResult;
  
  // Source maps
  'nexus/resolveSource': (params: SourceParams) => SourceResult;
  'nexus/validateMaps': (params: ValidateParams) => ValidateResult;
}
WebSocket Events
typescript
// Client → Server
interface ClientEvents {
  'inspect:element': (selector: string) => void;
  'edit:css': (change: CSSChange) => void;
  'compile:request': (file: string) => void;
}

// Server → Client  
interface ServerEvents {
  'css:updated': (result: UpdateResult) => void;
  'impact:analysis': (analysis: ImpactAnalysis) => void;
  'compilation:status': (status: CompilationStatus) => void;
  'error:occurred': (error: NexusError) => void;
}
Preprocessor Adapter Interface
typescript
interface IPreprocessor {
  readonly name: string;
  readonly extensions: string[];
  readonly version: string;
  
  // Core compilation
  compile(filePath: string, options?: CompileOptions): Promise<CompileResult>;
  watch(filePath: string, callback: WatchCallback): WatchHandle;
  
  // Analysis
  parseDependencies(content: string, filePath: string): Dependency[];
  extractVariables(content: string, filePath: string): Variable[];
  findUsages(ast: any, target: string, filePath: string): Usage[];
  
  // Impact analysis
  analyzeImpact(change: Change, context: AnalysisContext): ImpactResult;
  getDependencyGraph(entryPoints: string[]): DependencyGraph;
  
  // Utilities
  validateConfig(config: any): ValidationResult;
  getCompilerInfo(): CompilerInfo;
}

interface CompileResult {
  css: string;
  sourceMap?: RawSourceMap;
  warnings: string[];
  errors: string[];
  dependencies: string[];
}
File Structure
text
~/.nexus/
├── config/
│   ├── global.json
│   └── projects/
├── cache/
│   ├── dependency-graphs/
│   ├── compiled-css/
│   └── source-maps/
└── logs/
    ├── compiler.log
    └── analysis.log

your-project/
├── .nexus/
│   ├── config.json
│   └── cache/ (gitignored)
├── src/
│   └── styles/
└── dist/
🛠️ Development Guide
Setting Up Development Environment
bash
# Clone repository
git clone https://github.com/nexus-css/plugin.git
cd plugin

# Install dependencies
npm install

# Build all packages
npm run build

# Start development servers
npm run dev:lsp        # Language Server
npm run dev:extension  # VS Code Extension  
npm run dev:browser    # Browser Helper
Project Structure
text
nexus-plugin/
├── packages/
│   ├── language-server/     # Core LSP implementation
│   ├── vscode-extension/    # VS Code integration
│   ├── browser-helper/      # Browser overlay & DevTools
│   ├── preprocessors/       # Adapter implementations
│   └── shared/              # Common types and utilities
├── examples/                # Example projects
├── docs/                    # Documentation
└── scripts/                 # Build and release scripts
Adding a New Preprocessor
1. Create Adapter Package
bash
cd packages/preprocessors
mkdir postcss-adapter
cd postcss-adapter
npm init
2. Implement Interface
typescript
// packages/preprocessors/postcss-adapter/src/index.ts
import { IPreprocessor, CompileResult } from '@nexus/shared';

export class PostCSSAdapter implements IPreprocessor {
  name = 'postcss';
  extensions = ['.css'];
  
  async compile(filePath: string, options: CompileOptions): Promise<CompileResult> {
    const postcss = await import('postcss');
    const content = await fs.readFile(filePath, 'utf8');
    
    const result = await postcss.process(content, {
      from: filePath,
      to: options.outputPath,
      map: options.sourceMap ? { inline: false } : false
    });
    
    return {
      css: result.css,
      sourceMap: result.map?.toJSON(),
      warnings: result.warnings().map(w => w.toString()),
      errors: [],
      dependencies: result.messages
        .filter(m => m.type === 'dependency')
        .map(m => m.file)
    };
  }
  
  // Implement other required methods...
}
3. Register Adapter
typescript
// packages/language-server/src/preprocessors/manager.ts
import { PostCSSAdapter } from '@nexus/postcss-adapter';

PreprocessorManager.register(new PostCSSAdapter());
4. Add Tests
typescript
// packages/preprocessors/postcss-adapter/test/index.test.ts
describe('PostCSSAdapter', () => {
  it('should compile basic CSS', async () => {
    const adapter = new PostCSSAdapter();
    const result = await adapter.compile('test.css');
    expect(result.css).toBeDefined();
  });
});
Building and Testing
bash
# Build specific package
npm run build --workspace=@nexus/language-server

# Run tests
npm test

# Test with example projects
npm run test:integration

# Benchmark performance
npm run benchmark
🗺️ Roadmap
Phase 1: MVP (Months 1-3)
Target: Basic SASS/SCSS workflow

✅ Project configuration and detection

✅ SASS compilation with source maps

✅ Basic impact analysis for variables

✅ VS Code extension

✅ Browser helper overlay

✅ Live reload for CSS changes

Phase 2: Enhanced Analysis (Months 4-6)
Target: Advanced insights and multi-preprocessor

🔄 Advanced impact analysis (mixins, extends)

🔄 LESS and Stylus adapters

🔄 Visual dependency graphs

🔄 Performance optimizations

🔄 WebStorm and Sublime extensions

Phase 3: Production Ready (Months 7-9)
Target: Enterprise features and polish

🔄 Team collaboration features

🔄 Visual regression testing

🔄 CI/CD integration

🔄 Advanced debugging tools

🔄 Plugin marketplace

Phase 4: AI & Advanced Features (Months 10-12)
Target: Next-generation CSS tooling

🔄 AI-assisted refactoring

🔄 Automated migration tools

🔄 Design system integration

🔄 Cross-browser compatibility insights

Future Considerations
CSS-in-JS support (Styled Components, Emotion)

Tailwind CSS JIT analysis

Design tool integration (Figma, Sketch)

Performance budgeting

Accessibility insights

🐛 Troubleshooting
Common Issues
Source Maps Not Working
bash
# Debug source maps
nexus debug:sourcemaps

# Check configuration
nexus validate:config

# Common fixes:
# 1. Ensure preprocessor generates source maps
# 2. Verify sourceMapContents is enabled
# 3. Check file paths in generated maps
Live Reload Not Updating
bash
# Check file watchers
nexus debug:watchers

# Verify compilation
nexus debug:compilation

# Reset cache if needed
nexus cache:clear
Impact Analysis Missing
bash
# Check preprocessor support
nexus debug:features

# Validate analysis setup
nexus validate:analysis
Debug Commands
bash
# Comprehensive debugging
nexus debug:all

# Specific debug areas
nexus debug:connection    # WebSocket status
nexus debug:compilation   # Preprocessor issues
nexus debug:analysis      # Impact analysis
nexus debug:performance   # Speed issues
Log Files
Compiler Logs: ~/.nexus/logs/compiler.log

Analysis Logs: ~/.nexus/logs/analysis.log

Connection Logs: ~/.nexus/logs/connection.log

🤝 Contributing
Getting Started
Fork the repository

Create a feature branch: git checkout -b feature/amazing-feature

Commit changes: git commit -m 'Add amazing feature'

Push to branch: git push origin feature/amazing-feature

Open a Pull Request

Development Standards
Code Style: ESLint + Prettier configuration provided

Testing: Jest for unit tests, Playwright for integration

Documentation: Update README and relevant docs

Commit Messages: Conventional commits format

Areas Needing Contribution
New preprocessor adapters

Additional editor integrations

Performance optimizations

Documentation improvements

Bug fixes and issue triaging

📄 License
MIT License - see LICENSE for details.

Copyright (c) 2024 NEXUS CSS Contributors

🆘 Support
Documentation: docs.nexus-css.dev

GitHub Issues: Report Bugs

Discord: Community Chat

Twitter: @nexus_css

🔄 Changelog
v1.0.0 (Planned)
Initial release with SASS/SCSS support

VS Code extension

Basic impact analysis

Live editing and reload

v1.1.0 (Planned)
LESS and Stylus support

Enhanced impact analysis

Additional editor integrations

Performance improvements

This documentation is actively maintained. For the latest updates, always check our documentation site.
