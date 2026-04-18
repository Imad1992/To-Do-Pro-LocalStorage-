To‑Do Pro (LocalStorage)
A modern single‑file To���Do web app built with HTML + CSS + Vanilla JavaScript.
It stores your tasks in LocalStorage, so everything stays saved even after you refresh or close the browser.

This project is designed to be easy to upload to GitHub and run instantly (no build tools, no frameworks).

Contents
Features
How it works
Data model
LocalStorage persistence
Rendering & UI updates
Filtering, search, sorting
Preview panel (right side)
Edit modal
Bulk actions
Export / Import
How to run
Deploy to GitHub Pages
Customization
Troubleshooting
Roadmap ideas
Features
Core task features
Add tasks with:
Title
Due date
Priority: low / normal / high
Tags (comma-separated)
Mark tasks complete (checkbox)
Delete tasks
Edit tasks (modal with notes + tags + due date + priority)
Productivity features
Search across title, notes, and tags
Filters:
All
Active
Completed
Due Today
Overdue
Sorting:
Manual (drag reorder supported)
Due date
Priority
Newest
Oldest
Bulk actions
Select visible tasks
Mark selected done
Delete selected
UI features
Beautiful “glass” style UI with responsive layout
Sticky right-side preview panel (desktop)
Click a task to preview details
Shows priority, due date, tags, created time, and notes
Tag shortcuts panel (click a tag chip to filter)
Toast notifications for quick feedback
Persistence & portability
Uses LocalStorage (no backend needed)
Export tasks to JSON file
Import tasks from JSON file (restores tasks)
How it works
The whole app is inside a single index.html file:

<style> contains all CSS
<script> contains all app logic
Data model
Each task is stored as an object like:

{
  id: "uuid",
  title: "Pay rent",
  done: false,
  priority: "high" | "normal" | "low",
  dueDate: "YYYY-MM-DD" | null,
  tags: ["work", "urgent"],
  notes: "Optional long text",
  createdAt: 1712345678901,
  selected: false
}
All tasks are stored in an array:

let todos = [ /* task objects */ ];
LocalStorage persistence
Tasks are saved to the browser using:

Key: todo_pro_v1
When the page loads:

The script reads localStorage.getItem("todo_pro_v1")
Parses JSON
Validates / normalizes fields
Loads tasks into memory (todos)
After every change (add/edit/toggle/delete/reorder/import):

the app calls:
localStorage.setItem("todo_pro_v1", JSON.stringify(todos));
Rendering & UI updates
The UI is generated dynamically from the todos array.

A render() function:

Applies filter + search + sort
Builds the list HTML for the visible tasks
Attaches event listeners (toggle, delete, edit, select, preview)
Updates:
stats chips
tag shortcuts
preview panel (if a task is selected)
Whenever something changes, render() is called so the UI stays in sync.

Filtering, search, sorting
The visible task list is computed by:

Filter selection (active/done/today/overdue)
Optional tag filter (click tag chip on the right)
Search text (title/notes/tags)
Sort selection (manual/due/priority/newest/oldest)
Manual sorting keeps the original array order.
When manual is selected, drag-and-drop reorder is enabled.

Preview panel (right side)
The right panel shows a “preview” of a selected task.

How it works:

Each task’s content area has data-preview="<taskId>"
Clicking it runs:
setPreview(taskId)
That stores previewId and calls renderPreview() to update:

title
status (Active/Completed)
meta (priority, due, tags, created time)
notes (or a placeholder message)
Important: On smaller screens the layout becomes 1 column, so the right panel moves below the list.
Make your browser wider than ~760px to keep it on the right.

Edit modal
Click Edit on any task to open a modal.

The modal allows editing:

title
due date
priority
tags
notes
On Save:

the task is updated in todos
changes are persisted to LocalStorage
the UI re-renders
You can close the modal by:

clicking Close/Cancel
clicking outside the modal
pressing Esc
Bulk actions
Tasks can be “selected” for bulk operations.

Select visible marks tasks currently visible in the list as selected
Mark done completes all selected tasks
Delete removes all selected tasks
These actions update the selected property per task.

Export / Import
Export
Exports a JSON file named todo-pro-export.json
Format:
{
  "version": 1,
  "todos": [ ...tasks ]
}
Import
Choose a JSON file exported earlier
The app reads it, validates, normalizes, and replaces the current tasks
Then saves to LocalStorage and re-renders
How to run
Option A: Open directly
Just open index.html in your browser.

Option B: Run a local server (recommended)
Python

python -m http.server 8000
Then open:

http://localhost:8000
Node

npx serve .
Deploy to GitHub Pages
Create a GitHub repo (example: todo-pro)
Add these files:
index.html
README.md
Push to main
On GitHub:
Settings → Pages
Source: Deploy from a branch
Branch: main, folder: / (root)
Wait for GitHub to publish your site.
Customization
Change LocalStorage key
Search for:

const STORAGE_KEY = "todo_pro_v1";
Change it to a new string if you want.

Default sorting / filtering
Set default selected values in the HTML <select> elements:

filter
sort
Add new priorities or tags behavior
Update the priorityRank() function and related CSS badges.
Troubleshooting
“Preview panel is not on the right”
That’s expected on small screens:

The CSS uses a responsive breakpoint:
if width <= ~760px, the layout becomes 1 column
the preview panel will move below the task list
Fix:

Make the window wider, or increase the breakpoint in CSS:
@media (max-width: 760px){
  .grid{ grid-template-columns: 1fr; }
}
“Tag shortcuts are empty”
They only appear after you add tasks with tags](#)
