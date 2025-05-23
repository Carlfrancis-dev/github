# github

# Student Academic System

# Class to store student information and calculate results
class Student:
    def __init__(self, name, age, course, grades):
        self.name = name
        self.age = age
        self.course = course                                                                                                                                                                                                                                    
        self.grades = grades  # List of grades
        self.average = self.calculate_average()
        self.passed = self.check_pass_fail()

    def calculate_average(self):
        # Calculate average of grades
        return sum(self.grades) / len(self.grades) if self.grades else 0

    def check_pass_fail(self):
        # Determine pass/fail based on average >= 75
        return self.average >= 75

# Recursive function to calculate factorial 
def factorial(n):
    if n == 0 or n == 1:
        return 1
    else:
        return n * factorial(n - 1)

# Function to search for a student by name 
def search_student(students, name):
    for student in students:
        if student.name.lower() == name.lower():
            return student
    return None

# Function to sort students by average grade (highest to lowest)
def sort_students(students):
    return sorted(students, key=lambda student: student.average, reverse=True)

# Linked list implementation to track attendance (student names)
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None

    def append(self, data):
        new_node = Node(data)
        if not self.head:
            self.head = new_node
            return
        last = self.head
        while last.next:
            last = last.next
        last.next = new_node

    def display(self):
        current = self.head
        attendance_list = []
        while current:
            attendance_list.append(current.data)
            current = current.next
        return attendance_list

# Binary Search Tree (BST) for storing and searching student names as IDs
class TreeNode:
    def __init__(self, student_id):
        self.student_id = student_id
        self.left = None
        self.right = None

class BST:
    def __init__(self):
        self.root = None

    def insert(self, student_id):
        if self.root is None:
            self.root = TreeNode(student_id)
        else:
            self._insert_rec(self.root, student_id)

    def _insert_rec(self, node, student_id):
        if student_id < node.student_id:
            if node.left is None:
                node.left = TreeNode(student_id)
            else:
                self._insert_rec(node.left, student_id)
        elif student_id > node.student_id:
            if node.right is None:
                node.right = TreeNode(student_id)
            else:
                self._insert_rec(node.right, student_id)
        # If equal, do nothing (no duplicates)

    def search(self, student_id):
        return self._search_rec(self.root, student_id)

    def _search_rec(self, node, student_id):
        if node is None or node.student_id == student_id:
            return node
        if student_id < node.student_id:
            return self._search_rec(node.left, student_id)
        return self._search_rec(node.right, student_id)

# Max Heap implementation to track top grades
import heapq

class MaxHeap:
    def __init__(self):
        self.heap = []

    def push(self, grade):
        # Push negative grade to simulate max heap with min heap
        heapq.heappush(self.heap, -grade)

    def top_n(self, n):
        # Return top n grades sorted descending
        return sorted([-x for x in self.heap], reverse=True)[:n]

# Function to display students in a neat table format
def display_students_table(students):
    header = f"{'Name':<15} {'Age':<5} {'Course':<15} {'Grades':<20} {'Average':<8} {'Pass/Fail':<10}"
    print(header)
    print("-" * len(header))
    for s in students:
        grades_str = ", ".join(str(g) for g in s.grades)
        status = 'Pass' if s.passed else 'Fail'
        print(f"{s.name:<15} {s.age:<5} {s.course:<15} {grades_str:<20} {s.average:<8.2f} {status:<10}")

# Main program
def main():
    students = []
    attendance = LinkedList()
    bst = BST()
    max_heap = MaxHeap()

    print("Enter student details. Type 'calculate' as name to finish input.")

    # Input loop for student data
    while True:
        name = input("Enter student name: ").strip()
        if name.lower() == 'calculate':
            break
        if not name:
            print("Name cannot be empty.")
            continue

        try:
            age = int(input("Enter student age: ").strip())
        except ValueError:
            print("Invalid age. Please enter a number.")
            continue

        course = input("Enter student course: ").strip()
        if not course:
            print("Course cannot be empty.")
            continue

        grades_input = input("Enter student grades separated by space: ").strip()
        try:
            grades = list(map(int, grades_input.split()))
            if not grades:
                print("Grades cannot be empty.")
                continue
        except ValueError:
            print("Invalid grades. Please enter numbers separated by space.")
            continue

        # Create student object and add to list
        student = Student(name, age, course, grades)
        students.append(student)

        # Track attendance
        attendance.append(name)

        # Insert student name as ID in BST
        bst.insert(name)

        # Add each grade to max heap
        for grade in grades:
            max_heap.push(grade)

    # Sort students by average grade
    sorted_students = sort_students(students)

    # Output: Display sorted student table
    print("\nFinal Output:")
    display_students_table(sorted_students)

    # Output: Show top 3 highest grades across all students
    top_grades = max_heap.top_n(3)
    print(f"\nTop 3 Highest Grades: {top_grades}")

    # Display attendance list
    attendance_list = attendance.display()
    print("\nAttendance order:")
    print(", ".join(attendance_list))


    # Loop for searching students by name
    while True:
        search_name = input("\nEnter a name to search (type 'search' to quit): ").strip()
        if search_name.lower() == 'search':
            break
        found_student = search_student(students, search_name)
        if found_student:
            print(f"Found: {found_student.name}, Average: {found_student.average:.2f}, Pass: {'Yes' if found_student.passed else 'No'}")
        else:
            print("Student not found.")

if __name__ == "__main__":
    main()
