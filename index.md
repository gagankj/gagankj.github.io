# Library Management System



## Start bench server


```bash
cd frappe-bench/
bench start
```

This will start several processes including a Python web server based on Gunicorn, redis servers for caching, job queuing and socketio pub-sub, background workers, node server for socketio and a node server for compiling JS/CSS files.

The web server will start listening on the port 8000 but we don't have any sites yet to serve. Our next step is to create our app and create a site that will have this app installed.

Run new tab in terminal by clicking on the top left button.


## Create app

To create our Library Management app, run the following command from the frappe-bench directory:

```bash
bench new-app library_management
```


## Create a new site

To create a new site, run the following command from the frappe-bench directory:

```bash
bench new-site library.test
```

This command will create a new database, so you need to enter your MySQL root password. It will also ask to set the password for the Administrator user, just set a password that you won't forget. This will be useful later.

Now, you will have a new folder named library.test in the sites directory.

## Site Directory Structure

As you can see, the `private` folder will contain any database backups and private files. Private files are user uploaded files that need authentication to be accessible.

The `public` folder will contain files that are accessible without authentication. This can contain website images that should be accessible without login.

The `site_config.json` file contains configuration that is specific to this site which should not be version controlled. This is similar to an environment variables file. If you look at the contents of the file, it contains the database configuration values for this site.

## Access Sites in you browser

Frappe will identify which site to serve by matching the hostname of the request with the site name, so you should be able to access your site on ` http://library.test:8000 ` but this won't work because we have to tell our operating system that library.test should point to localhost. To do that, you can add the following entry to your /etc/hosts file.

```bash
127.0.0.1 library.test
```

This will map library.test to localhost. Bench has a convenient command to do just that.

```bash
bench --site library.test add-to-hosts
```

This will ask for your root password and will add an entry to your /etc/hosts file.

Now you can access your site at ` http://library.test:8000 `. 

## Install App on Site

To install our Library Management app on our site, run the following command:

```bash
$ bench --site library.test install-app library_management

Installing library_management...
Updating Dashboard for library_management
```

You should see `frappe` and `library_management` as installed apps on your site.

When you create a new site, the `frappe` app is installed by default.

## Login into desk

To create DocTypes in our app, we must log in to Desk. Go to ` http://library.test:8000 ` and it should show you a login page.

![login](./assets/login_page.png)

Enter Administrator as the user and password that you set while creating the site.

![successful_login](./assets/Successful_login.png)

After successful login, you will see the setup wizard. This is a one-time setup wizard used to set up the site. Go ahead, select your country and complete the wizard.

You should see the Desk that looks something like this:

![desk](./assets/desk.png)

Good job making it this far!

## Site Commands

` https://frappe.school/courses/frappe-framework-tutorial/learn/5.6 `

---

## Create a DocType

DocType is analogous to a Model in other frameworks. Apart from defining properties, it also defines the behavior of the Model.

Before we can create DocTypes, we need to enable developer mode. This will enable boilerplate creation when we create doctypes and we can track them into version control with our app.

Go to your terminal and from the frappe-bench directory, run the following command:

```bash
bench --site library.test set-config --global developer_mode 1
```

## Creating a DocType

While in Desk, type 'doctype' in the search bar and select the **DocType List** option. You will be navigated to the DocType list where you will see a bunch of DocTypes. These are the DocTypes that are bundled with the framework.

The first doctype we will create is **Article**. To create it, click on New.

- Enter Name as Article
- Select Library Management in Module
- Add the following fields in the Fields table:
  - Article Name (Data)
  - Image (Attach Image)
  - Author (Data)
  - Description (Text Editor)
  - ISBN (Data)
  - Status (Select) - Enter two options: Issued and Available (Type Issued, hit enter, then type Available)
  - Publisher (Data)

  After adding the fields, click on Save.

  <!-- ![first_doctyoe](./assets/first_doctype.webm) -->

