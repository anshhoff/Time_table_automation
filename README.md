# 🚀 Automated Timetable Scheduler

<div align="center">
  <p><em>Smart Scheduling for Modern Education</em></p>
</div>

## 🌟 Overview

The **Automated Timetable Scheduler** is a cutting-edge Java-based solution that revolutionizes academic scheduling. Powered by **OptaPlanner**, this intelligent system generates optimized, conflict-free timetables while balancing faculty workloads, student requirements, and room availability. Experience the future of academic scheduling with our advanced constraint-based optimization engine.

## ✨ Key Features

- **🤖 Smart Optimization**: Advanced constraint-based scheduling using OptaPlanner
- **📊 Comprehensive Entity Support**: Courses, Faculties, Student Batches, Rooms, Lessons, and Time Slots
- **🏫 Flexible Room Management**: Support for various room types (Lecture Rooms, Computer Labs, Hardware Labs)
- **⚖️ Workload Balancing**: Equitable distribution of teaching hours among faculty
- **🚫 Conflict Prevention**: Zero overlapping sessions for faculty, rooms, and student batches
- **📝 CSV Integration**: Seamless data import/export for all entities
- **📱 Google Sheets Integration**: Export timetables directly to Google Sheets for easy sharing
- **🎯 Real-time Updates**: Dynamic scheduling with instant conflict resolution

## 🛠️ Technical Stack

- **Java 11+**
- **OptaPlanner 8.44.0**
- **OpenCSV 5.7.1**
- **Maven**
- **Google Sheets API**

## 🚀 Quick Start

### Prerequisites
- Java 11 or higher
- Maven
- Google Sheets API credentials

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/automated-timetable-scheduler.git

# Navigate to project directory
cd automated-timetable-scheduler

# Build the project
mvn clean install
```

### Running the Application

1. **Prepare your data**:
   - Update the CSV files in the `data` directory with your institution's information
   - Follow the format specified in the [WORKING.md](WORKING.md) file

2. **Run the scheduler**:
   ```bash
   mvn exec:java -Dexec.mainClass="com.timetable.TimeTableApp"
   ```

3. **Export to Google Sheets**:
   - The generated timetable will be automatically exported to Google Sheets
   - Share the sheet with faculty and students for easy access

## 📊 Data Structure

### Input Files
- `faculty.csv`: Faculty information and availability
- `rooms.csv`: Room details and capacities
- `courses.csv`: Course information and requirements
- `studentBatches.csv`: Student batch details and course enrollments

### Output
- `timetable_solution.csv`: Generated timetable in CSV format
- Google Sheets: Interactive timetable view with filtering options

## 🎯 Features in Detail

### Smart Scheduling
- **Constraint-Based Optimization**: Hard and soft constraints for optimal scheduling
- **Faculty Preferences**: Consider faculty availability and preferred time slots
- **Room Utilization**: Maximize room usage while maintaining comfort
- **Student Convenience**: Minimize gaps between classes

### Data Management
- **CSV Import/Export**: Easy data management through CSV files
- **Data Validation**: Automatic validation of input data
- **Error Handling**: Comprehensive error checking and reporting

### Visualization
- **Google Sheets Integration**: Real-time timetable updates
- **Filtering Options**: Filter by faculty, room, or student batch
- **Conflict Highlighting**: Visual indication of scheduling conflicts

## 📈 Performance

- **Fast Processing**: Generates timetables in minutes
- **Scalable**: Handles large institutions with thousands of students
- **Reliable**: Proven track record in academic institutions

## 🤝 Contributing

We welcome contributions! Please follow these steps:

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the HARDAHA SOFTWARE LICENSE - see the [LICENSE](LICENSE) file for details.

## 📞 Contact

For questions or support, please contact Ansh Hardaha at ansh.hardaha@example.com.

## 🙏 Acknowledgments

- OptaPlanner team for their excellent constraint solver
- OpenCSV for their robust CSV handling library
- Google Sheets API for seamless integration

---

<div align="center">
  <p>Made with ❤️ by Ansh Hardaha</p>
  <p>© 2025 Automated Timetable Scheduler</p>
</div>

# Automated Timetable Scheduler

## Table of Contents
1. [Overview](#overview)
2. [Key Features](#key-features)
3. [Project Structure](#project-structure)
4. [OptaPlanner Integration](#optaplanner-integration)
5. [Data Loading](#data-loading)
6. [Usage](#usage)
7. [Running the Project Using IntelliJ IDEA](#running-the-project-using-intellij-idea)
8. [Execution Samples](#execution-samples)
9. [Contributing](#contributing)
10. [License](#license)
11. [Contact](#contact)

## Overview
The **Automated Timetable Scheduler** is a comprehensive Java-based project designed to generate optimized and conflict-free timetables for academic institutions. Using **OptaPlanner**, the project ensures that all scheduling constraints are met while balancing faculty workloads, student requirements, and room availability. This project emphasizes the use of Object-Oriented Programming (OOP) principles and advanced Java techniques, providing a robust foundation for solving real-world scheduling problems.

## Key Features

- **Constraint-Based Optimization**: Leverages OptaPlanner to solve scheduling problems with hard and soft constraints.
- **Support for Diverse Entities**: Includes Courses, Faculties, Student Batches, Rooms, Lessons, and Time Slots.
- **Enhanced Room Types**: Handles various room types such as lecture rooms, computer labs, and hardware labs.
- **Flexible Scheduling**: Supports minor courses and ensures their compatibility with time slots and rooms.
- **Balanced Workload**: Ensures equitable distribution of teaching hours among faculty.
- **Conflict-Free Schedules**: Avoids overlapping sessions for faculty, rooms, and student batches.
- **CSV Integration**: Enables easy data import/export for courses, faculty, rooms, and schedules.

---

## Project Structure

### Classes

#### **Course**
```java
public class Course {
    private Long id;
    private String courseCode;
    private String name;
    private String courseType; // Regular or elective
    private List<Integer> batchIds; // Batch identifiers
    private int lectureHours;
    private int theoryHours;
    private int practicalHours; // Hours for practical sessions
    private int credits;
    private int hoursPerWeek; // Calculated from lecture, theory, and practical hours
    private List<Faculty> eligibleFaculty; // Faculty eligible to teach the course
    private List<Long> lectureRoomIDs; // Specific to minors
    private boolean isMinor;
}
```

#### **Faculty**
```java
public class Faculty extends User {
    private List<String> subjects;
    private List<TimeSlot> preferredSlots;
    private int maxHoursPerDay;
    private boolean isAvailable;
    private List<Lesson> assignedLessons;

    // Constructor
    public Faculty(Long id, String name, String email, String password, List<String> subjects, int maxHoursPerDay) {
        super(id, name, email, password);
        this.subjects = subjects;
        this.maxHoursPerDay = maxHoursPerDay;
        this.isAvailable = true;
        this.assignedLessons = new ArrayList<>();
        this.preferredSlots = new ArrayList<>();
    }
}
```

#### **Lesson**
```java
@PlanningEntity
public class Lesson {
    @PlanningId
    private Long id;
    private Course course;
    private StudentBatch studentBatch;
    private String lessonType;
    private Faculty faculty;
    private Room room;

    @PlanningVariable(valueRangeProviderRefs = "timeSlotRange")
    private TimeSlot timeSlot;

    private List<Room> roomList;
}
```

#### **Room**
```java
public class Room {
    private Long id;
    private String roomNumber;
    private int capacity;
    private RoomType roomType;
    private boolean isAvailable;
}
```

#### **RoomType**
```java
public enum RoomType {
    LECTURE_ROOM,
    COMPUTER_LAB,
    HARDWARE_LAB,
    SEATER_120,
    SEATER_240;

    public boolean isLabRoom() {
        return false;
    }
}
```

#### **StudentBatch**
```java
public class StudentBatch {
    private Long id;
    private String batchName;
    private int year;
    private int strength;
    private List<Course> courses;
    private List<Long> lectureRoomIDs;
    private List<Long> practicalRoomIDs;

    public StudentBatch(Long id, String batchName, int year, int strength, List<Course> courses, List<Long> lectureRoomIDs, List<Long> practicalRoomIDs) {
        this.id = id;
        this.batchName = batchName;
        this.year = year;
        this.strength = strength;
        this.courses = courses != null ? courses : new ArrayList<>();
        this.lectureRoomIDs = lectureRoomIDs != null ? lectureRoomIDs : new ArrayList<>();
        this.practicalRoomIDs = practicalRoomIDs != null ? practicalRoomIDs : new ArrayList<>();
    }
}
```

#### **TimeSlot**
```java
public class TimeSlot {
    private Long id;
    private String day;
    private LocalTime startTime;
    private LocalTime endTime;
    private String slotType;

    public TimeSlot(Long id, String day, LocalTime startTime, LocalTime endTime, String slotType) {
        this.id = id;
        this.day = day;
        this.startTime = startTime;
        this.endTime = endTime;
        this.slotType = slotType;
    }
}
```

---

## OptaPlanner Integration

#### **TimeTable**
```java
@PlanningSolution
public class TimeTable {
    private Long id;

    @PlanningEntityCollectionProperty
    private List<Lesson> lessonList;

    @ProblemFactCollectionProperty
    @ValueRangeProvider(id = "facultyRange")
    private List<Faculty> facultyList;

    @ProblemFactCollectionProperty
    @ValueRangeProvider(id = "roomRange")
    private List<Room> roomList;

    @ProblemFactCollectionProperty
    @ValueRangeProvider(id = "timeSlotRange")
    private List<TimeSlot> timeSlotList;

    @PlanningScore
    private HardSoftScore score;

    public TimeTable(Long id, List<Lesson> lessonList, List<Faculty> facultyList, List<Room> roomList, List<TimeSlot> timeSlotList) {
        this.id = id;
        this.lessonList = lessonList;
        this.facultyList = facultyList;
        this.roomList = roomList;
        this.timeSlotList = timeSlotList;
    }
}
```

#### **TimeTableConstraintProvider**
```java
public class TimeTableConstraintProvider implements ConstraintProvider {
    @Override
    public Constraint[] defineConstraints(ConstraintFactory factory) {
        return new Constraint[] {
                roomConflict(factory),
                teacherConflict(factory),
                studentGroupConflict(factory),
                roomCapacity(factory),
                teacherQualification(factory),
                weeklyLabScheduling(factory),
                balanceFacultyLoad(factory),
                minimizeGapsInSchedule(factory)
                // and many more
        };
    }
}
```

---

## Data Loading
**CSVDataLoader** provides utility methods for importing data:

- `loadFaculty(String csvFile)`
- `loadRooms(String csvFile)`
- `loadCourses(String csvFile, List<Faculty> facultyList)`
- `loadStudentBatches(String csvFile, List<Course> courseList)`

---

## Usage

1. **Set Up Data**: Prepare CSV files for faculties, rooms, courses, and student batches.
2. **Run Solver**:
    ```java
    SolverFactory<TimeTable> solverFactory = SolverFactory.create(new SolverConfig());
    TimeTable solution = solver.solve(problem);
    ```
3. **Export Solution**: Save the generated timetable to a CSV file.

---

## Running the Project Using IntelliJ IDEA

To run the **Automatic Timetable Scheduler** project using IntelliJ IDEA:

1. **Open the Project**:
    - Launch IntelliJ IDEA.
    - Click on **File > Open** and select the project's root directory.

2. **Import Dependencies**:
    - Ensure that the required dependencies are listed in the `pom.xml` or `build.gradle` file.
    - IntelliJ IDEA will prompt you to import or refresh the project dependencies.

3. **Build the Project**:
    - Click on **Build > Build Project** or press `Ctrl+F9` (Windows/Linux) or `Cmd+F9` (macOS).

4. **Run the Application**:
    - Navigate to the main class **TimeTableApp** containing the `public static void main(String[] args)` method.
    - Right-click on the file and select **Run**.

5. **View Results**:
    - Check the console output or generated files for the timetable solution.

---

## Execution Samples

Screenshots of the sample executions:

![image](https://github.com/user-attachments/assets/8bf1f152-c444-4551-9915-04733520e7ba)
<img width="941" alt="image" src="https://github.com/user-attachments/assets/a254a540-555e-4f62-aa1c-db9173a67fb6" />
<img width="603" alt="image" src="https://github.com/user-attachments/assets/1084803b-e33c-48b2-9729-162582d586a5" />
<img width="1017" alt="image" src="https://github.com/user-attachments/assets/a90759da-c299-44db-98e2-5a34cda06a30" />
<img width="1025" alt="image" src="https://github.com/user-attachments/assets/7e2a482d-1552-40a8-9d4f-725ce9b36723" />


<img width="1022" alt="Screenshot 2025-04-24 at 2 45 04 PM" src="https://github.com/user-attachments/assets/629f4746-bb06-4c92-8fb5-c339aeb53f9d" />
<img width="897" alt="Screenshot 2025-04-24 at 2 45 11 PM" src="https://github.com/user-attachments/assets/939665fc-567f-4534-89f4-9ab3dbf1b6e9" />


---

## Contributing

We welcome contributions to the Automated Timetable Scheduler project. Please follow these steps:

1. Fork the repository.
2. Create a new branch for your feature or bug fix.
3. Make your changes and commit them with clear and concise messages.
4. Push your changes to your forked repository.
5. Submit a pull request to the main repository.

---

## License

This project is licensed under the HARDAHA SOFTWARE LICENSE. See the LICENSE file for more details.



