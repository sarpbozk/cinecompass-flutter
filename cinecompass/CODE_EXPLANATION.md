# CineCompass Code Structure Explanation

## Project Architecture
The app follows a clean architecture approach with BLoC pattern, organized into several distinct layers:

### 1. Presentation Layer
Contains UI components and how they interact with the state management.

#### Screens
- **HomeScreen**: Displays trending movies and search functionality
- **FavoritesScreen**: Shows movies marked as favorites
- **MovieDetailScreen**: Shows detailed information about a selected movie
- **MainScreen**: Handles bottom navigation between Home and Favorites screens

#### Widgets
- **MovieCard**: Reusable widget that displays movie information in a card format

### 2. Business Logic Layer
Handles the application's business logic using the BLoC pattern.

#### Movie BLoC
- **MoviesBloc**: Manages movie-related operations (loading trending, searching)
- **MoviesEvent**: Defines events like LoadTrendingMovies and SearchMovies
- **MoviesState**: Represents different states the UI can be in (loading, loaded, error)

#### Favorites BLoC
- **FavoritesBloc**: Manages favorite movies (add, remove, load)
- **FavoritesEvent**: Defines events like AddFavorite, RemoveFavorite, LoadFavorites
- **FavoritesState**: Represents different states of favorites (loading, loaded, error)

### 3. Data Layer
Handles data operations and API communication.

#### Models
- **Movie**: Data class for movie information, extends Equatable for proper state comparison

#### Services
- **MovieService**: Handles API calls to TheMovieDB
- **SecureStorageService**: Securely stores and retrieves the API key

## Key Technologies and Libraries

### BLoC Pattern
- Separates business logic from UI
- Provides unidirectional data flow: Event → BLoC → State → UI
- Makes testing easier through isolation of components

### Equatable
Used for value comparison of objects, ensuring proper state updates.

### Flutter Secure Storage
Securely stores sensitive data like API keys.

### Shared Preferences
Stores user's favorite movies locally.

### Cached Network Image
Efficiently loads and caches movie posters and backdrops.

## Code Flow

### App Initialization
1. In `main.dart`, the `SecureStorageService` is initialized
2. The BLoC providers are set up
3. The main app UI is rendered

### Home Screen Flow
1. `HomeScreen` loads and dispatches `LoadTrendingMovies` event to `MoviesBloc`
2. `MoviesBloc` fetches trending movies through `MovieService`
3. UI updates based on state changes (loading, loaded, error)
4. Search functionality triggers `SearchMovies` events when query changes

### Favorites Flow
1. `FavoritesScreen` loads and dispatches `LoadFavorites` event
2. `FavoritesBloc` loads saved favorites from SharedPreferences
3. When user toggles favorites, appropriate events are dispatched
4. `FavoritesBloc` updates local storage and emits new state

### Movie Detail Flow
1. User selects a movie, navigating to `MovieDetailScreen`
2. Detailed movie information is displayed
3. Favorite status is determined by `FavoritesBloc.isFavorite()`
4. User can toggle favorite status directly from this screen

## Data Persistence
- **API Key**: Securely stored using `flutter_secure_storage`
- **Favorites**: Stored using `shared_preferences` in JSON format

## API Integration
The app integrates with TheMovieDB API for:
- Fetching trending (top-rated) movies
- Searching for movies
- Getting detailed movie information 