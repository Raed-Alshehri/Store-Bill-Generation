
# Store Bill Generation

 This project is a program written in Python and it is required to do the following:

- Generate a bill for some store: The store owner provides two text files as an input. The program first reads the input file named
“items.txt” which contains a list of all items in the store with their unit-prices. 

- The program stores the items in in one list called itemList: according to their order given in the items.txt
file. The index of each item in this list is used as a serial number (S/N) of the item. 

- The program reads the other input file named “promo.txt” which contains the information about
special discounts for promo items. 

- When a customer places an order, a text file is created
where each lines has the item S/N and the quantity. 

- The program then generates a bill for the customer using the information stored in
the itemList.





 
## Functions and Description

```
getItemList():
This function reads the items and promo files, and then store them into a list and returns the list
```

```
createBill(fileName):
This function takes the file name (entered by the user), write on it, 
then creates the bill using "createBill" function, and prints the table
Create 'bill name.txt'

```

```
calculateOrder(fileName)
This function calculates the order from the file entered by the user, and sort it by serail number, 
then calculates the quantity and unit price as well as the total amount of each item and returns it in a dictionary
```

```
finalBill(fileName)
This function takes the order prepared in "calculateOrder" function, and applys BOGO, Standard discounts,
then returns the order after applying all discounts 
```

```
main()
the main function prompts the user for the file name,
then creates the bill fomrated in a tabular shape after applying all discounts
```


## 🛠 Skills Used
Python: expressions, decision structures, looping, functions, lists, files and exceptions.
## 🚀 About Me
👋 Hi, I’m @Raed-Alshehri

👀 I’m interested in data science, machine learning, and statistics.

🌱 I’m applying my skills in the data analytics field using Python, R, and SQL


## 🔗 Links
[![portfolio](https://img.shields.io/badge/my_portfolio-000?style=for-the-badge&logo=ko-fi&logoColor=white)](https://raed-alshehri.github.io/RaedAlshehri.github.io/)
[![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/raedalshehri/)


## Feedback

If you have any feedback, please reach out to me at alshehri.raeda@gmail.com

