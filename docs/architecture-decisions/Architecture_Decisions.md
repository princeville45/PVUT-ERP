# Key Architectural Decisions PVUT ERP

These decisions represent the core logic of the Prince Victor University of Technology ERP. Every decision is driven by real world university operational constraints.

## 1. Course vs. Course Offering
A `Course` is the stable curriculum definition. A `CourseOffering` is one specific semester's run, its own lecturer, venue, capacity, and timetable. `Registration` always references a Course Offering, never the abstract Course. The same course can legitimately run as multiple parallel offerings in the same semester (different lecturer, room, and time each).

## 2. Electives as Groups, Not Flags
Marking a course `elective = true` loses the actual business rule. Restricted electives (a department approved list, "choose 2 from these 4") and special electives (a university wide catalogue with a required unit count) are modeled as their own `ElectiveGroup` entity, so curriculum changes update one row instead of every affected student.

## 3. Carry Over as a Chain, Not a Flag on Student
An outstanding academic requirement traces back to *why* it exists: `ExamResult → Failed → OutstandingAcademicRequirement → NextEligibleCourseOffering`. A failed second semester course only reappears in a second semester offering, never out of season. Carry over limits are policy configurable. The system warns and allows, rather than hard rejecting, based on institutional policy.

## 4. Payments Settle Obligations, Not People
`Payment` never references `Student` directly. It references an `ExpectedCharge`. Finance doesn't invent debt. It records settlement. This means partial payments, multiple simultaneous obligations (tuition, hostel, library fine), and audit trails all resolve unambiguously. Payments are always student initiated. The university never deducts funds automatically. Duplicate payment protection and a `pending` intermediate state (rather than an immediate success or fail assumption) prevent double charging on retry or network failure.

## 5. Results Are Independent Transactions
A bulk result upload is not one giant transaction. Each student's result commits independently. If a connection drops mid upload, the results that saved are real and stand. Only the remainder needs resubmission.

## 6. Graduation Is One Committed Fact
Certificate printing, Alumni record creation, NYSC notification, and email are all downstream reactions to a single committed graduation transaction. None of them can undo it if they fail.

## 7. Clearance Is a Generic, Pluggable Contract
Graduation never queries Finance, Library, and Hostel individually. It asks one shared `Clearance` module a single question. Any department, including ones that don't exist yet, registers itself as a `ClearanceRequirement` and reports status through a generic event. Adding a new clearance requiring department next year requires zero changes to Graduation's code.

## 8. Waiting Lists Are FIFO, Never Races
A freed seat (a course offering, a hostel bed) goes to the first person in a properly ordered queue, notified with a claim window, never thrown open for the fastest click or best network connection to win.

## 9. Role Based Access Control, Least Privilege
Every role sees only what its responsibility requires. A department secretary sees student biodata for their own department only, never medical or finance records. A lecturer sees their own students' relevant data, not university wide access. For this implementation, the project owner holds System Administrator and ICT Administrator roles with full system visibility.
