# jman-ui Project Plan

jman-ui is a frontend application designed to display and manage data served by the backend at `https://jman.jcore.fi/api`.

## 1. Endpoints & Data Model

The application will consume the following endpoints:

- `/servers`: A list of all the servers.
- `/sites`: A list of all the sites.
- `/plugins`: A list of plugins indexed by `site_id` and `name`.

## 2. Data Strategy & Caching

- **Fetching**: On initialization (or user request), the app will fetch data from all three endpoints.
- **Storage**: Data will be stored in `localStorage` to minimize API requests and allow for snappy client-side operations.
- **Data Processing**: Because the full datasets are stored locally, search, sorting, and pagination will be handled client-side.
- **Relationships**: The frontend will handle "joining" data (e.g., mapping a Site to its Server, and finding all Plugins associated with a specific `site_id`).
- **Refresh Strategy**: Provide a way to refresh data (e.g., a "Refresh Data" button) to fetch the latest state from the API and update `localStorage`.

## 3. Tech Stack

- **Framework**: Vue 3 with Vite.
- **Styling**: Plain CSS.
- **Routing**: Vue Router for handling navigation between the Main View and Detail View.

## 4. UI Structure & Features

### Main View: Site List

- **Search Bar**: A text input to filter the site list by site name or URL.
- **Data Table**:
  - Columns could include: Site Name, URL, Server Name, Plugin Count.
  - Clickable column headers to sort the data (ascending/descending).
- **Pagination Controls**:
  - Page navigation (Previous, Next, Current Page).
  - Selectable rows per page dropdown: 50, 100, 150, 200, 250.

### Detail View: Site Information

- Accessed by clicking a specific row/site in the Data Table.
- Displays detailed Site information.
- Displays information about the Server hosting the site (joined from the `/servers` data).
- Displays a list of Plugins installed on the site (filtered from the `/plugins` data using the `site_id`).

## 5. Implementation Phases

### Phase 1: Project Setup

- Initialize the project repository with chosen framework and tools.
- Set up styling architecture.
- Set up basic application layout and routing.

### Phase 2: API & Data Layer

- Create API fetching utilities for `/servers`, `/sites`, and `/plugins`.
- Implement the `localStorage` caching mechanism (save and retrieve).
- Create helper functions/stores to manage and cross-reference the local data.

### Phase 3: The Data Table (Main View)

- Build the base table component rendering the sites.
- Implement client-side pagination state and logic.
- Implement client-side sorting and text-based searching functionality.

### Phase 4: The Detail View

- Build the Site Detail page or modal.
- Implement logic to retrieve a specific site by ID.
- Display the cross-referenced server details and plugin list for that site.

### Phase 5: Polish & Edge Cases

- Add loading states (spinners/skeletons) during the initial API fetch.
- Add error boundaries and notifications for API failures.
- Add empty states (e.g., "No sites found matching your search").
- Implement the manual "Refresh Data" action.
