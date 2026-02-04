# ToodooL Architecture

## Overview

ToodooL follows a **class-based, modular architecture** with clear separation of concerns. The application is built using vanilla JavaScript with a focus on maintainability, simplicity, and performance.

The architecture separates the application into three primary concerns:
- **Data Model** (Todo.js)
- **Data Persistence** (Storage.js)
- **User Interface** (UI.js)

## Architecture Diagram

```
┌─────────────────────────────────────────┐
│         User Interface (UI.js)          │
│  - Event Listeners                      │
│  - DOM Manipulation                     │
│  - Modal Management                     │
│  - Theme Toggle                         │
└──────────────┬──────────────────────────┘
               │
               ├──► Creates ──► Todo Class
               │
               └──► Uses ──────► Storage Class
                                    │
                                    └──► Browser LocalStorage
```

## Module Breakdown

### 1. Todo.js - Data Model

**Purpose**: Defines the data structure for todos

**Class**: `Todo`

**Properties**:
```javascript
{
  id: number,              // Auto-incrementing unique identifier
  title: string,           // Todo list title
  todoItems: Array,        // Array of todo items with completion status
  dateCreated: string      // Formatted creation date/time
}
```

**Structure of todoItems**:
```javascript
[
  {
    todoItem: string,    // Description of the task
    isComplete: boolean  // Completion status
  }
  // ... more items
]
```

**Key Features**:
- Static `idCounter` for auto-incrementing IDs
- Pulls highest ID from Storage on initialization
- Immutable data structure (no modification methods)
- Constructor validates inputs

**Example Usage**:
```javascript
const newTodo = new Todo(
  'Shopping List',
  [
    { todoItem: 'Buy milk', isComplete: false },
    { todoItem: 'Buy eggs', isComplete: true }
  ],
  '2024-02-04 10:30'
);

console.log(newTodo.id);        // 1
console.log(newTodo.title);     // 'Shopping List'
console.log(newTodo.todoItems); // [...]
```

---

### 2. Storage.js - Data Persistence Layer

**Purpose**: Abstracts browser localStorage operations with error handling

**Class**: `Storage` (static methods only)

**Methods**:

#### `storeTodo(todo)`
- **Description**: Save a todo object to localStorage
- **Parameters**: `todo` (Todo instance)
- **Returns**: undefined
- **Error Handling**: Validates todo.id before storing
- **Example**:
  ```javascript
  Storage.storeTodo(newTodo);
  // Sets localStorage[newTodo.id] = JSON.stringify(newTodo)
  ```

#### `retrieveAllTodos()`
- **Description**: Load all todos from localStorage
- **Parameters**: None
- **Returns**: Object with todo IDs as keys, todo objects as values
- **Error Handling**: try-catch for JSON parsing, returns empty object if no data
- **Example**:
  ```javascript
  const todos = Storage.retrieveAllTodos();
  // {
  //   "1": { id: 1, title: "Shopping", ... },
  //   "2": { id: 2, title: "Work", ... }
  // }
  ```

#### `retrieveTodoById(id)`
- **Description**: Fetch a specific todo by ID
- **Parameters**: `id` (number)
- **Returns**: Todo object or null if not found
- **Error Handling**: Validates ID, try-catch for JSON parsing
- **Example**:
  ```javascript
  const todo = Storage.retrieveTodoById(1);
  ```

#### `getHighestTodoId()`
- **Description**: Calculate the highest ID currently in storage
- **Parameters**: None
- **Returns**: Highest ID number (0 if no todos)
- **Error Handling**: Filters out non-numeric keys
- **Example**:
  ```javascript
  const nextId = Storage.getHighestTodoId() + 1;
  ```

#### `removeTodo(id)`
- **Description**: Delete a todo from localStorage
- **Parameters**: `id` (number)
- **Returns**: undefined
- **Error Handling**: Validates ID before removal
- **Example**:
  ```javascript
  Storage.removeTodo(1);
  // localStorage.removeItem('1')
  ```

**Error Handling**:
- Try-catch blocks for JSON parsing
- Console error logging for debugging
- Validation of inputs before operations
- Safe handling of malformed data

**localStorage Schema**:
```json
{
  "1": {
    "id": 1,
    "title": "Shopping List",
    "todoItems": [
      {"todoItem": "Buy milk", "isComplete": false},
      {"todoItem": "Buy eggs", "isComplete": true}
    ],
    "dateCreated": "2024-02-04 10:30"
  },
  "2": {
    "id": 2,
    "title": "Work Tasks",
    "todoItems": [...],
    "dateCreated": "2024-02-04 11:15"
  }
}
```

---

### 3. UI.js - View Controller

**Purpose**: Manages all DOM manipulation, event handling, and user interactions

**Class**: `UI` (static methods only)

**Key Methods**:

#### `init()`
- **Description**: Initialize application on page load
- **Called by**: index.js on DOMContentLoaded
- **Actions**:
  1. Load and display stored todos
  2. Set up form toggle listeners
  3. Set up theme toggle listener
  4. Clear input fields
  5. Set up delete item listeners
  6. Set up save button listener

#### `displayRetrieveTodos()`
- **Description**: Load todos from storage and render them
- **Called by**: init()
- **Process**:
  1. Retrieve all todos from Storage
  2. Check if empty (no todos to display)
  3. Convert todo object to array
  4. Iterate and display each todo

#### `addTodo()`
- **Description**: Create and save a new todo from form input
- **Event**: Save button click
- **Process**:
  1. Get form input (title and items)
  2. Get formatted date
  3. Create new Todo instance
  4. Display on page
  5. Save to storage
  6. Clear input fields

#### `displayTodoOnPage(todo)`
- **Description**: Render a single todo as a card on the page
- **Parameters**: `todo` (Todo object)
- **Process**:
  1. Create div with class "display-card"
  2. Build list HTML from todoItems array
  3. Apply strike-through styling for completed items
  4. Set innerHTML with title, items list, and date
  5. Append to display container

#### `toggleTodoForm()`
- **Description**: Show/hide the todo input form modal
- **Event**: "+ New Todo" button, close button, save button
- **Process**:
  1. Toggle "overlay" class on overlay div
  2. Toggle "hidden" class on form wrapper
  3. Clear inputs when opening/closing
  4. Reset delete listeners

#### `toggleTheme()`
- **Description**: Switch between light and dark mode
- **Event**: Theme icon click
- **Process**:
  1. Toggle "light-mode" and "dark-mode" classes on body
  2. Update icon text to reflect new theme
  3. CSS variables automatically apply theme colors

#### `createNewItemRow()`
- **Description**: Dynamically add a new input row for todo items
- **Event**: User types in last item input (when length === 1)
- **Process**:
  1. Create new div with class "todo-item-row"
  2. Add checkbox, text input, delete button
  3. Attach input listener to create next row when typing
  4. Attach delete listener to new row
  5. Append to container

#### `deleteItemRow(event)`
- **Description**: Remove an item row from the form
- **Event**: Delete icon click
- **Process**:
  1. Get parent element (todo-item-row)
  2. Remove from DOM

---

## Data Flow

### Creating a Todo

```
User Input (Form Submission)
    ↓
UI.addTodo() triggered
    ↓
Collect form data:
- title from #todo-title
- items from .todo-item-row elements
- date via UI.getFormattedDate()
    ↓
new Todo(title, todoItems, dateCreated)
    ↓
UI.displayTodoOnPage(todo)
    ↓ (simultaneously)
Storage.storeTodo(todo)
    ↓ (simultaneously)
UI.clearInputFields()
    ↓
User sees new todo card on page
User data persists in localStorage
```

### Loading Todos on Page Refresh

```
Page Load
    ↓
index.js: DOMContentLoaded event
    ↓
UI.init()
    ↓
UI.displayRetrieveTodos()
    ↓
Storage.retrieveAllTodos()
    ↓
For each todo:
    UI.displayTodoOnPage(todo)
    ↓
User sees previously saved todos
```

### Toggling Theme

```
User clicks theme icon
    ↓
UI.toggleTheme() triggered
    ↓
Toggle body classes
Update icon text
    ↓
CSS Variables apply new colors
    ↓
User sees theme change
(No data persistence needed - handled by body class)
```

---

## Build Process

### Development Build

```
webpack.config.js
    ↓
Entry: ./src/index.js
    ↓
Modules:
- src/modules/Todo.js
- src/modules/Storage.js
- src/modules/UI.js
- src/index.js (imports all modules)
    ↓
CSS Processing:
- src/styles/main.css
- Extracted via MiniCssExtractPlugin
    ↓
Output:
- dist/bundle.js (unoptimized)
- dist/main.css
- dist/index.html (from template.html)
    ↓
Dev Server:
- Hot module reload enabled
- Source maps for debugging
- Runs on localhost:8080
```

### Production Build

```
webpack.prod.js
    ↓
Entry: ./src/index.js
    ↓
Optimization:
- Mode: production
- Code splitting: vendor chunks separated
- Minification: HTML, CSS, JavaScript
- Content hashing: [name].[contenthash].js
    ↓
Output:
- dist/main.[contenthash].js (minified)
- dist/vendors~main.[contenthash].js (vendor code)
- dist/main.[contenthash].css (minified)
- dist/index.html (minified)
    ↓
Benefits:
- Cache busting via content hash
- Smaller bundle size (~30-40% reduction)
- Optimized for production delivery
```

---

## Design Patterns Used

### 1. Static Class Pattern
```javascript
export class Storage {
  static storeTodo(todo) { ... }
  static retrieveAllTodos() { ... }
  // No instance needed - all methods are static
}

// Usage
Storage.storeTodo(todo);  // Call directly on class
```

**Rationale**: Singleton-like behavior for utility functions, no instance needed.

### 2. Module Pattern
Each file (Todo.js, Storage.js, UI.js) is a module with private scope.

**Benefits**:
- Prevents global namespace pollution
- Clear separation of concerns
- Easy to test individual modules
- Maintainable code organization

### 3. Factory Pattern
```javascript
// Todo class acts as a factory for creating todo objects
const todo1 = new Todo('Shopping', [...], '2024-02-04 10:30');
const todo2 = new Todo('Work', [...], '2024-02-04 11:15');
```

**Rationale**: Consistent creation of todo objects with validation.

### 4. Observer Pattern (Event Listeners)
```javascript
document.addEventListener('DOMContentLoaded', () => UI.init());
saveBtn.addEventListener('click', UI.saveTodoHandler);
```

**Rationale**: React to DOM events without tight coupling.

---

## Data Types and Structures

### Todo Object
```javascript
{
  id: Number,
  title: String,
  todoItems: Array<{todoItem: String, isComplete: Boolean}>,
  dateCreated: String
}
```

### localStorage Format
- **Keys**: String representation of todo ID
- **Values**: JSON stringified Todo objects

### UI Classes (CSS)
- `.display-card`: Individual todo card styling
- `.light-mode` / `.dark-mode`: Theme classes on body
- `.todo-item-row`: Single item input row
- `.hidden`: Hide form modal
- `.overlay`: Show modal backdrop

---

## Performance Considerations

### Optimizations
1. **Selective DOM Updates**: Only affected elements updated
2. **Event Delegation**: Listeners added once, handle multiple elements
3. **Efficient Storage**: Direct localStorage key lookup for IDs
4. **Minimal Dependencies**: Only date-fns for date formatting
5. **Code Splitting**: Webpack separates vendor code in production

### Scalability Limits
- **localStorage Limit**: 5-10MB per browser (sufficient for ~10,000 todos)
- **DOM Rendering**: Linear performance with todo count
- **Memory Usage**: Minimal, scales linearly

### Trade-offs
- **Simplicity** vs. **Performance**: Chose simplicity for local app
- **No Backend** vs. **No Sync**: No data sync across devices
- **Vanilla JS** vs. **Framework**: No framework overhead, more code

---

## Error Handling Strategy

### Defensive Practices
1. **Input Validation**: Check for null/undefined before operations
2. **Try-Catch Blocks**: Wrap JSON parsing in Storage
3. **Console Logging**: Errors logged for debugging
4. **Graceful Degradation**: App continues if one operation fails

### Example
```javascript
static retrieveTodoById(id) {
  if (!id) {
    console.error('Invalid ID');
    return null;
  }
  const jsonString = localStorage.getItem(id);
  try {
    return JSON.parse(jsonString);
  } catch (e) {
    console.error(`Error parsing JSON for ID ${id}: ${e}`);
    return null;
  }
}
```

---

## Future Improvements

### Short Term
1. Add edit todo functionality
2. Add delete todo card functionality
3. Add input validation (prevent empty titles)
4. Implement XSS protection

### Medium Term
1. Add unit tests (Jest)
2. Add TypeScript for type safety
3. Implement service worker for offline support
4. Add local caching layer

### Long Term
1. Cloud sync backend
2. User authentication
3. Collaborative features
4. PWA support
5. Mobile app (React Native)

---

## Testing Architecture

### Unit Test Areas
- `Todo.js`: Constructor, ID generation, data structure
- `Storage.js`: CRUD operations, error handling, data persistence
- `UI.js`: Event handlers, DOM updates, form validation

### Integration Test Areas
- Todo creation → Storage → Display flow
- Theme toggle persistence
- Form validation and clearing

### Manual Testing Areas
- Cross-browser compatibility
- LocalStorage limits
- Performance with many todos
- Mobile responsiveness

---

## Deployment Architecture

### Development
```
webpack serve (hot reload) → localhost:8080
```

### Production
```
npm run build
  ↓
webpack.prod.js compiles and optimizes
  ↓
dist/ directory created with optimized files
  ↓
npm run deploy
  ↓
gh-pages deploys dist/ to GitHub Pages
  ↓
https://theankushgautam.github.io/Toodool/
```

---

## Summary

ToodooL's architecture prioritizes:
- **Simplicity**: Easy to understand and modify
- **Maintainability**: Clear separation of concerns
- **Performance**: Minimal overhead, direct DOM manipulation
- **Reliability**: Error handling and data persistence
- **Scalability**: Efficient up to thousands of todos

The modular design allows easy addition of features without breaking existing functionality.
