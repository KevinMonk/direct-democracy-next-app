Below is a set of instructions on transforming a ToDo app into a course-based platform. The instructions are broken down into **bite-sized tasks**.

---

## Overall Goal

- **Replace** all ToDo features (tables, routes, UI) with **Course**, **Section**, and **Lesson** entities and views.
- **Incrementally** implement and test each part in manageable chunks.

---

## Step-by-Step Roadmap
Here is my step-by-step roadmap:

1. Remove ToDo Artifacts
2. Create Course/Section/Lesson Models
3. Build Basic CRUD Routes
4. Front-End Pages & Layout
5. Incremental Improvements



### 1. Remove ToDo Artifacts

1. **Delete the ToDo Database Schema or Migrations**
   - **Task:** Locate and remove the migration or schema definitions for your `todos` table (or rename them if you want to preserve the code as reference).
   - **Action to be taken:** Remove the migration file for the `todos` table, and any references to `Todo` in my schema.

2. **Clean Out ToDo Routes & Controllers**
   - **Task:** Remove or comment out all endpoints that handle ToDo operations (e.g., `/api/todos`, `createTodo`, etc.).
   - **Action to be taken:** Search the routes folder for any `todo` routes and remove them. Remove any `todo`-related controller functions.

3. **Remove Front-End Todo Components**
   - **Task:** Delete or rename any front-end React (or Next.js) components that display or manipulate to-do items.
   - **Action to be taken:** Find any components named `TodoList`, `TodoForm`, or `TodoItem` and remove them from the codebase.

#### Checkpoint
- Your code should still **compile** but effectively do “nothing” related to todos.
- Once you confirm everything is gone, **commit** your changes.

---

### 2. Create Course/Section/Lesson Models

1. **Database Models/Migrations**
   - **Task:** Define `Course`, `Section`, and `Lesson` for Drizzle and create migrations if you use raw SQL.
   - **Action to be taken:**
     > “Add three new models for: Course, Section, and Lesson, each with fields for `title`, `description`, and any foreign keys needed (Section → Course, Lesson → Section). Generate and run migrations.”

2. **Relationships**
   - **Task:** Ensure the relationships between Course → Section → Lesson are properly set (one-to-many).
   - **Action to be taken:**
     > “In the schema, give `Section` a foreign key to `Course`, and `Lesson` a foreign key to `Section`. Make sure each Course can have multiple Sections, and each Section can have multiple Lessons.”

3. **Seed Data (Optional)**
   - **Task:** Create seed data for a sample course with a couple of sections and lessons to test.
   - **Action to be taken:**
     > “Write a seed script that creates one Course called ‘Building Full-Stack Apps with AI’ with 2 sections (‘Tech Stack’, ‘Your 1st App’), and each section has 2 sample lessons.”

#### Checkpoint
- Verify that the DB migrations run successfully and that you can see your new tables in your database.
- Optionally, **commit** your changes again.

---

### 3. Build Basic CRUD Routes

1. **Course Routes**
   - **Task:** Create routes to list courses, get a single course, create/update/delete courses.
   - **Action to be taken:**
     > “Set up a new `routes/courses.ts` file with:
       - `GET /courses`
       - `GET /courses/:id`
       - `POST /courses`
       - `PUT /courses/:id`
       - `DELETE /courses/:id`”

2. **Section Routes**
   - **Task:** Similarly, create routes to manage sections (preferably nested under `/courses/:courseId/sections`).
   - **Action to be taken:**
     > “In `routes/sections.ts`, create nested routes under `/courses/:courseId/sections` for listing, creating, etc. Also a route for `PUT` and `DELETE /sections/:id`.”

3. **Lesson Routes**
   - **Task:** Manage lessons, possibly nested under `/sections/:sectionId/lessons`.
   - **Action to be taken:**
     > “In `routes/lessons.ts`, implement a route to get, create, update, and delete lessons, referencing the parent `sectionId`.”

#### Checkpoint
- Test these endpoints using Postman, Thunder Client, or your browser (for GET requests).
- Once working, **commit** your changes.

---

### 4. Front-End Pages & Layout

1. **Course Listing Page**
   - **Task:** Create a page that lists all courses. For each course, show title and a link to a Course Detail page.
   - **Action to be taken:**
     > “Create a new Next.js page at `/courses` that fetches the list of courses from our new `GET /courses` endpoint. Display each course with a clickable title.”

2. **Course Detail Page**
   - **Task:** Display a course’s details (title, description, etc.) plus its sections.
   - **Action to be taken:**
     > “On `/courses/[id].tsx`, fetch the single course by ID (`GET /courses/:id`), then list each section’s title. Include a link to a new page for that section.”

3. **Section + Lessons**
   - **Task:** For each section, show a list of lessons, linking to a lesson detail page.
   - **Action to be taken:**
     > “Create a page `/sections/[id].tsx` that fetches the section’s details and lessons. Display each lesson’s title and link to `/lessons/[id]`.”

4. **Lesson Page**
   - **Task:** Show a single lesson’s content, possibly embedding a video or text.
   - **Action to be taken:**
     > “On `/lessons/[id].tsx`, fetch the lesson, display the title, video URL, and any content fields we have in the database.”

#### Checkpoint
- You should now be able to **navigate** from listing all courses → course detail → section → lessons.
- **Commit** your changes.

---

### 5. Incremental Improvements

1. **Styling & Layout**
   - Swap out or copy layout code from the existing ToDo app (if it had global styling) to achieve the dark theme.
   - **Action to be taken:**
     > “Apply the existing global `.css` or tailwind config from the todo app to the new course pages so they have a consistent dark theme.”

2. **Add/Edit Forms**
   - Decide if you want to allow creating/editing courses, sections, and lessons via UI forms (rather than manually calling the API).
   - **Action to be taken:**
     > “Create an ‘Add Course’ form on `/courses/new` that calls `POST /courses`. Similarly, add an ‘Edit Course’ page at `/courses/[id]/edit`.”

3. **User Progress Tracking** (Optional)
   - If you want to track which lessons a user has completed, add a `UserLessonProgress` table, or store a `completedAt` date on the lesson.
   - **Action to be taken:**
     > “Add a new model `UserLessonProgress` with fields `userId`, `lessonId`, `completedAt`. Create a new endpoint to update that record.”

4. **Deployment**
   - Finally, deploy your new course app to Vercel.
   - **Action to be taken:**
     > “Set up the environment variables in the Vercel dashboard, run migrations in production, and verify the site is live.”

---

## Feeding to an AI Agent Incrementally

- **Best Practice**: Don’t dump this entire plan into the AI at once. Instead, tackle one **Task** or **Step** at a time.
- **Example Flow**:
  1. “AI: Remove all todo references from the code. Search for any file/folder with `todo`. Delete or rename them.”
  2. “AI: Create a new `Course` model in `schema.prisma`. Migrate the database.”
  3. “AI: Make a GET /courses endpoint and confirm it returns a placeholder JSON.”
  4. …and so on.
- **Verify** each step manually or by testing before moving on.

---

### Final Thoughts

By breaking everything into **discrete tasks**, you’ll have an easier time guiding your AI assistant (or your own workflow) through each step. This approach also makes it straightforward to **test** and **commit** after each milestone, ensuring you always have a stable version to fall back on if something goes wrong.

Good luck, and enjoy turning your simple ToDo app into a full-fledged course platform!
