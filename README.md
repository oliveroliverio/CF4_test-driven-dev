# Meet App - Event Discovery PWA

A serverless, progressive web application (PWA) built with React using Test-Driven Development (TDD) principles. The app allows users to discover and explore events in different cities by fetching data from the Google Calendar API.

## Features

### 1. Filter Events by City
- **User Story**: As a user, I should be able to filter events by city so that I can see a list of events in that city.
- **Scenario**: User can filter events by city to show only relevant local events
  - **Given** the list of events has been loaded
  - **When** user selects a city from the filter dropdown or types a city name
  - **Then** a list of cities will be displayed in the filter dropdown
  - **When** user clicks on a city in the filter dropdown
  - **Then** the list of events will be filtered to show only events in the selected city

### 2. Show/Hide Event Details
- **User Story**: As a user, I should be able to show/hide event details so that I can see more information about an event when I need.
- **Scenario**: User can expand an event to see its details
  - **Given** the list of events has been loaded
  - **When** user clicks on "Show details" button for an event
  - **Then** the event element will be expanded to show the event details

### 3. Specify Number of Events
- **User Story**: As a user, I should be able to specify the number of events so that I can limit how many events are displayed.
- **Scenario**: User can specify the number of events to display
  - **Given** the list of events has been loaded, and the user has selected a city
  - **When** user selects a number from the filter dropdown
  - **Then** the list of events will be filtered to show only the specified number of events

### 4. Use the App When Offline
- **User Story**: As a user, I should be able to use the app when offline using cached data so that I can see event info if I lack internet access.
- **Scenario**: User can use the app when offline using cached data
  - **Given** the list of events has been loaded, and the user has selected a city
  - **When** user does nothing (this is automatic, no user input needed)
  - **Then** the list of filtered events will be displayed using cached data

### 5. Add an App Shortcut to the Home Screen
- **User Story**: As a user, I should be able to click on an icon that takes me to the home screen so that I can start over if I need to.
- **Scenario**: The user can click on a home button to go back to home page
  - **Given** the user is on any page of the app
  - **When** user clicks on home button
  - **Then** the user will be taken back to the home page

### 6. Display Charts Visualizing Event Details
- **User Story**: As a user, I should be able to view charts visualizing event details so that I can see event information in a visual format.
- **Scenario**: User selects multiple events and clicks on "compare" to show visual charts (comparison table) comparing events (size, location, length, date, etc)
  - **Given** that the user has selected multiple events components
  - **When** user clicks on "compare" button
  - **Then** the app should display a comparison table showing the selected events' details

## Technical Details

- **Frontend**: React
- **Build Tool**: Vite
- **Testing**: Jest, React Testing Library, Cucumber (for BDD)
- **Progressive Web App**: Offline support, installable, responsive design
- **Data Visualization**: Charts for event statistics and comparisons
- **State Management**: React Context API
- **Styling**: CSS Modules

## Getting Started

### Prerequisites

- Node.js (v14 or later)
- npm or yarn

### Installation

1. Clone the repository
2. Install dependencies:
   ```bash
   cd meet
   npm install
   ```
3. Start the development server:
   ```bash
   npm run dev
   ```
4. Open your browser and visit `http://localhost:3000`

## Running Tests

```bash
# Run unit tests
npm test

# Run end-to-end tests
npm run test:e2e
```

## Deployment

The app is configured for deployment as a PWA. To create a production build:

```bash
npm run build
```

## License

This project is licensed under the MIT License.
