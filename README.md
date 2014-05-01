#CMYKsoup#

The Python client for interacting with the CMYKsoup API hosted at api.cmyksoup.com

## Installation ##
Download the zip file. Inside the folder named cmyksoup, there is a file called default_config.py. Change its name to config.py and set the values of `API_KEY` and `API_SECRET_ACCESS_KEY` with the keys you obtained from us. 
Now run the setup.py with `python setup.py install`. 

---------------------------------------------------------------------

## Usage ##
In your python code or the interpreter, just add this import line
`from cmyksoup import *`

---------------------------------------------------------------------

## Product ##

### Attributes ###

`id` - The id of the product used in our DB

`name` - A unique name for the product ( unique for a given user )

`design_file` - The location of the design file

`status` - The status of the design file 

`quantity` - The quantity of items of the product available in stock

`third_party` - set to True if the product is from a 3rd party manufacturer

----------------------------------------------------------------------


#### Creating a product ####

Use this static method for creating a product

`Product.create(name=None,  third_party=False, template_id=None )`


##### Parameters #####

`name` -  A name for the product. If provided, it has to be unique among all your products. That is you cannot give the same name to any two of your products.

`third_party` - Set this flag to true if the product is manufactured by a third party and CMYKsoup is just used for shipping

`template_id` - Set the template_id of the template to be used (if known)

Note that creating a product does not mean it is ready for shipping. You still need to stock the items of the product by giving us a stock order on our website or offline.


##### Response #####
A `Product` object is returned with all the attributes set.

#####Example usage:#####

`product = Product.create("My Sticker", third_party=True)`

---------------------------------------------------------------------------------

##Customer##

###Attributes###

`id` - The id of the customer used in our DB
'
And the following self explanatory attributes:

`'name', 'phone_number', 'address1', 'address2','city','state','country','pincode' `

------------------------------------------------------------------------------------

##Package ##

### Attributes ###

`id` - The id of the package used in our DB

`contents` - A list of tuples containing the names and quantities of the products in the package 

`delivery_date` - Promised delivery date

`status` - status of the package

`gross_amount` - Total cost for sending the package including shipping and tax 

`tracking_url` -  The package tracking url

`customer_form_url` - If customer has not been added for the package yet, this is set to an one time form url which can be given to your users 

`customer` - If customer is set, this points to a `Customer` object with various attributes set

--------------------------------------------------------------------------------------------------

### Creating a Package ###
For the most common use cases, we have 4 convenience factory methods for creating products.

------------------------------------------------------------------------------------------------------

#### To create a single package of a single product ####

`Package.product( product_name,quantity).create(customer_id, next)`

#####Parameters#####

`customer_id` - Provide the customer id if known. If not provided, the package object returned has the attribute `customer_form_url` set

`next` - Provide the url to redirect the user to, after filling the customer form. If given, the `customer_form_url` attribute will have this value appended at the end as a GET parameter 



#####Returns#####
A package object with various attributes set

#####Example##### 

`package = Package.product( "External Tshirt L",1).create( next= "hackerearth.com" )`

------------------------------------------------------------------------------------------------------

#### To create multiple packages of a single product ####

`Package.product( product_name,quantity).bulk_create(no_of_packages, customer_ids, next)`

#####Parameters#####

`no_of_packages` - The number of packages to create. Provide this if the list of customer ids is not known

`customer_id` - If the list of customers to send is known in advance, provide the list of ids instead of setting the previous parameter

`next` - Provide the url to redirect the user to, after filling the customer form. If given, the `customer_form_url` attribute will have this value appended at the end as a GET parameter


#####Returns#####

A tuple of packages and failures. First is an array of package objects which were created successfully. Second is a list of dictionaries where each has an error message with the key `message` saying why package creation failed for that package

#####Example##### 

`pkgs, failures = Package.product( "External Tshirt L",1).bulk_create(2)`

-------------------------------------------------------------------------------------------------------


#### To create a single package of a set of products ####

`Package.products( *args ).create(customer_id, next)`

#####Parameters#####

`*args` - Each arg is a tuple containing the product name and quantity. Any number of tuples can be given

`customer_id` - Provide the customer id if known. If not provided, the package object returned has the attribute `customer_form_url` set

`next` - Provide the url to redirect the user to, after filling the customer form. If given, the `customer_form_url` attribute will have this value appended at the end as a GET parameter 


#####Returns#####

A package object with various attributes set

#####Example#####

`pkg = Package.products( ("Globe Tshirt L",1), ("New Tshirt M",1) ).create( next= "hackerearth.com" )`

-----------------------------------------------------------------------------------------------------

#### To create multiple packages of a set of products ####

`Package.products( * args ).bulk_create(no_of_packages, customer_ids, next)`


#####Parameters#####

`*args` - Each arg is a tuple containing the product name and quantity. Any number of tuples can be given

`no_of_packages` - The number of packages to create. Provide this if the list of customer ids is not known

`customer_id` - If the list of customers to send is known in advance, provide the list of ids instead of setting the previous parameter

`next` - Provide the url to redirect the user to, after filling the customer form. If given, the `customer_form_url` attribute will have this value appended at the end as a GET parameter


#####Returns#####

A tuple of packages and failures. First is an array of package objects which were created successfully. Second is a list of dictionaries where each has an error message with the key `message` saying why package creation failed for that package

#####Example#####
`pkgs, failures = Package.products( ("Globe Tshirt L",1), ("New Tshirt M",1) ).bulk_create(2)`

------------------------------------------------------------------------------------------------------------






