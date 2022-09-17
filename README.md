
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


## Code
```python
# This function reads the items and promo files, and then store them into a list and returns the list
## Generating the standard items list
def getItemList():
    # Getting each item data and storing it in a list of dicts
    itemFile = open('items.txt', 'r')
    itemLines = itemFile.readlines()  
    itemList = [] 
    for item in itemLines: 
        itemList.append({
            'itemType': 'S',
            'itemName': item.split()[0],
            'itemPrice': float(item.split()[1])
        })
    itemFile.close() 
    # Updating item list according to promos
    promoFile = open('promo.txt', 'r') 
    promoLines = promoFile.readlines() 
    for promo in promoLines:
        itemList[(int(promo.split()[0])) - 1]['itemType'] = 'P'
    promoFile.close()
    return itemList


# This function takes the file name (entered by the user), write on it, 
# then creates the bill using "createBill" function, and prints the table
## Create 'bill name.txt'
def createBill(fileName):
    # @param is the file name, which is the file that the user enters
    # Getting the final bill data for the order after applying all discounts
    bill = finalBill(fileName)
    # Creating the bill text file
    outputFile = open(f'bill_ {fileName}', "w+")
    # Writing lines formatted with minimum field width to have a table shape
    outputFile.write('=' * 42 + '\n')
    outputFile.write(
        '%(serial)-5s%(item)-15s%(qty)3s%(price)9s%(amount)10s\n' % {
            'serial': "SN",
            'item': "ITEM",
            'qty': "QTY",
            'price': "U.PRICE",
            'amount': "AMOUNT"
        })
    outputFile.write('-' * 42 + '\n')
    # Creating a row for each item in the order
    for item in bill['orderItems']:
        outputFile.write(
            '%(serial)-5s%(item)-15s%(qty)3d%(price)9.2f%(amount)10.2f\n' %
            {
                'serial': item["S/N"],
                'item': item["itemName"],
                'qty': item["itemQty"],
                'price': item["itemPrice"],
                'amount': item["itemAmount"]
            })
    # Creating the bill footer
    outputFile.write('-' * 42 + '\n')
    outputFile.write(' ' * 20 + '%(prefix)-12s%(amount)8.2f\n' % {
        'prefix': "Sales Amount..",
        'amount': bill["saleTotal"]
    })
    outputFile.write(' ' * 20 + '%(prefix)-12s%(amount)8.2f\n' % {
        'prefix': "Discount......",
        'amount': bill["total_disc"]
    })
    outputFile.write(' ' * 20 + '%(prefix)-12s%(amount)8.2f\n' % {
        'prefix': "Total Amount..",
        'amount': bill["total_amount"]
    })
    outputFile.write(' ' * 20 + '%(prefix)-12s%(amount)8.2f\n' % {
        'prefix': "VAT 15%.......",
        'amount': bill["vat"]
    })
    outputFile.write(' ' * 20 + '%(prefix)-12s%(amount)8.2f\n' % {
        'prefix': "Grand Total...",
        'amount': bill["grand_total"]
    })
    outputFile.write('=' * 42 + '\n')
    outputFile.close()
    print(f'\nbill {fileName} was created!\n')


# This function calculates the order from the file entered by the user, and sort it by serail number, 
# then calculates the quantity and unit price as well as the total amount of each item and returns it in a dictionary
## Calculating Order
def calculateOrder(fileName):
    # @param is the file name, which is the file that the user enters
    total = 0.0
    orderFile = open(f'{fileName}', 'r')
    orderLines = orderFile.readlines()
    # Acquiring the standard items list
    itemList = getItemList()
    # Creating the lists needed to process the order
    orderItems = []
    uniqueItems = []
    for order in orderLines:
        # Getting the values for each item in the order
        itemSerial = int(order.split()[0])
        itemName = itemList[(int(order.split()[0])) - 1]['itemName']
        itemPrice = itemList[(int(order.split()[0])) - 1]['itemPrice']
        itemType = itemList[(int(order.split()[0])) - 1]['itemType']
        itemQty = float(order.split()[1])
        # Calculating the sales total
        total += (itemPrice * itemQty)
        # Ensuring the uniqueness of the item even if it was repeated
        if itemSerial in uniqueItems:
            # If the item serial already exist in uniqueItems list, we just increase its quantity
            for item in orderItems:
                if item['S/N'] == itemSerial:
                    item['itemQty'] += itemQty
        else:
            # If the item serial is not in uniqueItems add its serial, and add its data to orderItems list
            uniqueItems.append(itemSerial)
            orderItems.append({
                'S/N': itemSerial,
                'itemName': itemName,
                'itemQty': itemQty,
                'itemPrice': itemPrice,
                'itemType': itemType
            })
    # Creating a sorted list of orderItems list sorted by S/N (serial number)
    sortedOrderList = []
    uniqueItems.sort()
    for item in uniqueItems:
        for dic in orderItems:
            if item == dic['S/N']:
                sortedOrderList.append(dic)
    # Adding the amount value to each item in the sortedOrderList
    for dic in sortedOrderList:
        dic["itemAmount"] = dic["itemQty"] * dic["itemPrice"]
    # Creating a dictionary of the data that will be returned
    orderDic = {'orderItems': sortedOrderList, 'saleTotal': total}
    return orderDic

# This function takes the order prepared in "calculateOrder" function, and applys BOGO, Standard discounts,
# then returns the order after applying all discounts 
## Adding Discount Calculations
def finalBill(fileName):
    # @param is the file name, which is the file that the user enters
    # Getting the order data before discount
    orderDic = calculateOrder(fileName)
    # Declaring the needed variables
    standardItemsPrices = []
    highestStandardItem = 0.0
    saleTotal = orderDic['saleTotal']
    totalExcludePromo = saleTotal
    # Calculating the BOGO (Buy One Get One free) discount
    bogoDisc = 0.0
    for item in orderDic['orderItems']:
        if item['itemType'] == 'P':
            # Excluding promo discount from the total to use it in the ">1000 discount"
            totalExcludePromo -= item['itemAmount']
            # Calculating the bogo discount
            if int(item['itemQty']) % 2 == 0:
                # Incase the quantity of the item is even
                bogoDisc += (item['itemAmount'] / 2)
            else:
                # Incase the quantity of the item is odd
                bogoDisc += ((item['itemAmount'] - item["itemPrice"]) / 2)
        # Calculating More Than 1000 discount
        if item['itemType'] == 'S' and totalExcludePromo > 1000:
            # If the item is not promo and >1000 discount is valid add it to standardItemsPrices list
            standardItemsPrices.append(item['itemPrice'])
        if standardItemsPrices:
            # If the standardItemsPrices is not empty get the price of the item with highest price
            highestStandardItem = max(standardItemsPrices)
    # Creating a list of the data that will be returned
    orderDic['total_disc'] = bogoDisc + highestStandardItem
    orderDic['total_amount'] = saleTotal - orderDic['total_disc']
    orderDic['vat'] = round((orderDic['total_amount'] * 0.15), 2)
    orderDic['grand_total'] = orderDic['total_amount'] + orderDic['vat']
    return orderDic

# the main function prompts the user for the file name,
# then creates the bill fomrated in a table shape after applying all discounts
def main():
    # Getting the user input (the file name)
    fileName = input("Please enter the order file name (or Q to quit): ")
    if fileName.upper() != 'Q':
        try:
            createBill(fileName)
        except: # to handle a wrong file name entry
            print("\nSomething went wrong!\
                \nFirst: Please check that 'items.txt' and 'promo.txt' exist and are well formatted.\
                \nThen: Enter a valid text file name like 'person.txt'.\n")
        finally:
            # Restart the main function
            main()


# Running the program
main()
```

    Please enter the order file name (or Q to quit): sultan.txt
    
    bill sultan.txt was created!
    
    Please enter the order file name (or Q to quit): 12
    
    Something went wrong!                
    First: Please check that 'items.txt' and 'promo.txt' exist and are well formatted.                
    Then: Enter a valid text file name like 'person.txt'.
    
    Please enter the order file name (or Q to quit): Q
    


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

