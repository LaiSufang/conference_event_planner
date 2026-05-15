# Conference Expense Planner

Conference Expense Planner is a beginner-friendly React app for building a conference budget in one place. You can choose a venue, add audio/visual extras, select meals, and see the running total update as you make changes.

## What the app does

The app is organized into three main parts:

1. Venue selection. Choose a room type and adjust the quantity.
2. Add-ons selection. Add items like projectors, speakers, microphones, whiteboards, and signage.
3. Meals selection. Pick one or more meal options and set the number of people.

At any time, you can switch to the details view to review everything you selected and see the final event cost.

## How to use the app

1. Open the app in your browser.
2. Click Get Started on the landing page.
3. Choose a venue that fits your event size and budget.
4. Add any audio/visual equipment your event needs.
5. Select the meals you want to offer and enter the number of people.
6. Click Show Details to review the selected items and total cost.

## How Redux state management is implemented

This project uses Redux Toolkit to keep the planner state predictable and easy to update.

### 1. The store combines feature slices

The Redux store is created in [src/store.js](src/store.js) with `configureStore`. It combines three reducers:

- `venue` for venue room quantities
- `av` for add-on quantities
- `meals` for meal selections

Each slice owns one part of the app state, which keeps the logic separated and easier to maintain.

### 2. Each slice defines its own data and actions

The slice files hold the initial state and the reducers for each feature:

- [src/venueSlice.js](src/venueSlice.js) stores venue items and supports incrementing and decrementing quantity.
- [src/avSlice.js](src/avSlice.js) stores add-on items and supports quantity changes.
- [src/mealsSlice.js](src/mealsSlice.js) stores meal options and toggles whether a meal is selected.

Redux Toolkit uses Immer under the hood, so the reducers can be written in a simple mutable style while still producing immutable updates.

### 3. The app reads state with `useSelector`

The main planner component, [src/ConferenceEvent.jsx](src/ConferenceEvent.jsx), uses `useSelector` to read the current venue, add-on, and meal data from the store.

This lets the UI always reflect the latest Redux state without manual syncing.

### 4. User actions dispatch Redux updates

When a user clicks a plus, minus, or checkbox control, the component dispatches an action using `useDispatch`.

Examples include:

- adding or removing a venue room
- increasing or decreasing an add-on quantity
- toggling a meal option on or off

These actions update the store, and React re-renders the UI automatically.

### 5. The total cost is derived from store state

The planner calculates venue, add-on, and meal totals from the current Redux values, then passes those totals into [src/TotalCost.jsx](src/TotalCost.jsx) for display.

This means the total is always derived from state instead of being entered manually.

### 6. The store is provided to the app

In [src/main.jsx](src/main.jsx), the app is wrapped with Redux `Provider`, which makes the store available to all child components.

That setup is what allows any component in the tree to read from or dispatch to Redux.

## Project structure

- [src/App.jsx](src/App.jsx) handles the landing page and reveals the planner.
- [src/ConferenceEvent.jsx](src/ConferenceEvent.jsx) contains the main booking interface.
- [src/TotalCost.jsx](src/TotalCost.jsx) shows the final cost summary.
- [src/store.js](src/store.js) sets up the Redux store.

## Available scripts

Install dependencies first, then use:

- `npm run dev` to start the development server
- `npm run build` to create a production build
- `npm run lint` to check the code for lint issues

## Getting started as a beginner

If you are new to React and Redux, a good way to understand this app is to follow the data flow:

1. A user clicks a button or checkbox in the UI.
2. The component dispatches a Redux action.
3. The matching slice reducer updates the store.
4. `useSelector` reads the new state.
5. React re-renders the updated totals and selections.

That pattern is the core of the app and is a practical example of how Redux helps manage shared state in a React project.

## Next steps

Here are a few improvements that could make the app even better:

1. Add form validation for the number of people and item quantities.
2. Improve accessibility with clearer labels, focus states, and keyboard-friendly controls.
3. Add persistence so selected items stay available after a page refresh.
4. Display images and richer descriptions for each venue, add-on, and meal option.
5. Break the planner into smaller reusable components to make the code easier to maintain.
