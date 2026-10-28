# Spring Boot Data Fetching — Field Injection

![Diagram of the Spring Boot field-injection example](assets/field-injection-diagram.svg)

This project demonstrates **field injection**: Spring supplies a `Data` implementation to `FetchData`, and the app calls that implementation to print a result.

## What I built

- **`pom.xml`** — Maven configuration for the Spring Boot project and Java 17.
- **`SpringBootDependencyDataFetchProjectApplication.java`** — starts Spring Boot, retrieves `FetchData`, and calls `show()`.
- **`Data.java`** — interface defining the `getData()` operation.
- **`Oracle.java`** — `Data` implementation marked `@Primary`; prints `Fetching data from oracle`.
- **`MYSQL.java`** — alternate `Data` implementation; prints `fetch data from sql`.
- **`FetchData.java`** — uses `@Autowired private Data d;` and calls `d.getData()` inside `show()`.

## How `@Primary` is used

Both `Oracle` and `MYSQL` implement `Data`. When Spring injects a `Data` bean into `FetchData`, `@Primary` marks `Oracle` as the default choice. That is why the app prints the Oracle message.

## Compared with my Paradise Restaurant Spring project

In the Paradise project, **`Billcounter.java` uses constructor injection** through `Billcounter(PaymentMode p)`. In this project, **`FetchData.java` uses field injection** through `@Autowired` on `Data d`.

## Console output

```text
Fetching data from oracle
get the data
```

![Eclipse run showing the Oracle class and console output](assets/field-injection-output.png)
