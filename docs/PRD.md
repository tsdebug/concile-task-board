# Trello-Style Task Board

## 1. What are we building?

We are building a small Trello-style task board for organizing study or project work.

The goal is not to build a complete Trello replacement. The goal is to build one small,
useful application first with Convex, and then move the same application to Concile using
Concile's migration feature.

This gives us a simple project where we can learn:

- how a realtime backend works;
- how queries and mutations are used;
- how a frontend talks to the backend; and
- how much of an existing Convex project can move to Concile automatically.

We should keep the first version small enough to understand from beginning to end.

## 2. Who is this for?

This app is for one person or a small study group that wants to keep track of work.

For the first version, we will not build user accounts or permissions. Anyone using the
local app will work with the same board. This keeps our attention on the task board and
the Convex-to-Concile migration.

## 3. The main idea

The board has a few columns. Each column contains cards.

For example:

```text
To Do          In Progress          Done
---------      ------------          ----
Read docs      Build schema          Finish README
Plan UI        Test mutations
```

Users can add cards, update them, move them between columns, and remove them.

When the board is open in two browser tabs, a change in one tab should appear in the
other tab without manually refreshing the page. This realtime behavior is an important
part of the project.

## 4. First-release features

### 4.1 View the board

When the app opens, the user should see:

- the board name;
- all columns;
- the cards inside each column; and
- a helpful message when a column has no cards.

The board should show a loading message while data is being fetched and a clear error
message if the data cannot be loaded.

### 4.2 Create a column

The user should be able to add a new column by entering a name.

Examples:

- Ideas
- Review
- Blocked

The column name cannot be empty.

### 4.3 Create a card

The user should be able to add a card to a column.

Each card should have:

- a title;
- an optional description; and
- its current column.

The title is required. The app should not create a card when the title is empty.

### 4.4 Edit a card

The user should be able to change a card's title and description.

The edit experience can be simple. An inline form or a small dialog is enough for the
first version.

### 4.5 Move a card

The user should be able to move a card from one column to another.

For the first working version, buttons or a select menu are acceptable. A polished
drag-and-drop experience is optional and should come later, after the basic behavior
works correctly.

### 4.6 Mark a card complete

The user should be able to mark a card as complete or incomplete.

The card should make its completed state easy to recognize, for example with a checkbox,
different text styling, or both.

### 4.7 Delete a card

The user should be able to delete a card.

The action should be clear and should not happen accidentally. A confirmation step may
be added if the final interaction needs one.

### 4.8 Realtime updates

If the same board is open in two browser tabs:

1. The user creates or changes a card in the first tab.
2. The second tab receives the change automatically.
3. The user does not need to refresh the second tab.

We will test this behavior first with Convex and then again with Concile.

## 5. Data we need

The first version will need three kinds of records:

### Board

- name
- creation time

### Column

- board it belongs to
- name
- position
- creation time

### Card

- column it belongs to
- title
- description
- position
- completed state
- creation time
- last updated time

The exact database field names and types will be decided during the schema chunk. The
important thing is that cards belong to columns and columns belong to the board.

## 6. A simple user journey

Here is the normal flow we want to support:

1. The user opens the app.
2. The board appears with a few starter columns.
3. The user adds a card to the To Do column.
4. The user edits the card with more details.
5. The user moves the card to In Progress.
6. The user marks the card complete.
7. The user moves it to Done.
8. The user opens another browser tab and confirms that changes stay in sync.

## 7. What is not part of the first version?

We will leave these features out initially:

- user registration and login;
- multiple boards;
- team invitations and permissions;
- comments;
- file uploads;
- labels and advanced filters;
- due dates and reminders;
- notifications;
- search;
- offline support;
- production deployment;
- a full drag-and-drop interaction;
- performance benchmarking.

These are possible future improvements, but they would make the first migration
experiment harder to understand.

## 8. Technical direction

We will build the same application in two stages.

### Stage 1: Convex

We will:

- create a React and TypeScript frontend;
- create the backend functions in a `convex/` folder;
- define the schema;
- add queries for reading the board;
- add mutations for changing cards and columns;
- connect the frontend using Convex's React client; and
- verify the app before migration.

### Stage 2: Concile

We will:

- save a clean Convex checkpoint;
- run Concile's migration command in dry-run mode first;
- read the generated migration report;
- run the actual migration;
- fix only the items that require manual attention;
- run the app with Concile locally; and
- repeat the same user and realtime tests.

The application should behave the same after migration unless a difference is clearly
explained and documented.

## 9. Migration experiment goals

The migration is part of the product exercise, not an afterthought.

We want to answer these questions honestly:

- Which files and imports does Concile migrate automatically?
- Does the `convex/` backend directory move to `concile/`?
- Does the existing schema remain easy to understand?
- Do the React hooks need large changes?
- Which Convex patterns require manual edits?
- Does the migrated app still support realtime updates?
- How much time does the manual work take?
- What is easier or harder when running the backend locally?

We will not claim that migration is completely automatic. We will record the actual
report, code changes, and test results.

## 10. Completion checklist

We can call the project complete when:

- the Convex version can display the board;
- users can create, edit, move, complete, and delete cards;
- users can add columns;
- the Convex version passes the agreed test cases;
- realtime updates work in two browser tabs;
- the project has a clean Convex checkpoint;
- Concile's dry-run report has been reviewed;
- the app has been migrated to Concile;
- any manual migration items have been fixed and documented;
- the Concile version passes the same test cases;
- realtime updates work after migration; and
- we have enough screenshots, command output, and measurements for the article.

## 11. What we will do next

We will work in small chunks:

1. Initialize the project.
2. Agree on the database schema.
3. Build the Convex queries.
4. Build the Convex mutations.
5. Create the basic board interface.
6. Connect the interface to the backend.
7. Add card movement and simple polish.
8. Test and save the Convex baseline.
9. Preview and apply the Concile migration.
10. Test the Concile version and write down what we learned.

The immediate next step is project initialization. We should not add application features
until the project setup is working and easy for another developer to run.
