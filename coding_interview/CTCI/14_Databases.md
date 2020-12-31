# Databases

### SQL Syntax and Variation

+ Explicit Join:

  ```sql
  SELECT CourseName, TeacherName
  FROM Courses INNER JOIN Teachers
  ON Courses.TeacherID = Teachers.TeacherID
  ```

+ Implicit Join

  ```sql
  SELECT CourseName, TeacherName
  FROM Courses, Teachers
  WHERE Courses.TeacherID = Teachers.TeacherID
  ```

#### Example

+ Database (* : primary key)

  ```sql
  Courses: CourseID*, CourseName, TeacherID
  Teachers: TeacherID*, TeacherName
  Student: StudentID*, StudentName
  StudentCourses: CourseID*, StudentID*
  ```

+ Query 1: Student Enrollment: get a list of all students and how may courses each student is enrolled in

  ```sql
  SELECT StudentName, Students.StudentID, Cnt
  FROM(
  	SELECT Students.StudentID, count(StudentCourses.CourseID) as [Cnt]
      FROM Student LEFT JOIN StudentCourses
      ON Student.StudentID = StudentCourses.StudentID
      GROUP BY Student.StudentID
  ) T INNER JOIN Students on T.studentID = Students.StudentID
  ```

  

### Denormalized vs. Normalized databases

+ Normalized: minimize redundancy. 
  + one table contains a column of IDs which is a foreign key to another table
+ Denormalized: optimized read time
  + Store information in another table again in the first table

p 181



p 453



1-4