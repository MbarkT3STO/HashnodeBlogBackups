# How to Write Semantic Git Commit Messages?

Git is a powerful version control system that allows developers to track changes to their codebase. One of the key features of Git is the ability to write clear and descriptive commit messages. This can make it easier for other developers to understand what changes were made and why. In this article, we will discuss how to write semantic Git commit messages and provide examples of good commit messages.

## **Why Use Semantic Commit Messages?**

Semantic commit messages are important for several reasons. First, they make it easier for other developers to understand what changes were made and why. This can save time when reviewing code and identifying potential issues. Additionally, well-written commit messages can make it easier to track the progress of a project and understand how different features were developed.

## **The Anatomy of a Good Commit Message**

A good commit message should have a clear and concise subject line that describes the changes made in the commit. The subject line should be written in present tense and should not exceed 50 characters. Additionally, a good commit message should include a detailed description of the changes made. This can include information about the problem that was being addressed, the solution that was implemented, and any other relevant details.

## **Examples of Good Commit Messages**

Here are a few examples of good commit messages:

* **"Fix bug in login page"** - This commit message clearly states the problem (a bug in the login page) and the solution (the bug was fixed).
    
* **"Add new feature: user profiles"** - This commit message clearly states the new feature that was added (user profiles) and uses the present tense.
    
* **"Refactor code for better performance"** - This commit message clearly states the problem (poor performance) and the solution (code was refactored for better performance).
    

## Angular Conventions format

The Angular Commit Message Conventions format is a widely used format for structuring Git commit messages in Angular projects. It follows the pattern of:

```csharp
<type>(<scope>): <subject>
```

Where `type` describes the type of change made (e.g. feat, fix, docs, etc.), `scope` describes the area of the codebase affected by the change, and `subject` is a short description of the change.

Here are the different types of changes that are typically used in this format:

* **feat**: A new feature
    
* **fix**: A bug fix
    
* **docs**: Changes to documentation
    
* **style**: Changes to code formatting, white-space, punctuation, etc.
    
* **refactor**: Changes to the codebase that do not affect the public API
    
* **perf**: Changes that improve performance
    
* **test**: Adding missing tests or correcting existing tests
    
* **chore**: Changes to the build process, tooling, etc.
    

For example:

```csharp
feat(users): Add user profiles feature
```

This commit message describes a new feature that was added (user profiles) and specifies the area of the codebase affected (users).

### Examples

The following are a few examples of commit messages following the Angular Commit Message Conventions format:

* **"feat(users): Add user registration feature"** - This commit message describes a new feature that was added (user registration) and specifies the area of the codebase affected (users).
    
* **"fix(authentication): Fix error in token validation"** - This commit message describes a bug that was fixed (error in token validation) and specifies the area of the codebase affected (authentication).
    
* **"docs(installation): Update installation instructions for new dependencies"** - This commit message describes an update to documentation (installation instructions) and specifies the area of the codebase affected (new dependencies).
    
* **"refactor(search): Improve search algorithm for better performance"** - This commit message describes a refactoring of the code (the search algorithm) for the purpose of improving performance and specifies the area of the codebase affected (search).
    
* **"style(homepage): Update homepage layout and design"** - This commit message describes an update to the style of the codebase (homepage layout and design) and specifies the area of the codebase affected (homepage).
    
* **"perf(image-upload): Optimize image upload process"** - This commit message describes an update to the performance of the codebase (optimize image upload process) and specifies the area of the codebase affected (image-upload).
    
* **"test(profile): Add missing test for profile update feature"** - This commit message describes the addition of missing test for a feature (profile update) and specifies the area of the codebase affected (profile).
    
* **"chore(dependencies): Update dependencies to the latest version"** - This commit message describes a change to the build process (update dependencies) and specifies the area of the codebase affected (dependencies).
    

### Note

It is important to note that even though Angular Commit Message Conventions format is widely used, it is not the only one and it's also not set in stone. Depending on the project, the team may decide to adapt it or create a new one that better fits their needs.

## Conventional Commits

It's worth noting that while the Angular Commit Message Conventions format is widely used and accepted, it's not the only format out there, and different teams and projects may have their own conventions or preferences. The most important thing is to have a consistent and clear format for commit messages that is easily understandable by all members of the team.

Another widely used format is the "Conventional Commits" format, which is a specification for adding structured commit messages. It follows the pattern of :

```csharp
<type>(<scope>): <subject>

<body>

<footer>
```

Where `type` describes the type of change made (e.g. feat, fix, docs, etc.), `scope` describes the area of the codebase affected by the change, `subject` is a short description of the change, `body` is a more detailed explanation of the change, and `footer` is used for additional information like breaking changes, issue numbers, etc.

In any case, the most important is to ensure that the commit messages are clear and informative, and that they provide enough context for other developers to understand the changes made and their rationale.

here are a few examples of commit messages following the Conventional Commits format:

* **"feat(users): Add user registration feature"**
    

```csharp
Add user registration functionality to the application, allowing new users to create an account and log in to the system.

- Implement registration form
- Add validation for registration fields
- Add new endpoint for handling registration requests

Closes #123
```

* **"fix(authentication): Fix error in token validation"**
    

```csharp
Fix an error in the token validation process that was causing authentication to fail for some users.

- Update validation logic to correctly handle expired tokens
- Add additional logging for debugging

BREAKING CHANGE: The format of the token has changed, and old tokens will no longer be accepted by the system.
```

* **"docs(installation): Update installation instructions for new dependencies"**
    

```csharp
Update the installation instructions in the README to include new dependencies that are required for the application to run.

- Add instructions for installing new dependencies
- Update version numbers for existing dependencies
- Include links to documentation for new dependencies
```

* **"refactor(search): Improve search algorithm for better performance"**
    

```csharp
Refactor the search algorithm to improve performance and reduce the amount of time required to return results.

- Replace linear search with binary search
- Optimize query processing
- Add additional indexing for faster lookups
```

* **"style(homepage): Update homepage layout and design"**
    

```csharp
Update the layout and design of the homepage to improve user experience and make it more visually appealing.

- Update the structure of the homepage
- Add new images and graphics
- Improve typography and color scheme
```

It's important to note that this format can be adapted to the specific requirement of the project, or even the team, and this is just one way of providing a consistent and easy to understand commit message.

## **Conclusion**

Writing semantic Git commit messages is an important part of any development project. By following the guidelines outlined in this article, you can ensure that your commit messages are clear, concise, and informative. This can make it easier for other developers to understand what changes were made and why, which can save time and improve the overall quality of the codebase.