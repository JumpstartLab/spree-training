## Creating a Theme System

The first spree extension that we created added very little to the current
experience. We relied on the existing infrastructure with very few changes.
Now we want to create an extension that will require us to add additional
database migrations, models, controllers, and views.

The goal of this exercise is to allow admins to provide some creative control
over their ecommerce platform through the admin panel. An admin would be able
to login, view the admin section, select the theme menu, and be able to choose
and set various colors.

### Adding a New Menu Item

Lets start by adding the "Theme" menu item to the already existing list of
menu items.

The menu is rendered on every admin page. It is part of the standard layout.
Let's start by replacing the entire layout and then replacing the menu item
tactically with Deface.

### Adding the Theme Page

Now that we have the menu item lets add the destination page for our themes.

We will want to create a new controller to handle all the theme management.

### Which Elements Should an Admin Control

We want to allow an admin to configure and control most of their website. With
that in mind, we could simply allow an admin to copy-and-paste an entirely new
set of css overrides into an text area and call it done.

However, we might also want to only them to configure a small subset of the
features (e.g. background colors, font colors, font sizes) through a variety of
smaller textfields or controls to ensure that the stores have a sane look and
feel and possibly have a similar branding across multiple sites.

For this exercise lets start with providing the admin with the ability to
configure a small set of features. We could always later implement a system
that allows the admin to provide complete overrides with what we learn with the
more restrictive system.

### How to Control the Themes

Starting with the background color we want to allow the admin to specify a color
value within a text field. When the admin is done specifying the color they
will press the submit button and the details will be saved in a controller
action.

### Storing the Details in the Database

Our controller action receives the setting changes and it is our job to save
them. At the moment our most readily available data store is the database
that contains all the other information about our platform. So we will want
to save our data to our database.

How should this data be modeled? In a lot of ways this data is not really
suited for a relational database. As we want to add more fields to the
configuration we would need to add more columns to our table. As our layout
changes we may find ourself with orphaned columns or constantly need to remove
columns.

Ultimately it may be best for us to store all our style preferences within the
database in a single column simply as YAML data which we will extract and
convert to a ruby object.

#### Creating and Running the Databae Migration

So our migration is simple. We want to create a new table which has a single
column to store the styles.

### Allowing an Admin to Configure the Details

Our ActiveRecord model is extremely simple. When we receive settings from the
admin we could simply store the entire response into our database field.

### Interating with our Theme Details in a Sane Way

Our overly simple ActiveRecord model does not immediately provide a great
interface for interacting with the theme data that the admin provided. If we
were lazy we could start passing around the hash of data that we simply stored
in the database.

It would be a better choice to provide an interface on our object for
interacting. If we wanted to provide defaults, override functionality, and
provide a better experience for us as developers if we decide to change how
the data is stored.

### Overriding our Base Theme with the Details

With the details of our theme complete lets apply our theme to

We could override the layout and apply our settings within a style tag. An
alternative would be to render a stylesheet and allow for our settings to
be applied in-line to the stylesheet.

Stylesheets within the asset pipeline can have the **erb** extension applied
similar to how we have previously defined templates. Rails will process these
stylesheets first as **erb**, processing any of our escape tags which we will
use to apply our settings.

### Extracting Into An Extension

Now that we are done with our theming it is time to extract all the contents
of the theme out of our current Spree project and move it into an extension.