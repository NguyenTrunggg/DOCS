# Use Case Diagrams - Recipe Management System

## Overview

This directory contains 12 PlantUML use case diagrams for the Recipe Management System:
- **1 System Context Diagram**: High-level overview of all 11 modules
- **11 Detailed Module Diagrams**: Individual use case diagrams for each functional module

## File Structure

```
UseCase/
├── 00_System_Context.puml                   # Overall system context
├── 01_User_Management.puml                  # UC-A1: User Management (Admin)
├── 02_Recipe_Management.puml                # UC-A2: Recipe Management (Admin)
├── 03_Category_Ingredient_Management.puml   # UC-A3: Category & Ingredient (Admin)
├── 04_Admin_Account_Management.puml         # UC-A4: Admin Account Management (Super Admin)
├── 05_Analytics_Reporting.puml              # UC-A5: Analytics & Reporting (Admin)
├── 06_Authentication_Profile.puml           # UC1: Authentication & Profile (User)
├── 07_Recipe_Discovery.puml                 # UC2: Recipe Discovery (User/Guest)
├── 08_Personal_Recipe_Management.puml       # UC3: Personal Recipe Management (User)
├── 09_Social_Interaction.puml               # UC4: Social Interaction (User/Guest)
├── 10_Virtual_Fridge.puml                   # UC5: Virtual Fridge (User)
├── 11_Meal_Planning.puml                    # UC6: Meal Planning (User)
└── README.md                                # This file
```

## How to Generate Diagrams

### Option 1: Online PlantUML Server (Recommended for Quick View)

1. Visit [PlantUML Online Server](http://www.plantuml.com/plantuml/uml/)
2. Copy the contents of any `.puml` file
3. Paste into the editor
4. Click "Submit" to generate diagram
5. Download as PNG or SVG

### Option 2: VS Code Extension

1. Install [PlantUML Extension](https://marketplace.visualstudio.com/items?itemName=jebbs.plantuml)
2. Open any `.puml` file
3. Press `Alt+D` to preview
4. Export to PNG/SVG using the extension

### Option 3: Command Line with Node.js

```bash
# Install PlantUML globally
npm install -g node-plantuml

# Generate all diagrams as PNG
cd DOCS/UML/UseCase
plantuml *.puml

# Generate specific diagram
plantuml 00_System_Context.puml

# Generate with custom format (SVG)
plantuml -tsvg *.puml
```

### Option 4: Java Installation (Full Features)

```bash
# Download PlantUML JAR
wget https://sourceforge.net/projects/plantuml/files/plantuml.jar/download -O plantuml.jar

# Generate diagrams
java -jar plantuml.jar DOCS/UML/UseCase/*.puml
```

## Diagram Key

### Color Coding

- **Red (#FFB6C1)**: HIGH Priority use cases
- **Yellow (#FFE4B5)**: MEDIUM Priority use cases
- **Blue (#E0F0FF)**: LOW Priority use cases
- **Orange (#FFE4CC)**: Admin subsystem packages
- **Light Blue (#CCE5FF)**: User subsystem packages

### Relationship Types

- `-->` : Association (Actor to Use Case)
- `..>` with `<<include>>` : Required functionality
- `..>` with `<<extend>>` : Optional functionality

### Actor Colors

- **Super Admin**: Red (#FF6B6B)
- **Admin**: Orange (#FFA500)
- **User**: Turquoise (#4ECDC4)
- **Guest**: Gray (#95A5A6)

## Diagram Descriptions

### 00_System_Context.puml
**Overview diagram** showing all 11 modules and 4 actor types. Provides high-level architecture of the entire system.

### 01-05: Admin Modules (UC-A1 to UC-A5)

**01_User_Management.puml**
- Manage user accounts
- Disable/enable functionality
- Full audit trail

**02_Recipe_Management.puml**
- Manage system recipes
- Approve/reject user submissions
- Approval workflow

**03_Category_Ingredient_Management.puml**
- Manage categories (4 use cases)
- Manage ingredients (4 use cases)
- Data standardization

**04_Admin_Account_Management.puml**
- Super Admin only
- RBAC/ABAC implementation (HIGH PRIORITY)
- Permission management

**05_Analytics_Reporting.puml**
- Dashboard with multiple chart types
- Export reports (PDF/CSV)
- HIGH PRIORITY module

### 06-11: User Modules (UC1 to UC6)

**06_Authentication_Profile.puml**
- Registration, login, logout
- OAuth integration
- Password management

**07_Recipe_Discovery.puml**
- Smart search algorithms (HIGH PRIORITY)
- AI recipe generation (HIGH PRIORITY)
- Filter and sort capabilities

**08_Personal_Recipe_Management.puml**
- Create/edit/delete recipes
- Approval workflow
- 10 recipes/day limit

**09_Social_Interaction.puml**
- Favorites system
- Ratings and comments
- Multi-platform sharing

**10_Virtual_Fridge.puml**
- Smart inventory management
- Expiry tracking
- Unit conversion

**11_Meal_Planning.puml**
- Meal plan creation
- Calendar views
- Shopping list generation with integration

## Use in Documentation

These diagrams can be:
1. Embedded in PDF reports
2. Included in presentation slides
3. Published in online documentation
4. Used for stakeholder communication
5. Referenced in development specifications

## Best Practices

1. **Always use English** for consistency with SD, CD, ERD
2. **Update diagrams** when use cases change
3. **Keep relationships simple** to maintain readability
4. **Use color coding** for quick priority identification
5. **Include detailed notes** for complex use cases

## Related Documentation

- Use Cases: `/DOCS/UC/`
- Sequence Diagrams: `/DOCS/SD/`
- Class Diagrams: `/DOCS/CD/`
- ERD: `/DOCS/ERD/`
- API Documentation: `/DOCS/API/`

## Maintenance

When updating diagrams:
1. Keep naming conventions consistent
2. Update priority colors if needed
3. Maintain actor color scheme
4. Document any structural changes
5. Update this README if adding new files

## Support

For issues with diagram generation or questions about the use cases, refer to the main system documentation in `/DOCS/UC_SUMMARY_REPORT.md`.




