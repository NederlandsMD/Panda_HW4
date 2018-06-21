
# PyCity Schools Analysis

- **Observable Trend 1**: The top 5 performing schools are all charter schools, the bottom 5 are all district schools
- **Observable Trend 2**: The average reading score is higher than the average math score at every school in the city
- **Observable Trend 3**: District schools performed far worse across the board in math and reading scores than charter schools <br>
  (And... the lowest scores were also observed among schools that spend the most per student, and the largest schools)


```python
#import dependencies

import pandas as pd 
import numpy as np
```


```python
#read in the school data and have a first look

school_file = "raw_data/schools_complete.csv"
schools_df = pd.read_csv(school_file)
schools_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>name</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
    </tr>
  </tbody>
</table>
</div>




```python
#read in the student file and have a first look 

student_file = "raw_data/students_complete.csv"
students_df = pd.read_csv(student_file)
students_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Student ID</th>
      <th>name</th>
      <th>gender</th>
      <th>grade</th>
      <th>school</th>
      <th>reading_score</th>
      <th>math_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Paul Bradley</td>
      <td>M</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>66</td>
      <td>79</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Victor Smith</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>94</td>
      <td>61</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Kevin Rodriguez</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>90</td>
      <td>60</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Dr. Richard Scott</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>67</td>
      <td>58</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Bonnie Ray</td>
      <td>F</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>97</td>
      <td>84</td>
    </tr>
  </tbody>
</table>
</div>



# DISTRICT SUMMARY


```python
#If I want to set index to school id, just do schools_raw.set_index("School ID")
Total_schools = len(schools_df["name"].unique())

Total_students = schools_df["size"].sum()

Total_budget = schools_df["budget"].sum()
# now we'll have to mask in currency form

avg_math = students_df["math_score"].sum()/students_df["name"].count()
avg_math = round(avg_math, 2)

avg_reading = students_df["reading_score"].sum()/students_df["name"].count()
avg_reading = round(avg_reading, 2)

passing_math = len(students_df.loc[students_df["math_score"]>70])/len(students_df["name"])*100
passing_math = round(passing_math, 2)

passing_reading = len(students_df.loc[students_df["reading_score"]>70])/len(students_df["name"])*100
passing_reading = round(passing_reading, 2)

overall_passing_rate = (passing_math +  passing_reading) / 2

# Create the table
Dist_summary = pd.DataFrame({"Total Schools":[Total_schools], "Total Students":[Total_students], 
                             "Total Budget":[Total_budget], "Average Math Score":[avg_math], 
                             "Average Reading Score":[avg_reading], "% Passing Math": [passing_math],
                            "% Passing Reading": [passing_reading], "% Overall Passing Rate":[overall_passing_rate]})

Dist_summary["Total Students"] = Dist_summary["Total Students"].map("{:,}".format)
Dist_summary["Total Budget"] = Dist_summary["Total Budget"].map("${:,.2f}".format)

org_dist_summary = Dist_summary[["Total Schools", "Total Students", "Total Budget", "Average Math Score", "Average Reading Score",
                                "% Passing Math", "% Passing Reading", "% Overall Passing Rate"]]

org_dist_summary
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Schools</th>
      <th>Total Students</th>
      <th>Total Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15</td>
      <td>39,170</td>
      <td>$24,649,428.00</td>
      <td>78.99</td>
      <td>81.88</td>
      <td>72.39</td>
      <td>82.97</td>
      <td>77.68</td>
    </tr>
  </tbody>
</table>
</div>



# SCHOOL SUMMARY


```python
#Start with the schools data frame we have and clean it up
schools_df.head()
schools_df.set_index("School ID")
schools_summ = schools_df.rename(columns={"name": "School Name", "type": "School Type", "size": "Total Students", 
                                          "budget":"Total School Budget"})
#Create the Per Student Budget Column
schools_summ["Per Student Budget"] = schools_summ["Total School Budget"] / schools_summ["Total Students"]
schools_summ = schools_summ.set_index("School ID")
```


```python
#Find the average and passing scores for each school, store them in lists
av_m = []
av_r = []
pass_m = []
pass_r = []
overall = []
for school in schools_summ["School Name"]:
    my_school_frame = students_df.loc[students_df["school"]==school,:]
    av_m.append(my_school_frame["math_score"].sum()/my_school_frame["name"].count())
    av_r.append(my_school_frame["reading_score"].sum()/my_school_frame["name"].count())
    pass_math = len(my_school_frame.loc[my_school_frame["math_score"]>70])/len(my_school_frame["name"])*100
    pass_m.append(pass_math)   
    pass_read = len(my_school_frame.loc[my_school_frame["reading_score"]>70])/len(my_school_frame["name"])*100
    pass_r.append(pass_read)
    overall.append((pass_math + pass_read)/2)
```


```python
# Now append those lists to our data frame, and set the index as the school name to match the presentation image
schools_summ["Average Math Score"] = av_m
schools_summ["Average Reading Score"] = av_r
schools_summ["% Passing Math"] = pass_m
schools_summ["% Passing Reading"] = pass_r
schools_summ["% Overall Passing Rate"] = overall
schools_summ_present = schools_summ.set_index("School Name")
schools_summ_present["Total Students"] = schools_summ_present["Total Students"].map("{:,}".format)
schools_summ_present["Total School Budget"] = schools_summ_present["Total School Budget"].map("${:,.2f}".format)
schools_summ_present["Per Student Budget"] = schools_summ_present["Per Student Budget"].map("${:,.2f}".format)
del schools_summ_present.index.name
schools_summ_present
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2,917</td>
      <td>$1,910,635.00</td>
      <td>$655.00</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>63.318478</td>
      <td>78.813850</td>
      <td>71.066164</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2,949</td>
      <td>$1,884,411.00</td>
      <td>$639.00</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>63.750424</td>
      <td>78.433367</td>
      <td>71.091896</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>Charter</td>
      <td>1,761</td>
      <td>$1,056,600.00</td>
      <td>$600.00</td>
      <td>83.359455</td>
      <td>83.725724</td>
      <td>89.892107</td>
      <td>92.617831</td>
      <td>91.254969</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4,635</td>
      <td>$3,022,020.00</td>
      <td>$652.00</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>64.746494</td>
      <td>78.187702</td>
      <td>71.467098</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1,468</td>
      <td>$917,500.00</td>
      <td>$625.00</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>89.713896</td>
      <td>93.392371</td>
      <td>91.553134</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2,283</td>
      <td>$1,319,574.00</td>
      <td>$578.00</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>90.932983</td>
      <td>93.254490</td>
      <td>92.093736</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1,858</td>
      <td>$1,081,356.00</td>
      <td>$582.00</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>89.558665</td>
      <td>93.864370</td>
      <td>91.711518</td>
    </tr>
    <tr>
      <th>Bailey High School</th>
      <td>District</td>
      <td>4,976</td>
      <td>$3,124,928.00</td>
      <td>$628.00</td>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>64.630225</td>
      <td>79.300643</td>
      <td>71.965434</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>Charter</td>
      <td>427</td>
      <td>$248,087.00</td>
      <td>$581.00</td>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>90.632319</td>
      <td>92.740047</td>
      <td>91.686183</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962</td>
      <td>$585,858.00</td>
      <td>$609.00</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>91.683992</td>
      <td>92.203742</td>
      <td>91.943867</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>Charter</td>
      <td>1,800</td>
      <td>$1,049,400.00</td>
      <td>$583.00</td>
      <td>83.682222</td>
      <td>83.955000</td>
      <td>90.277778</td>
      <td>93.444444</td>
      <td>91.861111</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3,999</td>
      <td>$2,547,363.00</td>
      <td>$637.00</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>64.066017</td>
      <td>77.744436</td>
      <td>70.905226</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4,761</td>
      <td>$3,094,650.00</td>
      <td>$650.00</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>63.852132</td>
      <td>78.281874</td>
      <td>71.067003</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>District</td>
      <td>2,739</td>
      <td>$1,763,916.00</td>
      <td>$644.00</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>65.753925</td>
      <td>77.510040</td>
      <td>71.631982</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>Charter</td>
      <td>1,635</td>
      <td>$1,043,130.00</td>
      <td>$638.00</td>
      <td>83.418349</td>
      <td>83.848930</td>
      <td>90.214067</td>
      <td>92.905199</td>
      <td>91.559633</td>
    </tr>
  </tbody>
</table>
</div>



# Top Performing Schools (by Passing Rate)


```python
Top_5_df = schools_summ_present.sort_values("% Overall Passing Rate", ascending = False)
Top_5_df = Top_5_df.iloc[0:5,:]
del Top_5_df.index.name
Top_5_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2,283</td>
      <td>$1,319,574.00</td>
      <td>$578.00</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>90.932983</td>
      <td>93.254490</td>
      <td>92.093736</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962</td>
      <td>$585,858.00</td>
      <td>$609.00</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>91.683992</td>
      <td>92.203742</td>
      <td>91.943867</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>Charter</td>
      <td>1,800</td>
      <td>$1,049,400.00</td>
      <td>$583.00</td>
      <td>83.682222</td>
      <td>83.955000</td>
      <td>90.277778</td>
      <td>93.444444</td>
      <td>91.861111</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1,858</td>
      <td>$1,081,356.00</td>
      <td>$582.00</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>89.558665</td>
      <td>93.864370</td>
      <td>91.711518</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>Charter</td>
      <td>427</td>
      <td>$248,087.00</td>
      <td>$581.00</td>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>90.632319</td>
      <td>92.740047</td>
      <td>91.686183</td>
    </tr>
  </tbody>
</table>
</div>



# Bottom Performing Schools (by Passing Rate)


```python
Bottom_5_df = schools_summ_present.sort_values("% Overall Passing Rate", ascending = True)
Bottom_5_df = Bottom_5_df.iloc[0:5,:]
del Bottom_5_df.index.name
Bottom_5_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3,999</td>
      <td>$2,547,363.00</td>
      <td>$637.00</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>64.066017</td>
      <td>77.744436</td>
      <td>70.905226</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2,917</td>
      <td>$1,910,635.00</td>
      <td>$655.00</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>63.318478</td>
      <td>78.813850</td>
      <td>71.066164</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4,761</td>
      <td>$3,094,650.00</td>
      <td>$650.00</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>63.852132</td>
      <td>78.281874</td>
      <td>71.067003</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2,949</td>
      <td>$1,884,411.00</td>
      <td>$639.00</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>63.750424</td>
      <td>78.433367</td>
      <td>71.091896</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4,635</td>
      <td>$3,022,020.00</td>
      <td>$652.00</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>64.746494</td>
      <td>78.187702</td>
      <td>71.467098</td>
    </tr>
  </tbody>
</table>
</div>



# Average Math Scores by Grade


```python
#Find the average math score grouped by school and grade
grade_students = students_df.groupby(["school", "grade"])
math_score_gs = grade_students.mean()
math_score_gs.reset_index(level=0, inplace=True)

#Use a loop to read the average scores into separated lists by grade
schools = []
grade_ten = []
grade_eleven = []
grade_twelve = []
grade_nine = []
for x in range(0,len(math_score_gs["math_score"]),4):
    schools.append(math_score_gs.iloc[x,0])
    grade_ten.append(math_score_gs.iloc[x,3])
    grade_eleven.append(math_score_gs.iloc[x+1,3])
    grade_twelve.append(math_score_gs.iloc[x+2,3])
    grade_nine.append(math_score_gs.iloc[x+3,3])
```


```python
#Bundle those lists together in a new data frame
Math_grades = pd.DataFrame({"School": schools, "9th Grade": grade_nine, "10th Grade": grade_ten,
                            "11th Grade": grade_eleven, "12th Grade":grade_twelve})
Math_grades = Math_grades.set_index("School")
Math_grades = Math_grades[["9th Grade", "10th Grade", "11th Grade", "12th Grade"]]
del Math_grades.index.name
Math_grades
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>9th Grade</th>
      <th>10th Grade</th>
      <th>11th Grade</th>
      <th>12th Grade</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>77.083676</td>
      <td>76.996772</td>
      <td>77.515588</td>
      <td>76.492218</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.094697</td>
      <td>83.154506</td>
      <td>82.765560</td>
      <td>83.277487</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>76.403037</td>
      <td>76.539974</td>
      <td>76.884344</td>
      <td>77.151369</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>77.361345</td>
      <td>77.672316</td>
      <td>76.918058</td>
      <td>76.179963</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>82.044010</td>
      <td>84.229064</td>
      <td>83.842105</td>
      <td>83.356164</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>77.438495</td>
      <td>77.337408</td>
      <td>77.136029</td>
      <td>77.186567</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.787402</td>
      <td>83.429825</td>
      <td>85.000000</td>
      <td>82.855422</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>77.027251</td>
      <td>75.908735</td>
      <td>76.446602</td>
      <td>77.225641</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>77.187857</td>
      <td>76.691117</td>
      <td>77.491653</td>
      <td>76.863248</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.625455</td>
      <td>83.372000</td>
      <td>84.328125</td>
      <td>84.121547</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>76.859966</td>
      <td>76.612500</td>
      <td>76.395626</td>
      <td>77.690748</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>83.420755</td>
      <td>82.917411</td>
      <td>83.383495</td>
      <td>83.778976</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.590022</td>
      <td>83.087886</td>
      <td>83.498795</td>
      <td>83.497041</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.085578</td>
      <td>83.724422</td>
      <td>83.195326</td>
      <td>83.035794</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.264706</td>
      <td>84.010288</td>
      <td>83.836782</td>
      <td>83.644986</td>
    </tr>
  </tbody>
</table>
</div>



# Average Reading Scores by Grade


```python
#Find the average reading score grouped by school and grade
grade_students = students_df.groupby(["school", "grade"])
read_score_gs = grade_students.mean()
read_score_gs.reset_index(level=0, inplace=True)

#Use a loop to read the average scores into separated lists by grade
schools = []
grade_ten = []
grade_eleven = []
grade_twelve = []
grade_nine = []
for x in range(0,len(read_score_gs["reading_score"]),4):
    schools.append(read_score_gs.iloc[x,0])
    grade_ten.append(read_score_gs.iloc[x,2])
    grade_eleven.append(read_score_gs.iloc[x+1,2])
    grade_twelve.append(read_score_gs.iloc[x+2,2])
    grade_nine.append(read_score_gs.iloc[x+3,2])
    
#Bundle those lists together in a new data frame
Read_grades = pd.DataFrame({"School": schools, "9th Grade": grade_nine, "10th Grade": grade_ten,
                            "11th Grade": grade_eleven, "12th Grade":grade_twelve})
Read_grades = Read_grades.set_index("School")
Read_grades = Read_grades[["9th Grade", "10th Grade", "11th Grade", "12th Grade"]]
del Read_grades.index.name
Read_grades
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>9th Grade</th>
      <th>10th Grade</th>
      <th>11th Grade</th>
      <th>12th Grade</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>81.303155</td>
      <td>80.907183</td>
      <td>80.945643</td>
      <td>80.912451</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.676136</td>
      <td>84.253219</td>
      <td>83.788382</td>
      <td>84.287958</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>81.198598</td>
      <td>81.408912</td>
      <td>80.640339</td>
      <td>81.384863</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>80.632653</td>
      <td>81.262712</td>
      <td>80.403642</td>
      <td>80.662338</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>83.369193</td>
      <td>83.706897</td>
      <td>84.288089</td>
      <td>84.013699</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>80.866860</td>
      <td>80.660147</td>
      <td>81.396140</td>
      <td>80.857143</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.677165</td>
      <td>83.324561</td>
      <td>83.815534</td>
      <td>84.698795</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>81.290284</td>
      <td>81.512386</td>
      <td>81.417476</td>
      <td>80.305983</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>81.260714</td>
      <td>80.773431</td>
      <td>80.616027</td>
      <td>81.227564</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.807273</td>
      <td>83.612000</td>
      <td>84.335938</td>
      <td>84.591160</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>80.993127</td>
      <td>80.629808</td>
      <td>80.864811</td>
      <td>80.376426</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>84.122642</td>
      <td>83.441964</td>
      <td>84.373786</td>
      <td>82.781671</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.728850</td>
      <td>84.254157</td>
      <td>83.585542</td>
      <td>83.831361</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.939778</td>
      <td>84.021452</td>
      <td>83.764608</td>
      <td>84.317673</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.833333</td>
      <td>83.812757</td>
      <td>84.156322</td>
      <td>84.073171</td>
    </tr>
  </tbody>
</table>
</div>



# Scores by School Spending


```python
# Use the data frame schools_summ, create bins, then group the data by these bins
ss = schools_summ
bins = [560, 585, 610, 635, 660]
bin_names = ["\$560-\$585", "\$585-\$610", "\$610-\$635", "\$635-\$660"]
Binning = pd.cut(ss["Per Student Budget"], bins, labels=bin_names)
ss["Spending Range per Student"] = Binning
spending_bins = ss.groupby("Spending Range per Student")
spend_bin = spending_bins.mean()
spend_bin = spend_bin[["Average Math Score", "Average Reading Score", "% Passing Math", "% Passing Reading", "% Overall Passing Rate"]]
spend_bin
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>Spending Range per Student</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>\$560-\$585</th>
      <td>83.455399</td>
      <td>83.933814</td>
      <td>90.350436</td>
      <td>93.325838</td>
      <td>91.838137</td>
    </tr>
    <tr>
      <th>\$585-\$610</th>
      <td>83.599686</td>
      <td>83.885211</td>
      <td>90.788049</td>
      <td>92.410786</td>
      <td>91.599418</td>
    </tr>
    <tr>
      <th>\$610-\$635</th>
      <td>80.199966</td>
      <td>82.425360</td>
      <td>77.172061</td>
      <td>86.346507</td>
      <td>81.759284</td>
    </tr>
    <tr>
      <th>\$635-\$660</th>
      <td>77.866721</td>
      <td>81.368774</td>
      <td>67.957362</td>
      <td>80.268067</td>
      <td>74.112715</td>
    </tr>
  </tbody>
</table>
</div>



# Scores by School Size


```python
# Use the data frame schools_summ, create bins, then group the data by these bins
bins = [0, 1000, 2000, 5000]
bin_names = ["Small (<1000)", "Medium (1000-2000)", "Large (2000-5000)"]
Binning = pd.cut(ss["Total Students"], bins, labels=bin_names)
ss["School Size"] = Binning
sizing_bins = ss.groupby("School Size")
size_bin = sizing_bins.mean()
size_bin = size_bin[["Average Math Score", "Average Reading Score", "% Passing Math", "% Passing Reading", "% Overall Passing Rate"]]
size_bin
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>School Size</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Small (&lt;1000)</th>
      <td>83.821598</td>
      <td>83.929843</td>
      <td>91.158155</td>
      <td>92.471895</td>
      <td>91.815025</td>
    </tr>
    <tr>
      <th>Medium (1000-2000)</th>
      <td>83.374684</td>
      <td>83.864438</td>
      <td>89.931303</td>
      <td>93.244843</td>
      <td>91.588073</td>
    </tr>
    <tr>
      <th>Large (2000-5000)</th>
      <td>77.746417</td>
      <td>81.344493</td>
      <td>67.631335</td>
      <td>80.190800</td>
      <td>73.911067</td>
    </tr>
  </tbody>
</table>
</div>



# Scores by School Type


```python
# School type is already there, no need to bin, just groupby
type_bins = ss.groupby("School Type")
type_df = type_bins.mean()
type_df = type_df[["Average Math Score", "Average Reading Score", "% Passing Math", "% Passing Reading", "% Overall Passing Rate"]]
type_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>School Type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Charter</th>
      <td>83.473852</td>
      <td>83.896421</td>
      <td>90.363226</td>
      <td>93.052812</td>
      <td>91.708019</td>
    </tr>
    <tr>
      <th>District</th>
      <td>76.956733</td>
      <td>80.966636</td>
      <td>64.302528</td>
      <td>78.324559</td>
      <td>71.313543</td>
    </tr>
  </tbody>
</table>
</div>


