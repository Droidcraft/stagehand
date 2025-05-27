# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Essential Commands

### Build & Development

- `pnpm run build` - Full build pipeline (linting + DOM scripts + JS + types)
- `pnpm run build-js` - Build JavaScript using tsup
- `pnpm run build-types` - Generate TypeScript declarations
- `pnpm run build-dom-scripts` - Generate DOM manipulation scripts

### Linting & Formatting

- `pnpm run lint` - Run prettier and eslint (fixes formatting automatically)
- `pnpm run prettier` - Check code formatting
- `pnpm run prettier:fix` - Fix code formatting
- `pnpm run eslint` - Run ESLint

### Testing & Evaluation

- `pnpm run evals` - Run evaluation suite (builds first)
- `pnpm run e2e` - Run end-to-end tests with standard config
- `pnpm run e2e:bb` - Run e2e tests with Browserbase config
- `pnpm run e2e:local` - Run e2e tests with local browser config

### Examples

- `pnpm run example` - Run the default example script
- `pnpm run example 2048` - Run specific example (e.g., 2048.ts)

### Documentation

- `pnpm run docs` - Start documentation development server

## Architecture Overview

Stagehand is an AI web browsing framework built on Playwright with multiple LLM provider support. The codebase follows a modular architecture:

### Core Structure

- **`lib/`** - Core Stagehand library implementation
- **`types/`** - TypeScript type definitions
- **`examples/`** - Example scripts and usage demonstrations
- **`evals/`** - Evaluation and testing suite
- **`docs/`** - Documentation source files

### Key Components

- **`lib/index.ts`** - Main Stagehand class and public API
- **`lib/StagehandPage.ts`** - Page-level automation methods (act, extract, observe)
- **`lib/StagehandContext.ts`** - Browser context management
- **`lib/llm/`** - LLM client implementations (OpenAI, Anthropic, Google, etc.)
- **`lib/handlers/`** - Method handlers for act, extract, observe, and agent operations
- **`lib/dom/`** - DOM processing and element selection utilities
- **`lib/cache/`** - Caching system for LLM responses and actions

### LLM Provider System

The framework supports multiple LLM providers through a unified interface:

- Legacy providers: OpenAI, Anthropic, Google, Groq, Cerebras
- AI SDK providers: All providers supported by Vercel's AI SDK
- Custom clients can be passed in via `llmClient` parameter

### Agent Architecture

Two types of agents:

1. **Computer Use Agents** - OpenAI and Anthropic computer use models
2. **Open Operator Agent** - Default agent using DOM-based automation

### Environment Support

- **LOCAL** - Uses local Chromium browser with Playwright
- **BROWSERBASE** - Uses Browserbase remote browser service

## Important Development Notes

### Before Making Changes

1. Always run the full test suite after changes: `pnpm run e2e`
2. Ensure linting passes: `pnpm run lint`
3. Build succeeds: `pnpm run build`

### Configuration

- Main config: `stagehand.config.ts` - Default configuration for examples
- Environment variables are loaded from `.env` file
- Browserbase credentials: `BROWSERBASE_API_KEY`, `BROWSERBASE_PROJECT_ID`
- LLM API keys: `OPENAI_API_KEY`, `ANTHROPIC_API_KEY`, etc.

### Monorepo Structure

Uses pnpm workspaces with the following packages:

- `@browserbasehq/stagehand` - Main package (published)
- `@browserbasehq/stagehand-lib` - Library source (private)
- `@browserbasehq/stagehand-evals` - Evaluation suite (private)
- `@browserbasehq/stagehand-examples` - Examples (private)

### DOM Processing

- DOM scripts are generated at build time via `lib/dom/genDomScripts.ts`
- Scripts are injected into pages for element selection and interaction
- Uses CDP (Chrome DevTools Protocol) for browser automation

### Error Handling

- Custom error classes in `types/stagehandErrors.ts`
- Strict failure modes - remove failovers that compromise desired outcomes
- API client errors in `types/stagehandApiErrors.ts`
