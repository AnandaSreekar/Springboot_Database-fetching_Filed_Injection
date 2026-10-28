

Spring Boot Field Injection: Data Fetch Example

This project demonstrates field injection in Spring Boot by wiring a `Data` implementation into `FetchData`.
Project question
How can a service fetch data through a shared interface while Spring chooses which implementation to inject?
What this example demonstrates
`Data` defines the shared `getData()` contract.
`Oracle` and `MYSQL` are Spring components that implement `Data`.
`Oracle` is marked `@Primary`, so Spring selects it when injecting a `Data` bean.
`FetchData` uses `@Autowired` on its `Data` field. This is field injection.
The application starts Spring Boot, retrieves `FetchData`, and calls `show()`.
In my Paradise Restaurant Spring project, I demonstrated constructor injection. This separate data-fetch example demonstrates field injection using a different application.
Runtime flow
The sequence diagram shows startup, Spring's bean selection, and the method calls. It is a runtime sequence—not a project file tree.
```mermaid
sequenceDiagram
    actor Main
    participant Boot as Spring Boot
    participant Context as ApplicationContext
    participant Oracle as Oracle bean
    participant MySQL as MYSQL bean
    participant Fetch as FetchData bean
    participant Console
    Main->>Boot: SpringApplication.run(...) 
    Boot->>Context: Create application context
    Context->>Oracle: Discover @Component
    Context->>MySQL: Discover @Component
    Note over Oracle,MySQL: Both implement Data; Oracle has @Primary
    Context->>Fetch: Create FetchData component
    Context->>Fetch: @Autowired injects Oracle into field d
    Main->>Context: getBean(FetchData.class)
    Context-->>Main: Return FetchData bean
    Main->>Fetch: show()
    Fetch->>Oracle: d.getData()
    Oracle-->>Console: Fetching data from oracle
    Fetch-->>Console: get the data
```
Run output screenshot
![Eclipse screenshot showing the Oracle component and Spring Boot console output](assets/field-injection-output.png)
The code prints `Fetching data from oracle` and then `get the data`.
