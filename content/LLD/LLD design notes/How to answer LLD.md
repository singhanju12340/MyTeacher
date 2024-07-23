**Low Level Design (LLD)** or **Object Oriented Design (OOD)** interview tests your ability to translate high-level requirements into detailed class structures, methods and their interactions using **object-oriented design principles**.

This article will guide you through a **step-by-step** approach to answer LLD interview questions effectively using an example problem: **Design Stack Overflow**.

![[Screenshot 2024-07-11 at 12.54.23 PM.png]]

## Step 1: Clarify Requirements

The first step in a LLD interview is to understand the **requirements** and **use-cases**.

Start by asking questions to understand what's required:

- What are the **core features** we need to support?
    
- Are there any specific features we should **prioritize**?
    
- Who are the **primary users** of this system?
    
- What **actions** can users take?
    
- Are there any specific **constraints** or **limitations**?
    
- Do we need to handle **concurrency**?
    
- Do we need to handle errors, edge cases, exceptions, and unexpected input?
    

For our **Stack Overflow design**, we might ask:

- Do we need comments on questions and answers?
- Should we implement tagging for questions?
- Should we design the voting system for questions and answers?
- Should we include a search functionality for questions and answers?
- Should we limit the length of questions?


Let’s assume the interviewer wants us to focus on:

- Users can post questions, answer questions, and comment on questions and answers.
- Users can vote on questions and answers.
- Questions should have tags associated with them.
- Users can search for questions based on keywords, tags, or user profiles.
- The system should assign reputation score to users based on their activity and the quality of their contributions.

## Step 2: Identify Entities

After you are clear with the requirements, break down the problem and identify the **core entities** or **objects** you need to have in the design.

Core entities are the key objects or concepts around which your system is built.

These entities will become the classes in your object-oriented design. Think of them as the **nouns** in the problem description.

For Stack Overflow, different entities we can have are:
![[Screenshot 2024-07-11 at 12.55.55 PM.png]]

## Step 3: Class Design

After identifying the core entities, the next step is to design the **classes, enums** and **interfaces** that will represent the entities in your system.

### **3.1 Define classes and relationships**

Translate entities into classes and come up with a **list of attributes** you want to have in those classes.

If your design consists of multiple classes, figure out how would they would relate with each other.

In our Stack Overflow example, here are some of the relationships between classes:

- A `User` can ask many `Questions`, provide many `Answers` and add many `Comments`.
- A `Question` can have many `Answers,` `Comments, Tags` and `Votes`.
- An `Answer` can have many `Comments` and `Votes`.
- A `Comment` is composed within a `Question` or an `Answer`.
- A `Tag` is composed within a `Question`.

You can draw a **UML class diagram** to illustrate the relationships between classes or code the class structure directly in an **object oriented programming language** of your choice.![[Screenshot 2024-07-11 at 12.56.35 PM.png]]

### **3.2 Define interfaces and core methods**

After you've outlined the classes, their attributes and the relationship between classes, the next step is to define **interfaces** and **core methods** in each of the classes.

These methods encapsulate the **actions or behaviors** that each class is responsible for. Essentially, they are the **verbs** associated with your entities.

Since both `Question` and `Answer` classes need to support **comments** and **votes**, we can define **interfaces** for these features.

Here are the interfaces we can have in our design:

- **Commentable:** Defines the contract for objects that can receive comments (eg. Question, Answer).
    
    - **addComment(comment):** Adds a comment to this object.
    - **getComments():** Retrieves all comments for this object.
- **Votable:** Defines the contract for objects that can be voted on.
    - **vote():** Registers a vote on this object.
    - **getVoteCount():** Retrieves the total vote count for this object.

Each class need to have **methods** for the tasks it can perform.

Here are the core method for Stack Overflow classes:

- `User` Class:
    
    - **askQuestion(title, content, tags):** Creates a new question asked by this user.
    - **answerQuestion(question, content):** Creates a new answer by this user for the given question.
    - **addComment(commentable, comment):** Adds a comment by this user to a question or answer.
    - **updateReputation(value):** Updates the user's reputation score.

- `Question` Class:
    
    - **addAnswer(answer):** Adds an answer to this question.
    - **addComment(comment):** Adds a comment to this question.
    - **vote(user, value):** Registers a vote on this question.
    - **addTag(user, value):** Adds a tag to this question.
- `Answer` Class:
    
    - **addComment(comment):** Adds a comment to this answer.
    - **vote(user, value):** Registers a vote on this answer.
    - **markAsAccepted():** Marks this answer as accepted.

We can have following core methods in the **StackOverflow** class:

- **createUser(username, email):** Creates and registers a new user in the system.
- **askQuestion(user, title, content, tags):** Allows a user to ask a new question.
- **answerQuestion(user, question, content):** Allows a user to answer an existing question. 
- **addComment(user, commentable, content):** Allows a user to add a comment on an existing question/answer.
- **voteQuestion(user, question, value):** Registers a vote on a question.
- **voteAnswer(user, answer, value):** Registers a vote on an answer.
- **acceptAnswer(answer):** Mark an answer as accepted
- **searchQuestions(query):** Searches for questions based on a query string.
- **getQuestionsByUser(user):** Searches questions added by a user.

### 4.1 Follow good coding practices

While writing the code for a LLD interview problem, you should try your best to follow good coding principles like:

- Use meaningful names for classes, methods, and variables.
    
- Focus on simplicity and readability.
    
- Favor composition over inheritance to promote flexibility and avoid tight coupling.
    
- Avoid duplicating code or logic.
    
- Use interfaces to define contracts and enable loose coupling between components.
    
- Only implement what is required.
    
- Strive for modularity and separation of concerns to make the codebase maintainable and scalable.
    
- Apply design principles and design patterns wherever necessary.
    
- Make your code scalable so that it performs well with large data sets.