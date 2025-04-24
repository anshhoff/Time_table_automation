# Working of the Automated Timetable Scheduler

## Introduction
This document provides a detailed walkthrough of how to set up and run the Automated Timetable Scheduler project. It includes setup instructions, execution steps, and expected outputs to help users understand the working of the project.

## Setup Instructions
1. **Clone the Repository**: Ensure you have cloned the project repository to your local machine.
2. **Install Dependencies**: Make sure all dependencies are installed as specified in the `pom.xml` or `build.gradle` file.
3. **Prepare Data**: Create CSV files for faculties, rooms, courses, and student batches as per the required format.

## CSV File Format and Structure
The project requires specific CSV files to properly function. These need to be in the following format:

1. **faculty.csv**:
   ```
   id,name,email,password,subjects,maxHoursPerDay
   1,Dr. John Smith,john.smith@example.com,password,"Mathematics,Physics",8
   2,Dr. Jane Doe,jane.doe@example.com,password,"Computer Science,Data Structures",6
   ```

2. **rooms.csv**:
   ```
   id,roomNumber,capacity,roomType
   1,101,30,LECTURE_ROOM
   2,201,60,LECTURE_ROOM
   3,301,25,COMPUTER_LAB
   ```

3. **courses.csv**:
   ```
   id,courseCode,name,courseType,batchIds,lectureHours,theoryHours,practicalHours,credits,eligibleFacultyIds
   1,CS101,Introduction to Programming,Regular,"1,2",3,2,1,4,"1,2"
   2,MATH201,Calculus,Regular,"1",3,3,0,3,"1"
   ```

4. **studentBatches.csv**:
   ```
   id,batchName,year,strength,courseIds,lectureRoomIds,practicalRoomIds
   1,CSE-A,2023,60,"1,2","1,2","3"
   2,CSE-B,2023,55,"1","1,2","3"
   ```

## Updating CSV Files with Latest Data
To ensure your timetable reflects the most current academic data:

1. **Gather Latest Information**: Collect up-to-date information about faculty availability, course offerings, room allocations, and student enrollments.

2. **Update CSV Files**: Edit the CSV files in the `data` directory with the latest information. Make sure to follow the exact format shown above.

3. **Check for Consistency**: Ensure there are no conflicts in your data (e.g., assigning unavailable faculty, rooms that don't exist).

4. **Format Validation**: Verify that all CSV files maintain proper formatting with correct separators and escape characters where needed.

## Execution Steps
1. **Open the Project**: Launch your IDE (e.g., IntelliJ IDEA) and open the project directory.
2. **Build the Project**: Use the build tool to compile the project.
3. **Run the Solver**:
   - Navigate to the main class containing the `public static void main(String[] args)` method.
   - Execute the main method to start the solver.

## Generating the Final Timetable CSV

The system will process your input CSV files and generate a final timetable CSV file that represents the optimized schedule. This file will contain all lesson assignments with their corresponding time slots, rooms, and faculty.

1. **Output Location**: By default, the generated CSV file will be saved in the `output` directory as `timetable_solution.csv`.

2. **CSV Format**: The output file will have the following structure:
   ```
   Day,Time,Course,Faculty,Room,StudentBatch
   Monday,09:00-10:00,Introduction to Programming,Dr. John Smith,101,CSE-A
   Monday,10:00-11:00,Calculus,Dr. Jane Doe,201,CSE-A
   ```

3. **Viewing Results**: You can open the generated CSV file with any spreadsheet program (e.g., Microsoft Excel, Google Sheets) to view and further analyze the timetable.

4. **Distribution**: This CSV can be easily shared with faculty and students or imported into other systems for display and distribution.

## Expected Output
Upon successful execution, the solver will generate a timetable that meets all specified constraints. The output will be displayed in the console and exported to the CSV file as described above.

## Sample Code
Below is a sample code snippet to demonstrate how to run the solver and generate the CSV file:

```java
// Load data from CSV files
List<Faculty> facultyList = CSVDataLoader.loadFaculty("data/faculty.csv");
List<Room> roomList = CSVDataLoader.loadRooms("data/rooms.csv");
List<Course> courseList = CSVDataLoader.loadCourses("data/courses.csv", facultyList);
List<StudentBatch> batchList = CSVDataLoader.loadStudentBatches("data/studentBatches.csv", courseList);

// Create the timetable problem
TimeTable problem = TimeTableFactory.createTimeTable(courseList, facultyList, roomList, batchList);

// Solve the problem
SolverFactory<TimeTable> solverFactory = SolverFactory.create(new SolverConfig().withSolutionClass(TimeTable.class));
Solver<TimeTable> solver = solverFactory.buildSolver();
TimeTable solution = solver.solve(problem);

// Output the solution to console
System.out.println("Solved Timetable:");
for (Lesson lesson : solution.getLessonList()) {
    System.out.println(lesson.getTimeSlot().getDay() + " " + 
                      lesson.getTimeSlot().getStartTime() + "-" + 
                      lesson.getTimeSlot().getEndTime() + ", " + 
                      lesson.getCourse().getName() + ", " + 
                      lesson.getFaculty().getName() + ", " + 
                      lesson.getRoom().getRoomNumber() + ", " + 
                      lesson.getStudentBatch().getBatchName());
}

// Export the solution to CSV
CSVExporter.exportTimetable(solution, "output/timetable_solution.csv");
```

## Screenshots
Include any relevant screenshots of the input data, console logs, and generated timetable to provide a visual understanding of the project execution.

---

For further assistance, refer to the README file or contact the project maintainers. 