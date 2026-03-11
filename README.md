# iFinanceTracker

A robust, production-grade iOS expense tracking application demonstrating advanced Swift development practices, clean architecture, and seamless Core Data integration.

[![Swift Version](https://img.shields.io/badge/Swift-5.9+-orange.svg)](https://swift.org)
[![iOS Version](https://img.shields.io/badge/iOS-15.0+-blue.svg)](https://developer.apple.com/ios)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)

## 📱 Overview

iFinanceTracker is a feature-complete personal finance management tool built to exemplify production-ready iOS development. It goes beyond a simple tutorial app by implementing a layered MVVM architecture, sophisticated data persistence with Core Data, and a reactive UI using Combine and UIKit. This project serves as a portfolio piece demonstrating my ability to build complex, maintainable, and scalable iOS applications.

The app allows users to meticulously log expenses, categorize them, and gain insights through a visual dashboard, including a custom-drawn pie chart. It handles the full CRUD lifecycle with a focus on data integrity and a smooth user experience.

## Key Features

*   **Complete Expense Management**: Create, read, update, and delete expense logs with details like title, amount, date, and category.
*   **Advanced Filtering & Search**: Filter the expense list by user-defined categories and a real-time search query across log titles and notes.
*   **Flexible Sorting**: Sort expenses by date or amount in either ascending or descending order, with the sort state persisted.
*   **Interactive Dashboard**: Gain financial insights with a dashboard that displays:
    *   Total sum of all expenses.
    *   Total sum per expense category.
    *   A custom, interactive pie chart visualizing the distribution of expenses across categories, built with Core Graphics for smooth performance.
*   **Data Persistence**: All data is securely stored locally using Core Data, providing offline access and reliable performance. The Core Data stack is implemented as a singleton for efficient sharing across the app.
*   **Responsive UI**: Built with a mix of UIKit for complex, data-driven views and SwiftUI components for modern, declarative UI elements where appropriate.

## Architecture: MVVM-C

This project is meticulously structured following the **Model-View-ViewModel (MVVM)** architectural pattern, coordinated by a **Coordinator** pattern for navigation. This ensures a clean separation of concerns, testability, and maintainability.

*   **Model**:
    *   **Core Data Entities**: `ExpenseLog` (with attributes like `title`, `amount`, `date`, `category`) and `Category`. These are generated automatically from the `.xcdatamodeld` file.
    *   **API Data Models**: Swift structs (e.g., `CurrencyRate`) conforming to `Codable` for fetching and parsing external data, such as live currency exchange rates.
*   **View**:
    *   **UIKit ViewControllers & Storyboards**: `LogListView`, `LogFormView`, `DashboardViewController` manage the display and user interaction. They bind to the ViewModel's published properties to reactively update the UI.
    *   **SwiftUI Views**: Used for lighter components like the `CategoryPieChartView` and `FilterCategoriesView`, embedded within UIKit using `UIHostingController`.
*   **ViewModel**:
    *   The core logic layer. Each major View has a corresponding ViewModel (e.g., `LogListViewModel`, `DashboardViewModel`).
    *   **Business Logic**: Handles fetching, filtering, sorting, and formatting data from the Model.
    *   **State Management**: Uses `@Published` properties (from the Combine framework) to expose state to the View. Views observe these properties and react accordingly.
    *   **Data Transformation**: Formats raw Core Data objects (e.g., `Date`, `Double`) into displayable strings (e.g., "Mar 11, 2026", "$12.50").
*   **Coordinator**:
    *   `MainCoordinator` handles all navigation logic, removing this responsibility from ViewControllers and making the flow of the app easier to manage and test.

## Technology Stack

| Technology | Purpose |
| :--- | :--- |
| **Swift 5.9+** | Primary programming language, leveraging modern language features (e.g., property wrappers, optionals, closures). |
| **UIKit** | Core framework for building the primary user interface, providing fine-grained control over complex views and animations. |
| **SwiftUI** | Used for modern, declarative components like the pie chart and filter sheet, demonstrating interoperability. |
| **Combine** | Framework for handling asynchronous events and data binding between the ViewModel and View. |
| **Core Data** | Object graph and persistence framework for local storage of all user data, including `NSFetchedResultsController` for efficient table/collection view updates. |
| **Core Graphics** | Used to draw the custom pie chart in `DashboardViewController`, showcasing proficiency with low-level drawing APIs. |
| **URLSession** | For making network requests (e.g., fetching live currency rates) and handling JSON data with `Codable`. |
| **XCTest** | For writing unit tests to validate business logic in ViewModels and integration tests for data persistence. |

## Project Structure

A peek into the organized file structure of the project:

```
iFinanceTracker/
├── iFinanceTracker.xcodeproj
├── iFinanceTracker/
│   ├── AppDelegate.swift
│   ├── SceneDelegate.swift
│   ├── Info.plist
│   ├── Models/
│   │   ├── CoreData/
│   │   │   ├── iFinanceTracker.xcdatamodeld/
│   │   │   ├── CoreDataStack.swift        # Singleton Core Data stack manager
│   │   │   ├── ExpenseLog+Extension.swift # Convenience methods for ExpenseLog entity
│   │   │   └── Category+CoreDataProperties.swift
│   │   └── API/
│   │       └── CurrencyRate.swift         # Codable model for API response
│   ├── Views/
│   │   ├── LaunchScreen.storyboard
│   │   ├── Main.storyboard
│   │   ├── Logs/
│   │   │   ├── LogListViewController.swift
│   │   │   ├── LogListViewModel.swift
│   │   │   ├── LogFormViewController.swift
│   │   │   └── LogFormViewModel.swift
│   │   ├── Dashboard/
│   │   │   ├── DashboardViewController.swift
│   │   │   ├── DashboardViewModel.swift
│   │   │   └── PieChartView.swift        # SwiftUI view embedded in UIKit
│   │   └── Components/
│   │       ├── CategoryFilterView.swift   # SwiftUI filter chip component
│   │       └── SearchBarView.swift
│   ├── ViewModels/                         # Shared/Base ViewModels
│   │   └── BaseViewModel.swift
│   ├── Coordinators/
│   │   └── MainCoordinator.swift
│   ├── Extensions/
│   │   ├── Date+Extensions.swift
│   │   ├── Double+Extensions.swift        # For currency formatting
│   │   └── UIColor+Extensions.swift
│   ├── Helpers/
│   │   ├── Utils.swift
│   │   └── Constants.swift
│   └── Services/
│       ├── CurrencyService.swift          # Network service for fetching rates
│       └── CoreDataService.swift          # Wrapper for common Core Data operations
├── iFinanceTrackerTests/
│   ├── ViewModelTests/                    # Unit tests for ViewModels
│   │   ├── LogListViewModelTests.swift
│   │   └── DashboardViewModelTests.swift
│   └── ServicesTests/
│       └── CoreDataServiceTests.swift
├── iFinanceTrackerUITests/
│   └── iFinanceTrackerUITests.swift
├── .gitignore
├── LICENSE
├── README.md
└── promo.png
```

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites

*   **Xcode 15.0 or later** (Required for Swift 5.9 and iOS 17 SDK compatibility)
*   **iOS 15.0+ Simulator or Device**
*   **CocoaPods or Swift Package Manager** - *Note: Dependencies are managed via SPM.*

### Installation

1.  **Clone the repository**
    ```bash
    git clone https://github.com/eKidenge/iFinanceTracker.git
    cd iFinanceTracker
    ```

2.  **Open the project in Xcode**
    ```bash
    open iFinanceTracker.xcodeproj
    ```

3.  **Resolve Dependencies**
    *   The project uses Swift Package Manager. Xcode should automatically resolve the dependencies (if any) when you open the project. If not, go to `File > Packages > Resolve Package Versions`.

4.  **Build and Run**
    *   Select your target device or simulator.
    *   Press `Cmd + R` to build and run the application.

## Running the Tests

This project includes a suite of unit and UI tests to ensure reliability.

*   **Run all tests**: `Cmd + U`
*   **Run specific test class**: Click the diamond next to the class or method name.

### Test Coverage

*   **Unit Tests**: Cover the business logic in all ViewModels, ensuring sorting, filtering, and data transformation functions work correctly.
*   **Core Data Integration Tests**: Verify that data is correctly saved, fetched, and deleted without corruption.
*   **UI Tests**: Ensure critical user journeys (e.g., adding a new expense, viewing the dashboard) function as expected.

## Key Implementation Highlights

*   **Reactive UI with Combine**: ViewModels expose `@Published` properties. ViewControllers subscribe to these publishers using `sink` or assign them to UI elements (e.g., `UILabel.text`), ensuring the UI is always in sync with the underlying data without manual updates.
*   **Efficient List Management with `NSFetchedResultsController`**: The `LogListViewController` utilizes an `NSFetchedResultsController` to observe changes in the Core Data `ExpenseLog` entity. This provides optimal performance by automatically updating the `UITableView` when logs are added, deleted, or modified, including smooth animations.
*   **Custom Pie Chart with Core Graphics**: Instead of using a third-party library, the dashboard's pie chart is drawn programmatically using `UIBezierPath` and Core Graphics within a custom `UIView`. This demonstrates a deep understanding of iOS's drawing system and results in a lightweight, performant component.
*   **MVVM + Coordinator for Clean Navigation**: Navigation logic is abstracted away into a `MainCoordinator`. This makes the `UIViewControllers` simpler and more reusable, while also making the app's flow more declarative and easier to modify.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contact

Created by **Elisha Kidenge Odhiambo** – Feel free to contact me!

*   **GitHub**: [@eKidenge](https://github.com/eKidenge)
*   **LinkedIn**: [Elisha Kidenge Odhiambo](https://www.linkedin.com/in/elisha-kidenge-5175b5395)
*   **Portfolio**: [Link to your portfolio](https://my-portfolio-ten-eta-58.vercel.app/)

---

**If you find this project interesting or helpful, please consider giving it a star ⭐ on GitHub!**