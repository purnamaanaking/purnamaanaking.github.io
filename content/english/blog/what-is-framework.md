---
title: "What Is Framework ?"
meta_title: "What Is Framework"
description: "What Is Framework"
date: 2023-08-22T05:00:00Z
image: "/images/blog/what-is-framework.png"
categories: ["Technology"]
author: "Purnama Anaking"
tags: ["framework", "mvc", "model", "view", "controller"]
draft: false
---

Framework is a collection of ready-to-use program code with certain writing rules that aim to facilitate and speed up application development. Frameworks are created to simplify the performance of programmers. Thus, a programmer does not need to write code repeatedly. Because in it only needs to compile programming components. The main purpose of using a framework is to speed up the creation of applications, because there are various ready-made features in the framework. We just need to use these features without having to create everything from scratch. In addition, the writing rules in the framework will force us to use a good way of writing (following best practice standards).

#### What Framework Does ?

- **Make Program Code More Structured**. Frameworks usually have architectural patterns in writing code. Thus, the code written is easier and more structured. As a result, you can quickly find errors and fix them right away.

- **Improving Safety**. Apart from making the code more structured, frameworks can increase the security of your website. For example, the Laravel framework has adopted various security systems such as authentication, encryption, and hashing.

- **Speed up Website Creation**. Because developers can use the components that have been provided and do not need to write code from scratch, it can speed up the creation of a website.

- **Easier Website Maintenance**. Bug fixes, maintenance to add features and improving website security will be easier because most frameworks already use various architectural patterns.

#### Framework vs Library

Both contain program code created by other programmers and can be used to speed up application development. Libraries generally contain a collection of program code for a more specific task (usually for 1 function only) so they are simpler than frameworks. For example, if we need to make statistical calculations, then we can look for libraries that contain ready-made statistical formulas. This program code is usually in the form of functions or classes. For example, dompdf is a PHP library that can be used to create pdf files and many more.

Frameworks are more complex than libraries and are used to create a whole application, not just for one task.
In practice, a framework may contain dozens of libraries that work together.
The main principle of frameworks is that we have no control over how the program code is written. Frameworks already have standardized writing rules that must be followed.

#### Types of Frameworks

There are various frameworks for different purposes:

{{< image src="images/blog/css-framework.png" caption="" alt="CSS Framework" height="" width="500" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

- **CSS Frameworks:** Bootstrap, Materialize, Bulma, Tailwind, etc.

{{< image src="images/blog/javascript-framework.png" caption="" alt="CSS Framework" height="" width="300" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

- **Javascript Frameworks:** React, Vue, Angular, etc.

{{< image src="images/blog/php-framework.png" caption="" alt="CSS Framework" height="" width="400" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

- **PHP Frameworks:** Codeigniter, Laravel, Symphony, Yii, etc.

#### Framework vs Native Language ? Always Debatable

- **Reasons not to use a framework:** the application is quite simple, don't have enough web programming basics, want to pursue performance.
- **Reasons to use a framework:** ready-to-use features available, follows best practices, easy for collaboration, easy to read program code, application security.

#### MVC Architecture (Model, View, Controller)

MVC (Model, View, Controller) is an architecture for designing program code. The goal is to break down the main program code into 3 separate components with specific tasks.

- Model → Accessing the database
- View → Display design (user interface)
- Controller → Program logic flow

The initial idea of the need for the MVC concept is so that applications can be easily managed and developed, especially for large applications. With a good implementation of the MVC concept, each part is independent of the other. If there are changes or modifications, just edit the necessary parts, not having to remodel the entire application.

MVC architecture only emphasizes the separation between Model, View and Controller. Its implemetation in each programming language may vary because the desktop programming environment is different from web programming. Especially for PHP, almost all popular PHP frameworks use MVC architecture, including Laravel. Each framework also implements MVC with different levels of strictness.

#### MVC Flow Diagram

{{< image src="images/blog/mvc.png" caption="" alt="CSS Framework" height="" width="600" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

1. **Controller** catch the request.
2. **Controller** communicate with **Model** to get data from database.
3. **Controller** forward data from model into view.
4. **View** display the data with the UI into web browser
