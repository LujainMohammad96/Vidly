# Vidly
[The Complete ASP.NET MVC 5 Course](https://www.udemy.com/the-complete-aspnet-mvc-5-course/)

&nbsp;
## 00 Start the project
* In VS start a new project named Vidly. Select *ASP.NET Web Application*. Use the *MVC* template.

&nbsp;
## 01 Add the *Movie* class

* In *Models* add the *Movie* class.

* Add the *Id* and *Name* properties.

```
public class Movie
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }
```

&nbsp;
## 02 Add the *MoviesController* controller and the *Random* movies view

* In *Controllers* add the empty *MoviesController*.

* Create the *Random* action. It contains an instance of the *Movie* model for the movie named "Shrek!" for demonstration.

```
    // GET: Movies/Random
    public ActionResult Random()
    {
        var movie = new Movie() { Name = "Shrek!" };
        return View(movie);
    }
```

* In *Views/Movies* add the *Random* view. It uses the *Shared/_Layout.cshtml*. Add the model defining directive and render the movie's *Name* property.

```
@model Vidly.Models.Movie

@{
    ViewBag.Title = "Random";
    Layout = "~/Views/Shared/_Layout.cshtml";
}

<h2>@Model.Name</h2>
```

&nbsp;
## 03 Add the *Lumen* Bootstrap theme

* Get the *Lumen* theme css code from Bootswatch.com.

* Add it in *Content* as *bootstrap-lumen.css*.

* Edit *App_Start/BundleConfig.cs* in order to set the theme to *Lumen*.

```
bundles.Add(new StyleBundle("~/Content/css").Include(
                      "~/Content/bootstrap-lumen.css",
                      "~/Content/site.css"));
```                      

&nbsp;
## 04 Add a partial view for the NavBar

* In *Shared* add the *_NavBar* partial view.

* Cut the NavBar code from *_Layout.cshtml* and paste it into *_Navbar.cshtml*.

* In place of the cut code in *_Layout.cshtml* add a call to the *Html.Partial* method.

```
@Html.Partial("_NavBar")
```


&nbsp;
## 05 Edit app name and nav links

* Change the app name to *Vidly* in the footer section of *_Layout.cshtml*.

```
<footer>
    <p>&copy; @DateTime.Now.Year - Vidly</p>
</footer>
```

* Change the app name to *Vidly* inside *_Navbar.cshtml*.

```
@Html.ActionLink("Vidly", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })
```

* In *_Navbar.cshtml* remove the existing links and add links for *Customers* and *Movies*.

```
<li>@Html.ActionLink("Customers", "Index", "Customers")</li>                
<li>@Html.ActionLink("Movies", "Index", "Movies")</li>
```


&nbsp;
## 06 Edit *MoviesController*, add *Index* and *Getmovies*

* Edit *MoviesController* and add the *Index* class, which is calling the *GetMovies* private method.  *Index* returns a *ViewResult* action result, which gets the return of *GetMovies* as an input parameter. *GetMovies* is defined as an `IEnumerable<Movie>` interface that returns a list of available movies.
```
    public ViewResult Index()
    {
        var movies = GetMovies();

        return View(movies);
    }

    private IEnumerable<Movie> GetMovies()
    {
        return new List<Movie>
        {
            new Movie { Id = 1, Name = "Shrek" },
            new Movie { Id = 2, Name = "Wall-e" }
        };
    }
```

&nbsp;
## 07 Add the *Movies/Index* view

* In *Views/Movies* add the *Index* view, using the *~/Views/Shared/_Layout.cshtml* layout.

* Configure the view so that it displays the *Name* property of each movie in the `IEnumerable<Vidly.Models.Movie>` model.
```
<h2>Movies</h2>
<table class="table table-bordered table-hover">
    <thead>
        <tr>
            <th>Movie</th>
        </tr>
    </thead>
    <tbody>
        @foreach (var movie in Model)
        {
            <tr>
                <td>@movie.Name</td>
            </tr>
        }
    </tbody>
</table>
```

&nbsp;
## 08 Add the *Customer* class and controller

* In *Models*, add the *Customer* class and give it an *Id* and a *Name* properties.

```
public class Customer
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }
```

* In *Controllers*, add the *CustomersController* controller. It contains the *Index* class, which is calling the *GetCustomers* private method.  *Index* returns a *ViewResult* action result, which gets the return of *GetCustomers* as an input parameter. *GetCustomers* is defined as an `IEnumerable<Customer>` interface that returns a list of available customers.

```
public class CustomersController : Controller
    {
        public ViewResult Index()
        {
            var customers = GetCustomers();

            return View(customers);
        }

        private IEnumerable<Customer> GetCustomers()
        {
            return new List<Customer>
            {
                new Customer { Id = 1, Name = "John Smith" },
                new Customer { Id = 2, Name = "Mary Williams" }
            };
        }
    }
```

&nbsp;
## 09 Add the *Customers/Index* view

* In *Views/Customers* add the *Index* view, using the *~/Views/Shared/_Layout.cshtml* layout.

* Configure the view so that it displays the *Name* property of each customer in the `IEnumerable<Vidly.Models.Customer>` model. If there are not customers an appropriate message is returned.

```
<h2>Customers</h2>
@if (!Model.Any())
{
    <p>There are no customers yet.</p>
}
else
{
    <table class="table table-bordered table-hover">
        <thead>
            <tr>
                <th>Customer</th>
            </tr>
        </thead>
        <tbody>
            @foreach (var customer in Model)
            {
                <tr>
                    <td>@customer.Name</td>
                </tr>
            }
        </tbody>
    </table>
}
```

&nbsp;
## 10 Add *Details* for customers

* In the *CustomersController*, add the *Details* method. It returns an *ActionResult* which provides a view of a single customer, whose Id matches the method's input parameter.
```
    public ActionResult Details(int id)
    {
        var customer = GetCustomers().SingleOrDefault(c => c.Id == id);

        if (customer == null)
            return HttpNotFound();

        return View(customer);
    }
```

* In *Views/Customers/Index* replace the code that displays each customer's *Name* with an *ActionLink* which displays the *customer.Name*, invokes the *Details* method from the *Customers* controller and assigns the *customer.Id* on the method's *id* argument.

```
<td>@Html.ActionLink(customer.Name, "Details", "Customers", new { id = customer.Id }, null)</td>
```

* In *Views/Customers* add the *Details* view, using the *~/Views/Shared/_Layout.cshtml* layout. Set the model to *Vidly.Models.Customer*, the title to the *Model.Name* and display the *Model.Name*.

```
@model Vidly.Models.Customer

@{
    ViewBag.Title = Model.Name;
    Layout = "~/Views/Shared/_Layout.cshtml";
}

<h2>@Model.Name</h2>
```


&nbsp;
## 11 Add *Details* for movies

* In the *MoviesController*, add the *Details* method. It returns an *ActionResult* which provides a view of a single movie, whose Id matches the method's input parameter.

```
public ActionResult Details(int id)
{
      var movie = GetMovies().SingleOrDefault(m => m.Id == id);

      if (movie == null)
          return HttpNotFound();

      return View(movie);
}
```

* In *Views/Movies/Index* replace the code that displays each movie's *Name* with an *ActionLink* which displays the *movie.Name*, invokes the *Details* method from the *Movies* controller and assigns the *movie.Id* on the method's *id* argument.

```
<td>@Html.ActionLink(movie.Name, "Details", "Movies", new { id = movie.Id }, null)</td>
```

* In *Views/Movies* add the *Details* view, using the *~/Views/Shared/_Layout.cshtml* layout. Set the model to *Vidly.Models.Movie*, the title to the *Model.Name* and display the *Model.Id* and the *Model.Name*.

```
@model Vidly.Models.Movie

@{
    ViewBag.Title = Model.Name;
    Layout = "~/Views/Shared/_Layout.cshtml";
}

<h3>Details for movie <b>@Model.Name</b></h3>
<p>Id: @Model.Id</p>
<p>Name: @Model.Name</p>
```


## 12 Enable migrations

```
PM> enable-migrations
```

```
PM> add-migration InitialModel
```

```
PM> update-database
```

* In the created migration we can see calls to the CreateTable method for tables referring to users and roles, coming from the ASP.NET Identity framework.
Inside IdentityModels.cs, the ApplicationDbContext class inherits from IdentityDbContext. Entity framework located the auto generated Identity DbSets while creating the migration. No references to our domain classes (Movie, Customer) are found.


&nbsp;
## 13 Add the DbSet for *Customers*

* Inside IdentityModels.cs, in ApplicationDbContext, add the DbSet for *Customers*.

```
public DbSet<Customer> Customers { get; set; }
```

```
PM> Add-migration AddCustomersTable
```
```
PM> update-database
```


&nbsp;
## 14 Add a property to Customer

* Add the IsSubscribedToNewsletter property to the Customer model.

```
public bool IsSubscribedToNewsletter { get; set; }
```

```
PM> add-migration AddIsSubscribedToNewsLetterToCustomer
```
```
PM> update-database
```


&nbsp;
## 15 Add the MembershipType model

* Add the MembershipType class in the models folder.

```
    public class MembershipType
    {
        public byte Id { get; set; }
        public short SignUpFee { get; set; }
        public byte DurationInMonths { get; set; }
        public byte DiscountRate { get; set; }
    }
```

* MembershipType has to be associated with the Customer class. Add the MembershipType navigational property to the Customer class as well as the MembershipTypeId property which is recognized by convention by the Entity framework as a foreign key.

```
    public class Customer
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public bool IsSubscribedToNewsletter { get; set; }
        public MembershipType MembershipType { get; set; }
        public byte MembershipTypeId { get; set; }
    }
```

```
PM> add-migration AddMembershipType
```
```
PM> update-database
```


&nbsp;
## 16 Populate MembershipTypes with a migration

* Create an empty migration.

```
PM> add-migration PopulateMembershipTypes
```
```
public partial class PopulateMembershipTypes : DbMigration
    {
        public override void Up()
        {
        }

        public override void Down()
        {
        }
    }
```

* Then call the *Sql* method and type in SQL statements.

```
public override void Up()
{
    Sql("INSERT INTO MembershipTypes (Id, SignUpFee, DurationInMonths, DiscountRate) VALUES (1, 0, 0, 0)");
    Sql("INSERT INTO MembershipTypes (Id, SignUpFee, DurationInMonths, DiscountRate) VALUES (2, 30, 1, 10)");
    Sql("INSERT INTO MembershipTypes (Id, SignUpFee, DurationInMonths, DiscountRate) VALUES (3, 90, 3, 15)");
    Sql("INSERT INTO MembershipTypes (Id, SignUpFee, DurationInMonths, DiscountRate) VALUES (4, 300, 12, 20)");
}
```

* By updating, we can seed the MembershipTypes data to the database.

```
PM> update-database
```


&nbsp;
## 17 Override conventions with data annotations

* In the Customer model, apply attributes with DataAnnotations in order to make Name not nullable and set its string length to 255.

```
using System.ComponentModel.DataAnnotations;
```

```
    public class Customer
    {
        public int Id { get; set; }
        [Required]
        [StringLength(255)]
        public string Name { get; set; }
        public bool IsSubscribedToNewsletter { get; set; }
        public MembershipType MembershipType { get; set; }
        public byte MembershipTypeId { get; set; }
    }
```

```
PM> add-migration ApplyAnnotationsToCustomerName
```
```
PM> update-database
```


&nbsp;
## 18 Get customers from a database


* In Web.config, edit the connection string in order to point to the database of our choice.

```
<connectionStrings>    
  <add name="DefaultConnection"
    connectionString="Data Source=(LocalDb)\MSSQLLocalDB;Initial Catalog=Vidly20171202;Integrated Security=SSPI"
    providerName="System.Data.SqlClient" />
</connectionStrings>
```


* In CustomersController.cs, declare a dbContext in order to access the database. Add a constructor (ctor +Tab) and initialize the dbContext inside it. Override the dispose method of the base controller class and call Dispose() on the dbContext.

```
    private ApplicationDbContext _context;

    public CustomersController()
    {
        _context = new ApplicationDbContext();
    }

    protected override void Dispose(bool disposing)
    {
        _context.Dispose();
    }
```


* Instead of calling the *GetCustomers* method, initialize the *customers* variable using the dbContext's DbSet *Customers*. Force immediate execution of the query by calling the *ToList* method.

```
    public ViewResult Index()
    {
        var customers = _context.Customers.ToList();

        return View(customers);
    }
```


* In the *Details* action, we must also replace the *GetCustomers* method and get the customers from the database by use of the dbContext.

```
    public ActionResult Details(int id)
    {
        var customer = _context.Customers.SingleOrDefault(c => c.Id == id);

        if (customer == null)
            return HttpNotFound();

        return View(customer);
    }
```


* Run the project. The database declared in Web.config (Vidly20171202) will be created. If we browse the Customers tab, we see that there are no customers in the database, as dictated by the *@if (!Model.Any())* condition in the Views/Customers/Index.cshtml view.

```
There are no customers yet.
```


* Add some customer data manually since these are not reference data (unlike the MembershipType data we seeded previously).

* Now the customer index and detail pages can load customer data from the database. The movie index and detail still receive data created with the GetMovies helper seed method.



&nbsp;
## 19 Eager load customer membership type

* Edit the *Views/Customers/Index.cshtml* in order to display the customers' discount rate, which is described in the MembershipType DbSet.

```
<table class="table table-bordered table-hover">
  <thead>
      <tr>
          <th>Customer</th>
          <th>Discount Rate</th>
      </tr>
  </thead>
  <tbody>
      @foreach (var customer in Model)
      {
          <tr>
              <td>@Html.ActionLink(customer.Name, "Details", "Customers", new { id = customer.Id }, null)</td>
              <td>@customer.MembershipType.DiscountRate%</td>
          </tr>
      }
  </tbody>
</table>
```


* If we attempt to run this page we will get a null reference exception error because the MembershipType object is null. By default Entity framework loads only the declared objects and not their related objects.

* Use the *Include* method in the Index action in CustomersController.cs in order to enable Eager Loading for the Customers and their MembershipTypes.

```
using System.Data.Entity;
```

```
    public ViewResult Index()
    {
        var customers = _context.Customers.Include(c => c.MembershipType).ToList();

        return View(customers);
    }
```


&nbsp;
## 20 Apply names to MembershipTypes

* Edit the MembershipType model. Add the Name property. Make it non nullable.

```
using System.ComponentModel.DataAnnotations;
```

```
    public class MembershipType
    {
        public byte Id { get; set; }
        [Required]
        public string Name { get; set; }
        public short SignUpFee { get; set; }
        public byte DurationInMonths { get; set; }
        public byte DiscountRate { get; set; }
    }
```

* Apply the migration.
```
PM> add-migration AddNameToMembershipType
```
```
PM> update-database
```

* Apply another migration in order to update the database entries with the appropriate names.

```
PM> add-migration SetNamesOfMembershipTypes
```

* In the empty migration, use the Sql command on order to apply SQL update commands and set the names.

```
public override void Up()
{
    Sql("UPDATE MembershipTypes SET Name = 'Pay as You Go' WHERE Id = 1");
    Sql("UPDATE MembershipTypes SET Name = 'Monthly' WHERE Id = 2");
    Sql("UPDATE MembershipTypes SET Name = 'Quarterly' WHERE Id = 3");
    Sql("UPDATE MembershipTypes SET Name = 'Annual' WHERE Id = 4");
}
```
```
PM> update-database
```

* Edit the Views/Customers/Index.cshtml in order to display the customers' membership type name.

```
<table class="table table-bordered table-hover">
  <thead>
      <tr>
          <th>Customer</th>
          <th>Membership Type</th>
          <th>Discount Rate</th>
      </tr>
  </thead>
  <tbody>
      @foreach (var customer in Model)
      {
          <tr>
              <td>@Html.ActionLink(customer.Name, "Details", "Customers", new { id = customer.Id }, null)</td>
              <td>@customer.MembershipType.Name</td>
              <td>@customer.MembershipType.DiscountRate%</td>
          </tr>
      }
  </tbody>
</table>
```
