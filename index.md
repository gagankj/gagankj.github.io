# Library Management System



## Start bench server
we can start the Frappe web server by running the following command:

    cd frappe-bench/
    bench start


This will start several processes including a Python web server based on Gunicorn, redis servers for caching, job queuing and socketio pub-sub, background workers, node server for socketio and a node server for compiling JS/CSS files.

The web server will start listening on the port 8000 but we don't have any sites yet to serve. Our next step is to create our app and create a site that will have this app installed.

Run new tab in terminal by clicking on the top left button.


## Create app

To create our Library Management app, run the following command from the frappe-bench directory:


    bench new-app library_management

App Title (default: Library Management):

App Description: Library Management System

App Publisher: Faris Ansari

App Email: faris@example.com

App Icon (default 'octicon octicon-file-directory'):

App Color (default 'grey'):

App License (default 'MIT'):

You will be prompted with details of your app, fill them up and an app named library_management will be created in the apps folder.

## Create a Site

To create a new site, run the following command from the frappe-bench directory:


    bench new-site library.test


This command will create a new database, so you need to enter your MySQL root password. It will also ask to set the password for the Administrator user, just set a password that you won't forget. This will be useful later.

Now, you will have a new folder named library.test in the sites directory.

## Access Sites in you browser

Now, that we have created our first site, we can access it on http://localhost:8000 in our browser.

However, bench allows you to create multiple sites and access them separately in the browser on the same port. This is what we call multi-tenancy support in bench.

To achieve that, we must disable the serving of the default site. Go back to your terminal and delete the file sites/currentsite.txt from the frappe-bench directory.

```bash
rm sites/currentsite.txt
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
```

To confirm if the app was installed, run the following command:.
    bench --site library.test list-apps
    
You should see `frappe` and `library_management` as installed apps on your site.


## Login into desk

To create DocTypes in our app, we must log in to Desk. Go to ` http://library.test:8000 ` and it should show you a login page.

Enter Administrator as the user and password that you set while creating the site.


![Screenshot from 2023-03-01 17-47-03](https://user-images.githubusercontent.com/103517339/222136988-2aab243b-8ddc-4ba9-868d-20c7ed294bb1.png)

After successful login, you will see the setup wizard. This is a one-time setup wizard used to set up the site. Go ahead, select your country and complete the wizard.

You should see the Desk that looks something like this:
![Screenshot from 2023-03-01 17-47-43](https://user-images.githubusercontent.com/103517339/222137160-4a850e87-117d-4576-91a0-85821d2b44c5.png)


## Enable Developer Mode

Go to your terminal and from the frappe-bench directory, run the following command:

```bash
bench set-config -g developer_mode true
bench start
```

## Creating a DocType

While in Desk, navigate to the DocType List using the Awesomebar. This list will include DocTypes bundled with the framework, those that are a part of the installed Frappe apps and custom ones, which you can create specific to each site.

The first doctype we will create is **Article**. To create it, click on New.

- Enter Name as Article
- Select Library Management in Module
- Add the following fields in the Fields table:
  - Article Name (Data,Mandatory)
  - Image (Attach Image)
  - Author (Data)
  - Description (Text Editor)
  - ISBN (Data)
  - Status (Select) - Enter two options: Issued and Available (Type Issued, hit enter, then type Available)
  - Publisher (Data)


![image](https://user-images.githubusercontent.com/103517339/223105137-93f19b9d-96f4-40be-b0ab-63668eb043bd.png)


![image](https://user-images.githubusercontent.com/103517339/223105267-6ffc2244-05de-4fc2-af11-67237bffbc8e.png)

Edit the Status Field. Refer to this image:
![image](https://user-images.githubusercontent.com/103517339/223105467-a0f1bf27-c3ca-4875-819a-715019a71e0b.png)


  After adding the fields, click on Save.
  You will see a Go to Article List button at the top right of the form. Click on it to go to the Article List. Here you will see a blank list with no records because the table has no records.

Let's create some records. But before that, we need to clear the Desk cache. Click on the Settings dropdown on the right side of the navbar and click on Reload.

Now, you should see the New button. Click on it and you will see the Form view of the Article doctype. Fill in the form and click on Save. You have created your first Article document. Go back to the list view and you should see one record.

![image](https://user-images.githubusercontent.com/103517339/223105906-2025080d-b830-4742-a8da-215fc20c856a.png)


  
  You will see a Go to Article List button at the top right of the form. Click on it to go to the Article List. Here you will see a blank list with no records because the table has no records.

Let's create some records. But before that, we need to clear the Desk cache. Click on the Settings dropdown on the right side of the navbar and click on Reload.

Now, you should see the New button. Click on it and you will see the Form view of the Article doctype. Fill in the form and click on Save. You have created your first Article document. Go back to the list view and you should see one record.


## Doctype Features

Previously we created the Article doctype. Let's see what other features we can customize.

## Naming

If you created a document with the Form, you might have noticed that the `name` value of the document was a randomly generated hash. Let's make a change so that the Article Name we provide becomes the `name` of the document.

To do that, open the doctype list from the search bar and click on Article. Now, scroll down to the Naming section and in the Auto Name field enter **field:article_name**. Click on Save.
![image](https://user-images.githubusercontent.com/103517339/223106571-3a2fdafe-5a3a-4188-8161-2da47d498c60.png)


Now, go back to the Article List and create a new article again(for example- catch22 is name of the article).
![image](https://user-images.githubusercontent.com/103517339/223106941-656397ee-a31c-4b28-9915-4f0c3b65ec30.png)

Now, the name of the document will be the Article Name and it must be unique across Articles. So you cannot create another article with the same name.



## Form Layout

Let's customize the layout of how the fields are laid out in the form while making good use of the available space. Go to the Article doctype, scroll to the Fields section, and add two new fields of type Column Break and Section Break. We will also hide the Image field as it is not needed to be shown in the form. Check out the GIF to see it in action.

![image](https://user-images.githubusercontent.com/103517339/223107441-6b78451c-5968-4c39-8f85-04b37de51535.png)


## Form Settings

Go to the Article doctype and scroll down to the Form Settings section. Enter image in the Image Field. This will show the image at the left top of the form. You can also enable Allow Rename to allow renaming of documents.

![image](https://user-images.githubusercontent.com/103517339/223107613-cfaa50be-d2a0-4a52-93d0-150e60072859.png)

## Permissions

You can also configure what roles you want to allow and which actions you want to restrict for a DocType. Go to the Article doctype, scroll down to the Permission Rules section, and add the roles.

You can also configure the type of action that is allowed for a particular role. Let's add a Librarian role that has permission for all actions and a Library Member role that has permission for Read action.

![image](https://user-images.githubusercontent.com/103517339/223107850-5659be74-7a2e-4826-bab5-109c6596319c.png)

You can test this by creating a new User that has the Librarian role, and another User that has the Library Member role. Login with each user, and see what actions are allowed.


## Controller Methods

Controller methods allow you to write business logic during the lifecycle of a document.

Let's create our second doctype: Library Member. It will have the following fields:

` DocType List ` > ` Add DocType `

- First Name (Data, Mandatory)
- Last Name (Data)
- Full Name (Data, Read Only)
- Email Address (Data)
- Phone (Data)

![image](https://user-images.githubusercontent.com/103517339/223108151-83259b80-2b3b-4dcc-93d4-a32175e5b289.png)

After you have created the doctype, go to Library Member list, clear the cache from Settings > Reload and create a new Library Member.

If you notice, the Full Name field is not shown in the form. This is because we set it as Read Only. It will be shown only when it has some value.

Let's write code in our python controller class such that Full Name is computed automatically from First Name and Last Name.

Open your code editor and open the file library_member.py and make the following changes:

```bash
class LibraryMember(Document):
    # this method will run every time a document is saved
    def before_save(self):
        self.full_name = f'{self.first_name} {self.last_name or ""}'
```
If the above snippet doesn't work for you , make sure server side scripts are enabled, and then restart bench
    bench --site <your_site> set-config server_script_enabled true

## Types of DocType

## Library Membership

Let's learn about the different types of doctype in the framework by creating more doctypes.

Let's create another doctype: **Library Membership**. It will have the following fields:

- Library Member (Link, Mandatory)
- Full Name (Data, Read Only)
- From Date (Date)
- To Date (Date)
- Paid (Check)

It will have Is Submittable enabled. It will have Naming set as LMS.##### and restricted to Librarian role. Also, the Title Field should be set to full_name in the View Settings section.

![image](https://user-images.githubusercontent.com/103517339/223108454-e45167cd-d0f1-487e-a652-f81b3eccc52c.png)

Add fields

![Screenshot from 2023-03-06 18-14-41](https://user-images.githubusercontent.com/103517339/223114039-63a9c13c-26ed-4068-8991-f941b83b35b0.png)

Edit Full Name 

![Screenshot from 2023-03-06 18-11-48](https://user-images.githubusercontent.com/103517339/223113289-170c52d0-9432-416a-a302-51932c66e5ea.png)

Permission Rules

![Screenshot from 2023-03-06 18-16-01](https://user-images.githubusercontent.com/103517339/223114229-3699b4b1-dc72-428a-9d2c-5f7bebeca674.png)


The Link field Library Member is similar to a Foreign Key column in other frameworks. It will let you link the value to a record in another DocType. In this case, it links to a record of Library Member DocType.

The Full Name field is a Read Only field that will be automatically fetched from from the full_name field in the linked record Library Member.

Now, go to the Library Membership list and create a new document. You will see that the Library Member field is a dropdown with existing records shown as options. 

![Screenshot from 2023-03-06 18-19-54](https://user-images.githubusercontent.com/103517339/223115079-fb8586c3-b880-4018-9713-f8e8aebee3c7.png)

Select a Library Member and the Full Name will be fetched automatically. 

![Screenshot from 2023-03-06 18-20-27](https://user-images.githubusercontent.com/103517339/223115326-53066bd8-20b3-4d1a-9c9a-a735d151e0be.png)



## Controller Validation for Membership

Now, let's write code that will make sure whenever a Library Membership is created, there is no active membership for the Member.

**library_membership.py**

```bash
# Copyright (c) 2023, vipin and contributors
# For license information, please see license.txt

import frappe
from frappe.model.document import Document


class LibraryMembership(Document):
	# check before submitting this document
    def before_submit(self):
        exists = frappe.db.exists(
            'Library Membership',
            {
                'library_member': self.library_member,
                # check for submitted documents
                'docstatus': 1,
                # check if the membership's end date is later than this membership's start date
                'to_date': ('>', self.from_date),
            },
        )
        if exists:
            frappe.throw('There is an active membership for this member')
```

We wrote our logic in the before_submit method which will run before we submit the document. We used the frappe.db.exists method to check if a Library Membership record exists with our provided filters. If it exists, we used frappe.throw to stop the execution of program with a message that will show up letting the user know the reason.

Now, try creating a Library Membership with an overlapping period and you should see an error when you submit the document.

![Screenshot from 2023-03-06 18-22-49](https://user-images.githubusercontent.com/103517339/223115716-1882fe63-0fbc-49d3-83c9-a850505c2cf9.png)


## Library Transaction

Let's create a DocType to record an Issue or Return of an Article by a Library Member who has an active membership.

This doctype will be called **Library Transaction** and will have the following fields:

- Article - Link to Article
- Library Member - Link to Library Member
- Type - Select with 2 options: Issue and Return
- Date - Date of Transaction

This doctype will also be a Submittable doctype.

![Screenshot from 2023-03-06 18-23-38](https://user-images.githubusercontent.com/103517339/223116133-7777f205-b482-4f7f-81cc-fe49e395aaa9.png)

![Screenshot from 2023-03-06 18-23-54](https://user-images.githubusercontent.com/103517339/223116146-d6433305-4836-4ec5-9232-7ed8c1e0814c.png)

![Screenshot from 2023-03-06 18-24-09](https://user-images.githubusercontent.com/103517339/223116157-99e0830c-d977-4c97-be00-7214e06ccbb2.png)

![Screenshot from 2023-03-06 18-24-24](https://user-images.githubusercontent.com/103517339/223116172-5157484b-7bb8-41b7-a144-1aeeba78d246.png)


## Validation for Transaction

When an Article is issued, we should verify whether the Library Member has an active membership. We should also check whether the Article is available for Issue. Let's write the code for these validations.

**library_transaction.py**

```bash
# Copyright (c) 2023, vipin and contributors
# For license information, please see license.txt

import frappe
from frappe.model.document import Document

class LibraryTransaction(Document):
    def before_submit(self):
        if self.type == 'Issue':
            self.validate_issue()
            # set the article status to be Issued
            article = frappe.get_doc('Article', self.article)
            article.status = 'Issued'
            article.save()

        elif self.type == 'Return':
            self.validate_return()
            # set the article status to be Available
            article = frappe.get_doc('Article', self.article)
            article.status = 'Available'
            article.save()

    def validate_issue(self):
        self.validate_membership()
        article = frappe.get_doc('Article', self.article)
        # article cannot be issued if it is already issued
        if article.status == 'Issued':
            frappe.throw('Article is already issued by another member')

    def validate_return(self):
        article = frappe.get_doc('Article', self.article)
        # article cannot be returned if it is not issued first
        if article.status == 'Available':
            frappe.throw('Article cannot be returned without being issued first')

    def validate_membership(self):
        # check if a valid membership exist for this library member
        valid_membership = frappe.db.exists(
            'Library Membership',
            {
                'library_member': self.library_member,
                'docstatus': 1,
                'from_date': ('<', self.date),
                'to_date': ('>', self.date),
            },
        )
        if not valid_membership:
            frappe.throw('The member does not have a valid membership')
```

There is a lot of code here but it should be self explanatory. There are inline code comments for more explanation.

## Library Settings

Let's create the last doctype for our app: Library Settings. It will have the following fields:

- Loan Period - Will define the loan period in number of days
- Maximum Number of Issued Articles - Restrict the maximum number of articles that can be issued by a single member

Since we don't need to have multiple records for these settings, we will enable Is Single for this doctype.

![Screenshot from 2023-03-06 18-28-59](https://user-images.githubusercontent.com/103517339/223119265-ac53dc5c-7eca-46bf-a917-04d83fb8007d.png)


![Screenshot from 2023-03-06 18-29-15](https://user-images.githubusercontent.com/103517339/223119276-045d278f-727e-4c9e-b08c-3dbc564b0faa.png)


After creating the doctype, click on Go to Library Settings, to go to the form and set the values for Loan Period and Maximum Number of Issued Articles.


## Validation for Library Settings

Let's make the change in Library Membership such that, the To Date automatically computed based on the Loan Period and the From Date.

**library_membership.py**

```bash
import frappe
from frappe.model.document import Document
from frappe.model.docstatus import DocStatus


class LibraryMembership(Document):
    # check before submitting this document
    def before_submit(self):
        exists = frappe.db.exists(
            "Library Membership",
            {
                "library_member": self.library_member,
                "docstatus": DocStatus.submitted(),
                # check if the membership's end date is later than this membership's start date
                "to_date": (">", self.from_date),
            },
        )
        if exists:
            frappe.throw("There is an active membership for this member")

        # get loan period and compute to_date by adding loan_period to from_date
        loan_period = frappe.db.get_single_value("Library Settings", "loan_period")
        self.to_date = frappe.utils.add_days(self.from_date, loan_period or 30)
```

We have used `frappe.db.get_single_value` method to get the value of `loan_period` from the Library Settings doctype.

Now, let's make the change in Library Transaction such that when an Article is Issued, it checks whether the maximum limit is reached.

**library_transaction.py**

```bash

import frappe
from frappe.model.document import Document
from frappe.model.docstatus import DocStatus


class LibraryTransaction(Document):
    def before_submit(self):
        if self.type == "Issue":
            self.validate_issue()
            self.validate_maximum_limit()
            # set the article status to be Issued
            article = frappe.get_doc("Article", self.article)
            article.status = "Issued"
            article.save()

        elif self.type == "Return":
            self.validate_return()
            # set the article status to be Available
            article = frappe.get_doc("Article", self.article)
            article.status = "Available"
            article.save()

    def validate_issue(self):
        self.validate_membership()
        article = frappe.get_doc("Article", self.article)
        # article cannot be issued if it is already issued
        if article.status == "Issued":
            frappe.throw("Article is already issued by another member")

    def validate_return(self):
        article = frappe.get_doc("Article", self.article)
        # article cannot be returned if it is not issued first
        if article.status == "Available":
            frappe.throw("Article cannot be returned without being issued first")

    def validate_maximum_limit(self):
        max_articles = frappe.db.get_single_value("Library Settings", "max_articles")
        count = frappe.db.count(
            "Library Transaction",
            {"library_member": self.library_member, "type": "Issue", "docstatus": DocStatus.submitted()},
        )
        if count >= max_articles:
            frappe.throw("Maximum limit reached for issuing articles")

    def validate_membership(self):
        # check if a valid membership exist for this library member
        valid_membership = frappe.db.exists(
            "Library Membership",
            {
                "library_member": self.library_member,
                "docstatus": DocStatus.submitted(),
                "from_date": ("<", self.date),
                "to_date": (">", self.date),
            },
        )
        if not valid_membership:
            frappe.throw("The member does not have a valid membership")
```

We added a validate_maximum_limit method and used frappe.db.count to count the number of transactions made by the member.

With that, we have covered the basics of doctype creation and types of doctype. We also wrote business logic for various doctypes.


## Form Scripts

Form Scripts are client-side javascript code that enhances the UX of your Forms.

Let's say you want to create a membership for a member. To do this, you have to go to the Library Membership list, create a new form, select the member and other fields and then save.

Similarly, when you want to create a transaction against a member, you have to make a new Library Transaction form.

We can make this process easier. Write the following code in **library_member.js**

```bash
frappe.ui.form.on('Library Member', {
    refresh: function(frm) {
        frm.add_custom_button('Create Membership', () => {
            frappe.new_doc('Library Membership', {
                library_member: frm.doc.name
            })
        })
        frm.add_custom_button('Create Transaction', () => {
            frappe.new_doc('Library Transaction', {
                library_member: frm.doc.name
            })
        })
    }
});
```

Now, refresh your page and go to the Library Member form. You should see two buttons on the top right. Click on them to try them out. They will automatically set the Library Member in each of those documents making the process easier.

![Screenshot from 2023-03-06 18-43-03](https://user-images.githubusercontent.com/103517339/223119953-32f5dfd4-bdf2-4d6a-9634-6d07358d33dc.png)


We have only scratched the surface here. You can do a lot more with Form Scripts. Learn more about the API at [Form Scripts API](https://frappeframework.com/docs/v14/user/en/api/form).

---

## Web View Pages

Web View Pages are server rendered pages for your website visitors.

We have been exclusively working with the Desk which is the admin interface accessible by System Users. Usually you will want to give limited access to your customers. In our case, we want Library Members to be able to view available Articles that they can issue from our website. Web View Pages can help us achieve that.

Go to Article doctype, and scroll down to the Web View section.

 1.Enable Has Web View and Allow Guest to View
 2.Enter articles in the Route field
 3.Add fields named Route and Published in the fields table
 4.Click on Save
 
The published field will help filter out those documents which are not supposed to be shown in web view, otherwise you will run into an error.

![image](https://user-images.githubusercontent.com/103517339/223120269-a477fe3e-5b24-4855-865b-46d2a76442ab.png)

![image](https://user-images.githubusercontent.com/103517339/223120309-491999ff-30ec-4329-af5a-bc30fd07d6ad.png)


We have now enabled web views for Article doctype. This means you can now view details of an Article on your website without logging into Desk. Let's test this out by creating a new Article. You should see See on Website at the top left of your form. Click on it to view the web view of the Article.


## Customize Web View Template

The default web view that is generated is pretty barebones and serves only as a starting point for us to customize it. When we made Article a web view, two HTML files were created namely: `article.html` and article_row.html

Let's edit `article.html` first. Frappe uses Bootstrap 4 by default for it's web views. So, you can use any valid Bootstrap 4 markup to style your pages. Add the following HTML to `article.html`.


```bash
{%  extends 'templates/web.html' %}

{% block page_content %}
<div class='py-20 row'>
    <div class='col-sm-2'>
        <img alt='{{ title }}' src='{{ image }}'>
    </div>
    <div class='col'>
        <h1>{{ title }}</h1>
        <p class='lead'>By {{ author }}</p>
        <div>
            {%- if status == 'Available' -%}
            <span class='badge badge-success'>Available</span>
            {%- elif status == 'Issued' -%}
            <span class='badge badge-primary'>Issued</span>
            {%- endif -%}
        </div>
        <div class='mt-4'>
            <div>Publisher: <strong>{{ publisher }}</strong></div>
            <div>ISBN: <strong>{{ isbn }}</strong></div>
        </div>
        <p>{{ description }}</p>
    </div>
</div>
{% endblock %}
```

Now, go to any Article and click on See on Website. If you have filled in all fields of your Article, you should see a page like this:

![Screenshot from 2023-03-06 18-46-11](https://user-images.githubusercontent.com/103517339/223120597-e1a043de-e1c0-4cf4-bb8b-7b11addf7eaf.png)


Now, open http://library.test:8000/articles. This should show the list of articles, but it is also pretty barebones. Let's customize the HTML.

Edit the article_row.html and add the following HTML:


```bash
<div class='py-8 row'>
    <div class='col-sm-1'>
        <img alt='{{ doc.name }}' src='{{ doc.image }}'>
    </div>
    <div class='col'>
        <a class='font-size-lg' href='{{ doc.route }}'>{{ doc.name }}</a>
        <p class='text-muted'>By {{ doc.author }}</p>
    </div>
</div>
```

Now, the articles list should look prettier. You can click on any article to view it's details.

![Screenshot from 2023-03-07 18-02-36](https://user-images.githubusercontent.com/103517339/223423191-2e8f7958-eb1d-42f6-9c18-82646d31e8a4.png)
