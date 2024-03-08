# Epassi Recruitment App

## General information

- In this recruitment process, we would like to see how you think and what your approach is to the given problem.
- We expect that you will write production-ready code with tests, where the project builds without issues and all tests are passing.
- In each task, you will read some business backstory and rough description of the assigment, which should help you think in the right direction.
- For each task, please create separate branches, i.e., "task-1", "task-2", "task-3".
- Tip: It would be best to base one task's branch on the previous one, creating somewhat a cascade of branches.
- Extra note: Using AI tools is not allowed, as we want to see how you really think.

## Project backstory

Imagine that you are working in the R&D department of a Bookstore company.\
The Bookstore is not yet open, as we're still working on the technical side of the business.

For now, we're in the phase where we have very limited functionality, and we still need to deliver a few more things for the grand opening.

At this stage, we're able to create, update, read & delete books in our global inventory.\
Worth mentioning: in order to simplify some parts of project, we're using UUID as ISBN (International Standard Book Number).

## Technical information

### Prerequisites

1. Make sure that you have JDK 21 installed, as this project is using Java 21.

2. Initialize fresh git repository in project:

```
rm -rf .git
git init
git add -A
git commit -m "Initial commit"
```

### Running & testing the application

1. How to build project

```bash
./gradlew clean build -x check
```

2. How to run tests

```
./gradlew clean test
```

3. How to run application

```bash
./gradlew bootRun
# or
./gradlew clean build && java -jar build/libs/recruitment-app.jar
```

**Note**: Logs can be found in console or in directory: `logs/`.

### Packaging by feature

As you can see, the application has been packaged by feature, instead of by layer.\
We want to continue with such packaging, so make your changes are in specific packages (or create new one to avoid tightly-coupled entities).

### Testing

All tests are executed in parallel mode, thanks to [Junit Platform configuration](src/test/resources/junit-platform.properties).

Note: tests are using the same database as the application in run mode, however, thanks to `@Transactional` and `@Rollback` annotations, the test data
should be deleted after the test is completed.

### Database

In this project, we are using H2 database that is configured to be compatible with MySQL.\
This configuration enables us to utilize SQL migration files through Flyway.

Initially, there is only one migration file, and it can be found
here: [V20240114000000__create_books_table.sql](src/main/resources/db/migration/V20240114000000__create_books_table.sql).
\
\
When you will start the project or run the tests the database file will be created in `db` directory in root project.\

### Rest API

Available REST API methods:

1. Get list of all books: `GET /api/v1/books`, responds with body of `List<BookDto>`.

```bash
curl -X GET 'http://localhost:8080/api/v1/books' -H 'Content-Type: application/json'
```

2. Get book by ISBN: `GET /api/v1/books/{isbn}`, responds with body of `BookDto`.

```bash
curl -X GET 'http://localhost:8080/api/v1/books/5e0a115e-3160-4690-a7fe-50689cb23e68' -H 'Content-Type: application/json' 
```

3. Create a book: `POST /api/v1/book`, responds with body of ISBN string.

```bash
curl -X POST 'http://localhost:8080/api/v1/books' -H 'Content-Type: application/json' --data-raw '{
   "isbn": "5e0a115e-3160-4690-a7fe-50689cb23e68",
   "title": "Lorem ipsum",
   "author": "Cicero",
   "price": "15.50"
}'
```

4. Update a book: `PUT /api/v1/book`, responds with body of ISBN string.

```bash
curl -X PUT 'http://localhost:8080/api/v1/books' -H 'Content-Type: application/json' --data-raw '{
   "isbn": "5e0a115e-3160-4690-a7fe-50689cb23e68",
   "title": "Lorem ipsum",
   "author": "Cicero",
   "price": "25.50"
}'
```

5. Delete a book by ISBN: `DELETE /api/v1/books/{isbn}`, responds without body, response status 200 OK.

```bash
curl -X DELETE 'http://localhost:8080/api/v1/books/5e0a115e-3160-4690-a7fe-50689cb23e68' -H 'Content-Type: application/json'
```

We are using [BookDto](src/main/java/fi/epassi/recruitment/book/BookDto.java) in the communication,
and [BookModel](src/main/java/fi/epassi/recruitment/book/BookModel.java) for storing Book information in the database.

Note: each response is encapsulated in [ApiResponse](src/main/java/fi/epassi/recruitment/api/ApiResponse.java) entity, which we use, to make sure that
we will always have the same structure of the response.

## Your assignments

Before working on the tasks, make sure, that you have initialized fresh git repository.

### Task #1 - Missing feature required by grand opening

- **Backstory**:\
  We have some time before the grand opening of the Bookstore, and we are still missing the most important feature.\
  We do not have the possibility to know how many books we have available to sell (how many book copies we have in the Inventory).


- **Assigment description**:\
  You need to add the possibility to store information about how many books we have in our global inventory.\
  Also, when introducing such feature, we should have an API with capabilities of:
    - Returning number of book copies that we have in Inventory by ISBN.
    - Returning number of book copies that we have in Inventory by Author.
    - Returning number of book copies that we have in Inventory by Title.
    - Updating number of book copies that we have in the Bookstore Inventory.

  Note, that the endpoint for managing book entities should not be changed.\
  The book availability (number of copies) should be separate information, available via separate API.

### Task #2 - First issues & technical debt impact

- **Backstory**:\
  Initially, we did an MVP (Minimum Viable Product) implementation, where all Books are being listed at once, on a single long page.
  We have also added the possibility to search for Books by Authors or Titles, which helped a lot.\
  In a very short time, we got pretty successful, with more than tens of thousands of books in the inventory.\
  Hence, the actual solution is no longer working, as there are too many books showing on a single page.\
  We need to add the possibility of returning only part of the data.\
  Also, for some time already, people are complaining that listing all the books takes a really long time, so we should also look into it as well.


- **Assigment description**:\
  You need to implement paging for the already existing APIs and also look into slow data search and fix it.

### Task #3 - Success of the product, new feature required by business

- **Backstory**:\
  The business is now super successful.\
  We are now selling thousands of books every day, with over a million books in the inventory.\
  As a company, we have invested and bought 5 buildings in different cities, in order to open other branches of the business there.\
  Also, we will as well start selling books online!


- **Assigment description**:\
  You need to implement solution introducing a "Bookstore" entity, with name, address etc. and extend the way that we're storing number of available
  Books to now be related to specific Bookstore.

## Recruitment Task Delivery

Upon completing your assignment, please follow the steps below:

- Create a **private** repository on [GitHub](https://github.com). ([See screenshot](github/create-private-repository.png))
- Upload the code to the mentioned repository. ([See screenshot](github/push-existing-repo-to-origin.png))
- Invite a collaborator with the name "epassi-group":
    - Navigate to `Settings` > `Access` > `Collaborators` > `Add People` ([See screenshot](github/manage-repository-access.png))
    - Locate the account `epassi-group` and
      click `Add epassi-group to this repository` ([See screenshot](github/invite-collaborator-to-repository.png))
- Send an email to `recruitment-task@epassi.com` and your technical recruiters as recipients.\
  Provide information indicating that the repository has been shared, including the repository name.\
  Then we will be able to continue the Recruitment process.
