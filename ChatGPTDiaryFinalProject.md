# ChatGPT Coding Diary

## Project Name: Object Oriented Adventures - Final Project

### Date: 1/14/2025

## First Major Prompt - Merchant Shop

## 1. **Task/Problem Description**

One of the first main prompts given to ChatGPT was: How would we make a proper shop for the merchant?

## 2. **Initial Approach/Code**

There was already a set up merchant class, with a buying and selling system, as well as checking whether the user could buy or sell the product depending on the amount of coins. Although armor was removed post-development, this is what was given to chatgpt to work with in order to make a proper shop.

```python
class Merchant:
    def __init__(self, weapons, potions, armor, products):
        self.weapons = weapons
        self.potions = potions
        self.armor = armor
        self.products = products
    
    def sell(self, item, user_balance):
        if user_balance >= item.cost:
            user_balance -= item.cost
            self.products.remove(item)  
            print(f"You have purchased {item.name} for {item.cost} coins.")
            print(f"Your remaining balance is {user_balance} coins.")
            return user_balance
        else:
            print("You do not have enough coins to purchase this item.")
            return user_balance

```

Firstly, I knew that the shop should be somewhat accessible through a main menu, and that it should be its own function. So, I made a draft of the main menu using conditional statements with 6 options which was later given to the final game to decide how to access the shop between story parts. I thought that the shop for the merchant should simply be a list that the user can choose from, but ChatGPT ended up formatting it much better in a more understandable manner.

Here was the original proposed location for the merchant's shop, which was later put into the game. ChatGPT did suggest using newline, or /n, so those changes were implemented here too. 

```python
def main_menu(user, merchant):
    while True:
        print("\nWhat would you like to do?")
        print("1. View inventory (Press 'I')")
        print("2. Go to the shop (Press 'S')")
        print("3. Check your coins (Press 'C')")
        print("4. Exit the game (Press 'E')")
        print("5. Go back to the story (Press 'G')")
        print("6. Check your HP (Press 'H')")

        choice = input("Please enter your choice: ").capitalize()

        if choice == "I":
            print(f"\nYour Inventory: {user.inventory}")
        elif choice == "S":  
            #Here is where the function to start the merchant's shop would have gone.
        elif choice == "C":
            print(f"\nYour Coins: {user.money}")
        elif choice == "E": 
            loading()
            print("Goodbye!")
            break
        elif choice == "G": 
            print("continuing...")
            break
        elif choice == "H":
            print(f"Your HP: {user.HP}")
        else:
            print("Invalid choice. Please try again.")
```

## 3. **Interaction with ChatGPT**

### Questions/Requests to ChatGPT

Write down the questions or requests you made to ChatGPT.

ChatGPT was asked to create a shop for the merchant. The original response was to modify a few classes, which was done, yet the actual approach was odd to me, so I asked for further simplification. The original code given by ChatGPT divided products into categories, which I think made it too complicated since the only products were weapons and potions. So, there were further questions asked after the original approach.

Here's the original approach ChatGPT provided.

```python
class Merchant:
    def __init__(self, weapons, potions, armor, products):
        self.weapons = weapons
        self.potions = potions
        self.armor = armor
        self.products = products
    
    def sell(self, item, user_balance):
        """Sell an item to the user if they can afford it."""
        if user_balance >= item.cost:
            user_balance -= item.cost
            print(f"You have purchased {item.name} for {item.cost} coins.")
            print(f"Your remaining balance is {user_balance} coins.")
            return user_balance
        else:
            print("You do not have enough coins to purchase this item.")
            return user_balance

    def show_shop(self):
        """Display the available items in the merchant's shop."""
        print("\nWelcome to the Merchant's Shop! Here are the available items:\n")
        
        print("Weapons:")
        for idx, weapon in enumerate(self.weapons):
            print(f"{idx + 1}. {weapon}")

        print("\nPotions:")
        for idx, potion in enumerate(self.potions):
            print(f"{idx + 1}. {potion}")

        print("\nArmor:")
        for idx, armor_item in enumerate(self.armor):
            print(f"{idx + 1}. {armor_item}")

    def buy_item(self, user, item_idx, category):
        """Allows the user to buy an item based on category and index."""
        if category == 'weapons':
            item = self.weapons[item_idx]
        elif category == 'potions':
            item = self.potions[item_idx]
        elif category == 'armor':
            item = self.armor[item_idx]

        # Sell the item to the user and update their balance
        user.money = self.sell(item, user.money)
        user.inventory.append(item)
        print(f"Your inventory: {user.inventory}")
        
class User:
    def __init__(self, name, HP, money, attack):
        self.name = name
        self.HP = HP
        self.attack = attack
        self.money = money
        self.inventory = []

    def __str__(self):
        return f"Name: {self.name}, HP: {self.HP}, Money: {self.money}, Attack: {self.attack}, Inventory: {self.inventory}"

    def buy(self, item):
        if self.money >= item.cost:
            self.inventory.append(item)
            self.money -= item.cost
            print(f"Bought {item.name}. Inventory: {self.inventory}")
        else:
            print("Not enough money to buy this item.")

    def heal(self, amount):
        self.HP += amount
        print(f"{self.name} healed! Current HP: {self.HP}")


def start_shop(user, merchant):
    """A simple shop interaction where the user can buy items from the merchant."""
    while True:
        # Show available items
        merchant.show_shop()

        # Get user input for selection
        try:
            category = input("\nEnter the category to buy from ('weapons', 'potions', 'armor') or 'exit' to leave: ").lower()
            if category == 'exit':
                print("Exiting shop...")
                break

            if category not in ['weapons', 'potions', 'armor']:
                print("Invalid category. Please try again.")
                continue

            # Ask for which item they want to buy
            item_idx = int(input(f"Enter the number of the {category} to buy: ")) - 1
            if category == 'weapons' and item_idx < len(merchant.weapons):
                merchant.buy_item(user, item_idx, 'weapons')
            elif category == 'potions' and item_idx < len(merchant.potions):
                merchant.buy_item(user, item_idx, 'potions')
            elif category == 'armor' and item_idx < len(merchant.armor):
                merchant.buy_item(user, item_idx, 'armor')
            else:
                print("Invalid item selection. Please try again.")
        except ValueError:
            print("Please enter a valid number.")
```

I tested this code in another file, and did like the way the shop was displayed. However, as stated before, armor was eventually omitted from the game, and categories were unnecessary. Furthermore, I didn't know what idx, enumerate, or ValueError were, so I asked questions about those.

Additional Questions:

- "Can you get rid of the categories and just make it a simple shop?"
- "What does idx do?"
- Furthermore, there were issues displaying inventory, as in the new code suggested, so I asked for clarification and debugging help.

## 4. **ChatGPT's Suggestions/Code Changes**

ChatGPT suggested some ideas such as adding all the items into a list called items, which the user can type a number to choose from. Idx refers to the position of an object in the list, and subtracts 1 from the user's choice due to the correspondence of numbers to the items in a list. For example, the first item in the list would be 0, yet it wouldn't make sense for the user to input the number 0 for the first item. So, the item would appear as #1, but when the user inputs 1, the actual function subtracts 1 so that it corresponds to the item in the 0 position in the list, preventing errors from occuring.

```python
class Potion:
    def __init__(self, name, healing_amount, cost):
        self.name = name
        self.healing_amount = healing_amount
        self.cost = cost

    def __str__(self):
        return f"{self.name} - Heals {self.healing_amount} HP - Cost: {self.cost} coins"


class Weapon:
    def __init__(self, name, damage, cost, crit_chance=None):
        self.name = name
        self.damage = damage
        self.cost = cost
        self.crit_chance = crit_chance  

    def __str__(self):
        crit_info = f" - Crit Chance: {self.crit_chance}" if self.crit_chance else ""
        return f"{self.name} - Damage: {self.damage} - Cost: {self.cost} coins{crit_info}"


class Merchant:
    def __init__(self, items):
        self.items = items  # List of all items (weapons, potions, armor, etc.)
    
    def sell(self, item, user_balance):
        """Sell an item to the user if they can afford it."""
        if user_balance >= item.cost:
            user_balance -= item.cost
            print(f"You have purchased {item.name} for {item.cost} coins.")
            print(f"Your remaining balance is {user_balance} coins.")
            return user_balance
        else:
            print("You do not have enough coins to purchase this item.")
            return user_balance

    def show_shop(self):
        """Display the available items in the merchant's shop."""
        print("\nWelcome to the Merchant's Shop! Here are the available items:\n")
        
        for idx, item in enumerate(self.items):
            print(f"{idx + 1}. {item}")

    def buy_item(self, user, item_idx):
        """Allows the user to buy an item based on the index."""
        item = self.items[item_idx]
        # Sell the item to the user and update their balance
        user.money = self.sell(item, user.money)
        user.inventory.append(item)
        print(f"Your inventory: {user.inventory}")
        

class User:
    def __init__(self, name, HP, money, attack):
        self.name = name
        self.HP = HP
        self.attack = attack
        self.money = money
        self.inventory = []

    def __str__(self):
        return f"Name: {self.name}, HP: {self.HP}, Money: {self.money}, Attack: {self.attack}, Inventory: {self.inventory}"

    def buy(self, item):
        if self.money >= item.cost:
            self.inventory.append(item)
            self.money -= item.cost
            print(f"Bought {item.name}. Inventory: {self.inventory}")
        else:
            print("Not enough money to buy this item.")

    def heal(self, amount):
        self.HP += amount
        print(f"{self.name} healed! Current HP: {self.HP}")


def start_shop(user, merchant):
    """A simple shop interaction where the user can buy items from the merchant."""
    while True:
        # Show available items
        merchant.show_shop()

        # Get user input for selection
        try:
            item_idx = int(input(f"Enter the number of the item to buy, or '0' to exit: ")) - 1
            if item_idx == -1:
                print("Exiting shop...")
                break

            if item_idx < 0 or item_idx >= len(merchant.items):
                print("Invalid selection. Please try again.")
                continue

            # Buy the item and update the user’s balance
            merchant.buy_item(user, item_idx)

        except ValueError:
            print("Please enter a valid number.")


# Example Setup
stick = Weapon("Stick", 5, 0)
basic_sword = Weapon("Basic Sword", 8, 9)
healing_potion = Potion("Healing Potion", 20, 5)
basic_armor = "Basic Armor"  # Example armor (no class needed for simplicity)

# Create the Merchant with all items
merchant = Merchant(items=[stick, basic_sword, healing_potion, basic_armor])

# Create a user with initial money and health
user = User(name="Hero", HP=100, money=50, attack=10)

# Start the shop interaction
start_shop(user, merchant)

```

Chat GPT's idea of creating new functions in the merchant class to create a numbered list of options was different from my original idea of just having a list so that the user would type in what they want. I thought it was more organized and a nice break from the line-to-line nature of the rest of the game, therefore it was implemented.

## Second Major Prompt - Inventory Printing Problems

## 1. **Task/Problem Description**

Throughout the game, the idea of using print(user.inventory) was used, yet it wouldn't print the name of items. This was a major issue. So, chatGPT was given the sample code, and asked "How come the following code doesnt print the name of the items in the inventory?"

## 2. **Initial Approach/Code**

As stated before, I originally tried to use f strings to print our the user's inventory with user.inventory. However, this wasn't printing the desired result. I gave ChatGPT a sample of the story code for Day One in the game, and asked why the printing wasn't functioning properly.

```python

time.sleep(0.8) 
user.inventory.append(stick) 
print ('INSTRUCTION: You can check your inventory any time after the story is over in the options. This lets you see potions, armor, and weapons.') 
time.sleep(2) 
choice4 = input("Press I to see your inventory").capitalize() 
while True: 
    if choice4 == "I": 
        print(f"Your Inventory: {user.inventory}") 
        break 
    else: 
        print("Invalid! Try again!") 


```


## 3. **Interaction with ChatGPT**

### Questions/Requests to ChatGPT

Overall, for this prompt, ChatGPT gave a pretty direct response. However, I did ask a followup question to understand what it wrote.

- "What does the str method do?"

To this, ChatGPT responded with:
The __str__ method in Python is a special method that allows you to define a custom string representation for objects of your class. When you use the str() function or call print() on an object, Python will automatically use the __str__ method to convert the object into a human-readable string.

Purpose:
The __str__ method is meant to provide a string representation of an object that is easy to read and understandable. It is what gets used when you call str(object) or print the object.

If you don’t define the __str__ method in your custom class, Python will use a default representation that usually looks something like this:

<__main__.ClassName object at 0xADDRESS>

How to Define __str__:
When you define the __str__ method, you can control what the string representation of your object should be. This method should return a string that represents the object in a readable way.

---

## 4. **ChatGPT's Suggestions/Code Changes**

ChatGPT suggested adding the str method to the weapons class.  

```python
#The issue arises because you are directly printing the user.inventory list, which contains Weapon objects (like stick) in their raw form, not the string representation of the objects.
#In Python, when you print a list of objects (e.g., user.inventory), it calls the __repr__ or __str__ method of the objects to convert them into a string. If you haven't defined a custom __str__ or __repr__ method in the Weapon class, the default behavior will print something like <__main__.Weapon object at 0x7f0ecb2a8c40>, which is not the expected human-readable format.

#Solution:
#You need to ensure that the Weapon class (or any other objects in the user.inventory) has a __str__ method defined, which will provide a human-readable description when you print the object.
#Fix:
#Make sure you have the following __str__ method in your Weapon class (it should already be there, but let’s confirm):


class Weapon:
    def __init__(self, name, damage, cost, crit_chance=None):
        self.name = name
        self.damage = damage
        self.cost = cost
        self.crit_chance = crit_chance

    def __str__(self):
        crit_info = f" - Crit Chance: {self.crit_chance}" if self.crit_chance else ""
        return f"{self.name} - Damage: {self.damage} - Cost: {self.cost} coins{crit_info}"

#Example with the Inventory Display:
#Here’s the part of your code that displays the inventory:

time.sleep(0.8)
user.inventory.append(stick)  # Add the weapon to the inventory
print('INSTRUCTION: You can check your inventory any time after the story is over in the options. This lets you see potions, armor, and weapons.')
time.sleep(2)

# Asking the user to view inventory
choice4 = input("Press I to see your inventory").capitalize()
while True:
    if choice4 == "I":
        # Print the string representation of each item in the inventory
        print("Your Inventory:")
        for item in user.inventory:
            print(item)  # This calls __str__ method of each item
        break
    else:
        print("Invalid! Try again!")

#Expected Output:
#When you press 'I', you should see something like:
#Your Inventory:
#Stick - Damage: 5 - Cost: 0 coins

#If you had other items in the inventory, they would display similarly.
```

Essentially, ChatGPT stated to use the str method, but also talked about iterating through the user's inventory, and printing out each item seperately with a for loop. This was something I had prior knowledge with, but didn't think to use in this situation, so it certainly was different from my approach.

## Third Major Prompt - Playing Music with Pygame