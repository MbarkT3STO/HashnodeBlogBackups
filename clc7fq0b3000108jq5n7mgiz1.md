# How To Configure Many-To-Many Relationship in EF Core?

One of the features of EF Core is its ability to handle many-to-many relationships, which occur when two entities have a relationship with each other through a third entity. In this article, we will explore how to configure a many-to-many relationship in EF Core using Fluent API and show an example of how to create, read, update, and delete records in a many-to-many relationship.

## **Setting up the Project**

Before we can start configuring a many-to-many relationship in EF Core, we need to set up a new project and install the necessary dependencies.

1. Create a new [ASP.NET](http://ASP.NET) Core web application using Visual Studio or the dotnet command-line interface.
    
2. Install the following NuGet packages:
    
    * Microsoft.EntityFrameworkCore
        
    * Microsoft.EntityFrameworkCore.SqlServer
        
3. Create a model for each entity in the many-to-many relationship. For example, let's say we have a `Student` model and a `Course` model and they have a many-to-many relationship through a `StudentCourse` model. The models should look something like this:
    

```csharp
public class Student
{
    public int StudentId { get; set; }
    public string Name { get; set; }
    public virtual ICollection<StudentCourse> StudentCourses { get; set; }
}

public class Course
{
    public int CourseId { get; set; }
    public string Name { get; set; }
    public virtual ICollection<StudentCourse> StudentCourses { get; set; }
}

public class StudentCourse
{
    public int StudentId { get; set; }
    public Student Student { get; set; }
    public int CourseId { get; set; }
    public Course Course { get; set; }
}
```

## **Configuring the Many-To-Many Relationship**

Now that we have our models set up, we can configure the many-to-many relationship using EF Core's Fluent API.

1. Create a `DbContext` class and add a `DbSet` property for each of the entities.
    

```csharp
public class ApplicationDbContext : DbContext
{
    public DbSet<Student> Students { get; set; }
    public DbSet<Course> Courses { get; set; }
    public DbSet<StudentCourse> StudentCourses { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=ManyToManyExample;Trusted_Connection=True;");
    }
}
```

1. Use the Fluent API to configure the many-to-many relationship in the `OnModelCreating` method of the `DbContext` class.
    

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<StudentCourse>()
        .HasKey(sc => new { sc.StudentId, sc.CourseId });

    modelBuilder.Entity<StudentCourse>()
        .HasOne(sc => sc.Student)
        .WithMany(s => s.StudentCourses)
        .HasForeignKey(sc => sc.StudentId);

    modelBuilder.Entity<StudentCourse>()
        .HasOne(sc => sc.Course)
        .WithMany(c => c.StudentCourses)
        .HasForeignKey(sc => sc.CourseId);
}
```

Here, we are specifying that the `StudentCourse` entity has a composite primary key made up of the `StudentId` and `CourseId` properties. We are also specifying that the `Student` and `Course` entities have a many-to-many relationship through the `StudentCourse` entity using the `WithMany` and `HasForeignKey` methods.

## **Working with the Many-To-Many Relationship**

Now that we have our models and DbContext set up, we can start working with the many-to-many relationship using EF Core's APIs. Here are some examples of how to create, read, update, and delete records in a many-to-many relationship:

### **Creating Records**

To create a new student and assign them to a course, we can use the following code:

```csharp
using (var context = new ApplicationDbContext())
{
    var student = new Student { Name = "John Smith" };
    var course = new Course { Name = "Math 101" };
    student.StudentCourses = new List<StudentCourse> { new StudentCourse { Course = course } };
    context.Students.Add(student);
    context.SaveChanges();
}
```

This will create a new student with the name "John Smith" and a new course with the name "Math 101", and it will create a new record in the `StudentCourse` table to link the student and course together.

### **Reading Records**

To retrieve a list of courses for a student, we can use the following code:

```csharp
using (var context = new ApplicationDbContext())
{
    var studentId = 1;
    var courses = context.Students
        .Where(s => s.StudentId == studentId)
        .SelectMany(s => s.StudentCourses)
        .Select(sc => sc.Course)
        .ToList();
}
```

This will retrieve all the courses for the student with the specified `studentId`.

### **Updating Records**

To update a student's courses, we can use the following code:

```csharp
using (var context = new ApplicationDbContext())
{
    var studentId = 1;
    var student = context.Students
        .Include(s => s.StudentCourses)
        .FirstOrDefault(s => s.StudentId == studentId);
    student.StudentCourses = new List<StudentCourse> { new StudentCourse { CourseId = 1 }, new StudentCourse { CourseId = 2 } };
    context.SaveChanges();
}
```

### **Deleting Records**

To delete a student and their courses, we can use the following code:

```csharp
using (var context = new ApplicationDbContext())
{
    var studentId = 1;
    var student = context.Students
        .Include(s => s.StudentCourses)
        .FirstOrDefault(s => s.StudentId == studentId);
    context.Students.Remove(student);
    context.SaveChanges();
}
```

This will delete the student with the specified `studentId` and all the records in the `StudentCourse` table that link the student to their courses.

## **Conclusion**

In this article, we learned how to configure a many-to-many relationship in EF Core using Fluent API and how to create, read, update, and delete records in a many-to-many relationship. EF Core makes it easy to work with many-to-many relationships and helps developers write clean, maintainable code when interacting with a database.