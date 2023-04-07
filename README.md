# Spring Project

## Overview

Congratulations! You have been hired by Access Camp and for your first task, you
have been tasked with building out a website to log campers with their
activities.

Your task is to build the API according to the deliverables below.

## Setup

1. Go to [https://start.spring.io/](https://start.spring.io/).
2. Select the project properties.
   1. Select "Maven Project", as we will use Maven as the build tool.
   2. Select "Java" as the language.
   3. Select the most recent version of Spring Boot 2. (Make sure it does not
      have "SNAPSHOT" listed after it.)
   4. Change the "Artifact" metadata field to "spring-project". (This will
      update the "Name" and "Package name" metadata fields too).
   5. Change the "Description" metadata field to "Spring Module Project".
   6. Select the appropriate Java JDK version.
3. Add dependencies.
   1. Let's add the Spring Web dependency to create a Spring web application.
   2. Click "ADD DEPENDENCIES".
   3. Search for "spring web".
   4. Select "Spring Web" from the list.
   5. Click on "ADD DEPENDENCIES" again and add  "Lombok" from the list.
   6. Click on "ADD DEPENDENCIES" again and add "Spring Data JPA" from the list.
   7. Click on "ADD DEPENDENCIES" again and add "PostgreSQL Driver" from the
      list.
   8. Click on "ADD DEPENDENCIES" again and add "Validation" from the list.
4. Click on the "Generate" button on the bottom. This will download a zip file
   containing the Spring Boot Project.
5. Unzip the archive and open it in IntelliJ or a preferred code editor.

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

If you clone this Git repository, you'll find the `data.sql` file that
contains the schema you can import into pgAdmin4.

## Project Requirements

Create a Spring Boot API that models the campers signing up for activities.
Make sure you consider the relationships that are shown in the
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
  - Note: You will most likely need to add the `ModelMapper` dependency to the
    `pom.xml`.
- Add the database properties to the `application.properties` file to connect to
  the PostgreSQL database, `camp_db`.
- Write the code for the repository classes to extend the `CrudRepository`
  interface or the `JpaRepository` interface.
  - Note: The lessons we have seen thus far have gone over the `CrudRepository`
    interface, but you can use the `JpaRepository` interface too as it provides
    some more built-in methods. For more information on the `JpaRepository`
    interface, please see
    [JpaRepository Documentation](https://docs.spring.io/spring-data/jpa/docs/current/api/org/springframework/data/jpa/repository/JpaRepository.html).
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

> REMINDER: Commit your changes to Git every time you get a feature working!
> This will create a clean git history and allow the instructors to see how
> you approached this project.

The above requirements are requirements you can start on now, as they make use
of concepts from the previous modules.

The following requirements are requirements that you will implement throughout
the course of this module:

- Make use of the logging framework and log to a file called
  `spring-project.log`.
- Add Spring Security and basic authentication features.
- Add unit testing to confirm everything is still working as expected.

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

### DELETE /activity/{id}

If the `Activity` exists, it should be removed from the database, along with any
`Signup`s that are associated with it (a `Signup` belongs to an `Activity`, so
you need to delete the `Signup`s before the `Activity` can be deleted).

After deleting the `Activity`, return an _empty_ response body, along with the
appropriate HTTP status code.

### POST /signup

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
  "time": 9,
  "camper_id": 1,
  "activity_id": 2
}
```

## Stretch Goals

If time allows, and you want to try to implement some stretch goals, consider
the following:

- Add PUT requests to update a camper, activity, or signup.
- Add TRACE-level log statements to the application.
- Implement authentication with the campers' credentials.
- Implement authorization so that the camper logged in can only access his or
  her signups.
- Add integration tests to the controller classes and repository classes.
- Add an acceptance test to test every endpoint in the application.
