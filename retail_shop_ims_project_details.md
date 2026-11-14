# Retail Shop Inventory Management System (Retail IMS)

## 1. Project Overview

**Project name:** Retail Shop Inventory Management System (Retail IMS)\
**Project type:** Web-based inventory and sales management application\
**Primary users:** Small retail shop owners and authorized shop staff\
**Currency:** Nepalese Rupees (NPR / Rs.)\
**Interface style:** Clean white-and-blue dashboard\
**Backend:** Python with Flask\
**Frontend:** HTML, CSS, JavaScript and Bootstrap (planned)\
**Database:** SQLite for the initial version\
**Database layer:** Flask-SQLAlchemy

Retail IMS is a web application intended to help a small retail shop
keep track of its products, stock quantities, suppliers and sales.
Instead of maintaining these records manually in notebooks or separate
spreadsheets, an authorized user can manage the information through one
application.

The initial version is deliberately scoped as a small, practical project
that can be developed incrementally. The first milestone is a working
product-management module. Authentication, dashboard statistics,
supplier workflows, sales processing and reports can be added in later
development stages.

## 2. Problem Statement

Small shops may find it difficult to maintain accurate product and stock
records manually. Common problems include:

-   Not knowing the current quantity of each product.
-   Finding it difficult to identify products that are running low.
-   Spending time searching through handwritten or scattered records.
-   Making mistakes when updating stock after purchases or sales.
-   Having no convenient summary of sales and inventory.

Retail IMS aims to centralize these records and make routine inventory
tasks easier.

## 3. Project Objectives

The main objectives are to:

1.  Store product information in a structured database.
2.  Allow authorized users to add, view, edit and delete product
    records.
3.  Track the available quantity of each product.
4.  identify products that have reached or fallen below a configurable
    low-stock limit.
5.  Store supplier and product-category information.
6.  Record sales and the items included in each sale.
7.  Provide useful summaries and reports as the application develops.
8.  Present information in a clear, responsive interface.
9.  Use NPR when displaying prices and sales amounts.

## 4. Intended Users and Roles

### Administrator

The administrator is intended to manage the application and shop
records. Planned permissions include:

-   Access the dashboard.
-   Manage products and categories.
-   Manage supplier records.
-   View inventory and sales reports.
-   Manage user accounts.

### Staff member

A staff role may be added for day-to-day shop operations. Possible
permissions include:

-   View products and stock.
-   Record sales.
-   View relevant transaction history.

**Implementation note:** The current database design includes a `User`
model, but complete login, role-based permissions and account-management
screens have not yet been implemented.

## 5. Main Modules and Features

### 5.1 Dashboard

The dashboard will provide a quick overview of shop activity.

Planned dashboard information:

-   Total number of products.
-   Total stock quantity.
-   Today's sales total.
-   Number of low-stock products.
-   Recent sales transactions.
-   Sales overview chart.

The dashboard figures should be calculated from database records rather
than entered as fixed values.

### 5.2 Product Management

Product management is the first major functional module.

Planned features:

-   Add a new product.
-   Display all products.
-   Edit product information.
-   Delete a product when it is safe to do so.
-   Search products by name.
-   Display the product price and available quantity.
-   Show a low-stock status.

Proposed product fields:

  -----------------------------------------------------------------------
  Field                               Description
  ----------------------------------- -----------------------------------
  `id`                                Unique product identifier

  `name`                              Product name

  `category_id`                       Optional link to a category

  `supplier_id`                       Optional link to a supplier

  `price`                             Product selling price

  `quantity`                          Current available quantity

  `low_stock_limit`                   Quantity at or below which the
                                      product is flagged
  -----------------------------------------------------------------------

The current product form is planned to accept the product name, price,
quantity and low-stock limit. Category and supplier selection can be
connected to the form in a later stage.

### 5.3 Category Management

Categories will help organize products into groups, such as groceries,
beverages, stationery or household items.

Planned features:

-   Add a category.
-   View categories.
-   Edit a category.
-   Delete a category when it is not in use, or handle dependent
    products safely.
-   Filter products by category.

### 5.4 Supplier Management

Supplier records will store information about the businesses or people
from whom the shop obtains products.

Proposed supplier fields:

-   Supplier ID.
-   Supplier name.
-   Phone number.
-   Address.

Planned features include adding, viewing, editing and deleting supplier
records, as well as associating products with suppliers.

### 5.5 Inventory Management

The inventory module will help the shop monitor stock.

Planned features:

-   View current stock quantities.
-   Identify low-stock products.
-   Record stock received from suppliers.
-   Update stock after a sale.
-   Maintain a record of stock changes.

A low-stock condition is intended to be:

`quantity <= low_stock_limit`

Stock should be changed through validated application operations so that
quantities do not accidentally become negative.

### 5.6 Sales Management

The sales module will record transactions made by the shop.

Planned features:

-   Select products for a sale.
-   Enter quantities.
-   Calculate line totals and the sale total.
-   Save the sale and its items.
-   Reduce product stock after a successful sale.
-   View previous sales.

A sale should be saved as one consistent database transaction. The
application should check that sufficient stock is available before
confirming the sale. If any part of the operation fails, the database
changes should be rolled back.

### 5.7 Reports

Reports will help the shop understand its activity.

Possible reports:

-   Daily sales.
-   Weekly sales.
-   Monthly sales.
-   Sales by product.
-   Current inventory.
-   Low-stock product list.

Charts may be implemented with Chart.js after the basic data and sales
workflows are working.

## 6. Technology Stack

  Technology         Purpose
  ------------------ -------------------------------------
  HTML5              Page structure
  CSS3               Styling and layout
  JavaScript         Client-side interactions
  Bootstrap 5        Responsive UI components (planned)
  Python             Application programming language
  Flask              Web framework and URL routing
  Flask-SQLAlchemy   Database models and queries
  SQLite             Initial relational database
  Flask-Login        Login-session management (planned)
  Chart.js           Dashboard charts (planned)
  Git and GitHub     Version control and project hosting

### Why Flask?

Flask is a lightweight Python web framework. It is suitable for this
project because it provides URL routing, request handling and template
rendering without requiring a large framework structure.

### Why SQLite?

SQLite is easy to set up and works well for a small local development
project. It stores the database in a file and does not require a
separate database server. PostgreSQL could be considered later if the
application grows or needs multiple concurrent users and a production
database service.

## 7. Database Design

The initial database design contains six models.

### 7.1 User

  -----------------------------------------------------------------------
  Column                  Suggested type          Purpose
  ----------------------- ----------------------- -----------------------
  `id`                    Integer, primary key    User identifier

  `username`              String, unique          Login name

  `password`              String                  Store a password hash,
                                                  not a plain-text
                                                  password
  -----------------------------------------------------------------------

### 7.2 Category

  Column   Suggested type         Purpose
  -------- ---------------------- ---------------------
  `id`     Integer, primary key   Category identifier
  `name`   String, unique         Category name

Relationship: one category can have many products.

### 7.3 Supplier

  Column      Suggested type         Purpose
  ----------- ---------------------- ---------------------
  `id`        Integer, primary key   Supplier identifier
  `name`      String                 Supplier name
  `phone`     String                 Contact number
  `address`   String                 Supplier address

A supplier can be associated with multiple products. A corresponding ORM
relationship can be added when supplier workflows are implemented.

### 7.4 Product

  Column              Suggested type         Purpose
  ------------------- ---------------------- ---------------------
  `id`                Integer, primary key   Product identifier
  `name`              String                 Product name
  `category_id`       Foreign key            Associated category
  `supplier_id`       Foreign key            Associated supplier
  `price`             Numeric(12, 2)         Price in NPR
  `quantity`          Integer                Available quantity
  `low_stock_limit`   Integer                Low-stock threshold

### 7.5 Sale

  Column           Suggested type         Purpose
  ---------------- ---------------------- -------------------------
  `id`             Integer, primary key   Sale identifier
  `total_amount`   Numeric(12, 2)         Total sale value in NPR
  `date`           DateTime               Date and time of sale

### 7.6 SaleItem

  Column         Suggested type         Purpose
  -------------- ---------------------- -----------------------------------
  `id`           Integer, primary key   Sale-item identifier
  `sale_id`      Foreign key            Parent sale
  `product_id`   Foreign key            Product sold
  `quantity`     Integer                Quantity sold
  `price`        Numeric(12, 2)         Unit price recorded for this sale

A sale can contain multiple sale items. Each sale item refers to a
product and records the quantity and price used for that transaction.

### Relationship overview

-   Category → Product: one-to-many.
-   Supplier → Product: one-to-many conceptually; the supplier foreign
    key is present in the product model.
-   Sale → SaleItem: one-to-many.
-   Product → SaleItem: one-to-many conceptually.

The current models define the sale-to-item relationship and the
category-to-product relationship. Additional ORM relationships and
deletion rules can be added as the relevant modules are developed.

## 8. Application Structure

The project is being developed with a simple Flask structure.

``` text
retail_ims/
│
├── app.py
├── requirements.txt
│
├── templates/
│   ├── index.html
│   ├── products.html
│   └── product_form.html
│
├── static/
│   ├── css/
│   │   └── style.css
│   └── js/
│       └── script.js
│
└── instance/
    └── ims.db
```

Folder and file responsibilities:

-   `app.py`: Flask setup, database configuration, models and routes.
-   `requirements.txt`: Python package dependencies.
-   `templates/`: Jinja HTML templates rendered by Flask.
-   `static/css/`: Stylesheets.
-   `static/js/`: Browser-side JavaScript.
-   `instance/ims.db`: Local SQLite database created by
    Flask-SQLAlchemy.

As the project grows, the application can be reorganized into separate
files for models, routes, forms and configuration.

## 9. Current Development Status

The project is being built step by step.

### Initial setup

The setup instructions cover:

-   Creating a project folder.
-   Creating and activating a Python virtual environment.
-   Installing Flask, Flask-SQLAlchemy and Flask-Login.
-   Creating the templates and static folders.
-   Configuring Flask and SQLite.
-   Creating the initial database models.

### Product management

The first CRUD module has been drafted with:

-   A route to list and search products.
-   A route to add products.
-   A route to edit products.
-   A route to delete products.
-   HTML templates for the product list and product form.
-   Basic white-and-blue CSS.

The user should test these files locally and resolve any errors before
treating the module as complete.

**Important status distinction:** The project plan and code examples
have been prepared in the conversation, but this document does not
independently verify the user's local files, database contents or
running application.

## 10. Development Roadmap

### Phase 1 --- Project setup

-   Create the project directory.
-   Set up the virtual environment.
-   Install dependencies.
-   Configure Flask.
-   Connect SQLite.

### Phase 2 --- Database models

-   Create User.
-   Create Category.
-   Create Supplier.
-   Create Product.
-   Create Sale.
-   Create SaleItem.
-   Create the database tables.

### Phase 3 --- Product CRUD

-   Build the product listing page.
-   Add products.
-   Edit products.
-   Delete products safely.
-   Search products.
-   Validate form input.

### Phase 4 --- Dashboard

-   Create the main dashboard layout.
-   Display product and stock counts.
-   Display low-stock count.
-   Add sales summary cards.
-   Add charts when sales data is available.

### Phase 5 --- Categories and suppliers

-   Build category CRUD.
-   Build supplier CRUD.
-   Connect categories and suppliers to product forms.

### Phase 6 --- Inventory workflows

-   Record stock received.
-   Record stock adjustments.
-   Track stock movements.
-   Apply low-stock rules.

### Phase 7 --- Sales

-   Create a sale form.
-   Add multiple products to a sale.
-   Validate stock.
-   Calculate totals.
-   Save sale records and sale items.
-   Deduct stock safely.

### Phase 8 --- Authentication and permissions

-   Create login and logout.
-   Hash passwords securely.
-   Protect private routes.
-   Add roles if required.

### Phase 9 --- Reports and testing

-   Create sales reports.
-   Create inventory reports.
-   Test invalid form inputs.
-   Test stock calculations.
-   Test database relationships.
-   Test access permissions.

### Phase 10 --- Deployment

-   Configure production settings.
-   Use a secure secret key from environment variables.
-   Choose a production database if needed.
-   Deploy to a suitable hosting service.
-   Configure backups and error logging.

## 11. User Interface Design

The selected design is a clean white-and-blue dashboard.

Design guidelines:

-   White or very light backgrounds.
-   Blue as the primary action and navigation color.
-   Clear headings and readable typography.
-   Consistent spacing and rounded cards.
-   Tables with clear column headings.
-   Green status for normal stock.
-   Red or orange status for low stock.
-   Responsive layouts for desktop and mobile.

The interface should prioritize clarity and quick access to common shop
tasks.

## 12. NPR Currency Handling

All prices and monetary totals should be displayed in Nepalese Rupees.

Example display:

-   `Rs. 2,500.00`
-   `Rs. 350.00`
-   `Rs. 12,750.00`

Store monetary values using a fixed-precision numeric database type
rather than binary floating-point values. Validate user input on the
server before saving it. Use consistent formatting when displaying
amounts.

## 13. Validation and Security Considerations

The application should include the following safeguards as features are
implemented:

-   Validate all form inputs on the server.
-   Reject negative prices and stock quantities.
-   Check stock availability before completing a sale.
-   Hash passwords using a suitable password-hashing function.
-   Keep secret keys outside source code in production.
-   Protect private routes with authentication.
-   Use POST requests for actions that change or delete data.
-   Add CSRF protection to forms, for example with Flask-WTF.
-   Avoid deleting products that are referenced by sales history.
-   Use database transactions for multi-step sales operations.
-   Back up the database regularly.
-   Do not expose debug mode in production.

The initial development server may use Flask debug mode locally. Debug
mode must be disabled for deployment.

## 14. Testing Plan

  Test                             Expected result
  -------------------------------- ------------------------------------
  Add a valid product              Product is saved and displayed
  Submit an empty product name     Validation message is shown
  Enter a negative price           Input is rejected
  Enter a negative quantity        Input is rejected
  Search for an existing product   Matching product is displayed
  Edit a product                   Updated information is displayed
  Delete an unused product         Product is removed
  View a low-stock product         Low-stock status is displayed
  Sell more than available stock   Sale is rejected
  Complete a valid sale            Sale is saved and stock is reduced

## 15. Expected Outcome

When the planned modules are complete, Retail IMS should provide a small
retail shop with one place to:

-   Maintain product records.
-   Organize products into categories.
-   Keep supplier details.
-   Monitor stock quantities.
-   Identify low-stock products.
-   Record sales.
-   Review sales and inventory reports.
-   Manage access for authorized users.

The project also provides practical experience with Python, Flask,
relational databases, SQLAlchemy, HTML templates, CSS, JavaScript, CRUD
operations, authentication and application deployment.

## 16. Future Improvements

Possible improvements after the first working version include:

-   Barcode scanning.
-   Printable invoices and receipts.
-   Product import and export using CSV.
-   PDF reports.
-   Customer records.
-   Purchase orders.
-   Stock movement history.
-   Multiple shop branches.
-   Product image uploads.
-   Role-based access control.
-   Automated database backups.
-   A PostgreSQL production database.

These are optional extensions and are not required for the first
version.

## 17. Conclusion

Retail Shop Inventory Management System is a practical full-stack web
development project focused on the daily needs of a small retail
business. It begins with a manageable Flask and SQLite setup and grows
through product management, inventory tracking, sales, reporting and
authentication.

The development approach is incremental: build and test one module at a
time, then connect the modules into a complete application.
