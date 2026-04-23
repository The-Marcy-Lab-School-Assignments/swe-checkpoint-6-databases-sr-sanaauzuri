# Short Response: Databases Checkpoint

Answer each question below in complete sentences. Aim for 3–5 sentences per answer — enough to show that you understand the concept, not just that you memorized a definition. Use the exact terms and concepts from the lessons, but write in your own words. Specific examples and analogies are encouraged.

---

## Question 1

What is the difference between **authentication** and **authorization**? Give a concrete example of how a user would encounter each in the context of a fullstack web application.

**Your answer:**

**Authentication** tells an application who you are, while **authorization** tells an application what you are allowed to do.  Authentication in a fullstack web application is when a user logins to the application and always happens before authorization because the server has to know who someone is to determine what they are allowed to do. Authorization in a full stack application is when a user requests to update their password, this always happens after authentication because the server has to verify that the person updating the password is the owner of that resource.

## Question 2

Why should passwords **never** be stored as plaintext in a database? Explain what hashing is and its key properties that allow a server to verify a password without ever storing the original?

**Your answer:**

Passwords should **never** be stored as plaintext in a database because if the database is ever breached, the passwords are easily readable. Most users reuse passwords across other sites, so a breach of an application can also compromise a user’s credit/debit, email, health, and employment accounts. To solve this, we **hash** passwords. **Hashing** is a process that uses a mathematical algorithm to convert a string, usually a password, into an irreversible fixed-length string called a hash. This *“process”* of hashing a password is executed through a **hashing function**. Every hashing function must be **one-way** and **pure**. One-way meaning you cannot reverse-engineer the hashed password.  Pure meaning consistent, the input of a password always returns the same hashed password. For instance, if you have a function that is pure but not one-way, a malicious user can reverse the hash and access the user’s password. Also, if you have a function that is one way and not pure, the database can’t compare/verify the user’s password because the output for a password will differ each time it is hashed.  This makes it impossible to compare a hash to its original string.

## Question 3

Explain what it means when we say that "HTTP is stateless"? Explain why cookies are necessary in order to keep users logged-in across multiple sessions and how a server and a client work together to achieve this functionality.

**Your answer:**

When we say that "HTTP is stateless" it means that the client and the server have no memory of previous requests. Cookies are necessary in order to keep users logged in because 

## Question 4

A frontend can hide a "Delete Account" button from users who aren't logged in. Why isn't that enough to protect the `DELETE /api/users/:id` route on the server? What are the two layers of protection that the backend implements to protect against this?

**Your answer:**

That isn't enough to protect the `DELETE /api/users/:id` route on the server because a person with network access can bypass the frontend and access your API directly through any HTTP client such as curl, or a browser extension that can call on an endpoint. The backend implements `checkAuthentication`, also called **Authorization middleware**, that protects routes that require login. This includes routes to *update*, *create*, or *delete* owner-based resources and **excludes** public routes, such as requests to see/get all posts. Ownership-based authorization, checks that the logged-in user owns a resource before allowing them to *update*, *delete*, or *create* that resource.


## Question 5

What is **SQL injection**? Explain what makes the code below unsafe, then describe how parameterized queries fix the problem.

```js
// Unsafe — never do this!
pool.query(`SELECT * FROM users WHERE username = '${username}'`);
```

**Your answer:**

**SQL injection** is when a malicious user can pass in something like `DROP TABLE` *`tablename`* as a value that requires user input, and without a parameterized query, this is executed and can delete a table from your database.  This code is unsafe because the SQL query: `SELECT * FROM users WHERE username =` and the user input: `'${username}'` aren’t separate. So a malicious user can pass in a destructive query into username and it executes because Postgres treats the value and entire string as a SQL command. A **parameterized query** uses placeholders like `$NUM` instead of directly inserting user input into a SQL string. This solves **SQL injection**, a common and destructive web vulnerability. Using placeholders keeps the SQL string and the values separate, so Postgres treats the user input as a value to be compared against your table rather than as a command to run. This means even if a malicious user tries to pass in a destructive query, it will not execute.

## Question 6

What problem does the **`/api/auth/me`** endpoint pattern solve? When does the frontend call it and what does it return?

**Your answer:**

---
The **`/api/auth/me`** endpoint pattern allows the database to authenticate users based on their session cookie, instead of requiring users to use their username and password on requests to update, delete, or create *(manage)* their own resources. Requiring users to do this after logging in to their account, is excessive and unnecessary. The frontend calls this endpoint in the case of a user requesting to update their password, delete their account, or create a post. The endpoint pattern returns the current user from session *(or 401: if the session is missing)*.