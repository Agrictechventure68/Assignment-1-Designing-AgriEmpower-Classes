# AssignmWe'll designing classes that represent key aspects of my agri_empower_learning app.

STAGE 1: DEFINING THE BASE CLASS - USER

Let's start with a User class, which is fundamental to any learning platform.

class User:
    """
    Represents a user of the agri_empower_learning app.
    """
    def __init__(self, user_id, name, email, role="Learner"):
        """
        Constructor for the User class.

        Args:
            user_id (int): The unique identifier for the user.
            name (str): The name of the user.
            email (str): The email address of the user.
            role (str, optional): The role of the user (e.g., "Learner", "Instructor", "Admin"). Defaults to "Learner".
        """
        if not isinstance(user_id, int) or user_id <= 0:
            raise ValueError("user_id must be a positive integer")
        if not isinstance(name, str) or not name.strip():
            raise ValueError("name must be a non-empty string")
        if not isinstance(email, str) or "@" not in email or "." not in email:
            raise ValueError("email must be a valid email address")
        if not isinstance(role, str) or not name.strip():
            raise ValueError("role must be a non-empty string")

        self.user_id = user_id
        self.name = name
        self.email = email
        self.role = role
        self.courses_enrolled = []  # List to store enrolled courses

    def enroll_in_course(self, course):
        """
        Enrolls the user in a course.

        Args:
            course (Course): The course object to enroll in.
        """
        if not isinstance(course, Course):  # Corrected to use the Course class
            raise TypeError("course must be an instance of Course") #Added Course Class
        if course not in self.courses_enrolled:
            self.courses_enrolled.append(course)
            print(f"User '{self.name}' enrolled in course '{course.title}'.")
        else:
            print(f"User '{self.name}' is already enrolled in course '{course.title}'.")

    def get_user_details(self):
        """
        Displays the user's details.
        """
        print(f"User ID: {self.user_id}")
        print(f"Name: {self.name}")
        print(f"Email: {self.email}")
        print(f"Role: {self.role}")
        print("Enrolled Courses:")
        if self.courses_enrolled:
            for course in self.courses_enrolled:
                print(f"- {course.title}")  # Access course title
        else:
            print("No courses enrolled.")
    def __str__(self):
        return f"User(user_id={self.user_id}, name='{self.name}', email='{self.email}', role='{self.role}')"
    def __repr__(self):
        return f"User({self.user_id}, '{self.name}', '{self.email}', '{self.role}')"

Explanation:

We define a User class with attributes like user_id, name, email, and role.

The constructor (__init__) initializes these attributes.  I've added input validation to ensure data integrity (e.g., user_id must be an integer, email must be a valid format).

The enroll_in_course method allows a user to enroll in a Course (which we'll define later).  I've added a check to prevent duplicate enrollments and type checking.

The get_user_details method displays the user's information, including enrolled courses.

Added __str__ and __repr__ methods for better string representation of User objects.

Stage 2: Creating Subclasses through Inheritance - Instructor and Admin

Now, let's create subclasses Instructor and Admin that inherit from User. This demonstrates inheritance and allows us to add specific functionalities for different user roles.

class Instructor(User):
    """
    Represents an instructor in the agri_empower_learning app.
    """
    def __init__(self, user_id, name, email, expertise):
        """
        Constructor for the Instructor class.

        Args:
            user_id (int): The unique identifier for the instructor.
            name (str): The name of the instructor.
            email (str): The email address of the instructor.
            expertise (str): The area of expertise of the instructor.
        """
        super().__init__(user_id, name, email, role="Instructor")  # Call the parent class's constructor
        if not isinstance(expertise, str) or not expertise.strip():
             raise ValueError("expertise must be a non-empty string")
        self.expertise = expertise
        self.courses_taught = [] #list of courses taught

    def create_course(self, title, description, content):
        """
        Creates a new course.

        Args:
            title (str): The title of the course.
            description (str): The description of the course.
            content (str): The content of the course.
        Returns:
            Course: The newly created Course object.
        """
        if not isinstance(title, str) or not title.strip():
            raise ValueError("Title must be a non-empty string")
        if not isinstance(description, str) or not description.strip():
            raise ValueError("Description must be a non-empty string")
        if not isinstance(content, str) or not content.strip():
            raise ValueError("Content must be a non-empty string")
        course = Course(title, description, content, instructor=self) #set instructor
        self.courses_taught.append(course)
        print(f"Instructor '{self.name}' created course '{title}'.")
        return course

    def get_instructor_details(self):
        """
        Displays the instructor's details, including expertise.
        """
        super().get_user_details()  # Call the parent class's method
        print(f"Expertise: {self.expertise}")
        print("Courses Taught:")
        if self.courses_taught:
            for course in self.courses_taught:
                print(f"- {course.title}")
        else:
            print("No courses taught.")

    def __str__(self):
        return f"Instructor(user_id={self.user_id}, name='{self.name}', email='{self.email}', expertise='{self.expertise}')"

    def __repr__(self):
        return f"Instructor({self.user_id}, '{self.name}', '{self.email}', '{self.expertise}')"

class Admin(User):
    """
    Represents an administrator in the agri_empower_learning app.
    """
    def __init__(self, user_id, name, email):
        """
        Constructor for the Admin class.

        Args:
            user_id (int): The unique identifier for the administrator.
            name (str): The name of the administrator.
            email (str): The email address of the administrator.
        """
        super().__init__(user_id, name, email, role="Admin")  # Call the parent class's constructor

    def manage_users(self):
        """
        Placeholder for user management functionality.
        """
        print("Admin is managing users...")
        # In a real application, you would implement user management logic here

    def manage_courses(self):
        """
        Placeholder for course management functionality.
        """
        print("Admin is managing courses...")
        # In a real application, you would implement course management logic here

    def __str__(self):
        return f"Admin(user_id={self.user_id}, name='{self.name}', email='{self.email}')"
    def __repr__(self):
        return f"Admin({self.user_id}, '{self.name}', '{self.email}')"

Explanation:

We define two new classes, Instructor and Admin, that inherit from User.

The __init__ methods of the subclasses call the parent class's __init__ using super().__init__(...) to initialize the inherited attributes.

Instructor adds an expertise attribute and a create_course method to create new courses.

Admin has methods for managing users and courses.  These are placeholders, but they illustrate how you can extend the base class for specific roles.

Added __str__ and __repr__ methods for better string representation.

Stage 3: Defining the Course Class

class Course:
    """
    Represents a course in the agri_empower_learning app.
    """
    def __init__(self, title, description, content, instructor=None):
        """
        Constructor for the Course class.

        Args:
            title (str): The title of the course.
            description (str): The description of the course.
            content (str): The content of the course.
            instructor (Instructor, optional): The instructor of the course. Defaults to None.
        """
        if not isinstance(title, str) or not title.strip():
            raise ValueError("Title must be a non-empty string")
        if not isinstance(description, str) or not description.strip():
            raise ValueError("Description must be a non-empty string")
        if not isinstance(content, str) or not content.strip():
            raise ValueError("Content must be a non-empty string")
        if instructor and not isinstance(instructor, Instructor):
            raise TypeError("Instructor must be an instance of Instructor")

        self.title = title
        self.description = description
        self.content = content
        self.instructor = instructor  # Store the instructor object
        self.students_enrolled = []

    def add_student(self, student):
        """
        Adds a student to the course.

        Args:
            student (User): The student to add to the course.
        """
        if not isinstance(student, User):
            raise TypeError("student must be an instance of User")
        if student not in self.students_enrolled:
            self.students_enrolled.append(student)
            print(f"Student '{student.name}' added to course '{self.title}'.")
        else:
            print(f"Student '{student.name}' is already enrolled in course '{self.title}'.")

    def get_course_details(self):
        """
        Displays the course details.
        """
        print(f"Title: {self.title}")
        print(f"Description: {self.description}")
        print(f"Content: {self.content[:50]}...")  # Show a snippet of the content
        if self.instructor:
            print(f"Instructor: {self.instructor.name}")
        else:
            print("Instructor: Not assigned")
        print("Enrolled Students:")
        if self.students_enrolled:
          for student in self.students_enrolled:
            print(f"- {student.name}")
        else:
            print("No students enrolled")

    def __str__(self):
        return f"Course(title='{self.title}', description='{self.description}')"

    def __repr__(self):
        return f"Course('{self.title}', '{self.description}')"

Explanation:

The Course class represents a course in the learning platform.

It includes attributes like title, description, content, and instructor.

The add_student method adds a student to the course's roster.

The get_course_details method displays the course's information.

Added __str__ and __repr__ methods.

Example Usage:

# Create some users
instructor1 = Instructor(101, "Dr. Alice Smith", "alice.smith@example.com", "Soil Science")
admin1 = Admin(201, "Bob Johnson", "bob.johnson@example.com")
learner1 = User(1, "John Doe", "john.doe@example.com")
learner2 = User(2, "Jane Smith", "jane.smith@example.com")

# Create a course
course1 = instructor1.create_course("Introduction to Soil Science", "Basic concepts of soil science", "Content of the course...")

# Enroll learners in the course
course1.add_student(learner1)
course1.add_student(learner2)
learner1.enroll_in_course(course1)
learner2.enroll_in_course(course1)

# Display details
print("\n--- Instructor Details ---")
instructor1.get_instructor_details()

print("\n--- Admin Details ---")
admin1.get_user_details()

print("\n--- Learner 1 Details ---")
learner1.get_user_details()

print("\n--- Course Details ---")
course1.get_course_details()

Activity 2: Polymorphism Challenge in AgriEmpower
Let's apply the concept of polymorphism to the agri_empower_learning app.  We'll consider different types of learning materials and how they are accessed.

Stage 1: Defining a Base Class or Interface with a Common Method - LearningMaterial

from abc import ABC, abstractmethod

class LearningMaterial(ABC):
    """
    Abstract base class for learning materials in the agri_empower_learning app.
    """
    def __init__(self, title):
        """
        Constructor for the LearningMaterial class.

        Args:
            title (str): The title of the learning material.
        """
        if not isinstance(title, str) or not title.strip():
            raise ValueError("Title must be a non-empty string")
        self.title = title

    @abstractmethod
    def access(self):
        """
        Abstract method to access the learning material.  Must be implemented by subclasses.
        """
        pass

    def get_title(self): #added get_title
        return self.title

Explanation:

We define an abstract base class LearningMaterial using the abc module.  Abstract base classes cannot be instantiated directly.

The __init__ method initializes the title attribute.

The access() method is declared as an abstract method using @abstractmethod.  This forces subclasses to provide their own implementation of this method.

Stage 2: Creating Concrete Resource Classes with Polymorphic access() Behavior

Now, let's create different types of learning materials that inherit from LearningMaterial and implement the access() method in their own way.

class VideoTutorial(LearningMaterial):
    """
    Represents a video tutorial.
    """
    def __init__(self, title, url):
        """
        Constructor for VideoTutorial.
        Args:
            title(str): title
            url(str): url of video
        """
        super().__init__(title)
        if not isinstance(url, str) or not url.strip():
            raise ValueError("URL must be a non-empty string.")
        self.url = url

    def access(self):
        """
        Opens the video tutorial.
        """
        print(f"Playing video tutorial '{self.title}' from {self.url}...")
        # In a real application, you would use a video player library here

class Article(LearningMaterial):
    """
    Represents a text article.
    """
    def __init__(self, title, content):
        """
        Constructor for Article.
        Args:
            title (str): The title of the article.
            content (str): The content of the article.
        """
        super().__init__(title)
        if not isinstance(content, str) or not content.strip():
            raise ValueError("Content must be a non-empty string")
        self.content = content

    def access(self):
        """
        Displays the article content.
        """
        print(f"Displaying article '{self.title}':\n{self.content[:200]}...")  # Show a snippet

class Quiz(LearningMaterial):
    """
    Represents an interactive quiz.
    """
    def __init__(self, title, questions):
        """
        Constructor for Quiz.

        Args:
            title (str): The title of the quiz.
            questions (list): A list of questions (each question could be a dictionary).
        """
        super().__init__(title)
        if not isinstance(questions, list):
            raise TypeError("questions must be a list")
        if not questions:
            raise ValueError("Questions list cannot be empty")
        self.questions = questions

    def access(self):
        """
        Starts the quiz.
        """
        print(f"Starting quiz '{self.title}'.")
        for i, question in enumerate(self.questions):
            print(f"\nQuestion {i+1}: {question}") #  Basic question display
        # In a real application, you would implement quiz interaction here

class PDFDocument(LearningMaterial):
    """Represents a PDF document."""
    def __init__(self, title, filepath):
        """
        Initializes a PDFDocument object.

        Args:
            title (str): The title of the PDF document.
            filepath (str): The file path to the PDF document.
        """
        super().__init__(title)
        if not isinstance(filepath, str):
            raise TypeError("filepath must be a string.")
        if not filepath:
            raise ValueError("filepath cannot be empty.")
        self.filepath = filepath

    def access(self):
        """Opens the PDF document."""
        print(f"Opening PDF document: {self.title} from {self.filepath}")
        # In a real application, you'd use a PDF viewer library.

Explanation:

We create four classes: VideoTutorial, Article, Quiz, and PDFDocument, all inheriting from LearningMaterial.

Each subclass provides its own implementation of the access() method:

VideoTutorial.access() simulates playing a video.

Article.access() displays a snippet of an article.

Quiz.access() starts a quiz (simplified here).

PDFDocument.access() simulates opening a PDF.

Example Usage:

# Create instances of different learning materials
video1 = VideoTutorial("Soil Preparation", "https://example.com/soil_prep.mp4")
article1 = Article("Understanding Soil pH", "Soil pH is a measure of the acidity or alkalinity of the soil...")
quiz1 = Quiz("Soil Chemistry Quiz", ["What is pH?", "What are the ideal pH levels for most crops?", "How does pH affect nutrient availability?"])
pdf_doc = PDFDocument("Fertilizer Guide", "/path/to/fertilizer_guide.pdf")

# Store them in a list
materials = [video1, article1, quiz1, pdf_doc]

# Demonstrate polymorphism
print("\n--- Accessing Learning Materials ---")
for material in materials:
    print(f"\nAccessing: {material.get_title()}")
    material.access()  # Call the access() method of the specific object

Key Improvements:

Error Handling: Added input validation to constructors to prevent errors.

Clarity: Improved comments and explanations.

Abstraction: Used abc and @abstractmethod to define a clear interface for learning materials.

Completeness: Provided a more complete example with different types of learning materials.

__str__ and __repr__: Added these methods to the classes.

Type Hinting: Could add type hints for even better code readability and maintainability (Optional).ent-1-Designing-AgriEmpower-Classes
