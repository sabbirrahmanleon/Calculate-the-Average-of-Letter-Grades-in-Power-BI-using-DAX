# Calculate the Average of Letter Grades in Power BI using DAX

Ever faced the need of calculating averages when your dataset contains letter grades (e.g., A+, A, A-) instead of numerical values?

While working on my current project, I faced a similar issue. I wanted to visualize all the columns from a table, but one of the columns contained letter grades instead of numerical values. Since Power BI can’t perform direct mathematical operations on text, I needed a way to calculate the average grade accurately. After some searching, I found the solution!

Here’s how to do it step by step 👇

**📌 Step 1: Convert Letter Grades to Numeric Values**

Since Power BI can’t average text values directly, we first map grades to numbers using this DAX measure:

```dax
Average_Grade_Value = 
VAR GradeMapping = 
    SWITCH(
        SELECTEDVALUE('Your Table Name'[Your Column Name]),
        "A+", 7,
        "A", 6,
        "A-", 5,
        "B+", 4,
        "B", 3,
        "B-", 2,
        "C", 1,
        "F", 0,
        "NA", BLANK(),
        BLANK()
    )
RETURN GradeMapping
```

🔹 This assigns a number for each grade, while ignoring "NA" values.


**📌 Step 2: Calculate the Average of Numeric Grades**

Now, let’s compute the average of the numeric values for all films:

```dax
Average_Numeric_Grade = 
AVERAGEX(
    'Your Table Name', 
    SWITCH('Your Table Name'[Your Column Name],
        "A+", 7,
        "A", 6,
        "A-", 5,
        "B+", 4,
        "B", 3,
        "B-", 2,
        "C", 1,
        "F", 0,
        "NA", BLANK(),  
        BLANK()
    )
)
```

🔹 This calculates the numeric average of all grades in the dataset.

**📌 Step 3: Convert the Numeric Average Back to a Letter Grade**

To make the output readable, we convert the calculated average back to a letter grade:

```dax
Average_Letter_Grade = 
VAR AvgValue = [Average_Numeric_Grade] 
RETURN
    SWITCH(TRUE(),
        AvgValue >= 6.5, "A+",
        AvgValue >= 5.5, "A",
        AvgValue >= 4.5, "A-",
        AvgValue >= 3.5, "B+",
        AvgValue >= 2.5, "B",
        AvgValue >= 1.5, "B-",
        AvgValue >= 0.5, "C",
        "F"  // Anything below 0.5 is treated as F
    )
```

🔹 Now, numeric averages are mapped back to their respective letter grades! You can now insert this measure to your visuals and see the results! 🎯

**Note:** The callout values may display the average numeric values instead of letter grades. To show letter grades, use a Card Visual and place the "Average_Letter_Grade" measure in it. For a demonstration, check out the results in the attached video!

**🚀 Why is This Useful?
**

✅ Allows quantitative analysis of letter-based grading systems

✅ Enables comparisons & trends in datasets with categorical scores

✅ Works seamlessly in Power BI reports & dashboards

Hope this helps everyone working with non-numeric data in Power BI! 💡






