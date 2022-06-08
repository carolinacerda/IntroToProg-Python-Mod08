**Dev:** *CCerda*  
**Date:** *06.07.2022*

# Module08 Website
---
This webpage is dedicated to Module 08 - Classes and Objects for the course Foundations of Programming: Python held during Spring 2022.

## Creating a Script Using Object-Oriented Programming Techniques

### Introduction

#### Creating the Script

##### Classes, Objects, and Object-Oriented Programming (OOP)

###### Creating a Constructor and Attributes

###### Creating Properties

###### Creating Methods

##### Processing Code for Saving and Reading Data from a File Using OOP

###### Saving Data to the File

###### Reading Data from the File

##### Presentation Code for Input and Output

###### Outputting Menu Items

###### Inputting Menu Choice

###### Outputting Current List Items

###### Inputting New Product Data

#### Code for the Main Body of the Script

### Running the Script

By building upon the starter script for the assignment for Module 06, as well as the various listing examples provided by Professor Root and from the textbook, Python® Programming for the Absolute Beginner, Third Edition, by Michael Dawson, the following completed script is displayed below.

```
# ------------------------------------------------------------------------ #
# Title: Assignment 08
# Description: Working with classes
#              to create a program that stores and
# ChangeLog (Who,When,What):
# RRoot,1.1.2030,Created started script
# RRoot,1.1.2030,Added pseudocode to start assignment 8
# CCerda,06.07.2022,Modified code to complete assignment 8
# ------------------------------------------------------------------------ #

# Data -------------------------------------------------------------------- #
strFileName = 'products.txt'
lstOfProductObjects = []


class Product:
    """Stores data about a product:
    properties:
        product_name: (string) with the product's name
        product_price: (float) with the product's standard price
    methods:
        to_string() returns comma separated product data (alias for __str__())
    changelog: (When,Who,What)
        RRoot,1.1.2030,Created Class
        CCerda,06.06.2022,Modified code to complete assignment 8
    """

    # -- Constructor --
    def __init__(self, product_name: str, product_price: float):
        """ Set name and price of a new object """
        # -- Attributes --
        try:
            self.__product_name = str(product_name)
            self.__product_price = float(product_price)
        except Exception as e:
            raise Exception("Error setting initial values: \n" + str(e))

    # -- Properties --
    # product_name
    @property
    def product_name(self):   # (getter or accessor)
        return str(self.__product_name)

    @product_name.setter
    def product_name(self, value: str):   # (setter or mutator)
        if not str(value).isnumeric():
            self.__product_name = value
        else:
            raise Exception("Product names cannot be numbers.")

    # product_price
    @property
    def product_price(self):  # (getter or accessor)
        return float(self.__product_price)  # cast to float

    @product_price.setter
    def product_price(self, value: float):  # (setter or mutator)
        if isinstance(value, float):
            self.__product_price = float(value)  # cast to float
        else:
            raise Exception("Product prices must be in numbers.")

    # -- Methods --
    def to_string(self):
        """ alias of __str__(), converts product data to string """
        return self.__str__()

    def __str__(self):
        """ Converts product data to string """
        return self.product_name + "," + str(self.product_price)

# Data -------------------------------------------------------------------- #


# Processing  ------------------------------------------------------------- #
class FileProcessor:
    """Processes data to and from a file and a list of product objects:
    methods:
        save_data_to_file(file_name,list_of_product_objects):
        read_data_from_file(file_name): -> (a list of product objects)
    changelog: (When,Who,What)
        RRoot,1.1.2030,Created Class
        CCerda,06.07.2022,Modified code to complete assignment 8
    """

    @staticmethod
    def save_data_to_file(file_name: str, list_of_product_objects: list):
        """ Write data to a file from a list of product rows
        :param file_name: (string) with name of file
        :param list_of_product_objects: (list) of product objects data saved to file
        :return: (bool) with status of success status
        """
        success_status = False
        try:
            file = open(file_name, "w")
            for product in list_of_product_objects:
                file.write(product.__str__() + "\n")
            file.close()
            success_status = True
        except Exception as e:
            print("There was an error!")
            print(e, e.__doc__, type(e), sep='\n')
        return success_status

    @staticmethod
    def read_data_from_file(file_name: str):
        """ Reads data from a file into a list of product rows
        :param file_name: (string) with name of file
        :return: (list) of product rows
        """
        list_of_product_rows = []
        try:
            file = open(file_name, "r")
            for line in file:
                name, value = line.split(",")
                row = Product(name, value.strip())
                list_of_product_rows.append(row)
            file.close()
        except Exception as e:
            print("There was an error!")
            print(e, e.__doc__, type(e), sep='\n')
        return list_of_product_rows

# Processing  ------------------------------------------------------------- #


# Presentation (Input/Output)  -------------------------------------------- #
class IO:
    """  A class for performing Input and Output
    methods:
        print_menu_items():
        print_current_list_items(list_of_rows):
        input_product_data():
    changelog: (When,Who,What)
        RRoot,1.1.2030,Created Class:
        CCerda,06.07.2022,Modified code to complete assignment 8
    """

    @staticmethod
    def print_menu_items():
        """ Print a menu of choices to the user  """
        print(''' 
        Menu of Options 
        1) Show Current Items 
        2) Add a New Item 
        3) Save Data to File 
        4) Exit Program''')
        print()  # Add an extra line for looks

    @staticmethod
    def input_menu_choice():
        """ Gets the menu choice from a user
        :return: string
        """
        print()  # Add an extra line for looks
        choice = str(input("Which option would you like to perform? [1 to 4]: ")).strip()
        print()  # Add an extra line for looks
        return choice

    @staticmethod
    def print_current_list_items(list_of_rows: list):
        """ Print the current items in the list of rows
        :param list_of_rows: (list) of rows you want to display
        """
        if int(len(list_of_rows)) > 0:
            print("---- YOUR CURRENT ITEMS: ----")
            print("PRODUCT  |  PRICE")
            for row in list_of_rows:
                print(row.product_name + "  |  $%.2f" % row.product_price)
            print("-----------------------------")
            print()  # Add an extra line for looks
        else:
            print("  There are no current items in the list. Please choose 'Option 2' to add new products.\n")

    @staticmethod
    def input_product_data():
        """ Gets data for a product object
        :return: (Product) object with input data
        """
        try:
            print("Type in your product and its price.")
            print()  # Add an extra line for looks
            name = str(input("  Enter the product name: ").title().strip())
            price = float(input("  Enter the product price: ").strip())
            print()  # Add an extra line for looks
            p = Product(product_name=name, product_price=price)
        except Exception as e:
            print(e)
        return p

    # Presentation (Input/Output)  -------------------------------------------- #


# Main Body of Script  ---------------------------------------------------- #

# Load data from file into a list of product objects when script starts
try:
    lstOfProductObjects = FileProcessor.read_data_from_file(strFileName)

    # Show user a menu of options
    IO.print_menu_items()
    while True:
        strChoice = IO.input_menu_choice()  # Get user's  menu option choice
        if strChoice.strip() == '1':
            # Show user current data in the list of product objects
            IO.print_current_list_items(lstOfProductObjects)
            continue  # to show the menu
        elif strChoice.strip() == '2':
            # Let user add data to the list of product objects
            lstOfProductObjects.append(IO.input_product_data())
            continue  # to show the menu
        elif strChoice.strip() == '3':
            # let user save current data to file and exit program
            FileProcessor.save_data_to_file(strFileName, lstOfProductObjects)
            print("  Data saved to file!\n")
            continue  # to show the menu
        elif strChoice.strip() == '4':
            print("  Goodbye!")
            break  # by exiting loop
        else:
            print("  Input is not a number from 1 to 4! Please try again.\n")
            continue
except Exception as e:
    print("There was an error! Check file permissions.")
    print(e, e.__doc__, type(e), sep='\n')
# Main Body of Script  ---------------------------------------------------- #
```

The final result of the script in both PyCharm and the OS Command are shown below in Figures 1 and 2, respectively.



![Figure 1. Final result of script in PyCharm](https://raw.githubusercontent.com/carolinacerda/IntroToProg-Python-Mod08/main/docs/Figure%201.png "Final Result of Script in PyCharm")

*Figure 1. Final Result of Script in PyCharm.*



![Figure 2. Final result of script in OS Command](https://raw.githubusercontent.com/carolinacerda/IntroToProg-Python-Mod08/main/docs/Figure%202.png "Final Result of Script in OS Command")

*Figure 2. Final Result of Script in OS Command.*



Finally, a verification of the success of the script is done by confirming that the data inputted by the user has successfully been saved within the text file, “products.txt” (Figure 3).



![Figure 3. File used in script to save user input data](https://raw.githubusercontent.com/carolinacerda/IntroToProg-Python-Mod08/main/docs/Figure%203.png "File Used in Script to Save User Input Data")

*Figure 3. File Used in Script to Save User Input Data*



### Summary
For this exercise, classes and their components were used to create a script that builds upon code seen previously throughout this course to manage a program that displays to the user a menu of options, outputs the current items in a list from a text file, allows the user to input new data, provides the option to save the data to a file, and then exit the program. Finally, GitHub Desktop was used to introduce a way to use GitHub directly from the computer as an application instead of from a browser or command line.
