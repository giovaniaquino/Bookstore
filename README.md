# Bookstore

This repository contains a simple RESTful API for a bookstore, built with Spring Boot and Spring Data JPA. The application allows for creating, retrieving, and deleting books, managing relationships between books, authors, publishers, and reviews.

## Features

*   **REST API:** Exposes endpoints for managing books.
*   **JPA Relationships:** Demonstrates One-to-One, Many-to-One, and Many-to-Many relationships.
*   **Transactional Operations:** Ensures data consistency for complex operations like saving a book with its related entities.
*   **DTO Pattern:** Uses a Data Transfer Object (`BookRecordDto`) for creating new books, separating API input from the data model.

## Technology Stack

*   **Java 21**
*   **Spring Boot**
*   **Spring Web MVC**
*   **Spring Data JPA**
*   **Hibernate**
*   **PostgreSQL**
*   **Maven**

## Data Models

The application uses the following JPA entities to model the bookstore domain:

*   **`BookModel`**: The main entity representing a book. It has relationships with `PublisherModel`, `AuthorModel`, and `ReviewModel`.
*   **`AuthorModel`**: Represents an author. A book can have multiple authors, and an author can write multiple books (`ManyToMany`).
*   **`PublisherModel`**: Represents a publisher. A publisher can have many books, but a book has only one publisher (`ManyToOne`).
*   **`ReviewModel`**: Represents a review for a book. Each book has one review, and each review is associated with one book (`OneToOne`).

## Getting Started

### Prerequisites

*   JDK 21 or later
*   Maven
*   A running instance of PostgreSQL

### Configuration

1.  Create a PostgreSQL database named `bookstore`.
2.  Update the database connection properties in `src/main/resources/application.properties` with your PostgreSQL username and password.

    ```properties
    spring.datasource.url=jdbc:postgresql://localhost:5432/bookstore
    spring.datasource.username=YOUR_POSTGRES_USERNAME
    spring.datasource.password=YOUR_POSTGRES_PASSWORD
    ```

### Running the Application

1.  Clone the repository:
    ```sh
    git clone https://github.com/giovaniaquino/bookstore.git
    cd bookstore
    ```

2.  Run the application using the Maven wrapper:
    ```sh
    ./mvnw spring-boot:run
    ```

The application will start on `http://localhost:8080`.

## API Endpoints

The following endpoints are available under the base path `/bookstore/books`.

### Create a Book

Creates a new book along with its review. You must provide existing UUIDs for the publisher and authors.

*   **URL:** `/bookstore/books`
*   **Method:** `POST`
*   **Body:**

    ```json
    {
      "title": "The Hobbit",
      "publisherId": "a1b2c3d4-e5f6-7890-1234-567890abcdef",
      "authorId": [
        "b1c2d3e4-f5a6-7890-1234-567890abcdef"
      ],
      "reviewComment": "A timeless classic."
    }
    ```

*   **Success Response (201 Created):**

    ```json
    {
        "id": "c1d2e3f4-g5h6-7890-1234-567890abcdef",
        "title": "The Hobbit",
        "publisher": {
            "id": "a1b2c3d4-e5f6-7890-1234-567890abcdef",
            "name": "Allen & Unwin"
        },
        "authors": [
            {
                "id": "b1c2d3e4-f5a6-7890-1234-567890abcdef",
                "name": "J.R.R. Tolkien"
            }
        ],
        "review": {
            "id": "d1e2f3g4-h5i6-7890-1234-567890abcdef",
            "comment": "A timeless classic."
        }
    }
    ```

### Get All Books

Retrieves a list of all books in the database.

*   **URL:** `/bookstore/books`
*   **Method:** `GET`
*   **Success Response (200 OK):** An array of book objects.

    ```json
    [
        {
            "id": "c1d2e3f4-g5h6-7890-1234-567890abcdef",
            "title": "The Hobbit",
            "publisher": {
                "id": "a1b2c3d4-e5f6-7890-1234-567890abcdef",
                "name": "Allen & Unwin"
            },
            "authors": [
                {
                    "id": "b1c2d3e4-f5a6-7890-1234-567890abcdef",
                    "name": "J.R.R. Tolkien"
                }
            ],
            "review": {
                "id": "d1e2f3g4-h5i6-7890-1234-567890abcdef",
                "comment": "A timeless classic."
            }
        }
    ]
    ```

### Delete a Book

Deletes a book by its UUID.

*   **URL:** `/bookstore/books/{id}`
*   **Method:** `DELETE`
*   **URL Parameters:** `id=[UUID]` (e.g., `c1d2e3f4-g5h6-7890-1234-567890abcdef`)
*   **Success Response (200 OK):**
    ```
    Book deleted successfully.