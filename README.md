# Spring Project

## Overview

Congratulations! You have been hired by Access Camp and for your first task, you
have been tasked with building out a website to log campers with their
activities.

Your task is to build the API according to the deliverables below.

## Setup

1. Go to [https://start.spring.io/](https://start.spring.io/).
2. Select the project properties.
3. Select "Maven Project", as we will use Maven as the build tool.
4. Select "Java" as the language.
5. Select the most recent version of Spring Boot 2. (Make sure it does not have
   "SNAPSHOT" listed after it.)
6. Change the "Artifact" metadata field to "spring-project". (This will update
   the "Name" and "Package name" metadata fields too).
7. Change the "Description" metadata field to "Spring Module Project".
8. Select the appropriate Java JDK version.
9. Add dependencies.
10. Let's add the Spring Web dependency to create a Spring web application.
11. Click "ADD DEPENDENCIES".
12. Search for "spring web".
13. Select "Spring Web" from the list.
14. Click on "ADD DEPENDENCIES" again and add  "Lombok" from the list.
15. Click on "ADD DEPENDENCIES" again and add "Spring Data JPA" from the list.
16. Click on "ADD DEPENDENCIES" again and add "PostgreSQL Driver" from the list.
17. Click on "ADD DEPENDENCIES" again and add "Validation" from the list.
18. Click on the "Generate" button on the bottom. This will download a zip file
    containing the Spring Boot Project.
19. Unzip the archive and open it in IntelliJ or a preferred code editor.

![spring-project-initializr-configurations](https://curriculum-content.s3.amazonaws.com/spring-mod-2/project/spring-initializr-project.png)

In pgAdmin4, create the camp_db:

![create-camp-db](https://curriculum-content.s3.amazonaws.com/spring-mod-2/project/pgadmin-create-camp-db.png)

## Models

You need to create the following relationships:

- A `Camper` can have many `Signups`.
- An `Activity` can have many `Signups`.
- A `Signup` can have a `Camper` for a certain `Activity`

Start by creating the models for the following database tables:

![camper-er-diagram](https://curriculum-content.s3.amazonaws.com/spring-mod-2/project/camp-er-diagram.png)

If you clone down this Git repository, you'll find the `data.sql` file that
contains the schema you can import into pgAdmin4.

## Project Requirements

Create a Spring Boot API that models the campers signing up for activities.
Make sure you consider the relationships, that are shown in the
entity-relationship (ER) diagram above.

It is recommended to follow this outline of how to go about creating the
project:

- Create all the classes and interfaces to set up the project structure.
- Write the code for the entity and DTO classes first.
    - For example: Create the `Activity`, `Camper` and `Signup` entities and then
      write their corresponding DTO classes.
    - Consider the relationships based on the ER diagram above.
    - Consider the validations that should be added to the DTO classes in the
      section below.
- Add the `ModelMapper` bean to the configuration file
  (i.e., `SpringProjectApplication`).
- Add the database properties to the `application.properties` file to connect to
  the PostgreSQL database, `camp_db`.
- Write the code for the repository classes to extend the `CrudRepository`
  interface.
- Write the code for the `ActivityService` and `ActivityController` classes.
  - It is recommended to do this in iterations.
    - For example, write the POST method to add a new activity to the database.
      Then test and make sure that works using Postman. Once that is working,
      move onto the next method until all desired methods are working in both
      the controller and service classes.
  - Consider the routes that need to be implemented and the example JSON files
    in the section below.
- Write the code for the `CamperService` and `CamperController` classes.
  - It is recommended to write the code in iterations.
  - Consider the routes that need to be implemented and the example JSON files
    in the section below.
- Write the code for the `SignupService` and `SignupController` classes.
  - It is recommended to write the code in iterations.
  - Consider the routes that need to be implemented and the example JSON files.

The above requirements are requirements you can start on now, as they make use
of concepts from the previous modules.

The following requirements are requirements that you will implement throughout
the course of this module:

- Add logging to the project.
- Add password encryption for when campers are logging into the camper portal.
- Add authentication and authorization security features.
- Add unit testing and integration testing to confirm everything is still
  working as expected.

Only submit your project once you are fully finished making changes to it to
your instructor.

### Validations

Add validations to the `Camper` DTO:

- The name field should not be empty nor `null`.
- The age field should be an integer between 8 and 18.
- The username to log into the camper portal should not be empty nor `null`.
- The password to log into the camper portal should not be empty nor `null`.
  This should also have a length of at least 8 characters.

Add validations to the `Signup` DTO:

- The time field must be between 0 and 23 (referring to the hour of day for the
  activity).

**Hint**: Add the validation annotations to the DTO classes and the `@Valid`
annotation to the respective controller classes. Refer back to the "Validations"
lesson from Spring Module 1.

### Routes

Set up the following routes. Make sure to return JSON data in the format
specified along with the appropriate HTTP verb.

#### POST /campers

This route should create a new `Camper`. It should accept an object with the
following properties in the body of the request:

```json
{
  "name":"Suzie Bingham",
  "age":14,
  "username":"sbingham",
  "password":"SuziePoo6626"
}
```

If the `Camper` is created successfully, send back a response with the new
`Camper`:

```json
{
  "id": 1,
  "name": "Suzie Bingham",
  "age": 14
}
```

If the `Camper` is **not** created successfully, return the following JSON data,
along with the appropriate HTTP status code:

```json
{
  "errors": ["validation errors"]
}
```

#### GET /campers

Return JSON data in the format below. **Note**: you should return a JSON
response in this format, without any additional nested data related to each
camper.

```json
[
  {
    "id": 1,
    "name": "Suzie Bingham",
    "age": 14
  },
  {
    "id": 2,
    "name": "Dustin Henderson",
    "age": 14
  }
]
```

#### GET /campers/{id}

If the `Camper` exists, return JSON data in the format below.

**Note**: you will need to serialize the data for this response differently than
for the `GET /campers` route. Make sure to include an array of activities for
each camper.

```json
{
  "id": 1,
  "name": "Suzie Bingham",
  "age": 14,
  "activities": [
    {
      "id": 1,
      "name": "Archery",
      "difficulty": 2
    },
    {
      "id": 2,
      "name": "Physics and Fun",
      "difficulty": 4
    }
  ]
}
```

If the `Camper` does not exist, return the following JSON data, along with the
appropriate HTTP status code:

```json
{
  "error": "Camper not found"
}
```

### POST /activity

This route should create a new `Activity`. It should accept an object with the
following properties in the body of the request:

```json
{
  "name": "Archery",
  "difficulty": 2
}
```

If the `Activity` is created successfully, send back a response with the new
`Activity`:

```json
{
  "id": 1,
  "name": "Archery",
  "difficulty": 2
}
```

#### GET /activities

Return JSON data in the format below:

```json
[
  {
    "id": 1,
    "name": "Archery",
    "difficulty": 2
  },
  {
    "id": 2,
    "name": "Physics and Fun",
    "difficulty": 4
  }
]
```

### DELETE /activities/{id}

If the `Activity` exists, it should be removed from the database, along with any
`Signup`s that are associated with it (a `Signup` belongs to an `Activity`, so
you need to delete the `Signup`s before the `Activity` can be deleted).

After deleting the `Activity`, return an _empty_ response body, along with the
appropriate HTTP status code.

If the `Activity` does not exist, return the following JSON data, along with the
appropriate HTTP status code:

```json
{
  "error": "Activity not found"
}
```

### POST /signups

This route should create a new `Signup` that is associated with an existing
`Camper` and `Activity`. It should accept an object with the following
properties in the body of the request:

```json
{
  "camper_id": 1,
  "activity_id": 2,
  "time": 9
}
```

If the `Signup` is created successfully, send back a response with the data
related to the `Signup`:

```json
{
  "id": 1,
  "camper_id": 1,
  "activity_id": 2,
  "time": 9
}
```

If the `Signup` is **not** created successfully, return the following JSON data,
along with the appropriate HTTP status code:

```json
{
  "errors": ["validation errors"]
}
```
