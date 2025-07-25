# Movie Town Database Management System Project

Developed in .NET Framework 4.8, using SQL Server and Dapper NuGet.

### Why Dapper?
Because it is "King of Micro ORM" in terms of speed and performance. Also, it is developed by Stack Overflow developers and isn't going anywhere any time soon.
Implementation is relatively simple than Entity Framework and ADO.NET for a small project. It can work after three-step process:
- Create an IDbConnection object.
- Write a query to perform CRUD operations.
- Pass query as a parameter in the Execute method.

Queries can be called using stored procedures. Stored procedures are stored in the database and they save time and memory.
Also, provides some level of protection for SQL injection. More information available on https://dapper-tutorial.net/. And I would like to thank [Tim Corey](https://www.iamtimcorey.com/) for his wonderful tutorials about database, [Dapper](https://www.youtube.com/watch?v=eKkh5Xm0OlU) and C# in general.


```csharp
  public List<Movie> GetMovie(string title)
  {
      using (IDbConnection connection = new System.Data.SqlClient.SqlConnection(Helper.CnnVal("MovieDatabaseDB")))
      {
          var movieList = connection.Query<Movie>("Movie_ViewAllOrSearchByTitle @Title",
              new { Title = title }).ToList();
          return movieList;
      }
  }
```
SQL queries can also executed with `connection.Execute(SQLQuery)` after connection is established.
In this code block Dapper's [Query](https://dapper-tutorial.net/query) method is used to select id. And [Execute](https://dapper-tutorial.net/query) method is used to insert selected values.
```c#
string selectMovieId = "SELECT Id FROM Movies WHERE Title = '" + movieTitle + "';";
var selectedMovieId = connection.Query<Movie>(selectMovieId).FirstOrDefault();

string insertToMovieGenres = "INSERT INTO MovieGenres(MovieId, GenreId) VALUES ( "
    + selectedMovieId.Id + "," + genreId + ");";

connection.Execute(insertToMovieGenres);
```

--------------
### Movie Town Interface
![Home][homepage]
![ui][ui]

### Schema Diagram

![er-diagram][schema]


[homepage]: images/MovieTown_Homepage.png "MovieTown_Homepage"
[ui]: images/MovieTown_UI2.png "MovieTown_UI2"
[schema]:images/Schema&#32;Diagram&#32;Movie&#32;Town.png "schema"



