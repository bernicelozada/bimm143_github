# Class 6 - R functions
Bernice Lozada (A16297973)

All functions have 3 things:

- a **name**
- input **arguments**
- the **body**

## A first silly function

Let’s write a function to add some numbers - `add()`

``` r
x <- 10
y <- 10
x + y
```

    [1] 20

``` r
add <- function(x) {
  y <- 10
  x + y
}
```

``` r
add(10)
```

    [1] 20

Making it with two inputs:

``` r
add <- function(x,y) {
  x + y
}

add(10,10)
```

    [1] 20

``` r
add(x=10,y=10)
```

    [1] 20

\##2nd example: grade() function

``` r
grade <- function(student) {
  # replaces NA with 0
  grade_edited <- ifelse(is.na(student),0, student)
  
  # find the index of the minimum score in the vector
  lowest_index = which.min(grade_edited)
  
  # remove lowest score
  student_new = grade_edited[-1*lowest_index]
  
  #average grade
  average_grade = mean(student_new)
}
```

``` r
# Example input vectors to start with
student1 <- c(100, 100, 100, 100, 100, 100, 100, 90)
student2 <- c(100, NA, 90, 90, 90, 90, 97, 80)
student3 <- c(90, NA, NA, NA, NA, NA, NA, NA)

#find the student with the highest grade
grade(student1)
grade(student2)
grade(student3)
```

Q2: Find the student with the highest grade

The `apply()` function is super useful but can be confusing to begin
with.

``` r
url <-"https://tinyurl.com/gradeinput"
hw_data <- read.csv(url, row.names = 1)

ans <- apply(hw_data, 1, grade)

which.max(ans)
```

    student-18 
            18 

Student 18 has the highest grade.

Q3. From your analysis of the gradebook, which homework was toughest on
students (i.e. obtained the lowest scores overall?

``` r
which.min(apply(hw_data, 2, mean, na.rm = TRUE))
```

    hw3 
      3 

HW 3 is the toughest homework.

Q4. Optional Extension: From your analysis of the gradebook, which
homework was most predictive of overall score (i.e. highest correlation
with average grade score)? \[1pt\]

``` r
mask <- hw_data
mask[is.na(mask)] <- 0

apply(mask, 2, cor, y= ans)
```

          hw1       hw2       hw3       hw4       hw5 
    0.4250204 0.1767780 0.3042561 0.3810884 0.6325982 

``` r
which.max(apply(mask, 2, cor, y= ans))
```

    hw5 
      5 

HW 5 has the highest correlation with average grade score.
