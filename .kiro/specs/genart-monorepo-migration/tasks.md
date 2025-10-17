# Implementation Plan

- [x] 1. Set up new monorepo repository and Nx workspace
  - Create new `genart-monorepo` repository on GitHub
  - Initialize Nx workspace with Vite preset
  - Configure pnpm workspace settings
  - Set up basic project structure (apps/, libs/, tools/, docs/)
  - _Requirements: 1.1, 5.1, 5.2, 6.1_

- [x] 2. Migrate duo-chrome project with history preservation
  - [x] 2.1 Prepare duo-chrome for migration
    - Document current build and dev processes
    - Create backup of current repository state
    - _Requirements: 2.1, 3.1_
  
  - [x] 2.2 Migrate duo-chrome using git subtree
    - Use git subtree to preserve commit history
    - Move project to apps/duo-chrome/ directory
    - _Requirements: 2.1, 2.2_
  
  - [x] 2.3 Configure duo-chrome as Nx project
    - Create project.json with Vite executors
    - Update vite.config.js for monorepo context
    - Configure development server port (5173 or auto-assign)
    - _Requirements: 3.1, 3.3, 5.2_
  
  - [x] 2.4 Verify duo-chrome functionality
    - Test development server startup
    - Verify build process produces correct artifacts
    - Test all existing keyboard controls and features
    - _Requirements: 3.1, 3.2, 3.4_

- [x] 3. Analyze and migrate crude-collage-painter project
  - [x] 3.1 Analyze crude-collage-painter structure
    - Examine package.json and build configuration
    - Document current tech stack and dependencies
    - Identify potential conflicts with duo-chrome
    - _Requirements: 1.1, 3.2_
  
  - [x] 3.2 Migrate crude-collage-painter with history
    - Use git subtree to preserve commit history
    - Move project to apps/crude-collage-painter/ directory
    - _Requirements: 2.1, 2.2_
  
  - [x] 3.3 Configure crude-collage-painter as Nx project
    - Create appropriate project.json configuration
    - Adapt build system to work within monorepo
    - Configure unique development server port
    - _Requirements: 3.2, 3.3, 5.2_
  
  - [x] 3.4 Verify crude-collage-painter functionality
    - Test development and build processes
    - Ensure no conflicts with duo-chrome
    - _Requirements: 3.2, 3.4_

- [x] 4. Identify and extract shared code patterns
  - [x] 4.1 Analyze common patterns between projects
    - Compare p5.js setup and utility functions
    - Identify shared image processing logic
    - Document color palette and UI patterns
    - _Requirements: 4.1_
  
  - [x] 4.2 Create p5-utils shared library
    - Extract common p5.js setup and utility functions
    - Create TypeScript interfaces for sketch patterns
    - Implement getRandomUniqueItem and similar utilities
    - _Requirements: 4.1, 4.2_
  
  - [x] 4.3 Create color-palettes shared library
    - Extract RISO color definitions from duo-chrome
    - Create reusable color palette interfaces
    - Add palette management utilities
    - _Requirements: 4.1, 4.2_
  
  - [ ]* 4.4 Write unit tests for shared libraries
    - Create tests for p5-utils functions
    - Test color palette utilities
    - Set up Jest or Vitest for library testing
    - _Requirements: 4.2_

- [ ] 5. Update projects to use shared libraries
  - [ ] 5.1 Refactor duo-chrome to use shared libraries
    - Replace local utilities with shared p5-utils
    - Update color imports to use color-palettes library
    - Update import paths and dependencies
    - _Requirements: 4.2, 4.3_
  
  - [ ] 5.2 Refactor crude-collage-painter to use shared libraries
    - Identify applicable shared utilities
    - Update imports and remove duplicate code
    - _Requirements: 4.2, 4.3_
  
  - [ ] 5.3 Verify projects work with shared dependencies
    - Test both applications with shared libraries
    - Ensure no functionality regressions
    - Verify build processes work correctly
    - _Requirements: 4.3_

- [ ] 6. Optimize monorepo configuration and tooling
  - [ ] 6.1 Configure Nx caching and task dependencies
    - Set up build caching for faster rebuilds
    - Configure task dependencies between projects and libraries
    - Enable affected project detection
    - _Requirements: 5.2, 6.3_
  
  - [ ] 6.2 Standardize code quality tools
    - Configure ESLint for entire workspace
    - Set up StandardJS rules consistently
    - Configure Prettier if needed
    - _Requirements: 5.1_
  
  - [ ] 6.3 Create deployment configurations
    - Set up build targets for production deployment
    - Configure separate output directories
    - Document deployment processes for each project
    - _Requirements: 3.4, 5.4_

- [ ] 7. Create comprehensive documentation
  - [ ] 7.1 Create monorepo README
    - Document project structure and purpose
    - Provide quick start guide for development
    - List all applications and libraries
    - _Requirements: 6.4_
  
  - [ ] 7.2 Document shared library usage
    - Create API documentation for shared libraries
    - Provide usage examples and best practices
    - Document how to add new shared utilities
    - _Requirements: 4.3, 6.4_
  
  - [ ] 7.3 Create project addition guide
    - Document how to add new creative coding projects
    - Provide templates and conventions
    - Explain shared library integration process
    - _Requirements: 6.2, 6.4_

- [ ] 8. Final verification and cleanup
  - [ ] 8.1 Test complete monorepo functionality
    - Verify all projects build and run independently
    - Test shared library changes affect dependent projects
    - Validate deployment processes
    - _Requirements: 1.1, 3.1, 3.2, 4.3_
  
  - [ ] 8.2 Archive original repositories
    - Mark original duo-chrome repository as archived
    - Update repository descriptions with monorepo links
    - Create migration documentation
    - _Requirements: 2.3_
  
  - [ ] 8.3 Set up continuous integration
    - Configure GitHub Actions for monorepo
    - Set up automated testing and building
    - Configure deployment pipelines
    - _Requirements: 5.2, 6.3_