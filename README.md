# 📊 Student Performance Analytics — Power BI Workshop

Welcome to the **Student Performance Analytics** Power BI workshop.

In this workshop, you will work with academic data and use **Power BI** to move from raw data to meaningful insights.

The goal is not to create the prettiest dashboard.

The goal is to learn how to:

**Understand Data → Ask Questions → Analyze → Visualize → Find Insights**

---

# 🎯 Workshop Objective

You are given data related to students, their academic performance, attendance, study habits, backlogs, and subject-level marks.

Your task is to explore the data and answer questions such as:

* How are students performing academically?
* Which branches or years show different patterns?
* Is attendance related to academic performance?
* Where are backlogs concentrated?
* What additional patterns can we discover from subject-level data?

You will first work with the raw data and then use **Power BI** to investigate it more efficiently.

---

# 📁 Repository Contents

This repository contains the datasets required for the workshop.

```text
student-performance-powerbi/
│
├── students.csv
├── subject_marks.csv
└── README.md
```

You do **not** need to create the datasets yourself.

---

# 📌 Dataset Overview

The project contains two CSV files.

## 1. `students.csv`

This file contains **one row per student**.

It contains information such as:

| Column                  | Description                |
| ----------------------- | -------------------------- |
| `Student_ID`            | Unique student identifier  |
| `Branch`                | Student's academic branch  |
| `Year`                  | Current year of study      |
| `Semester`              | Current semester           |
| `Gender`                | Student gender             |
| `Study_Hours_Per_Week`  | Average weekly study hours |
| `Attendance_Percentage` | Attendance percentage      |
| `Assignment_Average`    | Average assignment marks   |
| `Internal_Marks`        | Aggregate internal marks   |
| `External_Marks`        | Aggregate external marks   |
| `CGPA`                  | Student CGPA               |
| `Backlogs`              | Number of backlogs         |
| `Placement_Status`      | Placement status           |

The file contains **520 students**.

---

## 2. `subject_marks.csv`

This file contains **subject-level academic performance**.

Each student has multiple subject records.

| Column           | Description                            |
| ---------------- | -------------------------------------- |
| `Student_ID`     | Student identifier                     |
| `Subject_Name`   | Subject name                           |
| `Semester`       | Semester in which the subject is taken |
| `Internal_Marks` | Internal marks out of 40               |
| `External_Marks` | External marks out of 60               |
| `Total_Marks`    | Total marks out of 100                 |
| `Grade`          | Grade obtained                         |
| `Result_Status`  | Pass or Fail                           |

The file contains **1,040 subject records**.

---

# 🔗 Understanding the Relationship

Both datasets contain:

```text
Student_ID
```

This is the key used to connect the two tables.

The relationship is:

```text
students
   1
   │
   │ Student_ID
   │
   *
subject_marks
```

In Power BI, the relationship should be:

**`students[Student_ID]` → `subject_marks[Student_ID]`**

with:

* `students` on the **one** side
* `subject_marks` on the **many** side

This relationship allows you to combine student-level information with subject-level performance.

---

# 🧩 PART 1 — THE DATA CHALLENGE

Before jumping into Power BI, try to understand the data yourself.

Open `students.csv`.

You may use:

* Excel
* Google Sheets
* Python
* Calculator
* Any basic data-analysis method available to you

### Your challenge:

Try to find useful patterns in the dataset.

For example:

### Academic Performance

* Which branch has the highest average CGPA?
* Which branch has the lowest average CGPA?
* What is the overall average CGPA?

### Attendance

* What is the average attendance?
* Which year has the lowest average attendance?
* Does attendance appear to have any relationship with CGPA?

### Study Habits

* Do students who study more tend to have higher CGPAs?
* Are there students who study fewer hours but still perform well?

### Backlogs

* How many students have at least one backlog?
* Which branch appears to have the highest backlog problem?
* Are backlogs concentrated in any particular group?

### Placement

* How does placement status vary with CGPA?
* Do students with backlogs appear differently from students without backlogs?

---

## 💡 Important

You are **not expected to find every answer**.

The purpose of this activity is to experience the problem of working with raw data.

A spreadsheet can contain hundreds of rows and still not directly tell you:

> **"What should I do about this?"**

That is where data analysis begins.

---

# 🚀 PART 2 — BUILD YOUR FIRST POWER BI REPORT

Now open **Power BI Desktop**.

Import:

```text
students.csv
subject_marks.csv
```

Make sure the data types are appropriate.

For example:

* `Student_ID` → Text
* `Branch` → Text
* `Year` → Whole Number
* `Semester` → Whole Number
* `Attendance_Percentage` → Decimal/Whole Number
* `CGPA` → Decimal Number
* `Backlogs` → Whole Number
* Marks → Numeric

Then create the relationship between the two tables using `Student_ID`.

---

# 📄 PAGE 1 — LIVE WORKSHOP TASK

During the live workshop, you will build **Page 1**.

The purpose of this page is to answer a broad question:

> **"What is happening in the student population?"**

You should investigate overall academic performance, attendance, and backlogs.

### Your dashboard should help you investigate things such as:

* Overall number of students
* Overall academic performance
* Overall attendance
* Students with backlogs
* Differences between branches
* Differences between years
* Relationship between attendance and CGPA
* Distribution of backlogs

You are free to decide **how you want to visualize these questions**.

There is no prescribed dashboard layout.

### Think about:

> What type of visual would make this question easiest to understand?

For example:

* Is the question asking for a single number?
* Is it comparing categories?
* Is it showing a relationship?
* Is it showing change across groups?
* Does the user need to filter the result?

The important part is the **question**, not the chart.

---

# 🧮 DAX MEASURES — PAGE 1

You can use the following measures while building Page 1.

Create these as **New Measures** in Power BI.

---

## 1. Total Students

**Used on:** Page 1

```DAX
Total Students =
DISTINCTCOUNT(students[Student_ID])
```

This gives the number of unique students.

---

## 2. Average CGPA

**Used on:** Page 1

```DAX
Average CGPA =
AVERAGE(students[CGPA])
```

This calculates the average CGPA of the currently filtered students.

---

## 3. Average Attendance

**Used on:** Page 1

```DAX
Average Attendance =
AVERAGE(students[Attendance_Percentage])
```

This calculates average attendance.

You can format the result according to how you want to display the value.

---

## 4. Students With Backlogs

**Used on:** Page 1

```DAX
Students With Backlogs =
CALCULATE(
    DISTINCTCOUNT(students[Student_ID]),
    students[Backlogs] > 0
)
```

This counts students who have **at least one backlog**.

> Do not simply sum the `Backlogs` column if your question is "How many students have backlogs?"

Those are two different questions.

---

## 5. Backlog Rate

**Used on:** Page 1

```DAX
Backlog Rate =
DIVIDE(
    CALCULATE(
        DISTINCTCOUNT(students[Student_ID]),
        students[Backlogs] > 0
    ),
    DISTINCTCOUNT(students[Student_ID])
)
```

This calculates the percentage of students with at least one backlog.

---

# 🎛️ INTERACTIVITY

Try adding filters that allow the report user to explore the data.

For example, you may want users to investigate:

* Branch
* Year
* Semester

But don't simply add filters because they are available.

Ask:

> **"Will this filter help someone investigate the question?"**

---

# 🔎 PAGE 1 — THINK LIKE AN ANALYST

Once your first report is working, don't stop at:

> "The chart is showing numbers."

Ask:

### What does the data actually tell us?

For example:

* Is one branch noticeably different?
* Is there a year where attendance changes?
* Do higher-attendance students generally have higher CGPAs?
* Are backlogs concentrated in a particular branch?
* Are there exceptions to the patterns?

Remember:

> **A visualization is not automatically an insight.**

You still have to interpret what you are seeing.

---

# 🧠 IMPORTANT: CORRELATION ≠ CAUSATION

If you observe that two variables appear related, be careful about your conclusion.

For example:

> "Students with higher attendance tend to have higher CGPA."

is different from:

> "Higher attendance causes higher CGPA."

The dataset can show patterns and relationships.

It does not automatically prove why those relationships exist.

---

# 🏠 PART 3 — TAKE-HOME EXERCISE

## PAGE 2 — SUBJECT PERFORMANCE DEEP DIVE

After the workshop, use `subject_marks.csv` to investigate the next question:

> **"Where exactly is the academic problem?"**

Page 1 gives you an overall picture.

Now go deeper.

Your job is to investigate **subject-level performance**.

---

# 🎯 PAGE 2 QUESTIONS

Try to answer questions such as:

### Subject Performance

* What is the average subject score?
* Which subjects have lower average marks?
* Which subjects have higher failure rates?

### Pass / Fail

* What percentage of subject records are passes?
* What percentage are failures?
* Which subjects have unusually high failure rates?

### Internal vs External

Investigate:

* How do internal marks compare with external marks?
* Do students who perform well internally also perform well externally?
* Are there cases where the two differ significantly?

### Drill Down

Investigate performance at different levels:

```text
Branch
   ↓
Year
   ↓
Semester
   ↓
Subject
```

Try to find whether a problem that looks small at the overall level becomes more noticeable when you investigate a specific academic segment.

---

# 🧮 DAX MEASURES — PAGE 2

These measures are provided for the take-home exercise.

---

## 1. Average Subject Score

**Used on:** Page 2

```DAX
Average Subject Score =
AVERAGE(subject_marks[Total_Marks])
```

---

## 2. Passed Subject Records

**Used on:** Page 2

```DAX
Passed Subject Records =
CALCULATE(
    COUNTROWS(subject_marks),
    subject_marks[Result_Status] = "Pass"
)
```

---

## 3. Failed Subject Records

**Used on:** Page 2

```DAX
Failed Subject Records =
CALCULATE(
    COUNTROWS(subject_marks),
    subject_marks[Result_Status] = "Fail"
)
```

---

## 4. Pass Rate

**Used on:** Page 2

```DAX
Pass Rate =
DIVIDE(
    CALCULATE(
        COUNTROWS(subject_marks),
        subject_marks[Result_Status] = "Pass"
    ),
    COUNTROWS(subject_marks)
)
```

Format this measure as a percentage.

---

## 5. Fail Rate

**Used on:** Page 2

```DAX
Fail Rate =
DIVIDE(
    CALCULATE(
        COUNTROWS(subject_marks),
        subject_marks[Result_Status] = "Fail"
    ),
    COUNTROWS(subject_marks)
)
```

Format this measure as a percentage.

---

# 🕵️ THE TAKE-HOME CHALLENGE

Don't just build a collection of charts.

Try to find **one interesting academic problem** hidden inside the subject-level data.

For example:

> An overall subject may appear acceptable, but does its performance change significantly for a particular branch, year, or semester?

Investigate.

Then ask:

1. **What did you find?**
2. **Where does it occur?**
3. **How large is the difference?**
4. **What additional question would you ask next?**

You don't have to prove why the problem exists.

Your job is to **identify and communicate the pattern**.

---

# 💡 OPTIONAL CHALLENGE

Once you finish Page 2, try creating a small section that identifies students who may require additional academic attention.

You could investigate combinations such as:

* Low attendance
* Low CGPA
* Backlogs

Then think about:

> **How could an institution use this information?**

Remember that an analytical result can support a decision without automatically determining the decision.

---

# 🎨 DESIGN YOUR OWN REPORT

There is intentionally **no fixed dashboard layout** in this repository.

You decide:

* Which visual to use
* Where to place it
* How to arrange the page
* Which filters are useful
* How to format the report
* How to communicate the insight

Focus on answering the questions first.

You can improve the visual design later.

### Recommended workflow:

```text
Question
   ↓
Choose relevant data
   ↓
Analyze
   ↓
Choose an appropriate visual
   ↓
Find the pattern
   ↓
Explain the insight
```

---

# ⚠️ COMMON MISTAKES TO AVOID

### 1. Choosing a chart before understanding the question

Don't start with:

> "Which chart looks cool?"

Start with:

> "What am I trying to find out?"

---

### 2. Using too many visuals

More charts do not automatically mean more analysis.

Every visual should have a reason to exist.

---

### 3. Confusing counts and rates

For example:

**Students With Backlogs**

is different from:

**Backlog Rate**

One is a count.

The other is a percentage.

---

### 4. Treating correlation as causation

A relationship between two variables does not automatically explain why the relationship exists.

---

### 5. Stopping after building the dashboard

The dashboard is not the final answer.

Ask:

> **"So what?"**

What did you actually learn?

---

# 🧭 WHAT YOU SHOULD BE ABLE TO DO AFTER THIS

By the end of this exercise, you should have practiced:

* Importing CSV data into Power BI
* Understanding tables and columns
* Creating relationships
* Creating DAX measures
* Building basic visualizations
* Using filters/slicers
* Exploring relationships between variables
* Drilling from high-level information into detailed data
* Identifying patterns
* Turning visual observations into insights

Most importantly:

> **You should start thinking about data in terms of questions, not charts.**

---

# 🚀 WHERE TO GO NEXT

This workshop uses a relatively small academic dataset.

Real analytics projects can involve:

* SQL databases
* Millions of records
* Data cleaning pipelines
* Multiple data sources
* APIs
* Data warehouses
* Advanced DAX
* Automated reporting
* Business intelligence systems

Power BI is one part of that larger ecosystem.

A typical analytics workflow can look like:

```text
Data Source
    ↓
SQL / Data Preparation
    ↓
Data Model
    ↓
Power BI
    ↓
DAX
    ↓
Visualization
    ↓
Insights
    ↓
Decision
```

Your goal is not to memorize Power BI features.

Your goal is to learn how to move through this process.

---

# ⭐ FINAL CHALLENGE

The next time someone gives you a spreadsheet, don't immediately ask:

> **"Which chart should I make?"**

Ask:

> **"What question can this data answer?"**

Then build from there.

---

## DATA → ANALYSIS → INSIGHT → DECISION

**Happy analyzing! 📊**
