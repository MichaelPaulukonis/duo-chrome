# Design Document

## Overview

The `genart-monorepo` will be an Nx-powered monorepo containing multiple creative coding applications with shared libraries for common patterns. The design prioritizes maintaining existing project functionality while enabling code reuse and consistent development experience.

## Architecture

### Monorepo Structure
```
genart-monorepo/
├── apps/
│   ├── duo-chrome/                 # Migrated duo-chrome project
│   │   ├── src/
│   │   ├── public/
│   │   ├── vite.config.js
│   │   └── project.json
│   └── crude-collage-painter/      # Migrated crude-collage-painter project
│       ├── src/
│       ├── public/
│       └── project.json
├── libs/
│   ├── p5-utils/                   # Shared p5.js utilities
│   ├── color-palettes/             # Shared color systems (RISO, etc.)
│   ├── image-processing/           # Common image manipulation
│   └── ui-components/              # Shared UI components
├── tools/
│   └── scripts/                    # Custom build/deploy scripts
├── docs/
│   ├── architecture/
│   └── guides/
├── nx.json                         # Nx configuration
├── package.json                    # Root package.json
└── workspace.json                  # Workspace configuration
```

### Technology Stack Integration

**Build Systems:**
- **Nx**: Monorepo orchestration, task running, caching
- **Vite**: Maintained for duo-chrome and potentially crude-collage-painter
- **TypeScript**: Optional gradual adoption for better tooling

**Package Management:**
- **pnpm**: Workspace-aware package manager (already in use)
- **Shared dependencies**: Common packages hoisted to root
- **Project-specific dependencies**: Isolated when needed

## Components and Interfaces

### Nx Workspace Configuration

**Project Configuration (`project.json` per app):**
```json
{
  "name": "duo-chrome",
  "sourceRoot": "apps/duo-chrome/src",
  "projectType": "application",
  "targets": {
    "dev": {
      "executor": "@nx/vite:dev-server",
      "options": {
        "buildTarget": "duo-chrome:build"
      }
    },
    "build": {
      "executor": "@nx/vite:build",
      "outputs": ["{options.outputPath}"],
      "options": {
        "outputPath": "dist/apps/duo-chrome"
      }
    },
    "preview": {
      "executor": "@nx/vite:preview-server"
    }
  }
}
```

### Shared Libraries Architecture

**1. p5-utils Library**
```typescript
// libs/p5-utils/src/index.ts
export interface P5Sketch {
  setup: (p: p5) => void;
  draw: (p: p5) => void;
  keyPressed?: (p: p5) => void;
  mouseClicked?: (p: p5) => void;
}

export function createSketch(sketch: P5Sketch): (p: p5) => void;
export function getRandomUniqueItem<T>(arr: T[], excludeItems: T[]): T;
```

**2. color-palettes Library**
```typescript
// libs/color-palettes/src/index.ts
export interface ColorPalette {
  name: string;
  colors: Array<{ name: string; color: [number, number, number] }>;
}

export const RISO_COLORS: ColorPalette;
export const CUSTOM_PALETTES: ColorPalette[];
export function createCustomPalette(name: string, colors: string[]): ColorPalette;
```

**3. image-processing Library**
```typescript
// libs/image-processing/src/index.ts
export interface ImageLayer {
  img: p5.Image;
  color: [number, number, number];
  blendMode: string;
  scale: number;
}

export function createMonochromeLayer(img: p5.Image, color: [number, number, number]): p5.Graphics;
export function loadImageCollection(paths: string[]): Promise<p5.Image[]>;
```

## Data Models

### Project Migration Model
```typescript
interface ProjectMigration {
  originalPath: string;
  targetPath: string;
  buildSystem: 'vite' | 'webpack' | 'parcel';
  dependencies: string[];
  preserveHistory: boolean;
}

interface SharedLibraryCandidate {
  name: string;
  sourceFiles: string[];
  targetLibrary: string;
  dependencies: string[];
}
```

### Workspace Configuration Model
```typescript
interface WorkspaceConfig {
  projects: Record<string, ProjectConfig>;
  defaultProject: string;
  generators: GeneratorConfig;
  tasksRunnerOptions: TaskRunnerConfig;
}
```

## Error Handling

### Migration Error Handling
- **Git History Preservation**: Fallback to manual history copying if subtree fails
- **Dependency Conflicts**: Version resolution strategy with peer dependency warnings
- **Build Failures**: Isolated project builds with clear error reporting
- **Port Conflicts**: Automatic port assignment for development servers

### Runtime Error Handling
- **Shared Library Failures**: Graceful degradation when shared code fails
- **Build Cache Issues**: Cache invalidation and rebuild strategies
- **Cross-Project Dependencies**: Clear dependency graph validation

## Testing Strategy

### Migration Testing
1. **Pre-migration**: Verify both projects build and run independently
2. **Post-migration**: Ensure all projects build and run in monorepo
3. **Shared Library Testing**: Unit tests for extracted common code
4. **Integration Testing**: Verify apps work with shared libraries

### Ongoing Testing
- **Affected Project Testing**: Only test projects affected by changes
- **Shared Library Testing**: Comprehensive testing for shared code
- **Build Testing**: Verify all deployment targets work correctly

## Deployment Strategy

### Individual Project Deployment
- **duo-chrome**: Maintain current deployment process (likely static hosting)
- **crude-collage-painter**: Preserve existing deployment method
- **Future Projects**: Consistent deployment patterns using Nx

### Shared Infrastructure
- **Build Artifacts**: Separate dist folders per project
- **Deployment Scripts**: Nx-powered deployment tasks
- **Environment Management**: Per-project environment configuration

## Implementation Phases

### Phase 1: Monorepo Setup
1. Create new `genart-monorepo` repository
2. Initialize Nx workspace with Vite support
3. Set up basic project structure

### Phase 2: Project Migration
1. Migrate `duo-chrome` with git history preservation
2. Migrate `crude-collage-painter` with git history preservation
3. Verify both projects work independently

### Phase 3: Shared Library Extraction
1. Identify common patterns between projects
2. Extract shared utilities into libraries
3. Update projects to use shared libraries

### Phase 4: Optimization and Documentation
1. Optimize build processes and caching
2. Create comprehensive documentation
3. Set up deployment pipelines